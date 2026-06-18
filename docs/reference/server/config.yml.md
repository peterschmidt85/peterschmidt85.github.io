# ~/.dstack/server/config.yml

The `~/.dstack/server/config.yml` file is used
to configure [backends](../../concepts/backends.md) and other [server-level settings](../../guides/server-deployment.md).

## Root reference

###### [`projects`](#projects) - (Required) `list[object]` The list of projects. { #_projects data-toc-label='projects' class='reference-item' }
###### [`encryption`](#encryption) - (Optional) `object` The encryption config. { #_encryption data-toc-label='encryption' class='reference-item' }
###### [`default_permissions`](#default_permissions) - (Optional) `object` The default user permissions. { #_default_permissions data-toc-label='default_permissions' class='reference-item' }
###### `plugins` - (Optional) `list[str]` The server-side plugins to enable. { #plugins data-toc-label='plugins' class='reference-item' }


### `projects[n]` { #projects data-toc-label="projects" }

###### `name` - (Required) `str` The name of the project. { #name data-toc-label='name' class='reference-item' }
###### `backends` - (Optional) `list[object]` The list of backends. { #backends data-toc-label='backends' class='reference-item' }


#### `projects[n].backends` { #backends data-toc-label="backends" }

##### `projects[n].backends[type=aws]` { #aws data-toc-label="aws" }

###### `type` - (Required) `"aws"` The type of the backend. Must be `aws`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of AWS regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `vpc_name` - (Optional) `str` The name of custom VPCs. All configured regions must have a VPC with this name. If your custom VPCs don't have names or have different names in different regions, use `vpc_ids` instead.. { #vpc_name data-toc-label='vpc_name' class='reference-item' }
###### `vpc_ids` - (Optional) `dict` The mapping from AWS regions to VPC IDs. If `default_vpcs: true`, omitted regions will use default VPCs. { #vpc_ids data-toc-label='vpc_ids' class='reference-item' }
###### `default_vpcs` - (Optional) `bool` A flag to enable/disable using default VPCs in regions not configured by `vpc_ids`. Set to `false` if default VPCs should never be used. Defaults to `true`. { #default_vpcs data-toc-label='default_vpcs' class='reference-item' }
###### `public_ips` - (Optional) `bool` A flag to enable/disable public IP assigning on instances. `public_ips: false` requires at least one private subnet with outbound internet connectivity provided by a NAT Gateway or a Transit Gateway. Defaults to `true`. { #public_ips data-toc-label='public_ips' class='reference-item' }
###### `iam_instance_profile` - (Optional) `str` The name of the IAM instance profile to associate with EC2 instances. You can also specify the IAM role name for roles created via the AWS console. AWS automatically creates an instance profile and gives it the same name as the role. { #iam_instance_profile data-toc-label='iam_instance_profile' class='reference-item' }
###### `tags` - (Optional) `dict` The tags that will be assigned to resources created by `dstack`. { #tags data-toc-label='tags' class='reference-item' }
###### [`os_images`](#aws-os_images) - (Optional) `object` The mapping of instance categories (CPU, NVIDIA GPU) to AMI configurations. { #_os_images data-toc-label='os_images' class='reference-item' }
###### [`creds`](#aws-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=aws].creds` { #aws-creds data-toc-label="creds" }

=== "Access key"
    ###### `type` - (Required) `"access_key"` The type of credentials. Must be `access_key`. { #type data-toc-label='type' class='reference-item' }
    ###### `access_key` - (Required) `str` The access key. { #access_key data-toc-label='access_key' class='reference-item' }
    ###### `secret_key` - (Required) `str` The secret key. { #secret_key data-toc-label='secret_key' class='reference-item' }


=== "Default"
    ###### `type` - (Required) `"default"` The type of credentials. Must be `default`. { #type data-toc-label='type' class='reference-item' }


###### `projects[n].backends[type=aws].os_images` { #aws-os_images data-toc-label="os_images" }

###### [`cpu`](#aws-os_images-cpu) - (Optional) `object` The AMI used for CPU instances. { #_cpu data-toc-label='cpu' class='reference-item' }
###### [`nvidia`](#aws-os_images-nvidia) - (Optional) `object` The AMI used for NVIDIA GPU instances. { #_nvidia data-toc-label='nvidia' class='reference-item' }


###### `projects[n].backends[type=aws].os_images.cpu` { #aws-os_images-cpu data-toc-label="cpu" }

###### `name` - (Required) `str` The AMI name. { #name data-toc-label='name' class='reference-item' }
###### `owner` - (Optional) `str` The AMI owner, account ID or `self`. Defaults to `self`. { #owner data-toc-label='owner' class='reference-item' }
###### `user` - (Required) `str` The OS user for provisioning. { #user data-toc-label='user' class='reference-item' }


###### `projects[n].backends[type=aws].os_images.nvidia` { #aws-os_images-nvidia data-toc-label="nvidia" }

###### `name` - (Required) `str` The AMI name. { #name data-toc-label='name' class='reference-item' }
###### `owner` - (Optional) `str` The AMI owner, account ID or `self`. Defaults to `self`. { #owner data-toc-label='owner' class='reference-item' }
###### `user` - (Required) `str` The OS user for provisioning. { #user data-toc-label='user' class='reference-item' }


##### `projects[n].backends[type=azure]` { #azure data-toc-label="azure" }

###### `type` - (Required) `"azure"` The type of the backend. Must be `azure`. { #type data-toc-label='type' class='reference-item' }
###### `tenant_id` - (Required) `str` The tenant ID. { #tenant_id data-toc-label='tenant_id' class='reference-item' }
###### `subscription_id` - (Required) `str` The subscription ID. { #subscription_id data-toc-label='subscription_id' class='reference-item' }
###### `resource_group` - (Optional) `str` The resource group for resources created by `dstack`. If not specified, `dstack` will create a new resource group. { #resource_group data-toc-label='resource_group' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of Azure regions (locations). Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `vpc_ids` - (Optional) `dict` The mapping from configured Azure locations to network IDs. A network ID must have a format `networkResourceGroup/networkName` If not specified, `dstack` will create a new network for every configured region. { #vpc_ids data-toc-label='vpc_ids' class='reference-item' }
###### `subnet_ids` - (Optional) `dict` The mapping from configured Azure locations to subnet IDs. A subnet ID must have a format `networkResourceGroup/networkName/subnetName`. Cannot be configured for the same region as `vpc_ids`. { #subnet_ids data-toc-label='subnet_ids' class='reference-item' }
###### `public_ips` - (Optional) `bool` A flag to enable/disable public IP assigning on instances. `public_ips: false` requires `vpc_ids` or `subnet_ids` that specifies custom networks with outbound internet connectivity provided by NAT Gateway or other mechanism. Defaults to `true`. { #public_ips data-toc-label='public_ips' class='reference-item' }
###### `vm_managed_identity` - (Optional) `str` The managed identity to associate with provisioned VMs. Must have a format `managedIdentityResourceGroup/managedIdentityName`. { #vm_managed_identity data-toc-label='vm_managed_identity' class='reference-item' }
###### `tags` - (Optional) `dict` The tags that will be assigned to resources created by `dstack`. { #tags data-toc-label='tags' class='reference-item' }
###### [`creds`](#azure-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=azure].creds` { #azure-creds data-toc-label="creds" }

=== "Client"
    ###### `type` - (Required) `"client"` The type of credentials. Must be `client`. { #type data-toc-label='type' class='reference-item' }
    ###### `client_id` - (Required) `str` The client ID. { #client_id data-toc-label='client_id' class='reference-item' }
    ###### `client_secret` - (Required) `str` The client secret. { #client_secret data-toc-label='client_secret' class='reference-item' }


=== "Default"
    ###### `type` - (Required) `"default"` The type of credentials. Must be `default`. { #type data-toc-label='type' class='reference-item' }


##### `projects[n].backends[type=gcp]` { #gcp data-toc-label="gcp" }

###### `type` - (Required) `"gcp"` The type of backend. Must be `gcp`. { #type data-toc-label='type' class='reference-item' }
###### `project_id` - (Required) `str` The project ID. { #project_id data-toc-label='project_id' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of GCP regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `vpc_name` - (Optional) `str` The name of a custom VPC. If not specified, the default VPC is used. { #vpc_name data-toc-label='vpc_name' class='reference-item' }
###### `extra_vpcs` - (Optional) `list[str]` The names of additional VPCs used for multi-NIC instances, such as those that support GPUDirect. Specify eight VPCs to maximize bandwidth in clusters with eight-GPU instances. Each VPC must have a subnet and a firewall rule allowing internal traffic across all subnets. { #extra_vpcs data-toc-label='extra_vpcs' class='reference-item' }
###### `roce_vpcs` - (Optional) `list` The names of additional VPCs with the RoCE network profile. Used for RDMA on GPU instances that support the MRDMA interface type. A VPC should have eight subnets to maximize the bandwidth in clusters with eight-GPU instances.. { #roce_vpcs data-toc-label='roce_vpcs' class='reference-item' }
###### `vpc_project_id` - (Optional) `str` The shared VPC hosted project ID. Required for shared VPC only. { #vpc_project_id data-toc-label='vpc_project_id' class='reference-item' }
###### `public_ips` - (Optional) `bool` A flag to enable/disable public IP assigning on instances. Defaults to `true`. { #public_ips data-toc-label='public_ips' class='reference-item' }
###### `nat_check` - (Optional) `bool` A flag to enable/disable a check that Cloud NAT is configured for the VPC. This should be set to `false` when `public_ips: false` and outbound internet connectivity is provided by a mechanism other than Cloud NAT such as a third-party NAT appliance. Defaults to `true`. { #nat_check data-toc-label='nat_check' class='reference-item' }
###### `vm_service_account` - (Optional) `str` The service account to associate with provisioned VMs. { #vm_service_account data-toc-label='vm_service_account' class='reference-item' }
###### `tags` - (Optional) `dict` The tags (labels) that will be assigned to resources created by `dstack`. { #tags data-toc-label='tags' class='reference-item' }
###### `preview_features` - (Optional) `list` The list of preview GCP features to enable. There are currently no preview features. { #preview_features data-toc-label='preview_features' class='reference-item' }
###### [`creds`](#gcp-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=gcp].creds` { #gcp-creds data-toc-label="creds" }

=== "Service account"
    ###### `type` - (Required) `"service_account"` The type of credentials. Must be `service_account`. { #type data-toc-label='type' class='reference-item' }
    ###### `filename` - (Required) `str` The path to the service account file. { #filename data-toc-label='filename' class='reference-item' }
    ###### `data` - (Optional) `str` The contents of the service account file. When configuring via `server/config.yml`, it's automatically filled from `filename`. When configuring via UI, it has to be specified explicitly. { #data data-toc-label='data' class='reference-item' }


    ??? info "Specifying `data`"
        To specify service account file contents as a string, use `jq`:

        ```shell
        cat my-service-account-file.json | jq -c | jq -R
        ```

=== "Default"




##### `projects[n].backends[type=lambda]` { #lambda data-toc-label="lambda" }

###### `type` - (Required) `"lambda"` The type of backend. Must be `lambda`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of Lambda regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#lambda-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=lambda].creds` { #lambda-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=nebius]` { #nebius data-toc-label="nebius" }

###### `type` - (Required) `"nebius"` The type of backend. Must be `nebius`. { #type data-toc-label='type' class='reference-item' }
###### `projects` - (Optional) `list[str]` The list of allowed Nebius project IDs. Omit to use the default project in each region. The project is considered default if it is the only project in the region or if its name starts with `default`. { #projects data-toc-label='projects' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of allowed Nebius regions. Omit to allow all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `fabrics` - (Optional) `list[str]` The list of allowed fabrics for InfiniBand clusters. Omit to allow all fabrics. { #fabrics data-toc-label='fabrics' class='reference-item' }
###### `tags` - (Optional) `dict` The tags (labels) that will be assigned to resources created by `dstack`. { #tags data-toc-label='tags' class='reference-item' }
###### `creds` - (Required) `object` The credentials. { #creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=nebius].creds` { #nebius-creds data-toc-label="creds" }

###### `type` - (Required) `"service_account"` The type of credentials. Must be `service_account`. { #type data-toc-label='type' class='reference-item' }
###### `service_account_id` - (Optional) `str` Service account ID. Set automatically if `filename` is specified. When configuring via the UI, it must be specified explicitly. { #service_account_id data-toc-label='service_account_id' class='reference-item' }
###### `public_key_id` - (Optional) `str` ID of the service account public key. Set automatically if `filename` is specified. When configuring via the UI, it must be specified explicitly. { #public_key_id data-toc-label='public_key_id' class='reference-item' }
###### `private_key_file` - (Optional) `str` Path to the service account private key. Set automatically if `filename` or `private_key_content` is specified. When configuring via the UI, it must be specified explicitly. { #private_key_file data-toc-label='private_key_file' class='reference-item' }
###### `private_key_content` - (Optional) `str` Content of the service account private key. When configuring via `server/config.yml`, it's automatically filled from `private_key_file`. When configuring via UI, it has to be specified explicitly. { #private_key_content data-toc-label='private_key_content' class='reference-item' }
###### `filename` - (Optional) `str` The path to the service account credentials file. { #filename data-toc-label='filename' class='reference-item' }


##### `projects[n].backends[type=runpod]` { #runpod data-toc-label="runpod" }

###### `regions` - (Optional) `list[str]` The list of Runpod regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `community_cloud` - (Optional) `bool` Whether Community Cloud offers can be suggested in addition to Secure Cloud. Defaults to `false`. { #community_cloud data-toc-label='community_cloud' class='reference-item' }
###### [`creds`](#runpod-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=runpod].creds` { #runpod-creds data-toc-label="creds" }

###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=vastai]` { #vastai data-toc-label="vastai" }

###### `type` - (Required) `"vastai"` The type of backend. Must be `vastai`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of VastAI regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `community_cloud` - (Optional) `bool` Whether Community Cloud offers can be suggested in addition to Server Cloud. Defaults to `true`. { #community_cloud data-toc-label='community_cloud' class='reference-item' }
###### [`creds`](#vastai-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=vastai].creds` { #vastai-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


<!--

##### `projects[n].backends[type=tensordock]` { #tensordock data-toc-label="tensordock" }

###### `type` - (Required) `"tensordock"` The type of backend. Must be `tensordock`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of TensorDock regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#tensordock-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=tensordock].creds` { #tensordock-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }
###### `api_token` - (Required) `str` The API token. { #api_token data-toc-label='api_token' class='reference-item' }


-->

##### `projects[n].backends[type=oci]` { #oci data-toc-label="oci" }

###### `type` - (Required) `"oci"` The type of backend. Must be `oci`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of OCI regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### `compartment_id` - (Optional) `str` Compartment where `dstack` will create all resources. Omit to instruct `dstack` to create a new compartment. { #compartment_id data-toc-label='compartment_id' class='reference-item' }
###### [`creds`](#oci-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=oci].creds` { #oci-creds data-toc-label="creds" }

=== "Client"
    ###### `type` - (Required) `"client"` The type of credentials. Must be `client`. { #type data-toc-label='type' class='reference-item' }
    ###### `user` - (Required) `str` User OCID. { #user data-toc-label='user' class='reference-item' }
    ###### `tenancy` - (Required) `str` Tenancy OCID. { #tenancy data-toc-label='tenancy' class='reference-item' }
    ###### `key_file` - (Optional) `str` Path to the user's private PEM key. Either this or `key_content` should be set. { #key_file data-toc-label='key_file' class='reference-item' }
    ###### `key_content` - (Optional) `str` Content of the user's private PEM key. Either this or `key_file` should be set. { #key_content data-toc-label='key_content' class='reference-item' }
    ###### `pass_phrase` - (Optional) `str` Passphrase for the private PEM key if it is encrypted. { #pass_phrase data-toc-label='pass_phrase' class='reference-item' }
    ###### `fingerprint` - (Required) `str` User's public key fingerprint. { #fingerprint data-toc-label='fingerprint' class='reference-item' }
    ###### `region` - (Required) `str` Name or key of any region the tenancy is subscribed to. { #region data-toc-label='region' class='reference-item' }


=== "Default"
    ###### `type` - (Required) `"default"` The type of credentials. Must be `default`. { #type data-toc-label='type' class='reference-item' }
    ###### `file` - (Optional) `str` Path to the OCI CLI-compatible config file. Defaults to `~/.oci/config`. { #file data-toc-label='file' class='reference-item' }
    ###### `profile` - (Optional) `str` Profile to load from the config file. Defaults to `DEFAULT`. { #profile data-toc-label='profile' class='reference-item' }


##### `projects[n].backends[type=verda]` { #verda data-toc-label="verda" }

###### `type` - (Required) `"verda" | "datacrunch"` The type of backend. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of Verda regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#verda-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=verda].creds` { #verda-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `client_id` - (Required) `str` The client ID. { #client_id data-toc-label='client_id' class='reference-item' }
###### `client_secret` - (Required) `str` The client secret. { #client_secret data-toc-label='client_secret' class='reference-item' }


##### `projects[n].backends[type=kubernetes]` { #kubernetes data-toc-label="kubernetes" }

###### `type` - (Required) `"kubernetes"` The type of backend. Must be `kubernetes`. { #type data-toc-label='type' class='reference-item' }
###### `contexts` - (Optional) `list[str | object]` Enabled contexts (clusters). Each context should map to a separate cluster. The context name becomes the region name. If `contexts` is set, top-level `proxy_jump` and `namespace` must not be set. `proxy_jump`, if necessary, should be configured per-context; `namespace` is taken from the corresponding kubeconfig context's property. If `contexts` is not set (not recommended), the kubeconfig's `current-context` is used as the only context, with an empty string as the region name. { #contexts data-toc-label='contexts' class='reference-item' }
###### [`proxy_jump`](#kubernetes-proxy_jump) - (Optional) `object` Only used if `contexts` is not set; must not be set otherwise. The SSH proxy jump configuration. { #_proxy_jump data-toc-label='proxy_jump' class='reference-item' }
###### `namespace` - (Optional) `str` Only used if `contexts` is not set; must not be set otherwise. The namespace for resources managed by `dstack`. If `contexts` is not set, overrides the namespace set in the kubeconfig, even if not set. Defaults to `default`. Deprecated; will eventually be removed in future versions, but in the current version must be set if `contexts` is not set and the value is not equal to `default`. Future versions will use the namespace from the kubeconfig instead. To prepare for future versions, set the same value in the kubeconfig. { #namespace data-toc-label='namespace' class='reference-item' }
###### [`kubeconfig`](#kubernetes-kubeconfig) - (Required) `object` The kubeconfig configuration. { #_kubeconfig data-toc-label='kubeconfig' class='reference-item' }


###### `projects[n].backends[type=kubernetes].kubeconfig` { #kubernetes-kubeconfig data-toc-label="kubeconfig" }

###### `filename` - (Optional) `str` The path to the kubeconfig file. { #filename data-toc-label='filename' class='reference-item' }
###### `data` - (Optional) `str` The contents of the kubeconfig file. When configuring via `server/config.yml`, it's automatically filled from `filename`. When configuring via UI, it has to be specified explicitly. { #data data-toc-label='data' class='reference-item' }


??? info "Specifying `data`"
    To specify kubeconfig contents directly via `data`, convert it to a string:

    ```shell
    yq -o=json ~/.kube/config | jq -c | jq -R
    ```

###### `projects[n].backends[type=kubernetes].contexts[n]` { #kubernetes-contexts data-toc-label="contexts" }

###### `name` - (Required) `str` The name of the context. { #name data-toc-label='name' class='reference-item' }
###### [`proxy_jump`](#proxy_jump) - (Optional) `object` The SSH proxy jump configuration. { #_proxy_jump data-toc-label='proxy_jump' class='reference-item' }


###### `projects[n].backends[type=kubernetes].contexts[n].proxy_jump` { #kubernetes-contexts-proxy_jump data-toc-label="proxy_jump" }

###### `hostname` - (Optional) `str` The external IP address or hostname of any node. { #hostname data-toc-label='hostname' class='reference-item' }
###### `port` - (Optional) `int` Any port accessible outside of the cluster. { #port data-toc-label='port' class='reference-item' }


###### `projects[n].backends[type=kubernetes].proxy_jump` { #kubernetes-proxy_jump data-toc-label="proxy_jump" }

###### `hostname` - (Optional) `str` The external IP address or hostname of any node. { #hostname data-toc-label='hostname' class='reference-item' }
###### `port` - (Optional) `int` Any port accessible outside of the cluster. { #port data-toc-label='port' class='reference-item' }


##### `projects[n].backends[type=vultr]` { #vultr data-toc-label="vultr" }

###### `type` - (Required) `"vultr"` The type of backend. Must be `vultr`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of Vultr regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#vultr-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=vultr].creds` { #vultr-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=amddevcloud]` { #amddevcloud data-toc-label="amddevcloud" }

###### `type` - (Required) `"amddevcloud" | "digitalocean"` The type of backend. { #type data-toc-label='type' class='reference-item' }
###### `project_name` - (Optional) `str` The name of the project. { #project_name data-toc-label='project_name' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#amddevcloud-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=amddevcloud].creds` { #amddevcloud-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=digitalocean]` { #digitalocean data-toc-label="digitalocean" }

###### `type` - (Required) `"amddevcloud" | "digitalocean"` The type of backend. { #type data-toc-label='type' class='reference-item' }
###### `project_name` - (Optional) `str` The name of the project. { #project_name data-toc-label='project_name' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#digitalocean-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=digitalocean].creds` { #digitalocean-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=crusoe]` { #crusoe data-toc-label="crusoe" }

###### `type` - (Required) `"crusoe"` The type of backend. Must be `crusoe`. { #type data-toc-label='type' class='reference-item' }
###### `project_id` - (Required) `str` The Crusoe project ID. { #project_id data-toc-label='project_id' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of allowed Crusoe regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#crusoe-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=crusoe].creds` { #crusoe-creds data-toc-label="creds" }

###### `type` - (Required) `"access_key"` The type of credentials. Must be `access_key`. { #type data-toc-label='type' class='reference-item' }
###### `access_key` - (Required) `str` The Crusoe API access key. { #access_key data-toc-label='access_key' class='reference-item' }
###### `secret_key` - (Required) `str` The Crusoe API secret key. { #secret_key data-toc-label='secret_key' class='reference-item' }


##### `projects[n].backends[type=hotaisle]` { #hotaisle data-toc-label="hotaisle" }

###### `type` - (Required) `"hotaisle"` The type of backend. Must be `hotaisle`. { #type data-toc-label='type' class='reference-item' }
###### `team_handle` - (Required) `str` The Hot Aisle team handle. { #team_handle data-toc-label='team_handle' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of Hot Aisle regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#hotaisle-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=hotaisle].creds` { #hotaisle-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The Hot Aisle API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=jarvislabs]` { #jarvislabs data-toc-label="jarvislabs" }

###### `type` - (Required) `"jarvislabs"` The type of backend. Must be `jarvislabs`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of JarvisLabs regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#jarvislabs-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=jarvislabs].creds` { #jarvislabs-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The JarvisLabs API key. { #api_key data-toc-label='api_key' class='reference-item' }


##### `projects[n].backends[type=cloudrift]` { #cloudrift data-toc-label="cloudrift" }

###### `type` - (Required) `"cloudrift"` The type of backend. Must be `cloudrift`. { #type data-toc-label='type' class='reference-item' }
###### `regions` - (Optional) `list[str]` The list of CloudRift regions. Omit to use all regions. { #regions data-toc-label='regions' class='reference-item' }
###### [`creds`](#cloudrift-creds) - (Required) `object` The credentials. { #_creds data-toc-label='creds' class='reference-item' }


###### `projects[n].backends[type=cloudrift].creds` { #cloudrift-creds data-toc-label="creds" }

###### `type` - (Required) `"api_key"` The type of credentials. Must be `api_key`. { #type data-toc-label='type' class='reference-item' }
###### `api_key` - (Required) `str` The API key. { #api_key data-toc-label='api_key' class='reference-item' }


### `encryption` { #encryption data-toc-label="encryption" }

###### `keys` - (Required) `list[object]` The encryption keys. { #keys data-toc-label='keys' class='reference-item' }


#### `encryption.keys` { #encryption-keys data-toc-label="keys" }

##### `encryption.keys[n][type=identity]` { #encryption-keys-identity data-toc-label="identity" }

###### `type` - (Required) `"identity"` The type of the key. Must be `identity`. { #type data-toc-label='type' class='reference-item' }


##### `encryption.keys[n][type=aes]` { #encryption-keys-aes data-toc-label="aes" }

###### `type` - (Required) `"aes"` The type of the key. Must be `aes`. { #type data-toc-label='type' class='reference-item' }
###### `name` - (Required) `str` The key name for key identification. { #name data-toc-label='name' class='reference-item' }
###### `secret` - (Required) `str` Base64-encoded AES-256 key. { #secret data-toc-label='secret' class='reference-item' }


### `default_permissions` { #default_permissions data-toc-label="default_permissions" }

###### `allow_non_admins_create_projects` - (Optional) `bool` This flag controls whether regular users (non-global admins) can create and manage their own projects. Defaults to `true`. { #allow_non_admins_create_projects data-toc-label='allow_non_admins_create_projects' class='reference-item' }
###### `allow_non_admins_manage_ssh_fleets` - (Optional) `bool` This flag controls whether regular project members (i.e. Users) can add and delete SSH fleets. Defaults to `true`. { #allow_non_admins_manage_ssh_fleets data-toc-label='allow_non_admins_manage_ssh_fleets' class='reference-item' }
###### `allow_managers_manage_secrets` - (Optional) `bool` This flag controls whether project managers can manage project secrets. Defaults to `false`. { #allow_managers_manage_secrets data-toc-label='allow_managers_manage_secrets' class='reference-item' }

