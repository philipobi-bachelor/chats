---
Source: https://chatgpt.com/c/6834a07a-27c4-8008-b4cc-78ca6a511efe
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Check named pipe status

#### You:
how to check if named pipe has been closed python

#### ChatGPT:
In Python, detecting whether a **named pipe (FIFO)** has been closed (i.e., the other end is no longer connected) depends on whether you're reading from or writing to the pipe.

Here's how to handle both cases:

---

### ✅ Case 1: **Reading** from a named pipe

If you're **reading** from a named pipe and the **writer has closed the pipe**, `read()` or `readline()` will return an **empty string (`''`)**, which indicates **EOF (End Of File)**.

```python
with open('mypipe', 'r') as fifo:
    while True:
        data = fifo.readline()
        if data == '':
            print("Writer closed the pipe.")
            break
        print("Received:", data.strip())
```

---

### ✅ Case 2: **Writing** to a named pipe

If you're **writing** to a named pipe and the **reader closes the pipe**, the next attempt to `write()` will raise a `BrokenPipeError`.

```python
try:
    with open('mypipe', 'w') as fifo:
        while True:
            fifo.write("Hello\n")
            fifo.flush()
except BrokenPipeError:
    print("Reader closed the pipe.")
```

---

### 🛠️ Non-blocking Mode (optional advanced)

If you need to check status without blocking, you can use `os.open()` with flags like `os.O_NONBLOCK`, and then use `select.select()` or `os.read()` and handle `BlockingIOError`.

Let me know if you're working in a **multiprocessing** or **async** context — the strategy changes slightly.


#### You:
in python how to keep fifo open, read from it as long as data is being written and then preform some action after the fifo is closed, then wait for new data

#### ChatGPT:
To **continuously read from a named pipe (FIFO)** in Python while:

1. Keeping it open as long as data is being written,
2. Performing an action **after the writer closes the pipe**, and then
3. Waiting for **new writers to connect** and repeat,

you can follow this general pattern:

---

### ✅ Full Working Example

```python
import os
import time

FIFO_PATH = '/tmp/myfifo'

# Create the FIFO if it doesn't exist
if not os.path.exists(FIFO_PATH):
    os.mkfifo(FIFO_PATH)

def process_data(data):
    print(">> Processing data block:")
    print(data)

while True:
    print("Waiting for writer to open the FIFO...")
    
    with open(FIFO_PATH, 'r') as fifo:
        buffer = ''
        while True:
            line = fifo.readline()
            if line == '':
                # Writer closed the pipe (EOF)
                break
            buffer += line
        
        if buffer:
            process_data(buffer)
        else:
            print("Writer connected and closed without sending data.")

    # Now the writer is gone, loop restarts to wait for a new one
    print("Writer disconnected. Ready for the next writer.\n")
```

---

### 🔍 What's Happening:

- `open(FIFO_PATH, 'r')` blocks until a **writer** connects.
- `readline()` returns empty string (`''`) when the **writer closes** the pipe (EOF).
- After detecting EOF, we perform an action on the accumulated data.
- The loop restarts to **wait for new writers**.

---

### ⚠️ Notes

- Don't use `open(FIFO_PATH, 'r')` in **non-blocking** mode unless you're handling `BlockingIOError`.
- Named pipes are **synchronous**: `open(..., 'r')` blocks until there's a **writer**, and vice versa for `open(..., 'w')`.

Let me know if you want an async version, or if your use case involves **binary data**, **multiple writers**, or **non-blocking I/O**.
