# Markdown Documentation Conventions

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Background](#1-background)
- [2 Language Rules](#2-language-rules)
    - [2.1 Writing Form](#21-writing-form)
- [3 Style Rules](#3-style-rules)
    - [3.1 Line Length](#31-line-length)
        - [3.1.1 Exceptions](#311-exceptions)
        - [3.1.2 Pros](#312-pros)
        - [3.1.3 Cons](#313-cons)
    - [3.2 Indents](#32-indents)
    - [3.3 Headers](#33-headers)
        - [3.3.1 Numbering Format](#331-numbering-format)
        - [3.3.2 Writing Format](#332-writing-format)
        - [3.3.3 Horizontal Lines](#333-horizontal-lines)
        - [3.3.4 Section Links / Header References](#334-section-links--header-references)
    - [3.4 Standard Markdown Syntax](#34-standard-markdown-syntax)
        - [3.4.1 Advanced Formatting Syntax](#341-advanced-formatting-syntax)
    - [3.5 Code Blocks](#35-code-blocks)
    - [3.6 Relative Links](#36-relative-links)
    - [3.7 Math Expressions](#37-math-expressions)
        - [3.7.1 Inline](#371-inline)
        - [3.7.2 Blocks](#372-blocks)
- [4 Other Media](#4-other-media)
    - [4.1 Pictures](#41-pictures)
    - [4.2 Unicode Emojis](#42-unicode-emojis)
- [5 Common Content](#5-common-content)
    - [5.1 Table of Contents](#51-table-of-contents)

</details>

---

## 1 Background

Markdown is our primary documentation "language" for everything from
repository `README.md` docs, to resource docs and convention docs like this one.

---

## 2 Language Rules

All written non-coding / machine language is Canadian English.

- No oxford comma.
- Carry overs from British English like "colour".

### 2.1 Writing Form

Write in full sentences ending with a `.` or applicable punctuation.

---

## 3 Style Rules

### 3.1 Line Length

Max line length = `80`.

#### 3.1.1 Exceptions

Exceptions to the limit are:

- URLs, filepaths, etc.
- Long constants not containing whitespace that would be inconvenient to split.

#### 3.1.2 Pros

Conforms with most of our code conventions.

Matches most code generation tools, fits A4 print papers and typical maximum in
industry.

#### 3.1.3 Cons

None, mostly subjective.

### 3.2 Indents

Indent is 4 spaces.

### 3.3 Headers

#### 3.3.1 Numbering Format

Headers are numbered the following format.

```markdown
---

# N Header 1 (Title) Example Name Here

## N.N Header 2 Example Name Here

### N.N.N Header 3 Example Name Here

#### N.N.N.N Header 4 Example Name Here

---
```

- N is a number.

#### 3.3.2 Writing Format

Headers are named with standard `Title Formatting`.

#### 3.3.3 Horizontal Lines

Use horizontal line breaks `---` to separate long sections of writing.

The general recommendation is to separate on `Header level 1 & 2` to improve
readability.

#### 3.3.4 Section Links / Header References

Use header references when possible to make referencing easy.

See [5.1 Table of Contents](#51-table-of-contents).

### 3.4 Standard Markdown Syntax

```markdown
- Point form.

**Bold.**

_Italics_

**_Bold and Italics_**

---
```

#### 3.4.1 Advanced Formatting Syntax

For advanced syntax use as appropriate.

> See full documentation on GitHub
> Docs:
> [Working with advanced formatting](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting)

### 3.5 Code Blocks

If code blocks are used do your best to declare the language for clean
formatting and clarity of the intended language.

Follow the applicable code conventions for the code within the code block.

### 3.6 Relative Links

Unless specified, the relative link must always start at the root directory of
the repository.

> See full documentation on GitHub
> Docs:
> [Relative links](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#relative-links)

### 3.7 Math Expressions

#### 3.7.1 Inline

Use the inline format as shown below.

Inline: $x = \frac{-b \pm \sqrt{b^{2} - 4ac}}{2a}$.

```markdown
Inline: $x = \frac{-b \pm \sqrt{b^{2} - 4ac}}{2a}$.
```

#### 3.7.2 Blocks

Use the block format as shown below.

Block:

$$f' \left( x \right) = \lim_{ h \rightarrow 0 } \frac{f \left( x+h \right) - f
\left( x \right) }{h}$$

```markdown
$$f' \left( x \right) = \lim_{ h \rightarrow 0 } \frac{f \left( x+h \right) - f
\left( x \right) }{h}$$
```

---

### 4 Other Media

Links, pictures, etc are very helpful use them appropriately to write good
documentation.

#### 4.1 Pictures

Include a `pictures` directory at the highest root directory of the picture
reference.

#### 4.2 Unicode Emojis

- Unicode's emojis are allowed, but be sensible in usage.

---

## 5 Common Content

### 5.1 Table of Contents

Include a listed version of a table of contents for long documentations
following the format below.

```markdown
# Title Here

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [N Header 1 Example Name Here](#n-header-1-example-name-here)
    - [N.N Header 2 Example Name Here](#nn-header-2-example-name-here)
        - [N.N.N Header 3 Example Name Here](#nnn-header-3-example-name-here)
            - [N.N.N.N Header 4 Example Name Here](#nnnn-header-4-example-name-here)

</details>

---

...
```
