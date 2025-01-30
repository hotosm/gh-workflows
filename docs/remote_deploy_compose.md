# Remote Deploy (Docker Compose)

Deploy to a remote server using docker compose and SSH keys.

> This workflow uses the DOCKER_HOST variable underneath during deploy.

## Prerequisites

- There must be a `.env.example` file in the root of your repo.

  - This file describes all possible environment variables,
    with examples.
  - The variables in this file are substituted to produce the
    `.env` file used during deploy.

  ```dotenv
  SECRET_KEY=${SECRET_KEY:-somesuperdupersecretkeyfortesting}
  ALLOWED_HOSTS=${ALLOWED_HOSTS:-["*"]}
  DB_NAME=${DB_NAME:-""}
  DB_USER=${DB_USER:-""}
  DB_PASSWORD=${DB_PASSWORD:-""}
  DB_HOST=${DB_HOST:-""}
  DB_PORT=${DB_PORT:-5432}
  ```

- You must have variables configured for your Github environment:

  - SSH_HOST (var) - the domain name or IP address for deploy.
  - SSH_USER (var) - the user the SSH keypair was generated for.
  - SSH_PRIVATE_KEY (secret) - the private key of the keypair.

- When calling the workflow:

  - You need to pass the `environment` variable to
    extract the variables from your deployment environment.
  - This can be `environment: ${{ github.ref_name }}` to use the git branch name.
  - **Important**: the secrets param must be: `secrets: inherit`.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                           | TYPE   | REQUIRED | DEFAULT          | DESCRIPTION                                                                |
| ----------------------------------------------------------------------------------------------- | ------ | -------- | ---------------- | -------------------------------------------------------------------------- |
| <a name="input_docker_compose_file"></a>[docker_compose_file](#input_docker_compose_file)       | string | true     |                  | Path to docker compose file <br>to deploy.                                 |
| <a name="input_environment"></a>[environment](#input_environment)                               | string | false    |                  | The Github environment to get <br>variables from. Default repository vars. |
| <a name="input_example_env_file_path"></a>[example_env_file_path](#input_example_env_file_path) | string | false    | `".env.example"` | Path to example dotenv file <br>to substitute variables for.               |

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
  uses: hotosm/gh-workflows/.github/workflows/remote_deploy_compose.yml@main
  with:
    environment: ${{ github.ref_name }}
    docker_compose_file: "compose.${{ github.ref_name }}.yml"
  secrets: inherit
```
