# PyTest (via Docker Compose)

Set up the PNPM environment and run tests using
`pnpm run test`.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                             | TYPE   | REQUIRED | DEFAULT | DESCRIPTION                                         |
| ----------------------------------------------------------------- | ------ | -------- | ------- | --------------------------------------------------- |
| <a name="input_working_dir"></a>[working_dir](#input_working_dir) | string | false    | `"."`   | The directory containing the package.json <br>file. |

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

Run commands directly on `ubuntu-latest` machine.

```yaml
jobs:
  frontend-tests:
    uses: hotosm/gh-workflows/.github/workflows/test_pnpm.yml@main
    with:
      working_dir: src/frontend
```

Run using a custom container. For example Playwright:

```yaml
jobs:
  frontend-tests:
    uses: hotosm/gh-workflows/.github/workflows/test_pnpm.yml@main
    with:
      container_config: '{"image": "mcr.microsoft.com/playwright:v1.43.0"}'
      working_dir: src/frontend
      run_command: "test:e2e"
```
