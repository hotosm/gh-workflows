# HOT Github Workflows

<!-- markdownlint-disable -->
<p align="center">
  <img src="https://github.com/hotosm/fmtm/blob/main/images/hot_logo.png?raw=true" style="width: 200px;" alt="HOT"></a>
</p>
<p align="center">
  <em>Github workflows that can be shared across all HOT projects.</em>
</p>
<p align="center">
  <a href="https://github.com/hotosm/fmtm/releases" target="_blank">
      <img src="https://img.shields.io/github/v/release/hotosm/gh-workflows?logo=github" alt="Version">
  </a>
  <a href="https://github.com/hotosm/gh-workflows/actions/workflows/workflow_docs.yml" target="_blank">
      <img src="https://github.com/hotosm/gh-workflows/workflows/GH Workflow Docs/badge.svg" alt="Build Docs">
  </a>
  <a href="https://github.com/hotosm/gh-workflows/actions/workflows/docs.yml" target="_blank">
      <img src="https://github.com/hotosm/gh-workflows/workflows/Publish Docs/badge.svg" alt="Publish Docs">
  </a>
  <a href="https://github.com/hotosm/gh-workflows/blob/main/LICENSE.md" target="_blank">
      <img src="https://img.shields.io/github/license/hotosm/gh-workflows.svg" alt="License">
  </a>
</p>
<!-- markdownlint-enable -->

---

📖 **Documentation**: [https://hotosm.github.io/gh-workflows](https://hotosm.github.io/gh-workflows)

🖥️ **Source Code**: [https://github.com/hotosm/gh-workflows](https://github.com/hotosm/gh-workflows)

---

## Intro

⚙️ [Intro to Workflows](https://hotosm.github.io/gh-workflows/intro)

This repo contains reusable workflows, that can be called from any
other repo.

Motivations for creating this:

- Reduce code duplication for workflows across our repos.
- Easy version control and upgrading of our workflows over time.
- Attempt to find the best possible implementation of the workflow,
  then standardise across repos.

## To add new workflows

- Add your `.yml` workflow to `.github/workflows`.
- Create a `.md` file under `docs`, in the same format as the others.
  - The file should have empty content, with a title,
    and **##** headers: Inputs, Outputs, Secrets.
- Add to `.github/workflows/workflow_docs.yml`:
  - A step in the same format as others using `tj-actions/auto-doc@v3`,
    changing variables.
  - Add an entry under `files` in the **Verify Changed Files** step.
- Add an entry to `mkdocs.yml` nav section.

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
