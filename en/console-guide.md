## Storage > NAS > Console User Guide

<a id="create_volume"></a>
### Create Volume

Create a new volume. The created volume can be accessed by instances using the network file system (NFS) and common internet file system (CIFS) protocols.

| Item | Description |
| -- | -- |
| Name | Name of the volume to be created. The NFS or CIFS access path can be created with the volume name. Volume name is limited to less than 100 alphabetic characters, numbers, and some symbols ('-', '_'). |
| Description | A description for volume |
| Size | Size of the volume to be created. It can be entered from a minimum of 300 GB to a maximum of 10,000 GB. |
| Protocol | Select a protocol to access the volume. CIFS and NFS are supported, but not simultaneously. |
| VPC | The virtual private cloud (VPC) to access the volume. |
| Subnet | The subnet to access the volume. Only subnets in the selected VPC can be chosen. |
| Access Control List (ACL) | A list of the IPs or CIDR blocks that allow read and write permissions. |
| Auto Create Snapshot | Snapshots are created automatically at a specified time once per day. When the maximum number of saves is reached, the first automatically created snapshot is deleted.  |
| Snapshot Reserve Capacity | Pre-allocate space for snapshots to be stored. Data can be stored in any capacity except the one you set. If the actual size of the snapshot is larger than the snapshot reserve capacity setting, the data storage space beyond the reserve capacity is used to store the snapshot.|
| Encryption  | Select whether to enable volume encryption. This must be preceded by setting up encryption key store. |

> [Note] 
> The number of subnets available for each project is limited to 3. To increase the limit, contact the Customer Center.

<a id="create_volume.cifs"></a>
#### Manage CIFS Credentials
To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register a CIFS credential to access each CIFS volume.

##### CIFS Credential Rules

* ID Rules
    * It must start with a lowercase letter or number and cannot end with a period (.).
    * Only lowercase letters (a–z), numbers (0–9), hyphens (-), periods (.), and underscores (_) are allowed. All other characters are not permitted.
    * You can enter a maximum of 20 characters.
    * You can't use IDs that consist only of numbers, match reserved words (e.g., administrator, default, guest, krbtgt), or duplicate existing IDs.

* Password Rules
    * It must be at least 6 characters long and contain at least three of the following character types: uppercase letters, lowercase letters, numbers, and special characters.
    * The special characters that can be used are `~!@#^*_-+=\\|()[]:;"'<>,.?/`, and spaces are not allowed.
    * Passwords that include the ID cannot be used.

<a id="create_volume.encryption"></a>
#### Encryption Key Store Settings

Encrypted volume stores symmetric keys used for encryption in a key store in the NHN Cloud Secure Key Manager service. Therefore, to create encrypted volume, you must [create a key store](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_1) in the Secure Key Manager service in advance. [Check the ID of the key store](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_2) and enter it in the encryption key store settings.

When you create encrypted volume, the symmetric key is stored in the key store you set up. The symmetric key stored in the key store by the NAS service cannot be deleted while using encrypted volume. If you delete encrypted volume, the symmetric key is also deleted.

When you change the key store ID, the symmetric key for encrypted volume you create in the future is stored in the changed key store. Symmetric keys stored in the existing key store are retained.

> [Note]
> You will be charged for key store usage according to the Secure Key Manager service pricing policy. For more information, see [Secure Key Manager pricing](https://www.nhncloud.com/kr/service/security/secure-key-manager#price).
>
> Encrypted volume encrypts data with the XTS-AES-256 algorithm using two different symmetric keys. Therefore, two symmetric keys are stored in the key store for each encrypted volume.

<a id="change_volume_size"></a>
### Change Volume Size

Volume size is changed. Volume can be scaled up and down even while in use.

<a id="change_acl"></a>
### Change Access Control List Settings

The settings for the volume’s access control list is changed.

<a id="change_snapshots_settings"></a>
### Change Snapshot Settings

The settings for Auto Create Snapshot and Snapshot Reserve Capacity are changed.

<a id="delete_volume"></a>
### Delete Volume

Volume is deleted.

> [Caution]
> It is recommended to unmount the volume from the associated instance before deleting. Deleting the volume while it is mounted may cause problems on the user's system.
>
> When deleting the volume, all data including snapshots are deleted. Deleted data cannot be recovered.

<a id="snapshots"></a>
### Snapshot
Retrieve a list of created snapshots.

| Item | Description |
| --- | --- |
| Name | The name of the snapshot. If created by the system, the name is determined by a specified rule. |
| Volume usage when snapshots created | The volume usage at the time the snapshot is created. If the snapshot is being stored as it exceeded the snapshot reserve capacity, it appears as the data capacity excluding the excess capacity. |
| Created date | When the snapshot is created. |

> [Note]
> Snapshots prioritize the storage space on the volume by using the reserved capacity for snapshots. If the total snapshot size exceeds the reserved capacity, the excess is stored in the data storage space.

> [Note]
> Some snapshots are created by the system and cannot be deleted.


<a id="snapshots.create"></a>
### Create Snapshots

Snapshots of volume are created immediately.

<a id="snapshots.restore"></a>
#### Restore Snapshots
Restores volume to the point in time when the snapshot was created.

> [Caution]
> When restoring, snapshots created after the point of restoration are automatically deleted.

<a id="snapshots.restore_results"></a>
#### View Snapshot Restore Results
View the history of restoring volume.

<a id="snapshots.delete"></a>
#### Delete Snapshots

A specified snapshot is deleted. Once deleted, snapshots cannot be recovered.

<a id="network"></a>
### Network
Check the network connection information.

| Item | Description |
| --- | --- |
| Connection information | The connection path that the instance will use when mounting. |
| Subnet | The subnet information associated with the volume. |
| Status | The subnet association status. |

<a id="network.add_subnet"></a>
#### Add Subnet Association
Add a subnet association. Adding a subnet association allows you to access volume from the added subnet.

> [Caution]
> After adding a subnet association, if the subnet band does not exist in the ACL, mounting is not possible.

<a id="network.detach_subnet"></a>
#### Detach Subnet
Remove the subnet associated with the volume. IP ACLs must be removed separately if needed.

> [Caution]
> It is recommended to detach the subnet after unmounting from a connected instance. Detaching while mounted can cause problems for user systems.

<a id="monitoring"></a>
### Monitoring

Check various metrics of volume with graphs. After selecting the volume to check, click the **Monitoring** tab.

The default search period is the latest 1 hour, and you can search any period you want through the search period filter. Descriptions of monitoring metrics are found in the table below.

| Metric | Unit | Description |
| --- | --- | --- |
| Volume capacity | byte | The total capacity of volume and the capacity in use. |
| Volume usage | % | The capacity in use as a percentage of the total volume capacity. |
| IOPS | OPS | Volume operations per second. |
| Latency | usec | The volume latency time. |
| Snapshot number | - | The number of snapshots in volume. |
| Snapshot capacity | byte | The amount of capacity that volume is using for the snapshot. |
| Inode | - | The number of volume inodes. |
| Volume status | - | The volume status. |

<a id="replication"></a>
### Replication

View information about replication settings.

| Setting | Description |
| --- | --- |
| Replication Settings | Whether replication is enabled for the volume. |
| Replication setup status | The status of replication settings between source and target volume.<br>Active: Active status<br>Retrieve failed: Temporary failure to obtain information<br>Halt: Paused state<br>Initial replication progress additionally displays the replication progress and target volume encryption status. |
| Results of recent replication | The results of the last replication you ran. |
| Latest replication execution time | The last time you ran a replication. |
| Last replication success time | The last time replication completed. |
| Replication direction | The replication direction. |
| Target region | The replication target region. Only exposed if this is the source volume that you set up replication for. |
| Target volume | The replication target volume name. Exposed only if it is the source volume for which replication is set up.|
| Source region | The replication source region. Only exposed if replication is enabled on the target volume.|
| Source volume | The replication source volume name. Only exposed if replication is enabled on the target volume. |

<a id="replication.settings"></a>
#### Replication Settings

Set up replication to a selected region of a project within your organization.
When you set up replication, a target volume is created in the target location with the same size as the source volume. The target volume is created in a read-only state, and you must stop replication or turn off replication to change the state of the target volume.
When you update or delete data in the source volume, the data in the target volume is also updated or deleted.

Check the range of regions selectable by target project.

| Target project | Selectable region |
| -- | -- |
| Same project within an organization | Other regions |
| Other projects in an organization | All regions |

> [Caution]
> To change the size of a replicated volume, you must change both the source volume and the target volume. If the size of the source volume and the target volume are different, replication might fail.

> [Note]
> Volume replication is performed using snapshots. When configuring replication, snapshots in the format `{volume name}.mirror.%` are automatically created and cannot be deleted.

> [Note]
> The target volume created by the replication setup is billed for storage capacity according to the NAS service fee policy.
>
> You are charged for network traffic generated by replication between different regions.
>
> For more information on pricing, see our [NAS pricing guide](https://www.nhncloud.com/kr/service/storage/nas).

<a id="replication.start"></a>
#### Start Replication

Resumes replication of volume that is in a stopped state.
Replication runs asynchronously when changes occur on the source volume. Before replication runs, the data on the source and target volume might not match.

> [Caution]
> Replication might fail if the target volume size is smaller than the source volume size.
>
> When replication runs, all existing data on the target volume is deleted and replaced with the same geometry as the source volume.

<a id="replication.stop"></a>
#### Stop Replication

Stops replicating volume.
When you stop replication, the target volume becomes writable and can be mounted and used.

> [Caution]
> If you stop replication, the data on the source and target volume might not match. 

> [Note]
> Volume in the Stop replication state might also experience a small amount of traffic to check replication status.

<a id="replication.change_direction"></a>
#### Change Replication Direction

Change the direction of replication between source and target volume.

* Change replication direction from **[source->target]** to **[target->source]**
    * All existing data on the source volume is deleted, and the data on the target volume is replicated to the source volume.
    * Replication might fail if the source volume size is smaller than the target volume size.

* Change replication direction from **[target->source]** to **[source->target]** (revert)
    * All existing data on the target volume is deleted, and the data from the source volume is replicated to the target volume.
    * Replication might fail if the target volume size is smaller than the source volume size.

<a id="replication.disable"></a>
#### Disable Replication Settings

Disables volume replication.
When you disable replication, the target volume retains the source volume data as it was when the last replication completed.

> [Caution]
> When you re-establish replication, a new target volume is created. You cannot use the previously used target volume as the replication target volume again.

<a id="connect_volume"></a>
### Connect Volume

Volume can be mounted on instances by using the connection information of the created volume. However, the instance on which volume is mounted must be connected to the specified subnet.

<a id="connect_volume.nfs"></a>
#### NFS

##### Install NFS Package 

* Debian, Ubuntu: nfs-common, rpcbind
  ```
  sudo apt-get install nfs-common rpcbind
  ```
* CentOS: nfs-utils, rpcbind
  ```
  sudo yum install nfs-utils rpcbind
  ```

##### Run rpcbind Service 

```
sudo service rpcbind start
```

##### Mount volume

```
sudo mount -t nfs {nas source} {mount point}
```

* nas source: Volume information 
Example: 192.168.0.241:/data
* mount point: Directory on which volume is mounted 
Example: /mnt

<a id="connect_volume.cifs"></a>
#### CIFS

In File Explorer, right-click **My PC** in the left navigation pane and **Connect Network Drive**. In the Connect Network Drive window, select the drive you want to mount and enter the folder path.  The folder path is formatted as `\\{volume IP}\{volume name}`, for example: \\\\192.168.0.100\\cifs