# MKDocs Build

Build docs using Python mkdocs.

You need to have:

- `docs` directory at root level.
- mkdocs.yml config file.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                               | TYPE    | REQUIRED | DEFAULT                                     | DESCRIPTION                                                                   |
| ----------------------------------------------------------------------------------- | ------- | -------- | ------------------------------------------- | ----------------------------------------------------------------------------- |
| <a name="input_doxygen"></a>[doxygen](#input_doxygen)                               | string  | false    |                                             | Include doxygen output uploaded to <br>`artifact` key.                        |
| <a name="input_image"></a>[image](#input_image)                                     | string  | false    | `"ghcr.io/hotosm/gh-workflows/mkdocs:main"` | Override the image to build <br>mkdocs.                                       |
| <a name="input_include_artifacts"></a>[include_artifacts](#input_include_artifacts) | string  | false    |                                             | Include all uploaded artifacts from <br>the workflow.                         |
| <a name="input_keep_extra_files"></a>[keep_extra_files](#input_keep_extra_files)    | boolean | false    | `false`                                     | Only update modified files. Default <br>false, to clean repo before <br>push. |
| <a name="input_openapi"></a>[openapi](#input_openapi)                               | string  | false    |                                             | Include openapi output uploaded to <br>`artifact` key.                        |

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

Simple publish:

```yaml
name: Publish Docs

on:
  push:

jobs:
  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
```

Publish with Doxygen & OpenAPI YAML:

```yaml
name: Publish Docs

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
    uses: hotosm/gh-workflows/.github/workflows/doxygen_build.yml@main
    with:
      output_path: docs/apidocs

  build_openapi_json:
    uses: hotosm/gh-workflows/.github/workflows/openapi_build.yml@main
    with:
      image: ghcr.io/${{ github.repository }}/backend:ci-${{ github.ref_name }}
      example_env_file_path: ".env.example"
      output_path: docs/openapi.json

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
    needs:
      - build_doxygen
      - build_openapi_json
    with:
      image: ghcr.io/${{ github.repository }}/backend:ci-${{ github.ref_name }}
      doxygen: true
      openapi: true
```
