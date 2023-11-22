# Extract Env Vars

THIS WORKFLOW IS CURRENTLY NON-FUNCTIONAL

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                             | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                            |
| ----------------------------------------------------------------- | ------ | -------- | ------- | ---------------------------------------------------------------------- |
| <a name="input_environment"></a>[environment](#input_environment) | string | true     |         | The Github environment to retreive <br>the env and secret var <br>for. |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                     | VALUE                                         | DESCRIPTION                       |
| ---------------------------------------------------------- | --------------------------------------------- | --------------------------------- |
| <a name="output_all_vars"></a>[all_vars](#output_all_vars) | `"${{ jobs.get_env_vars.outputs.all_vars }}"` | All parsed variables and secrets. |

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

## Example Usage

```yaml
get-env-vars:
  uses: hotosm/gh-workflows/.github/workflows/env_vars.yml@1.3.1
  with:
    environment: ${{ github.ref_name }}

frontend-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml@1.3.1
  needs:
    - frontend-tests
    - get-env-vars
  with:
    context: src/frontend
    dockerfile: prod.dockerfile
    build_target: prod
    image_name: ghcr.io/${{ github.repository }}/frontend
    extra_build_args: |
      VITE_API_URL=${{ needs.get-env-vars.outputs.all_vars }}
```
