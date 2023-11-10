# Extract Env Vars

## Inputs

## Outputs

## Secrets

## Example Usage

```yaml
get-env-vars:
  uses: hotosm/gh-workflows/.github/workflows/env_vars.yml@1.2.3
  with:
    environment: ${{ github.ref_name }}

frontend-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml@1.2.2
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
