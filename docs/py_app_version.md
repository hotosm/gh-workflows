# Extract Python App Version

## Inputs

## Outputs

## Secrets

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
      build_args: |
        APP_VERSION=${{ needs.extract-vars.outputs.app_version }}
```
