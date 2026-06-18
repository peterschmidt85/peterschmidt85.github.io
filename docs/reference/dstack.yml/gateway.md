# `gateway`

The `gateway` configuration type allows creating and updating [gateways](../../concepts/gateways.md).

## Root reference

###### `name` - (Optional) `str` The gateway name. { #name data-toc-label='name' class='reference-item' }
###### `default` - (Optional) `bool` Make the gateway default. Defaults to `false`. { #default data-toc-label='default' class='reference-item' }
###### `backend` - (Required) `"amddevcloud" | "aws" | "azure" | "cloudrift" | "crusoe" | "cudo" | "datacrunch" | "digitalocean" | "dstack" | "gcp" | "hotaisle" | "jarvislabs" | "kubernetes" | "lambda" | "remote" | "nebius" | "oci" | "runpod" | "tensordock" | "vastai" | "verda" | "vultr"` The gateway backend. { #backend data-toc-label='backend' class='reference-item' }
###### `region` - (Required) `str` The gateway region. { #region data-toc-label='region' class='reference-item' }
###### `instance_type` - (Optional) `str` Backend-specific instance type to use for the gateway instance. Omit to use the backend's default, which is typically a small non-GPU instance. { #instance_type data-toc-label='instance_type' class='reference-item' }
###### [`router`](#router) - (Optional) `object` The router configuration for this gateway. E.g. `{ type: sglang, policy: round_robin }`.. { #_router data-toc-label='router' class='reference-item' }
###### `domain` - (Optional) `str` The gateway wildcard domain name, e.g. `example.com`. Service domain names are constructed as `<run name>.<gateway domain`. The domain name can use the `${{ run.project_name }}` variable to include the service’s project name. { #domain data-toc-label='domain' class='reference-item' }
###### `public_ip` - (Optional) `bool` Allocate public IP for the gateway. Defaults to `true`. { #public_ip data-toc-label='public_ip' class='reference-item' }
###### [`certificate`](#certificate) - (Optional) `object` The SSL certificate configuration. Set to `null` to disable. Defaults to `type: lets-encrypt`. { #_certificate data-toc-label='certificate' class='reference-item' }
###### `replicas` - (Optional) `int` The number of gateway replicas. Defaults to `1`. { #replicas data-toc-label='replicas' class='reference-item' }
###### `tags` - (Optional) `dict` The custom tags to associate with the gateway. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }


### `router`

=== "SGLang Model Gateway"

    ###### `type` - (Required) `"sglang"` The router type enabled on this gateway.. Must be `sglang`. { #type data-toc-label='type' class='reference-item' }
    ###### `policy` - (Optional) `"random" | "round_robin" | "cache_aware" | "power_of_two"` The routing policy. Deprecated: prefer setting policy in the service's router config. Options: `random`, `round_robin`, `cache_aware`, `power_of_two`. Defaults to `cache_aware`. { #policy data-toc-label='policy' class='reference-item' }


### `certificate`

Set to `null` to disable certificates (e.g. for [private gateways](../../concepts/gateways.md#public-ip)).

=== "Let's encrypt"

    ###### `type` - (Required) `"lets-encrypt"` Automatic certificates by Let's Encrypt. Must be `lets-encrypt`. { #type data-toc-label='type' class='reference-item' }


=== "ACM" 

    ###### `type` - (Required) `"acm"` Certificates by AWS Certificate Manager (ACM). Must be `acm`. { #type data-toc-label='type' class='reference-item' }
    ###### `arn` - (Required) `str` The ARN of the wildcard ACM certificate for the domain. { #arn data-toc-label='arn' class='reference-item' }

