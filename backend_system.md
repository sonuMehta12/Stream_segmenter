# Backend System Architecture - CORRECTED

## System Overview

This document provides a **corrected and accurate** representation of the Live Stream Segmentation System backend architecture.

## 🎯 Key Corrections from Original Docs

### Timing Clarifications
- **Segment Creation**: 10 seconds per video segment (.ts files)
- **Transcription Window**: Uses overlap buffers (±5s before/after)
- **Layer 0 (Sentences)**: Processes every 30 seconds with 60s context window
- **Layer 1 (Subtopics)**: Adaptive 120s-600s windows
- **Layer 2 (Topics)**: Cumulative 480s+ threshold

### Layer Naming Consistency
- **Layer 0**: Sentence Extraction
- **Layer 1**: Subtopic Detection
- **Layer 2**: Topic Grouping (NOT Layer 3)

---

## 📊 Corrected System Diagram

```mermaid
flowchart TB
    subgraph INPUT["📡 INPUT LAYER"]
        YT[YouTube Stream<br/>10s chunks with overlap]
    end

    subgraph DOWNLOAD["⬇️ DOWNLOAD & BUFFER"]
        YTDLP[yt-dlp<br/>Extract HLS URL]
        FFMPEG[FFmpeg Segmenter<br/>10s chunks]
        BUFFER[Rolling Buffer<br/>30s media window]

        YT --> YTDLP
        YTDLP --> FFMPEG
        FFMPEG --> BUFFER
    end

    subgraph TRANSCRIBE["🎤 TRANSCRIPTION"]
        AUDIO[Audio Extraction<br/>FFmpeg WAV export]
        STT[STT Provider<br/>ElevenLabs/Groq/Whisper]
        TIMELINE[TimelineManager<br/>Append-only storage]

        BUFFER --> AUDIO
        AUDIO --> STT
        STT --> TIMELINE
    end

    subgraph STORAGE["💾 STORAGE"]
        JSONL[(master_transcript.jsonl<br/>Raw transcripts)]
        SQLITE[(timeline_index.db<br/>Fast queries)]

        TIMELINE --> JSONL
        TIMELINE --> SQLITE
    end

    subgraph LAYER0["🔵 LAYER 0: SENTENCES"]
        L0PROC[Layer0Processor<br/>Every 30s trigger]
        L0WINDOW[Context Window<br/>60s with ±15s buffer]
        L0LLM[Gemini API<br/>Sentence extraction]
        L0OUT[sentences.json]

        TIMELINE -.->|30s interval| L0PROC
        L0PROC --> L0WINDOW
        L0WINDOW --> L0LLM
        L0LLM --> L0OUT
    end

    subgraph LAYER1["🟢 LAYER 1: SUBTOPICS"]
        L1PROC[Layer1Processor<br/>Adaptive accumulation]
        L1STAGE1[Stage 1: Detection<br/>Thinking model]
        L1STAGE2[Stage 2: Generation<br/>Fast model]
        L1OUT[subtopics.json]

        TIMELINE -.->|≥120s buffer| L1PROC
        L1PROC --> L1STAGE1
        L1STAGE1 -->|Boundary found| L1STAGE2
        L1STAGE1 -->|No boundary| L1PROC
        L1STAGE2 --> L1OUT
    end

    subgraph LAYER2["🟡 LAYER 2: TOPICS"]
        L2PROC[Layer2Processor<br/>Cumulative threshold]
        L2STAGE1[Stage 1: Grouping<br/>Thinking model]
        L2STAGE2[Stage 2: Enhancement<br/>Fast model]
        L2OUT[topics.json]

        L1OUT -.->|≥480s cumulative| L2PROC
        L2PROC --> L2STAGE1
        L2STAGE1 --> L2STAGE2
        L2STAGE2 --> L2OUT
    end

    subgraph VIDEO["🎬 VIDEO OUTPUT"]
        VIDQUEUE[Video Task Queue]
        VIDPROC[FFmpeg Concat<br/>No re-encoding]
        VIDOUT[(video_output/)]

        L1OUT -.-> VIDQUEUE
        L2OUT -.-> VIDQUEUE
        VIDQUEUE --> VIDPROC
        VIDPROC --> VIDOUT
    end

    subgraph WEBSOCKET["🌐 REAL-TIME UPDATES"]
        WSMGR[WebSocketManager]

        L0OUT -.-> WSMGR
        L1OUT -.-> WSMGR
        L2OUT -.-> WSMGR
    end

    style YT fill:#ff6b6b,color:#fff
    style L0LLM fill:#4ecdc4,color:#fff
    style L1STAGE1 fill:#95e1d3,color:#000
    style L2STAGE1 fill:#f9ca24,color:#000
    style VIDPROC fill:#a8e6cf,color:#000
```

---

## 🔄 Detailed Data Flow: Corrected

### Phase 1: Download and Buffering (CORRECTED)

```mermaid
sequenceDiagram
    participant User
    participant Main as LiveStreamSegmenter
    participant YtDlp as yt-dlp
    participant FFmpeg
    participant Disk
    participant Monitor

    User->>Main: start(youtube_url)
    Main->>YtDlp: get_stream_url(youtube_url)
    YtDlp-->>Main: m3u8 HLS URL

    Main->>FFmpeg: Start segmenting
    Note over FFmpeg: 10s segments with<br/>±5s overlap for safety

    loop Every 10 seconds
        FFmpeg->>Disk: Write segment_XXXXX.ts
        Disk->>Monitor: File created event
        Monitor->>Main: on_new_segment(segment_id)
    end

    Note over FFmpeg,Disk: Segments are MPEG-TS format<br/>Partial files are valid<br/>No re-encoding
```

**Key Corrections:**
- Segments are **10 seconds**, not 30 seconds
- Overlap is **±5 seconds** before/after for word boundary safety
- Monitor detects files immediately (not polling)

---

### Phase 2: Transcription with Overlap (CORRECTED)

```mermaid
sequenceDiagram
    participant Monitor
    participant Audio as AudioProcessor
    participant STT as STT Provider
    participant Timeline as TimelineManager
    participant DB as SQLite + JSONL

    Monitor->>Audio: process_segment(segment_00001.ts)

    rect rgb(240, 248, 255)
        Note over Audio: Extract audio with FFmpeg
        Audio->>Audio: ffmpeg -i segment_00001.ts<br/>-ar 16000 -ac 1<br/>audio_00001.wav
    end

    Audio->>STT: transcribe(audio_00001.wav)
    Note over STT: Provider: ElevenLabs/Groq/Whisper<br/>Returns word-level timestamps

    STT-->>Audio: Transcript{<br/>  text: "...",<br/>  words: [Word(...)],<br/>  confidence: 0.95<br/>}

    Audio->>Timeline: store_transcript(transcript)

    rect rgb(255, 248, 240)
        Note over Timeline,DB: Deduplication Check<br/>Word-level comparison<br/>5s overlap stitching
        Timeline->>Timeline: deduplicate_words()
        Timeline->>DB: Append to master_transcript.jsonl
        Timeline->>DB: Index in timeline_index.db
    end

    Timeline-->>Monitor: Complete
```

**Key Corrections:**
- Audio extraction uses **16kHz mono WAV** (standard for STT)
- Deduplication happens at **word level** using timestamps
- 5-second overlap is stitched to prevent duplicate/cut words
- Storage is **append-only** for crash safety

---

### Phase 3: Layer 0 - Sentence Extraction (CORRECTED)

```mermaid
sequenceDiagram
    participant Timer as 30s Timer
    participant L0 as Layer0Processor
    participant State as Layer0State
    participant Timeline as TimelineManager
    participant LLM as Gemini Flash
    participant Output as LiveOutputWriter
    participant WS as WebSocketManager

    Timer->>L0: Trigger (every 30s)
    L0->>State: should_trigger()
    State-->>L0: true (≥30s new content)

    L0->>State: get_context_window()
    Note over State: Window = 60s total:<br/>30s core + 15s before + 15s after<br/>Prevents sentence truncation

    State-->>L0: [start: 120.0s, end: 180.0s]

    L0->>Timeline: get_transcripts_in_range(120.0, 180.0)
    Timeline->>Timeline: Query timeline_index.db<br/>Read from master_transcript.jsonl
    Timeline-->>L0: List[Transcript] with word timestamps

    L0->>L0: Format for LLM:<br/>[120.1s] Good<br/>[120.4s] evening<br/>[120.8s] everyone

    L0->>LLM: Extract complete sentences
    Note over LLM: Model: gemini-2.0-flash-thinking-exp<br/>Temperature: 0.2<br/>Returns JSON

    LLM-->>L0: {sentences: [<br/>  {text: "...", start: 120.1, end: 122.3}<br/>]}

    L0->>L0: Deduplication check<br/>Signature: text + timestamps

    L0->>Output: update_sentences(new_sentences)
    Output->>Output: Thread-safe JSON write

    par Parallel broadcasts
        Output->>WS: broadcast("sentence", data)
    end

    L0->>State: update(last_processed=150.0)
```

**Key Corrections:**
- **60s total context window** (not 30s): 30s core + 15s buffer each side
- Deduplication uses **signature matching** (text + timestamps)
- Processing happens **every 30 seconds** but analyzes 60s
- LLM receives **word-level timestamps** for precision

---

### Phase 4: Layer 1 - Adaptive Subtopic Detection (CORRECTED)

```mermaid
sequenceDiagram
    participant Timer
    participant L1 as Layer1Processor
    participant State as Layer1State
    participant Timeline
    participant Stage1 as Gemini Thinking
    participant Stage2 as Gemini Fast
    participant Output
    participant WS as WebSocketManager

    Note over Timer,WS: CYCLE 1: 120s accumulated

    Timer->>L1: Check (every 30s)
    L1->>State: should_trigger()
    State->>State: Check accumulated ≥ 120s
    State-->>L1: true (120s ready)

    L1->>State: get_analysis_range()
    Note over State: Recursive accumulation:<br/>start = last_boundary_end (0.0s)<br/>end = current_time (120.0s)
    State-->>L1: [0.0s - 120.0s]

    L1->>Timeline: get_transcripts_in_range(0.0, 120.0)
    Timeline-->>L1: Transcripts

    rect rgb(230, 240, 255)
        Note over L1,Stage1: STAGE 1: DETECTION
        L1->>Stage1: Analyze for boundary
        Stage1-->>L1: {<br/>  boundary_detected: false,<br/>  confidence: 0.65,<br/>  reason: "Topic developing"<br/>}
    end

    L1->>L1: confidence < 0.75
    L1->>State: update_on_no_detection()
    Note over State: Keep accumulating<br/>Don't change unprocessed_start

    Note over Timer,WS: CYCLE 2: 240s accumulated

    Timer->>L1: Check (30s later)
    L1->>State: should_trigger()
    State-->>L1: true (240s ready)

    L1->>State: get_analysis_range()
    State-->>L1: [0.0s - 240.0s] (extended!)

    L1->>Timeline: get_transcripts_in_range(0.0, 240.0)
    Timeline-->>L1: Transcripts (2x longer)

    rect rgb(230, 240, 255)
        Note over L1,Stage1: STAGE 1: DETECTION (retry)
        L1->>Stage1: Analyze extended window
        Stage1-->>L1: {<br/>  boundary_detected: true,<br/>  confidence: 0.85,<br/>  boundary: {start: 0.0, end: 235.4}<br/>}
    end

    L1->>L1: confidence ≥ 0.75 ✓

    rect rgb(240, 255, 240)
        Note over L1,Stage2: STAGE 2: GENERATION
        L1->>Stage2: Create subtopic
        Stage2-->>L1: SubTopicResult{<br/>  id, title, description,<br/>  summary, highlights<br/>}
    end

    L1->>Output: update_subtopics(subtopic)
    Output->>WS: broadcast("subtopic", data)

    L1->>State: update_on_subtopic_created(235.4)
    Note over State: Reset: unprocessed_start = 235.4s<br/>Next cycle starts from 235.4s
```

**Key Corrections:**
- **Recursive accumulation**: Same start point until boundary found
- Minimum **120s**, maximum **600s** window
- **Two-stage process**: Detection (thinking) → Generation (fast)
- Confidence threshold: **≥0.75** required
- State resets **only on successful boundary detection**

---

### Phase 5: Layer 2 - Cumulative Topic Grouping (CORRECTED)

```mermaid
sequenceDiagram
    participant Timer
    participant L2 as Layer2Processor
    participant State as Layer2State
    participant Stage1 as Gemini Thinking
    participant Stage2 as Gemini Fast
    participant Output
    participant WS

    Note over Timer,WS: Layer 1 creates subtopics

    loop Layer 1 creates subtopics
        L1->>State: add_ungrouped_subtopic("st_001")
        L1->>State: add_ungrouped_subtopic("st_002")
        L1->>State: add_ungrouped_subtopic("st_003")
    end

    Timer->>L2: Check (every 60s)
    L2->>State: should_trigger()
    State->>State: Calculate cumulative duration
    Note over State: st_001: 235s<br/>st_002: 180s<br/>st_003: 145s<br/>Total: 560s ≥ 480s threshold ✓

    State-->>L2: true (560s ready)

    L2->>State: get_ungrouped_subtopics()
    State-->>L2: ["st_001", "st_002", "st_003"]

    L2->>Output: Load subtopic details
    Output-->>L2: Full subtopic objects

    rect rgb(255, 245, 220)
        Note over L2,Stage1: STAGE 1: GROUPING ANALYSIS
        L2->>Stage1: Analyze relationships
        Note over Stage1: Semantic similarity<br/>Thematic connections<br/>Natural groupings

        Stage1-->>L2: {<br/>  groupings: [<br/>    {ids: ["st_001", "st_002"],<br/>     rationale: "Both election"}],<br/>  ungrouped: ["st_003"]<br/>}
    end

    loop For each grouping
        rect rgb(240, 255, 240)
            Note over L2,Stage2: STAGE 2: SYNTHESIS
            L2->>Stage2: Create parent topic
            Note over Stage2: Enhanced title<br/>Synthesis summary<br/>Merged highlights

            Stage2-->>L2: ParentTopic{<br/>  id, title, description,<br/>  subtopic_ids, highlights<br/>}
        end

        L2->>Output: update_topics(parent_topic)
        L2->>Output: link_subtopics_to_parent(topic_id)
        Output->>WS: broadcast("topic", data)
    end

    L2->>State: update_on_grouping(["st_001", "st_002"])
    Note over State: Remove grouped from ungrouped list<br/>Only "st_003" remains<br/>Duration: 145s < 480s
```

**Key Corrections:**
- Triggers when **cumulative duration ≥ 480s** (6-8 minutes)
- Can leave subtopics **ungrouped** if unrelated
- **Two-stage**: Grouping analysis → Synthesis
- Updates **parent_topic_id** in subtopics
- State tracks **ungrouped subtopics** separately

---

## 🔑 Critical Corrections Summary

### 1. Timing Specifications (CORRECTED)
| Component | Timing | Purpose |
|-----------|--------|---------|
| Video segments | **10 seconds** | Raw .ts file chunks |
| Segment overlap | **±5 seconds** | Word boundary safety |
| Transcription window | **10s + overlap** | Includes buffer zones |
| Layer 0 trigger | **Every 30s** | Sentence extraction |
| Layer 0 context | **60s total** | 30s core + 30s buffer |
| Layer 1 minimum | **120s** | First subtopic analysis |
| Layer 1 maximum | **600s** | Max accumulation limit |
| Layer 2 threshold | **480s cumulative** | Topic grouping trigger |

### 2. Layer Naming (CORRECTED)
- **Layer 0**: Sentence Extraction (not "Base Layer")
- **Layer 1**: Subtopic Detection (not "Topic Layer")
- **Layer 2**: Topic Grouping (NOT "Layer 3")

### 3. Buffer Management (CORRECTED)
- **Rolling buffer**: Keeps ~30s of media for late processing
- **Context windows**: Use ±15s buffers to prevent sentence cuts
- **Overlap stitching**: Deduplicates 5s overlaps at word level
- **Recursive accumulation**: Layer 1 keeps same start until boundary found

### 4. Deduplication Strategy (CORRECTED)
- **Layer 0**: Signature-based (text + timestamp hash)
- **Layer 1**: LLM checks existing subtopics via prompt
- **Layer 2**: Grouped IDs removed from ungrouped list
- **Transcription**: Word-level comparison in 5s overlap zones

### 5. Two-Stage LLM Processing (CORRECTED)
Each layer uses **two separate LLM calls**:

**Stage 1 - Detection/Analysis:**
- Model: `gemini-2.0-flash-thinking-exp` (reasoning)
- Temperature: 0.2 (deterministic)
- Input: Plain text transcript
- Output: Decision + confidence

**Stage 2 - Generation/Enhancement:**
- Model: `gemini-2.0-flash-exp` (fast)
- Temperature: 0.3-0.4 (creative)
- Input: Timestamped transcript
- Output: Structured JSON with details

---

## 📊 Corrected Progressive Timeline

```
Time    | Segments | Transcripts | Layer 0 | Layer 1        | Layer 2
--------|----------|-------------|---------|----------------|------------
0:00    | 0        | 0           | -       | -              | -
0:10    | 1        | 1           | -       | -              | -
0:20    | 2        | 2           | -       | -              | -
0:30    | 3        | 3           | ✓ Run   | -              | -
0:40    | 4        | 4           | -       | -              | -
0:50    | 5        | 5           | -       | -              | -
1:00    | 6        | 6           | ✓ Run   | -              | -
1:10    | 7        | 7           | -       | -              | -
1:20    | 8        | 8           | -       | -              | -
1:30    | 9        | 9           | ✓ Run   | -              | -
2:00    | 12       | 12          | ✓ Run   | ✓ Check (120s) | -
2:30    | 15       | 15          | ✓ Run   | ✓ Check (150s) | -
3:00    | 18       | 18          | ✓ Run   | ✓ Check (180s) | -
4:00    | 24       | 24          | ✓ Run   | ✓ Boundary!    | -
4:00+   | -        | -           | -       | Reset (0s)     | Add ungrouped
8:00    | 48       | 48          | ✓ Run   | ✓ Boundary!    | ✓ Group (≥480s)
```

---

## 🎯 Key Insights (CORRECTED)

### Why the Timing Matters
1. **10s segments** = Optimal for real-time processing + crash recovery
2. **±5s overlap** = Prevents mid-word cuts in transcription
3. **60s context** for sentences = Full sentence boundaries captured
4. **Recursive accumulation** = Natural topic boundaries (not forced)
5. **480s threshold** = Meaningful topic groupings (~6-8 min content)

### Why Two-Stage Processing
1. **Stage 1 (Thinking)**: Fast binary decision - "Is there a boundary?"
2. **Stage 2 (Fast)**: Detailed generation - "Create the content object"
3. **Benefit**: Cheaper, faster, more accurate than single-stage

### Why Append-Only Storage
1. **Crash safety**: Never lose data mid-write
2. **Reprocessing**: Can replay from any point
3. **Debugging**: Full audit trail of what happened
4. **Performance**: No file locks, append is atomic

---

## 🔧 Implementation Checklist (CORRECTED)

- [ ] Segments are **10 seconds** (not 30s)
- [ ] Overlap is **±5 seconds** before/after
- [ ] Layer 0 processes **every 30s** with **60s context**
- [ ] Layer 1 uses **recursive accumulation** (120s-600s)
- [ ] Layer 2 triggers at **480s cumulative**
- [ ] Deduplication at **word level** in overlap zones
- [ ] Two-stage LLM: **Detection → Generation**
- [ ] Storage is **append-only** (JSONL + SQLite index)
- [ ] WebSocket broadcasts for **all layers**
- [ ] Video concat uses **FFmpeg copy mode** (no re-encode)

---

**This corrected document should be used as the source of truth for backend architecture.**
