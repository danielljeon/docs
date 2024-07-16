# Python Code Conventions

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Base Reference Conventions](#1-base-reference-conventions)
    - [1.1 Google Style](#11-google-style)
        - [1.1.2 Pros](#112-pros)
        - [1.1.3 Cons](#113-cons)
- [2 Base Reference Conventions Modifications / Extended Specifications](#2-base-reference-conventions-modifications--extended-specifications)
    - [2.1 Python Version](#21-python-version)
        - [2.1.2 Pros](#212-pros)
        - [2.1.3 Cons](#213-cons)
        - [2.1.4 Exceptions](#214-exceptions)
    - [2.2 Auto-Formatter](#22-auto-formatter)
        - [2.2.1 Pros](#221-pros)
        - [2.2.2 Cons](#222-cons)
        - [2.2.3 Integration](#223-integration)

</details>

---

## 1 Base Reference Conventions

### 1.1 Google Style

All python code should be written in compliance to
the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)

#### 1.1.2 Pros

A commonly followed industry standard and well documented.

#### 1.1.3 Cons

Difficult for newer coders and takes time to learn.

---

## 2 Base Reference Conventions Modifications / Extended Specifications

### 2.1 Python Version

All code should be written for a python version of `3.11` at a minimum.

#### 2.1.2 Pros

A commonly followed industry standard and well documented.

#### 2.1.3 Cons

Difficult for newer coders and takes time to learn.

#### 2.1.4 Exceptions

Using an older version of python is allowed only when modifying forks / reusing
publicly available code which was originally written in an older version.

## 2.2 Auto-Formatter

Use the [Black](https://github.com/psf/black) formatter for auto formatting.

#### 2.2.1 Pros

Convenient, uncompromising and a commonly followed industry standard.

#### 2.2.2 Cons

Takes time to set up.

#### 2.2.3 Integration

For best IDE integration practices, see your IDE's official documentation and
Black's
official [Editor integration](https://black.readthedocs.io/en/stable/integrations/editors.html)
documentation.

- PyCharm by
  JetBrains: [Reformat Python code with Black](https://www.jetbrains.com/help/pycharm/reformat-and-rearrange-code.html#format-python-code-with-black)
