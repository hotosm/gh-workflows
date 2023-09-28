# Doxygen Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                          | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                                                                                                             |
| -------------------------------------------------------------- | ------ | -------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <a name="input_cache_key"></a>[cache_key](#input_cache_key)    | string | false    |         | The cache key to pass <br>to mkdocs_build, e.g.: echo "cache_key=docs-build-$(date --utc '+%V')" <br>>> $GITHUB_ENV Then use, with: <br>cache_key: ${{ env.cache_key }} |
| <a name="input_output_dir"></a>[output_dir](#input_output_dir) | string | true     |         | Output directory for Doxygen to <br>compile                                                                                                                             |

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
name: Publish Docs

on:
  push:
    paths:
      - docs/**
      - osm_login_python/**
      - mkdocs.yml
    branches: [main]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  get_cache_key:
    runs-on: ubuntu-latest
    steps:
      - run: echo "cache_key=docs-build-$(date --utc '+%V')" >> $GITHUB_ENV

  build_doxygen:
    uses: hotosm/gh-workflows/.github/workflows/doxygen_build.yml@main
    with:
      output_dir: docs/apidocs
      cache_key: ${{ env.cache_key }}

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
    needs: [build_doxygen]
    with:
      cache_path: docs/apidocs
      cache_key: ${{ env.cache_key }}
```
