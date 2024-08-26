# Git and GitHub Cheat Sheet

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Overview](#1-Overview)
- [2 General](#2-general)
- [3 Destructive Actions](#3-destructive-actions)
    - [3.1 Reset](#31-reset)
        - [3.1.1 Move to Commit](#311-move-to-commit)
        - [3.1.2 Move to Branch Tip Reference](#312-move-to-branch-tip-reference)
        - [3.1.3 Flags](#313-flags)
    - [3.2 Amend](#32-amend)
        - [3.2.1 Flags](#321-flags)
    - [3.3 Pushing Destructive Changes](#33-pushing-destructive-changes)
- [4 Submodule](#411-results-gitmodules)
    - [4.1 Add Submodule](#41-add-submodule)
        - [4.1.1 Results (.gitmodules)](#411-results-gitmodules)
    - [4.2 Clone with Submodules](#42-clone-with-submodules)
    - [4.3 Fetch Submodule Commits](#43-fetch-submodule-commits)
    - [4.4 Pull Submodule](#44-pull-submodule)
    - [4.5 Update Submodule](#45-update-submodule)
    - [4.6 Remove Submodule](#46-remove-submodule)

</details>

---

## 1 Overview

### 1.1 Format

Commands are written in the following format.

````
```shell
git ... # Comment.
```
````

---

## 2 General

```shell
git commit -m "Add new xyz feature"
git push
```

---

## 3 Destructive Actions

### 3.1 Reset

#### 3.1.1 Move to Commit

To revert HEAD to a previous commit without the commit ID (also to Force Revert
commits).

```shell
git reset --hard HEAD^  # Reset to 1 commit back, add `^` for each additional commit.
git reset --hard <commit_hash>  # Reset to specific commit hash.
```

#### 3.1.2 Move to Branch Tip Reference

This resets the current branch's tip to the specified branch. In other words, it
makes the current branch point to the same commit as the target branch.

```shell
git reset --hard origin/<branch>  # Target to match head to remote branch.
git reset --hard <branch>  # Target to match head to local branch.
```

#### 3.1.3 Flags

`--soft`: Does not touch the index file or the working tree, but resets the head
to <commit>. Changes from <commit> onwards are preserved in the working
directory and as staged changes.

`--mixed`: Resets the index but not the working tree (i.e., the changed files
are preserved but not marked for commit) and reports what has not been updated.
This is the default action if no mode is specified.

`--hard`: Resets the index and working tree. Any changes to tracked files in the
working tree since <commit> are discarded.

`--merge`: Resets the index and updates the files in the working tree that are
different between <commit> and HEAD, but keeps those which are different between
the index and working tree (i.e., which have changes which have not been added).
If a merge conflict arises, it needs to be resolved manually.

`--keep`: Resets index entries and updates files in the working tree that are
different between <commit> and HEAD. If a file that is different between
<commit> and the index has unstaged changes, reset is aborted.

### 3.2 Amend

Amend changes the most recent commit. A new commit is created and the old is
removed. Thus this is a destructive action.

```shell
git add ./<changes> # Stage the changes to add.
git commit --amend -m <"New commit message">
```

#### 3.2.1 Flags

`--no-edit`: Keeps the same commit message as before. The commit message
flag `-m` and the argument can be skipped.

### 3.3 Pushing Destructive Changes

⚠️ Danger zone!

```shell
git push -f
```

---

## 4 Submodule

### 4.1 Add Submodule

```shell
git submodule add <remote_origin> <destination_path>
```

- `<destination_path>` is optional.

When adding a Git submodule, the new submodule will automatically be
staged. To commit and push, standard procedure can be followed.

### 4.1.1 Results (.gitmodules)

Following a new Git submodule commit, multiple actions will be performed
automatically:

1. A new directory is created in the repository named after the submodule added.
2. A `.gitmodules` file is created in the Git repository.
    - Contains the references to the remote repositories acting as submodules.
3. `.git/config` is modified to include the added submodule.

### 4.2 Clone with Submodules

```shell
git clone --recursive <https://github.com/github/.github>  # GitHub repo.
```

### 4.3 Fetch Submodule Commits

```shell
cd repository/submodule # Enter submodule directory.
git fetch
```

### 4.4 Pull Submodule

```shell
git submodule update # --init --recursive
```

### 4.5 Update Submodule

```shell
git submodule update # --remote --merge
```

- If you do not use the `–remote` flag, a manual `git pull` within each
  submodule will be required.

### 4.6 Remove Submodule

```shell
git submodule deinit <submodule> # Update .git/config.
git rm <submodule> # Remove submodule files.
```
