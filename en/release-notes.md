<a id="storage-nas-release-notes"></a>
## Storage > NAS > Release Notes { #storage-nas-release-notes }

<a id="august-25-2026"></a>

## August 25, 2026 { #august-25-2026 }

<a id="august-25-2026-added-features"></a>
### Added Features { #august-25-2026-added-features }

* Granular NAS resource access permissions
    * The Infrastructure NAS ADMIN role has been added to provide permissions for creating, modifying, and deleting NAS resources.
    * Creating, modifying, and deleting NAS resources is restricted for existing default infrastructure roles, except for the Infrastructure ADMIN role.

<a id="august-25-2026-feature-updates"></a>
### Feature Updates { #august-25-2026-feature-updates }

* Improved Volume Status display
    * When a volume status is Failed or Error, a detailed reason is now provided so that you can identify the cause.

<a id="may-27-2026"></a>
## May 27, 2026 { #may-27-2026 }

<a id="may-27-2026-added-features"></a>
### Added Features { #may-27-2026-added-features }

* Added snapshot usage information to volume usage status
    * Total snapshot usage
    * Snapshot usage within reserved capacity
    * Snapshot usage exceeding reserved capacity

<a id="november-25-2025"></a>
## November 25, 2025 { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
### Feature Updates { #november-25-2025-feature-updates }

* Feature Updates for monitoring
    * Added the feature to monitor CIFS protocol volumes.
    * Removed the client connection count monitoring.

<a id="may-27-2025"></a>
## May 27, 2025 { #may-27-2025 }

<a id="may-27-2025-added-features"></a>
### Added Features { #may-27-2025-added-features }

* Added Public APIs
* Added Volume name search

<a id="may-27-2025-feature-updates"></a>
### Feature Updates { #may-27-2025-feature-updates }

* Changed the CIFS credentials ID constraints
* Updated the volume replication feature
    * Replication is activated immediately upon changing the replication direction.
    * When stopping replication or changing the replication direction, the work proceeds after data up to the point of the request has been replicated.

<a id="march-4-2025"></a>
## March 4, 2025 { #march-4-2025 }

<a id="march-4-2025-added-features"></a>
### Added Features { #march-4-2025-added-features }

* Improved volume replication
    * You can replicate CIFS protocol volumes.

<a id="august-27-2024"></a>
## August 27, 2024 { #august-27-2024 }

<a id="august-27-2024-added-features"></a>
### Added Features { #august-27-2024-added-features }

* Added the feature to change the subnet association of a created volume.
* Improved volume replication
    * You can replicate volumes to other projects within your organization.
    * You can replicate an encrypted volume.
* Added display of usage at the time of creation in the snapshot list

<a id="may-28-2024"></a>
## May 28, 2024 { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
### Added Features { #may-28-2024-added-features }

* Added CIFS protocol support
    * You can also use volumes in the Windows environment.
* Added snapshot restore feature
* Added snapshot restore history view
* Added inter-region volume replication feature

<a id="march-26-2024"></a>
## March 26, 2024 { #march-26-2024 }

<a id="march-26-2024-added-features"></a>
### Added Features { #march-26-2024-added-features }

* Added the feature to create encrypted volumes

<a id="march-28-2023"></a>
## March 28, 2023 { #march-28-2023 }

<a id="march-28-2023-added-features"></a>
### Added Features { #march-28-2023-added-features }

* Added NAS monitoring feature
    * You can check various metrics for volumes along with graphs.

<a id="november-23-2022"></a>
## November 23, 2022 { #november-23-2022 }

<a id="november-23-2022-added-new-region"></a>
### Added New Region { #november-23-2022-added-new-region }

* Launched the NAS service in the Korea (Pyeongchon) region

<a id="july-26-2022"></a>
## July 26, 2022 { #july-26-2022 }

<a id="july-26-2022-new-service-release"></a>
### New Service Launch { #july-26-2022-new-service-release }

* NAS service launched in the Korea (Pangyo) region
* Support for connecting instance volumes through the project's VPC network
