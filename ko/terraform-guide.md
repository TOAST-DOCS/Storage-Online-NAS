## Storage > NAS > Terraform 사용 가이드

이 문서는 Terraform으로 NHN Cloud NAS 서비스를 사용하는 방법을 설명합니다.

<a id="terraform"></a>
## Terraform

Terraform은 인프라를 손쉽게 구축하고 안전하게 변경하며, 효율적으로 형상을 관리할 수 있는 오픈 소스 도구입니다. 기본적인 사용법은 [사용자 가이드 > NHN Cloud > Terraform 사용 가이드](/nhncloud/ko/terraform-guide/)를 참고하세요.

<a id="terraform-resource-dependency"></a>
### 리소스 의존성

일반적으로 각 리소스는 독립적이지만 다른 특정 리소스에 의존성을 가질 수도 있습니다. 리소스 레이블로 다른 리소스의 정보를 참조하면 Terraform은 자동으로 의존성을 설정합니다.
예를 들어, `volume1` 볼륨에 연결되는 `interface1` 인터페이스는 다음과 같이 표현할 수 있습니다.

```hcl
# 볼륨 리소스
resource "nhncloud_nas_storage_volume_v1" "volume1" {
  name = "volume1"
  size_gb = 300

  mount_protocol {
    protocol = "nfs"
  }
}

# 인터페이스 리소스
resource "nhncloud_nas_storage_volume_interface_v1" "interface1" {
  volume_id = nhncloud_nas_storage_volume_v1.volume1.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
}
```

> [참고]
> 명시적인 리소스 의존성 지정 방법은 [Terraform의 Resource dependencies](https://developer.hashicorp.com/terraform/tutorials/configuration-language/dependencies) 문서를 참고하세요.

<a id="terraform-resources-nas"></a>
## Resources

<a id="terraform-resources-create-volume"></a>
### 볼륨 생성하기

> [참고] CIFS 프로토콜 사용
> CIFS 프로토콜을 사용하려면 CIFS 인증 정보를 생성해야 합니다. 인증 정보는 프로젝트 단위로 관리되며, CIFS 볼륨마다 접근을 허용할 CIFS 인증 정보를 등록해야 합니다.
> CIFS 인증 정보는 콘솔의 **Storage > NAS > CIFS 인증 정보 관리** 창에서 생성할 수 있습니다.


<!-- -->

> [참고] 암호화 키 저장소 설정
> 암호화 볼륨을 생성하면 암호화에 사용하는 대칭 키가 NHN Cloud Secure Key Manager 서비스의 키 저장소에 저장됩니다. 따라서 암호화 볼륨을 만들려면 미리 Secure Key Manager 서비스에서 [키 저장소를 생성](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_1)해야 합니다. [키 저장소의 ID를 확인](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_2)하여 암호화 키 저장소 설정에 입력합니다.
> 생성한 키 저장소 ID는 콘솔의 **Storage > NAS > 암호화 키 저장소 설정** 창에서 입력할 수 있습니다. 암호화 볼륨을 생성하면 설정한 키 저장소에 대칭 키가 저장됩니다. 키 저장소에 저장된 대칭 키는 암호화 볼륨 사용 중에는 삭제할 수 없습니다. 암호화 볼륨을 삭제하면 대칭 키도 함께 삭제됩니다.
> 키 저장소 ID를 변경하면 이후 생성하는 암호화 볼륨의 대칭 키가 변경된 키 저장소에 저장됩니다. 기존 키 저장소에 저장된 대칭 키는 유지됩니다.


```hcl
# NFS 프로토콜의 빈 NAS 볼륨 생성
resource "nhncloud_nas_storage_volume_v1" "volume_01" {
  name = "nas_volume_01"
  size_gb = 300
  mount_protocol {
    protocol = "nfs"
  }
}

# CIFS 프로토콜의 빈 NAS 볼륨 생성
resource "nhncloud_nas_storage_volume_v1" "volume_02" {
  name = "nas_volume_02"
  size_gb = 300
  mount_protocol {
    protocol = "cifs"
    cifs_auth_ids = ["auth_id"]
  }
}

# ACL, 암호화 설정 등의 설정을 포함한 볼륨 생성하기
resource "nhncloud_nas_storage_volume_v1" "volume_03" {
  name = "nas_volume_03"
  description = "create nas volume by terraform"
  size_gb = 300

  acl = ["10.10.10.0/24"]

  encryption {
    enabled = true
  }

  mount_protocol {
    protocol = "cifs"
    cifs_auth_ids = ["auth_id"]
  }

  snapshot_policy {
    max_scheduled_count = 3
    reserve_percent = 10

    schedule {
      time = "00:00"
      time_offset = "+09:00"
      weekdays = [1, 3, 5]
    }
  }
}
```

| 이름 | 타입 | 필수 | 변경 가능 | 설명 |
| --- | --- | --- | --- | --- |
| region | String | - | - | 생성할 볼륨의 리전<br>기본값은 공급자 설정 파일에 설정된 리전 |
| name | String | O | - | 볼륨 이름 |
| description | String | - | O | 볼륨 설명 |
| size_gb | Integer | O | O | 볼륨 크기(GB)<br>볼륨은 최소 300GB에서 최대 10,000GB까지, 100GB 단위로 설정할 수 있습니다. |
| acl | List | - | O | 볼륨 생성 시 설정할 ACL 목록<br>IP 또는 CIDR 형식으로 입력할 수 있습니다. |
| encryption | Object | - | - | 볼륨 생성 시 암호화 설정 객체 |
| encryption.enabled | Boolean | - | - | 암호화 설정 활성화 여부<br>암호화 키 저장소가 설정된 후 해당 필드를 `true`로 설정하면 암호화가 활성화됩니다. |
| mount_protocol | Object | - | - | 볼륨 생성 시 프로토콜 설정 객체 |
| mount_protocol.cifs_auth_ids | List(String) | - | O | CIFS 인증 ID 목록<br>NFS 프로토콜 선택 시 입력 불필요 |
| mount_protocol.protocol | String | O | - | 볼륨 마운트 시 프로토콜 지정<br>`nfs`, `cifs` 중 하나를 선택할 수 있습니다. |
| snapshot_policy | Object | - | - | 볼륨 스냅숏 설정 객체 |
| snapshot_policy.max_scheduled_count | Integer | - | O | 스냅숏 최대 저장 개수<br>30개까지 설정 가능하며, 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 만들어진 스냅숏이 삭제됩니다. |
| snapshot_policy.reserve_percent | Integer | - | O | 스냅숏 용량 비율 |
| snapshot_policy.schedule | Object | - | - | 스냅숏 자동 생성 객체<br>`null`일 경우 스냅숏 자동 생성이 설정되지 않습니다. |
| snapshot_policy.schedule.time | String | - | O | 스냅숏 자동 생성 시간 |
| snapshot_policy.schedule.time_offset | String | - | O | 스냅숏 자동 생성 기준 시간대 |
| snapshot_policy.schedule.weekdays | List | - | O | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |

<a id="terraform-resources-connect-interface"></a>
### 볼륨에 인터페이스 연결하기

```hcl
data "nhncloud_networking_vpcsubnet_v2" "default_subnet" {
  ...
}

resource "nhncloud_nas_storage_volume_interface_v1" "nas_interface_01" {
  volume_id = nhncloud_nas_storage_volume_v1.volume_01.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
}
```

| 이름 | 타입 | 필수 | 변경 가능 | 설명 |
| --- | --- | --- | --- | --- |
| region | String | - | - | 연결할 볼륨의 리전<br>기본값은 공급자 설정 파일에 설정된 리전 |
| volume_id | String | O | - | 연결할 볼륨의 ID |
| subnet_id | String | O | - | 연결할 서브넷 ID |

<a id="terraform-resources-set-replication"></a>
### 복제 설정하기

복제 설정 리소스를 생성하면 대상 볼륨이 자동으로 생성됩니다.
복제 설정 리소스에서 `dst_volume`의 설정값을 변경하여 대상 볼륨을 업데이트할 수 있지만, 복제 설정 리소스를 삭제해도 대상 볼륨은 자동으로 삭제되지 않습니다.

> [주의]
> 복제 설정 리소스의 값을 변경하면 기존 리소스가 삭제되고 새로 생성될 수 있지만, 기존 대상 볼륨은 삭제되지 않습니다.
> 남아 있는 대상 볼륨과 새로운 대상 볼륨의 이름이 같으면 생성이 실패할 수 있으니 주의하세요.

<!-- -->

> [참고]
> 리소스 삭제 및 업데이트로 인해 삭제되지 않고 남아 있는 대상 볼륨은 콘솔에서 따로 관리해야 합니다.

```hcl
resource "nhncloud_nas_storage_volume_mirror_v1" "nas_mirror_01" {
  src_volume_id = nhncloud_nas_storage_volume_v1.volume_01.id
  dst_region    = "KR2"
  dst_tenant_id = "ba3be1254ab141bcaef674e74630a31f"

  dst_volume {
    name        = "nas_mirror"
    description = "create nas mirror by terraform"
    size_gb     = 400

    mount_protocol {
      protocol = "nfs"
    }
  }
}
```

| 이름 | 타입 | 필수 | 변경 가능 | 설명 |
| --- | --- | --- | --- | --- |
| src_region | String | - | - | 원본 볼륨의 리전<br>기본값은 공급자 설정 파일에 설정된 리전 |
| src_volume_id | String | O | - | 원본 볼륨의 ID |
| dst_region | String | O | - | 복제 대상 볼륨의 리전 |
| dst_tenant_id | String | O | - | 복제 대상 볼륨의 테넌트 ID |
| dst_volume | Object | O | - | 복제 대상 볼륨 생성 요청 객체 |
| dst_volume.acl | List | - | O | 볼륨 생성 시 설정할 ACL 목록<br>IP 또는 CIDR 형식으로 입력할 수 있습니다. |
| dst_volume.description | String | - | O | 볼륨 설명 |
| dst_volume.encryption | Object | - | - | 볼륨 생성 시 암호화 설정 객체 |
| dst_volume.encryption.enabled | Boolean | - | - | 암호화 설정 활성화 여부<br>암호화 키 저장소가 설정된 후 해당 필드를 `true`로 설정하면 암호화가 활성화됩니다. |
| dst_volume.mount_protocol | Object | - | - | 볼륨 생성 시 프로토콜 설정 객체 |
| dst_volume.mount_protocol.cifs_auth_ids | List | - | O | CIFS 인증 ID 목록<br>NFS 프로토콜 선택 시 입력 불필요 |
| dst_volume.mount_protocol.protocol | String | O | - | 볼륨 마운트 시 프로토콜 지정<br>`nfs`, `cifs` 중 하나를 선택할 수 있습니다. |
| dst_volume.name | String | O | - | 볼륨 이름 |
| dst_volume.size_gb | Integer | O | O | 볼륨 크기(GB)<br>볼륨은 최소 300GB에서 최대 10,000GB까지, 100GB 단위로 설정할 수 있습니다. |
| dst_volume.snapshot_policy | Object | - | - | 볼륨 스냅숏 설정 객체 |
| dst_volume.snapshot_policy.max_scheduled_count | Integer | - | O | 스냅숏 최대 저장 개수<br>30개까지 설정 가능하며, 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 만들어진 스냅숏이 삭제됩니다. |
| dst_volume.snapshot_policy.reserve_percent | Integer | - | O | 스냅숏 용량 비율 |
| dst_volume.snapshot_policy.schedule | Object | - | O | 스냅숏 자동 생성 객체<br>`null`일 경우 스냅숏 자동 생성이 설정되지 않습니다. |
| dst_volume.snapshot_policy.schedule.time | String | - | O | 스냅숏 자동 생성 시간 |
| dst_volume.snapshot_policy.schedule.time_offset | String | - | O | 스냅숏 자동 생성 기준 시간대 |
| dst_volume.snapshot_policy.schedule.weekdays | List | - | O | 스냅숏 자동 생성 요일<br>빈 목록은 매일을 의미하며, 요일은 0(일요일)부터 6(토요일)까지의 숫자 목록으로 지정합니다. |

<a id="reference"></a>
## 참고 사이트

Terraform - [https://www.terraform.io/](https://www.terraform.io/)
Terraform Registry - [https://registry.terraform.io/](https://registry.terraform.io/)

