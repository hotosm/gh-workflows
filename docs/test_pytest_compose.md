# PyTest (via Docker Compose)

Run pytest for your application, inside a container,
and as part of the docker compose stack.

This is useful for tests when the tests require, e.g. an
underlying database, or any additional services.

## Prerequisites

1. The tests must be available either:

- Built into your container image at available under WORKDIR.
- Mounted within the docker-compose.yml file under WORKDIR.

2. PyTest must be installed for the dockerfile USER.

3. The service in the docker-compose.yml must have a tag override.

```yaml
services:
  api:
    image: "ghcr.io/hotosm/fmtm/backend:${TAG_OVERRIDE:-debug}"
```

4. There must be a `.env.example` file in the root of your repo.

- This file describes all possible environment variables,
  with examples.
- The variables in this file are substituted to produce the
  `.env` file from Github environment variables.

> Note: the syntax above sets the default tag to 'debug', unless the
> TAG_OVERRIDE variable is present in the environment
> (this workflow sets the variable).

This allows the workflow to inject the tag of the image built for
a PR, or during deployment.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                              | TYPE    | REQUIRED | DEFAULT                | DESCRIPTION                                                                                |
| -------------------------------------------------------------------------------------------------- | ------- | -------- | ---------------------- | ------------------------------------------------------------------------------------------ |
| <a name="input_build_args"></a>[build_args](#input_build_args)                                     | string  | false    |                        | Space separated list of build <br>args to use for the <br>image.                           |
| <a name="input_build_context"></a>[build_context](#input_build_context)                            | string  | false    | `"."`                  | Root directory to start the <br>build from.                                                |
| <a name="input_build_dockerfile"></a>[build_dockerfile](#input_build_dockerfile)                   | string  | false    | `"Dockerfile"`         | Name of dockerfile, relative to <br>context dir.                                           |
| <a name="input_build_target"></a>[build_target](#input_build_target)                               | string  | false    | `"ci"`                 | The target to built to <br>(default to ci stage).                                          |
| <a name="input_cache_extra_imgs"></a>[cache_extra_imgs](#input_cache_extra_imgs)                   | string  | false    |                        | Space separated list of images <br>to cache on each run <br>(e.g. to avoid rate limiting). |
| <a name="input_cache_image"></a>[cache_image](#input_cache_image)                                  | boolean | false    | `true`                 | Cache the built image, for <br>the next run. Default true.                                 |
| <a name="input_docker_compose_file"></a>[docker_compose_file](#input_docker_compose_file)          | string  | false    | `"docker-compose.yml"` | The docker compose file used <br>to run the test.                                          |
| <a name="input_docker_compose_service"></a>[docker_compose_service](#input_docker_compose_service) | string  | true     |                        | The docker compose service to <br>run the test against.                                    |
| <a name="input_environment"></a>[environment](#input_environment)                                  | string  | false    | `"test"`               | The environment to use for <br>testing.                                                    |
| <a name="input_image_name"></a>[image_name](#input_image_name)                                     | string  | true     |                        | The image root name, without <br>tag. E.g. 'ghcr.io/[dollar]{{ github.repository }}'       |
| <a name="input_tag_override"></a>[tag_override](#input_tag_override)                               | string  | false    |                        | An override for the build <br>image tag. Must include tests <br>and have PyTest installed  |

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

Running during PR:

```yaml
name: PR Test Backend

on:
  pull_request:
    branches:
      - main
      - staging
      - development
    # Workflow is triggered only if src/backend changes
    paths:
      - src/backend/**
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  pytest:
    uses: hotosm/gh-workflows/.github/workflows/test_pytest_compose.yml@main
    with:
      image_name: ghcr.io/${{ github.repository }}/backend
      build_context: src/backend
      build_args: |
        APP_VERSION=${{ github.ref_name }}
        COMMIT_REF=${{ github.sha }}
      docker_compose_service: api
      cache_extra_imgs: |
        "docker.io/postgis/postgis:${{ vars.POSTGIS_TAG }}"
        "docker.io/minio/minio:${{ vars.MINIO_TAG }}"
    secrets: inherit
```

Running prior to deployment:

```yaml
name: Build and Deploy

on:
  # Push includes PR merge
  push:
    branches:
      - main
      - staging
      - development
    paths:
      # Workflow is triggered only if src changes
      - src/**
  # Allow manual trigger
  workflow_dispatch:

jobs:
  pytest:
    uses: hotosm/gh-workflows/.github/workflows/test_pytest_compose.yml@main
    with:
      image_name: ghcr.io/${{ github.repository }}/backend
      build_context: src/backend
      build_args: |
        APP_VERSION=${{ github.ref_name }}
        COMMIT_REF=${{ github.sha }}
      docker_compose_service: api
      tag_override: ci-${{ github.ref_name }}
    secrets: inherit
## Continue to deploy...
```
