# Just Runner

Used to run Just commands configured via Justfile in the repo,
for simple CI/CD workflows.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

| INPUT                                                                                           | TYPE   | REQUIRED | DEFAULT          | DESCRIPTION                                                  |
| ----------------------------------------------------------------------------------------------- | ------ | -------- | ---------------- | ------------------------------------------------------------ |
| <a name="input_command"></a>[command](#input_command)                                           | string | true     |                  | The Just command to run <br>(defined in Justfile).           |
| <a name="input_environment"></a>[environment](#input_environment)                               | string | true     |                  | The Github environment to use <br>for variables.             |
| <a name="input_example_env_file_path"></a>[example_env_file_path](#input_example_env_file_path) | string | false    | `".env.example"` | Path to example dotenv file <br>to substitute variables for. |

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
