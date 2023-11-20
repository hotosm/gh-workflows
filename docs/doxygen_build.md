# Doxygen Build

This workflow is used to build a Doxygen class hierarchy.

It needs a Doxyfile present under `docs/Doxyfile`.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                             | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                |
| ----------------------------------------------------------------- | ------ | -------- | ------- | -------------------------------------------------------------------------- |
| <a name="input_output_path"></a>[output_path](#input_output_path) | string | false    |         | If specified, the output dir <br>is uploaded to artifact extra-docs-files. |

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
name: 📖 Publish Docs

on:
  push:
    paths:
      - docs/**
      - src/**
      - mkdocs.yml
    branches: [development]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  build_doxygen:
    uses: hotosm/gh-workflows/.github/workflows/doxygen_build.yml@1.2.2
    with:
      output_path: docs/apidocs

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@1.2.2
    needs:
      - build_doxygen
    with:
      image: ghcr.io/${{ github.repository }}/backend:ci-${{ github.ref_name }}
      doxygen: true
```
