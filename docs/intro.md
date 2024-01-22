# Github Workflows Intro

- Github workflows are for CI/CD in Github, similar to Gitlab pipelines.

- The name for the platform as a whole is Github Actions (although
  they are used quite interchangably).

- There is similar external CI/CD software (e.g. CircleCI), but
  having workflows close to the code is helpful for better integration.

- At HOT we also use Github Container Registry to store container images.

## Concepts

There is comprehensive [official documentation](https://docs.github.com/en/actions)
of Github Actions / Workflows.

Important Info:

- There are 3 levels of jobs: steps --> jobs --> workflows.

  - A step is the lowest level, typically running the actual code
    in a bash shell.
  - A job encapsulates many steps, to have an end outcome.
  - A workflow encapsulates many jobs.

- Each job in a workflow run on a seperate machine (runner),
  with different packages, containers, cache, etc.

- Typically workflows run on the `ubuntu-latest` image. Unless
  something simple, it is generally better practice to run your
  job inside a specifc container (for consistency).

### Types

#### Local Repo Workflows

- Located under `.github/workflows/workflow_name.yml`.
- Specific to your repo. Run on based on set criteria.

#### Reusable Local Workflows

- Also located under `.github/workflows/workflow_name.yml`.
- Use the `workflow_call` trigger, meaning they can only be called
  from another parent workflow.
- Useful if you repeat the same job multiple times, i.e. DRY code.

#### Reusable Remote Workflows

- Located under `.github/workflows/workflow_name.yml`, but within
  another Git repository.
- A centralised location can be used to store workflows to be called
  from other repos.
- Exactly what this repo is!

#### Github Actions

- A single workflow job packaged up for use / calling from within
  another workflow.
- There is a lot of overlap here with reusable workflows - they operate
  in a very similar way.
- The file structure is defined slightly differently, but the important
  thing to note is that a single job is defined per published repo.
- HOT does not maintain any Actions as of now, but we use plenty of
  official and unofficial Actions within our workflows.

### Triggers

There are quite a few triggers.
The main ones to note are below.

#### push

- A standard push to the repo.
- Useful for deployment & publishing workflows.
- Workflow runs on the branch you merged into (e,g. main, development).
- Can specify which branch, or which files to trigger on.
- Also covers when a PR is merged (this is essentially a push too).
- Includes tag pushes.

#### pull_request

- Runs when a PR is made.
- Useful for testing workflows.
- Runs on the PR source branch (i.e. the one you want to merge).

#### pull_request_target

- Runs when a PR is made, BUT runs on the _target_ branch instead.
- For `feat/some-feat` --> `main`, then would be the `main` branch.
- Give's some extra permissions to the workflow, so use with caution.
- We only use this for the **PR Label** workflow, as it allows for labelling
  of the PR, even if the PR author does not have repo permissions.

#### workflow_call

- Runs when called from another workflow.
- What we use mostly throughout this repo, to make 'reusable' workflows.

## Using Reusable Workflows

You need two keys to run a reusable workflow: `uses` and `with`.

Example:

```yaml
frontend-build:
  uses: hotosm/gh-workflows/.github/workflows/image_build.yml@main
  needs: [frontend-tests]
  with:
    context: src/frontend
    dockerfile: prod.dockerfile
    build_target: prod
    image_name: ghcr.io/${{ github.repository }}/frontend
    extra_build_args: |
      APP_VERSION=${{ github.ref_name }}
      COMMIT_REF=${{ github.sha }}
      VITE_API_URL=${{ vars.URL_SCHEME }}://${{ vars.API_URL }}"
```

As you can see, `uses` is to define the workflow you want to run:

> The version of the workflow can be specified after the `@` symbol.
> It is bad practic to use @main. Generally you should release tagged
> versions of the reusable workflows, then specify the tag, e.g. @0.1.2

The `with` key is used to specify all of the **inputs** to pass to the workflow.
(these are predefined by the creator of the reusable workflow).

### Using Secrets in Reusable Workflows

By default reusable workflows will not have access to environment
secrets, unless specified in the workflow definition.

There are two ways to do this:

1. Specify secrets individually:

   ```yaml
   jobs:
     pytest:
       uses: hotosm/gh-workflows/.github/workflows/some_workflow.yml@main
       secrets:
         SECRET_VAR_1: ${{ secrets.SECRET_VAR_1 }}
         SECRET_VAR_2: ${{ secrets.SECRET_VAR_2 }}
   ```

2. Inherit all secrets (recommended):

   ```yaml
   jobs:
     pytest:
       uses: hotosm/gh-workflows/.github/workflows/some_workflow.yml@main
       secrets: inherit
   ```

## Passing Info Between Workflows

As noted above, each reusable workflow runs on a different machine (runner).

There are various ways to pass information between different workflow jobs.

### Environment Variables

Any variable in $GITHUB_ENV will be available to all jobs in the workflow.

Set a variable:

```bash
echo "README_PATH=./README.md" >> $GITHUB_ENV
```

Read a variable:

```bash
# Read the content of the file
cat $README_PATH
# Alternative syntax
cat ${{ env.README_PATH }}
```

### Outputs

Outputs can be defined on a **step** level, **job** level, and **workflow** level.

#### Step Output

Typically the output is defined by code within a step:

```bash
# ... some code to determine var_content
echo "var1=${var_content}" >> $GITHUB_OUTPUT
```

This can be read in another step (using the step id):

```yaml
steps:
  - id: first-task
    name: Set the var
    run: |
      echo "var1=${var_content}" >> $GITHUB_OUTPUT

  - name: Read the var
    run: |
      echo ${{ steps.first-task.outputs.var1 }}
```

#### Job Output

Outputs defined in a step can be set a job outputs,
making them available to other jobs in the workflow.

```yaml
jobs:
  build-image:
    runs-on: ubuntu-latest

    outputs:
      image_name: ${{ steps.get_image_name.outputs.image_name }}
      image_tag: ${{ steps.get_image_name.outputs.image_tag }}
```

This can be read by another job in the workflow:

> To use the output from a workflow, we have to use the
> `needs` keyword to create a linking between two jobs.

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest

    outputs:
      var1: ${{ steps.first-task.outputs.var1 }}

    steps:
      - id: first-task
        name: Set the var
        run: |
          echo "var1=${var_content}" >> $GITHUB_OUTPUT

  job2:
    runs-on: ubuntu-latest
    needs: [job1]

    steps:
      - name: Get the var
        run: |
          echo "${{ needs.job1.outputs.var1 }}"
```

#### Workflow Output

- Outputs can also be passed up to the workflow level.
- This is especially useful for reusable workflows.
- We can define an standard output for the workflow, then
  pass the value onto our next called workflow.

> As with passing data between jobs, we also have to use
> the `needs` keyword to pass data between workflows.

```yaml
# some_workflow.yml

on:
  workflow_call:
    outputs:
      var1:
        description: "The test var."
        value: ${{ jobs.job1.outputs.var1 }}

jobs:
  job1:
    runs-on: ubuntu-latest

    outputs:
      var1: ${{ steps.first-task.outputs.var1 }}

    steps:
      - id: first-task
      name: Set the var
      run: |
          echo "var1=${var_content}" >> $GITHUB_OUTPUT
```

We can then use this in our parent workflow:

```yaml
do-something:
  uses: hotosm/gh-workflows/.github/workflows/some_workflow.yml@main

do-something-else:
  uses: hotosm/gh-workflows/.github/workflows/some_workflow2.yml@main
  needs: [do-something]
  with:
    # Run workflow with a variable output from previous workflow
    some_variable: ${{ needs.do-something.outputs.var1 }}
```

### Cache

Caching is used for persisting data **across** workflow multiple runs.

Example flow:

1. Workflow pulls a container image to run a task.
2. This workflow runs the first time on a pull request.
3. The container image is cached.
4. The developer makes some edits and pushes new commits to the PR.
5. The cache is hit for the PR, so the container image does not download
   again.

As you can see, this is a useful efficiency gain for workflows running
many times consecutively.

> Caches are scoped to a branch, so the cache for `main` is
> not available within a PR.

### Artifacts

Artifacts are used for persisting files **within** a single workflow run.

Example flow:

1. Workflow 1 creates file
2. Uploads as artifact
3. Workflow 2 downloads artifacts
4. File can be used in workflow 2.

Artifact also make file outputs available via the Github CLI (e.g.
for making releases that the user can download).
