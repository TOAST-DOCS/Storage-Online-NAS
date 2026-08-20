<!-- machine_translated: true -->

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

!!! tip "Note"
    For information on how to specify explicit resource dependencies, refer to [Terraform's Resource dependencies](https://developer.hashicorp.com/terraform/tutorials/configuration-language/dependencies) document.

<a id="terraform-resources-nas"></a>
## Resources { #terraform-resources-nas }

<a id="terraform-resources-create-volume"></a>
### Create a Volume { #terraform-resources-create-volume }

!!! tip "Note: Using the CIFS Protocol"
    To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register CIFS credentials to access each CIFS volume.
    You can create CIFS credentials in the **Storage > NAS > Manage CIFS Credentials** window in the console.
{%- if encryption %}
<!-- -->

!!! tip "Note: Encryption Key Store Settings"
    When you create an encrypted volume, the symmetric key used for encryption is stored in a key store in the NHN Cloud Secure Key Manager service. Therefore, to create an encrypted volume, you must first [create a key store](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_1) in the Secure Key Manager service. [Check the key store ID](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_2) and enter it in the encryption key store settings.
    You can enter the keystore ID from the **Storage > NAS > Encryption Key Store Settings** in the console. When you create an encrypted volume, the symmetric key is stored in the key store you set up. The symmetric key stored in the key store cannot be deleted while the encrypted volume is in use. If you delete the encrypted volume, the symmetric key is deleted as well.
    When you change the key store ID, the symmetric key for encrypted volumes you create in the future is stored in the changed key store. Symmetric keys stored in the existing key store are retained.
{%- endif %}
```hcl
# Create an empty NAS volume with the NFS protocol
resource "nhncloud_nas_storage_volume_v1" "volume_01" {
  name = "nas_volume_01"
  size_gb = 300
  mount_protocol {
    protocol = "nfs"
  }
}

# Create an empty NAS volume with the CIFS protocol
resource "nhncloud_nas_storage_volume_v1" "volume_02" {
  name = "nas_volume_02"
  size_gb = 300
  mount_protocol {
    protocol = "cifs"
    cifs_auth_ids = ["auth_id"]
  }
}

# Create a volume with settings such as ACL{% if encryption %}, encryption settings{% endif %} and more
resource "nhncloud_nas_storage_volume_v1" "volume_03" {
  name = "nas_volume_03"
  description = "create nas volume by terraform"
  size_gb = 300

  acl = ["10.10.10.0/24"]

{% if encryption %}
  encryption {
    enabled = true
  }

{% endif %}
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
| region | String | N | - | Region of the volume to be created<br>Default is the region set in the provider configuration file |
| name | String | Y | - | Volume name |
| description | String | N | O | Volume description |
| size_gb | Integer | Y | O | Volume size (GB)<br>The volume can be set from a minimum of 300 GB to a maximum of 10,000 GB, in 100 GB increments. |
| acl | List | N | O | ACL list to set when creating a volume<br>Can be entered in IP or CIDR format. |

{%- if encryption %}
| encryption | Object | N | - | Encryption configuration object for volume creation |
| encryption.enabled | Boolean | N | - | Whether encryption is enabled<br>After the encryption keystore is set up, setting its field to `true` enables encryption. |
{%- endif %}
| mount_protocol | Object | N | - | Protocol configuration object for volume creation |
| mount_protocol.cifs_auth_ids | List(String) | N | O | List of CIFS authentication IDs<br>No input required for NFS protocol selection |
| mount_protocol.protocol | String | Y | - | Protocol to use when mounting the volume<br>You can select either `nfs` or `cifs`. |
| snapshot_policy | Object | N | - | Volume snapshot configuration object |
| snapshot_policy.max_scheduled_count | Integer | N | O | Maximum number of snapshots to store<br>You can set a maximum of 30, and the first automatically created snapshot will be deleted when the maximum number of saves is reached. |
| snapshot_policy.reserve_percent | Integer | N | O | Snapshot capacity percentage |
| snapshot_policy.schedule | Object | N | - | Snapshot auto-creation object<br>If `null`, snapshot auto-creation will not be configured. |
| snapshot_policy.schedule.time | String | N | O | Snapshot auto-creation time |
| snapshot_policy.schedule.time_offset | String | N | O | Time zone for snapshot auto-create |
| snapshot_policy.schedule.weekdays | List | N | O | Days of the week for snapshot auto-creation<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |

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
| region | String | N | - | Region of the volume to attach<br>Default is the region set in the provider configuration file |
| volume_id | String | Y | - | ID of the volume to attach |
| subnet_id | String | Y | - | ID of the subnet to attach |

<a id="terraform-resources-set-replication"></a>
### Set up Replication { #terraform-resources-set-replication }

When you create a Replication Settings resource, the target volume is automatically created.

You can update the target volume by changing the `dst_volume` configuration value in the Replication Settings resource, but deleting the Replication Settings resource does not automatically delete the target volume.

Creating a replication configuration resource automatically generates a destination volume.
While you can update the destination volume by modifying the `dst_volume` parameters within the replication resource, the destination volume is not automatically deleted even if the replication configuration resource is removed.

!!! danger "Caution"
    Modifying certain values in the replication configuration resource may cause the existing resource to be destroyed and recreated; however, the original destination volume will persist.
    Please be aware that if the existing destination volume and the new one share the same name, the creation process may fail.

<!-- -->

!!! tip "Note"
    Destination volumes that remain after a resource deletion or update must be managed manually via the console.

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
| src_region | String | N | - | Region of the source volume<br>Default is the region set in the provider configuration file |
| src_volume_id | String | Y | - | ID of the source volume |
| dst_region | String | Y | - | Region of the replication target volume |
| dst_tenant_id | String | Y | - | Tenant ID of the replication target volume |
| dst_volume | Object | Y | - | Replication target volume creation request object |
| dst_volume.acl | List | N | O | ACL list to set when creating a volume<br>Can be entered in IP or CIDR format. |
| dst_volume.description | String | N | O | Volume description |

{%- if encryption %}
| dst_volume.encryption | Object | N | - | Encryption settings object for volume creation |
| dst_volume.encryption.enabled | Boolean | N | - | Whether to enable encryption<br>After the encryption key store is set up, setting this field to `true` enables encryption. |
{%- endif %}
| dst_volume.mount_protocol | Object | N | - | Protocol settings object for volume creation |
| dst_volume.mount_protocol.cifs_auth_ids | List(String) | N | O | List of CIFS authentication IDs<br>No input required for NFS protocol selection |
| dst_volume.mount_protocol.protocol | String | Y | - | Specifies the protocol for mounting the volume<br>You can select one of `nfs` or `cifs`. |
| dst_volume.name | String | Y | - | Volume name |
| dst_volume.size_gb | Integer | Y | O | Volume size (GB)<br>The volume can be set from a minimum of 300 GB to a maximum of 10,000 GB, in 100 GB increments. |
| dst_volume.snapshot_policy | Object | N | - | Volume snapshot settings object |
| dst_volume.snapshot_policy.max_scheduled_count | Integer | N | O | Maximum number of snapshots to store<br>You can set a maximum of 30, and the first automatically created snapshot will be deleted when the maximum number of saves is reached. |
| dst_volume.snapshot_policy.reserve_percent | Integer | N | O | Snapshot capacity ratio |
| dst_volume.snapshot_policy.schedule | Object | N | O | Snapshot auto-creation object<br>If `null`, snapshot auto-creation will not be configured. |
| dst_volume.snapshot_policy.schedule.time | String | N | O | Snapshot auto-creation time |
| dst_volume.snapshot_policy.schedule.time_offset | String | N | O | Reference time zone for snapshot auto-creation |
| dst_volume.snapshot_policy.schedule.weekdays | List | N | O | Days of the week for snapshot auto-creation<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |
{%- endif %}

<a id="reference"></a>
## References { #reference }

* Terraform - [https://www.terraform.io/](https://www.terraform.io/)
* Terraform Registry - [https://registry.terraform.io/](https://registry.terraform.io/)
