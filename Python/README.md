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
