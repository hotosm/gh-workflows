# OpenAPI Build

Build OpenAPI YAML for use in Swagger or ReDoc documentation sites.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                              | TYPE   | REQUIRED | DEFAULT               | DESCRIPTION                                                                          |
| -------------------------------------------------------------------------------------------------- | ------ | -------- | --------------------- | ------------------------------------------------------------------------------------ |
| <a name="input_container_user"></a>[container_user](#input_container_user)                         | string | false    | `"root"`              | The user to run the <br>container as. Defaults to root.                              |
| <a name="input_example_env_file_path"></a>[example_env_file_path](#input_example_env_file_path)    | string | true     |                       | FastAPI must start with an <br>environment set. Path to a <br>.env with dummy vars.  |
| <a name="input_image"></a>[image](#input_image)                                                    | string | true     |                       | The image to build to <br>OpenAPI JSON (dependencies included, i.e. FastAPI.).       |
| <a name="input_output_file_write_path"></a>[output_file_write_path](#input_output_file_write_path) | string | false    | `"docs/openapi.json"` | The path to write the <br>OpenAPI JSON file to. Defaults <br>to `docs/openapi.json`. |
| <a name="input_output_path"></a>[output_path](#input_output_path)                                  | string | false    |                       | If specified, the output dir <br>is uploaded to the `artifact` <br>key.              |
| <a name="input_py_backend_app_context"></a>[py_backend_app_context](#input_py_backend_app_context) | string | false    | `"app.main"`          | The context to import the <br>FastAPI app from. Defaults to <br>`app.main`.          |
| <a name="input_py_backend_app_name"></a>[py_backend_app_name](#input_py_backend_app_name)          | string | false    | `"api"`               | The name of the FastAPI <br>app. Defaults to `api`.                                  |

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
  build_openapi_json:
    uses: hotosm/gh-workflows/.github/workflows/openapi_build.yml@1.6.0
    with:
      image: ghcr.io/${{ github.repository }}/backend:ci-${{ github.ref_name }}
      example_env_file_path: ".env.example"
      output_path: docs/openapi.json

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@1.6.0
    needs:
      - build_openapi_json
    with:
      image: ghcr.io/${{ github.repository }}/backend:ci-${{ github.ref_name }}
      openapi: true
```
