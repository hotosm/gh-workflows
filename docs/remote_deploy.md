# Remote Deploy

Deploy to a remote server using docker compose and SSH keys.

This workflow uses the DOCKER_HOST variable underneath during deploy.

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
