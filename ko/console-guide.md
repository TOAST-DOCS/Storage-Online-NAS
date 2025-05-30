## Storage > NAS > 콘솔 사용 가이드

### 스토리지 생성

새로운 스토리지를 생성합니다. 생성된 스토리지는 NFS(network file system, 네트워크 파일 시스템) 및 CIFS(common internet file system, 공용 인터넷 파일 시스템) 프로토콜을 이용하여 인스턴스에서 접근할 수 있습니다.

| 항목 | 설명 |
| -- | -- |
| 이름 | 생성할 스토리지의 이름입니다. 스토리지 이름을 통해 NFS 또는 CIFS의 접근 경로를 만듭니다. 이름은 100자 이내의 영문자와 숫자, 일부 기호('-', '_')만 입력할 수 있습니다. |
| 설명 | 스토리지에 대한 설명입니다. |
| 크기 | 생성할 스토리지의 크기입니다. 최소 300GB부터 최대 10,000GB까지 입력할 수 있습니다. |
| 프로토콜 | 스토리지에 접근할 프로토콜을 선택합니다. CIFS와 NFS를 지원하며 동시에 사용은 불가능합니다. |
| VPC | 스토리지에 접근할 VPC(virtual private cloud, 가상 사설 클라우드)입니다. |
| 서브넷 | 스토리지에 접근할 서브넷입니다. 선택된 VPC의 서브넷만 선택할 수 있습니다. |
| 접근 제어 목록(ACL) | 읽기, 쓰기 권한을 허용할 IP 또는 IP 대역 목록입니다. |
| 스냅숏 자동 생성 | 매일 1회 지정된 시간에 자동으로 생성합니다. 최대 저장 개수에 도달하면 자동으로 생성된 스냅숏 중 가장 먼저 만들어진 스냅숏이 삭제됩니다.  |
| 스냅숏 예약 용량 | 스냅숏이 저장될 공간을 미리 할당합니다. 설정한 용량을 제외한 용량에 데이터 저장이 가능합니다. 스냅숏 예약 용량 설정보다 실제 스냅숏의 크기가 클 경우 예약 용량 외의 데이터 저장 공간까지 스냅숏 저장에 사용합니다. |
| 암호화 | 스토리지 암호화 사용 여부를 선택합니다. 암호화 키 저장소 설정이 선행되어야 합니다. |

> [참고]
> 2024년 2월 현재 프로젝트별로 사용 가능한 서브넷은 총 3개로 제한됩니다. 서브넷 개수 한도를 높이려면 고객 센터에 문의하십시오.

#### CIFS 인증 정보 관리
CIFS 프로토콜을 사용하기 위해서는 CIFS 인증 정보를 생성해야 합니다. 인증 정보는 프로젝트 단위로 관리되며, CIFS 스토리지마다 접근할 CIFS 인증 정보를 등록해야 합니다.

##### CIFS 인증 정보 규칙

* ID 규칙
    * 영문 소문자 또는 숫자로 시작해야 하며, 마침표(.)로 끝날 수 없습니다.
    * 사용할 수 있는 문자는 영문 소문자(a–z), 숫자(0–9), 하이픈(-), 마침표(.), 밑줄(_)이며, 그 외의 문자는 사용할 수 없습니다.
    * 최대 20자까지 입력할 수 있습니다.
    * 숫자만으로 구성된 ID, 예약어(administrator, default, guest, krbtgt), 중복 ID는 사용할 수 없습니다.

* 비밀번호 규칙
    * 최소 6자 이상이어야 하며, 영문 대문자, 영문 소문자, 숫자, 특수문자 중 세 가지 이상의 문자 종류를 포함해야 합니다.
    * 사용할 수 있는 특수문자는 `~!@#^*_-+=\\|()[]:;"'<>,.?/`이며, 공백은 사용할 수 없습니다.
    * ID가 포함된 비밀번호는 사용할 수 없습니다.

#### 암호화 키 저장소 설정

NAS 암호화 스토리지는 암호화에 사용하는 대칭 키를 NHN Cloud Secure Key Manager 서비스의 키 저장소에 저장합니다. 따라서 암호화 스토리지를 만들기 위해서는 미리 Secure Key Manager 서비스에서 [키 저장소를 생성](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_1)해야 합니다. [키 저장소의 ID를 확인](https://docs.nhncloud.com/ko/Security/Secure%20Key%20Manager/ko/getting-started/#_2)하여 암호화 키 저장소 설정에 입력합니다.

암호화 스토리지를 생성하면 설정한 키 저장소에 대칭 키가 저장됩니다. NAS 서비스에 의해 키 저장소에 저장된 대칭 키는 암호화 스토리지 사용 중에는 삭제할 수 없습니다. 암호화 스토리지를 삭제하면 대칭 키도 함께 삭제됩니다.

키 저장소 ID를 변경하면 이후 생성하는 암호화 스토리지의 대칭 키가 변경된 키 저장소에 저장됩니다. 기존 키 저장소에 저장된 대칭 키는 유지됩니다.

> [참고]
> Secure Key Manager 서비스 요금 정책에 따라 키 저장소 사용 요금이 청구됩니다. 이용 요금에 대한 자세한 사항은 [Secure Key Manager 요금 안내](https://www.nhncloud.com/kr/service/security/secure-key-manager#price)를 참고하세요.
>
> NAS 암호화 스토리지는 XTS-AES-256 알고리즘으로 서로 다른 두 개의 대칭 키를 사용하여 데이터를 암호화합니다. 따라서 암호화 스토리지마다 두 개의 대칭 키를 키 저장소에 저장합니다.

### 스토리지 크기 변경

NAS 스토리지의 크기를 변경합니다. 스토리지 사용 중에도 확장 및 축소가 가능합니다.

### 접근 제어 목록 설정 변경

스토리지의 접근 제어 목록(ACL) 설정을 변경합니다.

### 스냅숏 설정 변경

스냅숏 자동 생성 및 스냅숏 예약 용량에 대한 설정을 변경합니다.

### 스토리지 삭제

NAS 스토리지를 삭제합니다.

> [주의]
> 연결된 인스턴스에서 마운트 해제 후 삭제할 것을 권장합니다. 마운트 상태에서 스토리지를 삭제하면 사용자 시스템에 문제가 생길 수 있습니다.
>
> 스토리지를 삭제할 경우 스냅숏을 포함한 모든 데이터가 삭제됩니다. 삭제 후에는 데이터를 복구할 수 없습니다.

### 스냅숏
생성된 스냅숏 목록을 조회합니다.

| 항목 | 설명 |
| --- | --- |
| 이름 | 스냅숏의 이름을 표시합니다. 시스템에 의해 생성된 경우 지정된 규칙에 따라 이름이 정해집니다. |
| 스냅숏 생성 시 스토리지 사용량 | 스냅숏 생성 시점의 NAS 스토리지 사용량입니다. 스냅숏 예약 용량을 초과하여 저장 중일 경우 초과한 용량을 제외한 데이터 용량으로 표시됩니다. |
| 생성일 | 스냅숏이 생성된 시간입니다. |

> [참고]
> 스냅숏은 NAS 스토리지의 저장 공간 중 스냅숏 예약 용량을 우선 사용합니다. 전체 스냅숏 크기가 예약 용량을 초과하면, 초과분은 데이터 저장 공간에 저장됩니다.

> [참고]
> 일부 스냅숏은 시스템에 의해 생성되며, 삭제할 수 없습니다.


#### 스냅숏 생성
NAS 스토리지의 스냅숏을 즉시 생성합니다.

#### 스냅숏 복원
스토리지를 스냅숏이 생성된 시점으로 복원합니다.

> [주의]
> 복원 시 복원 시점 이후에 생성된 스냅숏은 자동으로 삭제됩니다.

#### 스냅숏 복원 결과 조회
스토리지를 복원한 이력을 조회합니다.

#### 스냅숏 삭제

지정된 스냅숏을 삭제합니다. 한 번 삭제된 스냅숏은 복구할 수 없습니다.

### 네트워크
네트워크 연결 정보를 확인합니다.

| 항목 | 설명 |
| --- | --- |
| 연결 정보 | 인스턴스에서 마운트 시 이용할 연결 경로를 표시합니다. |
| 서브넷 | 스토리지와 연결된 서브넷 정보입니다. |
| 상태 | 서브넷 연결 상태를 표시합니다. |


#### 서브넷 연결 추가
서브넷 연결을 추가합니다. 서브넷 연결을 추가하면 추가된 서브넷에서 NAS 스토리지에 접근 가능합니다.

> [주의]
> 서브넷 연결 추가 후 ACL에 서브넷 대역이 존재하지 않으면 마운트가 불가능합니다.

#### 서브넷 연결 해제
스토리지에 연결된 서브넷을 제거합니다. 필요한 경우 IP ACL은 별도로 제거해야 합니다.

> [주의]
> 연결된 인스턴스에서 마운트 해제 후 서브넷 연결을 해제할 것을 권장합니다. 마운트 상태에서 연결을 해제하면 사용자 시스템에 문제가 생길 수 있습니다.

### 모니터링

NAS 스토리지의 여러 지표를 그래프와 함께 확인합니다. 확인하고자 하는 스토리지를 선택 후, **모니터링** 탭을 선택합니다.

기본 조회 기간은 마지막 1시간이며, 조회 기간 필터를 통해 원하는 기간을 조회할 수 있습니다. 모니터링 지표에 대한 설명은 아래 표를 통해 확인할 수 있습니다.

| 지표 | 단위 | 설명 |
| --- | --- | --- |
| 스토리지 용량 | byte | 스토리지의 총 용량과 사용 중인 용량을 표시합니다. |
| 스토리지 사용률 | % | 스토리지 총 용량 대비 사용 중인 용량을 백분율로 표시합니다. |
| IOPS | OPS | 스토리지의 초당 연산수를 표시합니다. |
| Latency | usec | 스토리지의 지연 시간을 표시합니다. |
| 스냅숏 개수 | - | 스토리지의 스냅숏 개수를 표시합니다. |
| 스냅숏 용량 | byte | 스토리지가 스냅숏에 사용 중인 용량을 표시합니다. |
| Inode | - | 스토리지의 inode 수를 표시합니다. |
| 스토리지 상태 | - | 스토리지의 상태를 표시합니다. |
| 클라이언트 연결 개수 | - | 스토리지에 연결된 클라이언트 개수를 표시합니다. |

### 복제

복제 설정 관련 정보를 확인합니다.

| 설정 | 설명 |
| --- | --- |
| 복제 설정 | 스토리지의 복제 설정 여부를 표시합니다. |
| 복제 설정 상태 | 원본과 대상 스토리지 간 복제 설정 상태를 표시합니다.<br>Active: 활성화 상태<br>Retrieve failed: 일시적인 정보 획득 실패<br>Halt: 일시정지 상태<br>최초 복제 진행 상태에서는 복제 진행률과 대상 스토리지 암호화 상태를 추가로 표시합니다. |
| 최근 복제 실행 결과 | 최근에 실행한 복제 결과를 표시합니다. |
| 최근 복제 실행 시간 | 최근에 복제를 실행한 시간을 표시합니다. |
| 최근 복제 성공 시간 | 최근에 복제를 완료한 시간을 표시합니다. |
| 복제 방향 | 복제 방향을 표시합니다. |
| 대상 리전 | 복제 대상 리전를 표시합니다. 복제를 설정한 원본 스토리지인 경우에만 노출됩니다. |
| 대상 스토리지 | 복제 대상 스토리지 이름을 표시합니다. 복제를 설정한 원본 스토리지인 경우에만 노출됩니다.|
| 원본 리전 | 복제 원본 리전를 표시합니다. 복제가 설정된 대상 스토리지인 경우에만 노출됩니다.|
| 원본 스토리지 | 복제 원본 스토리지 이름을 표시합니다. 복제가 설정된 대상 스토리지인 경우에만 노출됩니다. |

#### 복제 설정

조직 내 프로젝트의 선택한 리전으로 복제를 설정할 수 있습니다.
복제를 설정하면 대상 위치에 원본 스토리지와 같은 크기의 대상 스토리지가 생성됩니다. 대상 스토리지는 읽기 전용 상태로 생성되며, 대상 스토리지의 상태를 변경하려면 복제 중지를 하거나 복제 설정을 해제해야 합니다.
원본 스토리지의 데이터를 업데이트하거나 삭제하면 대상 스토리지의 데이터도 업데이트 또는 삭제됩니다.

대상 프로젝트별 선택 가능한 리전 범위는 아래 표를 통해 확인할 수 있습니다.

| 대상 프로젝트 | 선택 가능한 리전 |
| -- | -- |
| 조직 내 동일 프로젝트 | 다른 리전 |
| 조직 내 다른 프로젝트 | 모든 리전 |

> [주의]
> 복제 설정된 스토리지의 크기를 변경하려면 원본 스토리지와 대상 스토리지 모두 변경해야 합니다. 원본 스토리지와 대상 스토리지의 크기가 다른 경우 복제에 실패할 수 있습니다.

> [참고]
> 스토리지 복제는 스냅숏을 이용하여 진행합니다. 복제 설정 시 `{스토리지명}.mirror.%` 형식의 스냅숏이 자동으로 생성되며 삭제할 수 없습니다.

> [참고]
> 복제 설정으로 생성된 대상 스토리지는 NAS 서비스 요금 정책에 따라 스토리지 용량 요금이 청구됩니다.
>
> 복제로 인해 발생하는 네트워크 트래픽에 대해 다른 리전 간 트래픽 요금이 청구됩니다.
>
> 이용 요금에 관한 자세한 사항은 [NAS 요금 안내](https://www.nhncloud.com/kr/service/storage/nas)를 참고하세요.

#### 복제 시작

중지 상태의 스토리지 복제를 다시 시작합니다.
원본 스토리지에 변경이 발생하면 비동기적으로 복제가 실행됩니다. 복제가 실행되기 전에는 원본 스토리지와 대상 스토리지의 데이터가 일치하지 않을 수 있습니다.

> [주의]
> 대상 스토리지 크기가 원본 스토리지 크기보다 작은 경우 복제에 실패할 수 있습니다.
>
> 복제가 실행되면 대상 스토리지의 기존 데이터는 모두 삭제되며 원본 스토리지 형상과 동일한 상태로 적용됩니다.

#### 복제 중지

스토리지 복제를 중지합니다.
복제를 중지하면 대상 스토리지가 쓰기 가능한 상태로 변경되며, 마운트하여 사용할 수 있습니다.

> [주의]
> 복제를 중지하면 원본 스토리지와 대상 스토리지의 데이터가 일치하지 않을 수 있습니다.

> [참고]
> 복제 중지 상태의 스토리지도 복제 상태 확인을 위해 소량의 트래픽이 발생할 수 있습니다.

#### 복제 방향 변경

원본 스토리지와 대상 스토리지의 복제 방향을 변경합니다.

* **[원본->대상]**에서 **[대상->원본]**으로 복제 방향 변경
    * 원본 스토리지의 기존 데이터는 모두 삭제되며, 대상 스토리지의 데이터를 원본 스토리지에 복제합니다.
    * 대상 스토리지 크기보다 원본 스토리지 크기가 작으면 복제에 실패할 수 있습니다.

* **[대상->원본]**에서 **[원본->대상]**으로 복제 방향 변경(원상 복귀)
    * 대상 스토리지의 기존 데이터는 모두 삭제되며, 원본 스토리지의 데이터를 대상 스토리지에 복제합니다.
    * 원본 스토리지 크기보다 대상 스토리지 크기가 작으면 복제에 실패할 수 있습니다.

#### 복제 설정 해제

스토리지 복제 설정을 해제합니다.
복제 설정을 해제하면 대상 스토리지는 마지막 복제 완료 시점의 원본 스토리지 데이터를 그대로 유지합니다.

> [주의]
> 복제를 다시 설정하면 새로운 대상 스토리지가 생성됩니다. 이전에 사용하던 대상 스토리지를 다시 복제 대상 스토리지로 사용할 수 없습니다.

### 스토리지 연결

생성된 스토리지의 연결 정보를 이용하여 인스턴스에 마운트할 수 있습니다. 단, 마운트할 인스턴스는 스토리지와 같은 서브넷에 연결되어 있어야 합니다.

#### NFS

##### nfs 패키지 설치

* Debian, Ubuntu: nfs-common, rpcbind
  ```
  sudo apt-get install nfs-common rpcbind
  ```
* CentOS: nfs-utils, rpcbind
  ```
  sudo yum install nfs-utils rpcbind
  ```

##### rpcbind 서비스 실행

```
sudo service rpcbind start
```

##### NAS 스토리지 마운트

```
sudo mount -t nfs {nas source} {mount point}
```

* nas source: NAS 스토리지 정보
  예: 192.168.0.241:/data
* mount point: NAS 스토리지를 마운트할 디렉터리
  예: /mnt

#### CIFS

파일 탐색기의 좌측 탐색창에서 **내 PC**를 우클릭 후 **네트워크 드라이브 연결**을 선택합니다. 네트워크 드라이브 연결 창이 뜨면, 마운트 할 드라이브를 선택 후 폴더 경로를 입력합니다.  폴더 경로 형식은 `\\{스토리지 ip}\{스토리지 명}`와 같습니다. 예: \\\\192.168.0.100\\cifs

