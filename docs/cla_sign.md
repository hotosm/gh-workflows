# Doxygen Build

Ensure contributors sign to agree to contribution guidelines, while making a PR.

## Inputs

## Outputs

## Secrets

## Example Usage

```yaml
name: 📜 Contribution Guidelines

on:
  issue_comment:
    types: [created]
  pull_request_target:
    types: [opened, closed, synchronize]

permissions:
  actions: write
  contents: write
  pull-requests: write
  statuses: write

jobs:
  sign:
    uses: hotosm/gh-workflows/.github/workflows/cla_sign.yml@3.4.0
    secrets: inherit
```
