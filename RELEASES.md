# Releases

### v1.0
- init

### v1.1
- added init option for [backend config](https://developer.hashicorp.com/terraform/language/settings/backends/configuration#partial-configuration)
- added option to pass `tfvars` file name with or without file extension (also supports `.tfvars.json` and `.tfvars.hcl`)
- added before/after TF init and before/after action hooks

### v1.2
- added support for `AWS_DEFAULT_PROFILE` env var as a default value for `AWS_PROFILE`


# TODOs

## Bugfixes
- FIX hooks that are not executed in container
- FIX `tc run <ALIAS>` executes in last workspace, NOT in <ALIAS> workspace

## Features
- ADD handling AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY env vars within the AWS_PROFILE/AWS_DEFAULT_PROFILE
- ADD consuming params from the command arguments - `terraform apply -var current=v2`
- ADD script that replaces env vars with placeholders inside code (to be used as dynamic module sourcing for example, e.g. `module-prep`)
- ADD applying not based on prepared plan, but on-the-fly as well
- ADD configuring workspace during the call. Maybe withing split alias onto project and env - `./tc run runner_STG destroy -var-file=nonprod.tfvars -target=module.privileged_gitlab_runner_cluster`
- ADD option to run `run` (or any??) command without ANY output. It's handy for capture output of some native TF commands, like `state`, `graph` etc
- ADD tc log
- ADD customisation for env and aliases files
- implement verbosity cmd flags
- reformat code to follow conventions
