# NPM Publish

Publish a package to NPMJS using PNPM.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

No inputs.

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

No outputs.

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

| SECRET                                                        | REQUIRED | DESCRIPTION                                   |
| ------------------------------------------------------------- | -------- | --------------------------------------------- |
| <a name="secret_NPM_TOKEN"></a>[NPM_TOKEN](#secret_NPM_TOKEN) | true     | The npmjs.org API token for <br>your project. |

<!-- AUTO-DOC-SECRETS:END -->

## Example Usage

```yaml
name: Publish to npmjs.org

on:
  release:
    types: [published]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  publish_to_npmjs:
    uses: hotosm/gh-workflows/.github/workflows/pnpm_publish.yml@main
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```
