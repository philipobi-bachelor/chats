---
Source: https://chatgpt.com/c/683b2a80-d138-8008-9a04-7a628bbaf042
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Debug C++ with GDB

#### You:
how to debug c++ executable in vscode using gdb

#### ChatGPT:
To debug a C++ executable in **VS Code** using **GDB**, you need to configure your project with the correct launch settings and ensure that your binary is compiled with debugging symbols. Here's a complete step-by-step guide:

---

### ✅ 1. **Install Requirements**

Make sure the following are installed:

- **G++** (or another compiler that supports `gdb`)
- **GDB**
- **Visual Studio Code**
- **C/C++ Extension** by Microsoft\
  *(Search for "C/C++" in the Extensions view and install it)*

---

### ✅ 2. **Compile with Debug Symbols**

You **must compile your code with `-g` flag** to include debug symbols:

```bash
g++ -g main.cpp -o my_program
```

> Without `-g`, GDB will not provide line-by-line debugging or variable names.

---

### ✅ 3. **Create a `launch.json` File**

1. Open your project folder in VS Code.
2. Go to the **Run and Debug** panel (Ctrl+Shift+D).
3. Click **"create a launch.json file"**.
4. Select **C++ (GDB/LLDB)**.
5. Replace the generated `launch.json` with the following (or adjust it):

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug C++ Executable",
      "type": "cppdbg",
      "request": "launch",
      "program": "${workspaceFolder}/my_program",  // <-- path to your compiled executable
      "args": [],                                   // <-- optional: pass CLI arguments
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,                     // true if you want a separate terminal
      "MIMode": "gdb",
      "setupCommands": [
        {
          "description": "Enable pretty-printing",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ]
    }
  ]
}
```

---

### ✅ 4. **Set Breakpoints**

- Open your `.cpp` file.
- Click to the **left of the line number** to set a breakpoint (a red dot will appear).

---

### ✅ 5. **Start Debugging**

- Go to the **Run and Debug** tab.
- Select your configuration (e.g., "Debug C++ Executable").
- Click the **green play button** ▶️ or press `F5`.

---

### 🔍 Tips for Effective Debugging

- **Step Over**: `F10`
- **Step Into**: `F11`
- **Continue**: `F5`
- **Watch Variables**: Use the "Watch" panel
- **Inspect Stack**: Use "Call Stack"
- **Variables Panel**: View current variable values

---

Let me know if you want to attach GDB to a **running process** or use a **Makefile or CMake** workflow — setup changes slightly.
