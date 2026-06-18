# `dev-environment`

The `dev-environment` configuration type allows running [dev environments](../../concepts/dev-environments.md).

## Root reference

###### `ide` - (Optional) `"cursor" | "vscode" | "windsurf" | "zed"` The IDE to pre-install. Supported values include `vscode`, `cursor`, `windsurf`, and `zed`. Defaults to no IDE (SSH only). { #ide data-toc-label='ide' class='reference-item' }
###### `version` - (Optional) `str` The version of the IDE. For `windsurf`, the version is in the format `version@commit`. { #version data-toc-label='version' class='reference-item' }
###### `init` - (Optional) `list[str]` The shell commands to run on startup. { #init data-toc-label='init' class='reference-item' }
###### `inactivity_duration` - (Optional) `bool | int | str | "off"` The maximum amount of time the dev environment can be inactive (e.g., `2h`, `1d`, etc). After it elapses, the dev environment is automatically stopped. Inactivity is defined as the absence of SSH connections to the dev environment, including VS Code connections, `ssh <run name>` shells, and attached `dstack apply` or `dstack attach` commands. Use `off` for unlimited duration. Can be updated in-place. Defaults to `off`. { #inactivity_duration data-toc-label='inactivity_duration' class='reference-item' }
###### [`ports`](#ports) - (Optional) `list[int | str | object]` Port numbers/mapping to expose. { #_ports data-toc-label='ports' class='reference-item' }
###### `name` - (Optional) `str` The run name. If not specified, a random name is generated. { #name data-toc-label='name' class='reference-item' }
###### `image` - (Optional) `str` The name of the Docker image to run. { #image data-toc-label='image' class='reference-item' }
###### `user` - (Optional) `str` The user inside the container, `user_name_or_id[:group_name_or_id]` (e.g., `ubuntu`, `1000:1000`). Defaults to the default user from the `image`. { #user data-toc-label='user' class='reference-item' }
###### `privileged` - (Optional) `bool` Run the container in privileged mode. Defaults to `false`. { #privileged data-toc-label='privileged' class='reference-item' }
###### `entrypoint` - (Optional) `str` The Docker entrypoint. { #entrypoint data-toc-label='entrypoint' class='reference-item' }
###### `working_dir` - (Optional) `str` The absolute path to the working directory inside the container. Defaults to the `image`'s default working directory. { #working_dir data-toc-label='working_dir' class='reference-item' }
###### [`registry_auth`](#registry_auth) - (Optional) `object` Credentials for pulling a private Docker image. { #_registry_auth data-toc-label='registry_auth' class='reference-item' }
###### `python` - (Optional) `"3.10" | "3.11" | "3.12" | "3.13" | "3.9"` The major version of Python. Mutually exclusive with `image` and `docker`. { #python data-toc-label='python' class='reference-item' }
###### `nvcc` - (Optional) `bool` Use image with NVIDIA CUDA Compiler (NVCC) included. Mutually exclusive with `image` and `docker`. { #nvcc data-toc-label='nvcc' class='reference-item' }
###### `single_branch` - (Optional) `bool` Whether to clone and track only the current branch or all remote branches. Relevant only when using remote Git repos. Defaults to `false` for dev environments and to `true` for tasks and services. { #single_branch data-toc-label='single_branch' class='reference-item' }
###### [`env`](#env) - (Optional) `list[str] | dict` The mapping or the list of environment variables. { #_env data-toc-label='env' class='reference-item' }
###### `shell` - (Optional) `str` The shell used to run commands. Allowed values are `sh`, `bash`, or an absolute path, e.g., `/usr/bin/zsh`. Defaults to `/bin/sh` if the `image` is specified, `/bin/bash` otherwise. { #shell data-toc-label='shell' class='reference-item' }
###### [`resources`](#resources) - (Optional) `object` The resources requirements to run the configuration. { #_resources data-toc-label='resources' class='reference-item' }
###### `priority` - (Optional) `int` The priority of the run, an integer between `0` and `100`. `dstack` tries to provision runs with higher priority first. Defaults to `0`. { #priority data-toc-label='priority' class='reference-item' }
###### [`volumes`](#volumes) - (Optional) `list[object]` The volumes mount points. { #_volumes data-toc-label='volumes' class='reference-item' }
###### `docker` - (Optional) `bool` Use Docker inside the container. Mutually exclusive with `image`, `python`, and `nvcc`. Overrides `privileged`. { #docker data-toc-label='docker' class='reference-item' }
###### [`repos`](#repos) - (Optional) `list[object]` The list of Git repos. { #_repos data-toc-label='repos' class='reference-item' }
###### [`files`](#files) - (Optional) `list[object]` The local to container file path mappings. { #_files data-toc-label='files' class='reference-item' }
###### `backends` - (Optional) `list["amddevcloud" | "aws" | "azure" | "cloudrift" | "crusoe" | "cudo" | "datacrunch" | "digitalocean" | "dstack" | "gcp" | "hotaisle" | "jarvislabs" | "kubernetes" | "lambda" | "remote" | "nebius" | "oci" | "runpod" | "tensordock" | "vastai" | "verda" | "vultr"]` The backends to consider for provisioning (e.g., `[aws, gcp]`). { #backends data-toc-label='backends' class='reference-item' }
###### `regions` - (Optional) `list[str]` The regions to consider for provisioning (e.g., `[eu-west-1, us-west4, westeurope]`). { #regions data-toc-label='regions' class='reference-item' }
###### `availability_zones` - (Optional) `list[str]` The availability zones to consider for provisioning (e.g., `[eu-west-1a, us-west4-a]`). { #availability_zones data-toc-label='availability_zones' class='reference-item' }
###### `instance_types` - (Optional) `list[str]` The cloud-specific instance types to consider for provisioning (e.g., `[g6e.24xlarge, n1-standard-4]`). { #instance_types data-toc-label='instance_types' class='reference-item' }
###### `reservation` - (Optional) `str` The existing reservation to use for instance provisioning. Supports AWS Capacity Reservations, AWS Capacity Blocks, and GCP reservations. { #reservation data-toc-label='reservation' class='reference-item' }
###### `spot_policy` - (Optional) `"auto" | "on-demand" | "spot"` The policy for provisioning spot or on-demand instances: `spot`, `on-demand`, `auto`. Defaults to `on-demand`. { #spot_policy data-toc-label='spot_policy' class='reference-item' }
###### [`retry`](#retry) - (Optional) `bool | object` The policy for resubmitting the run. Defaults to `false`. { #_retry data-toc-label='retry' class='reference-item' }
###### `max_duration` - (Optional) `int | str | "off"` The maximum duration of a run (e.g., `2h`, `1d`, etc) in a running state, excluding provisioning and pulling. After it elapses, the run is automatically stopped. Use `off` for unlimited duration. Defaults to `off`. { #max_duration data-toc-label='max_duration' class='reference-item' }
###### `stop_duration` - (Optional) `int | str | "off"` The maximum duration of a run graceful stopping. After it elapses, the run is automatically forced stopped. This includes force detaching volumes used by the run. Use `off` for unlimited duration. Defaults to `5m`. { #stop_duration data-toc-label='stop_duration' class='reference-item' }
###### `max_price` - (Optional) `float` The maximum instance price per hour, in dollars. { #max_price data-toc-label='max_price' class='reference-item' }
###### `creation_policy` - (Optional) `"reuse" | "reuse-or-create"` The policy for using instances from fleets: `reuse`, `reuse-or-create`. Defaults to `reuse-or-create`. { #creation_policy data-toc-label='creation_policy' class='reference-item' }
###### `idle_duration` - (Optional) `int | str` Time to wait before terminating idle instances. When the run reuses an existing fleet instance, the fleet's `idle_duration` applies. When the run provisions a new instance, the shorter of the fleet's and run's values is used. Defaults to `5m` for runs and `3d` for fleets. Use `off` for unlimited duration. Only applied for VM-based backends. { #idle_duration data-toc-label='idle_duration' class='reference-item' }
###### [`utilization_policy`](#utilization_policy) - (Optional) `object` Run termination policy based on utilization. { #_utilization_policy data-toc-label='utilization_policy' class='reference-item' }
###### `startup_order` - (Optional) `"any" | "master-first" | "workers-first"` The order in which master and workers jobs are started: `any`, `master-first`, `workers-first`. Defaults to `any`. { #startup_order data-toc-label='startup_order' class='reference-item' }
###### `stop_criteria` - (Optional) `"all-done" | "master-done"` The criteria determining when a multi-node run should be considered finished: `all-done`, `master-done`. Defaults to `all-done`. { #stop_criteria data-toc-label='stop_criteria' class='reference-item' }
###### [`schedule`](#schedule) - (Optional) `object` The schedule for starting the run at specified time. { #_schedule data-toc-label='schedule' class='reference-item' }
###### `fleets` - (Optional) `list[str | object]` The fleets considered for reuse. For fleets owned by the current project, specify fleet names. For imported fleets, specify `<project name>/<fleet name>`. { #fleets data-toc-label='fleets' class='reference-item' }
###### `instances` - (Optional) `list` The specific fleet instances to consider for reuse. Each value can be an instance name string, or an object with `name`, `hostname`, or `fleet` and `instance`. When set, the run is only placed on matching existing instances.. { #instances data-toc-label='instances' class='reference-item' }
###### `tags` - (Optional) `dict` The custom tags to associate with the resource. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }
###### `backend_options` - (Optional) `list[object]` Backend-specific options, applied only to offers from that backend. { #backend_options data-toc-label='backend_options' class='reference-item' }


### `retry`

###### `on_events` - (Optional) `list["no-capacity" | "interruption" | "error"]` The list of events that should be handled with retry. Supported events are `no-capacity`, `interruption`, `error`. Omit to retry on all events. { #on_events data-toc-label='on_events' class='reference-item' }
###### `duration` - (Optional) `int | str` The maximum period of retrying the run, e.g., `4h` or `1d`. The period is calculated as a run age for `no-capacity` event and as a time passed since the last `interruption` and `error` for `interruption` and `error` events.. { #duration data-toc-label='duration' class='reference-item' }


### `utilization_policy`

###### `min_gpu_utilization` - (Required) `int` Minimum required GPU utilization, percent. If any GPU has utilization below specified value during the whole time window, the run is terminated. { #min_gpu_utilization data-toc-label='min_gpu_utilization' class='reference-item' }
###### `time_window` - (Required) `int | str` The time window of metric samples taking into account to measure utilization (e.g., `30m`, `1h`). Minimum is `5m`. { #time_window data-toc-label='time_window' class='reference-item' }


### `schedule`

###### `cron` - (Required) `str | list[str]` A cron expression or a list of cron expressions specifying the UTC time when the run needs to be started. { #cron data-toc-label='cron' class='reference-item' }


### `instances[n]` { #_instances data-toc-label="instances" }

When `instances` is set, the run is placed only on matching existing fleet instances.

=== "By name"

    ###### `name` - (Required) `str` The fleet instance name. { #name data-toc-label='name' class='reference-item' }


=== "By hostname"

    ###### `hostname` - (Required) `str` The fleet instance hostname or IP address. { #hostname data-toc-label='hostname' class='reference-item' }


=== "By fleet and instance number"

    ###### [`fleet`](#fleet) - (Required) `str | object` The fleet reference. For fleets owned by the current project, specify the fleet name. For a fleet from another project, specify `<project name>/<fleet name>` or an object with `project` and `name`.. { #_fleet data-toc-label='fleet' class='reference-item' }
    ###### `instance` - (Required) `int` The fleet instance number. { #instance data-toc-label='instance' class='reference-item' }


??? info "Short syntax"

    The short syntax for instances is an instance name string.

    * `my-fleet-1`, same as `{name: my-fleet-1}`

### `resources`

###### [`cpu`](#resources-cpu) - (Optional) `int | str | object` The CPU requirements. { #_cpu data-toc-label='cpu' class='reference-item' }
###### `memory` - (Optional) `int | str` The RAM size (e.g., `8GB`). Defaults to `8GB..`. { #memory data-toc-label='memory' class='reference-item' }
###### `shm_size` - (Optional) `int | str` The size of shared memory (e.g., `8GB`). If you are using parallel communicating processes (e.g., dataloaders in PyTorch), you may need to configure this. { #shm_size data-toc-label='shm_size' class='reference-item' }
###### [`gpu`](#resources-gpu) - (Optional) `int | str | object` The GPU requirements. { #_gpu data-toc-label='gpu' class='reference-item' }
###### [`disk`](#resources-disk) - (Optional) `int | str | object` The disk resources. { #_disk data-toc-label='disk' class='reference-item' }


#### `resources.cpu` { #resources-cpu data-toc-label="cpu" }

###### `arch` - (Optional) `"arm" | "x86"` The CPU architecture, one of: `x86`, `arm`. { #arch data-toc-label='arch' class='reference-item' }
###### `count` - (Optional) `int | str` The number of CPU cores. Defaults to `2..`. { #count data-toc-label='count' class='reference-item' }


#### `resources.gpu` { #resources-gpu data-toc-label="gpu" }

###### `vendor` - (Optional) `"amd" | "google" | "intel" | "nvidia" | "tenstorrent"` The vendor of the GPU/accelerator, one of: `nvidia`, `amd`, `google` (alias: `tpu`), `intel`. { #vendor data-toc-label='vendor' class='reference-item' }
###### `name` - (Optional) `str | list[str]` The name of the GPU (e.g., `A100` or `H100`). { #name data-toc-label='name' class='reference-item' }
###### `count` - (Optional) `int | str` The number of GPUs. Defaults to `1..`. { #count data-toc-label='count' class='reference-item' }
###### `memory` - (Optional) `int | str` The RAM size (e.g., `16GB`). Can be set to a range (e.g. `16GB..`, or `16GB..80GB`). { #memory data-toc-label='memory' class='reference-item' }
###### `total_memory` - (Optional) `int | str` The total RAM size (e.g., `32GB`). Can be set to a range (e.g. `16GB..`, or `16GB..80GB`). { #total_memory data-toc-label='total_memory' class='reference-item' }
###### `compute_capability` - (Optional) `float | str` The minimum compute capability of the GPU (e.g., `7.5`). { #compute_capability data-toc-label='compute_capability' class='reference-item' }


#### `resources.disk` { #resources-disk data-toc-label="disk" }

###### `size` - (Required) `int | str` Disk size. { #size data-toc-label='size' class='reference-item' }


### `registry_auth`

###### `username` - (Required) `str` The username. { #username data-toc-label='username' class='reference-item' }
###### `password` - (Required) `str` The password or access token. { #password data-toc-label='password' class='reference-item' }


### `volumes[n]` { #_volumes data-toc-label="volumes" }

=== "Network volumes"

    ###### `name` - (Required) `str | list[str]` The network volume name or the list of network volume names to mount. If a list is specified, one of the volumes in the list will be mounted. Specify volumes from different backends/regions to increase availability. { #name data-toc-label='name' class='reference-item' }
    ###### `path` - (Required) `str` The absolute container path to mount the volume at. { #path data-toc-label='path' class='reference-item' }


=== "Instance volumes"

    ###### `instance_path` - (Required) `str` The absolute path on the instance (host). { #instance_path data-toc-label='instance_path' class='reference-item' }
    ###### `path` - (Required) `str` The absolute path in the container. { #path data-toc-label='path' class='reference-item' }
    ###### `optional` - (Optional) `bool` Allow running without this volume in backends that do not support instance volumes. Defaults to `false`. { #optional data-toc-label='optional' class='reference-item' }


??? info "Short syntax"

    The short syntax for volumes is a colon-separated string in the form of `source:destination`

    * `volume-name:/container/path` for network volumes
    * `/instance/path:/container/path` for instance volumes

### `repos[n]` { #_repos data-toc-label="repos" }

> Currently, a maximum of one repo is supported.

> Either `local_path` or `url` must be specified.

###### `local_path` - (Optional) `str` The path to the Git repo on the user's machine. Relative paths are resolved relative to the parent directory of the the configuration file. Mutually exclusive with `url`. { #local_path data-toc-label='local_path' class='reference-item' }
###### `url` - (Optional) `str` The Git repo URL. Mutually exclusive with `local_path`. { #url data-toc-label='url' class='reference-item' }
###### `branch` - (Optional) `str` The repo branch. Defaults to the active branch for local paths and the default branch for URLs. { #branch data-toc-label='branch' class='reference-item' }
###### `hash` - (Optional) `str` The commit hash. { #hash data-toc-label='hash' class='reference-item' }
###### `path` - (Optional) `str` The repo path inside the run container. Relative paths are resolved relative to the working directory. Defaults to `.`. { #path data-toc-label='path' class='reference-item' }
###### `if_exists` - (Optional) `"error" | "skip"` The action to be taken if `path` exists and is not empty. One of: `error`, `skip`. Defaults to `error`. { #if_exists data-toc-label='if_exists' class='reference-item' }


??? info "`if_exists` action"

    If the `path` already exists and is a non-empty directory, by default the run is terminated with an error.
    This can be changed with the `if_exists` option:

    * `error` – do not try to check out, terminate the run with an error (the default action since `0.20.0`)
    * `skip` – do not try to check out, skip the repo (the only action available before `0.20.0`)

    Note, if the `path` exists and is _not_ a directory (e.g., a regular file), this is always an error that
    cannot be ignored with the `skip` action.

??? info "Short syntax"

    The short syntax for repos is a colon-separated string in the form of `local_path_or_url:path`.

    * `.:/repo`
    * `..:repo`
    * `~/repos/demo:~/repo`
    * `https://github.com/org/repo:~/data/repo`
    * `git@github.com:org/repo.git:data/repo`

### `files[n]` { #_files data-toc-label="files" }

###### `local_path` - (Required) `str` The path on the user's machine. Relative paths are resolved relative to the parent directory of the the configuration file. { #local_path data-toc-label='local_path' class='reference-item' }
###### `path` - (Required) `str` The path in the container. Relative paths are resolved relative to the working directory. { #path data-toc-label='path' class='reference-item' }


??? info "Short syntax"

    The short syntax for files is a colon-separated string in the form of `local_path[:path]` where
    `path` is optional and can be omitted if it's equal to `local_path`.

    * `~/.bashrc`, same as `~/.bashrc:~/.bashrc`
    * `/opt/myorg`, same as `/opt/myorg/` and `/opt/myorg:/opt/myorg`
    * `libs/patched_libibverbs.so.1:/lib/x86_64-linux-gnu/libibverbs.so.1`

### `backend_options`

Backend-specific options that only take effect for offers of the respective backend.

#### `backend_options[n][type=vastai]` { #backend_options-vastai data-toc-label="vastai" }

###### `offer_order` - (Optional) `"price" | "score"` Controls the order in which offers are considered for provisioning. Use `score` to prioritize the highest overall score first (the default order in the Vast.ai console), or `price` to prioritize the lowest-cost offers first. Lower-cost offers are often less reliable, so consider applying stricter filters when using `price`. Defaults to `score`. { #offer_order data-toc-label='offer_order' class='reference-item' }
###### `min_reliability` - (Optional) `float` The minimum reliability threshold for offers, on a scale from `0` to `1`. Defaults to `0.9`. { #min_reliability data-toc-label='min_reliability' class='reference-item' }
###### `min_score` - (Optional) `int` The minimum overall score required for offers to be considered. The scoring scale varies and may require experimentation. Starting with a value in the low hundreds is generally recommended. { #min_score data-toc-label='min_score' class='reference-item' }

