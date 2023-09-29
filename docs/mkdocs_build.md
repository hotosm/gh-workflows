# MKDocs Build

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT       | TYPE   | REQUIRED | DEFAULT                                   | DESCRIPTION                                           |
| ----------- | ------ | -------- | ----------------------------------------- | ----------------------------------------------------- |
| cache_key   | string | false    |                                           | The cache key to receive from <br>a previous job.     |
| cache_paths | string | false    |                                           | The paths to cache for preceeding/following <br>jobs. |
| image       | string | false    | `"docker.io/squidfunk/mkdocs-material:9"` | Override the image to build mkdocs.                   |

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
name: Publish Docs

on:
  push:

jobs:
  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
```

## Notes

If you are running with a custom image and get errors like:

```
detected dubious ownership in repository at '/__w/fmtm/fmtm'
To add an exception for this directory, call:

Deployment Aborted!
 git config --global --add safe.directory /__w/fmtm/fmtm
```

Then ensure that you run the mkdocs command as uid 1001:

1. Install `gosu` in your Dockerfile.

```dockerfile
RUN set -ex \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install \
    -y --no-install-recommends \
        "gosu" \
    && rm -rf /var/lib/apt/lists/*
```

2. Create the user in your Dockerfile.

```bash
RUN useradd -r -u 1001 -m -c "hotosm account" -d /home/appuser -s /bin/false appuser
```

3. Run the mkdocs command as this user in the workflow.

```yaml
publish_docs:
  uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@main
  needs:
    - get_cache_key
    - build_doxygen
    - build_openapi_json
  with:
    image: ghcr.io/hotosm/fmtm/backend:ci-${{ github.ref_name }}
    prepend_command: "gosu appuser"
    cache_paths: |
      docs/apidocs
      docs/openapi.json
    cache_key: ${{ needs.get_cache_key.outputs.cache_key }}
```

> The important part is the `prepend_command`.
