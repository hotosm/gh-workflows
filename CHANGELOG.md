# Changelog

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
