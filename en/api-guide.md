<!-- machine_translated: true -->

{% include-markdown '../_online-nas-vars.md' %}

<!-- pre-align:aligned sig=06dac106ebf2 -->

{% macro interface_response_table(prefix='', desc_prefix='') -%}
| $[ prefix ]$id | Body | String | $[ desc_prefix ]$Interface ID |
| $[ prefix ]$path | Body | String | $[ desc_prefix ]$Interface path |
| $[ prefix ]$status | Body | String | $[ desc_prefix ]$Interface status |
| $[ prefix ]$subnetId | Body | String | $[ desc_prefix ]$Subnet ID of the interface |
| $[ prefix ]$tenantId | Body | String | $[ desc_prefix ]$Tenant ID of the interface |{% endmacro %}
{# end macro interface_response_table #}
{% macro volume_mirror_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | Replication settings ID |
| $[ prefix ]$role | Body | String | Replication role<br>- `SOURCE`: Source volume<br>- `DESTINATION`: Target volume |
| $[ prefix ]$status | Body | String | Replication settings status<br>- `INITIALIZED`: Settings complete<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings |
| $[ prefix ]$direction | Body | String | Replication direction<br>- `FORWARD`: Source volume → Target volume<br>- `REVERSE`: Target volume → Source volume |
| $[ prefix ]$directionChangedAt | Body | String | Replication direction change time |
| $[ prefix ]$dstProjectId | Body | String | Project ID of the replication target volume |
| $[ prefix ]$dstRegion | Body | String | Replication target volume region |
| $[ prefix ]$dstTenantId | Body | String | Replication target volume tenant ID |
| $[ prefix ]$dstVolumeId | Body | String | Replication target volume ID |
| $[ prefix ]$dstVolumeName | Body | String | Replication target volume name |
| $[ prefix ]$srcProjectId | Body | String | Project ID of the source volume |
| $[ prefix ]$srcRegion | Body | String | Source volume region |
| $[ prefix ]$srcTenantId | Body | String | Source volume tenant ID |
| $[ prefix ]$srcVolumeId | Body | String | Source volume ID |
| $[ prefix ]$srcVolumeName | Body | String | Source volume name |
| $[ prefix ]$createdAt | Body | String | Replication creation time |{% endmacro %}
{# end macro volume_mirror_response_table #}
{% macro volume_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | Volume ID |
| $[ prefix ]$name | Body | String | Volume name |
| $[ prefix ]$status | Body | String | Volume status |
| $[ prefix ]$description | Body | String | Volume description |
| $[ prefix ]$sizeGb | Body | Integer | Volume size (GB) |
| $[ prefix ]$projectId | Body | String | Project ID of the volume |
| $[ prefix ]$tenantId | Body | String | Tenant ID of the volume |
| $[ prefix ]$acl | Body | List | Volume ACL list |
{% if encryption -%}
| $[ prefix ]$encryption | Body | Object | Volume encryption information |
| $[ prefix ]$encryption.enabled | Body | Boolean | Whether volume encryption is enabled |
| $[ prefix ]$encryption.keys | Body | List | Volume encryption key information |
{%- endif %}
| $[ prefix ]$interfaces | Body | List | Volume interface object list |
$[ interface_response_table(prefix + 'interfaces.') ]$
{% if replication -%}
| $[ prefix ]$mirrors | Body | List | Volume replication settings object list |
$[ volume_mirror_response_table(prefix + 'mirrors.') ]$
{%- endif %}
| $[ prefix ]$mountProtocol | Body | Object | Volume mount protocol |
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | Volume CIFS authentication ID list |
| $[ prefix ]$mountProtocol.protocol | Body | String | Volume mount protocol |
| $[ prefix ]$snapshotPolicy | Body | Object | Volume snapshot settings object |
| $[ prefix ]$snapshotPolicy.maxScheduledCount | Body | Integer | Maximum number of snapshots to store |
| $[ prefix ]$snapshotPolicy.reservePercent | Body | Integer | Snapshot capacity ratio |
| $[ prefix ]$snapshotPolicy.schedule | Body | Object | Snapshot auto-creation object |
| $[ prefix ]$snapshotPolicy.schedule.time | Body | String | Snapshot auto-creation time |
| $[ prefix ]$snapshotPolicy.schedule.timeOffset | Body | String | Snapshot auto-creation reference timezone |
| $[ prefix ]$snapshotPolicy.schedule.weekdays | Body | List | Snapshot auto-creation days of the week<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |
| $[ prefix ]$createdAt | Body | String | Volume creation time |
| $[ prefix ]$updatedAt | Body | String | Volume last updated time |{% endmacro %}
{# end macro volume_response_table #}
{% macro volume_request_table(prefix='', method='') -%}
| $[ prefix ]$acl | Body | List | N | ACL list to configure when creating the volume<br>You can enter values in IP or CIDR format. |
| $[ prefix ]$description | Body | String | N | Volume description |
{% if method == 'post' %}
{% if encryption -%}
| $[ prefix ]$encryption | Body | Object | N | Encryption settings object when creating the volume |
| $[ prefix ]$encryption.enabled | Body | Boolean | N | Whether encryption settings are enabled<br>After the encryption keystore is set up, setting this field to `true` enables encryption. |
{%- endif %}
{% endif %}
{% if method == 'post' %}
| $[ prefix ]$interfaces | Body | List | N | List of interfaces to access the volume |
| $[ prefix ]$interfaces.subnetId | Body | String | N | Subnet ID of the volume interface |
{% endif %}
| $[ prefix ]$mountProtocol | Body | Object | N | Protocol settings object when creating the volume |
{% if method == 'post' %}
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | N | CIFS authentication ID list<br>Not required when NFS protocol is selected |
| $[ prefix ]$mountProtocol.protocol | Body | String | Y | Specifies the protocol when mounting the volume<br>You can select one of `nfs` or `cifs`. |
{% elif method == 'patch' %}
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | N | CIFS authentication ID list |
| $[ prefix ]$mountProtocol.protocol | Body | String | N | The protocol of an already created volume cannot be changed.<br>When changing the `cifsAuthIds` field, you must specify `cifs` in this field. |
{% endif %}
{% if method == 'post' %}
| $[ prefix ]$name | Body | String | Y | Volume name |
{% endif %}
| $[ prefix ]$sizeGb | Body | Integer | $[ 'Y' if method == 'post'  else 'N' ]$ | Volume size (GB)<br>The volume can be set from a minimum of 300 GB to a maximum of 10,000 GB, in 100 GB increments. |
| $[ prefix ]$snapshotPolicy | Body | Object | N | Volume snapshot settings object |
| $[ prefix ]$snapshotPolicy.maxScheduledCount | Body | Integer | N | Maximum number of snapshots to store<br>You can set a maximum of 30, and the first automatically created snapshot will be deleted when the maximum number of saves is reached. |
| $[ prefix ]$snapshotPolicy.reservePercent | Body | Integer | N | Snapshot capacity ratio |
| $[ prefix ]$snapshotPolicy.schedule | Body | Object | N | Snapshot auto-creation object<br>If `null`, snapshot auto-creation will not be configured. |
| $[ prefix ]$snapshotPolicy.schedule.time | Body | String | N | Snapshot auto-creation time |
| $[ prefix ]$snapshotPolicy.schedule.timeOffset | Body | String | N | Snapshot auto-creation reference timezone |
| $[ prefix ]$snapshotPolicy.schedule.weekdays | Body | List | N | Snapshot auto-creation days of the week<br>An empty list means every day, and the days of the week are specified as a list of numbers from 0 (Sunday) to 6 (Saturday). |{% endmacro %}
{# end macro volume_request_table #}
{% macro volume_mirror_response_json(indent=0, method='') -%}
$[ ' ' * indent ]$"createdAt":"2025-04-01T06:45:45+00:00",
$[ ' ' * indent ]$"direction": "FORWARD",
$[ ' ' * indent ]$"directionChangedAt": null,
$[ ' ' * indent ]$"dstProjectId": "K3y0CgOy",
$[ ' ' * indent ]$"dstRegion": "KR2",
$[ ' ' * indent ]$"dstTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
$[ ' ' * indent ]$"dstVolumeId": "e09281d2-0b1c-48a9-8a01-0098aa59f624",
$[ ' ' * indent ]$"dstVolumeName": "TEST-NAS-MIRROR-1",
$[ ' ' * indent ]$"id": "8116892c-7306-48be-9e3d-143311b2254c",
$[ ' ' * indent ]$"role": "SOURCE",
$[ ' ' * indent ]$"srcProjectId": "K3y0CgOy",
$[ ' ' * indent ]$"srcRegion": "KR1",
$[ ' ' * indent ]$"srcTenantId": "3b6179e5fa6b499386b827357c4cb8c4",
$[ ' ' * indent ]$"srcVolumeId": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
$[ ' ' * indent ]$"srcVolumeName": "TEST-NAS-1",
$[ ' ' * indent ]$"status": "PENDING"{% endmacro %}
{# end macro #}
{% macro volume_response_json(indent=0, method='') -%}
$[ ' ' * indent ]$"acl": [
$[ ' ' * indent ]$  "10.0.1.0/24"
$[ ' ' * indent ]$],
$[ ' ' * indent ]$"createdAt": "2025-04-01T06:44:25+00:00",
$[ ' ' * indent ]$"description": "NAS for Testing",
{% if encryption %}
$[ ' ' * indent ]$"encryption": {
$[ ' ' * indent ]$  "enabled": false
$[ ' ' * indent ]$},
{% endif %}
$[ ' ' * indent ]$"id": "fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
$[ ' ' * indent ]$"interfaces": [
$[ ' ' * indent ]$  {
$[ ' ' * indent ]$    "id": "9a8ec90f-cc27-4649-9bda-a1f0b193a402",
$[ ' ' * indent ]$    "path": "10.0.1.7:/TEST-NAS-1",
$[ ' ' * indent ]$    "status": "ACTIVE",
$[ ' ' * indent ]$    "subnetId": "cb779d62-72ef-43b6-b368-3fe28dcd812b",
$[ ' ' * indent ]$    "tenantId": "3b6179e5fa6b499386b827357c4cb8c4"
$[ ' ' * indent ]$  }
$[ ' ' * indent ]$],
{% if method == 'post' %}
$[ ' ' * indent ]$"mirrors": []
{% else %}
$[ ' ' * indent ]$"mirrors": [
$[ ' ' * indent ]$  {
$[ volume_mirror_response_json(indent+4) ]$
$[ ' ' * indent ]$  }
$[ ' ' * indent ]$],
{% endif %}
$[ ' ' * indent ]$"mountProtocol": {
$[ ' ' * indent ]$  "protocol": "cifs",
$[ ' ' * indent ]$  "cifsAuthIds": [
$[ ' ' * indent ]$    "cifs-test-id"
$[ ' ' * indent ]$  ]
$[ ' ' * indent ]$},
$[ ' ' * indent ]$"name": "TEST-NAS-1",
$[ ' ' * indent ]$"projectId": "K3y0CgOy",
$[ ' ' * indent ]$"sizeGb": 300,
$[ ' ' * indent ]$"snapshotPolicy": {
$[ ' ' * indent ]$  "maxScheduledCount": 1,
$[ ' ' * indent ]$  "reservePercent": 5,
$[ ' ' * indent ]$  "schedule": {
$[ ' ' * indent ]$    "time": "00:00",
$[ ' ' * indent ]$    "timeOffset": "+09:00",
$[ ' ' * indent ]$    "weekdays": [
$[ ' ' * indent ]$      1,
$[ ' ' * indent ]$      3,
$[ ' ' * indent ]$      5
$[ ' ' * indent ]$    ]
$[ ' ' * indent ]$  }
$[ ' ' * indent ]$},
$[ ' ' * indent ]$"stationId": null,
$[ ' ' * indent ]$"status": "ACTIVE",
$[ ' ' * indent ]$"tenantId": "3b6179e5fa6b499386b827357c4cb8c4",
$[ ' ' * indent ]$"updatedAt": "2025-04-01T06:47:13+00:00"{% endmacro %}
{# end macro #}
{% macro snapshot_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | Snapshot ID |
| $[ prefix ]$name | Body | String | Snapshot name |
| $[ prefix ]$size | Body | Integer | Snapshot size |
| $[ prefix ]$type | Body | String | Snapshot type<br>- `NORMAL`: Snapshot created by the user<br>- `SCHEDULED`: Snapshot created by auto-creation<br>- `MIRROR`: Snapshot created by replication |
| $[ prefix ]$preserved | Body | Boolean | Whether the snapshot is set as non-deletable by the system |
| $[ prefix ]$createdAt | Body | String | Snapshot creation time |{% endmacro %}
{# end macro snapshot_response_table #}
{% macro snapshot_response_json(indent=0) -%}
$[ ' ' * indent ]$"createdAt": "2025-04-01T09:34:27+00:00",
$[ ' ' * indent ]$"id": "8151fe33-0edc-11f0-b0e3-d039eaa3e920",
$[ ' ' * indent ]$"name": "TEST-SNAPSHOT-1",
$[ ' ' * indent ]$"preserved": false,
$[ ' ' * indent ]$"size": 3112960,
$[ ' ' * indent ]$"type": "NORMAL"{% endmacro %}
{# end macro #}
<a id="storage-nas-api-guide"></a>
## Storage > NAS > API Guide { #storage-nas-api-guide }

This document describes how to manage volumes and snapshots by using the API provided by NHN Cloud NAS.

<a id="nas_api_common"></a>
## NAS API Common Information { #nas_api_common }

<a id="nas_api_common.endpoint"></a>
### API Endpoint { #nas_api_common.endpoint }

NAS API uses the `nasv1` type endpoint. Refer to the `serviceCatalog` in the token issuance response for the valid endpoint.

| Region | Endpoint |
| --- | --- |
{% for region in regions %}| $[ region.name ]$ | $[ region.endpoint ]$ |
{% endfor %}
<a id="nas_api_common.authentication"></a>
### Authentication and Authorization { #nas_api_common.authentication }

NAS uses IaaS tokens for authentication and authorization when making API calls. The IaaS token is an authentication token used for NHN Cloud's OpenStack-based infrastructure services (IaaS).
For more information on issuing and using IaaS tokens, see [IaaS token]($[ identity_guide_url ]$).

<a id="nas_api_common.response"></a>
### Response Common Information { #nas_api_common.response }

This section describes the common response information provided by the NAS API. All API responses convey the result of a request with a `header` object.

| Name | Type | Description |
| --- | --- | --- |
| header | Object | Header Objects |
| header.isSuccessful | Boolean | Whether the request was successful (`true` or `false`) |
| header.resultCode | Integer | Result codes corresponding to HTTP status codes<br>- `200`: Success<br>- `201`: Resource creation successful<br>- `202`: Request received successfully, but not yet processed<br>- `400`: Requested with an invalid value<br>- `401`: Permission, authentication, or token-related error<br>- `404`: Requested resource not found<br>- `405`: The requested URL does not support the specified HTTP method<br>- `5XX`: The client's request is valid but the server failed to process it |
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

!!! tip "Note"
    Fields not specified in the guide may appear in API responses. These fields are used internally by NHN Cloud and are subject to change without prior notice, so they are not used.

<a id="volume"></a>
## Volume { #volume }

<a id="volume.list"></a>
### List Volume { #volume.list }

Return the list of volumes.

```
GET /v1/volumes
X-Auth-Token: {token-id}
```

<a id="volume.list-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| sizeGb | Query | String | N | Volume size |
| maxSizeGb | Query | String | N | Volume maximum size |
| minSizeGb | Query | String | N | Volume minimum size |
| name | Query | String | N | Volume name |
| nameContains | Query | String | N | Strings included in the volume name |
| subnetId | Query | String | N | Volume with interfaces on a subnet |
| limit | Query | String | N | Number of resources to expose on a page |
| page | Query | String | N | Page to search |
| sort | Query | String | N | Name of the field to sort by<br>Describe it in the form `{key}:{direction}`. Example: `name:asc`, `created_at:desc`<br>Possible key values: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

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
$[ volume_response_table('volumes.') ]$

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
$[ volume_response_json(indent=6) ]$
    }
  ]
}
```

</details>

<br>

<a id="volume.create"></a>
### Create Volume { #volume.create }

Create a new volume.

!!! tip "Note: Using the CIFS protocol"
    To use the CIFS protocol, you must create CIFS credentials. Credentials are managed on a per-project basis, and you must register CIFS credentials to allow to access each CIFS volume.
    You can create CIFS credentials through the **Storage > NAS > Manage CIFS Credentials** of the console.

{% if encryption %}
<!-- -->

!!! tip "Note: Setting up encryption key storage"
    When an encrypted volume is created, the symmetric key used for encryption is stored in the NHN Cloud Secure Key Manager store. To create encrypted volume,[you must first create a keystore](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_1) in the Secure Key Manager service. After creating the keystore, [check its ID](https://docs.nhncloud.com/en/Security/Secure%20Key%20Manager/en/getting-started/#_2) and enter it in the encryption keystore settings.
    You can enter the keystore ID from the **Storage > NAS > Encryption keystore settings** in the console. When you create encrypted volume, the symmetric key is stored in the specified keystore. The symmetric key stored in the keystore cannot be deleted while the encrypted volume is in use. When the encrypted volume is deleted, the corresponding symmetric key is also deleted.
    If you change the keystore ID, symmetric keys for newly created encrypted volume will be stored in the new keystore. Symmetric keys already stored in the previous keystore are retained.


{% endif %}
```
POST /v1/volumes
X-Auth-Token: {token-id}
```

<br>

<a id="volume.create-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume | Body | Object | Y | Volume creation request object |
$[ volume_request_table('volume.', 'post') ]$

<details>
  <summary>Request Example</summary>

```json
{
  "volume": {
    "acl": [
      "10.0.1.0/24"
    ],
    "description": "NAS for Testing",
{% if encryption %}
    "encryption": {
      "enabled": true
    },
{% endif %}
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
$[ volume_response_table('volume.') ]$

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
$[ volume_response_json(indent=4) ]$
  }
}
```

</details>

<br>

<a id="volume.delete"></a>
### Delete Volume { #volume.delete }

Deletes the specified volume.

```
DELETE /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID to delete |

<a id="volume.delete-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="volume.view"></a>
### View Volume { #volume.view }

Returns details about the specified volume.

```
GET /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.view-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID to query |

<a id="volume.view-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| volume | Body | Object | Volume objects |
$[ volume_response_table('volume.') ]$

<br>

<a id="volume.change_settings"></a>
### Change Volume Settings { #volume.change_settings }

Change the settings for the specified volume.

!!! danger "Caution"
    To change the size of a replicated volume, you must change both the source volume and the target volume. If the size of the source volume and the target volume are different, replication might fail.

```
PATCH /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.change_settings-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| volume | Body | Object | Y | Request object for changing volume settings |
$[ volume_request_table('volume.', 'patch') ]$

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
The volume is accessible from the set address and subnet. The accessible IP must be set separately in the access control (ACL) settings.

```
POST /v1/volumes/{volume_id}/interfaces
X-Auth-Token: {token-id}
```

<a id="volume.connect_interface-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| interface | Body | Object | Y | Interface Settings Object |
| interface.subnetId | Body | String | Y | Specify an interface subnet |

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
$[ interface_response_table('interface.', 'Created ') ]$

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
DELETE /v1/volumes/{volume_id}/interfaces/{interface_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete_interface-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| interface_id | URL | String | Y | Interface ID to delete |

<a id="volume.delete_interface-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="volume.view_snapshot_restore_history"></a>
### View snapshot restore history { #volume.view_snapshot_restore_history }

Returns a list of snapshot restore history for the specified volume.

```
GET /v1/volumes/{volume_id}/restore-histories
X-Auth-Token: {token-id}
```

<a id="volume.view_snapshot_restore_history-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- |---| --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| limit | Query | String | N | Number of resources to expose on a page |
| page | Query | String | N | Page to search |
| sort | Query | String | N | Name of the field to sort by<br>Describe it in the form `{key}:{direction}`. Example: `snapshotId:asc`, `requestedAt:desc`<br>Possible key values: `snapshotId`, `snapshotName`, `requestedAt`, `restoredAt`, `requestedUser`, `requestedIp`, `result` |

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
GET /v1/volumes/{volume_id}/usage
X-Auth-Token: {token-id}
```

<a id="volume.view_usage-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |

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
GET /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.list-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |

<a id="snapshots.list-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| snapshots | Body | List | Snapshot info object list |
$[ snapshot_response_table('snapshots.') ]$

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
$[ snapshot_response_json(6) ]$
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
POST /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.create-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| snapshot | Body | Object | Y | Snapshot creation objects |
| snapshot.name | Body | String | Y | Snapshot name |

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
$[ snapshot_response_table('snapshot.') ]$

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
$[ snapshot_response_json(4) ]$
  }
}
```

</details>

<br>

<a id="snapshots.delete"></a>
### Delete Snapshots { #snapshots.delete }

Deletes a snapshot of the specified volume.

```
DELETE /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.delete-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| snapshot_id | URL | String | Y | Snapshot ID |

<a id="snapshots.delete-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="snapshots.view"></a>
### View Snapshot { #snapshots.view }

Returns details of the specified snapshot.

```
GET /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.view-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| snapshot_id | URL | String | Y | Snapshot ID |
| showReclaimableSpace | Query | Boolean | N | Whether to expose `the reclaimableSpace` entry, which indicates the amount of space reclaimed when a snapshot is deleted. |

<a id="snapshots.view-response"></a>
#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object | Header Objects |
| snapshot | Body | Object | Snapshot information objects |
$[ snapshot_response_table('snapshot.') ]$

<br>

<a id="snapshots.restore"></a>
### Restore Snapshot { #snapshots.restore }

Restores volume to the specified snapshot.

```
POST /v1/volumes/{volume_id}/snapshots/{snapshot_id}/restore
X-Auth-Token: {token-id}
```

<a id="snapshots.restore-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| snapshot_id | URL | String | Y | Snapshot ID |

<a id="snapshots.restore-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

{% if replication %}

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

!!! danger "Caution"
    The size of the target volume for replication must be the same as the source volume.
    If the sizes of the source and target volumes differ, the replication may fail.

<!-- -->

!!! tip "Note"
    To set up encryption on the target volume for replication, you must configure a separate encryption key store for the project or region where the target volume belongs, independent of the source volume.

<!-- -->

!!! tip "Notice"
    If the source volume uses the CIFS protocol, the target volume must also use the CIFS protocol. To do this, you must create separate CIFS credentials (different from the source volume) and specify them in the `cifsAuthIds` field of the request body.

```
POST /v1/volumes/{volume_id}/volume-mirrors
X-Auth-Token: {token-id}
```

<a id="replication.setup-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Source volume ID |
| volumeMirror | Body | Object | Y | Volume replication settings request object  |
| volumeMirror.dstRegion | Body | String | Y | The region of replication target volume |
| volumeMirror.dstTenantId | Body | String | Y | The tenant ID of the replication target volume |
| volumeMirror.dstVolume | Body | Object | Y | Replication target volume request object |
$[ volume_request_table('volumeMirror.dstVolume.', 'post') ]$

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
$[ volume_mirror_response_table('volumeMirror.') ]$

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
$[ volume_mirror_response_json(4) ]$
  }
}
```

</details>

<br>

<a id="replication.disable"></a>
### Disable Replication Settings { #replication.disable }

Disable replication settings for the specified volume.

```
DELETE /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}
X-Auth-Token: {token-id}
```

<a id="replication.disable-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| volume_mirror_id | URL | String | Y | Replication setting ID |

<a id="replication.disable-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication.change_direction"></a>
### Change the replication direction { #replication.change_direction }

Change the direction of replication between source and target volume.

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/invert-direction
X-Auth-Token: {token-id}
```

<a id="replication.change_direction-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| volume_mirror_id | URL | String | Y | Replication setting ID |

<a id="replication.change_direction-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication.start"></a>
### Start Replication { #replication.start }

Start replication from the source volume to the target volume.

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/start
X-Auth-Token: {token-id}
```

<a id="replication.start-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| volume_mirror_id | URL | String | Y | Replication setting ID |

<a id="replication.start-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>

<a id="replication.status"></a>
### View replication status { #replication.status }

Returns the most recent replication state.

```
GET /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stat
X-Auth-Token: {token-id}
```

<a id="replication.status-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| volume_mirror_id | URL | String | Y | Replication setting ID |

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
| volumeMirrorStat.status | Body | String | Replication setting status<br>- `ACTIVE`: Active replication status<br>- `UPDATING`: Updating settings<br>- `DELETING`: Deleting settings<br>- `PENDING`: Creating settings<br>- `HALT`: Replication stopped status<br>- `RETRIEVE FAILED`: Temporary failure to obtain information |

<br>

<a id="replication.stop"></a>
### Stop replication { #replication.stop }

Stops replication from the source volume to the target volume.

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stop
X-Auth-Token: {token-id}
```

<a id="replication.stop-request"></a>
#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | Token ID |
| volume_id | URL | String | Y | Volume ID |
| volume_mirror_id | URL | String | Y | Replication setting ID |

<a id="replication.stop-response"></a>
#### Response

The response body does not contain any content other than header fields.

<br>
{% endif %}