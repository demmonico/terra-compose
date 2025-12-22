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

Bugfixes:
- fixed `tc run <ALIAS>` executes in last workspace, NOT in <ALIAS> workspace. Since it was related to needs run custom commands, sometimes BEFORE workspace creation, default workspace selection was added to both, `tf` (ex. `run`) and `exec` (ex. `shell`) commands. However, using new `--skip-workspace` flag, it's still possible to archieve same goals.

Core functionality /Terraform:
- added an easy way to proxy for destroy and other commands or options. After `--` arg all will be passed to the respective runtime command
- removed support `plan-debug` and `apply-debug` actions in favour of usage `plan` and `apply` actions with new `-q|--quick` flag
- renamed `run` -> `tf` and `shell` -> `exec` actions
- added `-a|--auto-approve` CLI flag, allowing to auto-approve asks in optional places (all except apply)
- added `init` action, allowing to make a standalone init your IaaC tool at the given folder
- removed TF init run as a part of the apply action
- added full IaaC flow run, including init (skippable), plan and apply run

Config/Customisation:
- added consuming params from the command arguments - `terraform apply -var current=v2`. It was unlocked by passing all arguments after `--` arg to the respective runtime command
- added auto-approve system config param complimenting `-a|--auto-approve` CLI flag (see above)
- added lookup logic to fallback to default when alias-specific config was not found
- performance improvement reading configs, adding config cache, decreasing config files reads. Alias config: `17` -> `1`, system config: `9` -> `1`


# TODOs

## Features

### Debuggin improvements

- improve and standardise output messages
- ADD option to run `tf`/`exec` (or any??) command without ANY output. It's handy for capture output of some native TF commands, like `state`, `graph` etc
- implement verbosity cmd flags

### Misc

- self-update
