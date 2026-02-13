# Python for AI Engineers: Learning Through Stream-Segmenter

## Table of Contents

- [Introduction](#introduction)
- [Part I: Foundations (Chapters 1-7)](#part-i-foundations)
- [Part II: Data & AI (Chapters 8-11)](#part-ii-data--ai)
- [Part III: Backend & Concurrency (Chapters 12-15)](#part-iii-backend--concurrency)
- [Part IV: AI & ML Integration (Chapters 16-18)](#part-iv-ai--ml-integration)
- [Part V: Production & Scale (Chapters 19-21)](#part-v-production--scale)
- [Appendices](#appendices)

---

## Introduction

### What You're About to Learn

You've built a production-grade AI video processing system using Claude Code without deeply understanding the Python syntax. Now you're going to learn Python the right way—by reading, understanding, and eventually modifying your own stream-segmenter codebase.

This guide differs from traditional Python tutorials in one critical way: **every example comes from your actual production code**, not abstract "Hello World" programs. You'll learn each concept exactly when you need it to understand the next file in your system.

### The Philosophy

- **Real > Theoretical**: Every concept grounded in actual production code
- **Progressive**: Start with simple data models, build up to system orchestration
- **Hands-On**: Use your repository as the practice ground
- **Practical**: No time wasted on concepts you don't need

### How to Use This Guide

1. **Read the concept** (10 minutes)
2. **Study the code example** from the repository (15 minutes)
3. **Try Claude Code prompts** to practice the concept (20 minutes)
4. **Complete repository exercises** to modify actual code (30-60 minutes)
5. **Move to the next chapter**

**Estimated time to completion**: 12-15 weeks (2-3 hours per week)

### Your Learning Path

```
Week 1-3: Data Models (segment.py)
    ↓
Week 4-6: Services (state_manager.py, transcriber.py)
    ↓
Week 7-9: Core Processing (stream_ingestion.py, layer_manager.py)
    ↓
Week 10-12: Advanced Patterns (main.py, async, threading)
    ↓
Week 13-15: Full System Understanding & Your First Feature
```

---

# PART I: FOUNDATIONS

## Chapter 1: Python Basics for Data Models

### What You'll Learn

Data models are the foundation of your entire system. Every video segment, transcript, topic, and word is represented by a dataclass. This chapter teaches you Python's type system, variables, and the `@dataclass` decorator through your actual code.

### 1.1 Variables and Data Types

Python has far simpler variable declaration than JavaScript. Let's look at real examples from your code:

**From `src/models/segment.py` lines 28-31:**

```python
@dataclass
class Word:
    text: str                    # string - text of the word
    start: float                 # floating point - time in seconds
    end: float                   # floating point - time in seconds
    confidence: float = 1.0      # optional: with default value
```

**Key Differences from JavaScript:**

| Concept | JavaScript | Python | Your Code |
|---------|-----------|--------|-----------|
| Variable declaration | `let age = 25` | `age = 25` | `start: float` |
| Type hinting | TypeScript: `age: number` | Python: `age: int` | `start: float` |
| String | `"hello"` or `'hello'` | `"hello"` or `'hello'` | `text: str` |
| Floating point | `19.99` | `19.99` | `start: float` |
| Boolean | `true`/`false` | `True`/`False` | `field(default=True)` |
| No value | `null` | `None` | `file_size: Optional[int] = None` |

**Live Examples from Your Code:**

```python
# From segment.py line 63-70 (Segment class)
id: int                                    # ID is an integer
file_path: Path                           # Path object (from pathlib)
start_time: float                         # Floating point seconds
status: str = "pending"                   # String with default
created_at: datetime = field(...)         # Datetime with factory
file_size: Optional[int] = None           # Can be int or None
```

**Try This Yourself:**

1. Open `src/models/segment.py` in your IDE
2. Find the `Word` class at line 17
3. Identify each variable's type
4. Ask yourself: "Why is `start` a float and not an int?"
   - Answer: You need sub-second precision for word timestamps

### 1.2 Understanding Type Hints

Type hints are Python's way of documenting what types variables should be. They don't enforce types at runtime (Python is dynamically typed), but they help:

1. **IDE autocomplete**: Know what methods are available
2. **Documentation**: Understand what a function expects
3. **Type checking**: Tools like `mypy` can catch bugs
4. **Readability**: Future you will understand the code

**The Basic Pattern:**

```python
# VARIABLE_NAME: TYPE = VALUE

# From your code:
text: str = "hello"                    # Variable named 'text', type string
start: float = 10.5                    # Variable named 'start', type float
words: List[Word] = []                 # List that contains Word objects
```

**Complex Types You'll See:**

```python
# From segment.py and throughout your codebase
Optional[str]              # Can be string or None
List[str]                  # List of strings
Dict[str, Any]             # Dictionary with string keys, any values
Tuple[float, float]        # Tuple with two floats
Union[str, int]            # Either string or int
```

**Why This Matters in Your Code:**

```python
# From segment.py line 72-83 (to_dict method)
def to_dict(self) -> Dict[str, Any]:
    """Convert segment to dictionary for JSON serialization."""
    return {
        'id': self.id,
        'file_path': str(self.file_path),
        'status': self.status,
        'created_at': self.created_at.isoformat(),
    }

# The "-> Dict[str, Any]" means:
# - This function RETURNS a dictionary
# - Keys are strings ('id', 'file_path', etc.)
# - Values can be anything (Any)
```

### 1.3 Dataclasses: The `@dataclass` Decorator

This is Python's modern solution to writing clean data container classes. Before dataclasses, you'd write ~20 lines of boilerplate. Now you write 5:

**The Old Way (Pre-2018):**

```python
class Word:
    def __init__(self, text, start, end, confidence=1.0):
        self.text = text
        self.start = start
        self.end = end
        self.confidence = confidence

    def __repr__(self):
        return f"Word(text={self.text}, start={self.start}, ...)"

    def __eq__(self, other):
        return (self.text == other.text and
                self.start == other.start and ...)
```

**The Modern Way (Your Code):**

```python
from dataclasses import dataclass

@dataclass
class Word:
    text: str
    start: float
    end: float
    confidence: float = 1.0
```

**Magic that `@dataclass` Provides:**

1. **`__init__` method**: Automatically created
   ```python
   w = Word(text="hello", start=1.5, end=2.0)
   # The __init__ is auto-generated from the field declarations
   ```

2. **`__repr__` method**: String representation for debugging
   ```python
   print(w)  # Word(text='hello', start=1.5, end=2.0, confidence=1.0)
   ```

3. **`__eq__` method**: Comparison
   ```python
   w1 = Word("hello", 1.5, 2.0)
   w2 = Word("hello", 1.5, 2.0)
   w1 == w2  # True (compares all fields)
   ```

**Fields with Default Values:**

```python
# From segment.py line 28-31
@dataclass
class Word:
    text: str                    # Required, no default
    start: float                 # Required
    end: float                   # Required
    confidence: float = 1.0      # Optional, defaults to 1.0

# Usage:
w1 = Word("hello", 1.0, 2.0)           # confidence defaults to 1.0
w2 = Word("hello", 1.0, 2.0, 0.95)     # or specify it
```

**Field with Factory Function:**

```python
# From segment.py line 69
from dataclasses import field

@dataclass
class Segment:
    # ... other fields ...
    created_at: datetime = field(default_factory=datetime.now)
    # default_factory runs ONCE per instance, not shared

# Why this matters:
# Without field():
created_at: datetime = datetime.now()  # WRONG: runs once at class definition
# All segments would have the same creation time!

# With field():
created_at: datetime = field(default_factory=datetime.now)  # CORRECT
# Each segment gets current time when created
```

### 1.4 Properties: The `@property` Decorator

Properties let you compute values on-the-fly without storing them. Look at this real example:

**From segment.py lines 93-96:**

```python
@dataclass
class Segment:
    # ... fields ...
    file_path: Path

    @property
    def exists(self) -> bool:
        """Check if the segment file exists on disk."""
        return self.file_path.exists()

# Usage:
segment = Segment(id=1, file_path=Path("data/segment_001.ts"), ...)
if segment.exists:  # Looks like an attribute, but runs code
    print("File exists!")
```

**Why Properties Matter:**

```python
# Without @property - looks weird:
if segment.exists():  # Looks like a method
    ...

# With @property - looks natural:
if segment.exists:    # Looks like an attribute
    ...

# But it's actually a method hiding underneath
```

### 1.5 Class Methods: The `@classmethod` Decorator

Class methods receive the class itself as the first parameter, not an instance. Your code uses this for factory methods:

**From segment.py lines 42-45:**

```python
@dataclass
class Word:
    text: str
    start: float
    end: float
    confidence: float = 1.0

    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'Word':
        """Create word from dictionary."""
        return cls(**data)  # cls is Word class itself

# Usage:
data = {'text': 'hello', 'start': 1.0, 'end': 2.0, 'confidence': 0.95}
word = Word.from_dict(data)  # Creates Word instance from dict
```

**Why This Pattern?**

```python
# Instead of:
word_dict = {'text': 'hello', ...}
word = Word(word_dict['text'], word_dict['start'], ...)  # Tedious

# You can:
word = Word.from_dict(word_dict)  # Cleaner!
```

### Claude Code Prompts for Chapter 1

1. **"Read `src/models/segment.py` lines 17-46 and explain: What is a dataclass and why does this code use them instead of regular classes?"**
   - Expected answer: Dataclasses automatically generate `__init__`, `__repr__`, `__eq__`. Less boilerplate, cleaner code.

2. **"Show me all the type hints in the Segment class (lines 48-97) and explain what each one means."**
   - Expected: Point out `int`, `Path`, `float`, `str`, `datetime`, `Optional[int]`, and explain what each means.

3. **"Create a dataclass for a `Video` with: id (int), title (str), duration (float), tags (list of strings), and created_at (datetime with default factory). Include a `@property` called `hours` that returns duration/3600."**
   - Expected: Full dataclass definition with fields and property.

### Repository Practice Exercise

**Exercise 1.1: Read and Understand**

1. Open `src/models/segment.py`
2. Find the `Segment` class (line 48)
3. For each field, write a comment explaining what it stores:
   - `id` - ?
   - `file_path` - ?
   - `start_time` - ?
   - etc.
4. Add one new field: `processed_time: Optional[float] = None` (time it took to process)

**Exercise 1.2: Create a New Dataclass**

In `src/models/segment.py`, add this new class at the bottom:

```python
@dataclass
class ProcessingMetrics:
    """Tracks metrics for a segment's processing."""
    segment_id: int
    transcription_time: float      # seconds to transcribe
    llm_analysis_time: float       # seconds for LLM analysis
    total_time: float              # total processing time

    @property
    def avg_time_per_second(self) -> float:
        """Average time spent per second of video."""
        # Calculate: total_time / (video duration)
        # Hint: look at how Segment.exists is implemented
        pass
```

Then create instances and use them:

```python
metrics = ProcessingMetrics(
    segment_id=1,
    transcription_time=5.2,
    llm_analysis_time=2.1,
    total_time=7.3
)
print(metrics)  # Should auto-format nicely
```

**Exercise 1.3: From Dict Pattern**

Add a `from_dict` classmethod to `ProcessingMetrics`:

```python
@classmethod
def from_dict(cls, data: Dict[str, Any]) -> 'ProcessingMetrics':
    """Create from dictionary (useful when loading from JSON)."""
    return cls(**data)
```

Then test it:

```python
data = {'segment_id': 1, 'transcription_time': 5.2, ...}
metrics = ProcessingMetrics.from_dict(data)
```

---

## Chapter 2: JSON Serialization and Dictionaries

### What You'll Learn

Your system saves and loads millions of JSON objects. You need to convert:
- Python objects → JSON strings (serialization)
- JSON strings → Python objects (deserialization)

This chapter teaches you dictionaries, JSON operations, and the serialization pattern that runs through your entire codebase.

### 2.1 Dictionaries: Python's Power Tool

Dictionaries are key-value pairs. In JavaScript, you'd use objects. Python dictionaries are similar but more powerful:

**Basic Dictionary Creation:**

```python
# Create empty
config = {}

# Create with initial values
segment = {
    'id': 1,
    'status': 'pending',
    'duration': 30.0
}

# Access values
print(segment['id'])        # 1
print(segment['status'])    # 'pending'

# Safe access (returns None if key doesn't exist)
print(segment.get('title'))  # None
print(segment.get('title', 'Unknown'))  # 'Unknown' (default)

# Check if key exists
if 'id' in segment:
    print("ID exists")

# Add new key
segment['processed'] = True

# Delete key
del segment['processed']

# All keys
segment.keys()     # dict_keys(['id', 'status', 'duration'])

# All values
segment.values()   # dict_values([1, 'pending', 30.0])

# Key-value pairs
segment.items()    # dict_items([('id', 1), ('status', 'pending'), ...])
```

**From Your Codebase:**

```python
# From segment.py lines 72-83 (Segment.to_dict method)
def to_dict(self) -> Dict[str, Any]:
    """Convert segment to dictionary for JSON serialization."""
    return {
        'id': self.id,
        'file_path': str(self.file_path),  # Convert Path to string
        'start_time': self.start_time,
        'end_time': self.end_time,
        'duration': self.duration,
        'status': self.status,
        'created_at': self.created_at.isoformat(),  # Convert datetime to string
        'file_size': self.file_size
    }
```

**Why This Pattern?**

You can't directly save a Python `datetime` or `Path` to JSON. You must convert to JSON-compatible types:
- `datetime` → `"2024-01-15T14:30:00.123456"` (ISO format string)
- `Path` → `"data/raw_segments/segment_00001.ts"` (string)
- Objects → dictionaries

### 2.2 JSON Operations: dumps() and loads()

```python
import json

# Python object to JSON string
segment_dict = {
    'id': 1,
    'status': 'processing',
    'created_at': '2024-01-15T14:30:00'
}

# Serialize to JSON string
json_string = json.dumps(segment_dict)
# Result: '{"id": 1, "status": "processing", "created_at": "2024-01-15T14:30:00"}'

# Deserialize JSON string to Python dict
loaded_dict = json.loads(json_string)
# Result: {'id': 1, 'status': 'processing', ...}

# Pretty-print (useful for debugging)
print(json.dumps(segment_dict, indent=2))
```

**From Your Code:**

```python
# From segment.py lines 163-165
def to_json(self) -> str:
    """Convert segment to JSON string."""
    return json.dumps(self.to_dict())

# From segment.py lines 167-172
@classmethod
def from_json(cls, json_str: str) -> 'Segment':
    """Create segment from JSON string."""
    data = json.loads(json_str)
    return cls.from_dict(data)
```

### 2.3 List Comprehensions: Transforming Collections

List comprehensions are Python's way to transform lists cleanly:

**Basic Pattern:**

```python
# JavaScript:
const doubled = numbers.map(x => x * 2)

# Python:
doubled = [x * 2 for x in numbers]

# With filter:
evens = [x for x in numbers if x % 2 == 0]

# Nested (transform items in nested lists):
matrix = [[1, 2], [3, 4], [5, 6]]
flat = [x for row in matrix for x in row]
# Result: [1, 2, 3, 4, 5, 6]
```

**From Your Code:**

```python
# From segment.py line 141 (serialize words)
[w.to_dict() for w in self.words]

# From segment.py line 145 (filter non-empty words)
[w for w in self.words if w.text.strip()]

# Combined
[w.to_dict() for w in self.words if w.text.strip()]
```

### 2.4 Dict Comprehensions: Transforming Dictionaries

Similar to list comprehensions but for dicts:

```python
# Create dict from lists
ids = [1, 2, 3]
names = ['alice', 'bob', 'charlie']
users = {id: name for id, name in zip(ids, names)}
# Result: {1: 'alice', 2: 'bob', 3: 'charlie'}

# Transform keys/values
scores = {'alice': 85, 'bob': 92, 'charlie': 78}
passed = {k: v for k, v in scores.items() if v >= 80}
# Result: {'alice': 85, 'bob': 92}

# Transform structure
words = [Word("hello", 1.0, 2.0), Word("world", 2.0, 3.0)]
word_dict = {w.text: w.to_dict() for w in words}
# Result: {'hello': {...}, 'world': {...}}
```

### 2.5 The Complete Serialization Pattern

Your codebase follows a consistent pattern for all data classes:

**From segment.py:**

```python
@dataclass
class Transcript:
    segment_id: int
    text: str
    words: List[Word] = field(default_factory=list)

    def to_dict(self) -> Dict[str, Any]:
        """Convert to dict for JSON serialization."""
        return {
            'segment_id': self.segment_id,
            'text': self.text,
            'words': [w.to_dict() for w in self.words]  # Recursive!
        }

    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'Transcript':
        """Create from dict (after JSON.loads)."""
        data = data.copy()
        # Transform nested Word dicts back to Word objects
        data['words'] = [
            Word.from_dict(w) if isinstance(w, dict) else w
            for w in data['words']
        ]
        return cls(**data)
```

**Complete Flow:**

```
Python Transcript Object
    ↓ to_dict()
Dictionary (all JSON-compatible types)
    ↓ json.dumps()
JSON String '{"segment_id": 1, ...}'
    ↓ Write to File
    ↓ Later: Read from File
JSON String
    ↓ json.loads()
Dictionary
    ↓ from_dict()
Python Transcript Object
```

### Claude Code Prompts for Chapter 2

1. **"Explain the complete serialization flow using Transcript class from segment.py. Why must we convert Path and datetime to strings?"**

2. **"Show me the from_dict method for Transcript. Why does it do `isinstance(w, dict)`? What's the difference between `data['words']` and `data.get('words', [])`?"**

3. **"Create a new dataclass `Video` with: id, title, duration, tags (list). Implement to_dict() and from_dict() following the same pattern."**

### Repository Practice Exercise

**Exercise 2.1: Trace Serialization**

1. Open `src/models/segment.py`
2. Find `Transcript` class (around line 100)
3. Find `to_dict()` method (around line 163)
4. Manually trace what happens:
   ```python
   transcript = Transcript(
       segment_id=1,
       text="Hello world",
       words=[Word("hello", 1.0, 2.0), Word("world", 2.0, 3.0)]
   )

   # What does transcript.to_dict() return?
   # What does json.dumps(transcript.to_dict()) return?
   # Can you save this to a file?
   ```

**Exercise 2.2: Round-Trip Test**

Test that serialization and deserialization work:

```python
# Create object
transcript1 = Transcript(
    segment_id=1,
    text="Hello",
    words=[Word("hello", 1.0, 2.0)]
)

# Serialize to JSON
json_str = transcript1.to_json()

# Deserialize back
transcript2 = Transcript.from_json(json_str)

# Should be identical
assert transcript1 == transcript2
print("Round-trip successful!")
```

**Exercise 2.3: Work with Real Data**

1. Navigate to `data/transcripts/` in your repo
2. Pick any `transcript_*.json` file
3. Load it with `json.load()`
4. Create a Transcript object with `Transcript.from_dict()`
5. Print what you loaded

---

## Chapter 3: File Operations and Path Management

### What You'll Learn

Your system constantly reads/writes files:
- Video segments: `data/raw_segments/segment_00001.ts`
- Audio files: `data/audio_extracted/audio_00001.wav`
- Transcripts: `data/transcripts/transcript_00001.json`
- State database: `data/state/segment_index.db`

Master file operations and you'll master 30% of systems programming.

### 3.1 The `pathlib.Path` Object

Python's modern way to handle file paths. Much better than string manipulation:

```python
from pathlib import Path

# Create a path (no file needs to exist yet)
path = Path("data/transcripts/transcript_001.json")

# Convert to string (when you need a string)
str(path)  # "data/transcripts/transcript_001.json"

# Get filename only
path.name  # "transcript_001.json"

# Get directory
path.parent  # Path("data/transcripts")

# Get extension
path.suffix  # ".json"

# Get all parent directories
path.parts  # ('data', 'transcripts', 'transcript_001.json')

# Check if exists
path.exists()  # True or False

# Get file size (bytes)
path.stat().st_size  # 1234

# Create parent directories if needed
path.parent.mkdir(parents=True, exist_ok=True)

# List files in directory
data_dir = Path("data")
json_files = list(data_dir.glob("**/*.json"))  # Find all JSON files

# Join paths (cleaner than string concatenation)
data_dir = Path("data")
transcript_dir = data_dir / "transcripts"  # Uses / operator
file_path = transcript_dir / "transcript_001.json"
```

**From Your Code:**

```python
# From segment.py line 64
@dataclass
class Segment:
    file_path: Path  # Use Path, not string!

# From segment.py lines 93-96
    @property
    def exists(self) -> bool:
        """Check if the segment file exists on disk."""
        return self.file_path.exists()
```

**Why Path > Strings:**

```python
# String approach (error-prone):
path_str = "data" + "/" + "transcripts" + "/" + "transcript_001.json"
# Windows breaks this: "data\transcripts\transcript_001.json"

# Path approach (cross-platform):
path = Path("data") / "transcripts" / "transcript_001.json"
# Works on Windows, Mac, Linux automatically
```

### 3.2 Context Managers: The `with` Statement

The `with` statement ensures files are properly closed, even if an error occurs:

**Bad Way (Don't Do This):**

```python
f = open("data/transcripts/transcript_001.json", 'r')
content = f.read()
print(content)
f.close()  # What if an error happens above? File never closes!
```

**Good Way (Always Do This):**

```python
with open("data/transcripts/transcript_001.json", 'r') as f:
    content = f.read()
    print(content)
# File automatically closes here, even if error occurred
```

**From Your Code:**

```python
# From segment.py lines 253-268
def save_metadata(self, output_dir: Path) -> Path:
    """Save topic metadata to JSON file."""
    metadata_path = output_dir / "metadata.json"
    metadata_path.parent.mkdir(parents=True, exist_ok=True)

    with open(metadata_path, 'w') as f:
        json.dump(self.to_dict(), f, indent=2)

    return metadata_path
```

### 3.3 Reading and Writing Files

**Text Files:**

```python
from pathlib import Path

path = Path("data/transcripts/test.txt")

# Write text
with open(path, 'w') as f:
    f.write("Hello world")

# Read text
with open(path, 'r') as f:
    content = f.read()  # Entire file as string

# Read line by line
with open(path, 'r') as f:
    for line in f:
        print(line.strip())

# Read as list of lines
with open(path, 'r') as f:
    lines = f.readlines()  # List of strings, each is a line
```

**JSON Files (Common in Your System):**

```python
import json
from pathlib import Path

path = Path("data/transcripts/transcript_001.json")

# Write JSON
data = {'segment_id': 1, 'text': 'Hello'}
with open(path, 'w') as f:
    json.dump(data, f, indent=2)  # Writes formatted JSON

# Read JSON
with open(path, 'r') as f:
    data = json.load(f)  # Returns Python dict/list
```

**Binary Files (Audio, Video):**

```python
from pathlib import Path

# Read binary (for audio/video processing)
audio_path = Path("data/audio_extracted/audio_001.wav")
with open(audio_path, 'rb') as f:  # 'rb' = read binary
    audio_data = f.read()

# Write binary
with open(audio_path, 'wb') as f:  # 'wb' = write binary
    f.write(audio_data)
```

### 3.4 Directory Operations

```python
from pathlib import Path

data_dir = Path("data")

# Create directory with parents
data_dir.mkdir(parents=True, exist_ok=True)
# parents=True: create parent dirs if needed
# exist_ok=True: don't error if already exists

# Create subdirectories
(data_dir / "raw_segments").mkdir(parents=True, exist_ok=True)
(data_dir / "audio_extracted").mkdir(parents=True, exist_ok=True)
(data_dir / "transcripts").mkdir(parents=True, exist_ok=True)
(data_dir / "state").mkdir(parents=True, exist_ok=True)

# List files
transcript_dir = Path("data/transcripts")
json_files = list(transcript_dir.glob("*.json"))  # All JSON files
all_files = list(transcript_dir.glob("*"))  # All files

# List recursively
all_json = list(data_dir.glob("**/*.json"))  # All JSON, any depth

# Delete file
json_files[0].unlink()

# Delete directory (must be empty)
empty_dir = Path("data/temp")
empty_dir.rmdir()

# Delete directory recursively
import shutil
shutil.rmtree(data_dir)  # CAREFUL! Deletes everything
```

### Claude Code Prompts for Chapter 3

1. **"Explain why `Path` is used instead of strings in your segment.py file. What problem does it solve?"**

2. **"Show me how to read a transcript JSON file, modify it, and write it back. Use the `with` statement."**

3. **"Write code that creates the complete data directory structure: data/raw_segments, data/audio_extracted, data/transcripts, data/state. Use Path and mkdir."**

### Repository Practice Exercise

**Exercise 3.1: Explore Your Directory Structure**

```python
from pathlib import Path

# Find your repository root
repo_root = Path(".")  # Current directory
print(f"Repository root: {repo_root.absolute()}")

# List main directories
for item in repo_root.iterdir():
    if item.is_dir():
        print(f"Directory: {item.name}")

# Count all Python files
py_files = list(repo_root.glob("**/*.py"))
print(f"Total Python files: {len(py_files)}")

# List data directory contents
data_dir = repo_root / "data"
if data_dir.exists():
    for subdir in data_dir.iterdir():
        if subdir.is_dir():
            file_count = len(list(subdir.glob("*")))
            print(f"{subdir.name}: {file_count} files")
```

**Exercise 3.2: Work with Existing Data**

```python
from pathlib import Path
import json

# Find first transcript JSON
transcript_dir = Path("data/transcripts")
json_files = list(transcript_dir.glob("*.json"))

if json_files:
    # Read first file
    transcript_path = json_files[0]
    with open(transcript_path, 'r') as f:
        transcript_data = json.load(f)

    print(f"Loaded: {transcript_path.name}")
    print(f"Keys: {transcript_data.keys()}")
    print(f"File size: {transcript_path.stat().st_size} bytes")
```

**Exercise 3.3: Create a Backup System**

```python
from pathlib import Path
import shutil
from datetime import datetime

# Create backup of data directory
data_dir = Path("data")
backup_dir = Path("backups")
backup_dir.mkdir(exist_ok=True)

# Create timestamped backup
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
backup_path = backup_dir / f"backup_{timestamp}"

# Copy entire directory
shutil.copytree(data_dir, backup_path)
print(f"Backup created: {backup_path}")

# List all backups
backups = sorted(backup_dir.glob("backup_*"))
print(f"Total backups: {len(backups)}")
```

---

## Chapter 4: Functions and Error Handling

### What You'll Learn

Functions are reusable blocks of code. Error handling ensures your system survives problems gracefully. These two concepts combined make your code resilient.

### 4.1 Function Basics

```python
# Simple function
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))  # "Hello, Alice!"

# Function with type hints (best practice in your codebase)
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b

# Function with default parameter
def create_user(name: str, role: str = "user") -> dict:
    """Create a user with optional role."""
    return {'name': name, 'role': role}

create_user("Alice")              # role defaults to "user"
create_user("Alice", role="admin")  # or specify it

# Function with multiple return values
def get_min_max(numbers: list) -> tuple:
    """Return (min, max) of a list."""
    return (min(numbers), max(numbers))

min_val, max_val = get_min_max([1, 5, 3])  # Unpacking
```

**From Your Code:**

```python
# From transcriber.py lines 130-137 (function signature)
def extract_audio(
    self,
    video_path: Path,
    output_path: Path,
    retries: int = 3
) -> bool:
    """
    Extract audio from video segment.

    Args:
        video_path: Path to .ts video file
        output_path: Where to save .wav file
        retries: Number of retry attempts

    Returns:
        True if successful, False otherwise
    """
```

### 4.2 Try/Except/Finally: Error Handling

Errors happen. Network timeouts. Files missing. APIs down. Your code must handle them:

**Basic Pattern:**

```python
try:
    # Code that might fail
    result = risky_operation()
except Exception as e:
    # Handle the error
    print(f"Error occurred: {e}")

# Example with file:
try:
    with open("missing_file.json", 'r') as f:
        data = json.load(f)
except FileNotFoundError:
    print("File doesn't exist!")
except json.JSONDecodeError:
    print("File isn't valid JSON!")
except Exception as e:
    print(f"Unknown error: {e}")
```

**Catch Specific Exceptions (Better):**

```python
try:
    # Something fails
    pass
except FileNotFoundError as e:
    print(f"File not found: {e.filename}")
except ValueError as e:
    print(f"Invalid value: {e}")
except Exception as e:  # Catch-all for unexpected errors
    print(f"Unexpected error: {e}")
finally:
    # This runs WHETHER OR NOT an error occurred
    print("Cleanup code here")
```

**From Your Code:**

```python
# From transcriber.py lines 144-219 (complete extract_audio with error handling)
def extract_audio(self, video_path: Path, output_path: Path, retries: int = 3) -> bool:
    """Extract audio from video segment."""

    for attempt in range(retries):  # Try multiple times
        try:
            # Validate inputs
            if not video_path.exists():
                logger.error(f"Video file not found: {video_path}")
                return False

            # Build FFmpeg command
            cmd = [
                'ffmpeg',
                '-i', str(video_path),
                '-q:a', '9',  # Quality
                '-n',  # No overwrite
                str(output_path)
            ]

            # Run FFmpeg
            result = subprocess.run(
                cmd,
                capture_output=True,
                check=True,
                timeout=60
            )

            logger.info(f"Audio extracted: {output_path}")
            return True

        except subprocess.TimeoutExpired:
            logger.warning(f"Attempt {attempt + 1}: FFmpeg timeout")
            if attempt == retries - 1:  # Last attempt
                logger.error(f"Failed to extract audio after {retries} attempts")
                return False
            # Try again

        except subprocess.CalledProcessError as e:
            logger.error(f"FFmpeg error: {e.stderr.decode()}")
            if attempt == retries - 1:
                return False

        except Exception as e:
            logger.error(f"Unexpected error: {e}", exc_info=True)
            return False

    return False
```

### 4.3 Retry Logic: Essential for Resilience

APIs fail. Networks drop. Your system must retry:

```python
import time

def call_api_with_retry(url: str, max_retries: int = 3) -> dict:
    """Call API, retrying on failure."""

    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()  # Raise if 4xx or 5xx
            return response.json()

        except requests.RequestException as e:
            logger.warning(f"Attempt {attempt + 1}/{max_retries} failed: {e}")

            if attempt == max_retries - 1:  # Last attempt
                raise  # Give up

            # Wait before retry (exponential backoff)
            wait_time = 2 ** attempt  # 1s, 2s, 4s
            logger.info(f"Waiting {wait_time}s before retry...")
            time.sleep(wait_time)

    return {}  # Default if all retries failed
```

**From Your Code:**

Your Gemini LLM service uses exactly this pattern (Chapter 11).

### 4.4 Logging Instead of Print

Professional code uses logging, not `print()`:

```python
import logging

# Set up logging (usually in main.py)
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

# Use logging
logger.debug("Detailed info for debugging")
logger.info("Important event occurred")
logger.warning("Something unexpected happened")
logger.error("Serious problem, operation failed")
```

**From Your Code:**

```python
# From transcriber.py (throughout)
logger = logging.getLogger(__name__)

logger.info(f"Transcription started for segment {segment_id}")
logger.warning(f"Attempt {attempt + 1}: {e}")
logger.error(f"Failed after {retries} attempts", exc_info=True)
```

### Claude Code Prompts for Chapter 4

1. **"Explain the extract_audio function from transcriber.py. Why does it use try/except inside a for loop? What happens on the last retry?"**

2. **"Create a function that downloads a file from a URL with retry logic and proper error handling. Include logging."**

3. **"Add error handling to the to_dict() method from segment.py to handle if any datetime is invalid."**

### Repository Practice Exercise

**Exercise 4.1: Trace Error Handling**

```python
# Open transcriber.py and find extract_audio method
# Answer these questions:
# 1. What exceptions can it raise or catch?
# 2. What happens on attempt 1? 2? 3?
# 3. When does it return False vs True?
# 4. Why is `exc_info=True` in logger.error?
```

**Exercise 4.2: Create a Safe JSON Loader**

```python
import json
import logging
from pathlib import Path
from typing import Optional, Dict, Any

logger = logging.getLogger(__name__)

def safe_load_json(file_path: Path) -> Optional[Dict[str, Any]]:
    """Safely load JSON file with error handling."""

    try:
        # Validate input
        if not isinstance(file_path, Path):
            file_path = Path(file_path)

        if not file_path.exists():
            logger.error(f"File not found: {file_path}")
            return None

        # Load JSON
        with open(file_path, 'r') as f:
            data = json.load(f)

        logger.info(f"Loaded JSON from {file_path}")
        return data

    except json.JSONDecodeError as e:
        logger.error(f"Invalid JSON in {file_path}: {e}")
        return None

    except Exception as e:
        logger.error(f"Error loading {file_path}: {e}", exc_info=True)
        return None

# Test it
test_file = Path("data/transcripts/transcript_00001.json")
data = safe_load_json(test_file)
if data:
    print(f"Loaded: {data.keys()}")
else:
    print("Failed to load")
```

**Exercise 4.3: Implement Retry Pattern**

```python
import time
import logging

logger = logging.getLogger(__name__)

def call_with_retry(func, *args, max_retries=3, **kwargs):
    """Generic retry wrapper for any function."""

    for attempt in range(max_retries):
        try:
            return func(*args, **kwargs)
        except Exception as e:
            logger.warning(f"Attempt {attempt + 1}/{max_retries} failed: {e}")

            if attempt == max_retries - 1:
                logger.error(f"All {max_retries} attempts failed")
                raise

            wait_time = 2 ** attempt
            logger.info(f"Retrying in {wait_time}s...")
            time.sleep(wait_time)

# Use it
def risky_operation():
    import random
    if random.random() < 0.7:  # Fails 70% of time
        raise ValueError("Random failure!")
    return "Success!"

result = call_with_retry(risky_operation, max_retries=5)
print(result)
```

---

## Chapter 5: Object-Oriented Programming Basics

### What You'll Learn

Classes bundle related data and functions together. Your StateManager is a perfect example: it bundles all database operations in one place.

### 5.1 Classes: Bundling Data and Methods

```python
class BankAccount:
    """Example class for teaching OOP."""

    def __init__(self, account_holder: str, balance: float = 0):
        """Initialize a bank account."""
        self.account_holder = account_holder
        self.balance = balance

    def deposit(self, amount: float) -> None:
        """Add money to account."""
        self.balance += amount
        print(f"{self.account_holder} deposited ${amount}")

    def withdraw(self, amount: float) -> bool:
        """Remove money from account."""
        if self.balance >= amount:
            self.balance -= amount
            print(f"{self.account_holder} withdrew ${amount}")
            return True
        else:
            print("Insufficient funds!")
            return False

# Usage
account = BankAccount("Alice", balance=1000)
account.deposit(500)     # Calls the deposit METHOD
account.withdraw(200)    # Calls the withdraw METHOD
print(account.balance)   # Access the balance ATTRIBUTE
```

**Key Terms:**
- **Class**: The blueprint (BankAccount)
- **Instance**: A specific object created from the blueprint (account)
- **Method**: A function inside a class (deposit, withdraw)
- **Attribute**: A variable inside a class (balance, account_holder)
- **`self`**: Reference to the current instance (like `this` in JavaScript)
- **`__init__`**: Constructor that runs when you create an instance

### 5.2 Your StateManager as a Real Example

StateManager is a perfect real-world class. Let's understand it:

**From state_manager.py lines 25-43:**

```python
class StateManager:
    """Manages segment metadata in SQLite database."""

    def __init__(self, db_path: Path = Path("data/state/segment_index.db")):
        """Initialize state manager with database path."""
        self.db_path = db_path  # Store the path
        self.connection = None   # Will hold the database connection

        # Create database if it doesn't exist
        self._init_database()

    def _init_database(self):
        """Create database tables if they don't exist."""
        with self._get_connection() as conn:
            # Create tables here
            conn.execute("""
                CREATE TABLE IF NOT EXISTS segments (
                    id INTEGER PRIMARY KEY,
                    file_path TEXT,
                    status TEXT,
                    ...
                )
            """)
```

**What's Happening:**

1. When you create an instance:
   ```python
   state_manager = StateManager()
   ```

2. Python calls `__init__` automatically
3. `__init__` stores the database path in `self.db_path`
4. `__init__` calls `_init_database()` to create the database

**Why Use a Class Here?**

Instead of:
```python
# Without a class - tedious and error-prone
db_path = Path("data/state/segment_index.db")

# Need to pass db_path to every function
def add_segment(db_path, segment_id, status):
    # ...

def get_segment(db_path, segment_id):
    # ...

def update_segment(db_path, segment_id, status):
    # ...

# Usage:
add_segment(db_path, 1, "pending")
get_segment(db_path, 1)
```

You can do:
```python
# With a class - clean and organized
state_manager = StateManager()

# db_path is stored in state_manager.db_path
state_manager.add_segment(1, "pending")
state_manager.get_segment(1)
```

### 5.3 Instance Variables vs Class Variables

```python
class VideoSegment:
    max_duration = 30  # CLASS variable - shared by all instances

    def __init__(self, id: int, duration: float):
        self.id = id              # INSTANCE variable - unique per segment
        self.duration = duration  # INSTANCE variable

# Each segment has its own id and duration
seg1 = VideoSegment(1, 30.0)
seg2 = VideoSegment(2, 30.0)

print(seg1.id)            # 1
print(seg2.id)            # 2
print(seg1.max_duration)  # 30 (same for all)
print(VideoSegment.max_duration)  # 30 (can access on class itself)
```

### 5.4 Methods: Functions Inside Classes

```python
class Transcript:
    def __init__(self, segment_id: int, text: str):
        self.segment_id = segment_id
        self.text = text

    # Instance method (receives self)
    def word_count(self) -> int:
        """Count words in transcript."""
        return len(self.text.split())

    # Another instance method
    def is_empty(self) -> bool:
        """Check if transcript is empty."""
        return len(self.text.strip()) == 0

    # Class method (receives cls, not self)
    @classmethod
    def from_json(cls, json_str: str) -> 'Transcript':
        """Create transcript from JSON."""
        data = json.loads(json_str)
        return cls(data['segment_id'], data['text'])

    # Static method (no self or cls)
    @staticmethod
    def validate_text(text: str) -> bool:
        """Check if text is valid."""
        return len(text) > 0

# Usage:
transcript = Transcript(1, "Hello world")
print(transcript.word_count())  # 2
print(Transcript.validate_text("test"))  # True
```

### 5.5 Private Methods and Variables

Methods starting with `_` or `__` are private (by convention):

```python
class SecureData:
    def __init__(self, api_key: str):
        self._api_key = api_key  # Private (convention: don't access)
        self.__secret = "hidden"  # Private (harder to access)

    def _validate(self):
        """Private method (internal use only)."""
        pass

    def get_data(self):
        """Public method (safe to call)."""
        self._validate()  # Can call private methods internally
        return "data"

data = SecureData("key123")
data.get_data()          # OK - public method
# data._validate()       # Bad practice - accessing private
# data.__secret          # Error - actually private
```

### Claude Code Prompts for Chapter 5

1. **"Explain the StateManager class from state_manager.py. Why is it better than having separate functions? What would be different without a class?"**

2. **"Show me all the methods in StateManager. Which ones are public (`add_segment`, `get_segment`) and which are private (`_get_connection`, `_init_database`)? Why?"**

3. **"Create a class called `ProcessingQueue` that stores video processing tasks. Include: `__init__`, `add_task()`, `get_next_task()`, and `task_count()` methods."**

### Repository Practice Exercise

**Exercise 5.1: Understand StateManager**

```python
from src.services.state_manager import StateManager
from src.models.segment import Segment
from pathlib import Path

# Create an instance
state_manager = StateManager()

# This calls __init__ which:
# 1. Stores db_path
# 2. Calls _init_database() to create tables

# Now you can use public methods
state_manager.add_segment(...)
state_manager.get_segment(...)

# Question: What would happen if you tried to access _init_database directly?
# Answer: It's private (underscore prefix) - bad practice
```

**Exercise 5.2: Create a Transcript Manager Class**

```python
import json
from pathlib import Path
from src.models.segment import Transcript
from typing import Optional

class TranscriptManager:
    """Manages saving/loading transcripts from disk."""

    def __init__(self, transcript_dir: Path = Path("data/transcripts")):
        """Initialize with directory for transcript files."""
        self.transcript_dir = transcript_dir
        self.transcript_dir.mkdir(parents=True, exist_ok=True)

    def save(self, transcript: Transcript) -> Path:
        """Save transcript to JSON file."""
        file_path = self.transcript_dir / f"transcript_{transcript.segment_id}.json"
        with open(file_path, 'w') as f:
            json.dump(transcript.to_dict(), f, indent=2)
        return file_path

    def load(self, segment_id: int) -> Optional[Transcript]:
        """Load transcript from JSON file."""
        file_path = self.transcript_dir / f"transcript_{segment_id}.json"
        if not file_path.exists():
            return None

        with open(file_path, 'r') as f:
            data = json.load(f)

        return Transcript.from_dict(data)

    def list_all(self):
        """List all transcript IDs."""
        return [int(f.stem.split('_')[1]) for f in self.transcript_dir.glob("transcript_*.json")]

# Test it
manager = TranscriptManager()
transcript_ids = manager.list_all()
print(f"Found {len(transcript_ids)} transcripts: {transcript_ids[:5]}")
```

**Exercise 5.3: Add Caching to TranscriptManager**

```python
# Modify TranscriptManager to cache loaded transcripts
# This way, if you load the same transcript twice, it's already in memory

class TranscriptManager:
    def __init__(self, transcript_dir: Path = Path("data/transcripts")):
        self.transcript_dir = transcript_dir
        self._cache = {}  # Private cache dictionary

    def load(self, segment_id: int) -> Optional[Transcript]:
        """Load transcript with caching."""
        # Check cache first
        if segment_id in self._cache:
            return self._cache[segment_id]

        # Load from disk
        file_path = self.transcript_dir / f"transcript_{segment_id}.json"
        if not file_path.exists():
            return None

        with open(file_path, 'r') as f:
            data = json.load(f)

        transcript = Transcript.from_dict(data)
        self._cache[segment_id] = transcript  # Cache it
        return transcript

    def clear_cache(self):
        """Clear the cache."""
        self._cache.clear()
```

---

## Chapter 6: SQLite Databases

### What You'll Learn

Your system needs to survive crashes. Without persistent storage, you'd lose all progress if the system restarts. SQLite stores segment metadata so you can recover.

### 6.1 SQLite Basics

SQLite is a lightweight database perfect for local storage:

```python
import sqlite3
from pathlib import Path

# Connect to database (creates file if it doesn't exist)
db_path = Path("data/state/test.db")
conn = sqlite3.connect(str(db_path))

# Create a cursor (used to execute SQL)
cursor = conn.cursor()

# Create a table
cursor.execute("""
    CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        email TEXT,
        age INTEGER
    )
""")

# Insert data
cursor.execute(
    "INSERT INTO users (name, email, age) VALUES (?, ?, ?)",
    ("Alice", "alice@example.com", 30)
)

# Commit changes (save to disk)
conn.commit()

# Query data
cursor.execute("SELECT * FROM users")
rows = cursor.fetchall()
for row in rows:
    print(row)  # (1, 'Alice', 'alice@example.com', 30)

# Close connection
conn.close()
```

### 6.2 Context Managers for Safe Database Operations

Your code uses context managers for safety:

**From state_manager.py lines 45-58:**

```python
@contextmanager
def _get_connection(self):
    """Get database connection (auto-commits and closes)."""
    conn = sqlite3.connect(str(self.db_path))
    conn.row_factory = sqlite3.Row  # Return dicts instead of tuples
    try:
        yield conn  # Hand connection to calling code
        conn.commit()  # Auto-commit on success
    except Exception:
        conn.rollback()  # Rollback on error
        raise
    finally:
        conn.close()  # Always close

# Usage:
with self._get_connection() as conn:
    cursor = conn.cursor()
    cursor.execute("INSERT INTO segments ...")
    # Auto-commits here
```

**Why This Pattern?**

```python
# Without context manager (error-prone):
conn = sqlite3.connect("data.db")
try:
    cursor = conn.cursor()
    cursor.execute("INSERT INTO segments ...")
    conn.commit()
except Exception:
    conn.rollback()
finally:
    conn.close()

# With context manager (safe and clean):
with get_connection() as conn:
    cursor = conn.cursor()
    cursor.execute("INSERT INTO segments ...")
    # Auto-commits and closes
```

### 6.3 Parameterized Queries (Safety!)

NEVER use string concatenation for SQL - it's vulnerable to SQL injection:

```python
import sqlite3

# BAD (vulnerable!):
segment_id = 1
cursor.execute(f"SELECT * FROM segments WHERE id = {segment_id}")
# What if segment_id = "1; DROP TABLE segments;" ???

# GOOD (safe):
cursor.execute("SELECT * FROM segments WHERE id = ?", (segment_id,))
# Parameter is safely escaped

# With named parameters:
cursor.execute(
    "SELECT * FROM segments WHERE id = ? AND status = ?",
    (1, "pending")
)

# Dict parameters:
cursor.execute(
    "INSERT INTO segments (id, status) VALUES (:id, :status)",
    {'id': 1, 'status': 'pending'}
)
```

### 6.4 Row Factories: Working with Dicts

```python
import sqlite3

conn = sqlite3.connect("data.db")
conn.row_factory = sqlite3.Row  # Return dicts not tuples
cursor = conn.cursor()

cursor.execute("SELECT id, name, email FROM users")

# Now you get dicts:
for row in cursor.fetchall():
    print(row['name'])   # Much better than row[1]
    print(row['email'])
    data_dict = dict(row)  # Convert to regular dict if needed
```

### 6.5 Real Example: StateManager

**From state_manager.py lines 60-133 (schema creation):**

```python
def _init_database(self):
    """Create database tables if they don't exist."""
    with self._get_connection() as conn:
        cursor = conn.cursor()

        # Segments table
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS segments (
                id INTEGER PRIMARY KEY,
                file_path TEXT NOT NULL UNIQUE,
                start_time REAL,
                end_time REAL,
                duration REAL,
                status TEXT,
                created_at TEXT,
                file_size INTEGER,
                processed_at TEXT
            )
        """)

        # Transcripts table
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS transcripts (
                segment_id INTEGER PRIMARY KEY,
                text TEXT,
                words_json TEXT,
                transcribed_at TEXT,
                FOREIGN KEY (segment_id) REFERENCES segments(id)
            )
        """)

        # Create indexes for fast queries
        cursor.execute("CREATE INDEX IF NOT EXISTS idx_status ON segments(status)")
        cursor.execute("CREATE INDEX IF NOT EXISTS idx_created ON segments(created_at)")
```

**Adding Data:**

```python
# From state_manager.py lines 137-160
def add_segment(self, segment: Segment) -> bool:
    """Add or update a segment in the database."""
    try:
        with self._get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                INSERT OR REPLACE INTO segments
                (id, file_path, start_time, end_time, duration, status, created_at, file_size)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?)
            """, (
                segment.id,
                str(segment.file_path),
                segment.start_time,
                segment.end_time,
                segment.duration,
                segment.status,
                segment.created_at.isoformat(),
                segment.file_size
            ))
        return True
    except Exception as e:
        logger.error(f"Error adding segment: {e}")
        return False

# Query data:
def get_segment(self, segment_id: int) -> Optional[Segment]:
    """Get a segment by ID."""
    with self._get_connection() as conn:
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM segments WHERE id = ?", (segment_id,))
        row = cursor.fetchone()

        if not row:
            return None

        # Convert row to Segment object
        return Segment(
            id=row['id'],
            file_path=Path(row['file_path']),
            start_time=row['start_time'],
            # ... etc
        )
```

### Claude Code Prompts for Chapter 6

1. **"Explain the _get_connection context manager. Why is `conn.commit()` inside the try block? What happens if an exception occurs?"**

2. **"Show me the add_segment and get_segment methods from state_manager.py. Why does add_segment use INSERT OR REPLACE?"**

3. **"Create a table for storing topic metadata with: id, title, start_time, end_time, confidence. Write SQL to create it."**

### Repository Practice Exercise

**Exercise 6.1: Explore Your Database**

```python
import sqlite3
from pathlib import Path

db_path = Path("data/state/segment_index.db")

if db_path.exists():
    conn = sqlite3.connect(str(db_path))
    conn.row_factory = sqlite3.Row
    cursor = conn.cursor()

    # List all tables
    cursor.execute("SELECT name FROM sqlite_master WHERE type='table'")
    tables = cursor.fetchall()
    print(f"Tables: {[t['name'] for t in tables]}")

    # Describe segments table
    cursor.execute("PRAGMA table_info(segments)")
    columns = cursor.fetchall()
    print("\nSegments columns:")
    for col in columns:
        print(f"  {col['name']}: {col['type']}")

    # Count segments
    cursor.execute("SELECT COUNT(*) as count FROM segments")
    count = cursor.fetchone()['count']
    print(f"\nTotal segments: {count}")

    # List first 5 segments
    cursor.execute("SELECT id, status, created_at FROM segments LIMIT 5")
    print("\nFirst 5 segments:")
    for row in cursor.fetchall():
        print(f"  ID {row['id']}: {row['status']} ({row['created_at']})")

    conn.close()
else:
    print("Database doesn't exist yet")
```

**Exercise 6.2: Create a Test Database**

```python
import sqlite3
from pathlib import Path
from datetime import datetime

# Create test database
test_db = Path("data/state/test.db")
test_db.parent.mkdir(parents=True, exist_ok=True)

conn = sqlite3.connect(str(test_db))
cursor = conn.cursor()

# Create table
cursor.execute("""
    CREATE TABLE IF NOT EXISTS test_segments (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        duration REAL,
        status TEXT,
        created_at TEXT
    )
""")

# Insert test data
cursor.execute("""
    INSERT INTO test_segments (id, name, duration, status, created_at)
    VALUES (?, ?, ?, ?, ?)
""", (1, "segment_1", 30.0, "pending", datetime.now().isoformat()))

cursor.execute("""
    INSERT INTO test_segments (id, name, duration, status, created_at)
    VALUES (?, ?, ?, ?, ?)
""", (2, "segment_2", 30.0, "transcribed", datetime.now().isoformat()))

conn.commit()

# Query it back
cursor.execute("SELECT * FROM test_segments ORDER BY id")
for row in cursor.fetchall():
    print(row)

conn.close()
print(f"Created: {test_db}")
```

**Exercise 6.3: Practice Parameterized Queries**

```python
import sqlite3

conn = sqlite3.connect(":memory:")  # In-memory database (temp)
cursor = conn.cursor()

# Create table
cursor.execute("""
    CREATE TABLE transcripts (
        id INTEGER PRIMARY KEY,
        segment_id INTEGER,
        text TEXT,
        confidence REAL
    )
""")

# Insert with parameters (SAFE!)
data = [
    (1, "Hello world", 0.95),
    (1, "This is a test", 0.92),
    (2, "Another segment", 0.88),
]

for segment_id, text, confidence in data:
    cursor.execute(
        "INSERT INTO transcripts (segment_id, text, confidence) VALUES (?, ?, ?)",
        (segment_id, text, confidence)
    )

conn.commit()

# Query with parameters
search_segment = 1
cursor.execute(
    "SELECT * FROM transcripts WHERE segment_id = ?",
    (search_segment,)
)

print(f"Results for segment {search_segment}:")
for row in cursor.fetchall():
    print(f"  {row[2]}: {row[3]*100:.1f}%")

conn.close()
```

---

## Chapter 7: Configuration and Environment Variables

### What You'll Learn

Your system needs configuration: API keys, database paths, model names, etc. Configuration should be easy to change without editing code.

### 7.1 YAML Configuration Files

YAML is human-readable configuration format:

**From config/settings.yaml:**

```yaml
stream:
  youtube_url: "https://www.youtube.com/watch?v=..."
  segment_duration: 30
  quality: "best"

transcription:
  provider: "elevenlabs"  # or "groq", "whisper"
  language: "en"
  sample_rate: 16000

llm:
  gemini:
    model: "gemini-2.5-flash"
    temperature: 0.3
    max_tokens: 2000

storage:
  data_dir: "data"
  output_dir: "output"
  max_segments_in_memory: 50

logging:
  level: "INFO"
  file: "logs/system.log"
```

**Reading YAML in Python:**

```python
import yaml
from pathlib import Path

config_path = Path("config/settings.yaml")

with open(config_path, 'r') as f:
    config = yaml.safe_load(f)

# Access nested values
youtube_url = config['stream']['youtube_url']
segment_duration = config['stream']['segment_duration']
llm_model = config['llm']['gemini']['model']
```

### 7.2 Environment Variables

Sensitive data (API keys) should NEVER be in YAML files or committed to git:

```python
import os

# Read from environment
GEMINI_API_KEY = os.getenv('GEMINI_API_KEY')
ELEVENLABS_API_KEY = os.getenv('ELEVENLABS_API_KEY')

# With defaults
DEBUG = os.getenv('DEBUG', 'False') == 'True'

# Validate that required variables exist
if not GEMINI_API_KEY:
    raise ValueError("GEMINI_API_KEY environment variable not set!")
```

**Set Environment Variables:**

```bash
# Linux/Mac
export GEMINI_API_KEY="sk-..."
export ELEVENLABS_API_KEY="..."

# Windows
set GEMINI_API_KEY=sk-...

# Or in .env file (python-dotenv)
# GEMINI_API_KEY=sk-...
# ELEVENLABS_API_KEY=...
```

**Using .env File:**

```python
from dotenv import load_dotenv
import os

# Load from .env file (must be in .gitignore!)
load_dotenv()

api_key = os.getenv('GEMINI_API_KEY')
```

### 7.3 Merging Configurations

Your system needs to handle:
1. Default config (checked into git)
2. Environment-specific overrides
3. Runtime overrides

**From main.py lines 102-133:**

```python
def _merge_configs(self, base: dict, runtime: dict) -> dict:
    """Merge runtime config into base config."""
    merged = copy.deepcopy(base)

    for key, value in runtime.items():
        if key in merged and isinstance(merged[key], dict) and isinstance(value, dict):
            # Recursive merge for nested dicts
            merged[key] = self._merge_configs(merged[key], value)
        else:
            # Override value
            merged[key] = value

    return merged
```

**Example:**

```python
# base_config from settings.yaml
base_config = {
    'stream': {'segment_duration': 30},
    'llm': {'temperature': 0.3}
}

# runtime overrides
runtime_config = {
    'llm': {'temperature': 0.5}  # Override just temperature
}

merged = merge_configs(base_config, runtime_config)
# Result:
# {
#     'stream': {'segment_duration': 30},  # Unchanged
#     'llm': {'temperature': 0.5}  # Updated
# }
```

### 7.4 Environment Variable Expansion in Config

Sometimes you want to reference environment variables in YAML:

```yaml
# config/settings.yaml
apis:
  gemini:
    api_key: "${GEMINI_API_KEY}"
  elevenlabs:
    api_key: "${ELEVENLABS_API_KEY}"
```

**From main.py lines 538-563:**

```python
def _expand_env_vars(self, value):
    """Replace ${VAR_NAME} with environment variables."""
    if isinstance(value, str):
        # Find all ${VAR_NAME} patterns
        import re
        pattern = r'\$\{([^}]+)\}'

        def replacer(match):
            var_name = match.group(1)
            return os.getenv(var_name, match.group(0))

        return re.sub(pattern, replacer, value)

    elif isinstance(value, dict):
        # Recursively expand all dict values
        return {k: self._expand_env_vars(v) for k, v in value.items()}

    elif isinstance(value, list):
        # Recursively expand all list items
        return [self._expand_env_vars(item) for item in value]

    return value
```

### Claude Code Prompts for Chapter 7

1. **"Show me how main.py loads config/settings.yaml and merges it with environment variables. Why is this pattern important?"**

2. **"Explain the difference between config values in settings.yaml vs environment variables. Which should contain API keys?"**

3. **"Create a settings.yaml file with sections for: stream, transcription, llm, storage, and logging. Include reasonable defaults."**

### Repository Practice Exercise

**Exercise 7.1: Load Your Actual Config**

```python
import yaml
from pathlib import Path

config_path = Path("config/settings.yaml")

with open(config_path, 'r') as f:
    config = yaml.safe_load(f)

# Explore the config
print(yaml.dump(config, default_flow_style=False))

# Access specific values
print(f"YouTube URL: {config['stream']['youtube_url']}")
print(f"Segment duration: {config['stream']['segment_duration']}")
print(f"LLM model: {config['llm']['gemini']['model']}")
```

**Exercise 7.2: Test Environment Variable Expansion**

```python
import os
import yaml
import re
from pathlib import Path

# Set environment variables
os.environ['TEST_API_KEY'] = 'secret123'
os.environ['TEST_MODEL'] = 'gpt-4'

# Test config with placeholders
test_config = {
    'apis': {
        'key': '${TEST_API_KEY}',
        'model': '${TEST_MODEL}'
    }
}

def expand_env_vars(obj):
    """Expand ${VAR} in config."""
    if isinstance(obj, str):
        pattern = r'\$\{([^}]+)\}'
        def replace_var(match):
            var_name = match.group(1)
            return os.getenv(var_name, match.group(0))
        return re.sub(pattern, replace_var, obj)

    elif isinstance(obj, dict):
        return {k: expand_env_vars(v) for k, v in obj.items()}

    elif isinstance(obj, list):
        return [expand_env_vars(item) for item in obj]

    return obj

expanded = expand_env_vars(test_config)
print(expanded)  # Should show actual API key
```

**Exercise 7.3: Create Config Manager Class**

```python
import yaml
import os
from pathlib import Path
from typing import Any

class ConfigManager:
    """Manages application configuration."""

    def __init__(self, config_path: Path = Path("config/settings.yaml")):
        """Load configuration from file."""
        self.config_path = config_path

        # Load YAML
        with open(config_path, 'r') as f:
            self.config = yaml.safe_load(f)

        # Expand environment variables
        self.config = self._expand_env_vars(self.config)

    def _expand_env_vars(self, obj):
        """Expand ${VAR} references."""
        if isinstance(obj, str):
            import re
            pattern = r'\$\{([^}]+)\}'

            def replace(match):
                var = match.group(1)
                return os.getenv(var, match.group(0))

            return re.sub(pattern, replace, obj)

        elif isinstance(obj, dict):
            return {k: self._expand_env_vars(v) for k, v in obj.items()}

        elif isinstance(obj, list):
            return [self._expand_env_vars(item) for item in obj]

        return obj

    def get(self, path: str, default=None) -> Any:
        """Get config value by dot notation.

        Example: config.get('stream.segment_duration')
        """
        parts = path.split('.')
        value = self.config

        for part in parts:
            if isinstance(value, dict) and part in value:
                value = value[part]
            else:
                return default

        return value

# Test it
config = ConfigManager()
print(config.get('stream.segment_duration'))  # 30
print(config.get('llm.gemini.temperature'))    # 0.3
print(config.get('nonexistent.path', 'default'))  # 'default'
```

---

# PART II: DATA & AI

## Chapter 8: API Integration and HTTP Requests

### What You'll Learn

Your system integrates 4+ external APIs:
- ElevenLabs (speech-to-text)
- Google Gemini (LLM analysis)
- Groq (alternative STT)
- YouTube (via yt-dlp)

This chapter teaches the patterns used to safely interact with external services.

### 8.1 Working with Binary Data: BytesIO

When you send audio to an API, you need to read the file into memory as binary data:

```python
from io import BytesIO
from pathlib import Path

# Read binary file (audio)
audio_path = Path("data/audio_extracted/audio_001.wav")

with open(audio_path, 'rb') as f:  # 'rb' = read binary
    audio_bytes = f.read()

# Create in-memory file object
audio_io = BytesIO(audio_bytes)

# Use with API
response = api_client.transcribe(
    file=audio_io,
    language='en'
)
```

**From your code (transcriber.py lines 257-272):**

```python
def transcribe(self, audio_path: Path, ...) -> Optional[Transcript]:
    """Transcribe audio using ElevenLabs API."""

    try:
        # Read audio file into memory
        with open(audio_path, 'rb') as f:
            audio_data = BytesIO(f.read())

        # Send to API
        transcription = self.client.speech_to_text.convert(
            file=audio_data,
            language_code=language
        )

        # Parse response
        return self._parse_response(transcription)

    except Exception as e:
        logger.error(f"Transcription failed: {e}")
        return None
```

### 8.2 API Client Wrapper Pattern

Don't call APIs directly everywhere. Create a wrapper class:

```python
import requests
from typing import Optional, Dict, Any

class WeatherAPIClient:
    """Wrapper around weather API."""

    def __init__(self, api_key: str):
        """Initialize client with API key."""
        self.api_key = api_key
        self.base_url = "https://api.openweathermap.org/data/2.5"

    def get_weather(self, city: str) -> Optional[Dict[str, Any]]:
        """Get current weather for a city."""
        try:
            response = requests.get(
                f"{self.base_url}/weather",
                params={
                    'q': city,
                    'appid': self.api_key
                },
                timeout=10
            )

            response.raise_for_status()  # Raise if error
            return response.json()

        except requests.RequestException as e:
            logger.error(f"Weather API error: {e}")
            return None
```

### 8.3 Real Example: ElevenLabs Transcriber

**From transcriber.py lines 225-334:**

```python
class ElevenLabsTranscriber:
    """Speech-to-text transcription using ElevenLabs."""

    def __init__(self, api_key: str):
        """Initialize with API key."""
        self.client = ElevenLabs(api_key=api_key)

    def transcribe(
        self,
        audio_path: Path,
        segment_id: int,
        language: str = 'en'
    ) -> Optional[Transcript]:
        """Transcribe audio file."""

        try:
            # Validate file exists
            if not audio_path.exists():
                logger.error(f"Audio file not found: {audio_path}")
                return None

            # Read binary audio
            with open(audio_path, 'rb') as f:
                audio_data = BytesIO(f.read())

            # Call API
            logger.info(f"Transcribing {audio_path}...")
            transcription = self.client.speech_to_text.convert(
                file=audio_data,
                language_code=language
            )

            # Parse response (remove markdown code blocks)
            text = transcription.text.replace('```', '')

            # Create Word objects with timestamps
            words = []
            if hasattr(transcription, 'words'):
                for word_data in transcription.words:
                    word = Word(
                        text=word_data['text'],
                        start=word_data['start'],
                        end=word_data['end']
                    )
                    words.append(word)

            # Create Transcript object
            transcript = Transcript(
                segment_id=segment_id,
                text=text,
                words=words
            )

            logger.info(f"Transcribed: {len(text)} chars, {len(words)} words")
            return transcript

        except Exception as e:
            logger.error(f"Transcription failed: {e}", exc_info=True)
            return None
```

### 8.4 Error Handling for APIs

APIs fail frequently. Always handle errors gracefully:

```python
import requests

def call_api_safely(url: str) -> Optional[dict]:
    """Call API with error handling."""

    try:
        response = requests.get(url, timeout=30)

        # Check HTTP status
        if response.status_code == 404:
            logger.error(f"API returned 404: {url}")
            return None

        if response.status_code == 429:
            logger.warning(f"Rate limited. Retry after: {response.headers.get('Retry-After')}")
            return None

        response.raise_for_status()  # Raise for 4xx/5xx

        # Parse JSON
        try:
            return response.json()
        except json.JSONDecodeError:
            logger.error(f"Invalid JSON response: {response.text}")
            return None

    except requests.Timeout:
        logger.error(f"Request timeout: {url}")
        return None

    except requests.ConnectionError:
        logger.error(f"Connection error: {url}")
        return None

    except Exception as e:
        logger.error(f"Unexpected error: {e}", exc_info=True)
        return None
```

### Claude Code Prompts for Chapter 8

1. **"Explain the ElevenLabsTranscriber class. Why is the audio read into BytesIO instead of passing the file path directly?"**

2. **"Show me all the error cases in transcribe(). What happens if the audio file is corrupted? If the API times out?"**

3. **"Create an OpenAI API client class that calls gpt-4. Include error handling for rate limiting (429) and timeouts."**

### Repository Practice Exercise

**Exercise 8.1: Test API Client Manually**

```python
from src.core.transcriber import ElevenLabsTranscriber
from pathlib import Path

# Get API key from environment
import os
api_key = os.getenv('ELEVENLABS_API_KEY')

if api_key:
    transcriber = ElevenLabsTranscriber(api_key)

    # Find an audio file
    audio_dir = Path("data/audio_extracted")
    audio_files = list(audio_dir.glob("*.wav"))

    if audio_files:
        audio_path = audio_files[0]
        print(f"Transcribing: {audio_path}")

        transcript = transcriber.transcribe(audio_path, segment_id=1)

        if transcript:
            print(f"Result: {transcript.text[:100]}...")
            print(f"Words: {len(transcript.words)}")
        else:
            print("Transcription failed")
else:
    print("ELEVENLABS_API_KEY not set")
```

**Exercise 8.2: Create a Retry Wrapper**

```python
import time
import logging

logger = logging.getLogger(__name__)

def call_with_retry(api_func, *args, max_retries=3, backoff_factor=2, **kwargs):
    """Call an API function with exponential backoff retry."""

    for attempt in range(max_retries):
        try:
            return api_func(*args, **kwargs)

        except Exception as e:
            logger.warning(f"Attempt {attempt + 1}/{max_retries} failed: {e}")

            if attempt == max_retries - 1:
                logger.error(f"All {max_retries} attempts failed")
                raise

            # Exponential backoff
            wait_time = backoff_factor ** attempt
            logger.info(f"Waiting {wait_time}s before retry...")
            time.sleep(wait_time)

# Test it
def failing_api(attempt_num):
    """Simulate flaky API."""
    if attempt_num < 3:
        raise Exception("API is down!")
    return "Success!"

attempt = 0
def mock_api():
    global attempt
    attempt += 1
    return failing_api(attempt)

result = call_with_retry(mock_api, max_retries=5)
print(result)  # Should succeed on retry 3
```

---

## Chapter 9: Timestamps and Time Operations

### What You'll Learn

Your system must track precise timestamps:
- When a segment was created
- When a word was spoken
- When processing started/ended

Get timestamps wrong and your videos will be out of sync.

### 9.1 Python datetime Module

```python
from datetime import datetime, timedelta

# Current time
now = datetime.now()
print(now)  # 2024-01-15 14:30:42.123456

# Create specific datetime
date = datetime(2024, 1, 15, 14, 30, 42)

# Datetime from string (parsing)
date_str = "2024-01-15T14:30:42.123456"
date = datetime.fromisoformat(date_str)  # ISO format

# Convert to ISO string (for JSON)
iso_str = date.isoformat()  # "2024-01-15T14:30:42.123456"

# Time differences
duration = timedelta(seconds=30)
end_time = now + duration

# Difference between times
start = datetime(2024, 1, 15, 10, 0, 0)
end = datetime(2024, 1, 15, 10, 0, 30)
elapsed = (end - start).total_seconds()  # 30.0
```

### 9.2 Float Timestamps (Seconds Since Start)

Your Word class uses float timestamps relative to segment start:

```python
# From segment.py lines 28-31
@dataclass
class Word:
    text: str
    start: float    # Seconds since segment start
    end: float      # Seconds since segment start
    confidence: float = 1.0

# Example:
# Word "hello" spoke from 1.5 to 2.0 seconds into the segment
word = Word("hello", start=1.5, end=2.0)

# Calculate duration
duration = word.end - word.start  # 0.5 seconds

# Convert to HH:MM:SS format
def format_timestamp(seconds: float) -> str:
    """Convert float seconds to HH:MM:SS."""
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = int(seconds % 60)
    millis = int((seconds % 1) * 1000)

    return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"

print(format_timestamp(65.5))  # "00:01:05,500"
```

**From segment.py lines 628-641:**

```python
def format_timestamp(seconds: float) -> str:
    """Format float seconds to SRT timestamp format."""
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = int(seconds % 60)
    millis = int((seconds % 1) * 1000)
    return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"

def parse_timestamp(ts_str: str) -> float:
    """Parse SRT timestamp to seconds."""
    # Format: "00:01:05,500"
    parts = ts_str.replace(',', '.').split(':')
    hours = int(parts[0])
    minutes = int(parts[1])
    seconds = float(parts[2])
    return hours * 3600 + minutes * 60 + seconds
```

### 9.3 Absolute vs Relative Time

This is critical for understanding your system:

```python
# RELATIVE time: Seconds since segment start
# Example: Word spoken at 1.5 seconds into a 30-second segment
word_relative_start = 1.5
word_relative_end = 2.0

# ABSOLUTE time: Seconds since stream start
# If segment starts at 120 seconds into stream:
segment_absolute_start = 120.0
word_absolute_start = segment_absolute_start + word_relative_start  # 121.5
word_absolute_end = segment_absolute_start + word_relative_end  # 122.0

# Example in code:
segment = Segment(
    id=1,
    start_time=120.0,  # Segment starts at 120s
    end_time=150.0
)

word = Word("hello", start=1.5, end=2.0)  # Relative

# Convert to absolute
absolute_start = segment.start_time + word.start  # 121.5
absolute_end = segment.start_time + word.end      # 122.0
```

### 9.4 Working with datetime in Dataclasses

**From segment.py lines 69-70:**

```python
@dataclass
class Segment:
    # ... other fields ...
    created_at: datetime = field(default_factory=datetime.now)
```

**Serialization to JSON:**

```python
# When saving to JSON (to_dict)
def to_dict(self) -> Dict[str, Any]:
    return {
        'created_at': self.created_at.isoformat(),  # Convert to string
        ...
    }

# When loading from JSON (from_dict)
@classmethod
def from_dict(cls, data: Dict[str, Any]) -> 'Segment':
    data = data.copy()
    data['created_at'] = datetime.fromisoformat(data['created_at'])  # Convert back
    return cls(**data)
```

### Claude Code Prompts for Chapter 9

1. **"Explain the difference between relative timestamps (Word.start) and absolute timestamps (Segment.start_time). Why does the system use relative?"**

2. **"Write a function to convert a datetime to ISO format string, and another to parse it back."**

3. **"Create a function that finds all words in a transcript spoken between 10-15 seconds absolute time. (Hint: Must convert relative to absolute)"**

### Repository Practice Exercise

**Exercise 9.1: Timestamp Conversion**

```python
from datetime import datetime
from pathlib import Path

def format_timestamp(seconds: float) -> str:
    """Format seconds to HH:MM:SS,mmm."""
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = int(seconds % 60)
    millis = int((seconds % 1) * 1000)
    return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"

# Test cases
print(format_timestamp(0))           # "00:00:00,000"
print(format_timestamp(1.5))         # "00:00:01,500"
print(format_timestamp(65))          # "00:01:05,000"
print(format_timestamp(3665.123))    # "01:01:05,123"
```

**Exercise 9.2: Load Timestamps from Transcripts**

```python
from pathlib import Path
import json
from src.models.segment import Transcript

# Load an actual transcript
transcript_path = Path("data/transcripts/transcript_00001.json")

if transcript_path.exists():
    with open(transcript_path, 'r') as f:
        data = json.load(f)

    transcript = Transcript.from_dict(data)

    # Print all words and their times
    print(f"Segment {transcript.segment_id}: {transcript.text}")
    print(f"Total words: {len(transcript.words)}\n")

    for i, word in enumerate(transcript.words[:10]):
        duration = word.end - word.start
        print(f"{i}: '{word.text:10s}' {word.start:.2f}s - {word.end:.2f}s ({duration:.3f}s)")
```

**Exercise 9.3: Find Words in Time Range**

```python
import json
from pathlib import Path
from src.models.segment import Transcript

def find_words_in_range(
    transcript: Transcript,
    segment_absolute_start: float,
    min_time: float,
    max_time: float
) -> list:
    """Find words spoken between min_time and max_time (absolute)."""

    results = []

    for word in transcript.words:
        # Convert relative to absolute time
        absolute_start = segment_absolute_start + word.start
        absolute_end = segment_absolute_start + word.end

        # Check if overlaps with range
        if absolute_end >= min_time and absolute_start <= max_time:
            results.append({
                'text': word.text,
                'relative_time': word.start,
                'absolute_time': absolute_start
            })

    return results

# Test it
transcript_path = Path("data/transcripts/transcript_00001.json")
with open(transcript_path, 'r') as f:
    transcript = Transcript.from_dict(json.load(f))

# Find words between 120-125 seconds
words = find_words_in_range(transcript, segment_absolute_start=100.0, min_time=120, max_time=125)

print(f"Words between 120-125s: {len(words)}")
for w in words:
    print(f"  {w['text']} at {w['absolute_time']:.2f}s")
```

---

---

## Chapter 10: Collections and Data Structures

### What You'll Learn

Python has powerful built-in collections optimized for different use cases. Use the right one and your code becomes simple. Use the wrong one and you'll have performance problems.

### 10.1 Lists: Ordered, Mutable Collections

```python
# Create lists
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True, None]
empty = []

# Access elements
first = numbers[0]          # 1
last = numbers[-1]          # 5
slice = numbers[1:4]        # [2, 3, 4]

# Modify lists
numbers.append(6)           # Add to end
numbers.extend([7, 8])      # Add multiple
numbers.insert(0, 0)        # Insert at index
numbers.remove(3)           # Remove by value
popped = numbers.pop()      # Remove and return last
numbers.pop(0)              # Remove at index

# Iteration
for num in numbers:
    print(num)

# List comprehensions
doubled = [x * 2 for x in numbers]
evens = [x for x in numbers if x % 2 == 0]

# Performance notes
# - Accessing by index: O(1) - very fast
# - Appending: O(1) amortized
# - Inserting/removing from middle: O(n) - slow
```

**From your code (segment.py):**

```python
# Line 121 - List of words
words: List[Word] = field(default_factory=list)

# Line 141 - Transform words to dicts
[w.to_dict() for w in self.words]

# Line 145 - Filter non-empty words
[w for w in self.words if w.text.strip()]
```

### 10.2 Dictionaries: Key-Value Mapping

```python
# Create dicts
config = {'host': 'localhost', 'port': 8000}
user = dict(name='Alice', age=30)
empty = {}

# Access
print(config['host'])                # 'localhost'
print(config.get('timeout', 10))     # 10 (default if not found)
print('port' in config)              # True

# Modify
config['timeout'] = 30
config.update({'ssl': True, 'debug': False})
del config['timeout']

# Iteration
for key in config:
    print(f"{key}: {config[key]}")

for key, value in config.items():
    print(f"{key}: {value}")

# Dict comprehensions
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Performance
# - Access by key: O(1) - very fast
# - Iteration: O(n)
# - Memory: More than lists, but worth it for fast lookup
```

**From your code:**

```python
# segment.py line 72-83 (to_dict method)
def to_dict(self) -> Dict[str, Any]:
    return {
        'id': self.id,
        'file_path': str(self.file_path),
        'status': self.status,
        ...
    }

# state_manager.py - dict of settings
config = {
    'db_path': 'data/state/segment_index.db',
    'cache_size': 100,
    'timeout': 30
}
```

### 10.3 Sets: Unique, Fast Lookup

```python
# Create sets
unique_ids = {1, 2, 3, 4, 5}
tags = {'python', 'web', 'fastapi'}
empty = set()  # Note: {} is dict, not set!

# Add/remove
tags.add('asyncio')
tags.discard('web')  # Doesn't error if missing
tags.remove('python')  # Errors if missing

# Operations
a = {1, 2, 3}
b = {3, 4, 5}

a & b  # Intersection: {3}
a | b  # Union: {1, 2, 3, 4, 5}
a - b  # Difference: {1, 2}

# Check membership
if 1 in a:  # O(1) - very fast!
    print("Found")

# Performance
# - Add/remove/lookup: O(1) average
# - Perfect for "have we processed this segment?"
```

**From your code (stream_processor.py):**

```python
# Line 42 - Track processed segments
self.processed_segments = set()

# Usage (O(1) lookup)
if segment_id not in self.processed_segments:
    process_segment(segment_id)
    self.processed_segments.add(segment_id)
```

### 10.4 Deques: Rolling Windows and Queues

```python
from collections import deque

# Fixed-size rolling buffer
buffer = deque(maxlen=3)

buffer.append(1)
buffer.append(2)
buffer.append(3)
print(buffer)  # deque([1, 2, 3])

buffer.append(4)  # Oldest value (1) automatically removed
print(buffer)  # deque([2, 3, 4])

# Get all items
list(buffer)  # [2, 3, 4]

# Performance
# - Append/pop from either end: O(1)
# - Perfect for "last N items" tracking
# - Much faster than list.pop(0)
```

**Common pattern in your system:**

```python
# Maintain last 10 minutes of transcripts
transcript_buffer = deque(maxlen=20)  # 20 segments × 30s = 10 minutes

for transcript in incoming_transcripts:
    transcript_buffer.append(transcript)
    # Buffer never exceeds 20, oldest auto-removed

# Use buffer for Layer 1 analysis
if len(transcript_buffer) >= 4:  # At least 2 minutes
    analyze(list(transcript_buffer))
```

### 10.5 defaultdict: Auto-Initializing Dicts

```python
from collections import defaultdict

# Regular dict - KeyError if missing
data = {}
data['count'] += 1  # ERROR! 'count' doesn't exist

# defaultdict - auto-initialize
data = defaultdict(int)
data['count'] += 1  # Works! Initializes to 0

data = defaultdict(list)
data['items'].append('thing1')  # Works! Initializes to []

data = defaultdict(set)
data['tags'].add('python')  # Works! Initializes to set()

# Usage
words_per_segment = defaultdict(int)
for transcript in transcripts:
    for word in transcript.words:
        words_per_segment[transcript.segment_id] += 1
```

### 10.6 When to Use Each

| Collection | Use Case | Access Time | Mutability |
|-----------|----------|-------------|-----------|
| **List** | Ordered items, may change | O(1) by index, O(n) search | Yes |
| **Dict** | Key-value lookups | O(1) by key | Yes |
| **Set** | Unique items, fast membership | O(1) membership | Yes |
| **Deque** | Rolling buffer, queue | O(1) at ends | Yes |
| **Tuple** | Immutable, hashable | O(1) by index | No |

### Claude Code Prompts for Chapter 10

1. **"Explain why stream_processor.py uses a set for `processed_segments` instead of a list. What's the performance difference?"**

2. **"Create a defaultdict(list) that groups words by their first letter. Process a transcript to populate it."**

3. **"Write a function that maintains a rolling window of the last 100 segments using deque. How is it better than a list?"**

### Repository Practice Exercise

**Exercise 10.1: Analyze Collection Performance**

```python
import time
from collections import deque

# Compare list vs deque for rolling buffer
def test_list_rolling_buffer():
    buffer = []
    start = time.time()
    for i in range(100000):
        buffer.append(i)
        if len(buffer) > 100:
            buffer.pop(0)  # O(n) - slow!
    return time.time() - start

def test_deque_rolling_buffer():
    buffer = deque(maxlen=100)
    start = time.time()
    for i in range(100000):
        buffer.append(i)  # O(1) - fast!
    return time.time() - start

print(f"List: {test_list_rolling_buffer():.4f}s")
print(f"Deque: {test_deque_rolling_buffer():.4f}s")
# Deque will be much faster!
```

**Exercise 10.2: Group Words by Confidence**

```python
from collections import defaultdict
from pathlib import Path
import json
from src.models.segment import Transcript

# Load a transcript
transcript_path = Path("data/transcripts/transcript_00001.json")

if transcript_path.exists():
    with open(transcript_path, 'r') as f:
        data = json.load(f)

    transcript = Transcript.from_dict(data)

    # Group words by confidence level
    by_confidence = defaultdict(list)

    for word in transcript.words:
        # Round confidence to 1 decimal
        level = round(word.confidence, 1)
        by_confidence[level].append(word.text)

    # Print results
    for level in sorted(by_confidence.keys(), reverse=True):
        words = by_confidence[level]
        print(f"Confidence {level}: {len(words)} words")
        print(f"  Examples: {words[:5]}")
```

**Exercise 10.3: Track Processing Status**

```python
from collections import defaultdict

class SegmentTracker:
    """Track segments by processing status."""

    def __init__(self):
        # Use defaultdict(set) to group by status
        self.by_status = defaultdict(set)
        self.all_segments = set()

    def add_segment(self, segment_id: int, status: str):
        """Add segment with status."""
        self.all_segments.add(segment_id)
        self.by_status[status].add(segment_id)

    def update_status(self, segment_id: int, old_status: str, new_status: str):
        """Update segment status."""
        self.by_status[old_status].discard(segment_id)
        self.by_status[new_status].add(segment_id)

    def get_by_status(self, status: str) -> set:
        """Get all segments with given status."""
        return self.by_status[status]

    def stats(self):
        """Print statistics."""
        print(f"Total segments: {len(self.all_segments)}")
        for status in sorted(self.by_status.keys()):
            count = len(self.by_status[status])
            print(f"  {status}: {count}")

# Test it
tracker = SegmentTracker()
for i in range(10):
    tracker.add_segment(i, 'pending')

tracker.update_status(1, 'pending', 'processing')
tracker.update_status(2, 'pending', 'processing')
tracker.update_status(1, 'processing', 'complete')

tracker.stats()
```

---

## Chapter 11: LLM API Integration (Gemini)

### What You'll Learn

Your system uses Google Gemini to analyze transcripts and detect topic boundaries. You'll learn how to wrap external APIs safely, handle JSON responses, and implement resilience patterns.

### 11.1 API Client Setup

**From gemini_llm_service.py lines 27-92:**

```python
import google.generativeai as genai

class GeminiLLMService:
    """Google Gemini LLM service for structured analysis."""

    def __init__(
        self,
        api_key: str,
        model_name: str = "gemini-3.0-flash",
        temperature: float = 0.3,
        max_retries: int = 3,
        retry_delay: float = 2.0
    ):
        """Initialize Gemini service."""
        self.api_key = api_key
        self.model_name = model_name
        self.temperature = temperature
        self.max_retries = max_retries
        self.retry_delay = retry_delay

        # Configure Gemini API
        genai.configure(api_key=api_key)

        # Generation configuration
        self.generation_config = {
            "temperature": temperature
        }

        # Safety settings (production-safe)
        self.safety_settings = [
            {
                "category": "HARM_CATEGORY_HARASSMENT",
                "threshold": "BLOCK_NONE"
            },
            {
                "category": "HARM_CATEGORY_HATE_SPEECH",
                "threshold": "BLOCK_NONE"
            },
            {
                "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
                "threshold": "BLOCK_NONE"
            },
            {
                "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
                "threshold": "BLOCK_NONE"
            }
        ]

        # Initialize model
        self.model = genai.GenerativeModel(
            model_name=model_name,
            generation_config=self.generation_config,
            safety_settings=self.safety_settings
        )

        logger.info(
            f"GeminiLLMService initialized: "
            f"model={model_name}, temperature={temperature}"
        )
```

**Key Concepts:**

- **API Key**: Authentication for Google Gemini API
- **Model Name**: Which model to use (gemini-3.0-flash, gemini-pro, etc.)
- **Temperature**: 0-1 scale controlling randomness
  - 0.0 = Deterministic (always same answer)
  - 0.3 = Default (balance of creativity and consistency)
  - 1.0 = Very creative/random
- **Safety Settings**: Prevent certain content types
- **Generation Config**: Parameters for this request

### 11.2 Making API Calls with Retry Logic

**From gemini_llm_service.py lines 93-172:**

```python
def generate(
    self,
    prompt: str,
    temperature: Optional[float] = None,
    max_tokens: Optional[int] = 4096
) -> str:
    """Generate response from Gemini with retry logic."""

    temp = temperature if temperature is not None else self.temperature

    for attempt in range(self.max_retries):
        try:
            # Update config for this request
            config = {
                "temperature": temp,
                "max_output_tokens": max_tokens,
            }

            # Create model with updated config
            model = genai.GenerativeModel(
                model_name=self.model_name,
                generation_config=config,
                safety_settings=self.safety_settings
            )

            # Prepare prompt with system instructions
            full_prompt = f"""You are a precise content analysis assistant.
Respond with valid JSON only.

{prompt}"""

            # Make API call
            logger.debug(f"Calling Gemini (attempt {attempt + 1}/{self.max_retries})")
            start_time = time.time()

            response = model.generate_content(full_prompt)

            elapsed = time.time() - start_time
            logger.debug(f"Gemini response received in {elapsed:.2f}s")

            # Extract response text
            response_text = response.text

            # Remove markdown code blocks if present
            response_text = response_text.replace('```json', '').replace('```', '')

            return response_text

        except Exception as e:
            logger.warning(f"Attempt {attempt + 1}/{self.max_retries} failed: {e}")

            if attempt == self.max_retries - 1:
                logger.error(f"All {self.max_retries} attempts failed")
                raise

            # Exponential backoff
            wait_time = self.retry_delay * (2 ** attempt)
            logger.info(f"Retrying in {wait_time}s...")
            time.sleep(wait_time)

    return ""
```

**Retry Strategy:**

```
Attempt 1: Fail → Wait 2s
Attempt 2: Fail → Wait 4s
Attempt 3: Fail → Wait 8s
Attempt 4: Give up
```

### 11.3 Parsing JSON Responses

```python
import json

def parse_json_response(response_text: str) -> dict:
    """Safely parse JSON from LLM response."""

    try:
        # Clean up response (remove markdown)
        text = response_text.replace('```json', '').replace('```', '').strip()

        # Parse JSON
        return json.loads(text)

    except json.JSONDecodeError as e:
        logger.error(f"Invalid JSON response: {response_text[:100]}...")
        return {}

    except Exception as e:
        logger.error(f"Error parsing response: {e}")
        return {}

# Usage
response_text = """```json
{
    "topic_complete": true,
    "confidence": 0.85,
    "title": "Election Results"
}
```"""

data = parse_json_response(response_text)
print(data['topic_complete'])  # True
```

### 11.4 Real-World Example: Topic Detection Prompt

**From layer1_prompts.py:**

```python
def build_layer1_stage1_prompt(transcripts: List[Transcript]) -> str:
    """Build prompt for Stage 1: Topic boundary detection."""

    # Combine transcripts into narrative
    combined_text = " ".join([t.text for t in transcripts])

    prompt = f"""Analyze this transcript segment for topic boundaries.

TRANSCRIPT:
{combined_text}

TASK: Determine if a topic boundary exists (topic has concluded).

RESPONSE FORMAT: JSON with these fields:
{{
    "boundary_detected": boolean,  // Has topic concluded?
    "confidence": float,           // 0.0 to 1.0
    "summary": string,             // What was discussed?
    "next_topic_hint": string      // What comes next?
}}

Respond ONLY with JSON, no other text."""

    return prompt

# Usage
gemini = GeminiLLMService(api_key="...")
prompt = build_layer1_stage1_prompt([transcript1, transcript2])
response = gemini.generate(prompt, temperature=0.3)
data = json.loads(response)
```

### 11.5 Temperature and Determinism

```python
# Low temperature: Deterministic (same input = same output)
response1 = gemini.generate(prompt, temperature=0.1)
response2 = gemini.generate(prompt, temperature=0.1)
# Usually identical!

# High temperature: Creative (more variation)
response1 = gemini.generate(prompt, temperature=0.9)
response2 = gemini.generate(prompt, temperature=0.9)
# Likely different!

# Your system uses low temperature (0.3) for consistency
```

### Claude Code Prompts for Chapter 11

1. **"Explain the retry pattern in GeminiLLMService.generate(). Why exponential backoff instead of immediate retry?"**

2. **"Show me the complete flow: prompt → API call → retry logic → response parsing → JSON extraction."**

3. **"Create a LLM service for OpenAI's GPT-4 following the same pattern as GeminiLLMService."**

### Repository Practice Exercise

**Exercise 11.1: Test Gemini API**

```python
import os
from src.services.gemini_llm_service import GeminiLLMService

api_key = os.getenv('GEMINI_API_KEY')

if api_key:
    gemini = GeminiLLMService(api_key=api_key)

    # Simple prompt
    prompt = """Respond with JSON only:
{
    "language": string,
    "word_count": integer
}

Analyze: 'The quick brown fox jumps over the lazy dog'"""

    response = gemini.generate(prompt, temperature=0.1)
    print(f"Response: {response}")

    # Parse it
    import json
    data = json.loads(response)
    print(f"Language: {data['language']}")
    print(f"Words: {data['word_count']}")
else:
    print("GEMINI_API_KEY not set")
```

**Exercise 11.2: Implement Caching**

```python
import json
import hashlib
from pathlib import Path

class CachedGeminiService:
    """Gemini service with response caching."""

    def __init__(self, gemini_service, cache_dir: Path = Path("data/gemini_cache")):
        self.gemini = gemini_service
        self.cache_dir = cache_dir
        self.cache_dir.mkdir(exist_ok=True)

    def _get_cache_key(self, prompt: str) -> str:
        """Create cache filename from prompt."""
        hash_val = hashlib.md5(prompt.encode()).hexdigest()
        return hash_val

    def generate(self, prompt: str, **kwargs) -> str:
        """Generate with caching."""
        cache_key = self._get_cache_key(prompt)
        cache_file = self.cache_dir / f"{cache_key}.json"

        # Check cache
        if cache_file.exists():
            with open(cache_file, 'r') as f:
                cached = json.load(f)
            print(f"Cache hit: {cache_key}")
            return cached['response']

        # Call API
        response = self.gemini.generate(prompt, **kwargs)

        # Save to cache
        with open(cache_file, 'w') as f:
            json.dump({'response': response, 'prompt': prompt[:100]}, f)

        print(f"Cache miss: {cache_key}")
        return response

# Usage
from src.services.gemini_llm_service import GeminiLLMService
gemini = GeminiLLMService(api_key=os.getenv('GEMINI_API_KEY'))
cached = CachedGeminiService(gemini)

# First call: hits API
response1 = cached.generate("What is 2+2?")
# Second call: uses cache
response2 = cached.generate("What is 2+2?")
```

---

## Chapter 12: Threading and Concurrency

### What You'll Learn

Your system runs multiple operations simultaneously:
- Ingestion: Reading from YouTube stream
- Transcription: Converting audio to text
- Analysis: Running LLM on transcripts
- Output: Writing results to disk

All at the same time, in different threads.

### 12.1 Threading Basics

```python
import threading
import time

def worker(name, duration):
    """A function that runs in a thread."""
    print(f"{name} starting")
    time.sleep(duration)
    print(f"{name} done")

# Create threads
t1 = threading.Thread(target=worker, args=("Task1", 2))
t2 = threading.Thread(target=worker, args=("Task2", 1))

# Start threads
t1.start()
t2.start()

# Wait for completion
t1.join()  # Wait for Task1 to finish
t2.join()  # Wait for Task2 to finish

print("All done")
```

**Output:**
```
Task1 starting
Task2 starting
Task2 done         (after 1 second)
Task1 done         (after 2 seconds)
All done
```

### 12.2 Daemon Threads (Background Workers)

**From main.py lines 671-678:**

```python
# Create a daemon thread (background worker)
self.retry_thread = threading.Thread(
    target=self._retry_failed_segments,
    daemon=True,  # Daemon = runs in background, doesn't block exit
    name="SegmentRetryThread"
)

self.retry_thread.start()

# With daemon=True:
# - Thread runs in background
# - Program exits even if thread still running
# - Perfect for background tasks
```

**Daemon vs Normal:**

```python
import threading
import time

def background_task():
    for i in range(100):
        print(f"Background: {i}")
        time.sleep(0.1)

# Daemon thread
t = threading.Thread(target=background_task, daemon=True)
t.start()

# Program exits immediately, thread stops
print("Main done")
```

### 12.3 Thread Safety with Locks

Multiple threads accessing same data = race conditions:

```python
import threading

# UNSAFE - data corruption!
counter = 0

def increment():
    global counter
    for _ in range(100000):
        counter += 1  # NOT atomic! Can lose updates

threads = [threading.Thread(target=increment) for _ in range(5)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(f"Counter: {counter}")  # Expected: 500000, Got: ~250000 (varies!)
```

**Safe with Lock:**

```python
import threading

counter = 0
lock = threading.Lock()

def increment_safe():
    global counter
    for _ in range(100000):
        with lock:  # Only one thread at a time
            counter += 1

threads = [threading.Thread(target=increment_safe) for _ in range(5)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(f"Counter: {counter}")  # Always 500000!
```

### 12.4 Shared State in Your System

```python
class StreamProcessor:
    """Example of thread-safe state management."""

    def __init__(self):
        self.processed_segments = set()
        self.lock = threading.Lock()

    def mark_processed(self, segment_id: int):
        """Thread-safe operation."""
        with self.lock:  # Get lock
            self.processed_segments.add(segment_id)
        # Lock released here

    def is_processed(self, segment_id: int) -> bool:
        """Thread-safe read."""
        with self.lock:
            return segment_id in self.processed_segments

    def get_count(self) -> int:
        """Thread-safe read."""
        with self.lock:
            return len(self.processed_segments)

# Usage from multiple threads
processor = StreamProcessor()

def worker(segment_id):
    processor.mark_processed(segment_id)
    if processor.is_processed(segment_id):
        print(f"Segment {segment_id} processed")
```

### 12.5 Thread Safety in Your Code

**From main.py:**

```python
# State that needs protection
self.running = False
self.processing_lock = threading.Lock()

# Protected update
def stop(self):
    with self.processing_lock:
        self.running = False

# Protected read
def is_running(self):
    with self.processing_lock:
        return self.running
```

### Claude Code Prompts for Chapter 12

1. **"Explain why daemon=True is used for the retry thread in main.py. What happens if it's False?"**

2. **"Show me how to safely update `self.running` from different threads without using a lock. Why is that a bad idea?"**

3. **"Create a thread-safe queue for processing segments. Multiple producers, one consumer."**

### Repository Practice Exercise

**Exercise 12.1: Thread Safety Demo**

```python
import threading
import time

# Unsafe version
unsafe_counter = 0

def unsafe_increment():
    global unsafe_counter
    for _ in range(10000):
        unsafe_counter += 1

# Safe version
safe_counter = 0
lock = threading.Lock()

def safe_increment():
    global safe_counter
    for _ in range(10000):
        with lock:
            safe_counter += 1

# Test unsafe
unsafe_counter = 0
threads = [threading.Thread(target=unsafe_increment) for _ in range(5)]
for t in threads:
    t.start()
for t in threads:
    t.join()
print(f"Unsafe: {unsafe_counter} (should be 50000)")

# Test safe
safe_counter = 0
threads = [threading.Thread(target=safe_increment) for _ in range(5)]
for t in threads:
    t.start()
for t in threads:
    t.join()
print(f"Safe: {safe_counter} (should be 50000)")
```

**Exercise 12.2: Implement a Segment Queue**

```python
import threading
from queue import Queue
from pathlib import Path

class SegmentQueue:
    """Thread-safe queue for segments."""

    def __init__(self, maxsize=100):
        self.queue = Queue(maxsize=maxsize)

    def add_segment(self, segment_id: int, path: Path):
        """Add segment to queue."""
        self.queue.put((segment_id, path))

    def get_next_segment(self):
        """Get next segment (blocks if empty)."""
        return self.queue.get(timeout=5)

    def size(self):
        """Current queue size."""
        return self.queue.qsize()

# Test
queue = SegmentQueue()

def producer():
    for i in range(10):
        queue.add_segment(i, Path(f"segment_{i}.ts"))
        print(f"Added segment {i}")

def consumer():
    while True:
        try:
            seg_id, path = queue.get_next_segment()
            print(f"Processing {seg_id}: {path}")
        except:
            break

# Run
t1 = threading.Thread(target=producer)
t2 = threading.Thread(target=consumer)
t1.start()
t2.start()
t1.join()
t2.join()
```

---

## Chapter 13: Async/Await and WebSockets

### What You'll Learn

Async programming lets you handle multiple tasks without threads. WebSockets enable real-time bidirectional communication with browsers.

### 13.1 Async/Await Basics

```python
import asyncio

# Async function (must use await to call other async functions)
async def fetch_data(url):
    """Simulate fetching from URL."""
    print(f"Fetching {url}...")
    await asyncio.sleep(1)  # Simulate network delay
    print(f"Got response from {url}")
    return "data"

# Regular code can't call async directly
# await can only be used in async functions
async def main():
    # Call async function with await
    result = await fetch_data("http://api.com/data")
    print(f"Result: {result}")

# Run async code
asyncio.run(main())
```

**VS Threading:**

```python
# Threading: Run in parallel (different processors if available)
# Async: Run concurrently (single processor, switch on I/O)

# Threading: Better for CPU-bound work
# Async: Better for I/O-bound work (network, files, databases)

# Your system:
# - Threading: Segment ingestion (reads from internet)
# - Async: WebSocket server (many clients)
```

### 13.2 Async Context from Sync

Your system bridges sync and async code:

**From stream_processor.py lines 173-180:**

```python
import asyncio

# Sync code
def process_segment(segment):
    # Need to call async callback
    # Can't use await here (not async function)

    loop = asyncio.new_event_loop()
    asyncio.set_event_loop(loop)
    try:
        # Run async code from sync context
        loop.run_until_complete(
            self.on_new_transcript(transcript)
        )
    finally:
        loop.close()
```

### 13.3 WebSockets and Real-Time Updates

**From server.py (FastAPI application):**

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
import json

app = FastAPI()

class ConnectionManager:
    def __init__(self):
        self.active_connections = []

    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)
        print(f"Client connected. Total: {len(self.active_connections)}")

    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)
        print(f"Client disconnected. Total: {len(self.active_connections)}")

    async def broadcast(self, message: dict):
        """Send to all connected clients."""
        for connection in self.active_connections:
            try:
                await connection.send_json(message)
            except Exception as e:
                print(f"Error sending to client: {e}")

manager = ConnectionManager()

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await manager.connect(websocket)
    try:
        while True:
            # Receive from client
            data = await websocket.receive_text()
            print(f"Received: {data}")

            # Send back to all clients
            await manager.broadcast({"msg": data})

    except WebSocketDisconnect:
        manager.disconnect(websocket)
        print("Client disconnected")
```

**Client Side (JavaScript):**

```javascript
// Connect to WebSocket
const ws = new WebSocket("ws://localhost:8000/ws");

// Receive messages from server
ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log("Got:", data);
};

// Send to server
ws.send(JSON.stringify({text: "hello"}));
```

### 13.4 Broadcast Pattern (Real-Time Updates)

Your system uses this to send live transcription updates to the web UI:

```python
# In segmenter (when new transcript arrives)
async def on_new_transcript(transcript: Transcript):
    # Broadcast to all WebSocket clients
    await websocket_manager.broadcast({
        "type": "transcript",
        "segment_id": transcript.segment_id,
        "text": transcript.text,
        "words": [w.to_dict() for w in transcript.words]
    })

# Browser receives in real-time
# User sees transcription appear as it happens!
```

### Claude Code Prompts for Chapter 13

1. **"Explain the difference between asyncio and threading. When would you use each?"**

2. **"Show me how the WebSocket broadcast pattern works. How do multiple clients all receive updates?"**

3. **"Create an async function that fetches 10 URLs in parallel (not sequentially). Use asyncio.gather."**

### Repository Practice Exercise

**Exercise 13.1: Simple Async**

```python
import asyncio

async def task(name, duration):
    """An async task."""
    print(f"{name} starting")
    await asyncio.sleep(duration)
    print(f"{name} done")

async def main():
    """Run multiple tasks concurrently."""
    # Run all at the same time!
    await asyncio.gather(
        task("Task1", 2),
        task("Task2", 1),
        task("Task3", 3)
    )

asyncio.run(main())
# Output:
# Task1 starting
# Task2 starting
# Task3 starting
# Task2 done (after 1s)
# Task1 done (after 2s)
# Task3 done (after 3s)
```

**Exercise 13.2: Simulate API Calls**

```python
import asyncio
import time
import random

async def fetch_transcript(segment_id: int):
    """Simulate fetching transcript from API."""
    # Random delay (0.5-2 seconds)
    delay = random.uniform(0.5, 2.0)
    print(f"Fetching segment {segment_id} (will take {delay:.1f}s)...")

    await asyncio.sleep(delay)

    print(f"Got segment {segment_id}")
    return {
        "segment_id": segment_id,
        "text": f"This is segment {segment_id}"
    }

async def main():
    """Fetch multiple transcripts in parallel."""
    start = time.time()

    # Fetch 5 segments in parallel
    results = await asyncio.gather(
        fetch_transcript(1),
        fetch_transcript(2),
        fetch_transcript(3),
        fetch_transcript(4),
        fetch_transcript(5)
    )

    elapsed = time.time() - start

    for result in results:
        print(f"  Segment {result['segment_id']}: {result['text']}")

    # Total time = max individual time, not sum!
    print(f"Total time: {elapsed:.1f}s (would be ~10s if sequential)")

asyncio.run(main())
```

---

## Chapter 14: Subprocess Management

### What You'll Learn

Your system runs external programs:
- **yt-dlp**: Extract YouTube stream URL
- **FFmpeg**: Segment video, extract audio, merge clips

Manage them safely and you'll have a rock-solid system.

### 14.1 Running External Programs

```python
import subprocess

# Simple command
result = subprocess.run(
    ["echo", "Hello world"],
    capture_output=True,
    text=True
)

print(result.stdout)      # "Hello world\n"
print(result.stderr)      # ""
print(result.returncode)  # 0 (success)

# With error checking
try:
    subprocess.run(
        ["missing_command"],
        check=True  # Raise CalledProcessError if error
    )
except subprocess.CalledProcessError as e:
    print(f"Command failed with code {e.returncode}")
```

### 14.2 Long-Running Processes

```python
# Popen for long-running processes
process = subprocess.Popen(
    ["ffmpeg", "-i", "input.mp4", "-c", "copy", "output.mp4"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE
)

# Process runs in background
# You can check on it later

# Wait for completion
stdout, stderr = process.communicate()

# Or with timeout
try:
    stdout, stderr = process.communicate(timeout=30)
except subprocess.TimeoutExpired:
    process.kill()  # Force stop
    print("Process killed!")
```

### 14.3 Piping: Connect Processes

**From stream_ingestion.py:**

```python
# Get YouTube stream URL
yt_dlp_cmd = ["yt-dlp", "-f", "best", "-g", youtube_url]
yt_process = subprocess.Popen(
    yt_dlp_cmd,
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    text=True
)

# Get output from yt-dlp
stream_url, _ = yt_process.communicate()
stream_url = stream_url.strip()

# Then use that URL in FFmpeg
ffmpeg_cmd = [
    "ffmpeg",
    "-i", stream_url,
    "-f", "segment",
    "-segment_time", "30",
    "-c", "copy",
    "segment_%05d.ts"
]

ffmpeg_process = subprocess.Popen(ffmpeg_cmd)
```

### 14.4 Real Example: Audio Extraction

**From transcriber.py lines 160-177:**

```python
def extract_audio(video_path: Path, output_path: Path) -> bool:
    """Extract audio from video using FFmpeg."""

    # Build command
    cmd = [
        'ffmpeg',
        '-i', str(video_path),      # Input video
        '-q:a', '9',                 # Audio quality
        '-n',                        # No overwrite
        str(output_path)             # Output audio
    ]

    try:
        # Run FFmpeg
        result = subprocess.run(
            cmd,
            capture_output=True,
            check=True,              # Raise if error
            timeout=60               # 60 second timeout
        )

        logger.info(f"Audio extracted: {output_path}")
        return True

    except subprocess.TimeoutExpired:
        logger.error(f"FFmpeg timeout on {video_path}")
        return False

    except subprocess.CalledProcessError as e:
        logger.error(f"FFmpeg error: {e.stderr.decode()}")
        return False

    except Exception as e:
        logger.error(f"Unexpected error: {e}")
        return False
```

### 14.5 Environment Variables for Subprocesses

```python
import os
import subprocess

# Create environment with custom vars
env = os.environ.copy()
env['MY_VAR'] = 'custom_value'

# Pass to subprocess
result = subprocess.run(
    ["python", "script.py"],
    env=env
)
```

### Claude Code Prompts for Chapter 14

1. **"Explain the extract_audio function in transcriber.py. What happens on timeout? How does it handle errors?"**

2. **"Show me how to pipe yt-dlp output into FFmpeg. Why not just save to a file first?"**

3. **"Create a Python script that runs FFmpeg to create a thumbnail from a video."**

### Repository Practice Exercise

**Exercise 14.1: Test FFmpeg Commands**

```python
import subprocess
from pathlib import Path

# Find an audio file
audio_dir = Path("data/audio_extracted")
audio_files = list(audio_dir.glob("*.wav"))

if audio_files:
    audio_path = audio_files[0]

    # Get audio info with ffprobe
    cmd = [
        "ffprobe",
        "-v", "error",
        "-show_format",
        "-show_streams",
        str(audio_path)
    ]

    result = subprocess.run(cmd, capture_output=True, text=True)
    print(result.stdout)
else:
    print("No audio files found")
```

**Exercise 14.2: Implement Safe Subprocess Wrapper**

```python
import subprocess
import logging

logger = logging.getLogger(__name__)

def run_command(cmd, timeout=30, max_retries=3):
    """Run command with retry and timeout."""

    for attempt in range(max_retries):
        try:
            logger.info(f"Running: {' '.join(cmd)}")

            result = subprocess.run(
                cmd,
                capture_output=True,
                check=True,
                timeout=timeout
            )

            logger.info("Command succeeded")
            return True

        except subprocess.TimeoutExpired:
            logger.warning(f"Attempt {attempt + 1}: Timeout")
            if attempt == max_retries - 1:
                raise

        except subprocess.CalledProcessError as e:
            logger.warning(f"Attempt {attempt + 1}: Exit code {e.returncode}")
            if attempt == max_retries - 1:
                raise

    return False

# Test it
# run_command(["echo", "hello"], timeout=5, max_retries=3)
```

---

## Chapter 15: Prompt Engineering

### What You'll Learn

The LLM prompts in your system determine quality. Small changes in wording can dramatically affect results.

### 15.1 F-Strings for Template Prompts

```python
# Simple formatting
segment_id = 1
text = "The election results are in"

prompt = f"Analyze this segment: {text}"

# Multi-line with variables
prompt = f"""Analyze this transcript:

SEGMENT ID: {segment_id}
TEXT: {text}

Return JSON with: topic, confidence, summary"""

# Expressions
numbers = [1, 2, 3]
prompt = f"Sum of {numbers} is {sum(numbers)}"  # Expressions work!
```

### 15.2 Prompt Structure

**Good prompts have:**

1. **Context**: What the AI is
2. **Task**: What to do
3. **Format**: How to respond
4. **Examples**: Few-shot learning

**From layer1_prompts.py:**

```python
prompt = f"""You are a TV news transcript analyzer.

TASK: Detect if a topic has concluded based on the transcript.

TRANSCRIPT:
{transcript_text}

ANALYSIS GUIDELINES:
- Topic concludes when:
  * Anchor moves to different story
  * Breaking news alert
  * Segment ends

RESPONSE FORMAT: Return JSON only:
{{
    "boundary_detected": boolean,
    "confidence": 0.0-1.0,
    "summary": "What was discussed?",
    "indicator": "Why you detected boundary?"
}}

Respond with JSON only."""
```

### 15.3 Few-Shot Learning

Give examples to improve quality:

```python
prompt = f"""Classify sentiment: positive, negative, neutral.

EXAMPLES:
"I love this!" → positive
"This is terrible" → negative
"It's raining" → neutral

Now classify: {user_text}

Response format: {{sentiment: string}}"""
```

### 15.4 Token Counting

```python
# Estimate tokens (roughly 4 chars per token)
def estimate_tokens(text: str) -> int:
    return len(text) // 4

prompt = "Long prompt here..."
tokens = estimate_tokens(prompt)

if tokens > 10000:
    # Truncate or summarize
    prompt = prompt[:5000] + "... (truncated)"
```

### 15.5 Temperature for Determinism

```python
# For topic detection: low temperature (consistent results)
response1 = gemini.generate(prompt, temperature=0.1)
response2 = gemini.generate(prompt, temperature=0.1)
# Usually same

# For creative tasks: high temperature
response1 = gemini.generate(prompt, temperature=0.9)
response2 = gemini.generate(prompt, temperature=0.9)
# Usually different
```

### Claude Code Prompts for Chapter 15

1. **"Analyze the subtopic detection prompt in layer1_prompts.py. Why include 'ANALYSIS GUIDELINES'?"**

2. **"Improve this prompt to detect topic boundaries in earnings calls. Include examples specific to that domain."**

3. **"Create a prompt that extracts key decisions from a transcript. Structure it with context, task, format, and examples."**

### Repository Practice Exercise

**Exercise 15.1: Test Different Prompts**

```python
import os
from src.services.gemini_llm_service import GeminiLLMService
import json

api_key = os.getenv('GEMINI_API_KEY')
gemini = GeminiLLMService(api_key=api_key)

sample_transcript = "The election results show clear victory for candidate A with 65% of votes."

# Prompt 1: Simple
prompt1 = f"""Analyze: {sample_transcript}
Return JSON with: topic, confidence"""

# Prompt 2: Structured
prompt2 = f"""Analyze this election news segment:

TEXT: {sample_transcript}

Extract:
- Topic discussed
- Confidence (0-1)
- Key numbers/facts

JSON FORMAT:
{{
    "topic": string,
    "confidence": float,
    "key_facts": list[string]
}}"""

# Test both
response1 = gemini.generate(prompt1, temperature=0.3)
response2 = gemini.generate(prompt2, temperature=0.3)

print("Prompt 1 (simple):")
print(response1)
print("\nPrompt 2 (structured):")
print(response2)
```

**Exercise 15.2: Few-Shot Prompt**

```python
import os
from src.services.gemini_llm_service import GeminiLLMService

api_key = os.getenv('GEMINI_API_KEY')
gemini = GeminiLLMService(api_key=api_key)

# Build few-shot prompt
prompt = """Detect news topic boundary.

EXAMPLES:
"We'll now turn to sports with the latest cricket match results" → BOUNDARY
"Continuing our analysis of the market" → NO BOUNDARY
"After the break, we'll have weather" → BOUNDARY

New text: "That concludes our political analysis. Now for business news."

Response JSON:
{
    "boundary_detected": boolean,
    "confidence": 0-1,
    "reasoning": string
}"""

response = gemini.generate(prompt, temperature=0.2)
print(response)
```

---

## Chapter 16: AI Pipeline Architecture

### What You'll Learn

Your system uses a **3-layer progressive architecture**:
- **Layer 0**: Real-time sentences (60s latency)
- **Layer 1**: Sub-topics (2min latency)
- **Layer 2**: Parent topics (8min latency)

Each layer triggers at different times, enabling fast output + accuracy.

### 16.1 The Three Layers

```
Incoming Transcript Stream
         ↓
    ┌────────────────────┐
    │ Layer 0 (30-60s)   │
    │ Extract sentences  │
    │ Real-time output   │
    │ sentences.json     │
    └────────────────────┘
         ↓
    ┌────────────────────┐
    │ Layer 1 (120s)     │
    │ Detect subtopics   │
    │ Send to LLM        │
    │ subtopics.json     │
    └────────────────────┘
         ↓
    ┌────────────────────┐
    │ Layer 2 (480s)     │
    │ Group into topics  │
    │ Create videos      │
    │ topics.json +      │
    │ video.mp4          │
    └────────────────────┘
```

### 16.2 Why Three Layers?

```
Layer 0 (Sentences):
- Pro: Real-time (users see text appearing)
- Con: Too granular, not useful for cutting video

Layer 1 (Sub-topics):
- Pro: Good balance of speed and usefulness
- Con: Still need grouping

Layer 2 (Topics):
- Pro: Final segments for video
- Con: Long delay (8+ minutes)

Together: Users see progress in real-time, final videos in reasonable time
```

### 16.3 Trigger Logic

**From layer_manager.py:**

```python
# Layer 0: Every 30 seconds
if time.time() - self.last_layer0_trigger >= 30:
    self.process_layer0()
    self.last_layer0_trigger = time.time()

# Layer 1: Every 2 minutes
if time.time() - self.last_layer1_trigger >= 120:
    self.process_layer1()
    self.last_layer1_trigger = time.time()

# Layer 2: Every 8 minutes
if time.time() - self.last_layer2_trigger >= 480:
    self.process_layer2()
    self.last_layer2_trigger = time.time()
```

### 16.4 State Persistence

Each layer maintains state so system survives restarts:

```python
# Layer states saved in: data/state/layer_states.json
{
    "layer0": {
        "last_processed_segment": 42,
        "last_trigger_time": 1234567890.5
    },
    "layer1": {
        "last_processed_segment": 40,
        "last_trigger_time": 1234567800.5,
        "buffer": [transcript_id_40, 41, 42]
    },
    "layer2": {
        "last_processed_segment": 36,
        "last_trigger_time": 1234567200.5
    }
}
```

**Crash Recovery:**

```python
# On restart, load state
with open("data/state/layer_states.json", 'r') as f:
    states = json.load(f)

# Resume from where we left off
last_processed = states['layer1']['last_processed_segment']  # 40
# Start layer 1 processing from segment 41
```

### 16.5 Progressive Output

Each layer writes to its own JSON file:

```json
// output/sentences.json (updates every 30-60s)
[
  {"id": 1, "text": "Good evening", "start": 0, "end": 2},
  {"id": 2, "text": "This is the news", "start": 2, "end": 5},
  ...
]

// output/subtopics.json (updates every 2min)
[
  {
    "id": "sub_1",
    "title": "Breaking news from politics",
    "segments": [1, 2, 3, 4, 5],
    "start": 0,
    "end": 150
  },
  ...
]

// output/topics.json (updates every 8min)
[
  {
    "id": "topic_1",
    "title": "Election Results Analysis",
    "video_path": "output/topics/topic_001/video.mp4",
    "transcript_path": "output/topics/topic_001/transcript.txt",
    "subtopics": ["sub_1", "sub_2"]
  },
  ...
]
```

### Claude Code Prompts for Chapter 16

1. **"Explain the 3-layer architecture. Why not just have one layer that detects topics?"**

2. **"Show me the trigger logic in layer_manager.py. How does it decide when to process each layer?"**

3. **"Design a 4-layer architecture for podcast transcription. What would Layer 3 detect?"**

### Repository Practice Exercise

**Exercise 16.1: Understand Layer States**

```python
import json
from pathlib import Path

# Load layer states
state_file = Path("data/state/layer_states.json")

if state_file.exists():
    with open(state_file, 'r') as f:
        states = json.load(f)

    # Print state of each layer
    for layer_name, layer_state in states.items():
        print(f"\n{layer_name}:")
        print(f"  Last processed: {layer_state.get('last_processed_segment')}")
        print(f"  Last trigger: {layer_state.get('last_trigger_time')}")

        if 'buffer' in layer_state:
            print(f"  Buffer size: {len(layer_state['buffer'])}")
else:
    print("Layer states not initialized yet")
```

**Exercise 16.2: Check Progressive Output**

```python
import json
from pathlib import Path

output_dir = Path("output")

# Check what's been written by each layer
for output_file in ["sentences.json", "subtopics.json", "topics.json"]:
    path = output_dir / output_file

    if path.exists():
        with open(path, 'r') as f:
            data = json.load(f)

        print(f"\n{output_file}:")
        print(f"  Total items: {len(data)}")

        if data:
            print(f"  First item: {data[0].get('title', data[0].get('text', '...'))[:50]}")
            print(f"  Last item: {data[-1].get('title', data[-1].get('text', '...'))[:50]}")
    else:
        print(f"\n{output_file}: Not created yet")
```

---

## Chapter 17: Video Processing and FFmpeg

### What You'll Learn

Once the system detects a topic boundary, it must:
1. Find all segments that belong to that topic
2. Merge them into a single MP4 video
3. Synchronize with transcripts

### 17.1 FFmpeg Core Operations

**Merge segments without re-encoding (fast!):**

```bash
# Create list of segments to merge
echo "file 'data/raw_segments/segment_00001.ts'" > list.txt
echo "file 'data/raw_segments/segment_00002.ts'" >> list.txt
echo "file 'data/raw_segments/segment_00003.ts'" >> list.txt

# Merge with COPY codec (no re-encoding)
ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp4
```

**From Python:**

```python
import subprocess
from pathlib import Path

def merge_segments(segment_ids: List[int], output_path: Path) -> bool:
    """Merge video segments into single file."""

    # Create concat list
    concat_list = Path("temp_concat.txt")
    with open(concat_list, 'w') as f:
        for seg_id in segment_ids:
            seg_path = Path(f"data/raw_segments/segment_{seg_id:05d}.ts")
            f.write(f"file '{seg_path.absolute()}'\n")

    try:
        # Merge with ffmpeg
        cmd = [
            'ffmpeg',
            '-f', 'concat',
            '-safe', '0',
            '-i', str(concat_list),
            '-c', 'copy',  # No re-encoding!
            '-y',  # Overwrite output
            str(output_path)
        ]

        result = subprocess.run(cmd, capture_output=True, check=True, timeout=300)
        concat_list.unlink()  # Clean up
        return True

    except Exception as e:
        logger.error(f"Merge failed: {e}")
        return False
```

### 17.2 Creating SRT Captions

```python
def create_srt_file(transcript: Transcript, output_path: Path):
    """Create SRT subtitle file."""

    with open(output_path, 'w') as f:
        for i, word in enumerate(transcript.words, 1):
            # Convert seconds to HH:MM:SS,mmm format
            start = format_timestamp(word.start)
            end = format_timestamp(word.end)

            # SRT format:
            # 1
            # 00:00:01,500 --> 00:00:07,000
            # Hello world

            f.write(f"{i}\n")
            f.write(f"{start} --> {end}\n")
            f.write(f"{word.text}\n")
            f.write("\n")

def format_timestamp(seconds: float) -> str:
    """Convert seconds to SRT timestamp."""
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = int(seconds % 60)
    millis = int((seconds % 1) * 1000)
    return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"
```

### 17.3 Handling Edge Cases

```python
# Segments might have gaps (missing/corrupt segments)
all_segments = [1, 2, 3, 4, 5, 7, 8, 9]  # Missing 6
# Decision: Skip 6 or stop at 5?

# Segments might be incomplete (partial data)
def is_segment_valid(segment_path: Path, min_size: int = 1000000) -> bool:
    """Check if segment file is valid."""
    if not segment_path.exists():
        return False

    size = segment_path.stat().st_size
    if size < min_size:
        logger.warning(f"Segment too small: {size} bytes")
        return False

    return True

# Videos might fail to merge
# Fallback: Create symlink instead of merge?
# Or: Use different codec if copy fails?
```

### Claude Code Prompts for Chapter 17

1. **"Explain why FFmpeg uses `-c copy` instead of re-encoding. What would happen with `-c:v libx264`?"**

2. **"Show me how to create SRT captions synchronized with transcript words."**

3. **"Create a function that checks if all required segments exist before attempting merge."**

### Repository Practice Exercise

**Exercise 17.1: Test Video Merging**

```python
from pathlib import Path
import subprocess

# Find some real segments
segments_dir = Path("data/raw_segments")
segments = sorted(segments_dir.glob("segment_*.ts"))[:5]  # First 5

if len(segments) >= 3:
    # Create concat file
    concat_file = Path("temp_concat.txt")
    with open(concat_file, 'w') as f:
        for seg in segments:
            f.write(f"file '{seg.absolute()}'\n")

    # Merge
    output = Path("test_merge.mp4")
    cmd = [
        'ffmpeg',
        '-f', 'concat',
        '-safe', '0',
        '-i', str(concat_file),
        '-c', 'copy',
        '-y',
        str(output)
    ]

    result = subprocess.run(cmd, capture_output=True)

    if result.returncode == 0:
        print(f"Success! Created {output}")
        print(f"Size: {output.stat().st_size / 1024 / 1024:.1f} MB")
    else:
        print(f"Error: {result.stderr.decode()}")

    # Cleanup
    concat_file.unlink()
else:
    print("Not enough segments to test merge")
```

**Exercise 17.2: Generate SRT from Transcript**

```python
import json
from pathlib import Path
from src.models.segment import Transcript

def format_timestamp(seconds: float) -> str:
    """Format as HH:MM:SS,mmm."""
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = int(seconds % 60)
    millis = int((seconds % 1) * 1000)
    return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"

def create_srt(transcript: Transcript, output_path: Path):
    """Create SRT file from transcript."""
    with open(output_path, 'w') as f:
        for i, word in enumerate(transcript.words, 1):
            f.write(f"{i}\n")
            f.write(f"{format_timestamp(word.start)} --> {format_timestamp(word.end)}\n")
            f.write(f"{word.text}\n")
            f.write("\n")

# Load transcript
trans_file = Path("data/transcripts/transcript_00001.json")
if trans_file.exists():
    with open(trans_file) as f:
        data = json.load(f)

    transcript = Transcript.from_dict(data)

    # Create SRT
    srt_file = Path("test_output.srt")
    create_srt(transcript, srt_file)
    print(f"Created: {srt_file}")
else:
    print("No transcript found")
```

---

## Chapter 18: FastAPI REST APIs

### What You'll Learn

Your system has a REST API for control and a WebSocket API for real-time updates. FastAPI makes this simple with automatic validation and docs.

### 18.1 FastAPI Basics

**From server.py lines 1-100:**

```python
from fastapi import FastAPI, HTTPException, Query
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
from typing import Optional

app = FastAPI(title="Stream Segmenter API")

# CORS: Allow browser requests
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Pydantic model for request validation
class StartConfig(BaseModel):
    """Configuration for starting the pipeline."""
    preset_id: str
    elevenlabs_api_key: str
    stream_url: Optional[str] = None

    def get_stream_url(self) -> Optional[str]:
        return self.stream_url

# Response model
class PipelineStatus(BaseModel):
    running: bool
    preset_id: Optional[str] = None
    stream_url: Optional[str] = None
    started_at: Optional[str] = None

# GET endpoint
@app.get("/api/status", response_model=PipelineStatus)
async def get_status():
    """Get current pipeline status."""
    return {
        "running": segmenter_instance.is_running,
        "preset_id": segmenter_instance.preset_id,
        "started_at": segmenter_instance.started_at_str
    }

# POST endpoint
@app.post("/api/start")
async def start_pipeline(config: StartConfig):
    """Start the pipeline with given config."""
    try:
        # Pydantic automatically validates config
        segmenter_instance = LiveStreamSegmenter.start_with_config(config.dict())
        return {"status": "started", "preset_id": config.preset_id}

    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

# Query parameters
@app.get("/api/transcripts")
async def get_transcripts(
    status: Optional[str] = Query(None),
    limit: int = Query(100)
):
    """Get transcripts with optional filtering."""
    # Filter by status if provided
    transcripts = timeline_manager.get_transcripts()

    if status:
        transcripts = [t for t in transcripts if t.get('status') == status]

    return transcripts[:limit]
```

### 18.2 Automatic API Documentation

```
GET http://localhost:8000/docs
```

FastAPI automatically generates interactive documentation!
- Try endpoints in browser
- See request/response schemas
- Auto-validates based on Pydantic models

### 18.3 Error Handling

```python
from fastapi import HTTPException

@app.post("/api/start")
async def start(config: StartConfig):
    if not config.stream_url:
        raise HTTPException(
            status_code=400,
            detail="stream_url is required"
        )

    if not config.elevenlabs_api_key:
        raise HTTPException(
            status_code=401,
            detail="API key required"
        )

    try:
        segmenter = LiveStreamSegmenter.start_with_config(...)
        return {"status": "started"}
    except Exception as e:
        raise HTTPException(
            status_code=500,
            detail=f"Failed to start: {str(e)}"
        )
```

### 18.4 Running the Server

```python
import uvicorn

# Start server
uvicorn.run(
    app,
    host="0.0.0.0",
    port=8000,
    log_level="info"
)

# In code:
# uvicorn server:app --reload --host 0.0.0.0 --port 8000
```

### Claude Code Prompts for Chapter 18

1. **"Show me the /api/start endpoint in server.py. Why use Pydantic BaseModel for config?"**

2. **"Explain CORS middleware. Why is it needed for browser requests?"**

3. **"Create a new endpoint that gets segments by time range: /api/segments?start_time=100&end_time=200"**

### Repository Practice Exercise

**Exercise 18.1: Test API Locally**

```python
from fastapi.testclient import TestClient
from src.api.server import app

client = TestClient(app)

# Test status endpoint
response = client.get("/api/status")
print(f"Status: {response.status_code}")
print(response.json())

# Test with invalid request
response = client.post("/api/start", json={})
print(f"Error: {response.status_code}")
print(response.json())
```

**Exercise 18.2: Create a New Endpoint**

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List

app = FastAPI()

class Segment(BaseModel):
    id: int
    status: str

segments = [
    {"id": 1, "status": "pending"},
    {"id": 2, "status": "processing"},
    {"id": 3, "status": "complete"},
]

@app.get("/segments", response_model=List[Segment])
async def get_segments(status: Optional[str] = None):
    """Get segments with optional status filter."""
    if status:
        return [s for s in segments if s['status'] == status]
    return segments

# Test it
from fastapi.testclient import TestClient
client = TestClient(app)

print(client.get("/segments").json())
print(client.get("/segments?status=complete").json())
```

---

## Chapter 19: Logging, Debugging, and Monitoring

### What You'll Learn

Professional systems log everything. When something fails in production, logs are your only window into what happened.

### 19.1 Python's Logging Module

**From main.py lines 43-52:**

```python
import logging

# Setup logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s | %(name)s | %(levelname)s | %(message)s',
    handlers=[
        logging.FileHandler('logs/system.log'),
        logging.StreamHandler()  # Also print to console
    ]
)

logger = logging.getLogger(__name__)

# Use logging
logger.debug("Detailed info for debugging")       # Only with level=DEBUG
logger.info("Important event occurred")           # Normal level
logger.warning("Something unexpected happened")   # Warning level
logger.error("Serious problem")                  # Error level
logger.critical("System failing!")               # Critical
```

**Log Levels (increasing severity):**

```
DEBUG (10)     - Detailed diagnostic info
INFO (20)      - Confirmation that system working
WARNING (30)   - Unexpected situation, but system continues
ERROR (40)     - Serious problem, operation failed
CRITICAL (50)  - System cannot continue
```

### 19.2 Structured Logging with Context

```python
# Bad - hard to debug
logger.error("Transcription failed")

# Good - includes context
logger.error(
    "Transcription failed",
    extra={
        'segment_id': segment_id,
        'audio_path': audio_path,
        'duration': duration,
        'error_code': error_code
    }
)

# Better - with formatting
logger.error(
    f"Transcription failed for segment {segment_id}: {error_msg}",
    extra={'audio_path': str(audio_path)}
)
```

### 19.3 Reading Logs

```bash
# Watch logs in real-time
tail -f logs/system.log

# Get last 100 lines
tail -100 logs/system.log

# Search for errors
grep ERROR logs/system.log

# Count different log levels
grep INFO logs/system.log | wc -l   # Count INFO messages
grep ERROR logs/system.log | wc -l  # Count errors
```

**Python:**

```python
# Search logs programmatically
with open('logs/system.log', 'r') as f:
    errors = [line for line in f if 'ERROR' in line]
    print(f"Total errors: {len(errors)}")

    for error in errors[-10:]:  # Last 10 errors
        print(error)
```

### 19.4 Performance Logging

```python
import time

start = time.time()

# Do something
result = transcriber.transcribe(audio_path)

elapsed = time.time() - start

logger.info(
    f"Transcription completed in {elapsed:.2f}s",
    extra={
        'segment_id': segment_id,
        'duration': elapsed,
        'words': len(result.words)
    }
)

# Log if slow
if elapsed > 10:
    logger.warning(f"Transcription slow: {elapsed:.2f}s")
```

### 19.5 Exception Logging

```python
try:
    result = risky_operation()
except Exception as e:
    # Log with stack trace
    logger.error(
        f"Operation failed: {str(e)}",
        exc_info=True  # Include stack trace!
    )
```

### Claude Code Prompts for Chapter 19

1. **"Show me how logging is set up in main.py. Why log to both file and console?"**

2. **"Create a function that parses logs/system.log and prints statistics: total messages, errors, warnings."**

3. **"Add timing logs to the extract_audio function in transcriber.py."**

### Repository Practice Exercise

**Exercise 19.1: Analyze Your Logs**

```python
from pathlib import Path
from collections import Counter
import re

log_file = Path("logs/system.log")

if log_file.exists():
    with open(log_file, 'r') as f:
        lines = f.readlines()

    # Count log levels
    levels = Counter()
    for line in lines:
        if ' INFO ' in line:
            levels['INFO'] += 1
        elif ' ERROR ' in line:
            levels['ERROR'] += 1
        elif ' WARNING ' in line:
            levels['WARNING'] += 1

    print("Log Summary:")
    for level, count in levels.most_common():
        print(f"  {level}: {count}")

    # Find slowest operations
    print("\nSlowest operations:")
    for line in lines[-100:]:  # Last 100 lines
        if 'took' in line.lower() or 'time' in line.lower():
            print(f"  {line.strip()}")
else:
    print("No logs yet")
```

**Exercise 19.2: Create a Log Analyzer**

```python
import logging
from pathlib import Path
from collections import defaultdict

def analyze_logs(log_file: Path):
    """Analyze and summarize logs."""

    stats = defaultdict(int)
    errors = []

    with open(log_file, 'r') as f:
        for line in f:
            # Count levels
            if ' INFO ' in line:
                stats['INFO'] += 1
            elif ' ERROR ' in line:
                stats['ERROR'] += 1
                errors.append(line.strip())
            elif ' WARNING ' in line:
                stats['WARNING'] += 1

    # Print report
    print("=== Log Analysis ===")
    print(f"Total lines: {sum(stats.values())}")

    for level in ['ERROR', 'WARNING', 'INFO']:
        print(f"{level}: {stats[level]}")

    print(f"\nLast 5 errors:")
    for error in errors[-5:]:
        print(f"  {error[:100]}...")

# Run it
analyze_logs(Path("logs/system.log"))
```

---

## Chapter 20: Testing with Pytest

### What You'll Learn

Tests ensure your code works and keep it working as you make changes.

### 20.1 Basic Test Structure

```python
# src/models/segment.py (the code)
@dataclass
class Word:
    text: str
    start: float
    end: float
    confidence: float = 1.0

# tests/test_models.py (the test)
import pytest
from src.models.segment import Word

def test_word_creation():
    """Test that we can create a Word."""
    word = Word("hello", 1.0, 2.0)
    assert word.text == "hello"
    assert word.start == 1.0
    assert word.end == 2.0
    assert word.confidence == 1.0

def test_word_default_confidence():
    """Test default confidence value."""
    word = Word("hello", 1.0, 2.0)
    assert word.confidence == 1.0  # Default

def test_word_to_dict():
    """Test serialization."""
    word = Word("hello", 1.0, 2.0, confidence=0.95)
    d = word.to_dict()

    assert d['text'] == "hello"
    assert d['confidence'] == 0.95

def test_word_from_dict():
    """Test deserialization."""
    d = {'text': 'world', 'start': 2.0, 'end': 3.0, 'confidence': 0.90}
    word = Word.from_dict(d)

    assert word.text == 'world'
    assert word.confidence == 0.90
```

### 20.2 Running Tests

```bash
# Run all tests
pytest

# Run specific file
pytest tests/test_models.py

# Run specific test
pytest tests/test_models.py::test_word_creation

# Verbose
pytest -v

# Show print statements
pytest -s

# Stop on first failure
pytest -x

# Run last failed tests
pytest --lf

# Coverage
pytest --cov=src
```

### 20.3 Fixtures (Reusable Setup)

```python
# conftest.py (shared fixtures)
import pytest
from src.models.segment import Word, Transcript

@pytest.fixture
def sample_word():
    """A sample Word for tests."""
    return Word("hello", 1.0, 2.0, confidence=0.95)

@pytest.fixture
def sample_transcript():
    """A sample Transcript."""
    return Transcript(
        segment_id=1,
        text="Hello world",
        words=[
            Word("hello", 0.0, 1.0),
            Word("world", 1.0, 2.0)
        ]
    )

# Use in test
def test_transcript_word_count(sample_transcript):
    assert len(sample_transcript.words) == 2

def test_word_properties(sample_word):
    assert sample_word.text == "hello"
```

### 20.4 Mocking External Dependencies

```python
from unittest.mock import patch, MagicMock

def test_transcriber_with_mocked_api():
    """Test transcriber without calling real API."""

    with patch('elevenlabs.ElevenLabs') as mock_api:
        # Setup mock
        mock_api.return_value.speech_to_text.convert.return_value = MagicMock(
            text="Hello world",
            words=[{'text': 'hello', 'start': 0, 'end': 1}]
        )

        # Now test transcriber
        from src.core.transcriber import ElevenLabsTranscriber
        transcriber = ElevenLabsTranscriber(api_key="fake_key")

        # This uses mocked API, not real one
        result = transcriber.transcribe(Path("audio.wav"), segment_id=1)

        assert result.text == "Hello world"
        assert len(result.words) == 1
```

### Claude Code Prompts for Chapter 20

1. **"Show me the test structure in tests/ directory. What are the fixtures in conftest.py?"**

2. **"Write tests for the StateManager class. What should we test?"**

3. **"Create tests for the extract_audio function using mocked subprocess calls."**

### Repository Practice Exercise

**Exercise 20.1: Write Tests for Segment**

```python
# tests/test_segment.py
import pytest
from pathlib import Path
from src.models.segment import Segment, Word
from datetime import datetime

def test_segment_creation():
    """Test Segment creation."""
    seg = Segment(
        id=1,
        file_path=Path("data/segment_001.ts"),
        start_time=0.0,
        end_time=30.0
    )

    assert seg.id == 1
    assert seg.duration == 30.0
    assert seg.status == "pending"

def test_segment_to_dict():
    """Test serialization."""
    seg = Segment(
        id=1,
        file_path=Path("segment_001.ts"),
        start_time=0.0,
        end_time=30.0
    )

    d = seg.to_dict()
    assert d['id'] == 1
    assert d['file_path'] == "segment_001.ts"

def test_segment_from_dict():
    """Test deserialization."""
    d = {
        'id': 1,
        'file_path': 'segment_001.ts',
        'start_time': 0.0,
        'end_time': 30.0,
        'status': 'pending',
        'created_at': datetime.now().isoformat(),
        'file_size': 1000000
    }

    seg = Segment.from_dict(d)
    assert seg.id == 1
    assert isinstance(seg.file_path, Path)

def test_segment_roundtrip():
    """Test to_dict and from_dict roundtrip."""
    original = Segment(
        id=42,
        file_path=Path("segment_042.ts"),
        start_time=1200.0,
        end_time=1230.0,
        status="transcribed"
    )

    # Serialize and deserialize
    d = original.to_dict()
    restored = Segment.from_dict(d)

    # Should be identical
    assert restored.id == original.id
    assert restored.status == original.status
```

**Exercise 20.2: Run Tests**

```bash
# Run the test file
pytest tests/test_segment.py -v

# Run with coverage
pytest tests/test_segment.py --cov=src.models

# Run specific test
pytest tests/test_segment.py::test_segment_creation -v
```

---

## Chapter 21: Understanding the Complete System

### What You'll Learn

It's time to put it all together. Understand how every piece works and how they communicate.

### 21.1 Complete Data Flow

```
STEP 1: INPUT
  YouTube URL (from API or config)
        ↓
  yt-dlp extracts HLS stream URL
        ↓
  FFmpeg connects to HLS stream

STEP 2: INGESTION (thread 1)
  FFmpeg writes segments every 30s
  segment_00001.ts (30 seconds of video)
  segment_00002.ts
  segment_00003.ts
        ↓
  File monitor detects new segments
  Adds to SQLite state database

STEP 3: TRANSCRIPTION (thread 2)
  Audio extracted from each segment
  FFmpeg: segment_00001.ts → audio_00001.wav
        ↓
  ElevenLabs API transcribes
  Returns: words with timestamps
        ↓
  Transcript saved:
  - SQLite database (for crash recovery)
  - Timeline (JSONL for fast queries)
  - Progress sent to WebSocket clients (real-time)

STEP 4: LAYER 0 - SENTENCES (every 30s)
  Process latest segments
  Extract sentences from transcripts
  Output: sentences.json (for display)

STEP 5: LAYER 1 - SUB-TOPICS (every 120s)
  Accumulate last 2 minutes of transcripts
  Send to Gemini LLM: "Has topic changed?"
  Get back: topic_boundary, confidence
  Output: subtopics.json
  Broadcast to WebSocket clients (real-time)

STEP 6: LAYER 2 - TOPICS (every 480s)
  Accumulate last 8 minutes
  Analyze with Gemini: "Group subtopics into parent topics"
  When topic complete:
        ↓
  STEP 7: VIDEO MERGING
    Get all segments for topic
    FFmpeg concat: merge to single MP4
    Create SRT subtitles from transcript
    Save metadata (title, summary, keywords)
        ↓
  OUTPUT CREATED:
    output/topics/topic_001/
    ├── video.mp4
    ├── transcript.txt
    ├── subtitles.srt
    └── metadata.json

    Broadcast to WebSocket: "New topic available"
```

### 21.2 System Components and Interactions

```python
# Main orchestrator (main.py)
class LiveStreamSegmenter:
    def __init__(self):
        # Initialize all components
        self.stream_ingestion = StreamIngestion()    # FFmpeg
        self.transcriber = ElevenLabsTranscriber()   # STT
        self.state_manager = StateManager()          # SQLite
        self.layer_manager = LayerManager()          # Analysis
        self.video_manager = VideoManager()          # Video cutting
        self.websocket_manager = WebSocketManager()  # Real-time

    def run(self):
        """Main loop."""
        while self.running:
            # Get latest segments
            new_segments = self.stream_ingestion.get_new_segments()

            for segment in new_segments:
                # Transcribe
                transcript = self.transcriber.transcribe(segment)

                # Save state
                self.state_manager.add_transcript(transcript)

                # Analyze with layers
                results = self.layer_manager.process(transcript)

                # Broadcast to UI
                self.websocket_manager.broadcast(results)

                # If topic complete, create video
                if results.get('video_ready'):
                    self.video_manager.create_video(results['topic_id'])
```

### 21.3 Error Recovery

Your system survives crashes:

```
CRASH!
↓
System restarts
↓
Load state from database:
- Last processed segment
- Layer states
- Buffered transcripts
↓
Resume from exact point
↓
No data loss!
```

### 21.4 Performance Bottlenecks

Where your system might get slow:

1. **FFmpeg audio extraction** (~5s per segment)
   - Solution: Parallel extraction of multiple segments

2. **ElevenLabs API calls** (~3s per segment)
   - Solution: Batch multiple segments, use WebSocket for lower latency

3. **Gemini LLM analysis** (~5s per analysis)
   - Solution: Lower temperature for faster, consistent responses

4. **Video merging** (~30s for 10min video)
   - Solution: Use `-c copy` (no re-encoding)

5. **Large JSON files** (millions of words)
   - Solution: Store in JSONL (one per line), read incrementally

### 21.5 Production Checklist

```python
def is_production_ready():
    """Verify system readiness."""

    checks = {
        "API keys set": os.getenv('GEMINI_API_KEY') is not None,
        "Logs writable": Path('logs').exists(),
        "Data dir exists": Path('data').exists(),
        "FFmpeg available": shutil.which('ffmpeg') is not None,
        "yt-dlp available": shutil.which('yt-dlp') is not None,
        "Database initialized": StateManager().is_initialized(),
        "Config valid": load_config() is not None,
    }

    for check, result in checks.items():
        status = "✓" if result else "✗"
        print(f"{status} {check}")

    return all(checks.values())

# Run before production
if not is_production_ready():
    sys.exit("System not ready!")
```

### 21.6 End-to-End Example

Let's trace a complete flow:

```python
# 1. Start segmenter
segmenter = LiveStreamSegmenter.start_with_config({
    'youtube_url': 'https://www.youtube.com/watch?v=...',
    'preset_id': 'budget',
    'elevenlabs_api_key': os.getenv('ELEVENLABS_API_KEY')
})

# 2. System starts
# - FFmpeg connects to YouTube stream
# - Segments begin arriving every 30s
# - WebSocket broadcasts ready to clients

# 3. For each segment (automatically)
# - Extract audio
# - Transcribe with ElevenLabs
# - Save to database
# - Broadcast to UI

# 4. Every 30 seconds (Layer 0)
# - Extract sentences
# - Update sentences.json
# - WebSocket: "New sentences available"

# 5. Every 2 minutes (Layer 1)
# - Detect subtopic boundaries
# - Update subtopics.json
# - WebSocket: "New subtopic"

# 6. Every 8 minutes (Layer 2)
# - Detect topic completion
# - Merge video segments
# - Create output files
# - WebSocket: "New topic video available"

# 7. Users see live results
# - Real-time transcription
# - Topics appear as they're detected
# - Videos ready for download
```

### Claude Code Prompts for Chapter 21

1. **"Trace the complete data flow from YouTube URL to final MP4 video. What happens at each step?"**

2. **"Show me how the system survives crashes. Where is state saved?"**

3. **"Identify the 3 biggest performance bottlenecks. How would you optimize each?"**

### Repository Practice Exercise

**Exercise 21.1: Complete System Trace**

```python
# Create a detailed trace through the system

from pathlib import Path
from src.models.segment import Segment, Transcript
from src.services.state_manager import StateManager
import json

# 1. Check ingestion
segments_dir = Path("data/raw_segments")
segments = sorted(segments_dir.glob("segment_*.ts"))
print(f"Ingested: {len(segments)} segments")
print(f"  Total data: {sum(s.stat().st_size for s in segments) / 1024 / 1024 / 1024:.2f} GB")

# 2. Check transcription
transcripts_dir = Path("data/transcripts")
transcripts = sorted(transcripts_dir.glob("transcript_*.json"))
print(f"\nTranscribed: {len(transcripts)} segments")

if transcripts:
    with open(transcripts[0]) as f:
        t = json.load(f)
    words = len(t.get('words', []))
    print(f"  Words per segment: ~{words}")
    print(f"  Total words: ~{words * len(transcripts)}")

# 3. Check layers
output_dir = Path("output")
for layer_file in ["sentences.json", "subtopics.json", "topics.json"]:
    path = output_dir / layer_file
    if path.exists():
        with open(path) as f:
            data = json.load(f)
        print(f"\n{layer_file}: {len(data)} items")

# 4. Check state recovery
state_file = Path("data/state/layer_states.json")
if state_file.exists():
    with open(state_file) as f:
        states = json.load(f)
    print(f"\nLayer states saved: {list(states.keys())}")

print("\n✓ System progression verified")
```

**Exercise 21.2: Performance Analysis**

```python
import json
from pathlib import Path
from collections import defaultdict
import time

# Analyze where time is spent

timings = defaultdict(list)

# Check log timestamps
log_file = Path("logs/system.log")

if log_file.exists():
    with open(log_file) as f:
        lines = f.readlines()

    # Parse timestamps and operations
    for line in lines:
        parts = line.split(" | ")
        if len(parts) >= 3:
            timestamp = parts[0]
            operation = parts[2]

            if "Transcription" in operation:
                # Extract duration from message
                if "seconds" in operation.lower():
                    try:
                        duration = float(operation.split()[-2])
                        timings['transcription'].append(duration)
                    except:
                        pass

            elif "Merge" in operation:
                if "seconds" in operation.lower():
                    try:
                        duration = float(operation.split()[-2])
                        timings['video_merge'].append(duration)
                    except:
                        pass

    # Print statistics
    print("=== Performance Analysis ===\n")

    for op, durations in timings.items():
        if durations:
            avg = sum(durations) / len(durations)
            max_d = max(durations)
            min_d = min(durations)
            print(f"{op}:")
            print(f"  Average: {avg:.2f}s")
            print(f"  Min: {min_d:.2f}s")
            print(f"  Max: {max_d:.2f}s")
            print()
```

---

# APPENDICES

## Appendix A: Complete Codebase Reading Roadmap

### Level 1: Foundation (Start Here!)
**Time: 3-4 hours**

1. `src/models/segment.py` - Data structures
   - Understand: Dataclasses, type hints, JSON serialization
   - Learn: Why `Path` instead of strings, properties

2. `src/models/cut_video.py` - Response models
   - Understand: Pydantic vs dataclasses, enums

### Level 2: Simple Services
**Time: 6-8 hours**

3. `src/utils/srt_parser.py` - Text parsing
   - Understand: Regex, file I/O, string operations

4. `src/services/state_manager.py` - Database operations
   - Understand: SQLite, context managers, CRUD operations
   - Learn: Transaction handling, row factories

5. `src/services/context_manager.py` - Configuration
   - Understand: YAML parsing, config management

### Level 3: Core Processing
**Time: 8-10 hours**

6. `src/core/transcriber.py` - Audio transcription
   - Understand: Subprocess, API client patterns, retry logic
   - Learn: Factory pattern, error handling

7. `src/services/gemini_llm_service.py` - LLM integration
   - Understand: API clients, JSON parsing, retry patterns
   - Learn: Temperature, safety settings, prompt structure

8. `src/core/stream_processor.py` - Stream processing
   - Understand: Threading, queues, file watching
   - Learn: Async/sync bridging

### Level 4: Advanced Systems
**Time: 12-15 hours**

9. `src/core/stream_ingestion.py` - Stream connection
   - Understand: Subprocess piping, process monitoring
   - Learn: Complex error handling, rate limiting

10. `src/core/layer_manager.py` - Progressive layers
    - Understand: State machines, trigger logic
    - Learn: Architecture design

11. `src/api/server.py` - REST API
    - Understand: FastAPI, Pydantic, WebSockets
    - Learn: API design, async patterns

12. `src/main.py` - Orchestration
    - **READ THIS LAST!** (1175 lines)
    - Understand: System initialization, lifecycle management
    - Learn: Configuration loading, signal handling, main loop

## Appendix B: JavaScript to Python Cheat Sheet

| JavaScript | Python | Example |
|-----------|--------|---------|
| `const x = 5` | `x = 5` | Variables |
| `let name = "Alice"` | `name = "Alice"` | Variables |
| `typeof x` | `type(x)` | Type checking |
| `Array<string>` | `List[str]` | Type hints |
| `Record<string, any>` | `Dict[str, Any]` | Type hints |
| `x \|\| default` | `x or default` | Defaults |
| `arr.map(x => x * 2)` | `[x * 2 for x in arr]` | Transform |
| `arr.filter(x => x > 5)` | `[x for x in arr if x > 5]` | Filter |
| `obj.method()` | `obj.method()` | Methods |
| `new Class()` | `Class()` | Instantiate |
| `async/await` | `async/await` | Async |
| `Promise.all()` | `asyncio.gather()` | Parallel |
| `try/catch/finally` | `try/except/finally` | Error handling |
| `JSON.stringify()` | `json.dumps()` | Serialize |
| `JSON.parse()` | `json.loads()` | Deserialize |
| `fs.readFile()` | `open(..., 'r')` | File reading |
| `process.env.KEY` | `os.getenv('KEY')` | Env vars |
| `setTimeout()` | `time.sleep()` | Wait |

## Appendix C: Key Files by Concept

| Concept | File | Lines |
|---------|------|-------|
| **Dataclasses** | segment.py | 17-97 |
| **JSON Serialization** | segment.py | 72-91 |
| **SQLite** | state_manager.py | 45-230 |
| **API Clients** | transcriber.py | 225-334 |
| **Retry Logic** | gemini_llm_service.py | 117-172 |
| **Threading** | main.py | 671-678 |
| **Async/Await** | stream_processor.py | 173-180 |
| **WebSockets** | server.py | WebSocket handler |
| **Subprocess** | transcriber.py | 160-177 |
| **Prompts** | layer1_prompts.py | All |
| **FastAPI** | server.py | Route decorators |
| **Logging** | main.py | 43-52 |

## Appendix D: Common Python Gotchas

### 1. Mutable Default Arguments

```python
# WRONG - list shared across all calls!
def add_item(item, items=[]):
    items.append(item)
    return items

add_item(1)  # [1]
add_item(2)  # [1, 2] - not [2]!

# RIGHT - create new list each time
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

### 2. Indentation Matters

```python
if True:
    x = 1
    y = 2
    z = 3  # Part of if block

z = 4  # Outside if block
```

### 3. String/Path Confusion

```python
# WRONG
path = "data/transcripts/transcript_001.json"
path.exists()  # ERROR - strings don't have exists()

# RIGHT
from pathlib import Path
path = Path("data/transcripts/transcript_001.json")
path.exists()  # Works!
```

### 4. Equality vs Assignment

```python
# WRONG in if statements
if x = 5:  # SyntaxError!
    pass

# RIGHT
if x == 5:  # Correct
    pass

# Assignment
x = 5
```

### 5. Global Variables

```python
# WRONG - won't modify global
count = 0

def increment():
    count += 1  # Error! count not defined

# RIGHT
count = 0

def increment():
    global count
    count += 1
```

---

## Conclusion: Your Next Steps

Congratulations! You now understand:

✅ Python basics and data structures
✅ How to work with files and JSON
✅ OOP and dataclasses
✅ APIs and error handling
✅ Threading and async programming
✅ Your complete system architecture

### What to Do Now

1. **Pick a chapter** (start with Chapter 1)
2. **Read** the concept explanation
3. **Run** the code examples
4. **Ask Claude Code** the prompts for that chapter
5. **Do** the repository exercises
6. **Build** something with your new knowledge

### Your Learning Loop

```
Read Concept (10 min)
    ↓
Run Examples (15 min)
    ↓
Use Claude Code (20 min)
    ↓
Practice Exercise (30-60 min)
    ↓
Move to Next Chapter
```

### Estimated Timeline

- **Weeks 1-3**: Chapters 1-7 (Foundations)
- **Weeks 4-6**: Chapters 8-11 (Data & APIs)
- **Weeks 7-9**: Chapters 12-15 (Concurrency & Prompts)
- **Weeks 10-12**: Chapters 16-18 (Architecture & APIs)
- **Weeks 13-15**: Chapters 19-21 (Production & Integration)

### Success Metrics

You'll know you're learning when you can:

✅ Read any file in stream-segmenter and explain what it does
✅ Trace a video segment's journey from YouTube to final output
✅ Modify the topic detection logic
✅ Debug using logs
✅ Add a new API endpoint
✅ Explain your system to someone else
✅ Build your own Python AI system

---

**Good luck! You've got this! 🚀**
