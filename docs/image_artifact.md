# Container Image Artifact

This workflow is used to upload container images
as artifacts during a workflow.

This is useful for 'caching' an downloaded image, if it
if used again within the same workflow, preventing repeated
pulls.

This may or may not be faster than pulling the image again,
as the image still has to pull from thr artifact API.

However, it does prevent repeated pull from container
registries where rate limiting is applied (e.g. dockerhub).

> Note: this strategy does not work across workflow runs.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                   | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                                                  |
| ----------------------------------------------------------------------- | ------ | -------- | ------- | ---------------------------------------------------------------------------- |
| <a name="input_artifact_name"></a>[artifact_name](#input_artifact_name) | string | true     |         | The artifact name to use, <br>e.g. images.                                   |
| <a name="input_image_names"></a>[image_names](#input_image_names)       | string | true     |         | A space separated list of <br>full image names to upload, <br>including tag. |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                                    | VALUE                           | DESCRIPTION                              |
| ------------------------------------------------------------------------- | ------------------------------- | ---------------------------------------- |
| <a name="output_artifact_name"></a>[artifact_name](#output_artifact_name) | `"${{ inputs.artifact_name }}"` | The artifact name used during <br>input. |

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
  artifact-imgs:
    uses: hotosm/gh-workflows/.github/workflows/image_artifact.yml@main
    with:
      image_names: |
        docker.io/postgis/postgis:${{ vars.POSTGIS_TAG }}
        ghcr.io/hotosm/fmtm/odkcentral:${{ vars.ODK_CENTRAL_TAG }}
        ghcr.io/hotosm/fmtm/odkcentral-proxy:${{ vars.ODK_CENTRAL_TAG }}
        docker.io/minio/minio:${{ vars.MINIO_TAG }}

  run-pytest:
    runs-on: ubuntu-latest
    needs: [artifact-imgs]
    environment:
      name: ${{ inputs.environment || 'test' }}

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Download Images Tars
        id: download-images
        uses: actions/download-artifact@v4
        with:
          path: /tmp/images

      - name: Load Tar Imgs
        run: |
          for image_tar in /tmp/images/*; do
              docker image load --input $image_tar || true
          done

      - name: Run PyTest
        run: |
          docker compose run api \
            wait-for-it fmtm-db:5432 --strict \
            -- wait-for-it central:8383 --strict --timeout=30 \
            -- pytest
```
