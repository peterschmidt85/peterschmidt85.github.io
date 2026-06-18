# Python API

The Python API enables running tasks, services, and managing runs programmatically.

## Usage example

Below is a quick example of submitting a task for running and displaying its logs.

```python
import sys

from dstack.api import Task, GPU, Client, Resources

client = Client.from_config()

task = Task(
    name="my-awesome-run",  # If not specified, a random name is assigned
    image="ghcr.io/huggingface/text-generation-inference:latest",
    env={"MODEL_ID": "TheBloke/Llama-2-13B-chat-GPTQ"},
    commands=[
        "text-generation-launcher --trust-remote-code --quantize gptq",
    ],
    ports=["80"],
    resources=Resources(gpu=GPU(memory="24GB")),
)

run = client.runs.apply_configuration(
    configuration=task,
    repo=None, # Specify to mount additional files
)

run.attach()

try:
    for log in run.logs():
        sys.stdout.buffer.write(log)
        sys.stdout.buffer.flush()
except KeyboardInterrupt:
    run.stop(abort=True)
finally:
    run.detach()
```

!!! info "NOTE:"
    1. The `configuration` argument in the `apply_configuration` method can be either `dstack.api.Task`, `dstack.api.Service`, or `dstack.api.DevEnvironment`.
    2. When you create `dstack.api.Task`, `dstack.api.Service`, or `dstack.api.DevEnvironment`, you can specify the `image` argument. If `image` isn't specified, the default image will be used. For a private Docker registry, ensure you also pass the `registry_auth` argument.
    3. The `repo` argument in the `apply_configuration` method allows the mounting of a remote repo or a
       programmatically created repo. In this case, the `commands` argument can refer to the files within this repo.
    4. The `attach` method waits for the run to start and, for `dstack.api.Task` sets up an SSH tunnel and forwards
    configured `ports` to `localhost`.

## `dstack.api` { #dstack.api data-toc-label="dstack.api" }

### `dstack.api.Client` { #dstack.api.Client data-toc-label="Client" }

::: dstack.api.Client
    options:
      show_root_heading: false
      show_root_toc_entry: false
      heading_level: 4

### `dstack.api.RunCollection` { #dstack.api.Client.runs data-toc-label="RunCollection" }

::: dstack.api.RunCollection
    options:
      show_bases: false
      show_symbol_type_heading: true
      show_root_toc_entry: false
      heading_level: 4

### `dstack.api.RepoCollection` { #dstack.api.Client.repos data-toc-label="RepoCollection" }

::: dstack.api.RepoCollection
    options:
      show_root_heading: false
      show_root_toc_entry: false
      heading_level: 4

[//]: # (### `dstack.api.BackendCollection` { #dstack.api.Client.backends data-toc-label="BackendCollection" })

[//]: # (::: dstack.api.BackendCollection)
[//]: # (    options:)
[//]: # (      show_bases: false)
[//]: # (      show_root_heading: false)
[//]: # (      show_root_toc_entry: false)
[//]: # (      heading_level: 4)

### `dstack.api.Task` { #dstack.api.Task data-toc-label="Task" }

###### `nodes` - (Optional) `int` Number of nodes. Defaults to `1`. { #nodes data-toc-label='nodes' class='reference-item' }
###### [`ports`](#ports) - (Optional) `list[int | str | object]` Port numbers/mapping to expose. { #_ports data-toc-label='ports' class='reference-item' }
###### `commands` - (Optional) `list[str]` The shell commands to run. { #commands data-toc-label='commands' class='reference-item' }
###### `name` - (Optional) `str` The run name. If not specified, a random name is generated. { #name data-toc-label='name' class='reference-item' }
###### `image` - (Optional) `str` The name of the Docker image to run. { #image data-toc-label='image' class='reference-item' }
###### `user` - (Optional) `str` The user inside the container, `user_name_or_id[:group_name_or_id]` (e.g., `ubuntu`, `1000:1000`). Defaults to the default user from the `image`. { #user data-toc-label='user' class='reference-item' }
###### `privileged` - (Optional) `bool` Run the container in privileged mode. Defaults to `false`. { #privileged data-toc-label='privileged' class='reference-item' }
###### `entrypoint` - (Optional) `str` The Docker entrypoint. { #entrypoint data-toc-label='entrypoint' class='reference-item' }
###### `working_dir` - (Optional) `str` The absolute path to the working directory inside the container. Defaults to the `image`'s default working directory. { #working_dir data-toc-label='working_dir' class='reference-item' }
###### [`registry_auth`](#dstack.api.RegistryAuth) - (Optional) `object` Credentials for pulling a private Docker image. { #_registry_auth data-toc-label='registry_auth' class='reference-item' }
###### `python` - (Optional) `"3.10" | "3.11" | "3.12" | "3.13" | "3.9"` The major version of Python. Mutually exclusive with `image` and `docker`. { #python data-toc-label='python' class='reference-item' }
###### `nvcc` - (Optional) `bool` Use image with NVIDIA CUDA Compiler (NVCC) included. Mutually exclusive with `image` and `docker`. { #nvcc data-toc-label='nvcc' class='reference-item' }
###### `single_branch` - (Optional) `bool` Whether to clone and track only the current branch or all remote branches. Relevant only when using remote Git repos. Defaults to `false` for dev environments and to `true` for tasks and services. { #single_branch data-toc-label='single_branch' class='reference-item' }
###### [`env`](#env) - (Optional) `list[str] | dict` The mapping or the list of environment variables. { #_env data-toc-label='env' class='reference-item' }
###### `shell` - (Optional) `str` The shell used to run commands. Allowed values are `sh`, `bash`, or an absolute path, e.g., `/usr/bin/zsh`. Defaults to `/bin/sh` if the `image` is specified, `/bin/bash` otherwise. { #shell data-toc-label='shell' class='reference-item' }
###### [`resources`](#dstack.api.Resources) - (Optional) `object` The resources requirements to run the configuration. { #_resources data-toc-label='resources' class='reference-item' }
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


### `dstack.api.Service`  { #dstack.api.Service data-toc-label="Service" }

###### `port` - (Required) `int | str | object` The port the application listens on. { #port data-toc-label='port' class='reference-item' }
###### `gateway` - (Optional) `bool | str | object` The name of the gateway. Specify boolean `false` to run without a gateway. Specify boolean `true` to run with the default gateway. Omit to run with the default gateway if there is one, or without a gateway otherwise. { #gateway data-toc-label='gateway' class='reference-item' }
###### `strip_prefix` - (Optional) `bool` Strip the `/proxy/services/<project name>/<run name>/` path prefix when forwarding requests to the service. Only takes effect when running the service without a gateway. Defaults to `true`. { #strip_prefix data-toc-label='strip_prefix' class='reference-item' }
###### `model` - (Optional) `str | object` Mapping of the model for the OpenAI-compatible endpoint provided by `dstack`. Can be a full model format definition or just a model name. If it's a name, the service is expected to expose an OpenAI-compatible API at the `/v1` path. { #model data-toc-label='model' class='reference-item' }
###### `https` - (Optional) `bool | "auto"` Enable HTTPS if running with a gateway. Set to `auto` to determine automatically based on gateway configuration. Defaults to `true`. { #https data-toc-label='https' class='reference-item' }
###### `auth` - (Optional) `bool` Enable the authorization. Defaults to `true`. { #auth data-toc-label='auth' class='reference-item' }
###### [`scaling`](#dstack.api.Scaling) - (Optional) `object` The auto-scaling rules. Required if `replicas` is set to a range. { #_scaling data-toc-label='scaling' class='reference-item' }
###### [`rate_limits`](#rate_limits) - (Optional) `list[object]` Rate limiting rules. { #_rate_limits data-toc-label='rate_limits' class='reference-item' }
###### `probes` - (Optional) `list[object]` The list of probes to determine service health. If `model` is set, defaults to a `/v1/chat/completions` probe. Set explicitly to override. { #probes data-toc-label='probes' class='reference-item' }
###### `replicas` - (Optional) `int | str | list[object]` The number of replicas or a list of replica groups. Can be an integer (e.g., `2`), a range (e.g., `0..4`), or a list of replica groups. Each replica group defines replicas with shared configuration (commands, resources, scaling). When `replicas` is a list of replica groups, top-level `scaling`, `commands`, and `resources` are not allowed and must be specified in each replica group instead. . { #replicas data-toc-label='replicas' class='reference-item' }
###### [`router`](#router) - (Optional) `object` Router configuration for the service. Requires a gateway with matching router enabled. . { #_router data-toc-label='router' class='reference-item' }
###### `commands` - (Optional) `list[str]` The shell commands to run. { #commands data-toc-label='commands' class='reference-item' }
###### `name` - (Optional) `str` The run name. If not specified, a random name is generated. { #name data-toc-label='name' class='reference-item' }
###### `image` - (Optional) `str` The name of the Docker image to run. { #image data-toc-label='image' class='reference-item' }
###### `user` - (Optional) `str` The user inside the container, `user_name_or_id[:group_name_or_id]` (e.g., `ubuntu`, `1000:1000`). Defaults to the default user from the `image`. { #user data-toc-label='user' class='reference-item' }
###### `privileged` - (Optional) `bool` Run the container in privileged mode. Defaults to `false`. { #privileged data-toc-label='privileged' class='reference-item' }
###### `entrypoint` - (Optional) `str` The Docker entrypoint. { #entrypoint data-toc-label='entrypoint' class='reference-item' }
###### `working_dir` - (Optional) `str` The absolute path to the working directory inside the container. Defaults to the `image`'s default working directory. { #working_dir data-toc-label='working_dir' class='reference-item' }
###### [`registry_auth`](#dstack.api.RegistryAuth) - (Optional) `object` Credentials for pulling a private Docker image. { #_registry_auth data-toc-label='registry_auth' class='reference-item' }
###### `python` - (Optional) `"3.10" | "3.11" | "3.12" | "3.13" | "3.9"` The major version of Python. Mutually exclusive with `image` and `docker`. { #python data-toc-label='python' class='reference-item' }
###### `nvcc` - (Optional) `bool` Use image with NVIDIA CUDA Compiler (NVCC) included. Mutually exclusive with `image` and `docker`. { #nvcc data-toc-label='nvcc' class='reference-item' }
###### `single_branch` - (Optional) `bool` Whether to clone and track only the current branch or all remote branches. Relevant only when using remote Git repos. Defaults to `false` for dev environments and to `true` for tasks and services. { #single_branch data-toc-label='single_branch' class='reference-item' }
###### [`env`](#env) - (Optional) `list[str] | dict` The mapping or the list of environment variables. { #_env data-toc-label='env' class='reference-item' }
###### `shell` - (Optional) `str` The shell used to run commands. Allowed values are `sh`, `bash`, or an absolute path, e.g., `/usr/bin/zsh`. Defaults to `/bin/sh` if the `image` is specified, `/bin/bash` otherwise. { #shell data-toc-label='shell' class='reference-item' }
###### [`resources`](#dstack.api.Resources) - (Optional) `object` The resources requirements to run the configuration. { #_resources data-toc-label='resources' class='reference-item' }
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


### `dstack.api.DevEnvironment` { #dstack.api.DevEnvironment data-toc-label="DevEnvironment" }

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
###### [`registry_auth`](#dstack.api.RegistryAuth) - (Optional) `object` Credentials for pulling a private Docker image. { #_registry_auth data-toc-label='registry_auth' class='reference-item' }
###### `python` - (Optional) `"3.10" | "3.11" | "3.12" | "3.13" | "3.9"` The major version of Python. Mutually exclusive with `image` and `docker`. { #python data-toc-label='python' class='reference-item' }
###### `nvcc` - (Optional) `bool` Use image with NVIDIA CUDA Compiler (NVCC) included. Mutually exclusive with `image` and `docker`. { #nvcc data-toc-label='nvcc' class='reference-item' }
###### `single_branch` - (Optional) `bool` Whether to clone and track only the current branch or all remote branches. Relevant only when using remote Git repos. Defaults to `false` for dev environments and to `true` for tasks and services. { #single_branch data-toc-label='single_branch' class='reference-item' }
###### [`env`](#env) - (Optional) `list[str] | dict` The mapping or the list of environment variables. { #_env data-toc-label='env' class='reference-item' }
###### `shell` - (Optional) `str` The shell used to run commands. Allowed values are `sh`, `bash`, or an absolute path, e.g., `/usr/bin/zsh`. Defaults to `/bin/sh` if the `image` is specified, `/bin/bash` otherwise. { #shell data-toc-label='shell' class='reference-item' }
###### [`resources`](#dstack.api.Resources) - (Optional) `object` The resources requirements to run the configuration. { #_resources data-toc-label='resources' class='reference-item' }
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


### `dstack.api.Run` { #dstack.api.Run data-toc-label="Run" }

::: dstack.api.Run
    options:
      show_bases: false
      show_root_heading: false
      show_root_toc_entry: false
      heading_level: 4

### `dstack.api.Resources` { #dstack.api.Resources data-toc-label="Resources" }

###### [`cpu`](#dstack.api.CPU) - (Optional) `int | str | object` The CPU requirements. { #_cpu data-toc-label='cpu' class='reference-item' }
###### `memory` - (Optional) `int | str` The RAM size (e.g., `8GB`). Defaults to `8GB..`. { #memory data-toc-label='memory' class='reference-item' }
###### `shm_size` - (Optional) `int | str` The size of shared memory (e.g., `8GB`). If you are using parallel communicating processes (e.g., dataloaders in PyTorch), you may need to configure this. { #shm_size data-toc-label='shm_size' class='reference-item' }
###### [`gpu`](#dstack.api.GPU) - (Optional) `int | str | object` The GPU requirements. { #_gpu data-toc-label='gpu' class='reference-item' }
###### [`disk`](#disk) - (Optional) `int | str | object` The disk resources. { #_disk data-toc-label='disk' class='reference-item' }


### `dstack.api.CPU` { #dstack.api.CPU data-toc-label="CPU" }

###### `arch` - (Optional) `"arm" | "x86"` The CPU architecture, one of: `x86`, `arm`. { #arch data-toc-label='arch' class='reference-item' }
###### `count` - (Optional) `int | str` The number of CPU cores. Defaults to `2..`. { #count data-toc-label='count' class='reference-item' }


### `dstack.api.GPU` { #dstack.api.GPU data-toc-label="GPU" }

###### `vendor` - (Optional) `"amd" | "google" | "intel" | "nvidia" | "tenstorrent"` The vendor of the GPU/accelerator, one of: `nvidia`, `amd`, `google` (alias: `tpu`), `intel`. { #vendor data-toc-label='vendor' class='reference-item' }
###### `name` - (Optional) `str | list[str]` The name of the GPU (e.g., `A100` or `H100`). { #name data-toc-label='name' class='reference-item' }
###### `count` - (Optional) `int | str` The number of GPUs. Defaults to `1..`. { #count data-toc-label='count' class='reference-item' }
###### `memory` - (Optional) `int | str` The RAM size (e.g., `16GB`). Can be set to a range (e.g. `16GB..`, or `16GB..80GB`). { #memory data-toc-label='memory' class='reference-item' }
###### `total_memory` - (Optional) `int | str` The total RAM size (e.g., `32GB`). Can be set to a range (e.g. `16GB..`, or `16GB..80GB`). { #total_memory data-toc-label='total_memory' class='reference-item' }
###### `compute_capability` - (Optional) `float | str` The minimum compute capability of the GPU (e.g., `7.5`). { #compute_capability data-toc-label='compute_capability' class='reference-item' }


### `dstack.api.Disk` { #dstack.api.Disk data-toc-label="Disk" }

###### `size` - (Required) `int | str` Disk size. { #size data-toc-label='size' class='reference-item' }


### `dstack.api.RemoteRepo` { #dstack.api.RemoteRepo data-toc-label="RemoteRepo" }

::: dstack.api.RemoteRepo
    options:
      show_bases: false
      show_root_heading: false
      show_root_toc_entry: false
      heading_level: 4

### `dstack.api.VirtualRepo` { #dstack.api.VirtualRepo data-toc-label="VirtualRepo" }

::: dstack.api.VirtualRepo
    options:
      show_bases: false
      show_root_heading: false
      show_root_toc_entry: false
      heading_level: 4

### `dstack.api.RegistryAuth` { #dstack.api.RegistryAuth data-toc-label="RegistryAuth" }

###### `username` - (Required) `str` The username. { #username data-toc-label='username' class='reference-item' }
###### `password` - (Required) `str` The password or access token. { #password data-toc-label='password' class='reference-item' }


### `dstack.api.Scaling` { #dstack.api.Scaling data-toc-label="Scaling" }

###### `metric` - (Required) `"rps"` The target metric to track. Currently, the only supported value is `rps` (meaning requests per second). { #metric data-toc-label='metric' class='reference-item' }
###### `target` - (Required) `float` The target value of the metric. The number of replicas is calculated based on this number and automatically adjusts (scales up or down) as this metric changes. { #target data-toc-label='target' class='reference-item' }
###### `window` - (Optional) `int | str` The time window used to calculate requests per second. Allowed values: `30s`, `60s`, `300s`. Defaults to `60s`. { #window data-toc-label='window' class='reference-item' }
###### `scale_up_delay` - (Optional) `int | str` The minimum time, in seconds, between a scaling event and the next scale-up decision. Used to prevent overly frequent scaling. Defaults to `300`. { #scale_up_delay data-toc-label='scale_up_delay' class='reference-item' }
###### `scale_down_delay` - (Optional) `int | str` The minimum time, in seconds, between a scaling event and the next scale-down decision. Used to prevent overly frequent scaling. Defaults to `600`. { #scale_down_delay data-toc-label='scale_down_delay' class='reference-item' }


### `dstack.api.BackendType` { #dstack.api.BackendType data-toc-label="BackendType" }

::: dstack.api.BackendType
    options:
      show_bases: false
      show_root_heading: false
      show_root_toc_entry: false
      heading_level: 4

<style>
.doc-heading .highlight {
    /* TODO pick color */
    --md-code-hl-name-color: var(--md-typeset-color);
    --md-code-hl-constant-color: var(--md-typeset-color);
}

.doc-symbol:after {
    display: none
}

</style>
