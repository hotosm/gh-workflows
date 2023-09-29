# MKDocs Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT           | TYPE   | REQUIRED | DEFAULT                                   | DESCRIPTION                                                                      |
| --------------- | ------ | -------- | ----------------------------------------- | -------------------------------------------------------------------------------- |
| cache_key       | string | false    |                                           | The cache key to receive from <br>a previous job.                                |
| cache_paths     | string | false    |                                           | The paths to cache for preceeding/following <br>jobs.                            |
| image           | string | false    | `"docker.io/squidfunk/mkdocs-material:9"` | Override the image to build mkdocs.                                              |
| prepend_command | string | false    |                                           | Command to run before mkdocs deploy <br>(e.g. 'gosu appuser' to run as appuser). |

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
