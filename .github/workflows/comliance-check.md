---
description: |
  This workflow checks if a PR is compliant with all open source software compliance guidelines.
  It adds a comment to the PR with results of the guidelines it checked.

on:
  pull_request:
    branches:
      - main

permissions:
  contents: read
  pull-requests: read

network: defaults

tools:
  github:
    toolsets: [repos, pull_requests]
    # If in a public repo, setting `lockdown: false` allows
    # reading issues, pull requests and comments from 3rd-parties
    # If in a private repo this has no particular effect.
    lockdown: false
    min-integrity: approved

safe-outputs:
  mentions: false
  add-comment:
    max: 5
    discussions: true
    hide-older-comments: true
    allowed-reasons: [outdated]
    footer: true
    normalize-closing-keywords: true
  allowed-github-references: []
  allowed-domains: []
---

# open source software compliance check

this workflow validates if the repository is compliant with the guidelines listed below.  
at the end of of validating all guidelines, the workflow adds a comment to the PR that triggered the workflow, listing the results of the guideline checks, in a format like this:

```md
| Guideline             | Result                     |
| --------------------- | -------------------------- |
| **Management**        |                            |
| adopt file structure  | ❌ missing files           |
| specify repo type     | ✅                         |
| **Category 2**        |                            |
| Reproducibility       | ✅                         |
| Assure code ownership | ⚠️ needs manual validation |

<!-- list of all guidelines which the repo is not compliant with, or that can't be checked automatically -->

## ❌ adopt file structure

the repo is missing one or more of the required files.

<!--- list missing files here -->

## ⚠️ Assure code ownership

you need to manually validate that every contributor has signed the CLA (Contributor Licence Agreement).
```

and the workflow hides previous comments it made (if any) on that same PR.

## compliance guidelines

### adopt file structure

the repo must have the following files:

- `/compliance-doc/README.md`
- `/.github/workflows/build.yml`
- `/.github/CODEOWNERS`
- `/README.md`

### specify repo type

the `compliance-docs/README.md` file must contain a table under the `## repo type` chapter in the following format:

```md
| repo type    | repo maturity |
| ------------ | ------------- |
| <value here> | <value here>  |
```

`repo type` must be one of these values:

- documentation
- demo
- application
- library

`repo maturity` must be one of these values:

- completed and in maintenance
- actively in development
- archived

### Reproducibility

If the repo type is `application` or `library`, then check if the `/README.md` contains working instructions on how to build the code.

### Assure code ownership

the workflow **can't** automatically check this guideline.  
always list `Assure code ownership` as `⚠️ needs manual validation` in the guideline check summary comment on the PR.  
just like in the example format above.
