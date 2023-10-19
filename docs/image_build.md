# Container Image Build

This workflow is used to build container
images in a standardised way.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                | TYPE    | REQUIRED | DEFAULT        | DESCRIPTION                                                                                   |
| -------------------------------------------------------------------- | ------- | -------- | -------------- | --------------------------------------------------------------------------------------------- |
| <a name="input_build_args"></a>[build_args](#input_build_args)       | string  | false    |                | Space separated list of build <br>args to use for the <br>image.                              |
| <a name="input_build_target"></a>[build_target](#input_build_target) | string  | false    |                | The target to built to <br>(default to end of the Dockerfile).                                |
| <a name="input_cache"></a>[cache](#input_cache)                      | boolean | false    | `true`         | Use GHCR caching. Default true. <br>Set this false if registry <br>is not ghcr.io.            |
| <a name="input_context"></a>[context](#input_context)                | string  | false    | `"."`          | Root directory to start the <br>build from.                                                   |
| <a name="input_dockerfile"></a>[dockerfile](#input_dockerfile)       | string  | false    | `"Dockerfile"` | Absolute path of the dockerfile <br>(inside the context dir).                                 |
| <a name="input_image_name"></a>[image_name](#input_image_name)       | string  | false    |                | Name of image, without tags. <br>Not required if image_tags specified.                        |
| <a name="input_image_tags"></a>[image_tags](#input_image_tags)       | string  | false    |                | Default=the images are automatically tagged. <br>Override tags with space separated <br>list. |
| <a name="input_multi_arch"></a>[multi_arch](#input_multi_arch)       | boolean | false    | `true`         | Build a multi-arch image for <br>AMD/ARM.                                                     |
| <a name="input_push"></a>[push](#input_push)                         | boolean | false    | `true`         | Override prevent pushing the image.                                                           |
| <a name="input_registry"></a>[registry](#input_registry)             | string  | false    | `"ghcr.io"`    | Override GHCR to use an <br>external reg.                                                     |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                           | VALUE                                          | DESCRIPTION          |
| ---------------------------------------------------------------- | ---------------------------------------------- | -------------------- |
| <a name="output_image_name"></a>[image_name](#output_image_name) | `"${{ jobs.build-image.outputs.image_name }}"` | The final image tag. |

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

## Example Usage

**Standalone**

Multi-arch, cached, auto-tag, auto-push:

```yaml
backend-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml@main
  with:
    context: src/backend
    build_target: prod
    image_name: ghcr.io/${{ github.repository }}/backend
    build_args: |
      APP_VERSION=${{ github.ref_name }}
      COMMIT_REF=${{ github.sha }}
```

Manual tagging:

```yaml
backend-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml
  with:
    context: src/backend
    build_target: prod
    image_tags: |
      "ghcr.io/hotosm/fmtm/backend:latest"
    build_args: |
      APP_VERSION=0.1.0
```

**Passing Variables**

Example variable extraction from FMTM:

```yaml
name: Extract Project Variables

on:
  workflow_call:
    inputs:
      environment:
        description: "The GitHub environment to extract vars from."
        type: string
        default: ""
    outputs:
      api_version:
        description: "Backend API Version."
        value: ${{ jobs.extract-vars.outputs.api_version }}

jobs:
  extract-vars:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    outputs:
      api_version: ${{ steps.extract_api_version.outputs.api_version }}

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Extract api version
        id: extract_api_version
        run: |
          cd src/backend
          API_VERSION=$(python -c 'from app.__version__ import __version__; print(__version__)')
          echo "api_version=${API_VERSION}" >> $GITHUB_OUTPUT
```

Then example variable passing from FMTM:

```yaml
jobs:
  extract-vars:
    needs:
      - pytest
      - frontend-tests
    uses: ./.github/workflows/extract_vars.yml
    with:
      environment: ${{ github.ref_name }}

  backend-build:
    uses: hotosm/gh-workflows/.github/workflows/image_build.yml
    needs: extract-vars
    with:
      context: src/backend
      build_target: prod
      image_tags: |
        "ghcr.io/hotosm/fmtm/backend:${{ needs.extract-vars.outputs.api_version }}-${{ github.ref_name }}"
        "ghcr.io/hotosm/fmtm/backend:latest"
      build_args: |
        APP_VERSION=${{ needs.extract-vars.outputs.api_version }}
```
