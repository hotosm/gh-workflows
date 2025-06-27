# Multi-Architecture Image Build

This workflow is used to build container images
that are compatible with multiple architectures.

Supports:

- linux/amd64
- linux/arm/v6
- linux/arm/v7
- linux/arm64

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                            | TYPE    | REQUIRED | DEFAULT                                                 | DESCRIPTION                                                                                   |
| -------------------------------------------------------------------------------- | ------- | -------- | ------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| <a name="input_build_target"></a>[build_target](#input_build_target)             | string  | false    |                                                         | The target to built to <br>(default to end of the Dockerfile).                                |
| <a name="input_cache"></a>[cache](#input_cache)                                  | boolean | false    | `true`                                                  | Use GHCR caching. Default true. <br>Set this false if registry <br>is not ghcr.io.            |
| <a name="input_context"></a>[context](#input_context)                            | string  | false    | `"."`                                                   | Root directory to start the <br>build from.                                                   |
| <a name="input_dockerfile"></a>[dockerfile](#input_dockerfile)                   | string  | false    | `"Dockerfile"`                                          | Name of dockerfile, relative to <br>context dir.                                              |
| <a name="input_environment"></a>[environment](#input_environment)                | string  | false    | `"${{ github.ref_name }}"`                              | The environment to use for <br>variables.                                                     |
| <a name="input_extra_build_args"></a>[extra_build_args](#input_extra_build_args) | string  | false    |                                                         | Space separated list of extra <br>build args to use for <br>the image.                        |
| <a name="input_image_name"></a>[image_name](#input_image_name)                   | string  | false    |                                                         | Name of image, without tags. <br>Not required if image_tags specified.                        |
| <a name="input_image_tags"></a>[image_tags](#input_image_tags)                   | string  | false    |                                                         | Default=the images are automatically tagged. <br>Override tags with space separated <br>list. |
| <a name="input_push"></a>[push](#input_push)                                     | boolean | false    | `true`                                                  | Override prevent pushing the image.                                                           |
| <a name="input_registry"></a>[registry](#input_registry)                         | string  | false    | `"ghcr.io"`                                             | Override GHCR to use an <br>external reg.                                                     |
| <a name="input_scan_dockerfile"></a>[scan_dockerfile](#input_scan_dockerfile)    | boolean | false    | `true`                                                  | Enable dockerfile vulnerability scanning, prior <br>to build.                                 |
| <a name="input_scan_image"></a>[scan_image](#input_scan_image)                   | boolean | false    | `true`                                                  | Enable image vulnerability scan, after <br>build.                                             |
| <a name="input_skip_cve"></a>[skip_cve](#input_skip_cve)                         | string  | false    | `"CKV_DOCKER_8,CKV_DOCKER_2,CKV_DOCKER_3,CKV_DOCKER_5"` | Skip specific CVE from checkcov <br>(override rules).                                         |

<!-- AUTO-DOC-INPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                           | VALUE                                           | DESCRIPTION                     |
| ---------------------------------------------------------------- | ----------------------------------------------- | ------------------------------- |
| <a name="output_image_name"></a>[image_name](#output_image_name) | `"${{ jobs.build-images.outputs.image_name }}"` | The final full image reference. |
| <a name="output_image_tag"></a>[image_tag](#output_image_tag)    | `"${{ jobs.build-images.outputs.image_tag }}"`  | The final image tag.            |

<!-- AUTO-DOC-OUTPUT:END -->

## Example Usage

```yaml
test-img-build:
uses: hotosm/gh-workflows/.github/workflows/image_build_multi.yml@1.3.3
with:
  image_name: ghcr.io/${{ github.repository }}
```
