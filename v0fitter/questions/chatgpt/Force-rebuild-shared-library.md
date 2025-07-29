---
Source: https://chatgpt.com/c/684f469d-d2dc-8008-bb1a-ccab9528f6fd
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Force rebuild shared library

#### You:
how to force scons to rebuild a specific shared library

#### ChatGPT:
To force **SCons** to rebuild a specific **shared library**, you have a few options depending on the level of control you want. Here are the most common and effective methods:

---

### ✅ **1. Use `-c` to clean the specific target**

You can first clean the target (e.g., `libfoo.so`) and then build again:

```bash
scons -c build/libfoo.so
scons build/libfoo.so
```

This explicitly deletes and rebuilds the shared library.

---

### ✅ **2. Use `touch` on a source file or header**

If you don’t want to clean but want to force a rebuild due to a perceived change:

```bash
touch src/foo.cpp
scons build/libfoo.so
```

This will cause SCons to think `src/foo.cpp` has changed and rebuild `libfoo.so`.

---

### ✅ **3. Use `Depends()` or `AlwaysBuild()` in the SConstruct/SConscript**

You can programmatically control rebuilds:

#### a. **Use `AlwaysBuild()` to force rebuild every time**

```python
lib = SharedLibrary('foo', ['foo.cpp'])
AlwaysBuild(lib)
```

This ensures `libfoo.so` is rebuilt on every `scons` invocation.

#### b. **Force rebuild once with a dummy dependency**

You can add a phony dependency that you `touch` when you want to force a rebuild:

```python
phony = File('force_rebuild')
lib = SharedLibrary('foo', ['foo.cpp'])
Depends(lib, phony)
```

Then just run:

```bash
touch force_rebuild
scons build/libfoo.so
```

---

### ✅ **4. Use the `--touch` flag to force SCons to consider all targets out-of-date**

This is a global rebuild method (for all targets):

```bash
scons --touch build/libfoo.so
```

Then run SCons again:

```bash
scons build/libfoo.so
```

---

### ✅ **5. Use `--implicit-deps-changed` or `--question` for build analysis**

If you suspect SCons isn't detecting dependency changes, you can test:

```bash
scons --question build/libfoo.so
```

To see if it would rebuild.

---

Let me know if you're using variant directories or multiple SConscript files — I can tailor the approach!
