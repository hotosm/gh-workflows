# MKDocs Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT         | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                           |
| ------------- | ------ | -------- | ------- | ----------------------------------------------------- |
| cache_key     | string | false    |         | The cache key to receive from <br>a previous job.     |
| cache_paths   | string | false    |         | The paths to cache for preceeding/following <br>jobs. |
| pyproject_dir | string | false    |         | The directory containing the pyproject.toml.          |

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
