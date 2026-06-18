# `volume`

The `volume` configuration type allows creating, registering, and updating [volumes](../../concepts/volumes.md).

=== "AWS"

    ###### `backend` - (Required) `"aws"` The volume backend. Must be `aws`. { #backend data-toc-label='backend' class='reference-item' }
    ###### `name` - (Optional) `str` The volume name. { #name data-toc-label='name' class='reference-item' }
    ###### `size` - (Optional) `str` The volume size. Must be specified when creating new volumes. { #size data-toc-label='size' class='reference-item' }
    ###### `auto_cleanup_duration` - (Optional) `int | str` Time to wait after volume is no longer used by any job before deleting it. Defaults to keep the volume indefinitely. Use the value `off` or `-1` to disable auto-cleanup. { #auto_cleanup_duration data-toc-label='auto_cleanup_duration' class='reference-item' }
    ###### `tags` - (Optional) `dict` The custom tags to associate with the volume. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }
    ###### `volume_id` - (Optional) `str` The volume ID. Must be specified when registering external volumes. { #volume_id data-toc-label='volume_id' class='reference-item' }
    ###### `region` - (Required) `str` The volume region. { #region data-toc-label='region' class='reference-item' }
    ###### `availability_zone` - (Optional) `str` The volume availability zone. { #availability_zone data-toc-label='availability_zone' class='reference-item' }


=== "GCP"

    ###### `backend` - (Required) `"gcp"` The volume backend. Must be `gcp`. { #backend data-toc-label='backend' class='reference-item' }
    ###### `name` - (Optional) `str` The volume name. { #name data-toc-label='name' class='reference-item' }
    ###### `size` - (Optional) `str` The volume size. Must be specified when creating new volumes. { #size data-toc-label='size' class='reference-item' }
    ###### `auto_cleanup_duration` - (Optional) `int | str` Time to wait after volume is no longer used by any job before deleting it. Defaults to keep the volume indefinitely. Use the value `off` or `-1` to disable auto-cleanup. { #auto_cleanup_duration data-toc-label='auto_cleanup_duration' class='reference-item' }
    ###### `tags` - (Optional) `dict` The custom tags to associate with the volume. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }
    ###### `volume_id` - (Optional) `str` The volume ID. Must be specified when registering external volumes. { #volume_id data-toc-label='volume_id' class='reference-item' }
    ###### `region` - (Required) `str` The volume region. { #region data-toc-label='region' class='reference-item' }
    ###### `availability_zone` - (Optional) `str` The volume availability zone. { #availability_zone data-toc-label='availability_zone' class='reference-item' }


=== "Runpod"

    ###### `backend` - (Required) `"runpod"` The volume backend. Must be `runpod`. { #backend data-toc-label='backend' class='reference-item' }
    ###### `name` - (Optional) `str` The volume name. { #name data-toc-label='name' class='reference-item' }
    ###### `size` - (Optional) `str` The volume size. Must be specified when creating new volumes. { #size data-toc-label='size' class='reference-item' }
    ###### `auto_cleanup_duration` - (Optional) `int | str` Time to wait after volume is no longer used by any job before deleting it. Defaults to keep the volume indefinitely. Use the value `off` or `-1` to disable auto-cleanup. { #auto_cleanup_duration data-toc-label='auto_cleanup_duration' class='reference-item' }
    ###### `tags` - (Optional) `dict` The custom tags to associate with the volume. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }
    ###### `volume_id` - (Optional) `str` The volume ID. Must be specified when registering external volumes. { #volume_id data-toc-label='volume_id' class='reference-item' }
    ###### `region` - (Required) `str` The volume region. { #region data-toc-label='region' class='reference-item' }


=== "Kubernetes"

    Kubernetes backend volumes are mapped to [`PersistentVolumeClaim`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) objects.

    To create a new claim, specify `size` and optionally `storage_class_name` and/or `access_modes`:

    ```yaml
    type: volume
    backend: kubernetes
    name: new-volume
    size: 100GB
    # By default, storage_class_name is not set, and the decision is delegated to
    # the DefaultStorageClass admission controller (if it is enabled)
    storage_class_name: test-nfs
    # access_modes defaults to [ReadWriteOnce]. For multi-attach-capable volumes
    # use ReadWriteMany and/or ReadOnlyMany
    access_modes:
      - ReadWriteMany
    ```

    To reuse an existing claim, specify `claim_name`:

    ```yaml
    type: volume
    backend: kubernetes
    name: existing-volume
    claim_name: existing-pvc
    ```

    ###### `backend` - (Required) `"kubernetes"` The volume backend. Must be `kubernetes`. { #backend data-toc-label='backend' class='reference-item' }
    ###### `name` - (Optional) `str` The volume name. { #name data-toc-label='name' class='reference-item' }
    ###### `size` - (Optional) `str` The requested volume size. Must be specified when creating new PVCs. Ignored if `claim_name` is set. { #size data-toc-label='size' class='reference-item' }
    ###### `auto_cleanup_duration` - (Optional) `int | str` Time to wait after volume is no longer used by any job before deleting it. Defaults to keep the volume indefinitely. Use the value `off` or `-1` to disable auto-cleanup. { #auto_cleanup_duration data-toc-label='auto_cleanup_duration' class='reference-item' }
    ###### `tags` - (Optional) `dict` The custom tags to associate with the volume. The tags are also propagated to the underlying backend resources. If there is a conflict with backend-level tags, does not override them. { #tags data-toc-label='tags' class='reference-item' }
    ###### `region` - (Required) `str` The volume region (cluster). { #region data-toc-label='region' class='reference-item' }
    ###### `claim_name` - (Optional) `str` The `PersistentVolumeClaim` name. Must be specified when registering the existing PVC instead of creating a new one. { #claim_name data-toc-label='claim_name' class='reference-item' }
    ###### `storage_class_name` - (Optional) `str` The `StorageClass` name. Ignored if `claim_name` is set. { #storage_class_name data-toc-label='storage_class_name' class='reference-item' }
    ###### `access_modes` - (Optional) `list[str]` A list of accepted access modes. Ignored if `claim_name` is set. Defaults to `["ReadWriteOnce"]`. { #access_modes data-toc-label='access_modes' class='reference-item' }
    ###### `read_only` - (Optional) `bool` If `true`, enforces the volume to be mounted as read-only. Defaults to `false`. { #read_only data-toc-label='read_only' class='reference-item' }

