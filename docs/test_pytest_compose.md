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

This allows the workflow to inject the tag of the image built for
a PR, or during deployment.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                              | TYPE   | REQUIRED | DEFAULT                | DESCRIPTION                                                                                |
| -------------------------------------------------------------------------------------------------- | ------ | -------- | ---------------------- | ------------------------------------------------------------------------------------------ |
| <a name="input_cache_imgs"></a>[cache_imgs](#input_cache_imgs)                                     | string | false    |                        | Space separated list of images <br>to cache on each run <br>(e.g. to avoid rate limiting). |
| <a name="input_docker_compose_file"></a>[docker_compose_file](#input_docker_compose_file)          | string | false    | `"docker-compose.yml"` | The docker compose file used <br>to run the test.                                          |
| <a name="input_docker_compose_service"></a>[docker_compose_service](#input_docker_compose_service) | string | true     |                        | The docker compose service to <br>run the test against.                                    |
| <a name="input_environment"></a>[environment](#input_environment)                                  | string | false    | `"test"`               | The environment to use for <br>testing.                                                    |
| <a name="input_image_tag"></a>[image_tag](#input_image_tag)                                        | string | false    |                        | Optional image tag override.                                                               |

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
    uses: ./.github/workflows/r-pytest.yml
    with:
      docker_compose_service: api
      cache_imgs: |
        "docker.io/postgis/postgis:${{ vars.POSTGIS_TAG }}"
        "docker.io/minio/minio:${{ vars.MINIO_TAG }}"
      # For caching odk central images, add:
      # "ghcr.io/${{ github.repository }}/odkcentral:${{ vars.ODK_CENTRAL_TAG }}"
      # "ghcr.io/${{ github.repository }}/odkcentral-proxy:${{ vars.ODK_CENTRAL_TAG }}"
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
    uses: ./.github/workflows/r-pytest.yml
    with:
      image_tag: ci-${{ github.ref_name }}
      docker_compose_service: api
    secrets: inherit
## Continue to deploy...
```
