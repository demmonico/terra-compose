# Terra Compose

`Terra Compose` is a wrapper for calling Terraform commands in the Docker.
It aims to simplify the management of multiple Terraform projects within a single mono-repo. 
By solving problems with fragile and long maintenance and uncertainty about the correctness changes, caused by low visibility of the workspace.

For that, it follows the approach of the Docker Compose and puts all the needed information into the YAML config, which is visible and trackable by any VCS within the codebase. 
This way it gets rid of human involvement as much as possible, minimizing the risk of human error.

**Important links, that might be useful:** 
- _[An article in Medium](https://medium.com/@demmonico/multiple-terraform-projects-in-a-mono-repo-how-to-survive-a-mess-e1ec5a136d17), explaining the tool and idea behind it._
- _[A Conf42 DevOps 2024 presentation](https://youtu.be/R7Ias3EeIYI?si=uotLrdORP6SqO8ew), doing the same._

## Installation

To install package please follow the steps:
- [install Docker engine](https://docs.docker.com/get-docker/) _(used for running Terraform commands in the container)_
- [download executable](#download)
- [setup cloud credentials](#setup-cloud-credentials) _(depends on your cloud provider, but **was tested ONLY with AWS**)_

### Download

See [Tested section](#tested) that clarifies which versions of OS / Terraform / etc were tested.

#### Download for MacOS

Tested on `Monterey`/`Sonoma` + `zsh` + `Docker Desktop`:

```shell

```shell
$ wget https://raw.githubusercontent.com/demmonico/terra-compose/master/tc \
  && chmod +x tc \
  && sudo ln -s ${PWD}/tc /usr/local/bin/tc
```

### Setup cloud credentials

The tool should work well with any cloud providers, but **was tested ONLY with AWS**. Thus, the following steps are related to AWS.

#### AWS credentials setup

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

### Tested

This section clarifies which versions of OS / Terraform / etc were tested.

#### OS versions

Was tested in the following OS environments:
- `MacOS Monterey / Ventura / Sonoma / Sequoia`
- shell `bash / zsh`

**Note**: it wasn't tested in other OS. Feel free to create a MR to add installation details for other environments.

#### Terraform versions

Was tested in Terraform versions `>= 0.14`, but `<= 1.6`.
**Is NOT compatible** with Terraform versions `< 0.14` due to the breaking changes in Terraform CLI arguments


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
      before_tf_init:       "run any bash script or command on host"
      after_tf_init:        "run any bash script or command on host"
      before_all:           "run any bash script or command on host"
      after_all:            "run any bash script or command on host"
      before_container_run: "run any bash script or command in container"
```

### Example

You can find full demo following [terra-compose-demo repo](https://github.com/demmonico/terra-compose-demo) or check out next example:

```yaml
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

### Advanced

#### Pass env variables

It's possible to pass the environment variables to the engine. For that just create an `.env` file at the root project level (same level as project's alias configuration file is).


### Development

For development you need to clone this repo and link script to the `bin` directory:
- `git clone git@github.com:demmonico/terra-compose.git`
- check for the Git config:
  ```shell
  git config user.name
  git config user.email
  ```
- **[optional]** backup the current version if needed `sudo mv -f /usr/local/bin/tc /usr/local/bin/tc.bak`
- `sudo ln -s ${PWD}/tc /usr/local/bin/tc`


## Releases

[List of releases](RELEASES.md#releases)


## TODOs

[List of known/planned TODOs](RELEASES.md#todos)
