# Multi-Architecture Image Build

This workflow is used to build container images
that are compatible with multiple architectures.

Supports:

- linux/amd64
- linux/arm/v6
- linux/arm/v7
- linux/arm64

## Inputs

## Secrets

## Outputs

## Example Usage

```yaml
test-img-build:
uses: hotosm/gh-workflows/.github/workflows/image_build_multi.yml@1.3.3
with:
  image_name: ghcr.io/${{ github.repository }}
```
