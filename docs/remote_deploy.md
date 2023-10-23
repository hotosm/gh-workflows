# Remote Deploy

Deploy to a remote server using docker compose and SSH keys.

> This workflow uses the DOCKER_HOST variable underneath during deploy.

## Prerequisites

1. There must be a `.env.example` file in the root of your repo.

- This file describes all possible environment variables,
  with examples.
- The variables in this file are substituted to produce the
  `.env` file used during deploy.

2. You must have variables configured for your environment:

- SSH_HOST (var)
- SSH_USER (var)
- SSH_PRIVATE_KEY (secret)

3. When calling the workflow:

- You need to pass the `environment` variable to
  extract the variables from your deployment environment.
- The secrets param must be: `secrets: inherit`.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                     | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                |
| ----------------------------------------------------------------------------------------- | ------ | -------- | ------- | -------------------------------------------------------------------------- |
| <a name="input_docker_compose_file"></a>[docker_compose_file](#input_docker_compose_file) | string | true     |         | Path to docker compose file <br>to deploy.                                 |
| <a name="input_environment"></a>[environment](#input_environment)                         | string | false    |         | The Github environment to get <br>variables from. Default repository vars. |

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
deploy-containers:
  needs:
    - smoke-test-backend
    - smoke-test-frontend
  uses: hotosm/gh-workflows/.github/workflows/remote_deploy.yml@main
  with:
    environment: ${{ github.ref_name }}
    docker_compose_file: "docker-compose.${{ github.ref_name }}.yml"
  secrets: inherit
```
