# PyTest (via Docker Compose)

Set up the PNPM environment and run tests using
`pnpm run test`.

## Inputs

## Outputs

## Secrets

## Example Usage

```yaml
jobs:
  frontend-tests:
    uses: hotosm/gh-workflows/.github/workflows/test_pnpm.yml@main
    with:
      working_dir: src/frontend
```
