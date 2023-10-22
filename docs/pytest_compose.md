# PyTest (via Docker Compose)

Run pytest for your application, inside a container,
and as part of the docker compose stack.

This is useful for tests when the tests require, e.g. an
underlying database, or any additional services.

## Inputs

## Outputs

## Secrets

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
