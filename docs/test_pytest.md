# PyTest (without Docker Compose)

Run pytest for your application, inside a container.

## Prerequisites

1. The tests must be included in the image (via Dockerfile).

2. PyTest must be installed for the dockerfile USER.

## Inputs

## Outputs

## Secrets

## Example Usage

Running on push and PR:

```yaml
name: pytest

on:
  # Run tests on all pushed branches
  push:
    branches:
      - "*"
  # Run tests on PR, prior to merge to main & development
  pull_request:
    branches:
      - main
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  pytest:
    uses: hotosm/gh-workflows/.github/workflows/test_pytest.yml@main
    with:
      image_name: ghcr.io/${{ github.repository }}
      build_args: |
        COMMIT_REF=${{ github.sha }}
```
