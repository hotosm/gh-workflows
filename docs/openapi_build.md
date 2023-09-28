# OpenAPI Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                 | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                         |
| --------------------- | ------ | -------- | ------- | ----------------------------------------------------------------------------------- |
| cache_key             | string | false    |         | The cache key to receive from <br>a previous job.                                   |
| cache_paths           | string | true     |         | The paths to cache for preceeding/following <br>jobs.                               |
| example_env_file_path | string | true     |         | FastAPI must start with an environment <br>set. Path to a .env with <br>dummy vars. |
| image                 | string | true     |         | The image to build to OpenAPI <br>JSON (dependencies included, i.e. FastAPI.).      |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

No outputs.

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->
