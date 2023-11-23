# Container Image Build

This workflow is used to build container images
in a standardised way.

## Usage

Basic usage of this action only requires the image_name input.

```yaml
test-img-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml@1.4.0
  with:
    image_name: ghcr.io/${{ github.repository }}
```

This will build an image for the repository.

If multiple images are built in the same repository, it is
possible to name the images under separate paths:

```yaml
ghcr.io/${{ github.repository }}/backend
ghcr.io/${{ github.repository }}/frontend
ghcr.io/${{ github.repository }}/some-other-service
```

### Defaults

- Build an image using the root directory, and file `Dockerfile`.
- Automatically tag your image, depending on the branch or
  version number.
- Inject the build-args:
  - APP_VERSION=${{ github.ref_name }} (the current branch or tag)
  - COMMIT_REF=${{ github.sha }} (the current commit)
- Cache your image in the Github Container Registry for future builds.
- Push your image to the registry for future use.

## Vulnerability Scanning

Two types of vulnerability scan are available.

Both are enabled by default.

### Static Code Analysis of Dockerfile

Scanning of Dockerfiles for best practice security is done
by [checkov](https://github.com/bridgecrewio/checkov-action).

This can be disabled with the input parameter:
`scan_dockerfile: false`.

### CVE Scanning of Built Image

The built image is scanned for CVEs present in the installed software
by [Trivy](https://github.com/aquasecurity/trivy-action).

This can be disabled with the input parameter:
`scan_image: false`.

## Multi Architecture Builds

There is basic support for building multi-architecture images.

By using the `multi_arch: true` option, builds can be made for
AMD64 (default Linux/Windows), and ARM64 (newer MacOS M-chips).

Please note, however, that using `multi_arch` may increase your build time
by up to 3x.

If speed is important, there is another workflow availble named
[image_build_multi](https://hotosm.github.io/gh-workflows/image_build_multi)
that will build across multiple Github runners
and should be faster (amd64 | arm/v6 | arm/v7 | arm64).

> Note: you should carefully consider if multi-architecture builds
> are worth the performance tradeoff.
>
> As of 2023, deployment on architectures other than AMD64 is rare.
> To accomodate MacOS users during app development, it is suggested
> they build the image themselves on their own architecture (often
> a single command).

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                            | TYPE    | REQUIRED | DEFAULT        | DESCRIPTION                                                                                   |
| -------------------------------------------------------------------------------- | ------- | -------- | -------------- | --------------------------------------------------------------------------------------------- |
| <a name="input_build_target"></a>[build_target](#input_build_target)             | string  | false    |                | The target to built to <br>(default to end of the Dockerfile).                                |
| <a name="input_cache"></a>[cache](#input_cache)                                  | boolean | false    | `true`         | Use GHCR caching. Default true. <br>Set this false if registry <br>is not ghcr.io.            |
| <a name="input_context"></a>[context](#input_context)                            | string  | false    | `"."`          | Root directory to start the <br>build from.                                                   |
| <a name="input_dockerfile"></a>[dockerfile](#input_dockerfile)                   | string  | false    | `"Dockerfile"` | Name of dockerfile, relative to <br>context dir.                                              |
| <a name="input_extra_build_args"></a>[extra_build_args](#input_extra_build_args) | string  | false    |                | Space separated list of extra <br>build args to use for <br>the image.                        |
| <a name="input_image_name"></a>[image_name](#input_image_name)                   | string  | false    |                | Name of image, without tags. <br>Not required if image_tags specified.                        |
| <a name="input_image_tags"></a>[image_tags](#input_image_tags)                   | string  | false    |                | Default=the images are automatically tagged. <br>Override tags with space separated <br>list. |
| <a name="input_multi_arch"></a>[multi_arch](#input_multi_arch)                   | boolean | false    | `false`        | Build a multi-arch image for <br>AMD64/ARM64.                                                 |
| <a name="input_push"></a>[push](#input_push)                                     | boolean | false    | `true`         | Override prevent pushing the image.                                                           |
| <a name="input_registry"></a>[registry](#input_registry)                         | string  | false    | `"ghcr.io"`    | Override GHCR to use an <br>external reg.                                                     |
| <a name="input_scan_dockerfile"></a>[scan_dockerfile](#input_scan_dockerfile)    | boolean | false    | `true`         | Enable dockerfile vulnerability scanning, prior <br>to build.                                 |
| <a name="input_scan_image"></a>[scan_image](#input_scan_image)                   | boolean | false    | `true`         | Enable image vulnerability scan, after <br>build.                                             |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                           | VALUE                                          | DESCRIPTION                     |
| ---------------------------------------------------------------- | ---------------------------------------------- | ------------------------------- |
| <a name="output_image_name"></a>[image_name](#output_image_name) | `"${{ jobs.build-image.outputs.image_name }}"` | The final full image reference. |
| <a name="output_image_tag"></a>[image_tag](#output_image_tag)    | `"${{ jobs.build-image.outputs.image_tag }}"`  | The final image tag.            |

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
    extra_build_args: |
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
    extra_build_args: |
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
      extra_build_args: |
        APP_VERSION=${{ needs.extract-vars.outputs.api_version }}
```
