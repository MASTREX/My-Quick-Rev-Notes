## built in functions
- `__class__`: to get the class of an object and to gets its name use `<object_variable>.__class__.__name__`
-  `id()` function returns a unique id for the specified object. All objects in Python has its own unique id. The id is assigned to the object when it is created.

## Operation on object functions
- `__init__`: same as constructor
- `__repr__`: to represent objects without ambiguity. Objects can be reconstructed from this form.
- `__str__`: human readable form representation of the objects. This is optional and if not defined then it is same as `__repr__`
- `__add__`: to add two objects
- `__mul__`: to multiply two objects

## decorator
A Python decorator is a function that takes another function (or method) as an argument, adds some functionality to it, and returns the modified function. Decorators are commonly used to modify the behavior of functions or methods in a clean and reusable way.

Defined like this
```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        # Code to be executed before the function call
        print("Before the function call")

        # Call the original function
        result = func(*args, **kwargs)

        # Code to be executed after the function call
        print("After the function call")

        return result
    return wrapper
```
apply it
```python
@my_decorator
def say_hello(name):
    print(f"Hello, {name}!")

# Call the decorated function
say_hello("Alice")

```

---
---

# Basic Modules
## os
The `os` module in Python provides a way of using operating system-dependent functionality like reading or writing to the file system, managing directories, and interacting with environment variables. It acts as a bridge between Python and the underlying OS.

### 📁 File and Directory Management
- `os.getcwd()` – Returns the current working directory.
- `os.chdir(path)` – Changes the current working directory.
- `os.listdir(path='.')` – Lists files and directories in the given path.
- `os.mkdir(path)` – Creates a new directory.
- `os.makedirs(path)` – Recursively creates directories.
- `os.remove(path)` – Deletes a file.
- `os.rmdir(path)` – Removes an empty directory.
- `os.rename(src, dst)` – Renames a file or directory.
- `os.path` – Submodule for file path manipulations (e.g., `os.path.join()`, `os.path.exists()`).
> **Tip:** Use `pathlib` (Python 3.4+) for a more modern and object-oriented approach to file paths.

### 🌍 Environment Variables
- `os.environ` – A dictionary representing the environment variables.
- `os.getenv(key, default=None)` – Gets the value of an environment variable.
- `os.putenv(key, value)` – Sets an environment variable (note: not always persistent across subprocesses).

### 🛠️ Process Management
- `os.system(command)` – Runs a shell command.
- `os.startfile(path)` *(Windows only)* – Opens a file with its associated application.
- `os.exec*()` – Replaces the current process with a new one.

### 📊 Useful Constants and Info
- `os.name` – Returns `'posix'`, `'nt'`, etc., depending on the OS.
- `os.sep` – Returns the path separator (`'/'` on Unix, `'\\'` on Windows).
- `os.linesep` – Returns the line separator (`'\n'`, `'\r\n'`, etc.).
- `os.path.abspath(path)` – Returns the absolute path.

---


## datetime
The `datetime` module in Python provides classes for manipulating dates and times in both simple and complex ways.

### Key Classes

- **`datetime.date`**: Handles **dates** (year, month, day).
- **`datetime.time`**: Handles **time** (hour, minute, second, microsecond).
- **`datetime.datetime`**: Combines `date` and `time`.
- **`datetime.timedelta`**: Represents the **difference** between two dates or times.
- **`datetime.tzinfo`**: Base class for dealing with **time zones**.
- **`datetime.timezone`**: A concrete subclass of `tzinfo` for fixed offset time zones.

### Usage of datetime
```python
d = date(2025, 6, 7)  # year, month, day
t = time(14, 30, 0)   # hour, minute, second
dt = datetime(2025, 6, 7, 14, 30)

# Current Date and Time
now = datetime.now()           # local current datetime
utc_now = datetime.utcnow()    # current UTC datetime
today = date.today()           # current date

# Formatting
formatted = dt.strftime("%Y-%m-%d %H:%M:%S")
parsed = datetime.strptime("2025-06-07", "%Y-%m-%d")

# timedelta (date arithmatic)
delta = timedelta(days=5)
future_date = date.today() + delta
difference = date(2025, 6, 10) - date(2025, 6, 7)  # timedelta(days=3)
```

### datetime formats

| Code | Meaning                      | Example           |
|------|------------------------------|-------------------|
| `%Y` | Year with century            | `2025`            |
| `%y` | Year without century (00–99) | `25`              |
| `%m` | Month (01–12)                | `06`              |
| `%B` | Full month name              | `June`            |
| `%b` | Abbreviated month name       | `Jun`             |
| `%d` | Day of the month (01–31)     | `07`              |
| `%A` | Full weekday name            | `Saturday`        |
| `%a` | Abbreviated weekday name     | `Sat`             |
| `%H` | Hour (24-hour clock, 00–23)  | `14`              |
| `%I` | Hour (12-hour clock, 01–12)  | `02`              |
| `%p` | AM/PM                        | `PM`              |
| `%M` | Minute (00–59)               | `30`              |
| `%S` | Second (00–59)               | `45`              |
| `%f` | Microsecond (000000–999999)  | `123456`          |
| `%z` | UTC offset                   | `+0000` or `-0500`|
| `%Z` | Time zone name               | `UTC`, `EST`      |
| `%j` | Day of the year (001–366)    | `158`             |
| `%U` | Week number (Sunday first)   | `23`              |
| `%W` | Week number (Monday first)   | `22`              |
| `%c` | Locale date and time         | `Sat Jun  7 14:30:00 2025` |
| `%x` | Locale date                  | `06/07/25`        |
| `%X` | Locale time                  | `14:30:00`        |

> ### Notes
  - `datetime` objects can be **naive** (no timezone) or **aware** (with timezone).
  - Always prefer timezone-aware datetimes in applications dealing with multiple time zones.

---


## logging
The `logging` module in Python provides a flexible framework for emitting log messages from Python programs. It is used to track events that happen when some software runs, which can help in debugging and monitoring.

### Key Features

- **Multiple Severity Levels:** Logs can be emitted at different levels: `DEBUG`, `INFO`, `WARNING`, `ERROR`, and `CRITICAL`.
- **Configurable Handlers:** Logs can be directed to different outputs like console, files, or remote servers using handlers (`StreamHandler`, `FileHandler`, etc.).
- **Formatters:** Control the layout of log messages (timestamps, log level, message, etc.).
- **Hierarchical Loggers:** Supports multiple loggers with parent-child relationships, allowing granular control.
- **Thread-safe:** Can be safely used in multi-threaded applications.

### Basic Usage Example

```python
import logging

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
logging.info("This is an info message")
logging.error("This is an error message")
```

### Common Components
- `Logger`: Entry point for logging messages (`logging.getLogger(__name__)`).
- `Handler`: Sends the log records to destinations (console, files).
- `Formatter`: Specifies the layout of the log message.
- `Filter`: Provides finer grained control over which log records to output.

#### Why Use logging Instead of print?
- Supports severity levels.
- Easy to enable/disable or route output without changing code.
- Provides timestamps and contextual information.
- Can be configured via code or config files.

### Best Practice for Multi-Module Logging

1. **Configure Logging Once (Usually in the Main Entry Point)**
   Set up logging configuration (handlers, formatters, levels) only once, typically in your main script or a dedicated config module.

2. **Get Loggers by Name in Each Module**
   Each module calls `logging.getLogger(__name__)` to get a logger specific to that module’s namespace.

3. **Let Loggers Propagate to the Root Logger**
   By default, child loggers propagate messages to the root logger, which handles the output.
---


## asyncio
- At `await` keyword the code is blocked in a non blocking way and other things in eventloop are excuted until this function is done.
```python
import asyncio

# method 1
async def async_function():
    await asyncio.sleep(1)
    return "Hello, async!"

result = asyncio.run(async_function())
print(result)

# method 2
loop = asyncio.get_event_loop()
result = loop.run_until_complete(async_function())
print(result)

# mehtod 3
async def main():
    task = asyncio.create_task(async_function())
    result = await task
    print(result)

asyncio.run(main())
```
