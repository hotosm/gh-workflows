# MKDocs Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

|                               INPUT                               |  TYPE  | REQUIRED | DEFAULT |                      DESCRIPTION                       |
|-------------------------------------------------------------------|--------|----------|---------|--------------------------------------------------------|
|    <a name="input_cache_key"></a>[cache_key](#input_cache_key)    | string |  false   |         |   The cache key to receive <br>from a previous job.    |
| <a name="input_cache_paths"></a>[cache_paths](#input_cache_paths) | string |  false   |         | The paths to cache for <br>preceeding/following jobs.  |

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
  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
```
