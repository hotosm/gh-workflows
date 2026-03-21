# Remote Deploy (Just)

Deploy to a remote server using Justfile command and SSH keys.

> This workflow uses the DOCKER_HOST variable underneath during deploy.

It's an alternative to `remote_deploy_compose` to allow each project
to include project-specific config for their deploy.

## Prerequisites

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
| <a name="input_command"></a>[command](#input_command)                                           | string | true     |                  | The Just command to run <br>(defined in Justfile).                         |
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
  uses: hotosm/gh-workflows/.github/workflows/remote_deploy_just.yml@main
  with:
    environment: ${{ github.ref_name }}
    command: start prod
  secrets: inherit
```
