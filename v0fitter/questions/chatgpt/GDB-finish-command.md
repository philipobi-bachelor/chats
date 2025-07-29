---
Source: https://chatgpt.com/c/683c6704-2cf0-8008-994d-d7d934798cd2
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# GDB finish command

#### You:
gdb continue until about to exit the function

#### ChatGPT:
To continue execution in GDB **until the current function is about to return**, you can use the `finish` command. This will run the program until the current function is about to exit, and then stop **just after** the return — displaying the return value if applicable.

### Usage:

```bash
(gdb) finish
```

### What it does:

- Continues execution until the **end of the current function**.
- Stops at the **caller of the current function** (right after the function returns).
- Shows the function's return value (if any).

### Example:

```c
int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(2, 3);
    return 0;
}
```

If you're at a breakpoint inside `add()`, and you do:

```bash
(gdb) finish
```

It will run until `add()` returns and stop right after in `main()`, showing:

```
Value returned is 5
```

---

If you instead want to stop **just before** the function returns (e.g. to inspect state), you'd have to set a **breakpoint at the return point**, or use a conditional breakpoint on the return line.

Let me know if you're debugging optimized code or working inlined functions — the behavior may differ slightly.


#### You:
grep return matches with line number

#### ChatGPT:
To use `grep` to search for lines containing `return` and show the **line numbers**, use the `-n` option:

```bash
grep -n return filename
```

### Example:

```bash
grep -n return mycode.c
```

### Output:

This shows lines like:

```
15:    return x;
42:    return 0;
```

### Additional useful options:

- `-r` or `-R`: Search recursively in directories.
- `-I`: Ignore binary files.
- `--include='*.c'`: Limit to specific file types.
- `-E`: Enable extended regex (for more complex patterns).

### Example (recursive search in `.c` files):

```bash
grep -nR --include="*.c" return .
```

Let me know if you want to match only `return` as a full word or filter comments.


#### You:
gdb print code of current function

#### ChatGPT:
To **print the source code of the current function** in GDB, use the `list` command with the function name. If you're already inside the function, you can do:

```bash
(gdb) list
```

This prints 10 lines around the current line.

To show the **entire current function**, use:

```bash
(gdb) list <function_name>
```

Or, if you're inside the function and want to be exact:

```bash
(gdb) list $
```

> `$` refers to the current frame/function — so this shows the code for the current function.

---

### Bonus Tips:

- To list more lines:
  ```bash
  (gdb) list ,+20  # Show 20 lines after current line
  ```

- To list lines from a specific range:
  ```bash
  (gdb) list 30,50
  ```

Let me know if you're debugging stripped binaries or need to extract the function code to a file.
