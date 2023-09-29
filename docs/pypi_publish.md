# PyPi Publish

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

|                              SECRET                              | REQUIRED |                  DESCRIPTION                  |
|------------------------------------------------------------------|----------|-----------------------------------------------|
| <a name="secret_PYPI_TOKEN"></a>[PYPI_TOKEN](#secret_PYPI_TOKEN) |   true   | The PyPi.org API token for <br>your project.  |

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
