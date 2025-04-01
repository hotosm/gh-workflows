# Create .env File Action

A GitHub Action to create a `.env` file from a template with environment variable substitution.

## Overview

This action automates the creation of a `.env` file for your application by:

1. Using a template file (like `.env.example`)
2. Substituting environment variables into the template
3. Adding standard Git branch and tag information
4. Supporting additional variables through JSON input

## Features

- Environment variable substitution with default value support (`${VAR:-default}`)
- Handles missing template files gracefully
- Supports custom template and output filenames
- Configurable working directory
- Add additional variables in JSON format
- Uses the powerful [a8m/envsubst](https://github.com/a8m/envsubst) tool

## Usage

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Create .env File
        uses: hotosm/gh-workflows/.github/actions/env_substitute@main
        with:
          working_directory: ./frontend
          template_dotenv: ".env.example"
```

## Inputs

| Input               | Description                                   | Required | Default                  |
| ------------------- | --------------------------------------------- | -------- | ------------------------ |
| `working_directory` | Directory containing the .env template        | No       | `'.'`                    |
| `template_dotenv`   | Name of the template .env file                | No       | `'.env.example'`         |
| `output_file`       | Name of the output .env file                  | No       | `'.env'`                 |
| `git_branch`        | Git branch to include in the .env file        | No       | `${{ github.ref_name }}` |
| `tag_override`      | Tag override to include in the .env file      | No       | `''`                     |
| `envsubst_version`  | Version of envsubst to use                    | No       | `'v1.2.0'`               |
| `additional_vars`   | Additional variables to include (JSON format) | No       | `'{}'`                   |

## How It Works

1. Checks if the template file exists in the specified directory
2. Downloads the specified version of `envsubst`
3. Substitutes environment variables in the template
4. Adds `GIT_BRANCH` and optional `TAG_OVERRIDE` to the `.env` file
5. Adds any additional variables provided in JSON format

## Example: With Additional Variables

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Load Environment Variables
        uses: hotosm/gh-workflows/.github/actions/vars_n_secret_to_env@main
        with:
          vars_context: ${{ toJson(vars) }}
          secrets_context: ${{ toJson(secrets) }}

      - name: Create .env File
        uses: hotosm/gh-workflows/.github/actions/env_substitute@main
        with:
          working_directory: "./frontend"
          template_dotenv: ".env.example"
          output_file: ".env"
          additional_vars: '{"BUILD_ID":"${{ github.run_id }}","DEPLOY_TIME":"$(date -u +"%Y-%m-%dT%H:%M:%SZ")"}'
```

## Template File Format

Your template file can contain environment variable references that will be substituted:

```
# Database configuration
DB_HOST=${DB_HOST:-localhost}
DB_PORT=${DB_PORT:-5432}
DB_NAME=${DB_NAME}
DB_USER=${DB_USER}
DB_PASSWORD=${DB_PASSWORD}

# Application settings
APP_URL=${APP_URL:-http://localhost:3000}
DEBUG=${DEBUG:-false}
```

## Requirements

- This action requires `curl` and `jq`, which are pre-installed on GitHub-hosted runners.

## License

This project is licensed under the MIT License.
