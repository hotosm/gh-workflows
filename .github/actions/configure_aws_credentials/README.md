# Action to authenticate to AWS either via OIDC or Access Keys

### About

Creates aws config file with fetched keys which can be later passed to container based actions (Ex: gruntwork-io/terragrunt-action@v2).

### For SSM actor based permissions

Write given content to file. Ex: `input.json`

```json
{
  "use_default": false,
  "include_claim_keys": ["repo", "context", "actor"]
}
```

Then call PUT API on github to send actor context too on `sub` claim field.

```sh
gh api -X PUT repos/hotosm/gh-workflows/actions/oidc/customization/sub --input input.json
```

Verify changes with:

```sh
gh api -X GET repos/hotosm/gh-workflows/actions/oidc/customization/sub
```
