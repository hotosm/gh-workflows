# PyPi Publish

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                               | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                      |
| ----------------------------------------------------------------------------------- | ------ | -------- | ------- | ---------------------------------------------------------------- |
| <a name="input_token_secret_name"></a>[token_secret_name](#input_token_secret_name) | string | true     |         | The name of the repository <br>secret containing the PyPi token. |

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
name: Publish to PyPi.org

on:
  release:
    types: [published]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  publish_to_pypi:
    uses: hotosm/gh-workflows/.github/workflows/pypi_publish.yml@main
    secrets:
      PYPI_TOKEN: ${{ secrets.PYPI_TOKEN }}
```
