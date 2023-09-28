# gh-workflows

Github workflows that can be shared across all HOT projects.

🕮 [Documentation](https://hotosm.github.io/gh-workflows/)

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
