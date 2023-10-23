# Extract Python App Version

Output the version of a Python application, using the **version**.py file.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                          |
| -------------------------------------------------------------------- | ------ | -------- | ------- | ---------------------------------------------------- |
| <a name="input_package_name"></a>[package_name](#input_package_name) | string | true     |         | The package_name that **version**.py sits <br>under. |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                              | VALUE                                                   | DESCRIPTION         |
| ------------------------------------------------------------------- | ------------------------------------------------------- | ------------------- |
| <a name="output_app_version"></a>[app_version](#output_app_version) | `"${{ jobs.extract-app-version.outputs.app_version }}"` | Python app version. |

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

## Example Usage

```yaml
jobs:
  extract-vars:
    uses: hotosm/gh-workflows/.github/workflows/py_app_version.yml@main
    with:
      package_name: osm_fieldwork

  backend-ci-build:
    uses: hotosm/gh-workflows/.github/workflows/image_build.yml@main
    needs: [extract-vars]
    with:
      context: .
      build_target: ci
      image_tags: |
        "ghcr.io/hotosm/osm-fieldwork:ci-${{ github.ref_name }}"
      extra_build_args: |
        APP_VERSION=${{ needs.extract-vars.outputs.app_version }}
```
