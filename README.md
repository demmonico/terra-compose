# Terra Compose

`Terra Compose` is a wrapper for calling Terraform commands in the Docker, 
solving the problem of the Terraform version drift and pain with the different sub-projects configurations in a mono-repo. 
As a configuration it uses a simple yaml file containing the list of projects and their configurations, which is visible and trackable by any VCS.

## Installation

To install package please follow the steps:
- [download executable](#download)
- [setup AWS credentials](#aws-credentials-setup)

### Versions

#### OS environment

Was tested in the following OS environment:
- `MacOS Monterey / Sonoma` + `zsh`

**Note**: it wasn't tested in other OS. Feel free to create a MR to add installation details for other environments. 

#### Terraform versions

Was tested in Terraform versions `>= 0.14`, but `<= 1.5.5`.
**Is NOT compatible** with Terraform versions `< 0.14` due to the breaking changes in Terraform CLI arguments

### Download

See [Versions section](#versions) that clarifies version specifics.

#### Download for MacOS

Tested on `Monterey`/`Sonoma` + `zsh`:

```shell
$ wget https://raw.githubusercontent.com/demmonico/terra-compose/master/tc \
  && chmod +x tc \
  && sudo ln -s ${PWD}/tc /usr/local/bin/tc
```

### AWS credentials setup

- get `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` env vars for your user with sufficient privileges from the AWS Console
- configure AWS CLI (see [docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) for details):
  ```shell
  $ aws configure --profile <YOUR_AWS_PROFILE_NAME>
  ```
  Follow all configuration steps, including filling out default region
- export the selected AWS profile in your current shell (works with both `AWS_DEFAULT_PROFILE` and `AWS_PROFILE`)
  ```shell
  $ export AWS_PROFILE=<YOUR_AWS_PROFILE_NAME>
  ```

## Configuration

Each projects, that you want to use `Terra Compose` with, should be presented in `Terra Compose`'s configuration file. 
Thus, in the root folder of the project, create file `aliases.yaml`. For details, see next sections 

### Default settings

Globally can be defined only Terraform version as a default value across the aliases. It's an optional section

```yaml
default:                                # [optional, used as a default across the aliases]
  tfversion: "x.x.x"                    # [optional, but if alias also does not have this section, an error will be thrown]
```

### Aliases settings

Aliases are the names, that `Terra Compose` can use to work with particular sub-project / environment. 
They are configured in the file `aliases.yaml` at the root folder of the project in the following format:

```yaml
aliases:
  alias_name:                           # [allowed only A-Za-z0-9_ symbols, SHOULD BE UNIQUE]
    path: "path/to/project/base/dir"    # [required]
    workspace: "live"                   # [optional, "default" will be used if exists and no more choice OR ask]
    tfvars: "nonprod"                   # [optional, workspace name will be used if skip OR ask, could be "-" for skipping tfvars attaching]
    tfversion: "x.x.x"                  # [optional, from the default section will be used if omitted]
    hooks:                              # [optional, scripts or commands to run before/after TF init or action in any combination]
      before_tf_init: "run any bash script or command"
      after_tf_init:  "run any bash script or command"
      before_action:  "run any bash script or command"
      after_action:   "run any bash script or command"
```

### Example

You can find full demo following [terra-compose-demo repo](https://github.com/demmonico/terra-compose-demo) or check out next example:

```yaml
# Terra Compose config
# 
# FORMAT:
#   default:                                [optional, used as a default across the aliases]
#     tfversion: "x.x.x"                    [optional, but if alias also does not have this section, an error will be thrown]
#   aliases:
#     alias_name:                           [allowed only A-Za-z0-9_ symbols]
#       path: "path/to/project/base/dir"'   [required]
#       workspace: "live"                   [optional, "default" will be used if exists and no more choice OR ask]
#       tfvars: "nonprod"                   [optional, workspace name will be used if skip OR ask]
#       tfversion: "x.x.x"                  [optional, from the default section will be used if omitted]
#       hooks:                              [optional, scripts or commands to run before/after TF init or action in any combination]
#         before_tf_init: "run any bash script or command"
#         after_tf_init:  "run any bash script or command"
#         before_action:  "run any bash script or command"
#         after_action:   "run any bash script or command"

default:
  tfversion: "0.12.0"

aliases:
  common:
    path: "common"
    workspace: "default"
    tfvars: "common"
  alerts:
    path: "projects/alerts"
    workspace: "default"
    tfversion: "1.3.0"

  runner_STAGING:
    path: "projects/runners"
    workspace: "staging"
    tfversion: "1.3.0"
  runner_PRODUCTION:
    path: "projects/runners"
    workspace: "production"
    tfversion: "1.3.0"
```

## Releases

[List of releases](RELEASES.md#releases)

## TODOs

[List of known/planned TODOs](RELEASES.md#todos)
