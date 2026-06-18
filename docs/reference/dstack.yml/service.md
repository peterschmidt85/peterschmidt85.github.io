# `service`

The `service` configuration type allows running [services](../../concepts/services.md).

## Root reference

###### `port` - (Required) `int | str | object` The port the application listens on. { #port data-toc-label='port' class='reference-item' }
###### `gateway` - (Optional) `bool | str | object` The name of the gateway. Specify boolean `false` to run without a gateway. Specify boolean `true` to run with the default gateway. Omit to run with the default gateway if there is one, or without a gateway otherwise. { #gateway data-toc-label='gateway' class='reference-item' }
###### `strip_prefix` - (Optional) `bool` Strip the `/proxy/services/<project name>/<run name>/` path prefix when forwarding requests to the service. Only takes effect when running the service without a gateway. Defaults to `true`. { #strip_prefix data-toc-label='strip_prefix' class='reference-item' }
###### `model` - (Optional) `str | object` Mapping of the model for the OpenAI-compatible endpoint provided by `dstack`. Can be a full model format definition or just a model name. If it's a name, the service is expected to expose an OpenAI-compatible API at the `/v1` path. { #model data-toc-label='model' class='reference-item' }
###### `https` - (Optional) `bool | "auto"` Enable HTTPS if running with a gateway. Set to `auto` to determine automatically based on gateway configuration. Defaults to `true`. { #https data-toc-label='https' class='reference-item' }
###### `auth` - (Optional) `bool` Enable the authorization. Defaults to `true`. { #auth data-toc-label='auth' class='reference-item' }
###### [`scaling`](#scaling) - (Optional) `object` The auto-scaling rules. Required if `replicas` is set to a range. { #_scaling data-toc-label='scaling' class='reference-item' }
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


### `model` { data-toc-label="model" }

=== "OpenAI"

    ###### `type` - (Required) `"chat"` The type of the model. Must be `chat`. { #type data-toc-label='type' class='reference-item' }
    ###### `name` - (Required) `str` The name of the model. { #name data-toc-label='name' class='reference-item' }
    ###### `format` - (Required) `"openai"` The serving format. Must be set to `openai`. { #format data-toc-label='format' class='reference-item' }
    ###### `prefix` - (Optional) `str` The `base_url` prefix (after hostname). Defaults to `/v1`. { #prefix data-toc-label='prefix' class='reference-item' }



### `scaling`

###### `metric` - (Required) `"rps"` The target metric to track. Currently, the only supported value is `rps` (meaning requests per second). { #metric data-toc-label='metric' class='reference-item' }
###### `target` - (Required) `float` The target value of the metric. The number of replicas is calculated based on this number and automatically adjusts (scales up or down) as this metric changes. { #target data-toc-label='target' class='reference-item' }
###### `window` - (Optional) `int | str` The time window used to calculate requests per second. Allowed values: `30s`, `60s`, `300s`. Defaults to `60s`. { #window data-toc-label='window' class='reference-item' }
###### `scale_up_delay` - (Optional) `int | str` The minimum time, in seconds, between a scaling event and the next scale-up decision. Used to prevent overly frequent scaling. Defaults to `300`. { #scale_up_delay data-toc-label='scale_up_delay' class='reference-item' }
###### `scale_down_delay` - (Optional) `int | str` The minimum time, in seconds, between a scaling event and the next scale-down decision. Used to prevent overly frequent scaling. Defaults to `600`. { #scale_down_delay data-toc-label='scale_down_delay' class='reference-item' }


### `rate_limits`

#### `rate_limits[n]`

###### `prefix` - (Optional) `str` URL path prefix to which this limit is applied. If an incoming request matches several prefixes, the longest prefix is applied. Defaults to `/`. { #prefix data-toc-label='prefix' class='reference-item' }
###### [`key`](#key) - (Optional) `object` The partitioning key. Each incoming request belongs to a partition and rate limits are applied per partition. Defaults to partitioning by client IP address. { #_key data-toc-label='key' class='reference-item' }
###### `rps` - (Required) `float` Max allowed number of requests per second. Requests are tracked at millisecond granularity. For example, `rps: 10` means at most 1 request per 100ms. { #rps data-toc-label='rps' class='reference-item' }
###### `burst` - (Optional) `int` Max number of requests that can be passed to the service ahead of the rate limit. Defaults to `0`. { #burst data-toc-label='burst' class='reference-item' }


##### `rate_limits[n].key` { data-toc-label="key" }

=== "IP address"

    Partition requests by client IP address.

    ###### `type` - (Required) `"ip_address"` Partitioning type. Must be `ip_address`. { #type data-toc-label='type' class='reference-item' }


=== "Header"

    Partition requests by the value of a header.

    ###### `type` - (Required) `"header"` Partitioning type. Must be `header`. { #type data-toc-label='type' class='reference-item' }
    ###### `header` - (Required) `str` Name of the header to use for partitioning. { #header data-toc-label='header' class='reference-item' }


### `probes`

#### `probes[n]`

###### `type` - (Required) `"http"` The probe type. Must be `http`. { #type data-toc-label='type' class='reference-item' }
###### `url` - (Optional) `str` The URL to request. Defaults to `/`. { #url data-toc-label='url' class='reference-item' }
###### `method` - (Optional) `"delete" | "get" | "head" | "patch" | "post" | "put"` The HTTP method to use for the probe (e.g., `get`, `post`, etc.). Defaults to `get`. { #method data-toc-label='method' class='reference-item' }
###### `headers` - (Optional) `list` A list of HTTP headers to include in the request. { #headers data-toc-label='headers' class='reference-item' }
###### `body` - (Optional) `str` The HTTP request body to send with the probe. { #body data-toc-label='body' class='reference-item' }
###### `timeout` - (Optional) `int | str` Maximum amount of time the HTTP request is allowed to take. Defaults to `10s`. { #timeout data-toc-label='timeout' class='reference-item' }
###### `interval` - (Optional) `int | str` Minimum amount of time between the end of one probe execution and the start of the next. Defaults to `15s`. { #interval data-toc-label='interval' class='reference-item' }
###### `ready_after` - (Optional) `int` The number of consecutive successful probe executions required for the replica to be considered ready. Used during rolling deployments. Defaults to `1`. { #ready_after data-toc-label='ready_after' class='reference-item' }
###### `until_ready` - (Optional) `bool` If `true`, the probe will stop being executed as soon as it reaches the `ready_after` threshold of successful executions. Defaults to `false`. { #until_ready data-toc-label='until_ready' class='reference-item' }


##### `probes[n].headers`

###### `probes[n].headers[m]`

###### `name` - (Required) `str` The name of the HTTP header. { #name data-toc-label='name' class='reference-item' }
###### `value` - (Required) `str` The value of the HTTP header. { #value data-toc-label='value' class='reference-item' }



### `replicas`

#### `replicas[n]`

###### `name` - (Optional) `str` The name of the replica group. If not provided, defaults to '0', '1', etc. based on position.. { #name data-toc-label='name' class='reference-item' }
###### `count` - (Required) `int | str` The number of replicas. Can be a number (e.g. `2`) or a range (`0..4` or `1..8`). If it's a range, the `scaling` property is required. { #count data-toc-label='count' class='reference-item' }
###### [`scaling`](#scaling) - (Optional) `object` The auto-scaling rules. Required if `count` is set to a range. { #_scaling data-toc-label='scaling' class='reference-item' }
###### [`resources`](#resources) - (Optional) `object` The resources requirements for replicas in this group. { #_resources data-toc-label='resources' class='reference-item' }
###### `spot_policy` - (Optional) `"auto" | "on-demand" | "spot"` The policy for provisioning spot or on-demand instances for replicas in this group: `spot`, `on-demand`, `auto`. { #spot_policy data-toc-label='spot_policy' class='reference-item' }
###### `reservation` - (Optional) `str` The existing reservation to use for replicas in this group. Supports AWS Capacity Reservations, AWS Capacity Blocks, and GCP reservations. { #reservation data-toc-label='reservation' class='reference-item' }
###### `commands` - (Optional) `list[str]` The shell commands to run for replicas in this group. { #commands data-toc-label='commands' class='reference-item' }
###### `image` - (Optional) `str` The name of the Docker image to run for replicas in this group. Mutually exclusive with group-level `docker` and `python`.. { #image data-toc-label='image' class='reference-item' }
###### `python` - (Optional) `"3.10" | "3.11" | "3.12" | "3.13" | "3.9"` The major version of Python for replicas in this group. Mutually exclusive with group-level `image` and `docker`.. { #python data-toc-label='python' class='reference-item' }
###### `nvcc` - (Optional) `bool` Use the image with NVIDIA CUDA Compiler (NVCC) included for replicas in this group. Mutually exclusive with group-level `docker`.. { #nvcc data-toc-label='nvcc' class='reference-item' }
###### `docker` - (Optional) `bool` Use the docker-in-docker image for this group (injects `start-dockerd` and runs privileged). Mutually exclusive with group-level `image`, `python`, and `nvcc`.. { #docker data-toc-label='docker' class='reference-item' }
###### `privileged` - (Optional) `bool` Run replicas in this group in privileged mode.. { #privileged data-toc-label='privileged' class='reference-item' }
###### [`router`](#router) - (Optional) `object` When set, replicas in this group run the in-service HTTP router (e.g. SGLang).. { #_router data-toc-label='router' class='reference-item' }


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

