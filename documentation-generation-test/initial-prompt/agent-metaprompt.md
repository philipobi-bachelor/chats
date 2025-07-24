---
Source: https://chatgpt.com/c/686c660f-ed58-8008-9c8b-e9406c886f01
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Improve AI Documentation Prompt

#### You:
Can this prompt which should be fed to an AI agent be improved?
```
Please add documentation to #file:NewV0Fitter.h  using the Doxygen syntax which is also detailed in #file:doxygen-overview.md and follow the documentation conventions layed out in the provided section of coding_conventions.rst. The implementation of the declarations in #file:NewV0Fitter.h  can be found in #file:NewV0Fitter.cc, however this file should not be changed. Please write concise and meaningful documentation and avoid producing too much text. Only modify the code by adding comments, do not change the logic.
```

#### ChatGPT:
Yes, your prompt is already clear, but it can be improved for **conciseness**, **tone**, and **precision**—especially if it's going to be used repeatedly or fed into an AI system. Here's a refined version:

---

### ✅ Improved Prompt

```
Add Doxygen-style documentation to #file:NewV0Fitter.h. Follow the conventions outlined in #file:doxygen-overview.md and the relevant section of coding_conventions.rst. Refer to #file:NewV0Fitter.cc for implementation details, but do not modify it. Keep the documentation concise, meaningful, and strictly comment-based—do not alter any code logic.
```

---

### 🔍 Key Improvements

- **Simplified structure**: Reduced wordiness ("please add documentation to..." → "Add Doxygen-style documentation to...").
- **Stronger action verbs**: More directive language helps AI models focus.
- **Clearer constraints**: "Strictly comment-based" and "do not alter any code logic" clarify the modification scope.
- **Consistent formatting**: Removed redundant spacing and clarified filenames.

Would you like a version that's optimized for a specific LLM system (e.g., OpenAI function-calling agents, Code Llama, Claude, etc.)?
