# Releases

### v1.0
- init



# TODOs

## Bugfixes
- FIX `tc run <ALIAS>` executes in last workspace, NOT in <ALIAS> workspace

## Features
- ADD handling AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY env vars within the AWS_PROFILE
- ADD consuming params from the command arguments - `terraform apply -var current=v2`
- ADD script that replaces env vars with placeholders inside code (to be used as dynamic module sourcing for example, e.g. `module-prep`)
- ADD applying not based on prepared plan, but on-the-fly as well
- ADD configuring workspace during the call. Maybe withing split alias onto project and env - `./tc run runner_STG destroy -var-file=nonprod.tfvars -target=module.privileged_gitlab_runner_cluster`
- ADD init option for [backend config](https://developer.hashicorp.com/terraform/language/settings/backends/configuration#partial-configuration)
