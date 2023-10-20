# Container Image Cache

This workflow is used to cache container images
for workflows, preventing the need to repeatedly
download the same images.

For example, a pytest workflow may use the same images
every time to run required services (postgres, redis).

Running pytest should be fast, so caching these images
is a sensible choice.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                             | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                 |
| ----------------------------------------------------------------- | ------ | -------- | ------- | --------------------------------------------------------------------------- |
| <a name="input_cache_key"></a>[cache_key](#input_cache_key)       | string | true     |         | The cache key to use, <br>e.g. image-cache.                                 |
| <a name="input_image_names"></a>[image_names](#input_image_names) | string | true     |         | A space separated list of <br>full image names to cache, <br>including tag. |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                        | VALUE                       | DESCRIPTION                          |
| ------------------------------------------------------------- | --------------------------- | ------------------------------------ |
| <a name="output_cache_key"></a>[cache_key](#output_cache_key) | `"${{ inputs.cache_key }}"` | The cache key used during <br>input. |

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

## Example Usage

In this example, we cache various dependent images, to
prevent download every time pytest is run.

```yaml
jobs:
  run-pytest:
    runs-on: ubuntu-latest
    environment:
      name: ${{ inputs.environment || 'test' }}

    outputs:
      img_cache_loaded: ${{ steps.load-cache.outputs.img_cache_loaded }}

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Download Image Cache
        id: download-images
        uses: actions/download-artifact@v3
        with:
          name: image-cache
          path: /tmp/images
        continue-on-error: true

      - name: Load Cached Imgs
        id: load-cache
        if: ${{ steps.download-images.outputs.download_path == '/tmp/images' }}
        run: |
          for image_tar in /tmp/images/*; do
              docker image load --input $image_tar || true
          done
          echo "img_cache_loaded=true" >> $GITHUB_OUTPUT

      - name: Run PyTest
        run: |
          docker compose run api \
            wait-for-it fmtm-db:5432 --strict \
            -- wait-for-it central:8383 --strict --timeout=30 \
            -- pytest

  # This stage only runs if the images were not found in the cache
  upload-img-cache:
    needs: [run-pytest]
    if: ${{ needs.run-pytest.outputs.img_cache_loaded != true}}
    uses: hotosm/gh-workflows/.github/workflows/image_cache.yml@main
    with:
      image_names: >
        docker.io/postgis/postgis:${{ vars.POSTGIS_TAG }}
        ghcr.io/hotosm/fmtm/odkcentral:${{ vars.ODK_CENTRAL_TAG }}
        ghcr.io/hotosm/fmtm/odkcentral-proxy:${{ vars.ODK_CENTRAL_TAG }}
        docker.io/minio/minio:${{ vars.MINIO_TAG }}
      artifact_name: image-cache
```

> Note that the image_names parameter uses YAML scalar
> syntax `>` instead of literal `|`. This is important.
