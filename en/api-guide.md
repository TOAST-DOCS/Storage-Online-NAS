<!-- pre-align:aligned sig=06dac106ebf2 -->

<a id="storage-nas-api-guide"></a>
## Storage > NAS > API Guide { #storage-nas-api-guide }

<a id="nas_api_common"></a>
## NAS API Common Information { #nas_api_common }

<a id="nas_api_common.endpoint"></a>
### API Endpoint { #nas_api_common.endpoint }

NAS API uses the `nasv1` type endpoint. Refer to the `serviceCatalog` in the token issuance response for the valid endpoint.

| Region | Endpoint |
| --- | --- |
| Korea (Pangyo) Region | https://kr1-api-nas-infrastructure.nhncloudservice.com |
| Korea (Pyeongchon) Region | https://kr2-api-nas-infrastructure.nhncloudservice.com |
| Korea (Gwangju) Region | https://kr3-api-nas-infrastructure.nhncloudservice.com |


<a id="nas_api_common.authentication"></a>
### Authentication and Authorization { #nas_api_common.authentication }

NAS uses IaaS tokens for authentication and authorization when making API calls. The IaaS token is an authentication token used for NHN Cloud's OpenStack-based infrastructure services (IaaS). For more information on issuing and using IaaS tokens, please refer to the [IaaS Token](/nhncloud/en/public-api/iaas-token/).


<a id="nas_api_common.response"></a>
### Response Common Information { #nas_api_common.response }

This section describes the common response information provided by the NAS API. All API responses convey the result of a request with a `header` object.

| Name | Type | Description |
| --- | --- | --- |
| header | Object | Header Objects |
| header.isSuccessful | Boolean | Whether the request was successful (`true` or `false`) |
| header.resultCode | Integer | Result codes corresponding to HTTP status codes<br>- `200`: Success <br>- `201`: Resource creation successful<br>- `202`: Request received successfully, but not yet processed<br>- `400`: Requested with an invalid value<br>- `401`: Permission, authentication, or token-related error <br>- `404`: Requested resource not found<br>- `405`: The requested URL does not support the specified HTTP method<br>- `5XX`: The client's request is valid but the server failed to process it |
| header.resultMessage | String | Messages about the request processing results |

<details>
  <summary><strong>Success response</strong></summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 200,
    "resultMessage": "Success"
  }
}
```

</details>

<details>
  <summary><strong>Failure response</strong></summary>

```json
{
  "header": {
    "isSuccessful": false,
    "resultCode": 401,
    "resultMessage": "Authorization failed"
  }
}
```

</details>

<br>

> [Note]
> API response may show the fields not specified by the guide. These fields are internally used by NHN Cloud, and not used because they are subject to change without prior notice.

<a id="volume"></a>
## Volume { #volume }

<a id="volume.list"></a>
### List Volume { #volume.list }

Return the list of volumes.

```
GET  /v1/volumes
X-Auth-Token: {token-id}
```

<a id="volume.list-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| sizeGb | Query | String | - | Volume size |
| maxSizeGb | Query | String | - | Volume maximum size |
| minSizeGb | Query | String | - | Volume minimum size |
| name | Query | String | - | Volume name |
| nameContains | Query | String | - | Strings included in the volume name |
| subnetId | Query | String | - | Volume with interfaces on a subnet |
| limit | Query | String | - | Number of resources to expose on a page |
| page | Query | String | - | Page to search |
| sort | Query | String | - | Name of the field to sort by<br>Describe it in the form `{key}:{direction}`. Example: `name:asc`, `created_at:desc`<br>Possible key values: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

<a id="volume.list-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| paging | Body | Object | About the page |
| paging.limit | Body | Integer | Number of resources exposed on a page |
| paging.page | Body | Integer | Current page number |
| paging.totalCount | Body | Integer | Total Count |
| volumes | Body | List | List of Volume objects |
| volumes.id | Body | String | Volume ID |
| volumes.name | Body | String | Volume name |
| volumes.status | Body | String | Volume status |
| volumes.description | Body | String | Volume description |
| volumes.sizeGb | Body | Integer | Volume size (GB) |
| volumes.projectId | Body | String | The project ID to which the volume belongs |
| volumes.tenantId | Body | String | The tenant ID to which the volume belongs |
| volumes.acl | Body | List | Volume ACL List |
| volumes.encryption | Body | Object | Volume encryption information |
| volumes.encryption.enabled | Body | Boolean | Whether to enable volume encryption |
| volumes.encryption.keys | Body | List | Volume encryption keys information |
| volumes.interfaces | Body | List | List of volume interface objects |
| volumes.interfaces.id | Body | String | Interface ID |
| volumes.interfaces.path | Body | String | Interface path |
| volumes.interfaces.status | Body | String | Interface status |
| volumes.interfaces.subnetId | Body | String | The subnet ID of the interface |
| volumes.interfaces.tenantId | Body | String | The tenant ID of the interface |
| volumes.mirrors | Body | List | Volume replication settings object list |
| volumes.mirrors.id | Body | String | Replication setting ID |
| volumes.mirrors.role | Body | String | Replication roles<br>- `SOURCE`: Source volume<br>- `DESTINATION`: Target volume |
| volumes.mirrors.status | Body | String | Replication setting status<br>- `INITIALIZED`: Setup complete<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings |
| volumes.mirrors.direction | Body | String | Replication direction<br>- `FORWARD`: Source volume → Replica volume <br>- `REVERSE`: Replica volume → Source volume |
| volumes.mirrors.directionChangedAt | Body | String | When to change replication direction |
| volumes.mirrors.dstProjectId | Body | String | The project ID of the replication target volume |
| volumes.mirrors.dstRegion | Body | String | The region of the replication target volume |
| volumes.mirrors.dstTenantId | Body | String | The tenant ID of the replication target volume |
| volumes.mirrors.dstVolumeId | Body | String | The volume ID of the replication target volume |
| volumes.mirrors.dstVolumeName | Body | String | The volume name of the replication target volume |
| volumes.mirrors.srcProjectId | Body | String | The project ID of the source volume |
| volumes.mirrors.srcRegion | Body | String | The region of the source volume |
| volumes.mirrors.srcTenantId | Body | String | The tenant ID of the source volume |
| volumes.mirrors.srcVolumeId | Body | String | The volume ID of the source volume |
| volumes.mirrors.srcVolumeName | Body | String | Source volume name |
| volumes.mirrors.createdAt | Body | String | Replication creation time |
| volumes.mountProtocol | Body | Object | Volume mount protocols |
| volumes.mountProtocol.cifsAuthIds | Body | List | Volume CIFS Authentication ID List |
| volumes.mountProtocol.protocol | Body | String | Volume mount protocols |
| volumes.snapshotPolicy | Body | Object | Volume snapshot settings object |
| volumes.snapshotPolicy.maxScheduledCount | Body | Integer | The maximum number of snapshots that can be saved |
| volumes.snapshotPolicy.reservePercent | Body | Integer | Snapshot capacity ratio |
| volumes.snapshotPolicy.schedule | Body | Object | Snapshot auto-create objects |
| volumes.snapshotPolicy.schedule.time | Body | String | Snapshot auto-create time |
| volumes.snapshotPolicy.schedule.timeOffset | Body | String | Time zone for snapshot auto-create |
| volumes.snapshotPolicy.schedule.weekdays | Body | List | Days of the week that snapshots are automatically created<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |
| volumes.createdAt | Body | String | Volume created time |
| volumes.updatedAt | Body | String | Volume changed time |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 200,
    "resultMessage": "Success"
  },
  "paging": {
    "limit": 50,
    "page": 1,
    "totalCount": 1
  },
  "volumes": [
    {
      "acl": [
        "10.0.1.0/24"
      ],
      "createdAt": "2025-04-01T06:44:25+00:00",
      "description": "NAS for Testing",
      "encryption": {
        "enabled": false
      },
      "id": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
      "interfaces": [
        {
          "id": "9a8ec90f-cc27-4649-9bda-a1f0b193a402",
          "path": "10.0.1.7:/TEST-NAS-1",
          "status": "ACTIVE",
          "subnetId": "cb779d62-72ef-43b6-b368-3fe28dcd812b",
          "tenantId": "3b6179e5fa6b499386b827357c4cb8c4"
        }
      ],
      "mirrors": [
        {
          "createdAt":"2025-04-01T06:45:45+00:00",
          "direction": "FORWARD",
          "directionChangedAt": null,
          "dstProjectId": "K3y0CgOy",
          "dstRegion": "KR2",
          "dstTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
          "dstVolumeId": "e09281d2-0b1c-48a9-8a01-0098aa59f624",
          "dstVolumeName": "TEST-NAS-MIRROR-1",
          "id": "8116892c-7306-48be-9e3d-143311b2254c",
          "role": "SOURCE",
          "srcProjectId": "K3y0CgOy",
          "srcRegion": "KR1",
          "srcTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
          "srcVolumeId": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
          "srcVolumeName": "TEST-NAS-1",
          "status": "PENDING"
        }
      ],
      "mountProtocol": {
        "protocol": "cifs",
        "cifsAuthIds": [
          "cifs-test-id"
        ]
      },
      "name": "TEST-NAS-1",
      "projectId": "K3y0CgOy",
      "sizeGb": 300,
      "snapshotPolicy": {
        "maxScheduledCount": 1,
        "reservePercent": 5,
        "schedule": {
          "time": "00:00",
          "timeOffset": "+09:00",
          "weekdays": [
            1,
            3,
            5
          ]
        }
      },
      "stationId": null,
      "status": "ACTIVE",
      "tenantId": "3b6179e5fa6b499386b827357c4cb8c4",
      "updatedAt": "2025-04-01T06:47:13+00:00"
    }
  ]
}
```

</details>

<br>

<a id="volume.create"></a>
### Create Volume { #volume.create }

Create a new volume.

> [Note] Using the CIFS protocol
> To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register CIFS credentials to allow to access each CIFS volume.
> You can create CIFS credentials through the **Storage > NAS > Manage CIFS Credentials** of the console.

<!-- -->

> [Note] Setting up encryption key storage
> When an encrypted volume is created, the symmetric key used for encryption is stored in the NHN Cloud Secure Key Manager store. To create encrypted volume,[you must first create a keystore](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_1) in the Secure Key Manager service. After creating the keystore, [check its ID](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_2) and enter it in the encryption keystore settings.
> You can enter the keystore ID from the **Storage > NAS > Encryption keystore settings** in the console. When you create encrypted volume, the symmetric key is stored in the specified keystore. The symmetric key stored in the keystore cannot be deleted while the encrypted volume is in use. When the encrypted volume is deleted, the corresponding symmetric key is also deleted.
> If you change the keystore ID, symmetric keys for newly created encrypted volume will be stored in the new keystore. Symmetric keys already stored in the previous keystore are retained.


```
POST  /v1/volumes
X-Auth-Token: {token-id}
```

<br>

<a id="volume.create-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume | Body | Object | O | Volume creation request object |
| volume.acl | Body | List | - | List of ACLs to set when creating volume<br>You can enter it in IP or CIDR format. |
| volume.description | Body | String | - | Volume description |
| volume.encryption | Body | Object | - | Encryption settings object when creating volume |
| volume.encryption.enabled | Body | Boolean | - | Whether to enable encryption settings<br>After the encryption keystore is set up, setting its field to `true`enables encryption. |
| volume.interfaces | Body | List | - | List of interfaces to access volume |
| volume.interfaces.subnetId | Body | String | - | The subnet ID of the volume interface |
| volume.mountProtocol | Body | Object | - | Protocol settings object when creating volume |
| volume.mountProtocol.cifsAuthIds | Body | List | - | List of CIFS Authentication IDs<br>No input required for NFS protocol selection |
| volume.mountProtocol.protocol | Body | String | O | Specifying protocols when mounting volume<br>You can choose between `NFS` and `CIFS`. |
| volume.name | Body | String | O | Volume name |
| volume.sizeGb | Body | Integer | O | Volume size (GB)<br>Volume can be set from a minimum of 300GB to a maximum of 10,000GB, in 100GB increments. |
| volume.snapshotPolicy | Body | Object | - | Volume snapshot settings object |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | The maximum number of snapshots that can be saved<br>You can set a maximum of 30, and the first automatically created snapshot will be deleted when the maximum number of saves is reached. |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | Snapshot capacity ratio |
| volume.snapshotPolicy.schedule | Body | Object | - | Snapshot auto-create objects<br>If `null`, snapshot auto-creation will not be configured. |
| volume.snapshotPolicy.schedule.time | Body | String | - | Snapshot auto-create time |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | Time zone for snapshot auto-create |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | Days of the week that snapshots are automatically created<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |

<details>
  <summary>Request Example</summary>

```json
{
  "volume": {
    "acl": [
      "10.0.1.0/24"
    ],
    "description": "NAS for Testing",
    "encryption": {
      "enabled": true
    },
    "interfaces": [
      {
        "subnetId": "cb779d62-72ef-43b6-b368-3fe28dcd812b"
      }
    ],
    "mountProtocol": {
      "protocol": "nfs"
    },
    "name": "TEST-NAS-1",
    "sizeGb": 300,
    "snapshotPolicy": {
      "maxScheduledCount": 20,
      "reservePercent": 10,
      "schedule": {
        "time": "03:00",
        "timeOffset": "+09:00",
        "weekdays": [
          1,
          3,
          5
        ]
      }
    }
  }
}
```

</details>

<a id="volume.create-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| volume | Body | Object | Volume objects |
| volume.id | Body | String | Volume ID |
| volume.name | Body | String | Volume name |
| volume.status | Body | String | Volume status |
| volume.description | Body | String | Volume description |
| volume.sizeGb | Body | Integer | Volume size (GB) |
| volume.projectId | Body | String | The project ID to which the volume belongs |
| volume.tenantId | Body | String | The tenant ID to which the volume belongs |
| volume.acl | Body | List | Volume ACL List |
| volume.encryption | Body | Object | Volume encryption information |
| volume.encryption.enabled | Body | Boolean | Whether to enable volume encryption |
| volume.encryption.keys | Body | List | Volume encryption keys information |
| volume.interfaces | Body | List | List of volume interface objects |
| volume.interfaces.id | Body | String | Interface ID |
| volume.interfaces.path | Body | String | Interface path |
| volume.interfaces.status | Body | String | Interface status |
| volume.interfaces.subnetId | Body | String | The subnet ID of the interface |
| volume.interfaces.tenantId | Body | String | The tenant ID of the interface |
| volume.mirrors | Body | List | Volume replication settings object list |
| volume.mirrors.id | Body | String | Replication setting ID |
| volume.mirrors.role | Body | String | Replication roles<br>- `SOURCE`: Source volume<br>- `DESTINATION`: Target volume |
| volume.mirrors.status | Body | String | Replication setting status<br>- `INITIALIZED`: Setup complete<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings |
| volume.mirrors.direction | Body | String | Replication direction<br>- `FORWARD`: Original volume → Replica volume<br>- `REVERSE`: Replica volume → Original volume |
| volume.mirrors.directionChangedAt | Body | String | When to change replication direction |
| volume.mirrors.dstProjectId | Body | String | The project ID of the replication target volume |
| volume.mirrors.dstRegion | Body | String | The region of the replication target volume |
| volume.mirrors.dstTenantId | Body | String | The tenant ID of the replication target volume |
| volume.mirrors.dstVolumeId | Body | String | The volume ID of the replication target volume |
| volume.mirrors.dstVolumeName | Body | String | The volume name of the replication target volume |
| volume.mirrors.srcProjectId | Body | String | The project ID of the source volume |
| volume.mirrors.srcRegion | Body | String | The region of the source volume |
| volume.mirrors.srcTenantId | Body | String | The tenant ID of the source volume |
| volume.mirrors.srcVolumeId | Body | String | The volume ID of the source volume |
| volume.mirrors.srcVolumeName | Body | String | Source volume name |
| volume.mirrors.createdAt | Body | String | Replication creation time |
| volume.mountProtocol | Body | Object | Volume mount protocols |
| volume.mountProtocol.cifsAuthIds | Body | List | Volume CIFS Authentication ID List |
| volume.mountProtocol.protocol | Body | String | Volume mount protocols |
| volume.snapshotPolicy | Body | Object | Volume snapshot settings object |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | The maximum number of snapshots that can be saved |
| volume.snapshotPolicy.reservePercent | Body | Integer | Snapshot capacity ratio |
| volume.snapshotPolicy.schedule | Body | Object | Snapshot auto-create objects |
| volume.snapshotPolicy.schedule.time | Body | String | Snapshot auto-create time |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | Time zone for snapshot auto-create |
| volume.snapshotPolicy.schedule.weekdays | Body | List | Days of the week that snapshots are automatically created<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |
| volume.createdAt | Body | String | Volume created time |
| volume.updatedAt | Body | String | Volume changed time |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 201,
    "resultMessage": "Created"
  },
  "volume": {
    "acl": [
      "10.0.1.0/24"
    ],
    "createdAt": "2025-04-01T06:44:25+00:00",
    "description": "NAS for Testing",
    "encryption": {
      "enabled": false
    },
    "id": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
    "interfaces": [
      {
        "id": "9a8ec90f-cc27-4649-9bda-a1f0b193a402",
        "path": "10.0.1.7:/TEST-NAS-1",
        "status": "ACTIVE",
        "subnetId": "cb779d62-72ef-43b6-b368-3fe28dcd812b",
        "tenantId": "3b6179e5fa6b499386b827357c4cb8c4"
      }
    ],
    "mirrors": [
      {
        "createdAt":"2025-04-01T06:45:45+00:00",
        "direction": "FORWARD",
        "directionChangedAt": null,
        "dstProjectId": "K3y0CgOy",
        "dstRegion": "KR2",
        "dstTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
        "dstVolumeId": "e09281d2-0b1c-48a9-8a01-0098aa59f624",
        "dstVolumeName": "TEST-NAS-MIRROR-1",
        "id": "8116892c-7306-48be-9e3d-143311b2254c",
        "role": "SOURCE",
        "srcProjectId": "K3y0CgOy",
        "srcRegion": "KR1",
        "srcTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
        "srcVolumeId": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
        "srcVolumeName": "TEST-NAS-1",
        "status": "PENDING"
      }
    ],
    "mountProtocol": {
      "protocol": "cifs",
      "cifsAuthIds": [
        "cifs-test-id"
      ]
    },
    "name": "TEST-NAS-1",
    "projectId": "K3y0CgOy",
    "sizeGb": 300,
    "snapshotPolicy": {
      "maxScheduledCount": 1,
      "reservePercent": 5,
      "schedule": {
        "time": "00:00",
        "timeOffset": "+09:00",
        "weekdays": [
          1,
          3,
          5
        ]
      }
    },
    "stationId": null,
    "status": "ACTIVE",
    "tenantId": "3b6179e5fa6b499386b827357c4cb8c4",
    "updatedAt": "2025-04-01T06:47:13+00:00"
  }
}
```

</details>

<br>

<a id="volume.delete"></a>
### Delete Volume { #volume.delete }

Deletes the specified volume.

```
DELETE  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID to delete |

<a id="volume.delete-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="volume.view"></a>
### View Volume { #volume.view }

Returns details about the specified volume.

```
GET   /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.view-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID to query |

<a id="volume.view-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| volume | Body | Object | Volume objects |
| volume.id | Body | String | Volume ID |
| volume.name | Body | String | Volume name |
| volume.status | Body | String | Volume status |
| volume.description | Body | String | Volume description |
| volume.sizeGb | Body | Integer | Volume size (GB) |
| volume.projectId | Body | String | The project ID to which the volume belongs |
| volume.tenantId | Body | String | The tenant ID to which the volume belongs |
| volume.acl | Body | List | Volume ACL List |
| volume.encryption | Body | Object | Volume encryption information |
| volume.encryption.enabled | Body | Boolean | Whether to enable volume encryption |
| volume.encryption.keys | Body | List | Volume encryption keys information |
| volume.interfaces | Body | List | List of volume interface objects |
| volume.interfaces.id | Body | String | Interface ID |
| volume.interfaces.path | Body | String | Interface path |
| volume.interfaces.status | Body | String | Interface status |
| volume.interfaces.subnetId | Body | String | The subnet ID of the interface |
| volume.interfaces.tenantId | Body | String | The tenant ID of the interface |
| volume.mirrors | Body | List | Volume replication settings object list |
| volume.mirrors.id | Body | String | Replication setting ID |
| volume.mirrors.role | Body | String | Replication roles<br>- `SOURCE`: Source volume<br>- `DESTINATION`: Target volume |
| volume.mirrors.status | Body | String | Replication setting status<br>- `INITIALIZED`: Setup complete<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings |
| volume.mirrors.direction | Body | String | Replication direction<br>- `FORWARD`: Original volume → Replica volume<br>- `REVERSE`: Replica volume → Original volume |
| volume.mirrors.directionChangedAt | Body | String | When to change replication direction |
| volume.mirrors.dstProjectId | Body | String | The project ID of the replication target volume |
| volume.mirrors.dstRegion | Body | String | The region of the replication target volume |
| volume.mirrors.dstTenantId | Body | String | The tenant ID of the replication target volume |
| volume.mirrors.dstVolumeId | Body | String | The volume ID of the replication target volume |
| volume.mirrors.dstVolumeName | Body | String | The volume name of the replication target volume |
| volume.mirrors.srcProjectId | Body | String | The project ID of the source volume |
| volume.mirrors.srcRegion | Body | String | The region of the source volume |
| volume.mirrors.srcTenantId | Body | String | The tenant ID of the source volume |
| volume.mirrors.srcVolumeId | Body | String | The volume ID of the source volume |
| volume.mirrors.srcVolumeName | Body | String | Source volume name |
| volume.mirrors.createdAt | Body | String | Replication creation time |
| volume.mountProtocol | Body | Object | Volume mount protocols |
| volume.mountProtocol.cifsAuthIds | Body | List | Volume CIFS Authentication ID List |
| volume.mountProtocol.protocol | Body | String | Volume mount protocols |
| volume.snapshotPolicy | Body | Object | Volume snapshot settings object |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | The maximum number of snapshots that can be saved |
| volume.snapshotPolicy.reservePercent | Body | Integer | Snapshot capacity ratio |
| volume.snapshotPolicy.schedule | Body | Object | Snapshot auto-create objects |
| volume.snapshotPolicy.schedule.time | Body | String | Snapshot auto-create time |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | Time zone for snapshot auto-create |
| volume.snapshotPolicy.schedule.weekdays | Body | List | Days of the week that snapshots are automatically created<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |
| volume.createdAt | Body | String | Volume created time |
| volume.updatedAt | Body | String | Volume changed time |

<br>

<a id="volume.change_settings"></a>
### Change Volume Settings { #volume.change_settings }

Change the settings for the specified volume.

> [Caution]
To change the size of a replicated volume, you must change both the source volume and the target volume. If the size of the source volume and the target volume are different, replication might fail.

```
PATCH  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.change_settings-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume | Body | Object | O | Request object for changing volume settings |
| volume.acl | Body | List | - | List of ACLs to set when creating volume<br>You can enter it in IP or CIDR format. |
| volume.description | Body | String | - | Volume description |
| volume.mountProtocol | Body | Object | - | Protocol settings object when creating volume |
| volume.mountProtocol.cifsAuthIds | Body | List | - | List of CIFS Authentication IDs |
| volume.mountProtocol.protocol | Body | String | - | You cannot change the protocol of volume that has already been created.<br>When changing the `cifsAuthIds` field, you must specify the `cifs` in that field. |
| volume.sizeGb | Body | Integer | - | Volume size (GB)<br>Volume can be set from a minimum of 300 GB to a maximum of 10,000GB, in 100GB increments. |
| volume.snapshotPolicy | Body | Object | - | Volume snapshot settings object |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | The maximum number of snapshots that can be saved<br>You can set a maximum of 30, and the first automatically created snapshot will be deleted when the maximum number of saves is reached. |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | Snapshot capacity ratio |
| volume.snapshotPolicy.schedule | Body | Object | - | Snapshot auto-create objects<br>If `null`, snapshot auto-creation will not be configured. |
| volume.snapshotPolicy.schedule.time | Body | String | - | Snapshot auto-create time |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | Time zone for snapshot auto-create |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | Days of the week that snapshots are automatically created<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |

<details>
  <summary>Request Example</summary>

```json
{
  "volume": {
    "acl": [
      "10.0.1.0/24"
    ],
    "description": "Modified description",
    "mountProtocol": {
      "cifsAuthIds": [
        "cifs-test-id"
      ],
      "protocol": "cifs"
    },
    "sizeGb": 300,
    "snapshotPolicy": {
      "maxScheduledCount": 10,
      "reservePercent": 20,
      "schedule": {
        "time": "05:00",
        "timeOffset": "+09:00",
        "weekdays": [
          1,
          3,
          5
        ]
      }
    }
  }
}
```

</details>

<a id="volume.change_settings-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="volume.connect_interface"></a>
### Connect an interface to volume { #volume.connect_interface }

Sets the interface for the specified volume.
The volume is accessible from the set address and subnet. The accessible IP setting must be set separately in the access control (ACL) settings.

```
POST  /v1/volumes/{volume_id}/interfaces
X-Auth-Token: {token-id}
```

<a id="volume.connect_interface-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| interface | Body | Object | O | Interface Settings Object |
| interface.subnetId | Body | String | O | Specify an interface subnet |

<details>
  <summary>Request Example</summary>

```json
{
  "interface":{
    "subnetId":"3e5b4d63-d143-420a-9263-208a447a2a3f"
  }
}
```

</details>

<a id="volume.connect_interface-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| interface | Body | Object | Created interface information object |
| interface.id | Body | String | Created interface ID |
| interface.path | Body | String | Created interface path |
| interface.status | Body | String | Created interface status |
| interface.subnetId | Body | String | The subnet ID of the created interface |
| interface.tenantId | Body | String | The tenant ID of the created interface |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 201,
    "resultMessage": "Created"
  },
  "interface": {
    "id": "e7c6a340-6889-445b-ae2f-4e237b9afc9e",
    "path": null,
    "status": "BUILDING",
    "subnetId": "3e5b4d63-d143-420a-9263-208a447a2a3f",
    "tenantId": "3b6179e5fa6b499386b827357c4cb8c4"
  }
}
```

</details>

<br>

<a id="volume.delete_interface"></a>
### Delete an interface on volume { #volume.delete_interface }

Deletes the specified interface of the specified volume.

```
DELETE  /v1/volumes/{volume_id}/interfaces/{interface_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete_interface-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| interface_id | URL | String | O | Interface ID to delete |

<a id="volume.delete_interface-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="volume.view_snapshot_restore_history"></a>
### View snapshot restore history { #volume.view_snapshot_restore_history }

Returns a list of snapshot restore history for the specified volume.

```
GET  /v1/volumes/{volume_id}/restore-histories
X-Auth-Token: {token-id}
```

<a id="volume.view_snapshot_restore_history-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- |---| --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| limit | Query | String | - | Number of resources to expose on a page |
| page | Query | String | - | Page to search |
| sort | Query | String | - | Name of the field to sort by<br>Describe it in the form `{key}:{direction}`. Example: `snapshotId:asc`, `requestedAt:desc`<br>Possible key values: `snapshotId`, `snapshotName`, `requestedAt`, `restoredAt`, `requestedUser`, `requestedIp`, `result` |

<a id="volume.view_snapshot_restore_history-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| paging | Body | Object | About the page |
| paging.limit | Body | Integer | Number of resources exposed on a page |
| paging.page | Body | Integer | Current page number |
| paging.totalCount | Body | Integer | Total Count |
| restoreHistories | Body | List | Snapshot restore history object list |
| restoreHistories.requestedAt | Body | String | When the snapshot restore was requested |
| restoreHistories.requestedIp | Body | String | The address from which the snapshot restore was requested |
| restoreHistories.requestedUser | Body | String | The user ID that requested the snapshot restore |
| restoreHistories.restoredAt | Body | String | When the snapshot restore completed |
| restoreHistories.result | Body | String | Snapshot restore results |
| restoreHistories.snapshotId | Body | String | Snapshot ID to restore |
| restoreHistories.snapshotName | Body | String | Snapshot name to restore |
| restoreHistories.volumeId | Body | String | ID of the restored volume |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 200,
    "resultMessage": "Success"
  },
  "paging": {
    "limit": 50,
    "page": 1,
    "totalCount": 1
  },
  "restoreHistories": [
    {
      "requestedAt": "2025-04-01T08:29:28+00:00",
      "requestedIp": "10.163.23.45",
      "requestedUser": "14025c4b-cc93-4f97-9416-a8001cc771c1",
      "restoredAt": "2025-04-01T08:29:34+00:00",
      "result": "SUCCESS",
      "snapshotId": "5e9745a5-0ed3-11f0-b0e3-d039eaa3e920",
      "snapshotName": "TEST-SNAPSHOT-IMM-1",
      "volumeId": "70787a7e-605b-4447-b950-46aa3297e0ed"
    }
  ]
}
```

</details>

<br>

<a id="volume.view_usage"></a>
### View volume usage { #volume.view_usage }

Returns the usage status of the specified volume.

```
GET  /v1/volumes/{volume_id}/usage
X-Auth-Token: {token-id}
```

<a id="volume.view_usage-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |

<a id="volume.view_usage-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header objects |
| usage | Body | Object | Volume usage object |
| usage.snapshotReserveGb | Body | Integer | The amount of space reserved for snapshots on the volume |
| usage.snapshotUsedGb | Body | Integer | Snapshot usage |
| usage.snapshotUsedGbInReservedSpace | Body | Integer | Snapshot usage within the reserved capacity |
| usage.snapshotUsedGbInUserSpace | Body | Integer | Snapshot usage exceeding the reserved capacity |
| usage.usedGb | Body | Integer | Volume usage |
| usage.userDataGb | Body | Integer | The size of data actually written by the user |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 200,
    "resultMessage": "Success"
  },
  "usage": {
    "snapshotReserveGb": 20,
    "snapshotUsedGb": 11,
    "snapshotUsedGbInReservedSpace": 11,
    "snapshotUsedGbInUserSpace": 0,
    "usedGb": 152,
    "userDataGb": 152
  }
}
```

</details>

<br>

<a id="snapshots"></a>
## Snapshots { #snapshots }

<a id="snapshots.list"></a>
### List Snapshots { #snapshots.list }

View a list of snapshots.

```
GET  /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.list-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |

<a id="snapshots.list-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| snapshots | Body | List | Snapshot info object list |
| snapshots.id | Body | String | Snapshot ID |
| snapshots.name | Body | String | Snapshot name |
| snapshots.size | Body | Integer | Snapshot size |
| snapshots.type | Body | String | Snapshot types<br>- `NORMAL`: Snapshots created by the user<br>- `SCHEDULED`: Snapshots created by Auto Create Snapshot<br>- `MIRROR`: Snapshots created by replication |
| snapshots.preserved | Body | Boolean | Whether snapshots are made undeletable by the system |
| snapshots.createdAt | Body | String | Snapshot creation time |

<details><summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 200,
    "resultMessage": "Success"
  },
  "snapshots": [
    {
      "createdAt": "2025-04-01T09:34:27+00:00",
      "id": "8151fe33-0edc-11f0-b0e3-d039eaa3e920",
      "name": "TEST-SNAPSHOT-1",
      "preserved": false,
      "size": 3112960,
      "type": "NORMAL"
    }
  ]
}
```
</details>

<br>

<a id="snapshots.create"></a>
### Create Snapshots { #snapshots.create }

Creates a snapshot of the specified volume.

```
POST  /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.create-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| snapshot | Body | Object | O | Snapshot creation objects |
| snapshot.name | Body | String | O | Snapshot name |

<details>
  <summary>Request Example</summary>

```json
{
  "snapshot": {
    "name": "TEST-SNAPSHOT-1"
  }
}
```

</details>

<a id="snapshots.create-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| snapshot | Body | Object | Snapshot information objects |
| snapshot.id | Body | String | Snapshot ID |
| snapshot.name | Body | String | Snapshot name |
| snapshot.size | Body | Integer | Snapshot size |
| snapshot.type | Body | String | Snapshot types<br>- `NORMAL`: Snapshots created by the user<br>- `SCHEDULED`: Snapshots created by Auto Create Snapshot<br>- `MIRROR`: Snapshots created by replication |
| snapshot.preserved | Body | Boolean | Whether snapshots are made undeletable by the system |
| snapshot.createdAt | Body | String | Snapshot creation time |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 201,
    "resultMessage": "Created"
  },
  "snapshot": {
    "createdAt": "2025-04-01T09:34:27+00:00",
    "id": "8151fe33-0edc-11f0-b0e3-d039eaa3e920",
    "name": "TEST-SNAPSHOT-1",
    "preserved": false,
    "size": 3112960,
    "type": "NORMAL"
  }
}
```

</details>

<br>

<a id="snapshots.delete"></a>
### Delete Snapshots { #snapshots.delete }

Deletes a snapshot of the specified volume.

```
DELETE  /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.delete-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| snapshot_id | URL | String | O | Snapshot ID |

<a id="snapshots.delete-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="snapshots.view"></a>
### View Snapshot { #snapshots.view }

Returns details of the specified snapshot.

```
GET  /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.view-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| snapshot_id | URL | String | O | Snapshot ID |
| showReclaimableSpace | Query | Boolean | - | Whether to expose `the reclaimableSpace` entry, which indicates the amount of space reclaimed when a snapshot is deleted. |

<a id="snapshots.view-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| snapshot | Body | Object | Snapshot information objects |
| snapshot.id | Body | String | Snapshot ID |
| snapshot.name | Body | String | Snapshot name |
| snapshot.size | Body | Integer | Snapshot size |
| snapshot.type | Body | String | Snapshot types<br>- `NORMAL`: Snapshots created by the user<br>- `SCHEDULED`: Snapshots created by Auto Create Snapshot<br>- `MIRROR`: Snapshots created by replication |
| snapshot.preserved | Body | Boolean | Whether snapshots are made undeletable by the system |
| snapshot.createdAt | Body | String | Snapshot creation time |

<br>

<a id="snapshots.restore"></a>
### Restore Snapshot { #snapshots.restore }

Restores volume to the specified snapshot.

```
POST  /v1/volumes/{volume_id}/snapshots/{snapshot_id}/restore
X-Auth-Token: {token-id}
```

<a id="snapshots.restore-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| snapshot_id | URL | String | O | Snapshot ID |

<a id="snapshots.restore-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication"></a>
## Set up volume replication { #replication }

<a id="replication.setup"></a>
### Set up replication { #replication.setup }

Set up replication of the specified volume.
The selectable region ranges for each replication target project can be found in the table below.

| Target project | Selectable region |
| ------- | --------- |
| Same project within an organization | Other regions |
| Other projects in an organization | All regions |

<br>

> [Caution]
The size of the target volume for replication must be the same as the source volume.
If the sizes of the source and target volumes differ, the replication may fail.

<!-- -->

> [Note]
> To set up encryption on the target volume, you must configure a separate encryption keystore specific to the project or region the target volume belongs to.

<!-- -->

> [Note] If the source volume uses the CIFS protocol, the target volume must also use CIFS. To do this, you must create separate CIFS credentials (different from the source) and specify it in the`cifsAuthIds` field of the request body.


```
POST  /v1/volumes/{volume_id}/volume-mirrors
X-Auth-Token: {token-id}
```

<a id="replication.setup-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Source volume ID |
| volumeMirror | Body | Object | O | Volume replication settings request object  |
| volumeMirror.dstRegion | Body | String | O | The region of replication target volume |
| volumeMirror.dstTenantId | Body | String | O | The tenant ID of the replication target volume |
| volumeMirror.dstVolume | Body | Object | O | Replication target volume request object |
| volumeMirror.dstVolume.acl | Body | List | - | List of ACLs to set when creating volume<br>You can enter it in IP or CIDR format. |
| volumeMirror.dstVolume.description | Body | String | - | Volume description |
| volumeMirror.dstVolume.encryption | Body | Object | - | Encryption settings object when creating volume |
| volumeMirror.dstVolume.encryption.enabled | Body | Boolean | - | Whether to enable encryption settings<br>After the encryption keystore is set up, setting its field to `true`enables encryption. |
| volumeMirror.dstVolume.interfaces | Body | List | - | List of interfaces to access volume |
| volumeMirror.dstVolume.interfaces.subnetId | Body | String | - | The subnet ID of the volume interface |
| volumeMirror.dstVolume.mountProtocol | Body | Object | - | Protocol settings object when creating volume |
| volumeMirror.dstVolume.mountProtocol.cifsAuthIds | Body | List | - | List of CIFS Authentication IDs<br>No input required for NFS protocol selection |
| volumeMirror.dstVolume.mountProtocol.protocol | Body | String | O | Specifying protocols when mounting volume<br>You can choose between `NFS` and `CIFS`. |
| volumeMirror.dstVolume.name | Body | String | O | Volume name |
| volumeMirror.dstVolume.sizeGb | Body | Integer | O | Volume size (GB)<br>Volume can be set from a minimum of 300GB to a maximum of 10,000GB, in 100GB increments. |
| volumeMirror.dstVolume.snapshotPolicy | Body | Object | - | Volume snapshot settings object |
| volumeMirror.dstVolume.snapshotPolicy.maxScheduledCount | Body | Integer | - | The maximum number of snapshots that can be saved<br>You can set a maximum of 30, and the first automatically created snapshot will be deleted when the maximum number of saves is reached. |
| volumeMirror.dstVolume.snapshotPolicy.reservePercent | Body | Integer | - | Snapshot capacity ratio |
| volumeMirror.dstVolume.snapshotPolicy.schedule | Body | Object | - | Snapshot auto-create objects<br>If `null`, snapshot auto-creation will not be configured. |
| volumeMirror.dstVolume.snapshotPolicy.schedule.time | Body | String | - | Snapshot auto-create time |
| volumeMirror.dstVolume.snapshotPolicy.schedule.timeOffset | Body | String | - | Time zone for snapshot auto-create |
| volumeMirror.dstVolume.snapshotPolicy.schedule.weekdays | Body | List | - | Days of the week that snapshots are automatically created<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |

<details>
  <summary>Request Example</summary>

```json
{
  "volumeMirror": {
    "dstRegion": "KR1",
    "dstTenantId": "7debf04e6a7248c98777229bcb004b69",
    "dstVolume": {
      "description": "Volume Mirror Test",
      "mountProtocol": {
        "protocol": "nfs"
      },
      "name": "TEST-NAS-MIRROR",
      "sizeGb": 300
    }
  }
}
```

</details>

<a id="replication.setup-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| volumeMirror | Body | Object | Replication Settings Creation Object |
| volumeMirror.id | Body | String | Replication setting ID |
| volumeMirror.role | Body | String | Replication roles<br>- `SOURCE`: Source volume<br>- `DESTINATION`: Target volume |
| volumeMirror.status | Body | String | Replication setting status<br>- `INITIALIZED`: Setup complete<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings |
| volumeMirror.direction | Body | String | Replication direction<br>- `FORWARD`: Source volume → Replica volume<br>- `REVERSE`: Replica volume → Source volume |
| volumeMirror.directionChangedAt | Body | String | When to change replication direction |
| volumeMirror.dstProjectId | Body | String | The project ID of the replication target volume |
| volumeMirror.dstRegion | Body | String | The region of the replication target volume |
| volumeMirror.dstTenantId | Body | String | The tenant ID of the replication target volume |
| volumeMirror.dstVolumeId | Body | String | The volume ID of the replication target volume |
| volumeMirror.dstVolumeName | Body | String | The volume name of the replication target volume |
| volumeMirror.srcProjectId | Body | String | The project ID of the source volume |
| volumeMirror.srcRegion | Body | String | The region of the source volume |
| volumeMirror.srcTenantId | Body | String | The tenant ID of the source volume |
| volumeMirror.srcVolumeId | Body | String | The volume ID of the source volume |
| volumeMirror.srcVolumeName | Body | String | Source volume name |
| volumeMirror.createdAt | Body | String | Replication creation time |

<details>
  <summary>Example response</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 201,
    "resultMessage": "Created"
  },
  "volumeMirror": {
    "createdAt":"2025-04-01T06:45:45+00:00",
    "direction": "FORWARD",
    "directionChangedAt": null,
    "dstProjectId": "K3y0CgOy",
    "dstRegion": "KR2",
    "dstTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
    "dstVolumeId": "e09281d2-0b1c-48a9-8a01-0098aa59f624",
    "dstVolumeName": "TEST-NAS-MIRROR-1",
    "id": "8116892c-7306-48be-9e3d-143311b2254c",
    "role": "SOURCE",
    "srcProjectId": "K3y0CgOy",
    "srcRegion": "KR1",
    "srcTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
    "srcVolumeId": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
    "srcVolumeName": "TEST-NAS-1",
    "status": "PENDING"
  }
}
```

</details>

<br>

<a id="replication.disable"></a>
### Disable Replication Settings { #replication.disable }

Disable replication settings for the specified volume.

```
DELETE  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}
X-Auth-Token: {token-id}
```

<a id="replication.disable-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume_mirror_id | URL | String | O | Replication setting ID |

<a id="replication.disable-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication.change_direction"></a>
### Change the replication direction { #replication.change_direction }

Change the direction of replication between source and target volume.

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/invert-direction
X-Auth-Token: {token-id}
```

<a id="replication.change_direction-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume_mirror_id | URL | String | O | Replication setting ID |

<a id="replication.change_direction-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication.start"></a>
### Start Replication { #replication.start }

Start replication from the source volume to the target volume.

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/start
X-Auth-Token: {token-id}
```

<a id="replication.start-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume_mirror_id | URL | String | O | Replication setting ID |

<a id="replication.start-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication.status"></a>
### View replication status { #replication.status }

Returns the most recent replication state.

```
GET  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stat
X-Auth-Token: {token-id}
```

<a id="replication.status-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume_mirror_id | URL | String | O | Replication setting ID |

<a id="replication.status-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| volumeMirrorStat | Body | Object | Replication status object |
| volumeMirrorStat.lastSuccessTransferBytes | Body | Integer | Size of data transferred in the last successful replication (bytes) |
| volumeMirrorStat.lastSuccessTransferEndTime | Body | String | Last successful replication completion time |
| volumeMirrorStat.lastTransferBytes | Body | Integer | Size of data transferred in the last replication run (bytes) |
| volumeMirrorStat.lastTransferEndTime | Body | String | Last completed replication time |
| volumeMirrorStat.lastTransferStatus | Body | String | Recent replication run results |
| volumeMirrorStat.status | Body | String | Replication setting status<br>- `ACTIVE`: Replication active status<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings <br>- `HALT`: Halted replication status<br>- `RETRIEVE FAILED`: Temporary failure to obtain information |

<br>

<a id="replication.stop"></a>
### Stop replication { #replication.stop }

Stops replication from the source volume to the target volume.

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stop
X-Auth-Token: {token-id}
```

<a id="replication.stop-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume_mirror_id | URL | String | O | Replication setting ID |

<a id="replication.stop-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>