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

### v2.0-alpha

Core functionality /Terraform:
- enabled http backends by adding ability to skip workspace selection in aliases config
- added multiple IaaC tools (OpenTofu) support via parsing system and aliases configs for `tftool` and `tfimage` values (default is still Terraform)
- added handling `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` env vars alongside the `AWS_PROFILE/AWS_DEFAULT_PROFILE`
- added ability to disable that check, since creds might be provided directly in provider's config omitting env vars. Plus, AWS might be not used at all...

Config/Customisation:
- fixed alias config parsing: prefix search for the configurations was replaced by exact match with more stable separator. Means, more stable alias config search
- added customisation for env files: customisable filename and filepath (`env_vars_file_name` and `env_vars_file` keys on both alias and default levels)
- enabled env file per project in aliases config
- rewritten yaml processor for the aliases config: added lists and maps parsing
- added env vars maps injection to the runtime container
- added env var `ALIAS_CONFIG` to override aliases config location (by default file `$(PWD).tc.yaml` will be looked for)
- added optional system config file, located by default in `~/.tc.yaml` (changable via env var `SYSTEM_CONFIG`). It works as a meta-config for some of the config values (at the moment only alias config location and skip value, and app verbosity level are supported)
- added multiple tfvars files support
- added override tfvars files support (they are auto-wired, thus could be omitted by VCS)
- added multiple `backend-config` params support
- added override `backend-config` files support (they are auto-wired, thus could be omitted by VCS)

### v2.0-beta

Core functionality /Terraform:
- added an easy way to proxy for destroy and other commands or options. After `--` arg all will be passed to the respective runtime command
- removed support `plan-debug` and `apply-debug` actions in favour of usage `plan` and `apply` actions with new `-q|--quick` flag
- renamed `run` -> `tf` and `shell` -> `exec` actions

Config/Customisation:
- added consuming params from the command arguments - `terraform apply -var current=v2`. It was unlocked by passing all arguments after `--` arg to the respective runtime command


# TODOs

## Bugfixes
- FIX `tc run <ALIAS>` executes in last workspace, NOT in <ALIAS> workspace

## Features

### Config and Env vars flexibility

- ADD merging default and alias configs logic
- ADD script that replaces env vars with placeholders inside code (to be used as dynamic module sourcing for example, e.g. `module-prep`)

- ADD configuring workspace during the call. Maybe withing split alias onto project and env - `./tc run runner_STG destroy -var-file=nonprod.tfvars -target=module.privileged_gitlab_runner_cluster`

### Actions enhancements

- ADD applying not based on prepared plan, but on-the-fly as well
- when run apply after plan (plan file exists), no needs to initialise again (mb consider a lifetime of the generated file as well)

### Debuggin improvements

- ADD option to run `run` (or any??) command without ANY output. It's handy for capture output of some native TF commands, like `state`, `graph` etc
- ADD tc log
- implement verbosity cmd flags
