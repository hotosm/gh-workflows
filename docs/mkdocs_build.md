# MKDocs Build

Build docs using Python mkdocs.

You need to have:

- `docs` directory at root level.
- mkdocs.yml config file.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                 | TYPE   | REQUIRED | DEFAULT                                   | DESCRIPTION                                                       |
| ----------------------------------------------------- | ------ | -------- | ----------------------------------------- | ----------------------------------------------------------------- |
| <a name="input_doxygen"></a>[doxygen](#input_doxygen) | string | false    |                                           | Include doxygen output uploaded to <br>extra-docs-files artifact. |
| <a name="input_image"></a>[image](#input_image)       | string | false    | `"docker.io/squidfunk/mkdocs-material:9"` | Override the image to build <br>mkdocs.                           |
| <a name="input_openapi"></a>[openapi](#input_openapi) | string | false    |                                           | Include openapi output uploaded to <br>extra-docs-files artifact. |

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
