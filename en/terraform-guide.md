<!-- pre-align:aligned sig=ab931ac9d8ba -->

<a id="storage-nas-terraform-user-guide"></a>
## Storage > NAS > Terraform User Guide { #storage-nas-terraform-user-guide }
This document details how to use NHN Cloud NAS services with Terraform.

<a id="terraform"></a>
## Terraform { #terraform }

Terraform is an open-source tool designed for seamless infrastructure provisioning, secure updates, and efficient configuration management. For basics, refer to [User Guide > NHN Cloud > Terraform User Guide](/nhncloud/en/terraform-guide/).

<a id="terraform-resource-dependency"></a>
### Resource dependency { #terraform-resource-dependency }

While resources are generally independent, some may have dependencies on others. When a resource references information from another through its label, Terraform automatically establishes these dependencies.
For example, an interface named `interface1` that connects to a volume named `volume1` can be defined as follows.

```hcl
# volume resource
resource "nhncloud_nas_storage_volume_v1" "volume1" {
  name = "volume1"
  size_gb = 300

  mount_protocol {
    protocol = "nfs"
  }
}

# interface resource
resource "nhncloud_nas_storage_volume_interface_v1" "interface1" {
  volume_id = nhncloud_nas_storage_volume_v1.volume1.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
}
```

> [Note]
> For information on how to specify explicit resource dependencies, refer to [Terraform's Resource dependencies](https://developer.hashicorp.com/terraform/tutorials/configuration-language/dependencies) document.

<a id="terraform-resources-nas"></a>
## Resources { #terraform-resources-nas }

<a id="terraform-resources-create-volume"></a>
### Create a Volume { #terraform-resources-create-volume }

> [Note] Using the CIFS protocol
> To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register CIFS credentials to allow to access each CIFS volume.
> You can create CIFS credentials through the **Storage > NAS > Manage CIFS Credentials** of the console.

<!-- -->

> [Note] Setting up encryption key storage
> When an encrypted volume is created, the symmetric key used for encryption is stored in the NHN Cloud Secure Key Manager store. To create encrypted volume,[you must first create a keystore](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_1) in the Secure Key Manager service. After creating the keystore, [check its ID](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_2) and enter it in the encryption keystore settings.
> You can enter the keystore ID from the **Storage > NAS > Encryption keystore settings** in the console. When you create encrypted volume, the symmetric key is stored in the specified keystore. The symmetric key stored in the keystore cannot be deleted while the encrypted volume is in use. When the encrypted volume is deleted, the corresponding symmetric key is also deleted.
> If you change the keystore ID, symmetric keys for newly created encrypted volume will be stored in the new keystore. Symmetric keys already stored in the previous keystore are retained.


```hcl
# Create an Empty NAS Volume with NFS Protocol
resource "nhncloud_nas_storage_volume_v1" "volume_01" {
  name = "nas_volume_01"
  size_gb = 300
  mount_protocol {
    protocol = "nfs"
  }
}

# Create an Empty NAS Volume with CIFS Protocol
resource "nhncloud_nas_storage_volume_v1" "volume_02" {
  name = "nas_volume_02"
  size_gb = 300
  mount_protocol {
    protocol = "cifs"
    cifs_auth_ids = ["auth_id"]
  }
}

# Create a Volume with ACL and Encryption Settings
resource "nhncloud_nas_storage_volume_v1" "volume_03" {
  name = "nas_volume_03"
  description = "create nas volume by terraform"
  size_gb = 300

  acl = ["10.10.10.0/24"]

  encryption {
    enabled = true
  }

  mount_protocol {
    protocol = "cifs"
    cifs_auth_ids = ["auth_id"]
  }

  snapshot_policy {
    max_scheduled_count = 3
    reserve_percent = 10

    schedule {
      time = "00:00"
      time_offset = "+09:00"
      weekdays = [1, 3, 5]
    }
  }
}
```

| Name | Type | Required | Modifiable | Description |
| --- | --- | --- | --- | --- |
| region | String | - | - | Region of the volume to be created<br>Default is the region set in the provider configuration file |
| name | String | O | - | Volume name |
| description | String | - | O | Volume description |
| size_gb | Integer | O | O | Volume size (GB)<br>The volume can be set from a minimum of 300 GB to a maximum of 10,000 GB, in 100 GB increments. |
| acl | List | - | O | ACL list to set when creating a volume<br>Can be entered in IP or CIDR format. |
| encryption | Object | - | - | Encryption setting object when creating a volume |
| encryption.enabled | Boolean | - | - | Whether encryption is enabled<br>Encryption is enabled when this field is set to `true` after the encryption keystore is set. |
| mount_protocol | Object | - | - | Protocol setting object when creating a volume |
| mount_protocol.cifs_auth_ids | List(String) | - | O | List of CIFS authentication IDs<br>No input required when selecting the NFS protocol |
| mount_protocol.protocol | String | O | - | Protocol specification when mounting a volume<br>You can select either `nfs` or `cifs`. |
| snapshot_policy | Object | - | - | Volume snapshot setting object |
| snapshot_policy.max_scheduled_count | Integer | - | O | Maximum number of snapshots to store<br>You can set up to 30. When the maximum number of snapshots is reached, the oldest snapshot among the automatically created snapshots will be deleted. |
| snapshot_policy.reserve_percent | Integer | - | O | Snapshot capacity ratio |
| snapshot_policy.schedule | Object | - | - | Snapshot auto-generation object<br>If `null`, automatic snapshot generation is not set. |
| snapshot_policy.schedule.time | String | - | O | Automatic snapshot generation time |
| snapshot_policy.schedule.time_offset | String | - | O | Automatic snapshot generation time zone |
| snapshot_policy.schedule.weekdays | List | - | O | Automatic snapshot generation days<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday).

<a id="terraform-resources-connect-interface"></a>
### Attach an Interface to a Volume { #terraform-resources-connect-interface }

```hcl
data "nhncloud_networking_vpcsubnet_v2" "default_subnet" {
  ...
}

resource "nhncloud_nas_storage_volume_interface_v1" "nas_interface_01" {
  volume_id = nhncloud_nas_storage_volume_v1.volume_01.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
}
```

| Name | Type | Required | Modifiable | Description |
| --- | --- | --- | --- | --- |
| region | String | - | - | Region of the volume to attach<br>Default is the region set in the provider configuration file |
| volume_id | String | O | - | ID of the volume to attach |
| subnet_id | String | O | - | ID of the subnet to attach |

<a id="terraform-resources-set-replication"></a>
### Set up Replication { #terraform-resources-set-replication }

Creating a replication configuration resource automatically generates a destination volume.
While you can update the destination volume by modifying the `dst_volume` parameters within the replication resource, the destination volume is not automatically deleted even if the replication configuration resource is removed.

> [Caution]
> Modifying certain values in the replication configuration resource may cause the existing resource to be destroyed and recreated; however, the original destination volume will persist.
> Please be aware that if the existing destination volume and the new one share the same name, the creation process may fail.

<!-- -->

> [Note]
> Destination volumes that remain after a resource deletion or update must be managed manually via the console.

```hcl
resource "nhncloud_nas_storage_volume_mirror_v1" "nas_mirror_01" {
  src_volume_id = nhncloud_nas_storage_volume_v1.volume_01.id
  dst_region    = "KR2"
  dst_tenant_id = "ba3be1254ab141bcaef674e74630a31f"

  dst_volume {
    name        = "nas_mirror"
    description = "create nas mirror by terraform"
    size_gb     = 400

    mount_protocol {
      protocol = "nfs"
    }
  }
}
```

| Name | Type | Required | Modifiable | Description |
| --- | --- | --- | --- | --- |
| src_region | String | - | - | Region of the source volume<br>Default is the region set in the provider configuration file |
| src_volume_id | String | O | - | ID of the source volume |
| dst_region | String | O | - | Region of the replication target volume |
| dst_tenant_id | String | O | - | Tenant ID of the replication target volume |
| dst_volume | Object | O | - | Replication target volume creation request object |
| dst_volume.acl | List | - | O | ACL list to set when creating a volume<br>Can be entered in IP or CIDR format. |
| dst_volume.description | String | - | O | Volume description |
| dst_volume.encryption | Object | - | - | Encryption setting object when creating a volume |
| dst_volume.encryption.enabled | Boolean | - | - | Whether to enable encryption setting<br>Encryption is enabled when this field is set to `true` after the encryption keystore is set. |
| dst_volume.mount_protocol | Object | - | - | Protocol setting object when creating a volume |
| dst_volume.mount_protocol.cifs_auth_ids | List | - | O | List of CIFS authentication IDs<br>No input required when selecting an NFS protocol |
| dst_volume.mount_protocol.protocol | String | O | - | Specify protocol when mounting a volume<br>You can select either `nfs` or `cifs`. |
| dst_volume.name | String | O | - | Volume name |
| dst_volume.size_gb | Integer | O | O | Volume Size (GB)<br>The volume can be set from a minimum of 300 GB to a maximum of 10,000 GB, in 100 GB increments. |
| dst_volume.snapshot_policy | Object | - | - | Volume Snapshot Setting Object |
| dst_volume.snapshot_policy.max_scheduled_count | Integer | - | O | Maximum Number of Snapshots to Store<br>You can set up to 30. When the maximum number of snapshots is reached, the oldest automatically created snapshot will be deleted. |
| dst_volume.snapshot_policy.reserve_percent | Integer | - | O | Snapshot Capacity Ratio |
| dst_volume.snapshot_policy.schedule | Object | - | O | Automatic Snapshot Creation Object<br>If `null`, automatic snapshot creation is not set. |
| dst_volume.snapshot_policy.schedule.time | String | - | O | Automatic snapshot creation time |
| dst_volume.snapshot_policy.schedule.time_offset | String | - | O | Automatic snapshot creation time zone |
| dst_volume.snapshot_policy.schedule.weekdays | List | - | O | Automatic snapshot creation days<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday).

<a id="reference"></a>
## References { #reference }

Terraform - [https://www.terraform.io/](https://www.terraform.io/)
Terraform Registry - [https://registry.terraform.io/](https://registry.terraform.io/)
