# Doxygen Build

This workflow is used to build a Doxygen class hierarchy.

It needs a Doxyfile present under `docs/Doxyfile`.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                             | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                           |
| ----------------------------------------------------------------- | ------ | -------- | ------- | ----------------------------------------------------- |
| <a name="input_cache_key"></a>[cache_key](#input_cache_key)       | string | false    |         | The cache key to receive <br>from a previous job.     |
| <a name="input_cache_paths"></a>[cache_paths](#input_cache_paths) | string | false    |         | The paths to cache for <br>preceeding/following jobs. |

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

jobs:
  get_cache_key:
    runs-on: ubuntu-latest
    outputs:
      cache_key: ${{ steps.set_cache_key.outputs.cache_key }}
    steps:
      - name: Set cache key
        id: set_cache_key
        run: echo "cache_key=docs-build-$(date --utc '+%V')" >> $GITHUB_OUTPUT

  build_doxygen:
    uses: hotosm/gh-workflows/.github/workflows/doxygen_build.yml@main
    with:
      cache_paths: |
        docs/apidocs
      cache_key: ${{ steps.get_cache_key.outputs.cache_key }}

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
    needs: [build_doxygen]
    with:
      cache_paths: |
        docs/apidocs
      cache_key: ${{ steps.get_cache_key.outputs.cache_key }}
```
