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

## Outputs

## Secrets

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
