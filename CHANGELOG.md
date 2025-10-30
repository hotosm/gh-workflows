# Changelog

## 3.4.1 (2025-10-30)

### Fix

- ensure env and secrets exported prior to justfile deploy

## 3.4.0 (2025-10-30)

### Feat

- add remote_deploy_just workflow

## 3.3.3 (2025-08-19)

### Fix

- add concurrency=1 for mkdocs deploy workflow

## 3.3.2 (2025-06-28)

### Fix

- set default for TAG_OVERRIDE --> ci-dev

## 3.3.1 (2025-06-27)

### Fix

- checkout repo in just workflow
- add example env var + GITHUB_TOKEN var to just

### Refactor

- rename parent task of just job

## 3.3.0 (2025-06-27)

### Feat

- add just workflow for running any task

### Refactor

- remove debug_override param, not required

## 3.2.3 (2025-06-27)

### Fix

- set debug_override param to default false

## 3.2.2 (2025-06-27)

## 3.2.1 (2025-05-06)

## 3.2.0 (2025-04-01)

### Feat

- action to subsitute env example from GITHUB_ENV
- action to export vars N secret to GITHUB_ENV

### Fix

- hardcode VITE_S3_URL for FMTM (broken env var docker args)

## 3.1.2 (2025-03-18)

## 3.1.1 (2025-03-03)

### Fix

- hardcode VITE_MAPPER_URL until env var loading fixed

## 3.1.0 (2025-02-11)

## 3.0.0 (2025-02-11)

### Feat

- add terragrunt plan and apply reusable workflow
- action to authenticate to AWS via OIDC/ACCESS_KEYS

## 2.0.9 (2024-11-18)

## 2.0.8 (2024-11-13)

### Fix

- hardcode VITE_SYNC_URL param until all env var loading works

## 2.0.7 (2024-11-12)

### Feat

- add skip_cve option to img_build workflow

## 2.0.6 (2024-10-24)

### Fix

- update trivy scan action with fallback db (rate-limiting)

## 2.0.5 (2024-08-01)

### Fix

- mount playwright-report path correctly in docker
- run playwright report upload even if tests fail
- remove build_img var, replace with omitting image_name var
- add option to upload playwright traces.zip for debugging

## 2.0.4 (2024-07-27)

### Fix

- add extra step for pnpm_build to configure gh-pages

## 2.0.3 (2024-07-27)

### Fix

- add stage to deploy gh-pages directly to pnpm_build workflow

## 2.0.2 (2024-07-27)

### Fix

- add option to upload pnpm_artifact in gh-pages format

## 2.0.1 (2024-07-26)

### Fix

- artifact-upload key --> name variable

### Refactor

- rename stories_build to more generic pnpm_build
- replace upload/download artifact workflows with v4

## 2.0.0 (2024-07-26)

### BREAKING CHANGE

- Rename to workflow remote_deploy (allow for other remote deploy types)

### Feat

- add option to not build image for test_compose

### Fix

- make compose_command not mandatory for test_compose
- add check if envsubst download success, else exit workflow

### Refactor

- rename remote_deploy --> remote_deploy_compose

## 1.6.0 (2024-07-04)

### Feat

- upgrade to latest action versions (except artifact upload)

## 1.5.2 (2024-05-29)

### Refactor

- update WITH_MONITORING var --> MONITORING for docker builds

## 1.5.1 (2024-04-29)

### Fix

- add WITH_MONITORING as image build arg until pass in fixed

## 1.5.0 (2024-04-10)

### Feat

- allow specifying a container for test_pnpm, for e.g. playwright

### Fix

- add optional run_command var for test_pnpm workflow

## 1.4.10 (2024-04-08)

### Fix

- track bridgecrewio/checkov @v12 (updated via patches)

## 1.4.9 (2024-02-25)

### Fix

- add NODE_ENV var to images builds

## 1.4.8 (2024-02-24)

### Fix

- configurable example dotenv file location
- allow tests to pass if coverage files have no update
- do not download envsubst if not required

### Refactor

- fix typo in openapi json workflow

## 1.4.7 (2024-02-06)

### Fix

- substitute example.env prior to openapi.json gen

## 1.4.6 (2024-01-31)

### Fix

- correctly set image labels from meta action

## 1.4.5 (2024-01-23)

### Fix

- add keep_extra_files flag to mkdocs_build workflow

## 1.4.4 (2024-01-23)

### Fix

- export .env vars to env prior to echo
- coverage step push HEAD:gh-pages
- coverage push using separate dir for repo
- force move coverage files to overwrite
- add packages: write to test_compose for build downstream
- use gh-pages over wiki for coverage push
- conditional to trigger coverage for test_compose
- update test_compose for optional test coverage upload

### Refactor

- fix trigger condition for coverage in test_compose
- update git user for wiki sync workflow

## 1.4.3 (2024-01-21)

### Fix

- wiki publishing workflow using sudo apt

### Refactor

- typo in stories workflow name

## 1.4.2 (2024-01-05)

### Fix

- add --no-TTY to docker compose run

## 1.4.1 (2023-11-30)

### Refactor

- skip dockerfile scan for testing workflow builds

## 1.4.0 (2023-11-23)

### Feat

- add image_build_multi workflow

### Fix

- default multi_arch removed for test workflows

### Refactor

- update all refs 1.3.2 --> 1.4.0

## 1.3.2 (2023-11-23)

### Fix

- image_build config dockerfile vulnerability scan

### Refactor

- update image_build usage --> 1.3.2

## 1.3.1 (2023-11-22)

### Fix

- correct pin for trivy v0.14.0 --> 0.14.0
- multi-arch image_build conditional trigger
- update all usage of image_build --> 1.3.1
- update image_build action versions, pin all

## 1.3.0 (2023-11-20)

### Feat

- add env_vars workflow to extract all env vars

### Fix

- add stories_build, plus mkdocs_build compatibility
- is_empty_json check during image_build context check
- re-add VITE_API_URL hardcode until fixed

### Refactor

- remove VITE_API_URL hardcode from image_build

## 1.2.4 (2023-11-10)

### Fix

- handle cases when no .env.example in repo

## 1.2.3 (2023-11-10)

### Fix

- update all env releated workflows to not fail if empty

## 1.2.2 (2023-11-10)

### Fix

- mkdocs_build run git config prior to gh-deploy
- update mkdocs_build to use docker run over container
- update workflows refs to use latest tag
- test workflow use build_dockerfile arg correctly
- enable environment var injection to test_pytest

### Refactor

- replace @main --> @1.2.2 for docs build

## 1.2.1 (2023-11-02)

### Fix

- add pre_command input options for test_compose

## 1.2.0 (2023-11-01)

### Feat

- add npm_publish workflow

## 1.1.3 (2023-11-01)

### Fix

- add --force-recreate to remote deploy command

## 1.1.2 (2023-10-27)

### Fix

- hardcode passing VITE_API_URL to build-args

## 1.1.1 (2023-10-25)

### Fix

- test_compose incorrect linked input names

## 1.1.0 (2023-10-24)

### Feat

- replace test_pytest_compose with generic test_compose

## 1.0.1 (2023-10-23)

### Fix

- add is_empty_json check for VARS_CONTEXT
- pytest_test pin image_build=1.0.1
- pytest extraction of image name from tar
- case where no build args passed to image_build

### Refactor

- update workflows to use tagged versions

## 1.0.0 (2023-10-23)

### Feat

- update image_build to input GITHUB_ENV to build args
- add get_env_vars workflow to extract env vars
- add test_pytest workflow
- first version of test_pytest (no compose)
- rename pytest_compose --> test_pytest_compose, add test_pnpm
- add pytest_compose workflow
- also output image_tag from image_build
- add option to skip scan on image build
- rename image_cache --> image_artifact and docs
- update doc workflows to use artifacts over caches
- add remote_deploy workflow for docker-compose/ssh
- update cache_image to use artifacts over cache
- image_build replace checkcov with trivy scan
- update docker build workflow - cache, multiarch, autotag
- add python app version extract workflow

### Fix

- hardcode VITE_API_URL until fix for parsing all vars
- use fromJSON to parse env for build-args
- use multiline parser for image_build var outputs
- attempt set ALL_VARS and ALL_SECRETS via outputs
- update syntax for setting ALL_VARS and ALL_SECRETS
- parse all vars and secrets into build_args
- syntax for github.env var in image_build args
- automatically determine environment for image_build
- replace build_args with extra_build_args
- use latest $env:GITHUB_OUTPUT syntax for parsing
- correctly set inputs on env_vars workflow
- pytest workflow, set IMAGE var in loop
- DOCKER_HOST url parsing on deploy
- get ssh variables from github env on deploy
- accept multiline env, envsubst with .env.example
- ignore CKV_DOCKER_5 due to false positives
- typo image --> image_name during save local imgs
- mount tests inside container during pytest
- pulling img prior to package/save in pytest
- rename image_full_name var, replace with image_name
- full extracting image_full_name for pytest
- dollar syntax for test_pytest, placeholder
- don't pull new built images, fix extra_img pkg
- replace dollar in workflow with [dollar]
- escaping of github var in input label
- update pytest_compose to take generic build args
- allow dockerfiles without user creation
- specific file for checkcov action
- remove directory arg from checkcov (only run dockerfile)
- doxygen tar upload, download untar
- allow string literal input for image_artifact
- correctly set image_tag output on image_build
- image_tags output not set correctly
- prevent scan on false, echo image name
- package permissions write for image build
- add permissions: write for image building
- wait for docker pull to complete before pkg
- strip dots from imahe name on package
- use scalar over literal for image listing
- dotenv pass env var directly to jq, not github var
- to_envs single .env file parsing
- use correct multiline syntac for gh workflows
- add github specific parsing of multiline strings
- build_image setting image_name output for workflow
- image_build only return first image name
- image_cache use spaces for IFS split
- image_cache workflow with join operator
- parsing of image_names_array using while loop
- image_name array parsing for image_cache
- replace colons in artifact upload path
- update cache image path /tmp/images
- image_cache split image list prior to iterate
- download-artifact --> upload-artifact for image_cache
- use trivy action @master branch (latest)
- pin trivy action without prepend v
- run dockerfile lint in directory (single file only)
- also skip dockerfile CKV_DOCKER_2 (HEATHCHECK)
- image_build scan stages, output image_name
- image_build inputs.image_name to not required
- image_build missed double pipe or operator
- update image cache to use registry_prefix var
- image cache workflow to not need subdirs
- dont use input.image_name in job ids
- add key and restore-key to mkdocs cache
- incorrect job selection for py_version output
- direct reference package_version input for py_version
- py_app_version workflow, keep root dir
- write cache after docs are built
- image_build workflow, defualt dockerfile dir

### Refactor

- additional logs during .env creation
- add echo for extracted image_name in pytest
- add extra comments for test_pytest_compose
- rename image_cache --> image_artifact throughout
- correctly each images_array on cache
- image_cache rename cache_key to artifact_name
- use env.XXX syntax for referencing remote_deploy env
- simplify remote_deploy workflow, vars not required
- add extra echo statements to image_cache
- simplify image caching workflow
- add additional log to image_cache package
- image_build move scan step to separate job
- rename image cache job
- cd for doxygen --> working-directory
- rename image build stage
- move reusable workflows directory
