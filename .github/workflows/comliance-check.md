---
description: |
  This workflow checks if the repo is still compliant with all open source software compliance rules.
  It creates issues for everything non-compliant found.

on:
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

network: defaults

tools:
  github:
    # If in a public repo, setting `lockdown: false` allows
    # reading issues, pull requests and comments from 3rd-parties
    # If in a private repo this has no particular effect.
    lockdown: false
    min-integrity: none # This workflow is allowed to examine and comment on any issues

safe-outputs:
  mentions: false
  allowed-github-references: []
  create-issue:
    title-prefix: "[compliance-check] "
    labels: []
    close-older-issues: false
---

# open source software compliance check

validates if the repo contains anything non-compliant (things which should not be made public) according to the compliance rules listed below.
will create an issue for each rule the repo is not compliant with.

## compliance rules

### 1. repo must contain categorization info

the `.compliance/README.md` file must contain the categorization.  
the classification must be one of these:

- Static Research project
- POC project
- MVP project

### 2. repo must contain LICENSE file

The repository must contain information about the Outbound licence of the project code.
The preferred license for publication is `Apache-2.0`.
The license should be added as a `LICENSE` file in the root of the repository.
