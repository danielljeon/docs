# Git and GitHub DevOps Conventions

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Background](#1-background)
- [2 Git Setup](#2-git-setup)
    - [2.1 Repositories](#21-repositories)
        - [2.1.1 Permissions](#211-permissions)
        - [2.1.2 Naming](#212-naming)
    - [2.2 Branches](#22-branches)
    - [2.3 Common/Required files](#23-commonrequired-files)
        - [2.3.1 .gitignore*](#231-gitignore)
        - [2.3.2 README.md*](#232-readmemd)
        - [2.3.3 documentation/](#233-documentation)
        - [2.3.4 tests/](#234-tests)
- [3 Getting to main](#3-getting-to-main)
    - [3.1 History](#31-history)
    - [3.2 Commits](#32-commits)
    - [3.3 Pushes](#33-pushes)
        - [3.3.1 Force Pushes](#331-force-pushes)
    - [3.4 Merge/Rebase Branch](#34-mergerebase-branch)
        - [3.4.1 General Recommendation](#341-general-recommendation)
- [4 Documentation](#4-documentation)
- [5 GitHub Setup](#5-github-setup)
    - [5.1 Organization, Repo & Team Permissions](#51-organization-repo--team-permissions)
    - [5.2 Rulesets](#52-rulesets)
    - [5.3 .github/ Directory](#53-github-directory)
        - [5.3.1 CODEOWNERS*](#531-codeowners)
        - [5.3.2 Issue & Pull Request Templates](#532-issue--pull-request-templates)
    - [5.4 Labels](#54-labels)
    - [5.5 Issues & Pull Requests (PR)](#55-issues--pull-requests-pr)
        - [5.5.1 Creation](#551-creation)
        - [5.5.1.1 Name and Description](#5511-name-and-description)
        - [5.5.1.2 Labels](#5512-labels)
        - [5.5.1.3 Create Issue Reference Branch](#5513-create-issue-reference-branch)
        - [5.5.1.4 Assignment](#5514-assignment)
    - [5.5.2 Conversation](#552-conversation)
    - [5.5.3 Close](#553-close)
        - [5.5.3.1 Close Completed](#5531-close-completed)
        - [5.5.3.2 Close Inactive](#5532-close-inactive)

</details>

---

## 1 Background

DevOps are crucial for maintaining version control and managing production. The
following conventions are outlined to maintain good engineering practices, clear
documentation and formatting.

---

## 2 Git Setup

Version control is done through git.

### 2.1 Repositories

Each project is version controlled through one repo.

#### 2.1.1 Permissions

All new repos should be **private** unless cleared and agreed to by team
leadership.

#### 2.1.2 Naming

All repos should use appropriate naming, if arbitrary default to snake_case.

### 2.2 Branches

`main` is always the default/production/final release branch.

### 2.3 Common/Required files

#### 2.3.1 .gitignore*

Always include one in the main directory.

- Recommended templates: [gitignores](../gitignores).

#### 2.3.2 README.md*

Always include one in the main directory for complete repository documentation.

#### 2.3.3 documentation/

If reference documentation files such as pictures are required, use this
directory.

#### 2.3.4 tests/

If reference tests scripts or files are required, use this directory.

---

## 3 Getting to main

Never push directly to main unless working alone.

### 3.1 History

Always maintain linear history on the main branch, and try to keep linear
history on lower branches.

### 3.2 Commits

Name your commits starting with a present tense verb or technical adverb.

- Examples:
    - `Add cool feature xyz`.
    - `Refactor rename variable sneaky`.
    - `Clean up formatting`.

### 3.3 Pushes

Make sure to fetch and pull to prevent merge errors before pushing.

#### 3.3.1 Force Pushes

Never force push unless you absolutely know what you are doing.

- If you're force pushing you already messed up another practice guideline
  earlier.

### 3.4 Merge/Rebase Branch

Use pull requests and merge/rebase up the branches to add changes to main.

The decision for merge, squash or rebase is specific case by case, always aim to
use the most appropriate for each situation.

Some core conditions to consider:

1. Impact to shared history.
2. Traceability of changes.
3. Visibility of changes.

#### 3.4.1 General Recommendation

1. Merge: General shared branch to shared branch.
2. Squash: Private branch with many changes to central development/release
   branch.
3. Rebase: Private branch to any target set as a base for changes.

---

## 4 Documentation

Always follow documentation conventions follow by and defined in this repo.

---

## 5 GitHub Setup

Git origin, org, projects, etc. are all set up through GitHub.

### 5.1 Organization, Repo & Team Permissions

Organizational access role security must always be maintained.

### 5.2 Rulesets

Rule sets outline general rules.

**main.json**:

```json
{
  "id": 123456,
  "name": "main",
  "target": "branch",
  "source_type": "Repository",
  "source": "danielljeon/docs",
  "enforcement": "active",
  "conditions": {
    "ref_name": {
      "exclude": [],
      "include": [
        "~DEFAULT_BRANCH",
        "refs/heads/main"
      ]
    }
  },
  "rules": [
    {
      "type": "deletion"
    },
    {
      "type": "non_fast_forward"
    },
    {
      "type": "creation"
    },
    {
      "type": "update"
    },
    {
      "type": "required_linear_history"
    },
    {
      "type": "pull_request",
      "parameters": {
        "required_approving_review_count": 1,
        "dismiss_stale_reviews_on_push": true,
        "require_code_owner_review": true,
        "require_last_push_approval": true,
        "required_review_thread_resolution": true
      }
    },
    {
      "type": "required_status_checks",
      "parameters": {
        "strict_required_status_checks_policy": true,
        "required_status_checks": [
          {
            "context": "black",
            "integration_id": 123456
          }
        ]
      }
    },
    {
      "type": "code_scanning",
      "parameters": {
        "code_scanning_tools": [
          {
            "tool": "CodeQL",
            "security_alerts_threshold": "high_or_higher",
            "alerts_threshold": "errors"
          }
        ]
      }
    }
  ],
  "bypass_actors": [
    {
      "actor_id": 1,
      "actor_type": "OrganizationAdmin",
      "bypass_mode": "always"
    }
  ]
}
```

### 5.3 .github/ Directory

Add the standard `.github` directory and files at the root as a project gets
more complex.

#### 5.3.1 CODEOWNERS*

Code owners are used to maintain required reviewer branch protection rules and
rulesets.

#### 5.3.2 Issue & Pull Request Templates

> See GitHub Docs:
> [About issue and pull request templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates)

### 5.4 Labels

Use default labels, unless there is a logical reason for changing them.

### 5.5 Issues & Pull Requests (PR)

Issues define problems, features, tasks and all general TODOs.

Pull requests are used for merging, rebasing and closing branches.

#### 5.5.1 Creation

Create an issue using the GitHub website or GitHub CLI on the applicable
repository.

> See GitHub Docs:
> [Creating an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue)

##### 5.5.1.1 Name and Description

Use appropriate naming and description.

The name should be simple and concise, use the description for full
documentation of the issue.

##### 5.5.1.2 Labels

Apply the appropriate default label(s).

##### 5.5.1.3 Create Issue Reference Branch

For best practices, when creating an issue, create a new reference branch. The
closing pull request should also reference the issue (see further).

> See GitHub Docs:
> [Creating a branch to work on an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-a-branch-for-an-issue)

##### 5.5.1.4 Assignment

If you are the primary responsible developer for the issue and/or pull request,
assign yourself as the assigned.

#### 5.5.2 Conversation

Keep the conversation related to the specific issue/pull request at hand. If you
need to reference another issue/pull request, reference directly for convenience
and clarity.

> See GitHub Docs:
> [Assigning issues and pull requests to other GitHub users](https://docs.github.com/en/issues/tracking-your-work-with-issues/assigning-issues-and-pull-requests-to-other-github-users)

#### 5.5.3 Close

Closing is required upon reaching a state of "no further activity" (complete or
inactive). There are 2 reasons for closure: complete and inactive.

##### 5.5.3.1 Close Completed

Closing is required upon completion and when all elements within the entire
conversation have been resolved.

All completed items must be closed with a commit directly referencing the issue,
and ideally be closed with a pull request of the issue branch if available.

> See GitHub Docs:
> [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)

##### 5.5.3.2 Close Inactive

Closing is required upon reaching an inactive point, when it is no longer valid
to continue contributing and should no longer be worked on.

Examples of these situations include, but are not limited to:

1. Duplicate.
   > See GitHub Docs:
   > [Marking issues or pull requests as a duplicate](https://docs.github.com/en/issues/tracking-your-work-with-issues/marking-issues-or-pull-requests-as-a-duplicate).
2. No longer required for development.
3. Can no longer be supported.

All inactive items must be closed by a final comment in the issue conversation
stating the reason for closure and documenting the final results of closure as
needed.
