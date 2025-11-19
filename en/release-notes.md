## Storage > NAS > Release Notes

### November 25, 2025

#### Feature Updates
* Changed the monitoring feature.
    * Added the monitoring feature for CIFS protocol volume.
    * Removed the monitoring for the number of client connections.

### May 27, 2025

#### Added Features
* Added Public APIs.
* Added the feature to search the storage name.

#### Improved
* Changed the constraint on ID in Manage CIFS Credentials.

#### Changed
* Chaneged the feature to replicate storage.
    * Replication is activated immediately upon changing the replication direction.
    * When stopping replication or changing its direction, the operation proceeds after replicating data up to the time the request is made.
    

### March 4, 2025

#### Added Features

* Improved the volume cloning feature.
    * You can clone CIFS protocol volumes.

### August 27, 2024

#### Added Features

* Added the feature to change the subnet association of a created volume.
* Improved the volume replication feature.
    * You can replicate a volume to another project within your organization.
    * You can replicate an encrypted volume.
* The usage at the time the snapshot is created appears in the snapshot list.


### May 28, 2024. 

#### Added Features
* Added the CIFS protocol.
    * You can also use NAS storage in the Windows environment.
* Added the snapshot restore feature.
* Added the feature to view snapshot restore history.
* Added the feature to replicate storage between regions.

### March 26, 2024

#### Added Features

* Added the feature to create encrypted storage.

### March 28, 2023

#### Added Features

* NAS monitoring feature has been added.
    * You can check various metrics of NAS storage with graphs.
    
### November 23, 2022

#### Added New Region

* NAS has been released for Korea (Pyeongchon) region.

### July 26, 2022

#### New Service Release

* NAS has been released for Korea (Pangyo) region
* NAS storage can be attached to instances via a project's VPC network.
