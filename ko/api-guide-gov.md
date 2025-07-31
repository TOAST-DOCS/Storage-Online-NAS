## Storage > NAS > API 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](https://docs.gov-nhncloud.com/ko/Compute/Compute/ko/identity-api-gov/)를 참고하여 API 사용에 필요한 정보를 준비합니다.<br>
NAS 스토리지 API는 `nasv1` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 | 
| --- | --- | --- |
| nasv1 | 한국(판교) 리전 <br> 한국(평촌) 리전 | https://kr1-api-nas-infrastructure.gov-nhncloudservice.com  <br> https://kr2-api-nas-infrastructure.gov-nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

<br>

## 응답 공통 정보

NAS 스토리지 API에서 제공하는 공통 응답 정보에 대한 설명입니다. 모든 API 응답은 `header` 객체를 통해 요청 결과를 전달합니다.

### 응답 헤더

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| header.isSuccessful | Body | Boolean | 요청의 성공 여부(`true` 또는 `false`) |
| header.resultCode | Body | Integer | HTTP 상태 코드에 해당하는 결과 코드<br>- `200`: 성공 <br>- `201`: 리소스 생성 성공<br>- `202`: 요청이 정상적으로 수신되었으나, 아직 처리되지 않은 상태<br>- `400`: 유효하지 않은 값으로 요청됨<br>- `401`: 권한, 인증 또는 토큰 관련 오류 <br>- `404`: 요청한 리소스를 찾을 수 없음<br>- `405`: 요청한 URL이 지정한 HTTP 메서드를 지원하지 않음<br>- `5XX`: 클라이언트의 요청은 유효하지만 서버가 처리에 실패함 |
| header.resultMessage | Body | String | 요청 처리 결과에 대한 메시지 |

<details>
  <summary>응답 예시</summary>

```json
"header": {
  "isSuccessful": true,
  "resultCode": 200,
  "resultMessage": "Success"
}
```

</details>

<br>

## NAS 스토리지

### NAS 스토리지 목록 보기

NAS 스토리지 목록을 조회합니다.

```
GET  /v1/volumes
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| sizeGb | String | Query | - | NAS 스토리지 크기 |
| maxSizeGb | String | Query | - | NAS 스토리지 최대 크기 |
| minSizeGb | String | Query | - | NAS 스토리지 최소 크기 |
| name | String | Query | - | NAS 스토리지 이름 |
| nameContains | String | Query | - | NAS 스토리지 이름에 포함되는 문자열 |
| subnetId | String | Query | - | 서브넷의 인터페이스를 가진 NAS 스토리지 |
| limit | String | Query | - | 한 페이지에 노출할 리소스 개수 |
| page | String | Query | - | 조회할 페이지 |
| sort | String | Query | - | 정렬 기준이 될 필드 이름<br>`{key}:{direction}` 형태로 기술합니다. 예: `name:asc`, `created_at:desc`<br>사용 가능한 key 값: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| paging | Body | Object | 페이지 정보 |
| paging.limit | Body | Integer | 한 페이지에 노출되는 리소스 개수 |
| paging.page | Body | Integer | 현재 페이지 번호 |
| paging.totalCount | Body | Integer | 전체 수 |
| volumes | Body | List | NAS 스토리지 객체 목록 |
| volumes.id | Body | String | NAS 스토리지 ID |
| volumes.name | Body | String | NAS 스토리지 이름 |
| volumes.status | Body | String | NAS 스토리지 상태 |
| volumes.description | Body | String | NAS 스토리지 설명 |
| volumes.sizeGb | Body | Integer | NAS 스토리지 크기(GB) |
| volumes.projectId | Body | String | NAS 스토리지가 속한 프로젝트 ID |
| volumes.tenantId | Body | String | NAS 스토리지가 속한 테넌트 ID |
| volumes.acl | Body | List | NAS 스토리지 ACL 목록 |
| volumes.encryption | Body | Object | NAS 스토리지 암호화 정보 |
| volumes.encryption.enabled | Body | Boolean | NAS 스토리지 암호화 활성 여부 |
| volumes.encryption.keys | Body | List | NAS 스토리지 암호화 키 정보 |
| volumes.interfaces | Body | List | NAS 스토리지 인터페이스 객체 목록 |
| volumes.interfaces.id | Body | String | 인터페이스 ID |
| volumes.interfaces.path | Body | String | 인터페이스 경로 |
| volumes.interfaces.status | Body | String | 인터페이스 상태 |
| volumes.interfaces.subnetId | Body | String | 인터페이스의 서브넷 ID |
| volumes.interfaces.tenantId | Body | String | 인터페이스의 테넌트 ID |
| volumes.mirrors | Body | List | NAS 스토리지 복제 설정 객체 목록 |
| volumes.mirrors.id | Body | String | 복제 설정 ID |
| volumes.mirrors.role | Body | String | 복제 역할<br>- `SOURCE`: 원본 스토리지<br>- `DESTINATION`: 대상 스토리지 |
| volumes.mirrors.status | Body | String | 복제 설정 상태<br>- `INITIALIZED`: 설정 완료<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중 |
| volumes.mirrors.direction | Body | String | 복제 방향 <br>- `FORWARD`: 원본 스토리지 -> 복제 스토리지 <br>- `REVERSE`: 복제 스토리지 -> 원본 스토리지 |
| volumes.mirrors.directionChangedAt | Body | String | 복제 방향 변경 시각 |
| volumes.mirrors.dstProjectId | Body | String | 복제 대상 스토리지의 프로젝트 ID |
| volumes.mirrors.dstRegion | Body | String | 복제 대상 스토리지 리전 |
| volumes.mirrors.dstTenantId | Body | String | 복제 대상 스토리지 테넌트 ID |
| volumes.mirrors.dstVolumeId | Body | String | 복제 대상 스토리지의 NAS 스토리지 ID |
| volumes.mirrors.dstVolumeName | Body | String | 복제 대상 스토리지의 NAS 스토리지 이름 |
| volumes.mirrors.srcProjectId | Body | String | 원본 스토리지의 프로젝트 ID |
| volumes.mirrors.srcRegion | Body | String | 원본 스토리지 리전 |
| volumes.mirrors.srcTenantId | Body | String | 원본 스토리지 테넌트 ID |
| volumes.mirrors.srcVolumeId | Body | String | 원본 스토리지의 NAS 스토리지 ID |
| volumes.mirrors.srcVolumeName | Body | String | 원본 스토리지 NAS 스토리지 이름 |
| volumes.mirrors.createdAt | Body | String | 복제 생성 시각 |
| volumes.mountProtocol | Body | Object | NAS 스토리지 마운트 프로토콜 |
| volumes.mountProtocol.cifsAuthIds | Body | List | NAS 스토리지 CIFS 인증 ID 목록 |
| volumes.mountProtocol.protocol | Body | String | NAS 스토리지 마운트 프로토콜 |
| volumes.snapshotPolicy | Body | Object | NAS 스토리지 볼륨 스냅숏 설정 객체 |
| volumes.snapshotPolicy.maxScheduledCount | Body | Integer | 스냅숏 최대 저장 개수 |
| volumes.snapshotPolicy.reservePercent | Body | Integer | 스냅숏 용량 비율 |
| volumes.snapshotPolicy.schedule | Body | Object | 스냅숏 자동 생성 객체 |
| volumes.snapshotPolicy.schedule.time | Body | String | 스냅숏 자동 생성 시간 |
| volumes.snapshotPolicy.schedule.timeOffset | Body | String | 스냅숏 자동 생성 기준 시간대 |
| volumes.snapshotPolicy.schedule.weekdays | Body | List | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |
| volumes.createdAt | Body | String | NAS 스토리지 생성 시각 |
| volumes.updatedAt | Body | String | NAS 스토리지 변경 시각 |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":200,
      "resultMessage":"Success"
   },
   "paging":{
      "limit":50,
      "page":1,
      "totalCount":1
   },
   "volumes":[
      {
         "acl":[
            "10.0.1.0/24"
         ],
         "createdAt":"2025-04-01T06:44:25+00:00",
         "description":"NAS for Testing",
         "encryption":{
            "enabled":false
         },
         "id":"fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
         "interfaces":[
            {
               "id":"9a8ec90f-cc27-4649-9bda-a1f0b193a402",
               "path":"10.0.1.7:/TEST-NAS-1",
               "status":"ACTIVE",
               "subnetId":"cb779d62-72ef-43b6-b368-3fe28dcd812b",
               "tenantId":"3b6179e5fa6b499386b827357c4cb8c4"
            }
         ],
         "mirrors":[
            {
               "createdAt":"2025-04-01T06:45:45+00:00",
               "direction":"FORWARD",
               "directionChangedAt":null,
               "dstProjectId":"K3y0CgOy",
               "dstRegion":"KR2",
               "dstTenantId":"3b6179e5fa6b499386b827357c4cb8c4",
               "dstVolumeId":"e09281d2-0b1c-48a9-8a01-0098aa59f624",
               "dstVolumeName":"TEST-NAS-MIRROR-1",
               "id":"8116892c-7306-48be-9e3d-143311b2254c",
               "role":"SOURCE",
               "srcProjectId":"K3y0CgOy",
               "srcRegion":"KR1",
               "srcTenantId":"3b6179e5fa6b499386b827357c4cb8c4",
               "srcVolumeId":"fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
               "srcVolumeName":"TEST-NAS-1",
               "status":"INITIALIZED"
            }
         ],
         "mountProtocol":{
            "protocol":"nfs"
         },
         "name":"TEST-NAS-1",
         "projectId":"K3y0CgOy",
         "sizeGb":300,
         "snapshotPolicy":{
            "maxScheduledCount":1,
            "reservePercent":5,
            "schedule":{
               "time":"00:00",
               "timeOffset":"+09:00",
               "weekdays":[
                  
               ]
            }
         },
         "stationId":null,
         "status":"ACTIVE",
         "tenantId":"3b6179e5fa6b499386b827357c4cb8c4",
         "updatedAt":"2025-04-01T06:47:13+00:00"
      }
   ]
}
```

</details>

<br>

### NAS 스토리지 생성하기

새로운 NAS 스토리지를 생성합니다.

> [참고] CIFS 프로토콜 사용
> CIFS 프로토콜을 사용하기 위해서는 CIFS 인증 정보를 생성해야 합니다. 인증 정보는 프로젝트 단위로 관리되며, CIFS 스토리지마다 접근할 CIFS 인증 정보를 등록해야 합니다.
> CIFS 인증 정보는 콘솔의 **Storage > NAS > CIFS 인증 정보 관리** 창을 통해 생성할 수 있습니다.


```
POST  /v1/volumes
X-Auth-Token: {token-id}
```

<br>

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume | Body | Object | O | NAS 스토리지 생성 요청 객체 |
| volume.acl | Body | List | - | NAS 스토리지 생성 시 설정할 ACL ID 목록<br>IP 또는 CIDR 형식으로 입력할 수 있습니다. |
| volume.description | Body | String | - | NAS 스토리지 설명 |
| volume.interfaces | Body | List | - | NAS 스토리지에 접근할 인터페이스 목록 |
| volume.interfaces.subnetId | Body | String | - | NAS 스토리지 인터페이스의 서브넷 ID |
| volume.mountProtocol | Body | Object | - | NAS 스토리지 생성 시 프로토콜 설정 객체 |
| volume.mountProtocol.cifsAuthIds | Body | List | - | CIFS 인증 ID 목록<br>NFS 프로토콜 선택 시 입력 불필요 |
| volume.mountProtocol.protocol | Body | String | O | NAS 스토리지 마운트 시 프로토콜 지정<br>`nfs`, `cifs` 중 하나를 선택할 수 있습니다. |
| volume.name | Body | String | O | NAS 스토리지 이름 |
| volume.sizeGb | Body | Integer | O | NAS 스토리지 크기(GB)<br>NAS 스토리지는 최소 300GB에서 최대 10,000GB까지, 100GB 단위로 설정할 수 있습니다. |
| volume.snapshotPolicy | Body | Object | - | NAS 스토리지 볼륨 스냅숏 설정 객체 |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | 스냅숏 최대 저장 개수<br>30개까지 설정 가능하며, 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 만들어진 스냅숏이 삭제됩니다. |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | 스냅숏 용량 비율 |
| volume.snapshotPolicy.schedule | Body | Object | - | 스냅숏 자동 생성 객체<br>`null`일 경우 스냅숏 자동 생성이 설정되지 않습니다. |
| volume.snapshotPolicy.schedule.time | Body | String | - | 스냅숏 자동 생성 시간 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | 스냅숏 자동 생성 기준 시간대 |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | 스냅숏 자동 생성 요일. <br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |

<details>
  <summary>요청 예시</summary>

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
    "name": "TEST-NAS-2",
    "sizeGb": 300,
    "snapshotPolicy": {
      "maxScheduledCount": 20,
      "reservePercent": 10,
      "schedule": {
        "time": "03:00",
        "timeOffset": "+09:00",
        "weekdays": [1, 3, 5]
      }
    }
  }
}
```

</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volume | Body | Object | NAS 스토리지 객체 |
| volume.id | Body | String | NAS 스토리지 ID |
| volume.name | Body | String | NAS 스토리지 이름 |
| volume.status | Body | String | NAS 스토리지 상태 |
| volume.description | Body | String | NAS 스토리지 설명 |
| volume.sizeGb | Body | Integer | NAS 스토리지 크기(GB) |
| volume.projectId | Body | String | NAS 스토리지가 속한 프로젝트 ID |
| volume.tenantId | Body | String | NAS 스토리지가 속한 테넌트 ID |
| volume.acl | Body | List | NAS 스토리지 ACL 목록 |
| volume.encryption | Body | Object | NAS 스토리지 암호화 정보 |
| volume.encryption.enabled | Body | Boolean | NAS 스토리지 암호화 활성 여부 |
| volume.encryption.keys | Body | List | NAS 스토리지 암호화 키 정보 |
| volume.interfaces | Body | List | NAS 스토리지 인터페이스 객체 목록 |
| volume.interfaces.id | Body | String | 인터페이스 ID |
| volume.interfaces.path | Body | String | 인터페이스 경로 |
| volume.interfaces.status | Body | String | 인터페이스 상태 |
| volume.interfaces.subnetId | Body | String | 인터페이스의 서브넷 ID |
| volume.interfaces.tenantId | Body | String | 인터페이스의 테넌트 ID |
| volume.mirrors | Body | List | NAS 스토리지 복제 설정 객체 목록 |
| volume.mirrors.id | Body | String | 복제 설정 ID |
| volume.mirrors.role | Body | String | 복제 역할<br>- `SOURCE`: 원본 스토리지<br>- `DESTINATION`: 대상 스토리지 |
| volume.mirrors.status | Body | String | 복제 설정 상태<br>- `INITIALIZED`: 설정 완료<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중 |
| volume.mirrors.direction | Body | String | 복제 방향 <br>- `FORWARD`: 원본 스토리지 -> 복제 스토리지<br>- `REVERSE`: 복제 스토리지 -> 원본 스토리지 |
| volume.mirrors.directionChangedAt | Body | String | 복제 방향 변경 시각 |
| volume.mirrors.dstProjectId | Body | String | 복제 대상 스토리지의 프로젝트 ID |
| volume.mirrors.dstRegion | Body | String | 복제 대상 스토리지 리전 |
| volume.mirrors.dstTenantId | Body | String | 복제 대상 스토리지 테넌트 ID |
| volume.mirrors.dstVolumeId | Body | String | 복제 대상 스토리지의 NAS 스토리지 ID |
| volume.mirrors.dstVolumeName | Body | String | 복제 대상 스토리지의 NAS 스토리지 이름 |
| volume.mirrors.srcProjectId | Body | String | 원본 스토리지의 프로젝트 ID |
| volume.mirrors.srcRegion | Body | String | 원본 스토리지 리전 |
| volume.mirrors.srcTenantId | Body | String | 원본 스토리지 테넌트 ID |
| volume.mirrors.srcVolumeId | Body | String | 원본 스토리지의 NAS 스토리지 ID |
| volume.mirrors.srcVolumeName | Body | String | 원본 스토리지 NAS 스토리지 이름 |
| volume.mirrors.createdAt | Body | String | 복제 생성 시각 |
| volume.mountProtocol | Body | Object | NAS 스토리지 마운트 프로토콜 |
| volume.mountProtocol.cifsAuthIds | Body | List | NAS 스토리지 CIFS 인증 ID 목록 |
| volume.mountProtocol.protocol | Body | String | NAS 스토리지 마운트 프로토콜 |
| volume.snapshotPolicy | Body | Object | NAS 스토리지 볼륨 스냅숏 설정 객체 |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | 스냅숏 최대 저장 개수 |
| volume.snapshotPolicy.reservePercent | Body | Integer | 스냅숏 용량 비율 |
| volume.snapshotPolicy.schedule | Body | Object | 스냅숏 자동 생성 객체 |
| volume.snapshotPolicy.schedule.time | Body | String | 스냅숏 자동 생성 시간 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | 스냅숏 자동 생성 기준 시간대 |
| volume.snapshotPolicy.schedule.weekdays | Body | List | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |
| volume.createdAt | Body | String | NAS 스토리지 생성 시각 |
| volume.updatedAt | Body | String | NAS 스토리지 변경 시각 |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":200,
      "resultMessage":"Success"
   },
   "paging":{
      "limit":50,
      "page":1,
      "totalCount":1
   },
   "volumes":[
      {
         "acl":[
            "10.0.1.0/24"
         ],
         "createdAt":"2025-04-01T06:44:25+00:00",
         "description":"NAS for Testing",
         "encryption":{
            "enabled":false
         },
         "id":"fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
         "interfaces":[
            {
               "id":"9a8ec90f-cc27-4649-9bda-a1f0b193a402",
               "path":"10.0.1.7:/TEST-NAS-1",
               "status":"ACTIVE",
               "subnetId":"cb779d62-72ef-43b6-b368-3fe28dcd812b",
               "tenantId":"3b6179e5fa6b499386b827357c4cb8c4"
            }
         ],
         "mirrors":[
            {
               "createdAt":"2025-04-01T06:45:45+00:00",
               "direction":"FORWARD",
               "directionChangedAt":null,
               "dstProjectId":"K3y0CgOy",
               "dstRegion":"KR2",
               "dstTenantId":"3b6179e5fa6b499386b827357c4cb8c4",
               "dstVolumeId":"e09281d2-0b1c-48a9-8a01-0098aa59f624",
               "dstVolumeName":"TEST-NAS-MIRROR-1",
               "id":"8116892c-7306-48be-9e3d-143311b2254c",
               "role":"SOURCE",
               "srcProjectId":"K3y0CgOy",
               "srcRegion":"KR1",
               "srcTenantId":"3b6179e5fa6b499386b827357c4cb8c4",
               "srcVolumeId":"fc8b111a-32b7-45d3-b123-ff3ecaaf768a",
               "srcVolumeName":"TEST-NAS-1",
               "status":"INITIALIZED"
            }
         ],
         "mountProtocol":{
            "protocol":"nfs"
         },
         "name":"TEST-NAS-1",
         "projectId":"K3y0CgOy",
         "sizeGb":300,
         "snapshotPolicy":{
            "maxScheduledCount":1,
            "reservePercent":5,
            "schedule":{
               "time":"00:00",
               "timeOffset":"+09:00",
               "weekdays":[]
            }
         },
         "stationId":null,
         "status":"ACTIVE",
         "tenantId":"3b6179e5fa6b499386b827357c4cb8c4",
         "updatedAt":"2025-04-01T06:47:13+00:00"
      }
   ]
}
```

</details>

<br>

### NAS 스토리지 삭제하기

지정한 NAS 스토리지를 삭제합니다.

```
DELETE  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | 삭제할 NAS 스토리지 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### NAS 스토리지 보기

지정한 NAS 스토리지의 상세 정보를 반환합니다.

```
GET   /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | 조회할 NAS 스토리지 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volume | Body | Object | NAS 스토리지 객체 |
| volume.id | Body | String | NAS 스토리지 ID |
| volume.name | Body | String | NAS 스토리지 이름 |
| volume.status | Body | String | NAS 스토리지 상태 |
| volume.description | Body | String | NAS 스토리지 설명 |
| volume.sizeGb | Body | Integer | NAS 스토리지 크기(GB) |
| volume.projectId | Body | String | NAS 스토리지가 속한 프로젝트 ID |
| volume.tenantId | Body | String | NAS 스토리지가 속한 테넌트 ID |
| volume.acl | Body | List | NAS 스토리지 ACL 목록 |
| volume.encryption | Body | Object | NAS 스토리지 암호화 정보 |
| volume.encryption.enabled | Body | Boolean | NAS 스토리지 암호화 활성 여부 |
| volume.encryption.keys | Body | List | NAS 스토리지 암호화 키 정보 |
| volume.interfaces | Body | List | NAS 스토리지 인터페이스 객체 목록 |
| volume.interfaces.id | Body | String | 인터페이스 ID |
| volume.interfaces.path | Body | String | 인터페이스 경로 |
| volume.interfaces.status | Body | String | 인터페이스 상태 |
| volume.interfaces.subnetId | Body | String | 인터페이스의 서브넷 ID |
| volume.interfaces.tenantId | Body | String | 인터페이스의 테넌트 ID |
| volume.mirrors | Body | List | NAS 스토리지 복제 설정 객체 목록 |
| volume.mirrors.id | Body | String | 복제 설정 ID |
| volume.mirrors.role | Body | String | 복제 역할<br>- `SOURCE`: 원본 스토리지<br>- `DESTINATION`: 대상 스토리지 |
| volume.mirrors.status | Body | String | 복제 설정 상태<br>- `INITIALIZED`: 설정 완료<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중 |
| volume.mirrors.direction | Body | String | 복제 방향 <br>- `FORWARD`: 원본 스토리지 -> 복제 스토리지<br>- `REVERSE`: 복제 스토리지 -> 원본 스토리지 |
| volume.mirrors.directionChangedAt | Body | String | 복제 방향 변경 시각 |
| volume.mirrors.dstProjectId | Body | String | 복제 대상 스토리지의 프로젝트 ID |
| volume.mirrors.dstRegion | Body | String | 복제 대상 스토리지 리전 |
| volume.mirrors.dstTenantId | Body | String | 복제 대상 스토리지 테넌트 ID |
| volume.mirrors.dstVolumeId | Body | String | 복제 대상 스토리지의 NAS 스토리지 ID |
| volume.mirrors.dstVolumeName | Body | String | 복제 대상 스토리지의 NAS 스토리지 이름 |
| volume.mirrors.srcProjectId | Body | String | 원본 스토리지의 프로젝트 ID |
| volume.mirrors.srcRegion | Body | String | 원본 스토리지 리전 |
| volume.mirrors.srcTenantId | Body | String | 원본 스토리지 테넌트 ID |
| volume.mirrors.srcVolumeId | Body | String | 원본 스토리지의 NAS 스토리지 ID |
| volume.mirrors.srcVolumeName | Body | String | 원본 스토리지 NAS 스토리지 이름 |
| volume.mirrors.createdAt | Body | String | 복제 생성 시각 |
| volume.mountProtocol | Body | Object | NAS 스토리지 마운트 프로토콜 |
| volume.mountProtocol.cifsAuthIds | Body | List | NAS 스토리지 CIFS 인증 ID 목록 |
| volume.mountProtocol.protocol | Body | String | NAS 스토리지 마운트 프로토콜 |
| volume.snapshotPolicy | Body | Object | NAS 스토리지 볼륨 스냅숏 설정 객체 |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | 스냅숏 최대 저장 개수 |
| volume.snapshotPolicy.reservePercent | Body | Integer | 스냅숏 용량 비율 |
| volume.snapshotPolicy.schedule | Body | Object | 스냅숏 자동 생성 객체 |
| volume.snapshotPolicy.schedule.time | Body | String | 스냅숏 자동 생성 시간 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | 스냅숏 자동 생성 기준 시간대 |
| volume.snapshotPolicy.schedule.weekdays | Body | List | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |
| volume.createdAt | Body | String | NAS 스토리지 생성 시각 |
| volume.updatedAt | Body | String | NAS 스토리지 변경 시각 |

<br>

### NAS 스토리지 설정 변경하기

지정한 NAS 스토리지의 설정을 변경합니다.

> [주의]
> 복제 설정된 스토리지의 크기를 변경하려면 원본 스토리지와 대상 스토리지 모두 변경해야 합니다. 원본 스토리지와 대상 스토리지의 크기가 다른 경우 복제에 실패할 수 있습니다.

```
PATCH  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| volume | Body | Object | O | NAS 스토리지 생성 요청 객체 |
| volume.acl | Body | List | - | NAS 스토리지 생성 시 설정할 ACL ID들의 목록<br>IP 또는 CIDR 형식으로 입력할 수 있습니다. |
| volume.description | Body | String | - | NAS 스토리지 설명 |
| volume.mountProtocol | Body | Object | - | NAS 스토리지 생성 시 프로토콜 설정 객체 |
| volume.mountProtocol.cifsAuthIds | Body | List | - | CIFS 인증 ID 목록 |
| volume.mountProtocol.protocol | Body | String | - | 이미 생성된 NAS 스토리지의 프로토콜은 변경할 수 없습니다.<br>`cifsAuthIds` 필드 변경 시 해당 필드에 `cifs`를 명시해야 합니다. |
| volume.sizeGb | Body | Integer | O | NAS 스토리지 크기(GB)<br>NAS 스토리지는 최소 300GB에서 최대 10,000GB까지, 100GB 단위로 설정할 수 있습니다. |
| volume.snapshotPolicy | Body | Object | - | NAS 스토리지 볼륨 스냅숏 설정 객체 |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | 스냅숏 최대 저장 개수<br>30개까지 설정 가능하며, 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 만들어진 스냅숏이 삭제됩니다. |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | 스냅숏 용량 비율 |
| volume.snapshotPolicy.schedule | Body | Object | - | 스냅숏 자동 생성 객체<br>`null` 일 경우 스냅숏 자동 생성이 설정되지 않습니다. |
| volume.snapshotPolicy.schedule.time | Body | String | - | 스냅숏 자동 생성 시간 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | 스냅숏 자동 생성 기준 시간대 |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | 스냅숏 자동 생성 요일.<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |

<details>
  <summary>요청 예시</summary>

```json
{
   "volume":{
      "acl":[
         "10.0.1.0/24"
      ],
      "description":"Modified description",
      "mountProtocol":{
         "cifsAuthIds":[
            "cifs-test-id"
         ],
         "protocol":"cifs"
      },
      "sizeGb":300,
      "snapshotPolicy":{
         "maxScheduledCount":10,
         "reservePercent":20,
         "schedule":{
            "time":"05:00",
            "timeOffset":"+09:00",
            "weekdays":[
               2,
               4
            ]
         }
      }
   }
}
```

</details>

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### NAS 스토리지에 인터페이스 연결하기

지정한 NAS 스토리지의 인터페이스를 설정합니다.
설정된 주소 및 서브넷에서 NAS 스토리지에 접근 가능합니다. 접근 가능한 IP 설정은 접근 제어(ACL) 설정에서 별도 설정해야 합니다.

```
POST  /v1/volumes/{volume_id}/interfaces
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| interface | Body | Object | O | 인터페이스 설정 객체 |
| interface.subnetId | Body | String | O | 인터페이스 서브넷 지정 |

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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| interface | Body | Object | 생성된 인터페이스 정보 객체 |
| interface.id | Body | String | 생성된 인터페이스 ID |
| interface.path | Body | String | 생성된 인터페이스 경로 |
| interface.status | Body | String | 생성된 인터페이스 상태 |
| interface.subnetId | Body | String | 생성된 인터페이스의 서브넷 ID |
| interface.tenentId | Body | String | 생성된 인터페이스의 테넌트 ID |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":201,
      "resultMessage":"Created"
   },
   "interface":{
      "id":"e7c6a340-6889-445b-ae2f-4e237b9afc9e",
      "path":null,
      "status":"BUILDING",
      "subnetId":"3e5b4d63-d143-420a-9263-208a447a2a3f",
      "tenantId":"3b6179e5fa6b499386b827357c4cb8c4"
   }
}
```

</details>

<br>

### NAS 스토리지의 인터페이스 삭제하기

지정한 NAS 스토리지의 지정한 인터페이스를 삭제합니다.

```
DELETE  /v1/volumes/{volume_id}/interfaces/{interface_id}
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| interface\_id | URL | String | O | 삭제할 인터페이스 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### 스냅숏 복원 내역 보기

지정한 스토리지의 스냅숏 복원 내역 목록을 반환합니다.

```
GET  /v1/volumes/{volume_id}/restore-histories
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| limit | String | Query | X | 한 페이지에 노출할 리소스 개수 |
| page | String | Query | X | 조회할 페이지 |
| sort | String | Query | X | 정렬 기준이 될 필드 이름<br>`{key}:{direction}` 형태로 기술합니다. 예: `snapshotId:asc`, `requestedAt:desc`<br>사용 가능한 key 값: `snapshotId`, `snapshotName`, `requestedAt`, `restoredAt`, `requestedUser`, `requestedIp`, `result` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| paging | Body | Object | 페이지 정보 |
| paging.limit | Body | Integer | 한 페이지에 노출되는 리소스 개수 |
| paging.page | Body | Integer | 현재 페이지 번호 |
| paging.totalCount | Body | Integer | 전체 수 |
| restoreHistories | Body | List | 스냅숏 복원 내역 객체 목록 |
| restoreHistories.requestedAt | Body | String | 스냅숏 복원을 요청한 시각 |
| restoreHistories.requestedIp | Body | String | 스냅숏 복원을 요청한 주소 |
| restoreHistories.requestedUser | Body | String | 스냅숏 복원을 요청한 사용자 ID |
| restoreHistories.restoredAt | Body | String | 스냅숏 복원을 완료한 시각 |
| restoreHistories.result | Body | String | 스냅숏 복원 결과 |
| restoreHistories.snapshotId | Body | String | 복원 대상 스냅숏 ID |
| restoreHistories.snapshotName | Body | String | 복원 대상 스냅숏 이름 |
| restoreHistories.volumeId | Body | String | 복원한 NAS 스토리지의 ID |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":200,
      "resultMessage":"Success"
   },
   "paging":{
      "limit":50,
      "page":1,
      "totalCount":1
   },
   "restoreHistories":[
      {
         "requestedAt":"2025-04-01T08:29:28+00:00",
         "requestedIp":"10.163.23.45",
         "requestedUser":"14025c4b-cc93-4f97-9416-a8001cc771c1",
         "restoredAt":"2025-04-01T08:29:34+00:00",
         "result":"SUCCESS",
         "snapshotId":"5e9745a5-0ed3-11f0-b0e3-d039eaa3e920",
         "snapshotName":"TEST-SNAPSHOT-IMM-1",
         "volumeId":"70787a7e-605b-4447-b950-46aa3297e0ed"
      }
   ]
}
```

</details>

<br>

### NAS 스토리지 사용 현황 보기

지정한 NAS 스토리지의 사용 현황을 반환합니다.

```
GET  /v1/volumes/{volume_id}/usage
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| usage | Body | Object | NAS 스토리지 사용 현황 객체 |
| usage.snapshotReserveGb | Body | Integer | NAS 스토리지에서 스냅숏을 위해 예약한 공간 크기 |
| usage.usedGb | Body | Integer | NAS 스토리지 사용량 |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":200,
      "resultMessage":"Success"
   },
   "usage":{
      "snapshotReserveGb":30,
      "usedGb":2
   }
}
```

</details>

<br>

## Snapshots

### 스냅숏 목록 보기

스냅숏 목록을 조회합니다.

```
GET  /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| snapshots | Body | List | 스냅숏 정보 객체 목록 |
| snapshots.createdAt | Body | String | 스냅숏 생성 시각 |
| snapshots.id | Body | String | 스냅숏 ID |
| snapshots.name | Body | String | 스냅숏 이름 |
| snapshots.preserved | Body | Boolean | 시스템에 의해 삭제 불가 설정된 스냅숏 여부 |
| snapshots.size | Body | Integer | 스냅숏 크기 |
| snapshots.type | Body | String | 스냅숏 타입<br>- `NORMAL`: 사용자에 의해 생성된 스냅숏<br>- `SCHEDULED`: 스냅숏 자동 생성에 의해 생성된 스냅숏<br>- `MIRROR`: 복제로 인해 생성된 스냅숏 |

<details><summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":200,
      "resultMessage":"Success"
   },
   "snapshots":[
      {
         "createdAt":"2025-04-01T09:34:27+00:00",
         "id":"8151fe33-0edc-11f0-b0e3-d039eaa3e920",
         "name":"TEST-SNAPSHOT-1",
         "preserved":false,
         "size":3112960,
         "type":"NORMAL"
      },
      {
         "createdAt":"2025-04-01T09:35:00+00:00",
         "id":"00904f26-9cff-4131-a4d7-96f6a89e4ae7",
         "name":"TEST-NAS-1.mirror.2025-04-01_183500",
         "preserved":true,
         "size":3133440,
         "type":"MIRROR"
      }
   ]
}
```
</details>

<br>

### 스냅숏 생성하기

지정한 NAS 스토리지의 스냅숏을 생성합니다.

```
POST  /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| snapshot | Body | Object | O | 스냅숏 생성 객체 |
| snapshot.name | Body | String | O | 스냅숏 이름 |

<details>
  <summary>요청 예시</summary>

```json
{
   "snapshot":{
      "name":"TEST-SNAPSHOT-2"
   }
}
```

</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| snapshot | Body | List | 스냅숏 정보 객체 |
| snapshot.id | Body | String | 스냅숏 ID |
| snapshot.name | Body | String | 스냅숏 이름 |
| snapshot.preserved | Body | Boolean | 시스템에 의해 삭제 불가 설정된 스냅숏 여부 |
| snapshot.reclaimableSpace | Body | Integer | 스냅숏 삭제 시 확보되는 용량 |
| snapshot.type | Body | String | 스냅숏 타입<br>- `NORMAL`: 사용자에 의해 생성된 스냅숏<br>- `SCHEDULED`: 스냅숏 자동 생성에 의해 생성된 스냅숏<br>- `MIRROR`: 복제로 인해 생성된 스냅숏 |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":201,
      "resultMessage":"Created"
   },
   "snapshot":{
      "id":"0dc959d5-0edd-11f0-b0e3-d039eaa3e920",
      "name":"TEST-SNAPSHOT-2",
      "preserved":false,
      "type":"NORMAL"
   }
}
```

</details>

<br>

### 스냅숏 삭제하기

지정한 NAS 스토리지의 스냅숏을 삭제합니다.

```
DELETE  /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| snapshot\_id | URL | String | O | 스냅숏 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### 스냅숏 보기

지정한 스냅숏의 상세 정보를 반환합니다.

```
GET  /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| snapshot\_id | URL | String | O | 스냅숏 ID |
| showReclaimableSpace | Query | Boolean | - | 스냅숏 삭제 시 확보되는 용량을 나타내는 `reclaimableSpace` 항목 노출 여부 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| snapshot | Body | List | 스냅숏 정보 객체 |
| snapshot.createdAt | Body | String | 스냅숏 생성 시각 |
| snapshot.id | Body | String | 스냅숏 ID |
| snapshot.name | Body | String | 스냅숏 이름 |
| snapshot.preserved | Body | Boolean | 시스템에 의해 삭제 불가 설정된 스냅숏 여부 |
| snapshot.reclaimableSpace | Body | Integer | 스냅숏 삭제 시 확보되는 용량 |
| snapshot.size | Body | Integer | 스냅숏 크기 |
| snapshot.type | Body | String | 스냅숏 타입<br>- `NORMAL`: 사용자에 의해 생성된 스냅숏<br>- `SCHEDULED` : 스냅숏 자동 생성에 의해 생성된 스냅숏<br>- `MIRROR`: 복제로 인해 생성된 스냅숏 |

<br>

### 스냅숏 복원하기

지정한 스냅숏으로 NAS 스토리지를 복원합니다.

```
POST  /v1/volumes/{volume_id}/snapshots/{snapshot_id}/restore
X-Auth-Token: {token-id}
```

#### 요청

요청 본문은 필요하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| snapshot\_id | URL | String | O | 스냅숏 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

## NAS 스토리지 복제 설정

### 복제 설정하기

지정한 NAS 스토리지의 복제를 설정합니다.
복제 대상 프로젝트별 선택 가능한 리전 범위는 아래 표를 통해 확인할 수 있습니다.

| 대상 프로젝트 | 선택 가능한 리전 |
| ------- | --------- |
| 조직 내 동일 프로젝트 | 다른 리전 |
| 조직 내 다른 프로젝트 | 모든 리전 |

<br>

> [주의]
> 복제 대상 스토리지 크기는 원본 스토리지와 동일하게 설정해야 합니다. 원본 스토리지와 대상 스토리지의 크기가 다른 경우 복제에 실패할 수 있습니다.


<!-- 개행을 위한 주석 -->

> [참고] 
> 원본 스토리지가 CIFS 프로토콜를 사용하는 경우 대상 스토리지도 CIFS 프로토콜을 사용해야 합니다. 이를 위해 원본 스토리지와는 별개의 CIFS 인증 정보를 생성하여 요청 본문 `cifsAuthIds` 필드에 입력해야 합니다.


```
POST  /v1/volumes/{volume_id}/volume-mirrors
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | 원본 NAS 스토리지 ID |
| volumeMirror | Body | Object | O | NAS 스토리지 복제 설정 요청 객체 |
| volumeMirror.dstRegion | Body | String | O | 복제 대상 스토리지의 리전 |
| volumeMirror.dstTenantId | Body | String | O | 복제 대상 스토리지의 테넌트 ID |
| volumeMirror.dstVolume | Body | Object | O | 복제 대상 스토리지 생성 요청 객체 |
| volumeMirror.dstVolume.acl | Body | List | - | NAS 스토리지 생성 시 설정할 ACL ID 목록<br>IP 또는 CIDR 형식으로 입력할 수 있습니다. |
| volumeMirror.dstVolume.description | Body | String | - | NAS 스토리지 설명 |
| volumeMirror.dstVolume.interfaces | Body | List | - | NAS 스토리지에 접근할 인터페이스 목록 |
| volumeMirror.dstVolume.interfaces.subnetId | Body | String | - | NAS 스토리지 인터페이스의 서브넷 ID |
| volumeMirror.dstVolume.mountProtocol | Body | Object | - | NAS 스토리지 생성 시 프로토콜 설정 객체 |
| volumeMirror.dstVolume.mountProtocol.cifsAuthIds | Body | List | - | CIFS 인증 ID 목록<br>NFS 프로토콜 선택 시 입력 불필요 |
| volumeMirror.dstVolume.mountProtocol.protocol | Body | String | O | NAS 스토리지 마운트 시 프로토콜 지정<br>`nfs`, `cifs` 중 하나를 선택할 수 있습니다. |
| volumeMirror.dstVolume.name | Body | String | O | NAS 스토리지 이름 |
| volumeMirror.dstVolume.sizeGb | Body | Integer | O | NAS 스토리지 크기(GB)<br>NAS 스토리지는 최소 300GB에서 최대 10,000GB까지, 100GB 단위로 설정할 수 있습니다. |
| volumeMirror.dstVolume.snapshotPolicy | Body | Object | - | NAS 스토리지 볼륨 스냅숏 설정 객체 |
| volumeMirror.dstVolume.snapshotPolicy.maxScheduledCount | Body | Integer | - | 스냅숏 최대 저장 개수<br>30개까지 설정 가능하며, 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 만들어진 스냅숏이 삭제됩니다. |
| volumeMirror.dstVolume.snapshotPolicy.reservePercent | Body | Integer | - | 스냅숏 용량 비율 |
| volumeMirror.dstVolume.snapshotPolicy.schedule | Body | Object | - | 스냅숏 자동 생성 객체<br>`null`일 경우 스냅숏 자동 생성이 설정되지 않습니다. |
| volumeMirror.dstVolume.snapshotPolicy.schedule.time | Body | String | - | 스냅숏 자동 생성 시간 |
| volumeMirror.dstVolume.snapshotPolicy.schedule.timeOffset | Body | String | - | 스냅숏 자동 생성 기준 시간대 |
| volumeMirror.dstVolume.snapshotPolicy.schedule.weekdays | Body | List | - | 스냅숏 자동 생성 요일.<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |

<details>
  <summary>요청 예시</summary>

```json
{
   "volumeMirror":{
      "dstRegion":"KR1",
      "dstTenantId":"7debf04e6a7248c98777229bcb004b69",
      "dstVolume":{
         "description":"Volume Mirror Test",
         "mountProtocol":{
            "protocol":"nfs"
         },
         "name":"TEST-NAS-MIRROR",
         "sizeGb":300
      }
   }
}
```

</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volumeMirror | Body | Object | 복제 설정 생성 객체 |
| volumeMirror.id | Body | String | 복제 설정 ID |
| volumeMirror.role | Body | String | 복제 역할<br>- `SOURCE`: 원본 스토리지<br>- `DESTINATION`: 대상 스토리지 |
| volumeMirror.status | Body | String | 복제 설정 상태<br>- `INITIALIZED`: 설정 완료<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중 |
| volumeMirror.direction | Body | String | 복제 방향 <br>- `FORWARD`: 원본 스토리지 -> 복제 스토리지<br>- `REVERSE`: 복제 스토리지 -> 원본 스토리지 |
| volumeMirror.directionChangedAt | Body | String | 복제 방향 변경 시각 |
| volumeMirror.dstProjectId | Body | String | 복제 대상 스토리지의 프로젝트 ID |
| volumeMirror.dstRegion | Body | String | 복제 대상 스토리지 리전 |
| volumeMirror.dstTenantId | Body | String | 복제 대상 스토리지 테넌트 ID |
| volumeMirror.dstVolumeId | Body | String | 복제 대상 스토리지의 NAS 스토리지 ID |
| volumeMirror.dstVolumeName | Body | String | 복제 대상 스토리지의 NAS 스토리지 이름 |
| volumeMirror.srcProjectId | Body | String | 원본 스토리지의 프로젝트 ID |
| volumeMirror.srcRegion | Body | String | 원본 스토리지 리전 |
| volumeMirror.srcTenantId | Body | String | 원본 스토리지 테넌트 ID |
| volumeMirror.srcVolumeId | Body | String | 원본 스토리지의 NAS 스토리지 ID |
| volumeMirror.srcVolumeName | Body | String | 원본 스토리지 NAS 스토리지 이름 |
| volumeMirror.createdAt | Body | String | 복제 생성 시각 |

<details>
  <summary>응답 예시</summary>

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":201,
      "resultMessage":"Created"
   },
   "volumeMirror":{
      "createdAt":"2025-04-02T00:21:37+00:00",
      "direction":"FORWARD",
      "directionChangedAt":null,
      "dstProjectId":"7c5dVmxI",
      "dstRegion":"KR1",
      "dstTenantId":"7debf04e6a7248c98777229bcb004b69",
      "dstVolumeId":null,
      "dstVolumeName":null,
      "id":"f581af37-4b43-4c93-9478-0dbcad382641",
      "role":"SOURCE",
      "srcProjectId":"K3y0CgOy",
      "srcRegion":"KR1",
      "srcTenantId":"3b6179e5fa6b499386b827357c4cb8c4",
      "srcVolumeId":"70787a7e-605b-4447-b950-46aa3297e0ed",
      "srcVolumeName":"TEST-NAS-2",
      "status":"PENDING"
   }
}
```

</details>

<br>

### 복제 설정 해제하기

지정한 NAS 스토리지의 복제 설정을 해제합니다.

```
DELETE  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| volume\_mirror\_id | URL | String | O | 복제 설정 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### 복제 방향 변경하기

원본 스토리지와 대상 스토리지의 복제 방향을 변경합니다.

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/invert-direction
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| volume\_mirror\_id | URL | String | O | 복제 설정 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### 복제 시작하기

원본 스토리지에서 대상 스토리지로의 복제를 시작합니다.

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/start
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| volume\_mirror\_id | URL | String | O | 복제 설정 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>

### 복제 상태 확인하기

가장 최근의 복제 상태를 반환합니다.

```
GET  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stat
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| volume\_mirror\_id | URL | String | O | 복제 설정 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object | 헤더 객체 |
| volumeMirrorStat | Body | Object | 복제 상태 객체 |
| volumeMirrorStat.lastSuccessTransferBytes | Body | Integer | 최근 성공한 복제에서 전송된 데이터 크기(Byte) |
| volumeMirrorStat.lastSuccessTransferEndTime | Body | String | 최근 성공한 복제 완료 시간 |
| volumeMirrorStat.lastTransferBytes | Body | Integer | 최근 실행한 복제에서 전송된 데이터 크기(Byte) |
| volumeMirrorStat.lastTransferEndTime | Body | String | 최근 실행한 복제 완료 시간 |
| volumeMirrorStat.lastTransferStatus | Body | String | 최근 복제 실행 결과 |
| volumeMirrorStat.status | Body | String | 복제 설정 상태<br>- `ACTIVE`: 복제 활성화 상태<br>- `UPDATING`: 설정 변경 중<br>- `DELETING`: 설정 삭제 중<br>- `PENDING`: 설정 생성 중 <br>- `HALT`: 복제 중지 상태<br>- `RETRIEVE FAILED`: 일시적인 정보 획득 실패 |

<br>

### 복제 중지하기

원본 스토리지에서 대상 스토리지로의 복제를 중지합니다.

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stop
X-Auth-Token: {token-id}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | 토큰 ID |
| volume\_id | URL | String | O | NAS 스토리지 ID |
| volume\_mirror\_id | URL | String | O | 복제 설정 ID |

#### 응답

응답 본문에는 헤더 필드 외의 내용이 포함되지 않습니다.

<br>