## Storage > NAS > Console User Guide

### Create Storage

Create a new storage. The created storage can be accessed by instances using the network file system (NFS) and common internet file system (CIFS) protocols.

| Item | Description |
| -- | -- |
| Name | Name of the storage to be created. The NFS or CIFS access path can be created with the storage name. Storage name is limited to less than 100 alphabetic characters, numbers, and some symbols ('-', '_'). |
| Description | A description for storage |
| Size | Size of the storage to be created. It can be entered from a minimum of 300 GB to a maximum of 10,000 GB. |
| Protocol | Select a protocol to access the storage. CIFS and NFS are supported, but not simultaneously. |
| VPC | The virtual private cloud (VPC) to access the storage. |
| Subnet | The subnet to access the storage. Only subnets in the selected VPC can be chosen. |
| Access Control List (ACL) | A list of the IPs or CIDR blocks that allow read and write permissions. |
| Auto Create Snapshot | Snapshots are created automatically at a specified time once per day. When the maximum number of saves is reached, the first automatically created snapshot is deleted.  |
| Snapshot Reserve Capacity | Pre-allocate space for snapshots to be stored. Data can be stored in any capacity except the one you set. If the actual size of the snapshot is larger than the snapshot reserve capacity setting, the data storage space beyond the reserve capacity is used to store the snapshot.|
| Encryption  | Select whether to enable storage encryption. This must be preceded by setting up encryption key store. |

> [Note] 
> The number of subnets available for each project is limited to 3 as of Februrary 2024. To increase the limit, contact the Customer Center.

#### Manage CIFS Credentials
To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register a CIFS credential to access each CIFS storage.

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

#### Encryption Key Store Settings

NAS encrypted storage stores symmetric keys used for encryption in a key store in the NHN Cloud Secure Key Manager service. Therefore, to create encrypted storage, you must [create a key store](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_1) in the Secure Key Manager service in advance. [Check the ID of the key store](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_2) and enter it in the encryption key store settings.

When you create encrypted storage, the symmetric key is stored in the key store you set up. The symmetric key stored in the key store by the NAS service cannot be deleted while using encrypted storage. If you delete encrypted storage, the symmetric key is also deleted.

When you change the key store ID, the symmetric key for encrypted storage you create in the future is stored in the changed key store. Symmetric keys stored in the existing key store are retained.

> [Note]
> You will be charged for key store usage according to the Secure Key Manager service pricing policy. For more information, see [Secure Key Manager pricing](https://www.nhncloud.com/kr/service/security/secure-key-manager#price).
>
> NAS encrypted storage encrypts data with the XTS-AES-256 algorithm using two different symmetric keys. Therefore, two symmetric keys are stored in the key store for each encrypted storage.

### Change Storage Size

NAS storage size is changed. Storage can be scaled up and down even while in use.

### Change Access Control List Settings

The settings for the storage’s access control list is changed.

### Change Snapshot Settings

The settings for Auto Create Snapshot and Snapshot Reserve Capacity are changed.

### Delete Storage

NAS storage is deleted.

> [Caution]
> It is recommended to unmount the storage from the associated instance before deleting. Deleting the storage while it is mounted may cause problems on the user's system.
>
> When deleting the storage, all data including snapshots are deleted. Deleted data cannot be recovered.

### Snapshot
Retrieve a list of created snapshots.

| Item | Description |
| --- | --- |
| Name | The name of the snapshot. If created by the system, the name is determined by a specified rule. |
| Storage usage when snapshots created | The NAS storage usage at the time the snapshot is created. If the snapshot is being stored as it exceeded the snapshot reserve capacity, it appears as the data capacity excluding the excess capacity. |
| Created date | When the snapshot is created. |

> [Note]]
> Snapshots prioritize the storage space on the NAS by using the reserved capacity for snapshots. If the total snapshot size exceeds the reserved capacity, the excess is stored in the data storage space.

> [Note]
> Some snapshots are created by the system and cannot be deleted.


### Create Snapshots

Snapshots of NAS storage are created immediately.

#### Restore Snapshots
Restores storage to the point in time when the snapshot was created.

> [Caution]
> When restoring, snapshots created after the point of restoration are automatically deleted.

#### View Snapshot Restore Results
View the history of restoring storage.

#### Delete Snapshots

A specified snapshot is deleted. Once deleted, snapshots cannot be recovered.

### Network
Check the network connection information.

| Item | Description |
| --- | --- |
| Connection information | The connection path that the instance will use when mounting. |
| Subnet | The subnet information associated with the storage. |
| Status | The subnet association status. |


#### Add Subnet Association
Add a subnet association. Adding a subnet association allows you to access NAS storage from the added subnet.

> [Caution]
> After adding a subnet association, if the subnet band does not exist in the ACL, mounting is not possible.

#### Detach Subnet
Remove the subnet associated with the storage. IP ACLs must be removed separately if needed.

> [Caution]
> It is recommended to detach the subnet after unmounting from a connected instance. Detaching while mounted can cause problems for user systems.

### Monitoring

Check various metrics of NAS storage with graphs. After selecting the storage to check, click the **Monitoring** tab.

The default search period is the latest 1 hour, and you can search any period you want through the search period filter. Descriptions of monitoring metrics are found in the table below.

| Metric | Unit | Description |
| --- | --- | --- |
| Storage capacity | byte | The total capacity of storage and the capacity in use. |
| Storage usage | % | The capacity in use as a percentage of the total storage capacity. |
| IOPS | OPS | Storage operations per second. |
| Latency | usec | The storage latency time. |
| Snapshot number | - | The number of snapshots in storage. |
| Snapshot capacity | byte | The amount of capacity that storage is using for the snapshot. |
| Inode | - | The number of storage inodes. |
| Storage status | - | The storage status. |
| Number of client connections | - | The number of clients connected to storage. |

### Replication

View information about replication settings.

| Setting | Description |
| --- | --- |
| Replication Settings | Whether replication is enabled for the storage. |
| Replication setup status | The status of replication settings between source and target storage.<br>Active: Active status<br>Retrieve failed: Temporary failure to obtain information<br>Halt: Paused state<br>Initial replication progress additionally displays the replication progress and target storage encryption status. |
| Results of recent replication | The results of the last replication you ran. |
| Latest replication execution time | The last time you ran a replication. |
| Last replication success time | The last time replication completed. |
| Replication direction | The replication direction. |
| Target region | The replication target region. Only exposed if this is the source storage that you set up replication for. |
| Target storage | The replication target storage name. Exposed only if it is the source storage for which replication is set up.|
| Source region | The replication source region. Only exposed if replication is enabled on the target storage.|
| Source storage | The replication source storage name. Only exposed if replication is enabled on the target storage. |

#### Replication Settings

Set up replication to a selected region of a project within your organization.
When you set up replication, a target storage is created in the target location with the same size as the source storage. The target storage is created in a read-only state, and you must stop replication or turn off replication to change the state of the target storage.
When you update or delete data in the source storage, the data in the target storage is also updated or deleted.

Check the range of regions selectable by target project.

| Target project | Selectable region |
| -- | -- |
| Same project within an organization | Other regions |
| Other projects in an organization | All regions |

> [Caution]
> To change the size of a replicated storage, you must change both the source storage and the target storage. If the size of the source storage and the target storage are different, replication might fail.

> [Note]
> Storage replication is performed using snapshots. When configuring replication, snapshots in the format `{storage name}.mirror.%` are automatically created and cannot be deleted.

> [Note]
> The target storage created by the replication setup is billed for storage capacity according to the NAS service fee policy.
>
> You are charged for network traffic generated by replication between different regions.
>
> For more information on pricing, see our [NAS pricing guide](https://www.nhncloud.com/kr/service/storage/nas).

#### Start Replication

Resumes replication of storage that is in a stopped state.
Replication runs asynchronously when changes occur on the source storage. Before replication runs, the data on the source and target storage might not match.

> [Caution]
> Replication might fail if the target storage size is smaller than the source storage size.
>
> When replication runs, all existing data on the target storage is deleted and replaced with the same geometry as the source storage.

#### Stop Replication

Stops replicating storage.
When you stop replication, the target storage becomes writable and can be mounted and used.

> [Caution]
> If you stop replication, the data on the source and target storage might not match. 

> [Note]
> Storage in the Stop replication state might also experience a small amount of traffic to check replication status.

#### Change Replication Direction

Change the direction of replication between source and target storage.

* Change replication direction from \*\*[source->target** to \*\*[target->source**)
    * All existing data on the source storage is deleted, and the data on the target storage is replicated to the source storage.
    * Replication might fail if the source storage size is smaller than the target storage size.

* Change replication direction from \*\*[target->source]** to \*\*[source->target]** (revert)
    * All existing data on the target storage is deleted, and the data from the source storage is replicated to the target storage.
    * Replication might fail if the target storage size is smaller than the source storage size.

#### Disable Replication Settings

Disables storage replication.
When you disable replication, the target storage retains the source storage data as it was when the last replication completed.

> [Caution]
> When you re-establish replication, a new target storage is created. You cannot use the previously used target storage as the replication target storage again.

### Connect Storage

Storage can be mounted on instances by using the connection information of the created storage. However, the instance on which storage is mounted must be connected to the specified subnet.

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

##### Mount NAS Storage

```
sudo mount -t nfs {nas source} {mount point}
```

* nas source: NAS storage information 
Example: 192.168.0.241:/data
* mount point: Directory on which NAS storage is mounted 
Example: /mnt

#### CIFS

In File Explorer, right-click **My PC** in the left navigation pane and **Connect Network Drive**. In the Connect Network Drive window, select the drive you want to mount and enter the folder path.  The folder path is formatted as `\\{storage IP}\{storage name}`, for example: \\\\192.168.0.100\\cifs