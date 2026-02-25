# Doxygen Build

Ensure contributors sign to agree to contribution guidelines, while making a PR.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

No inputs.

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

No outputs.

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

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
    uses: hotosm/gh-workflows/.github/workflows/cla_sign.yml@3.5.0
    secrets: inherit
```
