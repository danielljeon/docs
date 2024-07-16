# Python Development Environment

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Overview](#1-overview)
    - [1.1 Install](#11-install)
    - [1.2 System Environment Variable](#12-system-environment-variable)
    - [1.3 Python Virtual Environment](#13-python-virtual-environment)
- [2 Windows](#2-windows)
    - [2.1 Install](#21-install)
        - [2.1.1 Python](#211-python)
    - [2.2 Manual Virtual Environment Creation](#22-manual-virtual-environment-creation)
- [3 macOS](#3-macos)
    - [3.1 Install](#31-install)
        - [3.1.1 Homebrew](#311-homebrew)
        - [3.1.2 pyenv](#312-pyenv)
        - [3.1.3 Python via pyenv](#313-python-via-pyenv)
    - [3.2 Manual Virtual Environment Creation](#32-manual-virtual-environment-creation)

</details>

---

## 1 Overview

The following document covers the installation and setup for python to get
started with other IDEs and dev envs.

### 1.1 Install

### 1.2 System Environment Variable

### 1.3 Python Virtual Environment

Normally the python virtual environment creation is handled via the IDE, but if
for some reason you want to manually create it and/or learn how it is done,
follow the manual setup instructions.

---

## 2 Windows

### 2.1 Install

#### 2.1.1 Python

Install python directly from the official website:

[Python downloads](https://www.python.org/downloads/).

It is **highly recommended to select / allow the `set environment variable`** (
might be slightly different wording) setting for system-wide activation.

### 2.2 Manual Virtual Environment Creation

```shell
python -m venv /path/to/new/virtual/environment  # Create venv.
```

> For official docs, see:
> [https://docs.python.org/3/library/venv.html#module-venv](https://docs.python.org/3/library/venv.html#module-venv).

---

## 3 macOS

### 3.1 Install

The following documentation is based on recommendations, there are multiple
methods for installation.

#### 3.1.1 Homebrew

See [homebrew.md](homebrew.md).

#### 3.1.2 pyenv

[github.com/pyenv/pyenv](https://github.com/pyenv/pyenv).

```shell
brew install pyenv  # Install pyenv.
```

#### 3.1.3 Python via pyenv

```shell
pyenv install 3.12.3  # Install python version 3.12.3.
pyenv uninstall 3.12.3  # Uninstall python version.

pyenv global 3.12.3  # Set version for active global use.
pyenv local 3.12.3  # Set version for active local use.
pyenv shell 3.12.3  # Set version for active shell use.
# Activation levels from highest to lowest is: global, local, shell.

pyenv version  # Show current active python version.

pyenv versions  # Show all installed python versions.
# The * points to the version currently active.

pyenv which python  # Show current active python location.
```

### 3.2 Manual Virtual Environment Creation

```shell
ls  # Move to the target venv location.

pyenv virtualenv <python_version> <environment_name>  # Create venv.

pyenv local <environment_name>  # Activate venv as the local environment.

pyenv which python  # Verify active python venv location.
pyenv which pip  # Verify active pip (part of venv) location.

pyenv activate <environment_name>  # Manually activate venv (if using multiple).
pyenv deactivate  # Deactivate current venv.
```
