# Container Image Cache

This workflow is used to cache container images
for workflows, preventing the need to repeatedly
download the same images.

For example, a pytest workflow may use the same images
every time to run required services (postgres, redis).

Running pytest should be fast, so caching these images
is a sensible choice.

## Inputs

## Outputs

## Secrets

## Example Usage

In this example, as PostGIS image is cached, to
prevent download every time pytest is run.

```yaml
jobs:
  cache-img-postgis:
    uses: hotosm/gh-workflows/.github/workflows/image_cache.yml@main
    with:
      image_name: postgis/postgis:14-3.3-alpine
      cache_key: img-postgis

  cache-img-odk:
    uses: hotosm/gh-workflows/.github/workflows/image_cache.yml@main
    with:
      image_name: ghcr.io/hotosm/fmtm/odkcentral:v2023.2.1
      cache_key: img-odk

  run-pytest:
    runs-on: ubuntu-latest
    environment:
      name: ${{ inputs.environment || 'test' }}
    needs:
      - postgis-img-cache
      - odk-img-cache

    steps:
      - name: Restore Img Caches
        id: restore-imgs
        uses: actions/cache@v3
        with:
          path: |
            ${{ needs.postgis-img-cache.outputs.cache_path }}
            ${{ needs.odk-img-cache.outputs.cache_path }}
          key: |
            ${{ needs.postgis-img-cache.outputs.cache_key }}
            ${{ needs.odk-img-cache.outputs.cache_key }}

      - name: Load Cached Imgs
        if: steps.restore-imgs.outputs.cache-hit == 'true'
        run: |
          docker image load --input ${{ needs.postgis-img-cache.outputs.cache_path }} || true
          docker image load --input ${{ needs.odk-img-cache.outputs.cache_path }} || true
```
