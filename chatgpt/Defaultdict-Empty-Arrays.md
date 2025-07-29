---
Source: https://chatgpt.com/c/68336bd8-20fc-8008-97e7-69ba57205827
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Defaultdict Empty Arrays

#### You:
python dictionary with empty arrays as default for all keys

#### ChatGPT:
To create a Python dictionary where all keys have **empty arrays (lists)** as their default value, you can use `collections.defaultdict`:

```python
from collections import defaultdict

my_dict = defaultdict(list)
```

### Example:

```python
my_dict['a'].append(1)
my_dict['b'].append(2)

print(my_dict)
# Output: defaultdict(<class 'list'>, {'a': [1], 'b': [2]})
```

In this example:

- `my_dict['a']` automatically starts as an empty list `[]`.
- You can use `.append()` or any list method without first checking or initializing the key.

Let me know if you want to convert this into a regular `dict` later or need it for a specific use case!
