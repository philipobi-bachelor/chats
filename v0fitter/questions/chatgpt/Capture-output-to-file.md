---
Source: https://chatgpt.com/c/684c928e-0634-8008-b5c7-5cb8a9339de5
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Capture output to file

#### You:
capture all output of a shell script i.e. stderr and stdout into file and display it in the terminal as well

#### ChatGPT:
To **capture both stdout and stderr** of a shell script into a file **and also display it in the terminal**, you can use the `tee` command with redirection.

Here’s the canonical way to do it:

```bash
#!/bin/bash

# Capture all output (stdout and stderr), write to file, and display in terminal
exec > >(tee -a output.log) 2>&1

# Your script content below
echo "This is stdout"
ls /nonexistent_directory  # This will produce stderr
```

### Explanation:

- `exec > >(tee -a output.log) 2>&1`:
  - Redirects **stdout (`>`)** to a process substitution `>(tee -a output.log)` which:
    - Appends to `output.log`.
    - Also sends the output to the terminal.
  - `2>&1` redirects **stderr** to the same place as **stdout**, ensuring both go through `tee`.

### Bonus Tip:

If you're calling a script and want to do this **from the outside**, not modifying the script itself:

```bash
./yourscript.sh 2>&1 | tee -a output.log
```

Let me know if you need it for a specific shell or context (e.g., cron, systemd, etc.).
