# NPM Publish

Publish a package to NPMJS using PNPM.

## Inputs

## Outputs

## Secrets

## Example Usage

```yaml
name: Publish to npmjs.org

on:
  release:
    types: [published]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  publish_to_npmjs:
    uses: hotosm/gh-workflows/.github/workflows/pnpm_publish.yml@main
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```
