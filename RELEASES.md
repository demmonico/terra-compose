# Releases

### v1.0
- init

### v1.1
- added init option for [backend config](https://developer.hashicorp.com/terraform/language/settings/backends/configuration#partial-configuration)
- added option to pass `tfvars` file name with or without file extension (also supports `.tfvars.json` and `.tfvars.hcl`)
- added before/after TF init and before/after action hooks

### v1.2
- added support for `AWS_DEFAULT_PROFILE` env var as a default value for `AWS_PROFILE`

### v1.3
- refactored run Terraform in container functions
- renamed host-based hooks; added container-based hook `before_container_run`
- improved functions and local vars naming

### v1.4 [DRAFT]
- enabled http backends by adding ability to skip workspace selection in aliases config
- fixed alias config parsing: prefix search for the configurations was replaced by exact match with more stable separator. Means, more stable alias config search
- added customisation for env files: customisable filename and filepath (`env_vars_file_name` and `env_vars_file` keys on both alias and default levels)
- enabled env file per project in aliases config


# TODOs

## Bugfixes
- FIX `tc run <ALIAS>` executes in last workspace, NOT in <ALIAS> workspace

## Features

### AWS access

- ADD handling AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY env vars within the AWS_PROFILE/AWS_DEFAULT_PROFILE

### Config and Env vars flexibility

- ADD tofu support (configurable runtime) !!!
- ADD multiple tfvars, to be able to include local !!!

- ADD consuming params from the command arguments - `terraform apply -var current=v2`
- ADD to alias config `env: TF_VAR_env2=test`
- ADD script that replaces env vars with placeholders inside code (to be used as dynamic module sourcing for example, e.g. `module-prep`)

- ADD customisation for aliases files (system-wise TC config + fallback to project one, customisable name for aliases file)
- ADD support for multiple alias configs, to be able to include local ones

- ADD configuring workspace during the call. Maybe withing split alias onto project and env - `./tc run runner_STG destroy -var-file=nonprod.tfvars -target=module.privileged_gitlab_runner_cluster`

### Actions enhancements

- ADD applying not based on prepared plan, but on-the-fly as well
- when run apply after plan (plan file exists), no needs to initialise again (mb consider a lifetime of the generated file as well)
- ADD an easy way to proxy for destroy and other commands

### Debuggin improvements

- ADD option to run `run` (or any??) command without ANY output. It's handy for capture output of some native TF commands, like `state`, `graph` etc
- ADD tc log
- implement verbosity cmd flags
