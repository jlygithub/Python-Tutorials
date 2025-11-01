# Sample 1: Python `with` Statement: Mastering Context Managers

## Tabel of Contents

1. Why `with` Matters
2. Using `with`
3. How It Works: Context Managers
4. Practical Example
5. Summary

## 1. Why `with` Matters

Python has a `with` statement that is used to manage resources. Without `with`, you have to manually close files:

```python
f = open('file.txt')
f.read()
# do something
f.close()
```

If you get an exception while reading the file, the file will not be closed. It will lead to a resource leak.
To avoid this, you can use a `try`/`finally` block to close the file:

```python
try:
    f = open('file.txt')
    f.read()
    # do something
finally:
    f.close()
```

However, this code is not very readable. It is hard to tell what resources are being managed.

## 2. Using `with`

The with statement automatically handles opening and closing:

```python
with open('file.txt') as f:
    f.read()
    # do something
```

By using `with`, you can be sure that the file is closed when you are done with it. The code is also more readable.

## 3. How It Works: Context Managers

The `with` statement is implemented using a context manager. A context manager is a class that implements the `__enter__` and `__exit__` methods.

```python
class ManagedFile:
    def __init__(self, filename):
        self.filename = filename

    def __enter__(self):
        self.file = open(self.filename)
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        self.file.close()

with ManagedFile("example.txt") as f:
    data = f.read()
```

- `__enter__` is called when the `with` statement is entered. It returns the file object.
- `__exit__` is called when the `with` statement is exited. It closes the file.

## 4. Practical Example

Usually we use python decorator `contextmanager` to create a context manager for convenience. Here is a practical example of using `with` and context managers to manage a TV. It uses a context manager to turn on the TV when the `with` statement is entered and turn off the TV when the `with` statement is exited.

```python
from contextlib import contextmanager

class TV:
    def __init__(self, name):
        self.name = name
        self.is_on = False

    def turn_on(self):
        if not self.is_on:
            print(f"{self.name} is now ON. Enjoy your time!")
            self.is_on = True

    def turn_off(self):
        if self.is_on:
            print(f"{self.name} is now OFF. Goodbye!")
            self.is_on = False

@contextmanager
def open_tv(tv: TV):
    tv.turn_on()           # __enter__
    try:
        yield tv           # User can do something with the tv
    finally:
        tv.turn_off()      # __exit__

# 使用示例
my_tv = TV("Living Room TV")

with open_tv(my_tv) as tv:
    print("🎬 Watching a movie...")
    print("🎮 Playing a game...")
```

The output will be:

```bash
Living Room TV is now ON. Enjoy your time!
🎬 Watching a movie...
🎮 Playing a game...
Living Room TV is now OFF. Goodbye!
```

## 5. Summary
The with statement is a powerful feature in Python that simplifies resource management. It ensures that resources are properly acquired and released, even when exceptions occur, which makes your code cleaner, safer, and more Pythonic.

**Key takeaways:**
- File operations
- Database connections
- Locks in multithreading
- Temporary resources
