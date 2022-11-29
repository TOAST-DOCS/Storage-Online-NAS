## Storage > NAS > Console User Guide

### Create Storage

New storage is created. The created storage can be accessed from instances by using the network file system (NFS) protocol.

| Item | Description | 
| -- | -- | 
| Name | Name of the storage to be created. The NFS access path can be created with the storage name. Storage name is limited to less than 100 alphabetic characters, numbers, and some symbols (‘_’). |
| Description | A description for storage |
| Size | Size of the storage to be created. It can be entered from a minimum of 300 GB to a maximum of 10,000 GB. | 
| VPC | The virtual private cloud (VPC) to access the storage. | 
| Subnet | The subnet to access the storage. Only subnets in the selected VPC can be chosen. | 
| Access Control List (ACL) | A list of the IPs or CIDR blocks that allow read and write permissions. | 
| Auto Create Snapshot | Snapshots are created automatically at a specified time once per day. When the maximum number of saves is reached, the first automatically created snapshot is deleted.  |
| Snapshot Storage  | If the sum of the snapshot capacity exceeds the set size, the first created snapshot among all snapshots is deleted. |

> [Note] The NAS service is only accessed from the specified subnet as of July 2022.

> [Note] The number of subnets available for each project is limited to 3 as of July 2022.



### Change Storage Size

NAS storage size is changed. Storage can be scaled up and down even while in use.

### Change Access Control List Settings

The settings for the storage’s access control list is changed.

### Change Snapshot Settings

The settings for Auto Create Snapshot and Snapshot Storage are changed.


### Delete Storage

NAS storage is deleted. 
It is recommended to delete the storage after unmounting the connected instance. 
When storage is deleted, all data including snapshots are deleted, and data cannot be recovered after deletion. 

### Create Snapshots

Snapshots of NAS storage are created immediately.

### Restore Storage

Storage is restored to the point in time at which the snapshot was taken. 
When restoring, snapshots created after the restore point are automatically deleted. 
To restore snapshots, you need to contact the customer center as of July 2022.

### Delete Snapshots

A specified snapshot is deleted.
Once deleted, snapshots cannot be recovered.


### Connect Storage
Storage can be mounted on instances by using the connection information of the created storage. However, the instance on which storage is mounted must be connected to the specified subnet.


#### Install NFS Package 

* Debian, Ubuntu: nfs-common, rpcbind  
  ```
  sudo apt-get install nfs-common rpcbind
  ```
* CentOS: nfs-utils, rpcbind  
  ```
  sudo yum install nfs-utils rpcbind
  ```

#### Run rpcbind Service 

```
sudo service rpcbind start
```

#### Mount NAS Storage

```
sudo mount -t nfs {nas source} {mount point}
```

* nas source: NAS storage information 
Example) 192.168.0.241:/data
* mount point: Directory on which NAS storage is mounted 
Example) /mnt

