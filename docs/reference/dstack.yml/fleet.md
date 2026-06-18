# `fleet`

The `fleet` configuration type allows creating and updating fleets.


=== "Backend fleet"

    ## Root reference

    ###### `name` - (Optional) `str` The fleet name. { #name data-toc-label='name' class='reference-item' }
    ###### `placement` - (Optional) `"any" | "cluster"` The placement of instances: `any` or `cluster`. { #placement data-toc-label='placement' class='reference-item' }
    ###### `blocks` - (Optional) `int | "auto"` The amount of blocks to split the instance into, a number or `auto`. `auto` means as many as possible. The number of GPUs and CPUs must be divisible by the number of blocks. Defaults to `1`, i.e. do not split. Defaults to `1`. { #blocks data-toc-label='blocks' class='reference-item' }
    ###### [`nodes`](#nodes) - (Required) `int | str | object` The number of instances. { #_nodes data-toc-label='nodes' class='reference-item' }
    ###### `reservation` - (Optional) `str` The existing reservation to use for instance provisioning. Supports AWS Capacity Reservations, AWS Capacity Blocks, and GCP reservations. { #reservation data-toc-label='reservation' class='reference-item' }
    ###### [`resources`](#resources) - (Optional) `object` The resources requirements. { #_resources data-toc-label='resources' class='reference-item' }
    ###### `backends` - (Optional) `list["amddevcloud" | "aws" | "azure" | "cloudrift" | "crusoe" | "cudo" | "datacrunch" | "digitalocean" | "dstack" | "gcp" | "hotaisle" | "jarvislabs" | "kubernetes" | "lambda" | "remote" | "nebius" | "oci" | "runpod" | "tensordock" | "vastai" | "verda" | "vultr"]` The backends to consider for provisioning (e.g., `[aws, gcp]`). { #backends data-toc-label='backends' class='reference-item' }
    ###### `regions` - (Optional) `list[str]` The regions to consider for provisioning (e.g., `[eu-west-1, us-west4, westeurope]`). { #regions data-toc-label='regions' class='reference-item' }
    ###### `availability_zones` - (Optional) `list[str]` The availability zones to consider for provisioning (e.g., `[eu-west-1a, us-west4-a]`). { #availability_zones data-toc-label='availability_zones' class='reference-item' }
    ###### `instance_types` - (Optional) `list[str]` The cloud-specific instance types to consider for provisioning (e.g., `[g6e.24xlarge, n1-standard-4]`). { #instance_types data-toc-label='instance_types' class='reference-item' }
    ###### `spot_policy` - (Optional) `"auto" | "on-demand" | "spot"` The policy for provisioning spot or on-demand instances: `spot`, `on-demand`, `auto`. Defaults to `on-demand`. { #spot_policy data-toc-label='spot_policy' class='reference-item' }
    ###### [`retry`](#retry) - (Optional) `bool | object` The policy for provisioning retry. Defaults to `false`. { #_retry data-toc-label='retry' class='reference-item' }
    ###### `max_price` - (Optional) `float` The maximum instance price per hour, in dollars. { #max_price data-toc-label='max_price' class='reference-item' }
    ###### `idle_duration` - (Optional) `int | str` Time to wait before terminating idle instances. Instances are not terminated if the fleet is already at `nodes.min`. Defaults to `5m` for runs and `3d` for fleets. Use `off` for unlimited duration. { #idle_duration data-toc-label='idle_duration' class='reference-item' }
    ###### `tags` - (Optional) `dict` The custom tags to associate with the resource. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }
    ###### `backend_options` - (Optional) `list[object]` Backend-specific options, applied only to offers from that backend. { #backend_options data-toc-label='backend_options' class='reference-item' }


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


    ### `retry`

    ###### `on_events` - (Optional) `list["no-capacity" | "interruption" | "error"]` The list of events that should be handled with retry. Supported events are `no-capacity`, `interruption`, `error`. Omit to retry on all events. { #on_events data-toc-label='on_events' class='reference-item' }
    ###### `duration` - (Optional) `int | str` The maximum period of retrying the run, e.g., `4h` or `1d`. The period is calculated as a run age for `no-capacity` event and as a time passed since the last `interruption` and `error` for `interruption` and `error` events.. { #duration data-toc-label='duration' class='reference-item' }


    ### `backend_options`

    Backend-specific options that only take effect for offers of the respective backend.

    #### `backend_options[n][type=vastai]` { #backend_options-vastai data-toc-label="vastai" }

    ###### `offer_order` - (Optional) `"price" | "score"` Controls the order in which offers are considered for provisioning. Use `score` to prioritize the highest overall score first (the default order in the Vast.ai console), or `price` to prioritize the lowest-cost offers first. Lower-cost offers are often less reliable, so consider applying stricter filters when using `price`. Defaults to `score`. { #offer_order data-toc-label='offer_order' class='reference-item' }
    ###### `min_reliability` - (Optional) `float` The minimum reliability threshold for offers, on a scale from `0` to `1`. Defaults to `0.9`. { #min_reliability data-toc-label='min_reliability' class='reference-item' }
    ###### `min_score` - (Optional) `int` The minimum overall score required for offers to be considered. The scoring scale varies and may require experimentation. Starting with a value in the low hundreds is generally recommended. { #min_score data-toc-label='min_score' class='reference-item' }


=== "SSH fleet"

    ## Root reference

    ###### `name` - (Optional) `str` The fleet name. { #name data-toc-label='name' class='reference-item' }
    ###### `placement` - (Optional) `"any" | "cluster"` The placement of instances: `any` or `cluster`. { #placement data-toc-label='placement' class='reference-item' }
    ###### `blocks` - (Optional) `int | "auto"` The amount of blocks to split the instance into, a number or `auto`. `auto` means as many as possible. The number of GPUs and CPUs must be divisible by the number of blocks. Defaults to `1`, i.e. do not split. Defaults to `1`. { #blocks data-toc-label='blocks' class='reference-item' }
    ###### [`ssh_config`](#ssh_config) - (Required) `object` The parameters for adding instances via SSH. { #_ssh_config data-toc-label='ssh_config' class='reference-item' }
    ###### [`env`](#env) - (Optional) `list[str] | dict` The mapping or the list of environment variables. { #_env data-toc-label='env' class='reference-item' }


    ### `ssh_config` { data-toc-label="ssh_config" }

    ###### `user` - (Optional) `str` The user to log in with on all hosts. { #user data-toc-label='user' class='reference-item' }
    ###### `port` - (Optional) `int` The SSH port to connect to. { #port data-toc-label='port' class='reference-item' }
    ###### `identity_file` - (Optional) `str` The private key to use for all hosts. { #identity_file data-toc-label='identity_file' class='reference-item' }
    ###### [`proxy_jump`](#ssh_config-proxy_jump) - (Optional) `object` The SSH proxy configuration for all hosts. { #_proxy_jump data-toc-label='proxy_jump' class='reference-item' }
    ###### [`hosts`](#ssh_config-hosts) - (Required) `list[str | object]` The per host connection parameters: a hostname or an object that overrides default ssh parameters. { #_hosts data-toc-label='hosts' class='reference-item' }
    ###### `network` - (Optional) `str` The network address for cluster setup in the format `<ip>/<netmask>`. `dstack` will use IP addresses from this network for communication between hosts. If not specified, `dstack` will use IPs from the first found internal network.. { #network data-toc-label='network' class='reference-item' }


    #### `ssh_config.proxy_jump` { #ssh_config-proxy_jump data-toc-label="proxy_jump" }

    ###### `hostname` - (Required) `str` The IP address or domain of proxy host. { #hostname data-toc-label='hostname' class='reference-item' }
    ###### `port` - (Optional) `int` The SSH port of proxy host. { #port data-toc-label='port' class='reference-item' }
    ###### `user` - (Required) `str` The user to log in with for proxy host. { #user data-toc-label='user' class='reference-item' }
    ###### `identity_file` - (Required) `str` The private key to use for proxy host. { #identity_file data-toc-label='identity_file' class='reference-item' }


    #### `ssh_config.hosts[n]` { #ssh_config-hosts data-toc-label="hosts" }

    ###### `hostname` - (Required) `str` The IP address or domain to connect to. { #hostname data-toc-label='hostname' class='reference-item' }
    ###### `port` - (Optional) `int` The SSH port to connect to for this host. { #port data-toc-label='port' class='reference-item' }
    ###### `user` - (Optional) `str` The user to log in with for this host. { #user data-toc-label='user' class='reference-item' }
    ###### `identity_file` - (Optional) `str` The private key to use for this host. { #identity_file data-toc-label='identity_file' class='reference-item' }
    ###### [`proxy_jump`](#proxy_jump) - (Optional) `object` The SSH proxy configuration for this host. { #_proxy_jump data-toc-label='proxy_jump' class='reference-item' }
    ###### `internal_ip` - (Optional) `str` The internal IP of the host used for communication inside the cluster. If not specified, `dstack` will use the IP address from `network` or from the first found internal network.. { #internal_ip data-toc-label='internal_ip' class='reference-item' }
    ###### `blocks` - (Optional) `int | "auto"` The amount of blocks to split the instance into, a number or `auto`. `auto` means as many as possible. The number of GPUs and CPUs must be divisible by the number of blocks. Defaults to the top-level `blocks` value. { #blocks data-toc-label='blocks' class='reference-item' }


    ##### `ssh_config.hosts[n].proxy_jump` { #proxy_jump data-toc-label="hosts[n].proxy_jump" }

    ###### `hostname` - (Required) `str` The IP address or domain of proxy host. { #hostname data-toc-label='hostname' class='reference-item' }
    ###### `port` - (Optional) `int` The SSH port of proxy host. { #port data-toc-label='port' class='reference-item' }
    ###### `user` - (Required) `str` The user to log in with for proxy host. { #user data-toc-label='user' class='reference-item' }
    ###### `identity_file` - (Required) `str` The private key to use for proxy host. { #identity_file data-toc-label='identity_file' class='reference-item' }

