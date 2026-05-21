```markdown
## Storage > NAS > API Guide

<a id="nas_api_common"></a>
## NAS API Common Information

<a id="nas_api_common.endpoint"></a>
### API Endpoint

NAS API uses the `nasv1` type endpoint. Refer to the `serviceCatalog` in the token issuance response for the valid endpoint.

| Region | Endpoint | 
| --- | --- |
| Korea (Pangyo) Region | https://kr1-api-nas-infrastructure.nhncloudservice.com |
| Korea (Pyeongchon) Region | https://kr2-api-nas-infrastructure.nhncloudservice.com |
| Korea (Gwangju) Region | https://kr3-api-nas-infrastructure.nhncloudservice.com |


<a id="nas_api_common.authentication"></a>
### Authentication and Authorization

NAS uses IaaS tokens for authentication and authorization when making API calls. The IaaS token is an authentication token used for NHN Cloud's OpenStack-based infrastructure services (IaaS).
For more information on issuing and using IaaS tokens, please refer to the [IaaS Token](/nhncloud/en/public-api/iaas-token/).


<a id="nas_api_common.response"></a>
### Response Common Information

This section describes the common response information provided by the NAS API. All API responses convey the result of a request via a `header` object.


| Name | Type | Description |
| --- | --- | --- |
| header | Object | Header Objects |
| header.isSuccessful | Boolean | Whether the request was successful (`true` or `false`) |
| header.resultCode | Integer | Result codes corresponding to HTTP status codes<br>- `200`: Success <br>- `201`: Resource creation successful<br>- `202`: Request received successfully, but not yet processed<br>- `400`: Requested with an invalid value<br>- `401`: Permission, authentication, or token-related error <br>- `404`: Requested resource not found<br>- `405`: The requested URL does not support the specified HTTP method<br>- `5XX`: The client's request is valid but the server failed to process it |
| header.resultMessage | String | Message about the result of request processing |

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
## Volume

<a id="volume.list"></a>
### List Volume

Return the list of volumes.

```
GET  /v1/volumes
X-Auth-Token: {token-id}
```

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
| volumes.mirrors.direction | Body | String | Replication direction<br>- `FORWARD`: Source volume → Replica volume<br>- `REVERSE`: Replica volume → Source volume |
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
### Create Volume

Create a new volume.

> [Note] Using the CIFS protocol
> To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register CIFS credentials to allow to access each CIFS volume.
> You can create CIFS credentials through the **Storage > NAS > Manage CIFS Credentials** of the console.

<!-- -->

> [Note] Setting up encryption key storage
> When an encrypted volume is created, the symmetric key used for encryption is stored in the NHN Cloud Secure Key Manager store. To create encrypted volume,[you must first create a keystore](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/ko/getting-started/#_1) in the Secure Key Manager service. After creating the keystore, [check its ID](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/ko/getting-started/#_2) and enter it in the encryption keystore settings.
> You can enter the keystore ID from the **Storage > NAS > Encryption keystore settings** in the console. When you create encrypted volume, the symmetric key is stored in the specified keystore. The symmetric key stored in the keystore cannot be deleted while the encrypted volume is in use. When the encrypted volume is deleted, the corresponding symmetric key is also deleted.
> If you change the keystore ID, symmetric keys for newly created encrypted volume will be stored in the new keystore. Symmetric keys already stored in the previous keystore are retained.


```
POST  /v1/volumes
X-Auth-Token: {token-id}
```

<br>

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
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | Days of the week that snapshots are automatically created.<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |

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
### Delete Volume

Deletes the specified volume.

```
DELETE  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID to delete |

#### Response

The response body does not contain any content other than header fields.

<br>

<a id="volume.view"></a>
### View Volume

Returns details about the specified volume.

```
GET   /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID to query |

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
### Change Volume Settings

Change the settings for the specified volume.

> [Caution]
To change the size of a replicated volume, you must change both the source volume and the target volume. If the size of the source volume and the target volume are different, replication might fail.

```
PATCH  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | Token ID |
| volume_id | URL | String | O | Volume ID |
| volume | Body | Object | O | Volume settings change request object |
| volume.acl | Body | List | - | List of ACLs to set when creating volume<br>You can enter it in IP or CIDR format. |
| volume.description | Body | String | - | Volume description |
| volume.mountProtocol | Body | Object | - | Protocol settings object when creating volume |
| volume.mountProtocol.cifsAuthIds | Body | List | - | List of CIFS Authentication IDs |
| volume.mountProtocol.protocol | Body | String | - | You cannot change the protocol of volume that has already been created.<br>When changing the `cifsAuthIds` field, you must specify the `cifs` in that field. |
| volume.sizeGb | Body | Integer | - | Volume size (GB)<br>Volume can be set from a minimum of 300 GB to a maximum of 10,000GB, in 100GB increments