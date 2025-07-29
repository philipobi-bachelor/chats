---
Source: https://chatgpt.com/c/683069e9-11ac-8008-8f68-42906a20b768
Model: o4-mini
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Append LESS variable option

#### You:
how to append an option to the LESS environment variable

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
In most shells you simply re‑export `LESS`, tacking your new flags on to whatever was already there. For example, to add the `-R` flag (allow raw control‐chars) in **bash**/ **zsh**:

```bash
# append "-R" to whatever LESS already contains (if anything)
export LESS="${LESS:+$LESS }-R"
```

If `$LESS` was empty, that expands to just `-R`; otherwise it becomes, e.g., `-iMS --long-prompt -R`.

To make it permanent, add that line to your `~/.bashrc` or `~/.zshrc`.

---

#### Other shells

- **fish**
  ```fish
  # prepend or append; here we append
  set -x LESS $LESS -R
  ```

- **tcsh/csh**
  ```csh
  # append "-R"
  setenv LESS "${LESS} -R"
  ```

---

#### A more robust “only add if not already present” trick (bash/zsh)

If you’d like to guard against adding duplicates, you can do:

```bash
case " $LESS " in
  *" -R "*) ;;
  *) export LESS="${LESS:+$LESS }-R" ;;
esac
```

This checks for `-R` in the current value and only appends it if missing.
