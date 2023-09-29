# OpenAPI Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

|                                              INPUT                                              |  TYPE  | REQUIRED | DEFAULT |                                     DESCRIPTION                                      |
|-------------------------------------------------------------------------------------------------|--------|----------|---------|--------------------------------------------------------------------------------------|
|                   <a name="input_cache_key"></a>[cache_key](#input_cache_key)                   | string |  false   |         |                  The cache key to receive <br>from a previous job.                   |
|                <a name="input_cache_paths"></a>[cache_paths](#input_cache_paths)                | string |   true   |         |                The paths to cache for <br>preceeding/following jobs.                 |
| <a name="input_example_env_file_path"></a>[example_env_file_path](#input_example_env_file_path) | string |   true   |         | FastAPI must start with an <br>environment set. Path to a <br>.env with dummy vars.  |
|                         <a name="input_image"></a>[image](#input_image)                         | string |   true   |         |   The image to build to <br>OpenAPI JSON (dependencies included, i.e. FastAPI.).     |

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

  build_openapi_json:
    uses: hotosm/gh-workflows/.github/workflows/openapi_build.yml@main
    with:
      image: ghcr.io/hotosm/fmtm/backend:ci-main
      example_env_file_path: ".env.example"
      cache_paths: |
        docs/openapi.json
      cache_key: ${{ steps.get_cache_key.outputs.cache_key }}

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
    needs: [build_doxygen, build_openapi_json]
    with:
      cache_paths: |
        docs/apidocs
        docs/openapi.json
      cache_key: ${{ steps.get_cache_key.outputs.cache_key }}
```
