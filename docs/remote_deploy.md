# Remote Deploy

Deploy to a remote server using docker compose and SSH keys.

This workflow uses the DOCKER_HOST variable underneath during deploy.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                     | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                      |
| ----------------------------------------------------------------------------------------- | ------ | -------- | ------- | -------------------------------------------------------------------------------- |
| <a name="input_docker_compose_file"></a>[docker_compose_file](#input_docker_compose_file) | string | true     |         | Path to docker compose file <br>to deploy.                                       |
| <a name="input_env_vars"></a>[env_vars](#input_env_vars)                                  | string | false    |         | Space sparated list of env <br>var pairs, e.g. SOME_VAR=${{ var.some_var <br>}}. |
| <a name="input_ssh_host"></a>[ssh_host](#input_ssh_host)                                  | string | true     |         | The SSH host server to <br>deploy to.                                            |
| <a name="input_ssh_user"></a>[ssh_user](#input_ssh_user)                                  | string | true     |         | The SSH user to deploy <br>as.                                                   |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

No outputs.

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

| SECRET                                                                          | REQUIRED | DESCRIPTION                                                    |
| ------------------------------------------------------------------------------- | -------- | -------------------------------------------------------------- |
| <a name="secret_ssh_private_key"></a>[ssh_private_key](#secret_ssh_private_key) | true     | The private key for the <br>ssh_host (with pub key on server). |

<!-- AUTO-DOC-SECRETS:END -->
