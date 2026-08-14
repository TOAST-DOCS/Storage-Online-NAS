{% include-markdown '../_online-nas-vars.md' %}

<!-- pre-align:aligned sig=06dac106ebf2 -->

{% macro interface_response_table(prefix='', desc_prefix='') -%}
| $[ prefix ]$id | Body | String | $[ desc_prefix ]$인터페이스 ID |
| $[ prefix ]$path | Body | String | $[ desc_prefix ]$인터페이스 경로 |
| $[ prefix ]$status | Body | String | $[ desc_prefix ]$인터페이스 상태 |
| $[ prefix ]$subnetId | Body | String | $[ desc_prefix ]$인터페이스의 서브넷 ID |
| $[ prefix ]$tenantId | Body | String | $[ desc_prefix ]$인터페이스의 테넌트 ID |{% endmacro %}
{# end macro interface_response_table #}
{% macro volume_mirror_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | 복제 설정 ID |
| $[ prefix ]$role | Body | String | 복제 역할<br>- `SOURCE`: 원본 볼륨<br>- `DESTINATION`: 대상 볼륨 |
| $[ prefix ]$status | Body | String | 복제 설정 상태<br>- `INITIALIZED`: 설정 완료<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중 |
| $[ prefix ]$direction | Body | String | 복제 방향<br>- `FORWARD`: 원본 볼륨 → 대상 볼륨<br>- `REVERSE`: 대상 볼륨 → 원본 볼륨 |
| $[ prefix ]$directionChangedAt | Body | String | 복제 방향 변경 시각 |
| $[ prefix ]$dstProjectId | Body | String | 복제 대상 볼륨의 프로젝트 ID |
| $[ prefix ]$dstRegion | Body | String | 복제 대상 볼륨 리전 |
| $[ prefix ]$dstTenantId | Body | String | 복제 대상 볼륨 테넌트 ID |
| $[ prefix ]$dstVolumeId | Body | String | 복제 대상 볼륨 ID |
| $[ prefix ]$dstVolumeName | Body | String | 복제 대상 볼륨 이름 |
| $[ prefix ]$srcProjectId | Body | String | 원본 볼륨의 프로젝트 ID |
| $[ prefix ]$srcRegion | Body | String | 원본 볼륨 리전 |
| $[ prefix ]$srcTenantId | Body | String | 원본 볼륨 테넌트 ID |
| $[ prefix ]$srcVolumeId | Body | String | 원본 볼륨 ID |
| $[ prefix ]$srcVolumeName | Body | String | 원본 볼륨 이름 |
| $[ prefix ]$createdAt | Body | String | 복제 생성 시각 |{% endmacro %}
{# end macro volume_mirror_response_table #}
{% macro volume_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | 볼륨 ID |
| $[ prefix ]$name | Body | String | 볼륨 이름 |
| $[ prefix ]$status | Body | String | 볼륨 상태 |
| $[ prefix ]$description | Body | String | 볼륨 설명 |
| $[ prefix ]$sizeGb | Body | Integer | 볼륨 크기(GB) |
| $[ prefix ]$projectId | Body | String | 볼륨이 속한 프로젝트 ID |
| $[ prefix ]$tenantId | Body | String | 볼륨이 속한 테넌트 ID |
| $[ prefix ]$acl | Body | List | 볼륨 ACL 목록 |
{% if encryption -%}
| $[ prefix ]$encryption | Body | Object | 볼륨 암호화 정보 |
| $[ prefix ]$encryption.enabled | Body | Boolean | 볼륨 암호화 활성 여부 |
| $[ prefix ]$encryption.keys | Body | List | 볼륨 암호화 키 정보 |
{%- endif %}
| $[ prefix ]$interfaces | Body | List | 볼륨 인터페이스 객체 목록 |
$[ interface_response_table(prefix + 'interfaces.') ]$
{% if replication -%}
| $[ prefix ]$mirrors | Body | List | 볼륨 복제 설정 객체 목록 |
$[ volume_mirror_response_table(prefix + 'mirrors.') ]$
{%- endif %}
| $[ prefix ]$mountProtocol | Body | Object | 볼륨 마운트 프로토콜 |
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | 볼륨 CIFS 인증 ID 목록 |
| $[ prefix ]$mountProtocol.protocol | Body | String | 볼륨 마운트 프로토콜 |
| $[ prefix ]$snapshotPolicy | Body | Object | 볼륨 스냅숏 설정 객체 |
| $[ prefix ]$snapshotPolicy.maxScheduledCount | Body | Integer | 스냅숏 최대 저장 개수 |
| $[ prefix ]$snapshotPolicy.reservePercent | Body | Integer | 스냅숏 용량 비율 |
| $[ prefix ]$snapshotPolicy.schedule | Body | Object | 스냅숏 자동 생성 객체 |
| $[ prefix ]$snapshotPolicy.schedule.time | Body | String | 스냅숏 자동 생성 시간 |
| $[ prefix ]$snapshotPolicy.schedule.timeOffset | Body | String | 스냅숏 자동 생성 기준 시간대 |
| $[ prefix ]$snapshotPolicy.schedule.weekdays | Body | List | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |
| $[ prefix ]$createdAt | Body | String | 볼륨 생성 시각 |
| $[ prefix ]$updatedAt | Body | String | 볼륨 변경 시각 |{% endmacro %}
{# end macro volume_response_table #}
{% macro volume_request_table(prefix='', method='') -%}
| $[ prefix ]$acl | Body | List | N | 볼륨 생성 시 설정할 ACL 목록<br>IP 또는 CIDR 형식으로 입력할 수 있습니다. |
| $[ prefix ]$description | Body | String | N | 볼륨 설명 |
{% if method == 'post' %}
{% if encryption -%}
| $[ prefix ]$encryption | Body | Object | N | 볼륨 생성 시 암호화 설정 객체 |
| $[ prefix ]$encryption.enabled | Body | Boolean | N | 암호화 설정 활성화 여부<br>암호화 키 저장소가 설정된 후 해당 필드를 `true`로 설정하면 암호화가 활성화됩니다. |
{%- endif %}
{% endif %}
{% if method == 'post' %}
| $[ prefix ]$interfaces | Body | List | N | 볼륨에 접근할 인터페이스 목록 |
| $[ prefix ]$interfaces.subnetId | Body | String | N | 볼륨 인터페이스의 서브넷 ID |
{% endif %}
| $[ prefix ]$mountProtocol | Body | Object | N | 볼륨 생성 시 프로토콜 설정 객체 |
{% if method == 'post' %}
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | N | CIFS 인증 ID 목록<br>NFS 프로토콜 선택 시 입력 불필요 |
| $[ prefix ]$mountProtocol.protocol | Body | String | Y | 볼륨 마운트 시 프로토콜 지정<br>`nfs`, `cifs` 중 하나를 선택할 수 있습니다. |
{% elif method == 'patch' %}
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | N | CIFS 인증 ID 목록 |
| $[ prefix ]$mountProtocol.protocol | Body | String | N | 이미 생성된 볼륨의 프로토콜은 변경할 수 없습니다.<br>`cifsAuthIds` 필드 변경 시 해당 필드에 `cifs`를 명시해야 합니다. |
{% endif %}
{% if method == 'post' %}
| $[ prefix ]$name | Body | String | Y | 볼륨 이름 |
{% endif %}
| $[ prefix ]$sizeGb | Body | Integer | $[ 'Y' if method == 'post'  else 'N' ]$ | 볼륨 크기(GB)<br>볼륨은 최소 300GB에서 최대 10,000GB까지, 100GB 단위로 설정할 수 있습니다. |
| $[ prefix ]$snapshotPolicy | Body | Object | N | 볼륨 스냅숏 설정 객체 |
| $[ prefix ]$snapshotPolicy.maxScheduledCount | Body | Integer | N | 스냅숏 최대 저장 개수<br>30개까지 설정 가능하며, 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 생성된 스냅숏이 삭제됩니다. |
| $[ prefix ]$snapshotPolicy.reservePercent | Body | Integer | N | 스냅숏 용량 비율 |
| $[ prefix ]$snapshotPolicy.schedule | Body | Object | N | 스냅숏 자동 생성 객체<br>`null`일 경우 스냅숏 자동 생성이 설정되지 않습니다. |
| $[ prefix ]$snapshotPolicy.schedule.time | Body | String | N | 스냅숏 자동 생성 시간 |
| $[ prefix ]$snapshotPolicy.schedule.timeOffset | Body | String | N | 스냅숏 자동 생성 기준 시간대 |
| $[ prefix ]$snapshotPolicy.schedule.weekdays | Body | List | N | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |{% endmacro %}
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
| $[ prefix ]$id | Body | String | 스냅숏 ID |
| $[ prefix ]$name | Body | String | 스냅숏 이름 |
| $[ prefix ]$size | Body | Integer | 스냅숏 크기 |
| $[ prefix ]$type | Body | String | 스냅숏 타입<br>- `NORMAL`: 사용자가 생성한 스냅숏<br>- `SCHEDULED`: 스냅숏 자동 생성으로 생성된 스냅숏<br>- `MIRROR`: 복제로 생성된 스냅숏 |
| $[ prefix ]$preserved | Body | Boolean | 시스템이 삭제 불가로 설정한 스냅숏 여부 |
| $[ prefix ]$createdAt | Body | String | 스냅숏 생성 시각 |{% endmacro %}
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
## Storage > NAS > API 가이드 { #storage-nas-api-guide }

이 문서는 NHN Cloud NAS가 제공하는 API로 볼륨과 스냅숏을 관리하는 방법을 설명합니다.

<a id="nas_api_common"></a>
## NAS API 공통 정보 { #nas_api_common }

<a id="nas_api_common.endpoint"></a>
### API 엔드포인트 { #nas_api_common.endpoint }

NAS API는 `nasv1` 타입 엔드포인트를 사용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 리전 | 엔드포인트 |
| --- | --- |
{% for region in regions %}| $[ region.name ]$ | $[ region.endpoint ]$ |
{% endfor %}

<a id="nas_api_common.authentication"></a>
### 인증 및 권한 { #nas_api_common.authentication }

NAS는 API 호출 시 인증/인가를 위해 IaaS 토큰을 사용합니다. IaaS 토큰은 NHN Cloud의 OpenStack 기반 인프라 서비스(IaaS)에서 사용하는 인증 토큰입니다.
IaaS 토큰 발급 및 사용 방법에 대한 자세한 내용은 [IaaS 토큰]($[ identity_guide_url ]$)을 참고합니다.

<a id="nas_api_common.response"></a>
### 응답 공통 정보 { #nas_api_common.response }

NAS API에서 제공하는 공통 응답 정보의 설명입니다. 모든 API 응답은 `header` 객체로 요청 결과를 전달합니다.

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| header | Object | 헤더 객체 |
| header.isSuccessful | Boolean | 요청의 성공 여부(`true` 또는 `false`) |
| header.resultCode | Integer | HTTP 상태 코드에 해당하는 결과 코드<br>- `200`: 성공<br>- `201`: 리소스 생성 성공<br>- `202`: 요청이 정상적으로 수신되었으나, 아직 처리되지 않은 상태<br>- `400`: 유효하지 않은 값으로 요청됨<br>- `401`: 권한, 인증 또는 토큰 관련 오류<br>- `404`: 요청한 리소스를 찾을 수 없음<br>- `405`: 요청한 URL이 지정한 HTTP 메서드를 지원하지 않음<br>- `5XX`: 클라이언트의 요청은 유효하지만 서버가 처리에 실패함 |
| header.resultMessage | String | 요청 처리 결과 메시지 |

<details>
  <summary><strong>성공 응답</strong></summary>

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
  <summary><strong>실패 응답</strong></summary>

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

!!! tip "알아두기"
    API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이러한 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

<a id="volume"></a>
## 볼륨 { #volume }

<a id="volume.list"></a>
### 볼륨 목록 보기 { #volume.list }

볼륨 목록을 조회합니다.

```
GET /v1/volumes
X-Auth-Token: {token-id}
```

<a id="volume.list-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| sizeGb | Query | String | N | 볼륨 크기 |
| maxSizeGb | Query | String | N | 볼륨 최대 크기 |
| minSizeGb | Query | String | N | 볼륨 최소 크기 |
| name | Query | String | N | 볼륨 이름 |
| nameContains | Query | String | N | 볼륨 이름에 포함되는 문자열 |
| subnetId | Query | String | N | 서브넷의 인터페이스를 가진 볼륨 |
| limit | Query | String | N | 한 페이지에 노출할 리소스 개수 |
| page | Query | String | N | 조회할 페이지 |
| sort | Query | String | N | 정렬 기준이 될 필드 이름<br>`{key}:{direction}` 형태로 기술합니다. 예: `name:asc`, `created_at:desc`<br>사용 가능한 key 값: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

<a id="volume.list-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| paging | Body | Object | 페이지 정보 |
| paging.limit | Body | Integer | 한 페이지에 노출되는 리소스 개수 |
| paging.page | Body | Integer | 현재 페이지 번호 |
| paging.totalCount | Body | Integer | 전체 수 |
| volumes | Body | List | 볼륨 객체 목록 |
$[ volume_response_table('volumes.') ]$

<details>
  <summary>응답 예시</summary>

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
### 볼륨 생성하기 { #volume.create }

새로운 볼륨을 생성합니다.

!!! tip "참고: CIFS 프로토콜 사용"
    CIFS 프로토콜을 사용하려면 CIFS 인증 정보를 생성해야 합니다. 인증 정보는 프로젝트 단위로 관리되며, CIFS 볼륨마다 접근을 허용할 CIFS 인증 정보를 등록해야 합니다.
    CIFS 인증 정보는 콘솔의 **Storage > NAS > CIFS 인증 정보 관리** 창에서 생성할 수 있습니다.

{% if encryption %}
<!-- -->

!!! tip "참고: 암호화 키 저장소 설정"
    암호화 볼륨을 생성하면 암호화에 사용하는 대칭 키가 NHN Cloud Secure Key Manager 서비스의 키 저장소에 저장됩니다. 따라서 암호화 볼륨을 만들려면 미리 Secure Key Manager 서비스에서 [키 저장소를 생성](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_1)해야 합니다. [키 저장소의 ID를 확인](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_2)하여 암호화 키 저장소 설정에 입력합니다.
    생성한 키 저장소 ID는 콘솔의 **Storage > NAS > 암호화 키 저장소 설정** 창에서 입력할 수 있습니다. 암호화 볼륨을 생성하면 설정한 키 저장소에 대칭 키가 저장됩니다. 키 저장소에 저장된 대칭 키는 암호화 볼륨 사용 중에는 삭제할 수 없습니다. 암호화 볼륨을 삭제하면 대칭 키도 함께 삭제됩니다.
    키 저장소 ID를 변경하면 이후 생성하는 암호화 볼륨의 대칭 키가 변경된 키 저장소에 저장됩니다. 기존 키 저장소에 저장된 대칭 키는 유지됩니다.

{% endif %}
```
POST /v1/volumes
X-Auth-Token: {token-id}
```

<br>

<a id="volume.create-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume | Body | Object | Y | 볼륨 생성 요청 객체 |
$[ volume_request_table('volume.', 'post') ]$

<details>
  <summary>요청 예시</summary>

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
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volume | Body | Object | 볼륨 객체 |
$[ volume_response_table('volume.') ]$

<details>
  <summary>응답 예시</summary>

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
### 볼륨 삭제하기 { #volume.delete }

지정한 볼륨을 삭제합니다.

```
DELETE /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 삭제할 볼륨 ID |

<a id="volume.delete-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="volume.view"></a>
### 볼륨 보기 { #volume.view }

지정한 볼륨의 상세 정보를 반환합니다.

```
GET /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.view-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 조회할 볼륨 ID |

<a id="volume.view-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volume | Body | Object | 볼륨 객체 |
$[ volume_response_table('volume.') ]$

<br>

<a id="volume.change_settings"></a>
### 볼륨 설정 변경하기 { #volume.change_settings }

지정한 볼륨의 설정을 변경합니다.

!!! danger "주의"
    복제 설정된 볼륨의 크기를 변경하려면 원본 볼륨과 대상 볼륨 모두 변경해야 합니다. 원본 볼륨과 대상 볼륨의 크기가 다른 경우 복제에 실패할 수 있습니다.

```
PATCH /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.change_settings-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| volume | Body | Object | Y | 볼륨 설정 변경 요청 객체 |
$[ volume_request_table('volume.', 'patch') ]$

<details>
  <summary>요청 예시</summary>

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
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="volume.connect_interface"></a>
### 볼륨에 인터페이스 연결하기 { #volume.connect_interface }

지정한 볼륨의 인터페이스를 설정합니다.
설정된 주소 및 서브넷에서 볼륨에 접근 가능합니다. 접근 가능한 IP는 접근 제어(ACL)에서 별도로 설정해야 합니다.

```
POST /v1/volumes/{volume_id}/interfaces
X-Auth-Token: {token-id}
```

<a id="volume.connect_interface-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| interface | Body | Object | Y | 인터페이스 설정 객체 |
| interface.subnetId | Body | String | Y | 인터페이스 서브넷 지정 |

<details>
  <summary>요청 예시</summary>

```json
{
  "interface":{
    "subnetId":"3e5b4d63-d143-420a-9263-208a447a2a3f"
  }
}
```

</details>

<a id="volume.connect_interface-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| interface | Body | Object | 생성된 인터페이스 정보 객체 |
$[ interface_response_table('interface.', '생성된 ') ]$

<details>
  <summary>응답 예시</summary>

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
### 볼륨의 인터페이스 삭제하기 { #volume.delete_interface }

지정한 볼륨의 지정한 인터페이스를 삭제합니다.

```
DELETE /v1/volumes/{volume_id}/interfaces/{interface_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete_interface-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| interface\_id | URL | String | Y | 삭제할 인터페이스 ID |

<a id="volume.delete_interface-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="volume.view_snapshot_restore_history"></a>
### 스냅숏 복원 내역 보기 { #volume.view_snapshot_restore_history }

지정한 볼륨의 스냅숏 복원 내역 목록을 반환합니다.

```
GET /v1/volumes/{volume_id}/restore-histories
X-Auth-Token: {token-id}
```

<a id="volume.view_snapshot_restore_history-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- |---| --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| limit | Query | String | N | 한 페이지에 노출할 리소스 개수 |
| page | Query | String | N | 조회할 페이지 |
| sort | Query | String | N | 정렬 기준이 될 필드 이름<br>`{key}:{direction}` 형태로 기술합니다. 예: `snapshotId:asc`, `requestedAt:desc`<br>사용 가능한 key 값: `snapshotId`, `snapshotName`, `requestedAt`, `restoredAt`, `requestedUser`, `requestedIp`, `result` |

<a id="volume.view_snapshot_restore_history-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| paging | Body | Object | 페이지 정보 |
| paging.limit | Body | Integer | 한 페이지에 노출되는 리소스 개수 |
| paging.page | Body | Integer | 현재 페이지 번호 |
| paging.totalCount | Body | Integer | 전체 수 |
| restoreHistories | Body | List | 스냅숏 복원 내역 객체 목록 |
| restoreHistories.requestedAt | Body | String | 스냅숏 복원을 요청한 시각 |
| restoreHistories.requestedIp | Body | String | 스냅숏 복원을 요청한 주소 |
| restoreHistories.requestedUser | Body | String | 스냅숏 복원을 요청한 사용자 ID |
| restoreHistories.restoredAt | Body | String | 스냅숏 복원을 완료한 시각 |
| restoreHistories.result | Body | String | 스냅숏 복원 결과 |
| restoreHistories.snapshotId | Body | String | 복원 대상 스냅숏 ID |
| restoreHistories.snapshotName | Body | String | 복원 대상 스냅숏 이름 |
| restoreHistories.volumeId | Body | String | 복원한 볼륨의 ID |

<details>
  <summary>응답 예시</summary>

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
### 볼륨 사용 현황 보기 { #volume.view_usage }

지정한 볼륨의 사용 현황을 반환합니다.

```
GET /v1/volumes/{volume_id}/usage
X-Auth-Token: {token-id}
```

<a id="volume.view_usage-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |

<a id="volume.view_usage-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| usage | Body | Object | 볼륨 사용 현황 객체 |
| usage.snapshotReserveGb | Body | Integer | 볼륨에서 스냅숏을 위해 예약한 공간 크기 |
| usage.snapshotUsedGb | Body | Integer | 스냅숏 사용량 |
| usage.snapshotUsedGbInReservedSpace | Body | Integer | 스냅숏 예약 용량 내 사용량 |
| usage.snapshotUsedGbInUserSpace | Body | Integer | 예약 용량 초과 스냅숏 사용량 |
| usage.usedGb | Body | Integer | 볼륨 사용량 |
| usage.userDataGb | Body | Integer | 사용자가 실제로 기록한 데이터 크기 |

<details>
  <summary>응답 예시</summary>

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
## 스냅숏 { #snapshots }

<a id="snapshots.list"></a>
### 스냅숏 목록 보기 { #snapshots.list }

스냅숏 목록을 조회합니다.

```
GET /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.list-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |

<a id="snapshots.list-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| snapshots | Body | List | 스냅숏 정보 객체 목록 |
$[ snapshot_response_table('snapshots.') ]$

<details><summary>응답 예시</summary>

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
### 스냅숏 생성하기 { #snapshots.create }

지정한 볼륨의 스냅숏을 생성합니다.

```
POST /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.create-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| snapshot | Body | Object | Y | 스냅숏 생성 객체 |
| snapshot.name | Body | String | Y | 스냅숏 이름 |

<details>
  <summary>요청 예시</summary>

```json
{
  "snapshot": {
    "name": "TEST-SNAPSHOT-1"
  }
}
```

</details>

<a id="snapshots.create-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| snapshot | Body | Object | 스냅숏 정보 객체 |
$[ snapshot_response_table('snapshot.') ]$

<details>
  <summary>응답 예시</summary>

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
### 스냅숏 삭제하기 { #snapshots.delete }

지정한 볼륨의 스냅숏을 삭제합니다.

```
DELETE /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.delete-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| snapshot\_id | URL | String | Y | 스냅숏 ID |

<a id="snapshots.delete-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="snapshots.view"></a>
### 스냅숏 보기 { #snapshots.view }

지정한 스냅숏의 상세 정보를 반환합니다.

```
GET /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.view-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| snapshot\_id | URL | String | Y | 스냅숏 ID |
| showReclaimableSpace | Query | Boolean | N | 스냅숏 삭제 시 확보되는 용량을 나타내는 `reclaimableSpace` 항목 노출 여부 |

<a id="snapshots.view-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| snapshot | Body | Object | 스냅숏 정보 객체 |
$[ snapshot_response_table('snapshot.') ]$

<br>

<a id="snapshots.restore"></a>
### 스냅숏 복원하기 { #snapshots.restore }

지정한 스냅숏으로 볼륨을 복원합니다.

```
POST /v1/volumes/{volume_id}/snapshots/{snapshot_id}/restore
X-Auth-Token: {token-id}
```

<a id="snapshots.restore-request"></a>
#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| snapshot\_id | URL | String | Y | 스냅숏 ID |

<a id="snapshots.restore-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

{% if replication %}

<a id="replication"></a>
## 볼륨 복제 설정 { #replication }

<a id="replication.setup"></a>
### 복제 설정하기 { #replication.setup }

지정한 볼륨의 복제를 설정합니다.
복제 대상 프로젝트별 선택 가능한 리전 범위는 아래 표에서 확인할 수 있습니다.

| 대상 프로젝트 | 선택 가능한 리전 |
| ------- | --------- |
| 조직 내 동일 프로젝트 | 다른 리전 |
| 조직 내 다른 프로젝트 | 모든 리전 |

<br>

!!! danger "주의"
    복제 대상 볼륨 크기는 원본 볼륨과 동일하게 설정해야 합니다. 원본 볼륨과 대상 볼륨의 크기가 다른 경우 복제에 실패할 수 있습니다.

<!-- -->

!!! tip "알아두기"
    복제 대상 볼륨에 암호화를 설정하려면 복제 대상 볼륨이 속한 프로젝트 또는 리전에 원본 볼륨과는 별도의 암호화 키 저장소를 설정해야 합니다.

<!-- -->

!!! tip "알아두기"
    원본 볼륨이 CIFS 프로토콜을 사용하는 경우 대상 볼륨도 CIFS 프로토콜을 사용해야 합니다. 이를 위해 원본 볼륨과는 별개의 CIFS 인증 정보를 생성하여 요청 본문 `cifsAuthIds` 필드에 입력해야 합니다.

```
POST /v1/volumes/{volume_id}/volume-mirrors
X-Auth-Token: {token-id}
```

<a id="replication.setup-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 원본 볼륨 ID |
| volumeMirror | Body | Object | Y | 볼륨 복제 설정 요청 객체 |
| volumeMirror.dstRegion | Body | String | Y | 복제 대상 볼륨의 리전 |
| volumeMirror.dstTenantId | Body | String | Y | 복제 대상 볼륨의 테넌트 ID |
| volumeMirror.dstVolume | Body | Object | Y | 복제 대상 볼륨 생성 요청 객체 |
$[ volume_request_table('volumeMirror.dstVolume.', 'post') ]$

<details>
  <summary>요청 예시</summary>

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
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volumeMirror | Body | Object | 복제 설정 생성 객체 |
$[ volume_mirror_response_table('volumeMirror.') ]$

<details>
  <summary>응답 예시</summary>

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
### 복제 설정 해제하기 { #replication.disable }

지정한 볼륨의 복제 설정을 해제합니다.

```
DELETE /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}
X-Auth-Token: {token-id}
```

<a id="replication.disable-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| volume\_mirror\_id | URL | String | Y | 복제 설정 ID |

<a id="replication.disable-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="replication.change_direction"></a>
### 복제 방향 변경하기 { #replication.change_direction }

원본 볼륨과 대상 볼륨의 복제 방향을 변경합니다.

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/invert-direction
X-Auth-Token: {token-id}
```

<a id="replication.change_direction-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| volume\_mirror\_id | URL | String | Y | 복제 설정 ID |

<a id="replication.change_direction-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="replication.start"></a>
### 복제 시작하기 { #replication.start }

원본 볼륨에서 대상 볼륨으로의 복제를 시작합니다.

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/start
X-Auth-Token: {token-id}
```

<a id="replication.start-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| volume\_mirror\_id | URL | String | Y | 복제 설정 ID |

<a id="replication.start-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

<a id="replication.status"></a>
### 복제 상태 확인하기 { #replication.status }

가장 최근의 복제 상태를 반환합니다.

```
GET /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stat
X-Auth-Token: {token-id}
```

<a id="replication.status-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| volume\_mirror\_id | URL | String | Y | 복제 설정 ID |

<a id="replication.status-response"></a>
#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volumeMirrorStat | Body | Object | 복제 상태 객체 |
| volumeMirrorStat.lastSuccessTransferBytes | Body | Integer | 최근 성공한 복제에서 전송된 데이터 크기(Byte) |
| volumeMirrorStat.lastSuccessTransferEndTime | Body | String | 최근 성공한 복제 완료 시간 |
| volumeMirrorStat.lastTransferBytes | Body | Integer | 최근 실행한 복제에서 전송된 데이터 크기(Byte) |
| volumeMirrorStat.lastTransferEndTime | Body | String | 최근 실행한 복제 완료 시간 |
| volumeMirrorStat.lastTransferStatus | Body | String | 최근 복제 실행 결과 |
| volumeMirrorStat.status | Body | String | 복제 설정 상태<br>- `ACTIVE`: 복제 활성화 상태<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중<br>- `HALT`: 복제 중지 상태<br>- `RETRIEVE FAILED`: 일시적인 정보 획득 실패 |

<br>

<a id="replication.stop"></a>
### 복제 중지하기 { #replication.stop }

원본 볼륨에서 대상 볼륨으로의 복제를 중지합니다.

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stop
X-Auth-Token: {token-id}
```

<a id="replication.stop-request"></a>
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | 토큰 ID |
| volume\_id | URL | String | Y | 볼륨 ID |
| volume\_mirror\_id | URL | String | Y | 복제 설정 ID |

<a id="replication.stop-response"></a>
#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>
{% endif %}
