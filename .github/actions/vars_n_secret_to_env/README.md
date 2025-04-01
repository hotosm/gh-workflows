# Load Environment Variables Action

A GitHub Action to load environment variables from GitHub variables & secrets contexts and parse nested variables.

## Overview

This action simplifies the process of making GitHub variables and secrets available as environment variables within your workflow, including parsing nested variables like `FRONTEND_ENV_VARS` that contain multiple key-value pairs.

## Features

- Load variables from GitHub `vars` context
- Load secrets from GitHub `secrets` context
- Parse and extract nested variables that contain multiple key-value pairs
- Handle multiline strings properly
- Support for Git branch and tag override variables

## Usage

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Load Environment Variables
        uses: hotosm/gh-workflows/.github/actions/vars_n_secret_to_env@main
        with:
          vars_context: ${{ toJson(vars) }}
          secrets_context: ${{ toJson(secrets) }}
```

## Inputs

| Input               | Description                                     | Required | Default                  |
| ------------------- | ----------------------------------------------- | -------- | ------------------------ |
| `git_branch`        | The Git branch name                             | No       | `${{ github.ref_name }}` |
| `tag_override`      | Optional tag override                           | No       | `''`                     |
| `vars_context`      | JSON string of variables context                | Yes      | -                        |
| `secrets_context`   | JSON string of secrets context                  | Yes      | -                        |
| `parse_nested_vars` | Parse nested variables (like FRONTEND_ENV_VARS) | No       | `'true'`                 |

## How It Works

1. The action uses `jq` to parse the JSON contexts and extract all variables and secrets.
2. It exports each value to `$GITHUB_ENV` using GitHub's multiline variable format.
3. If `parse_nested_vars` is set to `true`, it will look for and parse nested variables like `FRONTEND_ENV_VARS` that contain multiple key-value pairs.
4. Always adds `GIT_BRANCH` and optionally `TAG_OVERRIDE` to the environment.

## Example: Parse Nested Variables

If you have a GitHub variable called `FRONTEND_ENV_VARS` with content like:

```
TM_APP_BASE_URL=https://tasks-stage.hotosm.org
TM_API_BASE_URL=https://tasking-manager-staging-api.hotosm.org/api
```

This action will parse it and make both `TM_APP_BASE_URL` and `TM_API_BASE_URL` available as separate environment variables in subsequent steps.

## Advanced Example

```yaml
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Load Environment Variables
        uses: hotosm/gh-workflows/.github/actions/vars_n_secret_to_env@main
        with:
          vars_context: ${{ toJson(vars) }}
          secrets_context: ${{ toJson(secrets) }}
          tag_override: ${{ inputs.version_tag }}
          parse_nested_vars: "true"

      - name: Use Environment Variables
        run: |
          echo "Building for environment: $TM_APP_BASE_URL"
          echo "Using branch: $GIT_BRANCH"
```

## Requirements

- The workflow must have access to `jq`, which is pre-installed on GitHub-hosted runners.
