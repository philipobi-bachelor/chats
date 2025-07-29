---
Source: https://chatgpt.com/c/685c9ef9-ff10-8008-b8fc-9c8ee98c6e3f
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Collapsible Section in Markdown

#### You:
markdown collapsible section

#### ChatGPT:
In Markdown, you can create collapsible sections using HTML `<details>` and `<summary>` tags, which are supported by most Markdown renderers like GitHub, GitLab, and many static site generators.

### Example:

````markdown
<details>
  <summary>Click to expand</summary>

  This is the content inside the collapsible section.  
  You can put **Markdown** _formatting_ here too.

  - Bullet points
  - Images
  - Code blocks
  ```python
  print("Hello, world!")
````

</details>
```

### Rendered Output:

<details>
  <summary>Click to expand</summary>

This is the content inside the collapsible section.\
You can put **Markdown** *formatting* here too.

- Bullet points
- Images
- Code blocks

```python
print("Hello, world!")
```

</details>

Let me know if you're targeting a specific platform, as behavior can vary slightly.
