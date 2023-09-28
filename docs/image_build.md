# MKDocs Build

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

**Standalone**

```yaml
backend-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml
  with:
    context: src/backend
    build_target: prod
    image_tags: |
      "ghcr.io/hotosm/fmtm/backend:latest"
    build_args: |
      APP_VERSION=0.1.0
```

**Passing Variables**

Example variable extraction from FMTM:

```yaml
name: Extract Project Variables

on:
  workflow_call:
    inputs:
      environment:
        description: "The GitHub environment to extract vars from."
        type: string
        default: ""
    outputs:
      api_version:
        description: "Backend API Version."
        value: ${{ jobs.extract-vars.outputs.api_version }}

jobs:
  extract-vars:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    outputs:
      api_version: ${{ steps.extract_api_version.outputs.api_version }}

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Extract api version
        id: extract_api_version
        run: |
          cd src/backend
          API_VERSION=$(python -c 'from app.__version__ import __version__; print(__version__)')
          echo "api_version=${API_VERSION}" >> $GITHUB_OUTPUT
```

Then example variable passing from FMTM:

```yaml
jobs:
  extract-vars:
    needs:
      - pytest
      - frontend-tests
    uses: ./.github/workflows/extract_vars.yml
    with:
      environment: ${{ github.ref_name }}

  backend-build:
    uses: hotosm/gh-workflows/.github/workflows/image_build.yml
    needs: extract-vars
    with:
      context: src/backend
      build_target: prod
      image_tags: |
        "ghcr.io/hotosm/fmtm/backend:${{ needs.extract-vars.outputs.api_version }}-${{ github.ref_name }}"
        "ghcr.io/hotosm/fmtm/backend:latest"
      build_args: |
        APP_VERSION=${{ needs.extract-vars.outputs.api_version }}
```
