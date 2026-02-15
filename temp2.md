# Python for AI Engineers: The Complete Ebook

**From Zero to Production-Ready Python for AI Development**

Written for a JS developer who builds with AI coding tools.
Every concept includes code + Claude Code workflow prompts.

> Learn the concept first. Then let Claude Code implement it. Then understand every line.

---

## How to Use This Book with Claude Code

This book is designed for a specific learning workflow that combines understanding with AI-assisted coding:

1. **Read the concept** in this book. Understand WHAT it does and WHY it exists.
2. **Try the code examples yourself first.** Type them, run them, see the output.
3. **Ask Claude Code to build something with it:** "Build a file parser using context managers."
4. **Ask Claude Code to explain it in your existing code:** "Show me where decorators are used in my project."
5. **Build the mini-project** at the end of each chapter. This is where real learning happens.

### Your Learning Loop

```
Read concept (10 min)
    |
Run example code (5 min)
    |
Ask Claude Code to build something with it (20 min)
    |
Ask Claude Code to find this pattern in your existing projects (10 min)
    |
Build the chapter project (1-2 hours)
```

---

## Index

### Quick Reference: Find Any Topic

| Topic | Chapter | Section |
|-------|---------|---------|
| Variables & data types | [Ch 1](#chapter-1-python-basics-for-js-developers) | [1.1](#11-variables--data-types) |
| Control flow (if/for/while) | [Ch 1](#chapter-1-python-basics-for-js-developers) | [1.2](#12-control-flow) |
| List comprehensions | [Ch 1](#chapter-1-python-basics-for-js-developers) | [1.3](#13-list-comprehensions-python-superpower) |
| String formatting (f-strings) | [Ch 1](#chapter-1-python-basics-for-js-developers) | [1.4](#14-string-formatting) |
| Lists (arrays) | [Ch 2](#chapter-2-data-structures-deep-dive) | [2.1](#21-lists-like-js-arrays) |
| Dictionaries (objects) | [Ch 2](#chapter-2-data-structures-deep-dive) | [2.2](#22-dictionaries-like-js-objects) |
| Tuples, Sets, Counter, defaultdict | [Ch 2](#chapter-2-data-structures-deep-dive) | [2.3](#23-tuples-sets--collections) |
| Deque (rolling buffer) | [Ch 2](#chapter-2-data-structures-deep-dive) | [2.4](#24-deque-rolling-buffers--queues) |
| Function basics, *args, **kwargs | [Ch 3](#chapter-3-functions-closures--decorators) | [3.1](#31-function-basics--argskwargs) |
| Decorators (@timer, @retry) | [Ch 3](#chapter-3-functions-closures--decorators) | [3.2](#32-decorators-python-superpower) |
| Generators & context managers | [Ch 3](#chapter-3-functions-closures--decorators) | [3.3](#33-generators--context-managers) |
| Classes & dataclasses | [Ch 4](#chapter-4-object-oriented-python) | [4.1](#41-classes--dataclasses) |
| Inheritance & properties | [Ch 4](#chapter-4-object-oriented-python) | [4.2](#42-inheritance-properties--dunder-methods) |
| @classmethod, @staticmethod | [Ch 4](#chapter-4-object-oriented-python) | [4.3](#43-class-methods-and-static-methods) |
| try/except/finally | [Ch 5](#chapter-5-error-handling--debugging) | [5.1](#51-tryexceptfinally) |
| Custom exceptions | [Ch 5](#chapter-5-error-handling--debugging) | [5.2](#52-custom-exceptions) |
| Logging (not print!) | [Ch 5](#chapter-5-error-handling--debugging) | [5.3](#53-logging-not-print) |
| Retry patterns | [Ch 5](#chapter-5-error-handling--debugging) | [5.4](#54-retry-pattern) |
| pathlib.Path | [Ch 6](#chapter-6-file-io--data-formats) | [6.1](#61-pathlib-modern-file-paths) |
| Reading/writing files | [Ch 6](#chapter-6-file-io--data-formats) | [6.2](#62-reading--writing-files) |
| JSON and JSONL | [Ch 6](#chapter-6-file-io--data-formats) | [6.3](#63-json-and-jsonl) |
| YAML configs | [Ch 6](#chapter-6-file-io--data-formats) | [6.4](#64-yaml-for-configuration) |
| Environment variables (.env) | [Ch 6](#chapter-6-file-io--data-formats) | [6.5](#65-environment-variables) |
| Project structure | [Ch 7](#chapter-7-modules-packages--project-structure) | [7.1](#71-python-project-structure) |
| Imports (absolute vs relative) | [Ch 7](#chapter-7-modules-packages--project-structure) | [7.2](#72-imports) |
| Virtual environments | [Ch 7](#chapter-7-modules-packages--project-structure) | [7.3](#73-virtual-environments) |
| NumPy arrays & operations | [Ch 8](#chapter-8-numpy--the-foundation-of-everything) | [8.1](#81-arrays-shapes--dtypes) |
| Pandas DataFrames | [Ch 9](#chapter-9-pandas--data-wrangling) | [9.1](#91-dataframes-basics) |
| Type hints & Pydantic | [Ch 10](#chapter-10-type-hints--pydantic) | [10.1](#101-type-hints-basics) |
| async/await | [Ch 11](#chapter-11-async-python--concurrency) | [11.1](#111-asyncawait-basics) |
| Threading | [Ch 11](#chapter-11-async-python--concurrency) | [11.4](#114-threading-for-cpu-bound-work) |
| FastAPI routes & models | [Ch 12](#chapter-12-fastapi--building-ai-ready-apis) | [12.1](#121-your-first-api) |
| WebSockets | [Ch 12](#chapter-12-fastapi--building-ai-ready-apis) | [12.5](#125-websockets-for-real-time) |
| SQLite / SQLAlchemy | [Ch 13](#chapter-13-databases-with-sqlalchemy) | [13.1](#131-sqlite-basics) |
| JWT authentication | [Ch 14](#chapter-14-authentication--security) | [14.1](#141-jwt-tokens) |
| Pytest basics & fixtures | [Ch 15](#chapter-15-testing-with-pytest) | [15.1](#151-your-first-test) |
| AI API integration | [Ch 16](#chapter-16-working-with-ai-apis) | [16.1](#161-anthropic-claude-api) |
| Subprocess (FFmpeg, yt-dlp) | [Ch 17](#chapter-17-data-pipelines-for-ai) | [17.3](#173-subprocess-running-external-programs) |
| PyTorch tensors | [Ch 18](#chapter-18-pytorch-essentials) | [18.1](#181-tensors) |
| Docker | [Ch 19](#chapter-19-docker--deployment) | [19.1](#191-dockerfile-for-python) |
| Structured logging | [Ch 20](#chapter-20-logging-monitoring--best-practices) | [20.1](#201-structured-logging) |
| Build projects | [Ch 21](#chapter-21-build-projects--next-steps) | All |

### Reading Flow

```
PART I: Python Foundations (Weeks 1-2)
  Ch 1: Basics ──> Ch 2: Data Structures ──> Ch 3: Functions
  ──> Ch 4: OOP ──> Ch 5: Error Handling ──> Ch 6: Files
  ──> Ch 7: Project Structure

PART II: Python for Data & AI (Week 3)
  Ch 8: NumPy ──> Ch 9: Pandas ──> Ch 10: Type Hints
  ──> Ch 11: Async & Concurrency

PART III: Backend & API Development (Weeks 4-5)
  Ch 12: FastAPI ──> Ch 13: Databases ──> Ch 14: Auth
  ──> Ch 15: Testing

PART IV: AI & ML Python (Week 5-6)
  Ch 16: AI APIs ──> Ch 17: Data Pipelines ──> Ch 18: PyTorch

PART V: Production Python (Week 6)
  Ch 19: Docker ──> Ch 20: Best Practices ──> Ch 21: Projects
```

---

## Table of Contents

- [How to Use This Book with Claude Code](#how-to-use-this-book-with-claude-code)
- [Index](#index)

### Part I: Python Foundations
- [Chapter 1: Python Basics for JS Developers](#chapter-1-python-basics-for-js-developers)
- [Chapter 2: Data Structures Deep Dive](#chapter-2-data-structures-deep-dive)
- [Chapter 3: Functions, Closures & Decorators](#chapter-3-functions-closures--decorators)
- [Chapter 4: Object-Oriented Python](#chapter-4-object-oriented-python)
- [Chapter 5: Error Handling & Debugging](#chapter-5-error-handling--debugging)
- [Chapter 6: File I/O & Data Formats](#chapter-6-file-io--data-formats)
- [Chapter 7: Modules, Packages & Project Structure](#chapter-7-modules-packages--project-structure)

### Part II: Python for Data & AI
- [Chapter 8: NumPy - The Foundation of Everything](#chapter-8-numpy--the-foundation-of-everything)
- [Chapter 9: Pandas - Data Wrangling](#chapter-9-pandas--data-wrangling)
- [Chapter 10: Type Hints & Pydantic](#chapter-10-type-hints--pydantic)
- [Chapter 11: Async Python & Concurrency](#chapter-11-async-python--concurrency)

### Part III: Backend & API Development
- [Chapter 12: FastAPI - Building AI-Ready APIs](#chapter-12-fastapi--building-ai-ready-apis)
- [Chapter 13: Databases with SQLAlchemy](#chapter-13-databases-with-sqlalchemy)
- [Chapter 14: Authentication & Security](#chapter-14-authentication--security)
- [Chapter 15: Testing with Pytest](#chapter-15-testing-with-pytest)

### Part IV: AI & ML Python
- [Chapter 16: Working with AI APIs](#chapter-16-working-with-ai-apis)
- [Chapter 17: Data Pipelines for AI](#chapter-17-data-pipelines-for-ai)
- [Chapter 18: PyTorch Essentials](#chapter-18-pytorch-essentials)

### Part V: Production Python
- [Chapter 19: Docker & Deployment](#chapter-19-docker--deployment)
- [Chapter 20: Logging, Monitoring & Best Practices](#chapter-20-logging-monitoring--best-practices)
- [Chapter 21: Build Projects & Next Steps](#chapter-21-build-projects--next-steps)

### Appendices
- [Appendix A: JavaScript to Python Cheat Sheet](#appendix-a-javascript-to-python-cheat-sheet)
- [Appendix B: Common Python Gotchas](#appendix-b-common-python-gotchas)
- [Appendix C: Your Stream-Segmenter Codebase Map](#appendix-c-your-stream-segmenter-codebase-map)

---

# PART I: PYTHON FOUNDATIONS

---

## Chapter 1: Python Basics for JS Developers

**Timeline: Days 1-2**

You know JavaScript. Python is simpler in many ways. This chapter maps every JS concept to Python so you can start reading Python code immediately.

### 1.1 Variables & Data Types

In JavaScript, you write `let`, `const`, or `var`. In Python, you just write the name:

```python
# Numbers
age = 25           # int (no 'let' or 'const' needed)
price = 19.99      # float
big = 10**100      # Python handles arbitrarily large ints!

# Strings
name = 'Alice'
greeting = "Hello"
multiline = '''This is a
multi-line string'''

# f-strings (like JS template literals `${}`)
message = f'Hi {name}, you are {age} years old'

# Booleans (Capital T and F!)
is_active = True    # not 'true'
is_admin = False    # not 'false'

# None (like null in JS)
result = None       # not 'null'

# Type checking
print(type(age))             # <class 'int'>
print(isinstance(age, int))  # True
```

**JS vs Python comparison:**

| Concept | JavaScript | Python |
|---------|-----------|--------|
| Integer | `let x = 5` | `x = 5` |
| Float | `let x = 5.0` | `x = 5.0` |
| String | `let s = "hi"` | `s = "hi"` |
| Template string | `` `Hi ${name}` `` | `f'Hi {name}'` |
| Boolean | `true` / `false` | `True` / `False` |
| Null | `null` | `None` |
| Type check | `typeof x` | `type(x)` |
| Constant | `const X = 5` | `X = 5` (convention: UPPERCASE) |

**Key difference:** Python has no `let`/`const`/`var`. Variables are created when you assign to them. There is no block scoping like JS - Python uses function scoping.

### 1.2 Control Flow

Python uses **indentation** instead of curly braces. This is the biggest syntax difference from JS:

```python
# If/elif/else (no braces! indentation matters!)
score = 85

if score >= 90:
    grade = 'A'
elif score >= 80:       # 'elif' not 'else if'
    grade = 'B'
else:
    grade = 'C'

# Ternary (one-line if)
# JS:     const status = score >= 60 ? 'pass' : 'fail'
# Python:
status = 'pass' if score >= 60 else 'fail'

# For loops
for i in range(5):          # 0, 1, 2, 3, 4
    print(i)

# Loop over a list (like JS for...of)
fruits = ['apple', 'banana', 'cherry']
for fruit in fruits:
    print(fruit)

# Loop with index (like JS forEach with index)
for i, fruit in enumerate(fruits):
    print(f'{i}: {fruit}')

# While loop
count = 0
while count < 5:
    print(count)
    count += 1              # No count++ in Python!

# Loop controls
for i in range(10):
    if i == 3:
        continue            # Skip this iteration
    if i == 7:
        break               # Exit the loop
    print(i)
```

**Indentation rules:**
- Use 4 spaces per level (not tabs)
- Everything inside a block must be indented the same amount
- The block ends when indentation goes back

```python
# This is WRONG (will error):
if True:
print("hello")      # IndentationError!

# This is RIGHT:
if True:
    print("hello")   # 4 spaces indent
```

### 1.3 List Comprehensions (Python Superpower)

This is one of Python's best features. It replaces `map()`, `filter()`, and loops with clean one-liners:

```python
numbers = [1, 2, 3, 4, 5]

# JS: const doubled = numbers.map(x => x * 2)
doubled = [x * 2 for x in numbers]       # [2, 4, 6, 8, 10]

# JS: const evens = numbers.filter(x => x % 2 === 0)
evens = [x for x in numbers if x % 2 == 0]  # [2, 4]

# Combine map + filter in one line
result = [x * 2 for x in numbers if x > 3]  # [8, 10]

# Dict comprehension (create a dictionary)
squares = {x: x**2 for x in range(6)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Nested (like flatMap)
matrix = [[1, 2], [3, 4], [5, 6]]
flat = [x for row in matrix for x in row]  # [1, 2, 3, 4, 5, 6]
```

**How to read a list comprehension:**

```python
# Read it like English:
[x * 2 for x in numbers if x > 3]
# "Give me x*2, for each x in numbers, if x is greater than 3"

# The pattern is always:
# [EXPRESSION for VARIABLE in ITERABLE if CONDITION]
```

**In your stream-segmenter code**, you'll see this pattern everywhere:

```python
# From segment.py - convert Word objects to dicts
[w.to_dict() for w in self.words]

# From segment.py - filter empty words
[w for w in self.words if w.text.strip()]
```

### 1.4 String Formatting

f-strings are the modern way to format strings in Python:

```python
name = 'Alice'
age = 30

# f-strings (PREFERRED - always use these)
print(f'Name: {name}, Age: {age}')
print(f'Next year: {age + 1}')         # Expressions work!
print(f'Price: ${19.99:.2f}')          # Format: $19.99
print(f'{1000000:,}')                  # Format: 1,000,000
print(f'{"hello":>20}')               # Right-align in 20 chars
print(f'{"hello":<20}')               # Left-align in 20 chars
print(f'{"hello":^20}')               # Center in 20 chars

# Multi-line f-string
message = f"""
Hello {name},
You are {age} years old.
Next year you will be {age + 1}.
"""

# Common string methods
text = "  Hello World  "
text.strip()          # "Hello World" (remove whitespace)
text.lower()          # "  hello world  "
text.upper()          # "  HELLO WORLD  "
text.split()          # ["Hello", "World"]
text.replace("World", "Python")  # "  Hello Python  "
"hello" in text       # True (membership check)
```

### Claude Code Prompt for Chapter 1

> "I'm a JavaScript developer learning Python. Show me how common JS patterns translate to Python: variable declaration, loops, array methods (map/filter/reduce), object manipulation, string formatting, and conditional logic. Give me side-by-side examples."

### Chapter 1 Mini-Project

Build a CLI tool that:
1. Takes a list of numbers as input
2. Prints: count, sum, average, min, max
3. Prints only the even numbers
4. Prints numbers formatted as a comma-separated string

Use only what you learned in this chapter (variables, loops, list comprehensions, f-strings).

```python
# starter code
numbers = [23, 45, 12, 67, 34, 89, 11, 56, 78, 90]

# Your code here:
# 1. Print count, sum, average, min, max
# 2. Use list comprehension to get evens
# 3. Format output with f-strings
```

---

## Chapter 2: Data Structures Deep Dive

**Timeline: Days 3-4**

Python's built-in data structures are more powerful than JavaScript's. Master these and you'll write cleaner code.

### 2.1 Lists (like JS Arrays)

```python
fruits = ['apple', 'banana', 'cherry']

# Negative indexing (no JS equivalent!)
print(fruits[-1])   # 'cherry' (last element)
print(fruits[-2])   # 'banana' (second to last)

# Slicing (HUGE in Python, no JS equivalent)
nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(nums[2:5])    # [2, 3, 4]       start:stop (stop excluded)
print(nums[:3])     # [0, 1, 2]       first 3
print(nums[7:])     # [7, 8, 9]       from index 7
print(nums[::2])    # [0, 2, 4, 6, 8] every 2nd element
print(nums[::-1])   # [9, 8, ..., 0]  reversed!

# Modifying lists
fruits.append('date')           # Add to end
fruits.extend(['fig', 'grape']) # Add multiple items
fruits.insert(1, 'avocado')    # Insert at index 1
fruits.remove('banana')         # Remove by value
popped = fruits.pop()           # Remove & return last item
popped = fruits.pop(0)          # Remove & return first item

# Membership check (very readable!)
print('apple' in fruits)        # True
print('mango' not in fruits)    # True

# Sorting
nums = [3, 1, 4, 1, 5, 9]
nums.sort()                     # Sort in-place: [1, 1, 3, 4, 5, 9]
sorted_nums = sorted(nums)     # Returns new sorted list (original unchanged)
nums.sort(reverse=True)        # Descending: [9, 5, 4, 3, 1, 1]

# Useful built-ins
len(fruits)                     # Number of items
min(nums)                       # Smallest
max(nums)                       # Largest
sum(nums)                       # Sum of all
```

### 2.2 Dictionaries (like JS Objects)

Dictionaries are key-value pairs. They are THE most used data structure in Python:

```python
user = {'name': 'Alice', 'age': 30, 'skills': ['python', 'ml']}

# Access values
print(user['name'])              # 'Alice'
print(user.get('email'))         # None (no error!)
print(user.get('email', 'N/A')) # 'N/A' (with default)

# Modify
user['email'] = 'alice@test.com'  # Add new key
user['age'] = 31                  # Update existing
del user['skills']                # Delete a key

# Check if key exists
if 'name' in user:
    print("Name exists!")

# Iterating
for key in user:
    print(key)                    # Just keys

for key, value in user.items():   # Keys AND values
    print(f'{key}: {value}')

for value in user.values():       # Just values
    print(value)

# Dict comprehension
scores = {'alice': 85, 'bob': 92, 'charlie': 78}
passed = {k: v for k, v in scores.items() if v >= 80}
# {'alice': 85, 'bob': 92}

# Merging dicts (Python 3.9+)
defaults = {'color': 'blue', 'size': 'M'}
overrides = {'size': 'L', 'style': 'bold'}
merged = defaults | overrides
# {'color': 'blue', 'size': 'L', 'style': 'bold'}

# Merging dicts (Python 3.5+)
merged = {**defaults, **overrides}  # Same result
```

**In your stream-segmenter**, dicts are used everywhere for JSON serialization:

```python
# From segment.py - convert object to dict for JSON
def to_dict(self):
    return {
        'id': self.id,
        'file_path': str(self.file_path),
        'status': self.status,
    }
```

### 2.3 Tuples, Sets & Collections

```python
# TUPLES: Immutable lists (cannot be changed after creation)
point = (3, 4)
x, y = point           # Unpacking: x=3, y=4

# Multiple return values use tuples
def get_name_age():
    return 'Alice', 30  # Returns a tuple

name, age = get_name_age()  # Unpack it

# Star unpacking
first, *rest = [1, 2, 3, 4, 5]  # first=1, rest=[2,3,4,5]
first, *middle, last = [1, 2, 3, 4, 5]  # first=1, middle=[2,3,4], last=5

# SETS: Unique items, super fast membership check
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a & b)   # Intersection: {3, 4}
print(a | b)   # Union: {1, 2, 3, 4, 5, 6}
print(a - b)   # Difference: {1, 2}
print(3 in a)  # True (O(1) speed - instant!)

# IMPORTANT: {} creates an empty DICT, not a set
empty_set = set()    # This is an empty set
empty_dict = {}      # This is an empty dict

# COUNTER: Count occurrences automatically
from collections import Counter

words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple']
counts = Counter(words)
print(counts)                  # Counter({'apple': 3, 'banana': 2, 'cherry': 1})
print(counts.most_common(2))   # [('apple', 3), ('banana', 2)]

# DEFAULTDICT: Dict that auto-creates missing values
from collections import defaultdict

# Without defaultdict (annoying):
groups = {}
for name, dept in [('Alice','eng'), ('Bob','eng'), ('Charlie','sales')]:
    if dept not in groups:
        groups[dept] = []      # Must check and create manually
    groups[dept].append(name)

# With defaultdict (clean!):
groups = defaultdict(list)     # Missing keys auto-create empty list
for name, dept in [('Alice','eng'), ('Bob','eng'), ('Charlie','sales')]:
    groups[dept].append(name)  # Just works!
# {'eng': ['Alice', 'Bob'], 'sales': ['Charlie']}
```

### 2.4 Deque: Rolling Buffers & Queues

```python
from collections import deque

# Fixed-size rolling buffer (your stream-segmenter uses this!)
buffer = deque(maxlen=3)

buffer.append(1)  # [1]
buffer.append(2)  # [1, 2]
buffer.append(3)  # [1, 2, 3]
buffer.append(4)  # [2, 3, 4] -- oldest (1) auto-removed!
buffer.append(5)  # [3, 4, 5] -- oldest (2) auto-removed!

# Convert to list when needed
items = list(buffer)  # [3, 4, 5]

# Fast operations on both ends
buffer.appendleft(0)   # Add to front
buffer.popleft()        # Remove from front
```

**Why deque matters in your project:** Your stream-segmenter keeps a rolling buffer of the last N minutes of transcripts. Deque handles this automatically:

```python
# From your system's design:
transcript_buffer = deque(maxlen=20)  # Last 20 segments = ~10 minutes

# Every time a new transcript arrives:
transcript_buffer.append(new_transcript)
# Old ones automatically removed when buffer is full!
```

### 2.5 Real Examples from Your Stream-Segmenter

Everything above was theory. Now let's see how lists and dicts are actually used in your production code. These are real lines from your project.

#### Lists in Your Codebase

**1. Creating an empty list with `field(default_factory=list)`** - [segment.py:41](src/models/segment.py#L41)

```python
# Each Transcript has a list of Word objects
words: List[Word] = field(default_factory=list)
```

Why `default_factory=list` and not just `= []`? Because `= []` would share ONE list between ALL Transcript instances. `default_factory=list` creates a fresh empty list for each new Transcript. (See Appendix B, Gotcha #1.)

---

**2. List comprehension to convert objects** - [segment.py:141](src/models/segment.py#L141)

```python
# Convert every Word object to a dictionary (for JSON)
'words': [w.to_dict() for w in self.words]
```

Read it like English: *"Give me `w.to_dict()` for each `w` in `self.words`"*. This takes a list of Word objects and produces a list of dictionaries. One line replaces a 4-line for loop.

---

**3. List comprehension to build objects from database rows** - [state_manager.py:216-228](src/services/state_manager.py#L216-L228)

```python
# Convert database rows into Segment objects
return [
    Segment(
        id=row['id'],
        file_path=Path(row['file_path']),
        start_time=row['start_time'],
        end_time=row['end_time'],
        duration=row['duration'],
        status=row['status'],
        created_at=datetime.fromisoformat(row['created_at']),
        file_size=row['file_size']
    )
    for row in rows
]
```

Each database `row` becomes one `Segment` object. The result is a list of Segments. This pattern appears everywhere: database rows -> Python objects.

---

**4. Append to list + enumerate for numbering** - [context_manager.py:65-66](src/services/context_manager.py#L65-L66)

```python
# Build numbered topic list: "1. Sports", "2. Weather", etc.
for i, topic in enumerate(self.example_topics, 1):
    parts.append(f"{i}. {topic}")
```

`enumerate(list, 1)` gives you `(index, item)` pairs starting at 1. `.append()` adds to the end of the list.

---

**5. Sort a list with custom key** - [layer_states.py:252-253](src/services/layer_states.py#L252-L253)

```python
# Sort processed ranges by their start time
self.processed_ranges.sort(key=lambda r: r[0])
```

`.sort()` modifies the list in-place. `key=lambda r: r[0]` means "sort by the first element of each tuple". So `[(30, 60), (0, 30)]` becomes `[(0, 30), (30, 60)]`.

---

**6. Filter a list (remove specific items)** - [layer_states.py:792-795](src/services/layer_states.py#L792-L795)

```python
# Remove subtopics that have been grouped into topics
self.ungrouped_subtopic_ids = [
    sid for sid in self.ungrouped_subtopic_ids
    if sid not in grouped_subtopic_ids
]
```

This creates a NEW list containing only items that are NOT in `grouped_subtopic_ids`. It's the Python way of saying "remove these items from the list".

---

#### Dictionaries in Your Codebase

**1. Creating a dict to serialize an object to JSON** - [segment.py:74-83](src/models/segment.py#L74-L83)

```python
def to_dict(self) -> Dict[str, Any]:
    return {
        'id': self.id,
        'file_path': str(self.file_path),   # Path -> string for JSON
        'start_time': self.start_time,
        'end_time': self.end_time,
        'duration': self.duration,
        'status': self.status,
        'created_at': self.created_at.isoformat(),  # datetime -> string
        'file_size': self.file_size
    }
```

This is THE most important dict pattern in your project. It converts a Python object into a dictionary that can be saved as JSON. Notice how `Path` and `datetime` must be converted to strings because JSON doesn't understand Python objects.

---

**2. Safe nested access with chained `.get()`** - [main.py:357-358](src/main.py#L357-L358)

```python
# Safely dig into nested config without crashing
self.gemini_api_key = config.get('apis', {}).get('gemini', {}).get('api_key', '')
```

Without `.get()`, if `'apis'` key is missing: `config['apis']` crashes with KeyError. With `.get('apis', {})`, it returns an empty dict instead, and the chain continues safely. The final `''` is the default if nothing is found.

---

**3. Dict `.update()` to merge multiple values at once** - [server.py:1309-1318](src/api/server.py#L1309-L1318)

```python
# Update pipeline state with all new values at once
pipeline_state.update({
    "running": True,
    "source_type": "youtube",
    "preset_id": config.preset_id,
    "stream_url": stream_url,
    "started_at": datetime.now().isoformat(),
})
```

`.update()` adds or overwrites multiple key-value pairs at once. Much cleaner than setting each one separately.

---

**4. Dict comprehension from database results** - [state_manager.py:583](src/services/state_manager.py#L583)

```python
# Count segments by status: {"pending": 5, "processed": 10, ...}
segments_by_status = {row['status']: row['count'] for row in cursor.fetchall()}
```

This creates a dictionary in one line from database query results. Each row's `status` becomes a key, and `count` becomes the value.

---

**5. Dict `.pop()` to remove a key** - [segment.py:241](src/models/segment.py#L241)

```python
# Remove unwanted key before creating object
data.pop('segment_count', None)
```

`.pop('key', default)` removes a key from the dict and returns its value. If the key doesn't exist, returns `None` (the default) instead of crashing. Useful for cleaning data before passing to a constructor.

---

**6. Unpack dict as function arguments with `**`** - [segment.py:91](src/models/segment.py#L91)

```python
return cls(**data)
```

The `**` operator unpacks a dictionary into keyword arguments. If `data = {'text': 'hello', 'start': 1.0, 'end': 2.0}`, then `cls(**data)` becomes `cls(text='hello', start=1.0, end=2.0)`. This is how `from_dict()` works everywhere in your project.

---

**7. Complex nested dict (JSON template)** - [server.py:1481-1503](src/api/server.py#L1481-L1503)

```python
# Template for output JSON files
file_structures = {
    "sentences.json": {
        "stream_start": datetime.now().isoformat(),
        "stream_title": "Stream",
        "last_updated": datetime.now().isoformat(),
        "sentences": [],          # List inside a dict!
        "total_sentences": 0
    },
    "subtopics.json": {
        "stream_start": datetime.now().isoformat(),
        "subtopics": [],
        "total_subtopics": 0
    }
}
```

Dicts can contain other dicts, lists, strings, numbers - any type. This creates a template for the JSON files your system outputs.

---

**8. Dict `.setdefault()` - create key if missing** - [main.py:120-121](src/main.py#L120-L121)

```python
# If 'stream' key doesn't exist, create it as empty dict, then set youtube_url
result.setdefault('stream', {})['youtube_url'] = stream_url
```

`.setdefault('stream', {})` returns the value for `'stream'` if it exists, or creates it with `{}` if it doesn't. Then `['youtube_url'] = stream_url` sets a key inside that nested dict. One line does what would take 3 lines with `if/else`.

---

### When to Use Each

| Structure | Use When | Speed |
|-----------|----------|-------|
| **List** | Ordered items, may change | Index: fast. Search: slow |
| **Dict** | Key-value lookup | Lookup by key: fast |
| **Set** | Unique items, "is X in here?" | Membership check: fast |
| **Tuple** | Immutable data, dict keys, return values | Same as list |
| **Deque** | Rolling window, queue | Add/remove from ends: fast |
| **Counter** | Count occurrences | Same as dict |
| **defaultdict** | Group items, avoid KeyError | Same as dict |

### Claude Code Prompt for Chapter 2

> "Find all places in my project where I use dicts or lists. Show where a set would be faster for membership checks, where Counter would be cleaner for counting, or where defaultdict would simplify grouping code."

### Chapter 2 Mini-Project

Build a word frequency analyzer:
1. Read a text file (or use a sample string)
2. Count word frequencies using `Counter`
3. Find the top 20 most common words
4. Group words by their length using `defaultdict`
5. Output a formatted report

```python
# starter code
from collections import Counter, defaultdict

text = """Python is a great language. Python is used for AI.
AI is the future. Python and AI go together. The future
of programming is AI and Python together."""

# Your code here:
# 1. Split into words, lowercase, strip punctuation
# 2. Count with Counter
# 3. Group by length with defaultdict
# 4. Print formatted report
```

---

## Chapter 3: Functions, Closures & Decorators

**Timeline: Days 5-7**

Functions are first-class objects in Python (just like JS). But Python adds **decorators**, **generators**, and **context managers** - three features you'll use every day.

### 3.1 Function Basics & *args/**kwargs

```python
# Basic function
def greet(name, greeting='Hello'):
    """This is a docstring - explains what the function does."""
    return f'{greeting}, {name}!'

greet('Alice')                    # 'Hello, Alice!'
greet('Alice', greeting='Hey')    # 'Hey, Alice!'

# Multiple return values (using tuples)
def get_stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

low, high, avg = get_stats([1, 2, 3, 4, 5])

# *args: Accept any number of positional arguments
def add_all(*args):
    return sum(args)

add_all(1, 2, 3)        # 6
add_all(1, 2, 3, 4, 5)  # 15

# **kwargs: Accept any number of keyword arguments
def create_user(**kwargs):
    print(f'Creating user with: {kwargs}')

create_user(name='Alice', age=30, role='engineer')
# Creating user with: {'name': 'Alice', 'age': 30, 'role': 'engineer'}

# Unpacking (like JS spread operator)
nums = [1, 2, 3]
def add(a, b, c):
    return a + b + c

add(*nums)  # Same as add(1, 2, 3) = 6

config = {'name': 'Alice', 'age': 30}
def show(name, age):
    print(f'{name} is {age}')

show(**config)  # Same as show(name='Alice', age=30)
```

**Lambda functions** (like arrow functions, but limited to one expression):

```python
# JS:     const double = x => x * 2
# Python:
double = lambda x: x * 2

# Most useful for sorting and filtering
users = [{'name': 'Charlie'}, {'name': 'Alice'}, {'name': 'Bob'}]
users.sort(key=lambda u: u['name'])
# Sorted by name: Alice, Bob, Charlie
```

### 3.2 Decorators (Python Superpower)

A decorator is a function that wraps another function to add behavior. Think of it as middleware for functions:

```python
import time
import functools

# Step 1: Understand the concept
# A decorator takes a function, wraps it, returns the wrapper

def timer(func):
    """Decorator that prints how long a function takes."""
    @functools.wraps(func)  # Preserves original function name
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)  # Call the original function
        elapsed = time.time() - start
        print(f'{func.__name__} took {elapsed:.2f}s')
        return result
    return wrapper

# Step 2: Use it with @ syntax
@timer
def slow_function():
    time.sleep(1)
    return 'done'

slow_function()  # Prints: slow_function took 1.00s
```

**How to read `@timer`:**

```python
# This:
@timer
def slow_function():
    pass

# Is exactly the same as this:
def slow_function():
    pass
slow_function = timer(slow_function)
```

**Decorator with arguments** (more advanced):

```python
def retry(max_attempts=3, delay=1):
    """Decorator that retries a function on failure."""
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise  # Give up on last attempt
                    print(f'Attempt {attempt + 1} failed: {e}')
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, delay=2)
def call_api(url):
    # If this fails, it will retry up to 3 times
    pass
```

**Built-in useful decorators:**

```python
from functools import lru_cache

# Cache results (don't re-compute for same input)
@lru_cache(maxsize=128)
def expensive_computation(n):
    print(f'Computing for {n}...')
    return sum(i**2 for i in range(n))

expensive_computation(1000)  # Prints "Computing..." and calculates
expensive_computation(1000)  # Returns cached result instantly!
```

### 3.3 Generators & Context Managers

**Generators** produce values one at a time instead of building the whole list in memory:

```python
# Regular function: builds entire list in memory
def get_squares(n):
    result = []
    for i in range(n):
        result.append(i ** 2)
    return result  # All n items in memory at once!

# Generator: yields one value at a time (memory efficient)
def get_squares_gen(n):
    for i in range(n):
        yield i ** 2  # Produces one value, pauses, resumes

# For 1 million numbers, the generator uses almost no memory
for square in get_squares_gen(1_000_000):
    if square > 100:
        break  # Stop early - no wasted work!

# Generator expression (like list comprehension but lazy)
squares = (x**2 for x in range(1_000_000))  # No memory used yet!
first_ten = [next(squares) for _ in range(10)]  # Only computes 10
```

**Real use case:** Reading a huge file line by line without loading it all:

```python
def read_large_file(path):
    with open(path) as f:
        for line in f:
            yield line.strip()

# Process a 10GB file without running out of memory
for line in read_large_file('huge_dataset.jsonl'):
    process(line)
```

**Context managers** (`with` statement) ensure cleanup happens automatically:

```python
# Files auto-close when the 'with' block ends
with open('data.txt') as f:
    content = f.read()
# f is automatically closed here, even if an error occurred!

# Without 'with' (don't do this):
f = open('data.txt')
content = f.read()
f.close()  # What if an error happens before this line? File never closes!
```

**Make your own context manager:**

```python
from contextlib import contextmanager

@contextmanager
def timer_context(label):
    """Times whatever code runs inside the 'with' block."""
    start = time.time()
    yield                    # Code inside 'with' block runs here
    elapsed = time.time() - start
    print(f'{label}: {elapsed:.2f}s')

# Usage:
with timer_context('API call'):
    result = call_api()      # This gets timed
# Prints: API call: 1.23s
```

### Claude Code Prompt for Chapter 3

> "Build a utility module with these decorators: @timer, @retry(max_attempts, delay), @cache_result, @log_calls. Include type hints and docstrings. Then show me where decorators are used in my stream-segmenter project."

### Chapter 3 Mini-Project

Build an API client class with retry, caching, and timing decorators:
1. Create a `@timer` decorator that logs function execution time
2. Create a `@retry(max_attempts=3)` decorator
3. Create a `@cache_result` decorator using a dict
4. Build a fake API client that uses all three decorators
5. Test with httpbin.org or simulated failures

---

## Chapter 4: Object-Oriented Python

**Timeline: Days 7-9**

Classes bundle related data and functions together. You'll use them for models, services, and config in AI apps.

### 4.1 Classes & Dataclasses

```python
# REGULAR CLASS
class User:
    def __init__(self, name, email, role='user'):
        """__init__ is the constructor (like JS constructor)."""
        self.name = name       # 'self' is like 'this' in JS
        self.email = email
        self.role = role

    def __repr__(self):
        """String representation for debugging."""
        return f'User(name={self.name!r}, role={self.role!r})'

    def greet(self):
        """A method (function inside a class)."""
        return f'Hi, I am {self.name}'

# Create an instance
alice = User('Alice', 'alice@example.com', role='admin')
print(alice.name)     # 'Alice'
print(alice.greet())  # 'Hi, I am Alice'
print(alice)          # User(name='Alice', role='admin')
```

**DATACLASSES** are the modern, cleaner way to write data containers:

```python
from dataclasses import dataclass, field
from typing import Optional, List

# Without dataclass (verbose):
class OldUser:
    def __init__(self, name, email, role='user'):
        self.name = name
        self.email = email
        self.role = role
    def __repr__(self):
        return f'OldUser(name={self.name}, email={self.email})'
    def __eq__(self, other):
        return self.name == other.name and self.email == other.email

# With dataclass (clean!):
@dataclass
class User:
    name: str
    email: str
    role: str = 'user'                          # Default value
    tags: List[str] = field(default_factory=list)  # Mutable default

# __init__, __repr__, __eq__ are all auto-generated!
alice = User(name='Alice', email='alice@test.com')
print(alice)  # User(name='Alice', email='alice@test.com', role='user', tags=[])
```

**In your stream-segmenter**, dataclasses are used for all data models:

```python
# From segment.py - this is a real dataclass in your project
@dataclass
class Word:
    text: str               # The word text
    start: float            # Start time in seconds
    end: float              # End time in seconds
    confidence: float = 1.0 # Default confidence

# Usage:
word = Word(text="hello", start=1.5, end=2.0)
word = Word("hello", 1.5, 2.0, 0.95)  # Positional args also work
```

**Why `field(default_factory=list)` instead of just `= []`?**

```python
# WRONG - all instances share the SAME list!
@dataclass
class Bad:
    items: list = []  # This list is shared!

a = Bad()
b = Bad()
a.items.append(1)
print(b.items)  # [1] -- b's list changed too!

# RIGHT - each instance gets its OWN list
@dataclass
class Good:
    items: list = field(default_factory=list)  # New list per instance

a = Good()
b = Good()
a.items.append(1)
print(b.items)  # [] -- b is unaffected
```

### 4.2 Inheritance, Properties & Dunder Methods

**Properties** let you compute values that look like attributes:

```python
@dataclass
class Segment:
    start_time: float
    end_time: float
    file_path: str

    @property
    def duration(self):
        """Computed property - looks like an attribute, runs code."""
        return self.end_time - self.start_time

seg = Segment(start_time=0.0, end_time=30.0, file_path='seg_001.ts')
print(seg.duration)  # 30.0 (looks like an attribute, but computed!)
```

**Dunder (double underscore) methods** customize how your objects behave:

```python
class Dataset:
    def __init__(self, items):
        self._items = list(items)

    def __len__(self):
        """len(dataset) works."""
        return len(self._items)

    def __getitem__(self, idx):
        """dataset[0] and dataset[1:3] work."""
        return self._items[idx]

    def __iter__(self):
        """for item in dataset: works."""
        return iter(self._items)

    def __repr__(self):
        """print(dataset) shows useful info."""
        return f'Dataset({len(self)} items)'

    def __contains__(self, item):
        """'x' in dataset works."""
        return item in self._items

ds = Dataset([1, 2, 3, 4, 5])
print(len(ds))      # 5
print(ds[0])         # 1
print(ds[1:3])       # [2, 3]
print(3 in ds)       # True
for item in ds:
    print(item)      # Iterable!
```

### 4.3 Class Methods and Static Methods

```python
@dataclass
class Word:
    text: str
    start: float
    end: float

    def to_dict(self):
        """Instance method: uses self (the specific instance)."""
        return {'text': self.text, 'start': self.start, 'end': self.end}

    @classmethod
    def from_dict(cls, data):
        """Class method: receives the CLASS, not an instance.
        Used as a factory - alternative way to create instances."""
        return cls(text=data['text'], start=data['start'], end=data['end'])

    @staticmethod
    def validate_time(t):
        """Static method: no self, no cls. Just a utility function
        that logically belongs to this class."""
        return t >= 0

# Usage:
word = Word("hello", 1.0, 2.0)           # Normal creation
word_dict = word.to_dict()                # Instance method
word2 = Word.from_dict(word_dict)         # Class method (factory)
valid = Word.validate_time(1.5)           # Static method (utility)
```

**In your stream-segmenter**, the `from_dict` / `to_dict` pattern is used everywhere for JSON serialization:

```python
# Save to JSON:  object -> dict -> JSON string
json_str = json.dumps(word.to_dict())

# Load from JSON: JSON string -> dict -> object
data = json.loads(json_str)
word = Word.from_dict(data)
```

### 4.4 Private Methods (Convention)

```python
class StateManager:
    def __init__(self, db_path):
        self.db_path = db_path
        self._cache = {}         # _ prefix = "private" (convention)
        self.__secret = 'hidden' # __ prefix = "really private" (name mangled)

    def get_segment(self, id):
        """PUBLIC method - safe to call from outside."""
        return self._query_db(id)

    def _query_db(self, id):
        """PRIVATE method - internal use only (convention)."""
        pass

    def _init_database(self):
        """PRIVATE method - called by __init__ only."""
        pass
```

**Note:** Python doesn't enforce privacy. The `_` prefix is just a convention that says "don't use this from outside the class."

### Claude Code Prompt for Chapter 4

> "Build an AI config system using dataclasses: ModelConfig, RAGConfig, APIConfig. Add validation, JSON serialization (to_dict/from_dict), and a ConfigManager that loads from files. Then show me all the dataclasses in my stream-segmenter project."

### Chapter 4 Mini-Project

Build a complete data model system:
1. Create `Word`, `Transcript`, and `Topic` dataclasses
2. Add `to_dict()` and `from_dict()` methods to each
3. Add `@property` for computed values (duration, word_count)
4. Write a round-trip test: create object -> to_dict -> from_dict -> compare

---

## Chapter 5: Error Handling & Debugging

**Timeline: Days 9-10**

In production AI systems, things fail constantly: APIs timeout, files are missing, networks drop. Your code must handle every failure gracefully.

### 5.1 try/except/finally

```python
# Basic pattern
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Can't divide by zero!")

# Catch multiple exception types
try:
    data = json.loads(bad_string)
except json.JSONDecodeError:
    print("Invalid JSON!")
except TypeError:
    print("Wrong type!")
except Exception as e:          # Catch-all (use as last resort)
    print(f"Unexpected error: {e}")

# finally: runs WHETHER OR NOT an error occurred
try:
    f = open('data.txt')
    content = f.read()
except FileNotFoundError:
    print("File not found!")
finally:
    f.close()  # Always runs - cleanup is guaranteed

# Get the error details
try:
    risky_operation()
except ValueError as e:
    print(f"Error type: {type(e).__name__}")
    print(f"Error message: {e}")
    # For full stack trace in logs:
    import traceback
    traceback.print_exc()
```

### 5.2 Custom Exceptions

```python
# Define your own exception types
class APIError(Exception):
    """Base exception for API errors."""
    pass

class APITimeoutError(APIError):
    """API call timed out."""
    pass

class APIRateLimitError(APIError):
    """Too many requests."""
    def __init__(self, retry_after=60):
        self.retry_after = retry_after
        super().__init__(f"Rate limited. Retry after {retry_after}s")

# Raise them
def call_api(url):
    response = requests.get(url, timeout=10)
    if response.status_code == 429:
        raise APIRateLimitError(retry_after=60)
    if response.status_code == 504:
        raise APITimeoutError("Gateway timeout")

# Catch them
try:
    call_api("https://api.example.com")
except APIRateLimitError as e:
    print(f"Rate limited! Wait {e.retry_after}s")
    time.sleep(e.retry_after)
except APITimeoutError:
    print("Timeout - retrying...")
except APIError as e:
    print(f"API error: {e}")
```

### 5.3 Logging (NOT print!)

Professional code uses `logging`, not `print()`. Logging gives you levels, timestamps, file output, and filtering:

```python
import logging

# Setup logging (do this ONCE, usually in main.py)
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s | %(name)s | %(levelname)s | %(message)s',
    handlers=[
        logging.FileHandler('logs/app.log'),  # Save to file
        logging.StreamHandler()                # Also print to console
    ]
)

# Get a logger for this file
logger = logging.getLogger(__name__)

# Use different levels
logger.debug("Detailed info for debugging")    # Hidden at INFO level
logger.info("Segment 42 processed")            # Normal operations
logger.warning("Disk space low: 2GB remaining") # Attention needed
logger.error("API call failed: timeout")        # Something broke
logger.critical("Database corrupted!")          # System is down

# Include context (MUCH better than plain messages)
logger.info(f"Transcribed segment {seg_id}: {word_count} words in {elapsed:.2f}s")
logger.error(f"FFmpeg failed for {file_path}: exit code {return_code}")

# Log with stack trace (for exceptions)
try:
    risky_operation()
except Exception as e:
    logger.error(f"Operation failed: {e}", exc_info=True)
    # exc_info=True includes the full stack trace in the log!
```

**Log levels from least to most severe:**

```
DEBUG    → Detailed diagnostic info (hidden in production)
INFO     → Confirmation that things are working
WARNING  → Something unexpected but system continues
ERROR    → Something failed
CRITICAL → System cannot continue
```

### 5.4 Retry Pattern

APIs fail. Networks drop. Your system must retry:

```python
import time

def call_with_retry(func, max_retries=3, delay=2):
    """Call a function, retrying on failure with exponential backoff."""
    for attempt in range(max_retries):
        try:
            return func()
        except Exception as e:
            logger.warning(f"Attempt {attempt + 1}/{max_retries} failed: {e}")

            if attempt == max_retries - 1:  # Last attempt
                logger.error(f"All {max_retries} attempts failed")
                raise  # Give up

            # Exponential backoff: 2s, 4s, 8s...
            wait_time = delay * (2 ** attempt)
            logger.info(f"Retrying in {wait_time}s...")
            time.sleep(wait_time)
```

**In your stream-segmenter**, the Gemini LLM service uses exactly this pattern:

```python
# From gemini_llm_service.py
for attempt in range(self.max_retries):
    try:
        response = model.generate_content(prompt)
        return response.text
    except Exception as e:
        if attempt == self.max_retries - 1:
            raise
        wait_time = self.retry_delay * (2 ** attempt)
        time.sleep(wait_time)
```

### Claude Code Prompt for Chapter 5

> "Show me all the error handling in my stream-segmenter project. Find places where errors are silently swallowed (bare except or except Exception: pass). Show me the retry patterns used and suggest improvements."

### Chapter 5 Mini-Project

Build a resilient file processor:
1. Create custom exceptions: `FileProcessingError`, `InvalidFormatError`
2. Write a function that reads JSON files with proper error handling
3. Add retry logic for simulated API calls
4. Set up logging to both console and file
5. Process a directory of mixed files (some valid, some invalid) and log results

---

## Chapter 6: File I/O & Data Formats

**Timeline: Days 10-12**

Your system constantly reads and writes files: video segments, audio files, JSON transcripts, YAML configs, and more. This chapter covers all file operations you need.

### 6.1 pathlib: Modern File Paths

Never use string concatenation for file paths. Use `pathlib.Path`:

```python
from pathlib import Path

# Create a path (file doesn't need to exist)
path = Path("data/transcripts/transcript_001.json")

# Path properties
path.name        # "transcript_001.json"
path.stem        # "transcript_001" (name without extension)
path.suffix      # ".json"
path.parent      # Path("data/transcripts")
path.parts       # ('data', 'transcripts', 'transcript_001.json')

# Check if exists
path.exists()    # True or False
path.is_file()   # True if it's a file
path.is_dir()    # True if it's a directory

# Get file info
path.stat().st_size  # File size in bytes

# Join paths using / operator (works on ALL operating systems!)
data_dir = Path("data")
transcript_dir = data_dir / "transcripts"           # data/transcripts
file_path = transcript_dir / "transcript_001.json"  # data/transcripts/transcript_001.json

# WHY Path is better than strings:
# String approach (breaks on Windows!):
bad = "data" + "/" + "transcripts" + "/" + "file.json"

# Path approach (works everywhere!):
good = Path("data") / "transcripts" / "file.json"

# Create directories
Path("data/output").mkdir(parents=True, exist_ok=True)
# parents=True  → create parent dirs if needed
# exist_ok=True → don't error if already exists

# Find files
Path("data").glob("*.json")        # All JSON files in data/
Path("data").glob("**/*.json")     # All JSON files in data/ and subdirs
Path("src").glob("**/*.py")        # All Python files under src/

# Convert to string when needed (for external tools)
str(path)  # "data/transcripts/transcript_001.json"
```

### 6.2 Reading & Writing Files

Always use `with` statements (context managers) for file operations:

```python
from pathlib import Path

# READ a text file
path = Path("data/example.txt")
with open(path, 'r') as f:      # 'r' = read mode
    content = f.read()           # Read entire file as string

# Read line by line (memory efficient for large files)
with open(path, 'r') as f:
    for line in f:
        print(line.strip())      # strip() removes \n

# Read all lines into a list
with open(path, 'r') as f:
    lines = f.readlines()        # List of strings

# WRITE a text file
with open(path, 'w') as f:      # 'w' = write mode (overwrites!)
    f.write("Hello world\n")
    f.write("Second line\n")

# APPEND to a file
with open(path, 'a') as f:      # 'a' = append mode
    f.write("Added line\n")

# READ/WRITE binary files (for audio, video, images)
audio_path = Path("data/audio_001.wav")
with open(audio_path, 'rb') as f:    # 'rb' = read binary
    audio_data = f.read()

with open(audio_path, 'wb') as f:    # 'wb' = write binary
    f.write(audio_data)
```

### 6.3 JSON and JSONL

JSON is the most common data format in your system:

```python
import json
from pathlib import Path

# Python dict → JSON string
data = {'id': 1, 'text': 'Hello', 'score': 0.95}
json_string = json.dumps(data)           # '{"id": 1, "text": "Hello", ...}'
json_pretty = json.dumps(data, indent=2) # Formatted with indentation

# JSON string → Python dict
loaded = json.loads(json_string)         # Back to dict

# Write JSON to file
with open(Path("output.json"), 'w') as f:
    json.dump(data, f, indent=2)         # dump (no 's') writes to file

# Read JSON from file
with open(Path("output.json"), 'r') as f:
    data = json.load(f)                  # load (no 's') reads from file

# JSONL: One JSON object per line (great for large datasets)
records = [
    {'id': 1, 'text': 'First'},
    {'id': 2, 'text': 'Second'},
    {'id': 3, 'text': 'Third'},
]

# Write JSONL
with open(Path("data.jsonl"), 'w') as f:
    for record in records:
        f.write(json.dumps(record) + '\n')

# Read JSONL (memory efficient - one line at a time)
with open(Path("data.jsonl"), 'r') as f:
    for line in f:
        record = json.loads(line)
        print(record)
```

**The serialization pattern used throughout your project:**

```python
# Object → Dict → JSON string → File
word = Word("hello", 1.0, 2.0)
word_dict = word.to_dict()                # Object to dict
json_str = json.dumps(word_dict)          # Dict to JSON string
# Write json_str to file...

# File → JSON string → Dict → Object
# Read json_str from file...
word_dict = json.loads(json_str)          # JSON string to dict
word = Word.from_dict(word_dict)          # Dict to object
```

### 6.4 YAML for Configuration

YAML is more readable than JSON for config files:

```python
import yaml
from pathlib import Path

# Read YAML
with open(Path("config/settings.yaml"), 'r') as f:
    config = yaml.safe_load(f)  # Always use safe_load!

# Access nested values
youtube_url = config['stream']['youtube_url']
temperature = config['llm']['gemini']['temperature']

# Write YAML
config = {
    'stream': {'segment_duration': 30},
    'llm': {'temperature': 0.3}
}
with open(Path("config/settings.yaml"), 'w') as f:
    yaml.dump(config, f, default_flow_style=False)
```

### 6.5 Environment Variables

API keys and secrets should NEVER be in code or config files. Use environment variables:

```python
import os

# Read environment variable
api_key = os.getenv('GEMINI_API_KEY')              # Returns None if not set
api_key = os.getenv('GEMINI_API_KEY', 'default')   # With default value

# Validate required variables
if not os.getenv('GEMINI_API_KEY'):
    raise ValueError("GEMINI_API_KEY not set! Run: set GEMINI_API_KEY=your-key")

# Using python-dotenv (load from .env file)
# pip install python-dotenv
from dotenv import load_dotenv
load_dotenv()  # Loads from .env file in project root

api_key = os.getenv('GEMINI_API_KEY')  # Now available
```

**.env file** (add to `.gitignore`!):
```
GEMINI_API_KEY=sk-abc123...
ELEVENLABS_API_KEY=el-xyz789...
DEBUG=True
```

### Claude Code Prompt for Chapter 6

> "Show me all the file I/O in my stream-segmenter project. Where does it read/write JSON? Where does it use Path? Show me the config loading flow from settings.yaml to the running system."

### Chapter 6 Mini-Project

Build a data manager:
1. Create a `DataManager` class that handles JSON/JSONL file operations
2. Add methods: `save_json()`, `load_json()`, `append_jsonl()`, `read_jsonl()`
3. Load config from YAML with environment variable expansion
4. Handle all errors (missing files, invalid JSON, permission errors)

---

## Chapter 7: Modules, Packages & Project Structure

**Timeline: Days 12-13**

Understanding how Python projects are organized will help you navigate any codebase.

### 7.1 Python Project Structure

```
my_project/
|
|-- src/                    # Source code
|   |-- __init__.py         # Makes src/ a Python package
|   |-- main.py             # Entry point
|   |-- core/               # Core business logic
|   |   |-- __init__.py
|   |   |-- processor.py
|   |   +-- analyzer.py
|   |-- models/             # Data models
|   |   |-- __init__.py
|   |   +-- segment.py
|   |-- services/           # External service wrappers
|   |   |-- __init__.py
|   |   +-- api_client.py
|   +-- utils/              # Utility functions
|       |-- __init__.py
|       +-- helpers.py
|
|-- tests/                  # Tests mirror src/ structure
|   |-- test_processor.py
|   +-- test_analyzer.py
|
|-- config/                 # Configuration files
|   +-- settings.yaml
|
|-- data/                   # Data directory
|-- logs/                   # Log files
|-- requirements.txt        # Dependencies (pip)
|-- pyproject.toml          # Modern project config
+-- .env                    # Environment variables (in .gitignore!)
```

### 7.2 Imports

```python
# Standard library imports
import os
import json
from pathlib import Path
from datetime import datetime
from typing import List, Optional

# Third-party imports (installed with pip)
import yaml
import requests
from fastapi import FastAPI

# Local imports (from your own project)
from src.models.segment import Word, Transcript   # Absolute import
from ..models.segment import Word                  # Relative import (.. = parent)
from .helpers import format_time                   # Relative import (. = same dir)
```

**`__init__.py` explained:**

```python
# src/models/__init__.py
# This file makes the directory a "package" (importable)

# Empty __init__.py is fine! Just needs to exist.

# Or re-export commonly used classes:
from .segment import Word, Transcript, Segment
# Now users can do: from src.models import Word
# Instead of:       from src.models.segment import Word
```

### 7.3 Virtual Environments

Virtual environments keep project dependencies isolated:

```bash
# Create a virtual environment
python -m venv venv

# Activate it
# Windows:
venv\Scripts\activate.bat
# Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Save current dependencies
pip freeze > requirements.txt

# Deactivate when done
deactivate
```

**requirements.txt:**
```
fastapi>=0.100.0
uvicorn>=0.23.0
pydantic>=2.0.0
google-generativeai>=0.3.0
elevenlabs>=1.0.0
PyYAML>=6.0
python-dotenv>=1.0.0
```

### Claude Code Prompt for Chapter 7

> "Explain the project structure of my stream-segmenter. Show me the import chain: how does main.py import from core/, models/, and services/? Are there any circular import issues?"

### Chapter 7 Mini-Project

Restructure a flat Python script into a proper project:
1. Create the directory structure with `__init__.py` files
2. Split a monolithic script into models, services, and utils
3. Set up `requirements.txt` and virtual environment
4. Make all imports work correctly

---

# PART II: PYTHON FOR DATA & AI

---

## Chapter 8: NumPy - The Foundation of Everything

**Timeline: Days 14-17**

NumPy is the foundation of all data science and ML in Python. Even if you never use it directly, every AI library uses it under the hood.

### 8.1 Arrays, Shapes & Dtypes

```python
import numpy as np

# Create arrays
a = np.array([1, 2, 3, 4, 5])           # 1D array
b = np.array([[1, 2, 3], [4, 5, 6]])    # 2D array (matrix)

# Shape tells you the dimensions
print(a.shape)   # (5,)      - 1D, 5 elements
print(b.shape)   # (2, 3)    - 2D, 2 rows x 3 columns

# Dtype is the data type
print(a.dtype)   # int64
c = np.array([1.0, 2.0, 3.0])
print(c.dtype)   # float64

# Create special arrays
np.zeros((3, 4))        # 3x4 matrix of zeros
np.ones((2, 3))         # 2x3 matrix of ones
np.arange(0, 10, 2)     # [0, 2, 4, 6, 8]
np.linspace(0, 1, 5)    # [0, 0.25, 0.5, 0.75, 1.0]
np.random.randn(3, 4)   # 3x4 random normal values
```

### 8.2 Vectorized Operations (100x Faster Than Loops)

```python
import numpy as np

a = np.array([1, 2, 3, 4, 5])

# Operations apply to EVERY element at once (no loops!)
print(a * 2)        # [2, 4, 6, 8, 10]
print(a ** 2)       # [1, 4, 9, 16, 25]
print(a + 10)       # [11, 12, 13, 14, 15]
print(np.sqrt(a))   # [1.0, 1.41, 1.73, 2.0, 2.24]

# Boolean masking (filter with condition)
print(a > 3)        # [False, False, False, True, True]
print(a[a > 3])     # [4, 5] -- only elements where condition is True

# Matrix operations
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(A + B)        # Element-wise addition
print(A * B)        # Element-wise multiplication
print(A @ B)        # Matrix multiplication (dot product)
```

### 8.3 Why NumPy Matters for AI

```python
# Cosine similarity (used in semantic search)
def cosine_similarity(a, b):
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

# Embedding vectors from an AI model
vec1 = np.array([0.1, 0.3, 0.5, 0.7])
vec2 = np.array([0.2, 0.4, 0.4, 0.6])

similarity = cosine_similarity(vec1, vec2)
print(f"Similarity: {similarity:.4f}")  # 0.9876 (very similar!)
```

### Claude Code Prompt for Chapter 8

> "Help me learn NumPy by building something practical: a simple semantic search using cosine similarity. Create 10 fake document embeddings, then search for the most similar one to a query. Explain each NumPy operation."

### Chapter 8 Mini-Project

Build a mini embedding search engine:
1. Generate 100 random "document embeddings" (vectors of size 384)
2. Create a "query embedding"
3. Calculate cosine similarity between query and all documents
4. Return top 5 most similar documents
5. Time the operation with NumPy vs pure Python loops

---

## Chapter 9: Pandas - Data Wrangling

**Timeline: Days 17-19**

Pandas is for working with tabular data (like spreadsheets or database tables).

### 9.1 DataFrames Basics

```python
import pandas as pd

# Create from dict
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'age': [30, 25, 35, 28],
    'department': ['eng', 'sales', 'eng', 'sales'],
    'salary': [90000, 70000, 95000, 75000]
})

# Quick overview
print(df.head())       # First 5 rows
print(df.info())       # Column types, non-null counts
print(df.describe())   # Statistics for numeric columns
print(df.shape)        # (4, 4) - rows, columns

# Read from CSV
df = pd.read_csv('data/employees.csv')

# Select columns
df['name']                    # Single column (Series)
df[['name', 'age']]          # Multiple columns (DataFrame)

# Filter rows
df[df['age'] > 30]                           # Age over 30
df[df['department'] == 'eng']                # Engineers only
df.query('age > 30 and department == "eng"') # SQL-like syntax

# GroupBy
df.groupby('department')['salary'].mean()
# department
# eng      92500.0
# sales    72500.0
```

### 9.2 Common Operations

```python
# Sort
df.sort_values('salary', ascending=False)

# Add column
df['bonus'] = df['salary'] * 0.1

# Handle missing data
df['email'].fillna('unknown@email.com')  # Fill missing
df.dropna()                                # Drop rows with any missing

# Apply function to column
df['name_upper'] = df['name'].apply(str.upper)

# Save
df.to_csv('output.csv', index=False)
df.to_json('output.json', orient='records')
```

### Claude Code Prompt for Chapter 9

> "Help me learn Pandas by analyzing a real dataset. Download a sample CSV (use a public dataset URL), load it, explore it, clean it, and create a summary report. Explain each Pandas operation step by step."

### Chapter 9 Mini-Project

Analyze transcript data from your stream-segmenter:
1. Load all transcript JSON files into a DataFrame
2. Calculate: words per segment, average confidence, total duration
3. Group by time windows (morning/afternoon)
4. Create a summary report
5. Export to CSV

---

## Chapter 10: Type Hints & Pydantic

**Timeline: Days 19-20**

Type hints make your code self-documenting. Pydantic enforces types at runtime (critical for APIs).

### 10.1 Type Hints Basics

```python
from typing import List, Dict, Optional, Union, Tuple

# Basic type hints
name: str = "Alice"
age: int = 30
score: float = 95.5
active: bool = True

# Function type hints
def greet(name: str, times: int = 1) -> str:
    """The -> str means this function returns a string."""
    return f"Hello {name}! " * times

# Collection types
names: List[str] = ["Alice", "Bob"]
scores: Dict[str, float] = {"alice": 95.5}
point: Tuple[float, float] = (3.0, 4.0)

# Optional (can be the type OR None)
email: Optional[str] = None  # str or None

# Union (can be either type)
id: Union[str, int] = 42     # str or int
```

### 10.2 Pydantic (Runtime Validation)

```python
from pydantic import BaseModel, Field
from typing import Optional, List

class UserCreate(BaseModel):
    """Pydantic validates data at runtime (dataclasses don't!)."""
    name: str
    email: str
    age: int = Field(ge=0, le=150)  # ge=greater or equal, le=less or equal
    tags: List[str] = []

# Valid
user = UserCreate(name="Alice", email="alice@test.com", age=30)

# Invalid - raises ValidationError!
user = UserCreate(name="Alice", email="alice@test.com", age=-5)
# ValidationError: age must be >= 0

# Auto-converts types when possible
user = UserCreate(name="Alice", email="a@b.com", age="30")
print(user.age)      # 30 (converted string to int!)
print(type(user.age)) # <class 'int'>

# Serialize
print(user.model_dump())       # Dict
print(user.model_dump_json())  # JSON string
```

**Why Pydantic matters:** FastAPI uses Pydantic for request/response validation:

```python
from fastapi import FastAPI
app = FastAPI()

@app.post("/users")
async def create_user(user: UserCreate):  # Auto-validated!
    return {"message": f"Created {user.name}"}
```

### Claude Code Prompt for Chapter 10

> "Show me the difference between dataclasses and Pydantic BaseModel. When should I use each? Add Pydantic validation to my stream-segmenter's API request models."

### Chapter 10 Mini-Project

Build validated config models:
1. Create Pydantic models for: `StreamConfig`, `LLMConfig`, `APIConfig`
2. Add validation rules (URL format, API key length, temperature 0-1)
3. Load from YAML file with environment variable expansion
4. Handle validation errors with user-friendly messages

---

## Chapter 11: Async Python & Concurrency

**Timeline: Days 20-22**

Your system needs to do multiple things at once: download video, transcribe audio, analyze transcripts. This chapter teaches you how.

### 11.1 async/await Basics

```python
import asyncio

# An async function (coroutine)
async def fetch_data(url):
    print(f"Fetching {url}...")
    await asyncio.sleep(1)  # Simulate network delay
    print(f"Got response from {url}")
    return f"data from {url}"

# Run async code
async def main():
    result = await fetch_data("https://api.example.com")
    print(result)

asyncio.run(main())
```

### 11.2 Parallel Execution (like Promise.all)

```python
import asyncio
import time

async def fetch(url, delay):
    await asyncio.sleep(delay)
    return f"Result from {url}"

async def main():
    start = time.time()

    # Run ALL requests in parallel (like Promise.all)
    results = await asyncio.gather(
        fetch("api1", 2),
        fetch("api2", 1),
        fetch("api3", 3),
    )

    elapsed = time.time() - start
    print(f"All done in {elapsed:.1f}s")  # ~3s, not 6s!
    print(results)

asyncio.run(main())
```

### 11.3 Semaphores for Rate Limiting

```python
import asyncio

# Limit to 5 concurrent API calls
semaphore = asyncio.Semaphore(5)

async def call_api(url):
    async with semaphore:  # Only 5 at a time
        await asyncio.sleep(1)
        return f"result from {url}"

async def main():
    urls = [f"https://api.com/{i}" for i in range(20)]
    results = await asyncio.gather(*[call_api(url) for url in urls])
    # Processes 20 URLs, but only 5 concurrent at any time
```

### 11.4 Threading for CPU-Bound Work

```python
import threading

# Threading: for I/O-bound tasks (network, file operations)
def download_file(url):
    print(f"Downloading {url}...")
    time.sleep(2)
    print(f"Done: {url}")

# Create and start threads
threads = []
for url in ["file1.mp4", "file2.mp4", "file3.mp4"]:
    t = threading.Thread(target=download_file, args=(url,))
    t.start()
    threads.append(t)

# Wait for all to finish
for t in threads:
    t.join()

# Daemon threads (background workers that stop when main program exits)
t = threading.Thread(target=background_task, daemon=True)
t.start()
# Program can exit even if this thread is still running
```

**Thread safety with locks:**

```python
import threading

counter = 0
lock = threading.Lock()

def safe_increment():
    global counter
    with lock:           # Only one thread at a time
        counter += 1     # Now this is safe
```

**When to use what:**

| Approach | Best For | Example |
|----------|----------|---------|
| `asyncio` | Many network calls | API requests, WebSocket |
| `threading` | I/O mixed with sync code | FFmpeg + file monitoring |
| `multiprocessing` | CPU-heavy work | Data processing, ML training |

### Claude Code Prompt for Chapter 11

> "Show me all the threading and async code in my stream-segmenter. Explain why certain parts use threading (FFmpeg monitoring) vs async (WebSocket server). Are there any race conditions?"

### Chapter 11 Mini-Project

Build a parallel API fetcher:
1. Create an async function that calls 10 URLs concurrently
2. Add rate limiting with semaphore (max 3 concurrent)
3. Add retry logic for failed requests
4. Compare execution time: sequential vs parallel
5. Output results sorted by response time

---

# PART III: BACKEND & API DEVELOPMENT

---

## Chapter 12: FastAPI - Building AI-Ready APIs

**Timeline: Days 22-26**

FastAPI is the modern Python web framework. It's fast, has automatic docs, and uses Pydantic for validation.

### 12.1 Your First API

```python
from fastapi import FastAPI, HTTPException, Query
from pydantic import BaseModel
from typing import Optional, List

app = FastAPI(title="My AI API")

# GET endpoint
@app.get("/")
async def root():
    return {"message": "Hello World"}

# GET with path parameter
@app.get("/items/{item_id}")
async def get_item(item_id: int):
    return {"item_id": item_id}

# GET with query parameters
@app.get("/search")
async def search(q: str, limit: int = Query(default=10, le=100)):
    return {"query": q, "limit": limit}
```

### 12.2 Request & Response Models

```python
from pydantic import BaseModel

class ItemCreate(BaseModel):
    name: str
    price: float
    description: Optional[str] = None

class ItemResponse(BaseModel):
    id: int
    name: str
    price: float

# POST with validated body
@app.post("/items", response_model=ItemResponse)
async def create_item(item: ItemCreate):
    # item is already validated by Pydantic!
    return ItemResponse(id=1, name=item.name, price=item.price)

# Error handling
@app.get("/items/{item_id}")
async def get_item(item_id: int):
    if item_id not in database:
        raise HTTPException(status_code=404, detail="Item not found")
    return database[item_id]
```

### 12.3 Middleware

```python
from fastapi.middleware.cors import CORSMiddleware

# CORS: Allow browser requests from your frontend
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],  # Your frontend URL
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### 12.4 Streaming Responses (for LLM output)

```python
from fastapi.responses import StreamingResponse

async def generate_stream():
    """Stream LLM response token by token."""
    for word in ["Hello", " ", "world", "!"]:
        yield f"data: {word}\n\n"
        await asyncio.sleep(0.1)

@app.get("/stream")
async def stream():
    return StreamingResponse(
        generate_stream(),
        media_type="text/event-stream"
    )
```

### 12.5 WebSockets for Real-Time

```python
from fastapi import WebSocket, WebSocketDisconnect

# Track connected clients
connections: list[WebSocket] = []

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    connections.append(websocket)
    try:
        while True:
            data = await websocket.receive_text()
            # Broadcast to all clients
            for conn in connections:
                await conn.send_text(f"Echo: {data}")
    except WebSocketDisconnect:
        connections.remove(websocket)
```

### 12.6 Running the Server

```bash
# Install
pip install fastapi uvicorn

# Run (with auto-reload for development)
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# API docs auto-generated at:
# http://localhost:8000/docs
```

### Claude Code Prompt for Chapter 12

> "Show me the FastAPI server in my stream-segmenter (server.py). Explain each endpoint, the WebSocket handler, and the CORS setup. How does the frontend communicate with the backend?"

### Chapter 12 Mini-Project

Build a transcript management API:
1. POST /transcripts - Upload a transcript (validated with Pydantic)
2. GET /transcripts - List all transcripts (with pagination)
3. GET /transcripts/{id} - Get specific transcript
4. GET /transcripts/search?q=keyword - Search transcripts
5. WebSocket /ws - Stream real-time transcript updates
6. Auto-generated docs at /docs

---

## Chapter 13: Databases with SQLAlchemy

**Timeline: Days 26-28**

### 13.1 SQLite Basics

SQLite is a lightweight database perfect for local storage (no server needed):

```python
import sqlite3

# Connect (creates file if it doesn't exist)
conn = sqlite3.connect("data/app.db")
conn.row_factory = sqlite3.Row  # Return dicts instead of tuples

cursor = conn.cursor()

# Create table
cursor.execute("""
    CREATE TABLE IF NOT EXISTS segments (
        id INTEGER PRIMARY KEY,
        file_path TEXT NOT NULL,
        status TEXT DEFAULT 'pending',
        created_at TEXT
    )
""")

# Insert (ALWAYS use ? parameters, never string formatting!)
cursor.execute(
    "INSERT INTO segments (file_path, status) VALUES (?, ?)",
    ("segment_001.ts", "pending")
)
conn.commit()

# Query
cursor.execute("SELECT * FROM segments WHERE status = ?", ("pending",))
rows = cursor.fetchall()
for row in rows:
    print(dict(row))  # {'id': 1, 'file_path': 'segment_001.ts', ...}

conn.close()
```

**Your stream-segmenter uses SQLite** in `state_manager.py` for crash recovery. If the system crashes, it can resume from the last processed segment.

### 13.2 SQLAlchemy ORM

For larger projects, use SQLAlchemy instead of raw SQL:

```python
from sqlalchemy import create_engine, Column, Integer, String, Float
from sqlalchemy.orm import declarative_base, sessionmaker

Base = declarative_base()

class Segment(Base):
    __tablename__ = 'segments'
    id = Column(Integer, primary_key=True)
    file_path = Column(String, nullable=False)
    status = Column(String, default='pending')
    duration = Column(Float)

# Create engine and tables
engine = create_engine("sqlite:///data/app.db")
Base.metadata.create_all(engine)
Session = sessionmaker(bind=engine)

# Use it
session = Session()
segment = Segment(file_path="seg_001.ts", duration=30.0)
session.add(segment)
session.commit()

# Query
segments = session.query(Segment).filter_by(status='pending').all()
```

### Claude Code Prompt for Chapter 13

> "Show me the StateManager in my stream-segmenter. Explain how it uses SQLite for crash recovery. What tables exist? How does it track segment processing state?"

### Chapter 13 Mini-Project

Build a transcript database:
1. Create SQLite tables for segments, transcripts, and topics
2. Write CRUD functions with proper error handling
3. Add a context manager for safe database connections
4. Implement crash recovery: save state, detect incomplete processing on restart

---

## Chapter 14: Authentication & Security

**Timeline: Days 28-29**

### 14.1 JWT Tokens

```python
import jwt
from datetime import datetime, timedelta

SECRET_KEY = "your-secret-key"

# Create token
def create_token(user_id: int) -> str:
    payload = {
        "user_id": user_id,
        "exp": datetime.utcnow() + timedelta(hours=24)
    }
    return jwt.encode(payload, SECRET_KEY, algorithm="HS256")

# Verify token
def verify_token(token: str) -> dict:
    try:
        return jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
    except jwt.ExpiredSignatureError:
        raise ValueError("Token expired")
    except jwt.InvalidTokenError:
        raise ValueError("Invalid token")
```

### 14.2 API Key Auth

```python
from fastapi import Security, HTTPException
from fastapi.security import APIKeyHeader

api_key_header = APIKeyHeader(name="X-API-Key")

async def verify_api_key(api_key: str = Security(api_key_header)):
    if api_key != os.getenv("API_KEY"):
        raise HTTPException(status_code=403, detail="Invalid API key")

@app.get("/protected", dependencies=[Security(verify_api_key)])
async def protected_route():
    return {"message": "You have access!"}
```

### Claude Code Prompt for Chapter 14

> "Help me learn JWT authentication and API key security. Build a simple FastAPI app with both auth methods. Then show me how security is handled in my stream-segmenter's API."

### Chapter 14 Mini-Project

Add authentication to your Chapter 12 API:
1. JWT login endpoint
2. API key authentication for service-to-service calls
3. Protected routes that require authentication
4. Rate limiting per user

---

## Chapter 15: Testing with Pytest

**Timeline: Days 29-31**

### 15.1 Your First Test

```python
# tests/test_math.py
def test_addition():
    assert 1 + 1 == 2

def test_string():
    assert "hello".upper() == "HELLO"

# Run: pytest tests/test_math.py -v
```

### 15.2 Testing Your Data Models

```python
import pytest
from src.models.segment import Word, Transcript

def test_word_creation():
    word = Word(text="hello", start=1.0, end=2.0)
    assert word.text == "hello"
    assert word.confidence == 1.0  # Default

def test_word_roundtrip():
    """Test serialize -> deserialize produces identical object."""
    original = Word("hello", 1.0, 2.0, 0.95)
    restored = Word.from_dict(original.to_dict())
    assert original == restored

def test_transcript_word_count():
    transcript = Transcript(
        segment_id=1,
        text="Hello world",
        words=[Word("hello", 0, 1), Word("world", 1, 2)]
    )
    assert len(transcript.words) == 2
```

### 15.3 Fixtures (Reusable Setup)

```python
import pytest

@pytest.fixture
def sample_word():
    """Reusable test data."""
    return Word(text="hello", start=1.0, end=2.0, confidence=0.95)

@pytest.fixture
def sample_transcript(sample_word):
    """Can use other fixtures!"""
    return Transcript(segment_id=1, text="hello", words=[sample_word])

def test_with_fixture(sample_transcript):
    assert sample_transcript.segment_id == 1
```

### 15.4 Mocking External APIs

```python
from unittest.mock import patch, MagicMock

def test_transcriber_without_real_api():
    """Test without calling the real ElevenLabs API."""
    with patch('elevenlabs.ElevenLabs') as mock:
        # Setup fake response
        mock.return_value.speech_to_text.convert.return_value = MagicMock(
            text="Hello world",
            words=[{'text': 'hello', 'start': 0, 'end': 1}]
        )

        # Now test your transcriber - it uses the mock, not real API
        transcriber = ElevenLabsTranscriber(api_key="fake")
        result = transcriber.transcribe(Path("audio.wav"), segment_id=1)
        assert result.text == "Hello world"
```

### 15.5 Running Tests

```bash
pytest                          # Run all tests
pytest tests/test_models.py     # Run specific file
pytest -v                       # Verbose output
pytest -x                       # Stop on first failure
pytest --cov=src                # With coverage report
pytest -k "test_word"           # Run tests matching name
```

### Claude Code Prompt for Chapter 15

> "Write tests for my stream-segmenter's data models (segment.py). Test creation, serialization (to_dict/from_dict), and edge cases. Use fixtures for reusable test data. Mock external API calls."

### Chapter 15 Mini-Project

Add tests to your Chapter 12 API project:
1. Test all endpoints with FastAPI's TestClient
2. Test data models with roundtrip serialization
3. Mock external API calls
4. Achieve 80% code coverage
5. Add parametrized tests for edge cases

---

# PART IV: AI & ML PYTHON

---

## Chapter 16: Working with AI APIs

**Timeline: Days 31-34**

### 16.1 Anthropic Claude API

```python
import anthropic

client = anthropic.Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# Basic message
response = client.messages.create(
    model="claude-sonnet-4-5-20250929",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Explain Python decorators"}]
)
print(response.content[0].text)

# Streaming
with client.messages.stream(
    model="claude-sonnet-4-5-20250929",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Write a poem"}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)

# Tool use (function calling)
response = client.messages.create(
    model="claude-sonnet-4-5-20250929",
    max_tokens=1024,
    tools=[{
        "name": "get_weather",
        "description": "Get weather for a city",
        "input_schema": {
            "type": "object",
            "properties": {
                "city": {"type": "string"}
            },
            "required": ["city"]
        }
    }],
    messages=[{"role": "user", "content": "What's the weather in Paris?"}]
)
```

### 16.2 Google Gemini API

```python
import google.generativeai as genai

genai.configure(api_key=os.getenv("GEMINI_API_KEY"))
model = genai.GenerativeModel("gemini-2.5-flash")

# Basic generation
response = model.generate_content("Explain topic detection in news")
print(response.text)

# With JSON output
response = model.generate_content("""
Analyze this text and respond with JSON only:
{"topic": string, "confidence": float}

Text: The election results show a clear victory for...
""")
data = json.loads(response.text)
```

**In your stream-segmenter**, this is exactly how topic detection works:

```python
# From gemini_llm_service.py - the core pattern
response = model.generate_content(prompt)
result = json.loads(response.text)  # Parse JSON from LLM
```

### 16.3 Unified API Client Pattern

```python
class LLMClient:
    """Unified wrapper for multiple LLM providers."""

    def __init__(self, provider: str, api_key: str):
        self.provider = provider
        if provider == "anthropic":
            self.client = anthropic.Anthropic(api_key=api_key)
        elif provider == "gemini":
            genai.configure(api_key=api_key)
            self.client = genai.GenerativeModel("gemini-2.5-flash")

    def generate(self, prompt: str) -> str:
        if self.provider == "anthropic":
            response = self.client.messages.create(
                model="claude-sonnet-4-5-20250929",
                max_tokens=1024,
                messages=[{"role": "user", "content": prompt}]
            )
            return response.content[0].text
        elif self.provider == "gemini":
            response = self.client.generate_content(prompt)
            return response.text
```

### Claude Code Prompt for Chapter 16

> "Show me how my stream-segmenter calls the Gemini API. Trace the flow from prompt construction (layer1_prompts.py) through the API call (gemini_llm_service.py) to response parsing. What happens if the API returns invalid JSON?"

### Chapter 16 Mini-Project

Build an AI chat client:
1. Support both Claude and Gemini APIs
2. Include streaming output
3. Add retry logic and error handling
4. Track token usage and cost
5. Cache repeated queries

---

## Chapter 17: Data Pipelines for AI

**Timeline: Days 34-36**

### 17.1 Document Processing

```python
# PDF parsing
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    for page in pdf.pages:
        text = page.extract_text()
        print(text)
```

### 17.2 Chunking Strategies

```python
def chunk_text(text: str, chunk_size: int = 500, overlap: int = 50) -> list:
    """Split text into overlapping chunks."""
    words = text.split()
    chunks = []

    for i in range(0, len(words), chunk_size - overlap):
        chunk = " ".join(words[i:i + chunk_size])
        chunks.append(chunk)

    return chunks
```

### 17.3 Subprocess: Running External Programs

Your system runs FFmpeg and yt-dlp as external processes:

```python
import subprocess

# Simple command
result = subprocess.run(
    ["ffmpeg", "-version"],
    capture_output=True,
    text=True
)
print(result.stdout)

# With error handling and timeout
try:
    result = subprocess.run(
        ["ffmpeg", "-i", "input.ts", "-c", "copy", "output.mp4"],
        capture_output=True,
        check=True,     # Raise if exit code != 0
        timeout=60      # Kill if takes > 60 seconds
    )
except subprocess.TimeoutExpired:
    print("FFmpeg took too long!")
except subprocess.CalledProcessError as e:
    print(f"FFmpeg failed: {e.stderr}")

# Long-running process (runs in background)
process = subprocess.Popen(
    ["ffmpeg", "-i", stream_url, "-f", "segment", "-segment_time", "30",
     "-c", "copy", "segment_%05d.ts"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE
)
# process runs in background, check with process.poll()
```

### Claude Code Prompt for Chapter 17

> "Show me how my stream-segmenter uses subprocess to run FFmpeg. Trace the flow: YouTube URL -> yt-dlp extracts stream URL -> FFmpeg segments video -> file monitor detects new segments. How does it handle FFmpeg crashes?"

### Chapter 17 Mini-Project

Build a document processing pipeline:
1. Read PDF files from a directory
2. Extract text from each page
3. Chunk the text with overlap
4. Save chunks as JSONL
5. Add error handling for corrupt PDFs

---

## Chapter 18: PyTorch Essentials

**Timeline: Days 36-40**

### 18.1 Tensors

```python
import torch

# Create tensors (like NumPy arrays but GPU-compatible)
x = torch.tensor([1, 2, 3, 4])
y = torch.zeros(3, 4)          # 3x4 of zeros
z = torch.randn(3, 4)          # 3x4 random normal

# Operations
print(x + 2)                   # Element-wise
print(x * x)                   # Element-wise
print(torch.matmul(z, z.T))    # Matrix multiplication

# GPU (if available)
if torch.cuda.is_available():
    x_gpu = x.to('cuda')       # Move to GPU
```

### 18.2 Simple Neural Network

```python
import torch.nn as nn

class SimpleNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.layers = nn.Sequential(
            nn.Linear(10, 64),
            nn.ReLU(),
            nn.Linear(64, 1)
        )

    def forward(self, x):
        return self.layers(x)

# Training loop
model = SimpleNet()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
loss_fn = nn.MSELoss()

for epoch in range(100):
    prediction = model(input_data)
    loss = loss_fn(prediction, target)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
```

### Claude Code Prompt for Chapter 18

> "Help me understand PyTorch by building a simple text classifier. Create a small dataset, train a model, evaluate it. Explain tensors, autograd, and the training loop step by step."

### Chapter 18 Mini-Project

Build a sentiment classifier:
1. Create a simple dataset of movie reviews
2. Tokenize and embed text
3. Build a 2-layer neural network
4. Train for 10 epochs
5. Evaluate accuracy

---

# PART V: PRODUCTION PYTHON

---

## Chapter 19: Docker & Deployment

**Timeline: Days 40-42**

### 19.1 Dockerfile for Python

```dockerfile
# Use official Python image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install dependencies first (cached layer)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code
COPY src/ src/
COPY config/ config/

# Run the app
CMD ["python", "src/main.py"]
```

### 19.2 Docker Compose

```yaml
# docker-compose.yml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - GEMINI_API_KEY=${GEMINI_API_KEY}
    volumes:
      - ./data:/app/data
      - ./logs:/app/logs
```

```bash
# Build and run
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f api
```

### Claude Code Prompt for Chapter 19

> "Create a Dockerfile and docker-compose.yml for my stream-segmenter. Include FFmpeg installation, volume mounts for data/logs, and environment variable handling for API keys."

### Chapter 19 Mini-Project

Containerize your Chapter 12 API:
1. Write a multi-stage Dockerfile
2. Create docker-compose with API + database
3. Handle environment variables securely
4. Set up health checks

---

## Chapter 20: Logging, Monitoring & Best Practices

**Timeline: Days 42-43**

### 20.1 Structured Logging

```python
import logging
import json

class JSONFormatter(logging.Formatter):
    """Log in JSON format for easy parsing."""
    def format(self, record):
        return json.dumps({
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "module": record.module,
            "message": record.getMessage(),
        })

# Setup
handler = logging.StreamHandler()
handler.setFormatter(JSONFormatter())
logger = logging.getLogger("myapp")
logger.addHandler(handler)
logger.setLevel(logging.INFO)

logger.info("Segment processed", extra={"segment_id": 42, "duration": 30.5})
```

### 20.2 Code Quality Tools

```bash
# Format code automatically
pip install black ruff
black src/            # Auto-format
ruff check src/       # Lint (find issues)
ruff check src/ --fix # Auto-fix issues

# Type checking
pip install mypy
mypy src/             # Find type errors

# All at once with pre-commit
pip install pre-commit
# Add .pre-commit-config.yaml, then:
pre-commit install    # Runs checks on every git commit
```

### Claude Code Prompt for Chapter 20

> "Set up code quality tools for my stream-segmenter: black for formatting, ruff for linting, mypy for type checking. Create a pre-commit config that runs all three on every commit."

### Chapter 20 Mini-Project

Add production monitoring to your API:
1. Structured JSON logging
2. Request timing middleware
3. Health check endpoint
4. Set up black + ruff + mypy
5. Create pre-commit hooks

---

## Chapter 21: Build Projects & Next Steps

Here are 6 progressively harder projects. Each builds on the skills from this book. Use Claude Code to help, but **understand every line**.

### Project 1: CLI Task Manager (Beginner)

Command-line tool with file-based JSON storage.
**Uses:** Chapters 1-6, argparse, dataclasses.

**Claude Code Prompt:**
> "Build a Python CLI task manager. Features: add/list/complete/delete tasks, save to JSON, filter by status/priority. Use only built-in modules."

### Project 2: Web Scraper & Pipeline (Intermediate)

Scrape docs, clean data, chunk, store as JSONL.
**Uses:** async Python, BeautifulSoup, data processing.

**Claude Code Prompt:**
> "Build an async web scraper that crawls a documentation site, cleans HTML to markdown, chunks the content, and saves as JSONL."

### Project 3: REST API with Database (Intermediate)

Complete CRUD API with auth and Docker.
**Uses:** FastAPI, SQLAlchemy, Pydantic, JWT, Docker, pytest.

**Claude Code Prompt:**
> "Build a complete document management API: CRUD endpoints, JWT auth, PostgreSQL, Docker, 80% test coverage."

### Project 4: Semantic Search Engine (AI)

Embed documents, vector DB search.
**Uses:** AI APIs, NumPy, ChromaDB, FastAPI.

**Claude Code Prompt:**
> "Build a semantic search engine: embed 500+ documents, store in vector DB, search with cosine similarity, serve via API."

### Project 5: RAG Chatbot (AI Advanced)

Full RAG pipeline with streaming.
**Uses:** Everything from this book.

**Claude Code Prompt:**
> "Build a RAG chatbot: PDF ingestion, chunking, embedding, vector search, LLM generation with streaming, FastAPI backend, conversation memory."

### Project 6: AI Agent with Tools (AI Advanced)

Agent with web search, DB queries, file operations.
**Uses:** Function calling, orchestration.

**Claude Code Prompt:**
> "Build an AI agent that can search the web, query databases, read files, and compose reports. Include tool calling, error handling, and evaluation."

---

# APPENDICES

---

## Appendix A: JavaScript to Python Cheat Sheet

| JavaScript | Python | Notes |
|-----------|--------|-------|
| `const x = 5` | `x = 5` | No keywords needed |
| `let name = "Alice"` | `name = "Alice"` | Same |
| `typeof x` | `type(x)` | Type checking |
| `true` / `false` | `True` / `False` | Capital letters! |
| `null` | `None` | Capital N |
| `===` | `==` | Python only has `==` |
| `` `Hi ${name}` `` | `f'Hi {name}'` | f-strings |
| `arr.map(x => x*2)` | `[x*2 for x in arr]` | List comprehension |
| `arr.filter(x => x>5)` | `[x for x in arr if x>5]` | With condition |
| `arr.forEach(fn)` | `for x in arr: fn(x)` | For loop |
| `arr.length` | `len(arr)` | Built-in function |
| `arr.push(x)` | `arr.append(x)` | Add to end |
| `arr.includes(x)` | `x in arr` | Membership |
| `{...obj, key: val}` | `{**obj, 'key': val}` | Spread/unpack |
| `Object.keys(obj)` | `obj.keys()` | Dict method |
| `Object.entries(obj)` | `obj.items()` | Key-value pairs |
| `JSON.stringify(x)` | `json.dumps(x)` | Serialize |
| `JSON.parse(x)` | `json.loads(x)` | Deserialize |
| `new Class()` | `Class()` | No `new` keyword |
| `this` | `self` | Instance reference |
| `async/await` | `async/await` | Same concept |
| `Promise.all()` | `asyncio.gather()` | Parallel async |
| `try/catch/finally` | `try/except/finally` | `except` not `catch` |
| `import x from 'y'` | `from y import x` | Reversed order |
| `require('fs')` | `import os` / `from pathlib import Path` | Built-in |
| `setTimeout(fn, ms)` | `time.sleep(s)` | Seconds not ms |
| `console.log()` | `print()` | Basic output |
| `process.env.KEY` | `os.getenv('KEY')` | Env vars |
| `x => x * 2` | `lambda x: x * 2` | Arrow/lambda |
| `Array.isArray(x)` | `isinstance(x, list)` | Type check |

---

## Appendix B: Common Python Gotchas

### 1. Mutable Default Arguments

```python
# WRONG - list is shared across all calls!
def add_item(item, items=[]):
    items.append(item)
    return items

add_item(1)  # [1]
add_item(2)  # [1, 2] -- NOT [2]!

# RIGHT
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

### 2. = vs ==

```python
if x = 5:   # SyntaxError! (assignment)
if x == 5:  # Correct (comparison)
```

### 3. Indentation Errors

```python
# Mixing tabs and spaces = errors
# ALWAYS use 4 spaces, NEVER tabs
```

### 4. String vs Path

```python
path = "data/file.json"
path.exists()  # ERROR - strings don't have exists()

from pathlib import Path
path = Path("data/file.json")
path.exists()  # Works!
```

### 5. Integer Division

```python
7 / 2    # 3.5 (float division)
7 // 2   # 3   (integer division - rounds down)
```

### 6. Global Variables

```python
count = 0
def increment():
    count += 1    # ERROR! Python thinks count is local

def increment():
    global count  # Must declare global
    count += 1    # Now works
```

---

## Appendix C: Your Stream-Segmenter Codebase Map

This maps every concept in this book to files in your actual project:

| Concept | File | What to Look For |
|---------|------|-----------------|
| Dataclasses | `src/models/segment.py` | Word, Transcript, Segment classes |
| JSON serialization | `src/models/segment.py` | to_dict(), from_dict() methods |
| Properties | `src/models/segment.py` | @property decorators |
| SQLite database | `src/services/state_manager.py` | CREATE TABLE, INSERT, SELECT |
| Context managers | `src/services/state_manager.py` | _get_connection() with yield |
| API clients | `src/core/transcriber.py` | ElevenLabsTranscriber class |
| Subprocess | `src/core/transcriber.py` | FFmpeg audio extraction |
| Retry logic | `src/services/gemini_llm_service.py` | Exponential backoff loop |
| LLM prompts | `src/core/layer1_prompts.py` | Prompt construction |
| Threading | `src/main.py` | Daemon threads, locks |
| Configuration | `src/main.py` | YAML loading, env var expansion |
| FastAPI | `src/api/server.py` | Routes, WebSocket, CORS |
| WebSockets | `src/api/server.py` | Real-time broadcast |
| Logging | Throughout all files | logger = logging.getLogger() |
| Error handling | Throughout all files | try/except patterns |
| File operations | Throughout all files | Path, open(), json.load() |

### File Reading Order (Beginner to Advanced)

1. `src/models/segment.py` - Start here. Dataclasses, types, serialization.
2. `src/services/state_manager.py` - SQLite, context managers, CRUD.
3. `src/core/transcriber.py` - API clients, subprocess, retry.
4. `src/services/gemini_llm_service.py` - LLM API, JSON parsing, retry.
5. `src/core/layer1_prompts.py` - Prompt engineering, f-strings.
6. `src/core/stream_processor.py` - Threading, queues, file watching.
7. `src/api/server.py` - FastAPI, WebSockets, REST endpoints.
8. `src/main.py` - **Read last.** System orchestration, config, lifecycle.

---

## Your Python Learning Philosophy

You are NOT memorizing syntax. You are building a **mental model** of how Python works.

1. **Understand the concept first.** Read this book.
2. **Then use Claude Code to implement it.** Ask it to build something.
3. **Then ask Claude Code to show you the same pattern in your existing projects.** Connect theory to practice.
4. **Then build something real.** The mini-projects cement your knowledge.
5. **Repeat.**

In 6 weeks, Python will feel as natural as JavaScript does today.

---

*Written for AI engineers who build with Claude Code. Every concept connects to real production code.*
