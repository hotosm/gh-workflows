# gh-workflows

Github workflows that can be shared across all HOT projects.

🕮 [Documentation](https://hotosm.github.io/gh-workflows/)

## To add new workflows

- Add your `.yml` workflow to `.github/workflows`.
- Create a `.md` file under `docs`, in the same format as the others.
  - The file should have empty content, with a title,
    and **##** headers: Inputs, Outputs, Secrets.
- Add to `.github/workflows/workflow_docs.yml`:
  - A step in the same format as others using `tj-actions/auto-doc@v3`,
    changing variables.
  - Add an entry under `files` in the **Verify Changed Files** step.

## To manually document workflows using auto-doc

Replace the .yml and .md file names below:

```bash
curl -LO https://github.com/tj-actions/auto-doc/releases/download/v3.1.0/auto-doc_3.1.0_Linux_x86_64.tar.gz

tar -xzsf auto-doc_3.1.0_Linux_x86_64.tar.gz

rm -rf auto-doc_3.1.0_Linux_x86_64.tar.gz

./auto-doc --filename .github/workflows/openapi_build.yml \
    --output docs/openapi_build.md --reusable

rm auto-doc
```
