# PyTest (without Docker Compose)

Run pytest for your application, inside a container.

## Prerequisites

1. The tests must be included in the image (via Dockerfile).

2. PyTest must be installed for the dockerfile USER.

> Note: this workflow also injects dotenv vars present in your
> Github environment called `test` (or set by `inputs.environment`).
> See the test_compose docs for more details.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                            | TYPE    | REQUIRED | DEFAULT        | DESCRIPTION                                                                               |
| -------------------------------------------------------------------------------- | ------- | -------- | -------------- | ----------------------------------------------------------------------------------------- |
| <a name="input_build_context"></a>[build_context](#input_build_context)          | string  | false    | `"."`          | Root directory to start the <br>build from.                                               |
| <a name="input_build_dockerfile"></a>[build_dockerfile](#input_build_dockerfile) | string  | false    | `"Dockerfile"` | Name of dockerfile, relative to <br>context dir.                                          |
| <a name="input_build_target"></a>[build_target](#input_build_target)             | string  | false    | `"ci"`         | The target to built to <br>(default to ci stage).                                         |
| <a name="input_cache_image"></a>[cache_image](#input_cache_image)                | boolean | false    | `true`         | Cache the built image, for <br>the next run. Default true.                                |
| <a name="input_environment"></a>[environment](#input_environment)                | string  | false    | `"test"`       | The environment to use for <br>testing.                                                   |
| <a name="input_extra_build_args"></a>[extra_build_args](#input_extra_build_args) | string  | false    |                | Space separated list of build <br>args to use for the <br>image.                          |
| <a name="input_image_name"></a>[image_name](#input_image_name)                   | string  | true     |                | The image root name, without <br>tag. E.g. 'ghcr.io/[dollar]{{ github.repository }}'      |
| <a name="input_tag_override"></a>[tag_override](#input_tag_override)             | string  | false    |                | An override for the build <br>image tag. Must include tests <br>and have PyTest installed |

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

Running on push and PR:

```yaml
name: pytest

on:
  push:
    branches: [main]
  # Run tests on PR, prior to merge to main & development
  pull_request:
    branches: [main]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  pytest:
    uses: hotosm/gh-workflows/.github/workflows/test_pytest.yml@main
    with:
      image_name: ghcr.io/${{ github.repository }}
      extra_build_args: |
        COMMIT_REF=${{ github.sha }}
      tag_override: ${{ github.event_name == 'push' && 'ci' || '' }}
```

This includes a tag_override, so that in a pull_request event
the image is tagged with the PR number, but during a push
event the image is tagged as `ci` (to not confuse it with releases).
