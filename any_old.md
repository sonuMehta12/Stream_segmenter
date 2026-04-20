# AnyMall-Chat — Proposed Architecture (Orchestrator Pattern)

> Proposed · 2026-04-20

---

## 1. High-Level System Architecture

```mermaid
graph TB
    subgraph CLIENT["Client Layer"]
        FE["React / Mobile\nSSE consumer"]
    end

    subgraph API["API Layer  ·  FastAPI"]
        CHAT["POST /api/v1/chat/stream\n(SSE — single endpoint)"]
        PETS["GET /api/v1/pets"]
        SETUP["GET /api/v1/setup/questions"]
        HEALTH["GET /health"]
    end

    subgraph ORCHESTRATOR["Orchestrator Agent\n(only agent that talks to user)"]
        ORC["ConversationAgent\n• owns full conversation context\n• decides when to delegate\n• synthesizes sub-agent results\n• asks clarifying Qs naturally\n• applies guardrails before reply"]
    end

    subgraph SUB["Sub-Agents  (never talk to user directly)"]
        FOOD["FoodAI\n• LLM-controlled tool use\n• calls MCP for recipes\n• calls Tavily for food info\n• returns recipes + text + redirect flag"]
        HLTH["HealthAI\n• LLM-controlled tool use\n• calls Tavily for health context\n• returns structured summary + urgency\n• signals redirect if urgent"]
    end

    subgraph TOOLS["Tools (called by sub-agents)"]
        MCP["MCP Server\nrecipe search"]
        TAV["Tavily API\nweb search"]
    end

    subgraph BG["Background Pipeline\n(fire-and-forget, user never waits)"]
        COMP["CompressorAgent\nextract facts from message"]
        AGG["AggregatorAgent\nmerge facts → active_profile"]
        SQ["SuggestedQuestionsAgent\nregen 10 Qs/pet nightly"]
        HB["HistoryBuilder\nrebuild pet narrative"]
    end

    subgraph STORAGE["Storage"]
        PG[("PostgreSQL\nsource of truth")]
        VK[("Valkey\ncache + sessions")]
    end

    subgraph EXT["External"]
        AALDA["AALDA API\npet profiles"]
        LLM["LLM Provider\n(Azure OpenAI / OpenAI)"]
    end

    FE -->|SSE| CHAT
    CHAT --> ORC
    ORC -->|delegate food intent| FOOD
    ORC -->|delegate health intent| HLTH
    FOOD --> MCP
    FOOD --> TAV
    HLTH --> TAV
    FOOD -->|result + redirect flag| ORC
    HLTH -->|result + urgency| ORC

    ORC --> LLM
    FOOD --> LLM
    HLTH --> LLM

    CHAT -.->|async| BG
    BG --> COMP --> AGG
    AGG --> PG
    AGG --> VK
    BG --> SQ --> VK
    BG --> HB --> PG

    ORC --> AALDA
    ORC --> VK
    ORC --> PG
```

---

## 2. Request Flow

```mermaid
flowchart TD
    REQ["SSE Request\n{ message, session_id, pet_ids, language }"]

    A1["Auth  ·  X-User-Code → 401"]
    A2["AALDA Fetch\npet profile + facts\nValkey cache 5 min"]
    A3["Load Context\nthread · session history\nactive_profile · relationship"]

    ORC["Orchestrator\n(ConversationAgent)\nreads full context\ndecides next action"]

    D1{"what does\nthe message need?"}

    D2["Reply directly\nhealth chat / general Q\nclarification ask\nfollowup"]
    D3["Delegate → FoodAI\nrecipes / food info\nnutrition Qs"]
    D4["Delegate → HealthAI\ncurrent symptom\nurgent concern"]

    FA["FoodAI\n1. plan queries (tool call)\n2. MCP + Tavily in parallel\n3. synthesise reply + recipes\n4. set redirect flag if needed"]

    HA["HealthAI\n1. Tavily health context\n2. assess urgency\n3. return summary + redirect flag"]

    MERGE["Orchestrator\nreceives sub-agent result\nsynthesises final reply\napplies guardrails\nbuilds redirect payload"]

    SSE["Stream reply tokens → client\nframe: meta → token… → done\n(redirect in done frame)"]

    BG["🔥 Background Pipeline\npersist · compress · aggregate\nregen questions · build history"]

    REQ --> A1 --> A2 --> A3 --> ORC --> D1
    D1 -->|general / chat| D2
    D1 -->|food intent| D3
    D1 -->|health concern| D4
    D2 --> MERGE
    D3 --> FA --> MERGE
    D4 --> HA --> MERGE
    MERGE --> SSE
    SSE -.->|async| BG
```

---

## 3. Orchestrator Decision Sequence (SSE)

```mermaid
sequenceDiagram
    autonumber
    participant CLI  as Client
    participant API  as FastAPI
    participant ORC  as Orchestrator
    participant FOOD as FoodAI
    participant HLTH as HealthAI
    participant LLM  as LLM Provider
    participant MCP  as MCP Server
    participant TAV  as Tavily
    participant BG   as Background

    CLI->>API: POST /chat/stream { message }
    API->>API: auth · AALDA fetch · load context

    API->>ORC: run(message, pet_ctx, history, relationship)

    ORC->>LLM: decide — reply / delegate food / delegate health
    LLM-->>ORC: action decision

    API-->>CLI: frame: meta { output_mode, pet_names }

    alt direct reply  (general / clarification / followup)
        ORC->>LLM: stream reply
        loop token
            LLM-->>ORC: token
            ORC-->>API: token
            API-->>CLI: frame: token
        end
        API-->>CLI: frame: done { no redirect }

    else delegate → FoodAI
        API-->>CLI: frame: status "Looking up food info…"
        ORC->>FOOD: run(message, pet_ctx, mode)

        FOOD->>LLM: plan queries
        LLM-->>FOOD: mcp_query · tavily_query

        par parallel tool calls
            FOOD->>MCP: search_recipes
            FOOD->>TAV: web search
        end
        MCP-->>FOOD: recipes[]
        TAV-->>FOOD: results[]

        API-->>CLI: frame: tools_complete { recipes_by_pet }

        FOOD->>LLM: stream synthesis
        loop token
            LLM-->>FOOD: token
            FOOD-->>ORC: token
            ORC-->>API: token
            API-->>CLI: frame: token
        end

        FOOD-->>ORC: result { text, recipes, redirect_flag }
        ORC->>LLM: wrap / confirm reply (lightweight)
        API-->>CLI: frame: done { redirect? }

    else delegate → HealthAI
        API-->>CLI: frame: status "Checking health context…"
        ORC->>HLTH: run(message, pet_ctx)

        HLTH->>TAV: search health context
        TAV-->>HLTH: results[]

        HLTH->>LLM: assess urgency + summarise
        LLM-->>HLTH: { summary, urgency, redirect_flag }

        HLTH-->>ORC: result

        ORC->>LLM: stream empathetic reply with context
        loop token
            LLM-->>ORC: token
            ORC-->>API: token
            API-->>CLI: frame: token
        end
        API-->>CLI: frame: done { redirect if urgent }
    end

    API->>BG: fire-and-forget
    Note over BG: persist → compress facts\n→ aggregate profile\n→ regen suggestions
```

---

## 4. Agent Responsibilities (Clean Boundaries)

```mermaid
graph LR
    subgraph ORC_BOX["Orchestrator  (ConversationAgent)"]
        O1["owns conversation context"]
        O2["decides to reply or delegate"]
        O3["asks clarifying questions naturally\n(no separate clarification agent)"]
        O4["synthesises sub-agent results"]
        O5["applies guardrails"]
        O6["builds redirect payload"]
        O7["only agent user sees"]
    end

    subgraph FOOD_BOX["FoodAI"]
        F1["receives: message + pet_ctx + mode"]
        F2["plans MCP + Tavily queries (LLM tool-use)"]
        F3["fetches recipes (MCP)"]
        F4["fetches web context (Tavily)"]
        F5["synthesises food reply + recipes"]
        F6["signals redirect flag to orchestrator"]
        F7["never talks to user directly"]
    end

    subgraph HLTH_BOX["HealthAI"]
        H1["receives: message + pet_ctx"]
        H2["fetches health context (Tavily)"]
        H3["assesses urgency"]
        H4["returns summary + urgency + redirect flag"]
        H5["never talks to user directly"]
    end

    subgraph BGP["Background  (unchanged)"]
        B1["CompressorAgent\nextract facts"]
        B2["AggregatorAgent\nmerge into active_profile"]
        B3["SuggestedQuestionsAgent\n10 Qs/pet nightly"]
        B4["HistoryBuilder\npet narrative"]
        B5["ThreadSummarizer\nnightly compaction"]
        B6["RelationshipBuilder\nowner style"]
    end

    ORC_BOX -->|delegates| FOOD_BOX
    ORC_BOX -->|delegates| HLTH_BOX
    FOOD_BOX -->|result| ORC_BOX
    HLTH_BOX -->|result| ORC_BOX
```

---

## 5. What Changes vs What Stays

```mermaid
graph TD
    subgraph REMOVE["❌  Remove"]
        R1["IntentClassifier\n(orchestrator decides in context)"]
        R2["ClarificationAgent\n(orchestrator asks naturally)"]
        R3["FoodQueryPlanner as separate agent\n(FoodAI plans its own queries)"]
        R4["Guardrails per-agent\n(centralised in orchestrator)"]
    end

    subgraph CHANGE["♻️  Change"]
        C1["ConversationAgent\n→ becomes Orchestrator\nknows when to delegate"]
        C2["FoodAgent\n→ FoodAI sub-agent\nLLM-controlled tool use\nno direct user reply"]
        C3["HealthAI  (new)\nextracted from ConversationAgent\nTavily-aware health sub-agent"]
        C4["SSE frames\nadd sub-agent status frames\n(food / health thinking)"]
    end

    subgraph KEEP["✅  Keep"]
        K1["Background pipeline\nCompressor → Aggregator → HistoryBuilder"]
        K2["ThreadSummarizer + RelationshipBuilder\n(nightly jobs)"]
        K3["SuggestedQuestionsAgent\n(nightly regen)"]
        K4["Valkey cache layer\n(same keys, same TTLs)"]
        K5["PostgreSQL schema\n(no changes needed)"]
        K6["AALDA fetch + ContextBuilder"]
        K7["Redirect / DeepLink builder"]
        K8["Confidence scoring"]
    end
```

---

## Summary

| | Current | Proposed |
|---|---------|---------|
| **Routing** | Separate IntentClassifier (brittle) | Orchestrator decides in context |
| **Clarification** | Separate ClarificationAgent | Orchestrator asks naturally in conversation |
| **User-facing agent** | Varies by intent | Always Orchestrator — one voice |
| **Food handling** | FoodAgent + FoodQueryPlanner | FoodAI sub-agent (LLM tool use) |
| **Health handling** | ConversationAgent only | HealthAI sub-agent (Tavily-aware) |
| **LLM calls per turn** | 3–4 (classifier + planner + agent) | 2–3 (orchestrator decide + sub-agent) |
| **Code complexity** | 8+ agents, complex state | 3 agents, clean delegation |
| **Tradeoff** | Parallel tools are fast | +1 LLM hop for delegation decisions |
