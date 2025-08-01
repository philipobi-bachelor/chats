# Doxygen Cheat Sheet

*From Opensource.com (Stephan Avenwedde, CC BY-SA 4.0)*

---

## Running Doxygen

- `doxygen`  
  Run Doxygen to create documentation from annotated code.
- `doxygen -g`  
  Create a Doxygen template (generates a default `Doxyfile`).
- `doxygen -x`  
  Compare an existing `Doxyfile` against the template file.

---

## Comment Blocks

Doxygen supports several comment-block styles. Everything between the start and end markers is treated as documentation.

| Syntax       | Description                         |
| ------------ | ----------------------------------- |
| `///`        | Line comment **above** the code element. |
| `///<`       | Line comment **after** the code element. |
| `/** ... */` | Multiline documentation comment block.    |


Within any block you can use `\<command>` or `@<command>` to invoke Doxygen commands.

Example:

```cpp
/*!
 * A brief description of the foo() function
 *
 * This is a detailed description of the foo()
 * function, which can span several lines.
 *
 * @param bar  Description of the one argument named 'bar'
 */
void foo(int bar);
```

---

## Groups

Use groups to collect related items in the generated docs.

- `{@`\
  Mark the beginning of a group (inside a comment block).
- `@}`\
  Mark the ending of a group (inside a comment block).
- `@name`\
  Specify the name of the group.

---

## Special Commands

Use these inside comment blocks to control structure and cross-referencing:

- `@brief`\
  Mark a paragraph as the brief description.
- `@param[in/out]`\
  Describe input/output parameters of a function or method.
- `@tparam`\
  Describe a template parameter.
- `@mainpage`\
  Mark a comment block as the main page (becomes `index.html`).
- `@section`\
  Start a new section in a long comment block.
- `@subsection`\
  Start a subsection.
- `@ref`\
  Create a reference to a named section, subsection, page, or anchor.
- `@internal`\
  Generate documentation only when `INTERNAL_DOCS=yes` in `Doxyfile` (useful for private members).

---

## Commands for Arbitrary Positioning

These commands can appear anywhere, not just in special blocks:

- `@class` Document a C++ class
- `@struct` Document a C struct.
- `@union` Document a union
- `@enum` Document a enumeration type.
- `@fn` Document a function.
- `@var` Document a variable.
- `@def` Document a `#define`.
- `@typedef` Document a type definition.
- `@file` Document a file.
- `@namespace` Document a namespace.

Example:

```cpp
/*! 
 * @fn size_t ByteStream::size() const
 * @brief Returns the size of the used memory buffer
 *
 * The function behaves identically for both
 * internal and external buffers.
 */
```

---
