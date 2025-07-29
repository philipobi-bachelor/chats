---
Source: https://chatgpt.com/c/68595c98-d70c-8008-8ce6-4136334de6b4
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Break on vector exception

#### You:
how to break on this exception vector::_M_range_check: __n

#### ChatGPT:
The exception you're seeing:

```
vector::_M_range_check: __n
```

is a **runtime out-of-range exception** thrown by the C++ Standard Template Library (`std::vector`) when using the `at()` method with an invalid index.

### What this means

You're trying to access an element in a `std::vector` using `.at(index)`, and the index is either:

- negative (e.g., `-1`)
- greater than or equal to `vector.size()`

The method `at()` performs bounds checking and throws a `std::out_of_range` exception if the index is invalid. In contrast, `operator[]` (i.e. `vec[index]`) does **not** throw and leads to undefined behavior if misused.

---

### How to break on this exception

#### 🔧 If you're using **GDB** (GNU Debugger):

You can tell GDB to break on C++ exceptions.

```gdb
(gdb) catch throw
```

Then run your program. GDB will break when **any** exception is thrown. When it stops, type:

```gdb
(gdb) backtrace
```

to see the stack trace and understand where in your code the invalid index is being accessed.

To narrow down only to `std::out_of_range`, you can filter (available in some versions):

```gdb
(gdb) catch throw std::out_of_range
```

If your version of GDB doesn't support this, stick with the generic `catch throw`.

#### 🐞 If you're using **Visual Studio**:

1. Go to **Debug > Windows > Exception Settings**
2. Expand **C++ Exceptions**
3. Check the box next to `std::out_of_range`
4. Run your code. Visual Studio will break exactly when the exception is thrown.

#### 🧪 If you're using **LLDB** (Clang debugger):

```lldb
(lldb) breakpoint set -E c++
```

Or for a more specific match:

```lldb
(lldb) breakpoint set -E c++ -C std::out_of_range
```

---

### Pro Tips

- When debugging, always check `.size()` before accessing `.at(i)`.
- You can wrap access in a `try` block if you expect bad input:

```cpp
try {
    auto val = vec.at(i);
} catch (const std::out_of_range& e) {
    std::cerr << "Out of range: " << e.what() << '\n';
}
```

---

Would you like help narrowing down the exact location in your code this is happening? You can share a snippet.
