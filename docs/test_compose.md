# Test via Docker Compose

Run tests inside a container, as part of a docker
compose stack.

This is useful for tests when the tests require, e.g. an
underlying database, or any additional services.

> If you are not sure on what test to use, this is likely the one!
> Most tests require additional services like a database.

## Prerequisites

- The tests inside the container, by two possible options:

  - Copied into your container build under **WORKDIR**.
  - Mounted within the docker-compose.yml file under **WORKDIR**.

- The testing tool must be installed for the dockerfile **USER**.

- The service in the docker-compose.yml must have a **TAG_OVERRIDE**.

  ```yaml
  services:
    api:
      image: "ghcr.io/hotosm/fmtm/backend:${TAG_OVERRIDE:-debug}"
  ```

  This allows the workflow to inject the tag of the image built for
  a PR, or during deployment.

- There should be a `.env.example` file in the root of your repo,
  if you require any environment variables to run your service.

  - This file describes all possible environment variables,
    with examples.
  - The variables in this file are substituted to produce the
    `.env` file from Github environment variables.

  ```dotenv
  SECRET_KEY=${SECRET_KEY:-somesuperdupersecretkeyfortesting}
  ALLOWED_HOSTS=${ALLOWED_HOSTS:-["*"]}
  DB_NAME=${DB_NAME:-""}
  DB_USER=${DB_USER:-""}
  DB_PASSWORD=${DB_PASSWORD:-""}
  DB_HOST=${DB_HOST:-""}
  DB_PORT=${DB_PORT:-5432}
  ```

  > Note: the syntax above sets the default tag to 'debug', unless the
  > TAG_OVERRIDE variable is present in the environment
  > (this workflow sets the variable).

- Finally, the environment variables you wish to substitute
  must be present as environment variables or secrets in your
  Github repository settings.

  - The environment name default is `test`.
  - It is possible to override which environment to use by setting
    workflow input `environment`.
  - **Important**: to ensure that secrets are passed to the workflow,
    you must add `secrets: inherit` (see examples below).

## FAQ

### Environment variables are not injecting

There may be an issue with your `.env.example`.

A template such as the example above is required
to generate a `.env` file from `.env.example`.

In this example:

```dotenv
ALLOWED_HOSTS=${ALLOWED_HOSTS:-["*"]}
```

The variable `ALLOWED_HOSTS` will be substituted to:

```dotenv
# If ALLOWED_HOSTS is present in your environment
ALLOWED_HOSTS=["https://some.domain.org"]
# ["https://some.domain.org"] is the value in your Github environment

# If no variable is set in your environment, the default is used.
# This is the value after the :- above (in bash substitution syntax).
ALLOWED_HOSTS=["*"]
```

### Hostnames are not resolving

For example, your app requires the database,
but the database service name is not available.

An example in Django would look like:

```
django.db.utils.OperationalError: could not translate
host name "db" to address: Temporary failure in name resolution
```

This may be because the database has not initialised completely,
prior to running the command.

To solve this, update your docker compose file to use `depends_on`,
under your app service:

```yaml
services:
  app:
    ...
    depends_on:
      db:
        condition: service_healthy
```

To facilitate this, it is good practice to add a healthcheck to
containers. We should use a healthcheck that determines the database
has started and is healthy.

If building a custom Dockerfile, you can add a HEATHCHECK directive.

But many will use a standard unmodified database image, so the
heathcheck is added in docker compose:

```yaml
services:
  db:
    ...
    healthcheck:
      test: pg_isready -U ${DB_USER:-postgres} -d ${DB_NAME:-postgres}
      start_period: 5s
      interval: 10s
      timeout: 5s
      retries: 3
```

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                            | TYPE    | REQUIRED | DEFAULT                | DESCRIPTION                                                                                      |
| -------------------------------------------------------------------------------- | ------- | -------- | ---------------------- | ------------------------------------------------------------------------------------------------ |
| <a name="input_build_context"></a>[build_context](#input_build_context)          | string  | false    | `"."`                  | Root directory to start the <br>build from.                                                      |
| <a name="input_build_dockerfile"></a>[build_dockerfile](#input_build_dockerfile) | string  | false    | `"Dockerfile"`         | Name of dockerfile, relative to <br>context dir.                                                 |
| <a name="input_build_target"></a>[build_target](#input_build_target)             | string  | false    | `"ci"`                 | The target to built to <br>(default to ci stage).                                                |
| <a name="input_cache_extra_imgs"></a>[cache_extra_imgs](#input_cache_extra_imgs) | string  | false    |                        | Space separated list of images <br>to cache on each run <br>(e.g. to avoid rate limiting).       |
| <a name="input_cache_image"></a>[cache_image](#input_cache_image)                | boolean | false    | `true`                 | Cache the built image, for <br>the next run. Default true.                                       |
| <a name="input_compose_command"></a>[compose_command](#input_compose_command)    | string  | true     |                        | The command to run within <br>the docker compose service.                                        |
| <a name="input_compose_file"></a>[compose_file](#input_compose_file)             | string  | false    | `"docker-compose.yml"` | The docker compose file used <br>to run the test.                                                |
| <a name="input_compose_service"></a>[compose_service](#input_compose_service)    | string  | true     |                        | The docker compose service to <br>run the test against.                                          |
| <a name="input_environment"></a>[environment](#input_environment)                | string  | false    | `"test"`               | The environment to use for <br>testing.                                                          |
| <a name="input_extra_build_args"></a>[extra_build_args](#input_extra_build_args) | string  | false    |                        | Space separated list of build <br>args to use for the <br>image.                                 |
| <a name="input_image_name"></a>[image_name](#input_image_name)                   | string  | true     |                        | The image root name, without <br>tag. E.g. 'ghcr.io/[dollar]{{ github.repository }}'             |
| <a name="input_pre_command"></a>[pre_command](#input_pre_command)                | string  | false    |                        | A initialisation command to run <br>prior to the docker compose <br>command.                     |
| <a name="input_tag_override"></a>[tag_override](#input_tag_override)             | string  | false    |                        | An override for the build <br>image tag. Must include tests <br>and have test software installed |

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
    uses: hotosm/gh-workflows/.github/workflows/test_compose.yml@main
    with:
      image_name: ghcr.io/${{ github.repository }}/backend
      build_context: src/backend
      extra_build_args: |
        APP_VERSION=${{ github.ref_name }}
        COMMIT_REF=${{ github.sha }}
      docker_compose_service: api
      docker_compose_command: wait-for-it fmtm-db:5432 --strict -- wait-for-it central:8383 --strict --timeout=30 -- pytest
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
    uses: hotosm/gh-workflows/.github/workflows/test_compose.yml@main
    with:
      image_name: ghcr.io/${{ github.repository }}/backend
      build_context: src/backend
      extra_build_args: |
        APP_VERSION=${{ github.ref_name }}
        COMMIT_REF=${{ github.sha }}
      docker_compose_service: api
      docker_compose_command: wait-for-it fmtm-db:5432 --strict -- wait-for-it central:8383 --strict --timeout=30 -- pytest
      tag_override: ci-${{ github.ref_name }}
    secrets: inherit
## Continue to deploy...
```
