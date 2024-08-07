# Stories Build

This workflow is used to build stories for UI components.

The workflow uses PNPM to install dependnecies and run the
npm command that builds the stories dist.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                | TYPE    | REQUIRED | DEFAULT | DESCRIPTION                                                            |
| -------------------------------------------------------------------- | ------- | -------- | ------- | ---------------------------------------------------------------------- |
| <a name="input_command"></a>[command](#input_command)                | string  | true     |         | The pnpm command to run <br>in package.json.                           |
| <a name="input_github_pages"></a>[github_pages](#input_github_pages) | boolean | false    | `false` | Upload in format specific for <br>Github Pages `actions/deploy-pages`. |
| <a name="input_output_path"></a>[output_path](#input_output_path)    | string  | false    |         | The output path is uploaded <br>to a key named `artifact`.             |
| <a name="input_working_dir"></a>[working_dir](#input_working_dir)    | string  | false    | `"."`   | The directory containing the package.json <br>file.                    |

<!-- AUTO-DOC-INPUT:END -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

| OUTPUT                                                                    | VALUE     | DESCRIPTION                              |
| ------------------------------------------------------------------------- | --------- | ---------------------------------------- |
| <a name="output_artifact_name"></a>[artifact_name](#output_artifact_name) | `"build"` | The artifact name (default: `artifact`). |

<!-- AUTO-DOC-OUTPUT:END -->

## Secrets

<!-- AUTO-DOC-SECRETS:START - Do not remove or modify this section -->

No secrets.

<!-- AUTO-DOC-SECRETS:END -->

## Example Usage

```yaml
name: 📖 Publish Docs

on:
  push:
    paths:
      - docs/**
      - src/**
      - mkdocs.yml
    branches: [main]
  # Allow manual trigger (workflow_dispatch)
  workflow_dispatch:

jobs:
  build_stories:
    uses: hotosm/gh-workflows/.github/workflows/pnpm_build.yml@1.6.0
    with:
      npm_cmd: build:docs
      output_path: docs/stories

  publish_docs:
    uses: hotosm/gh-workflows/.github/workflows/mkdocs_build.yml@1.6.0
    needs: [build_stories]
    with:
      stories: true
```
