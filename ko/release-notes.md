<!-- pre-align:aligned sig=41a352a073ca -->

<a id="storage-nas-release-notes"></a>
## Storage > NAS > 릴리스 노트 { #storage-nas-release-notes }

<a id="august-25-2026"></a>
## 2026. 08. 25. { #august-25-2026 }

<a id="august-25-2026-added-features"></a>
### 신규 기능 추가 { #august-25-2026-added-features }

* NAS 리소스 접근 권한 세분화
    * Infrastructure NAS ADMIN 역할이 추가되어 NAS 리소스의 생성, 수정, 삭제 권한을 제공합니다.
    * Infrastructure ADMIN을 제외한 기존 기본 인프라 역할에서는 NAS 리소스의 생성, 수정, 삭제가 제한됩니다.

<a id="august-25-2026-feature-updates"></a>
### 기능 개선/변경 { #august-25-2026-feature-updates }

* 볼륨 상태 표시 개선
    * 볼륨 상태가 실패 또는 오류일 때 그 원인을 확인할 수 있도록 상세 사유가 함께 제공됩니다.

<a id="may-27-2026"></a>
## 2026. 05. 27. { #may-27-2026 }

<a id="may-27-2026-added-features"></a>
### 신규 기능 추가 { #may-27-2026-added-features }

* 볼륨 사용 현황에 스냅숏 사용량 정보 추가
    * 전체 스냅숏 사용량
    * 스냅숏 예약 용량 내 사용량
    * 예약 용량 초과 스냅숏 사용량

<a id="november-25-2025"></a>
## 2025. 11. 25. { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
### 기능 개선/변경 { #november-25-2025-feature-updates }

* 모니터링 기능 변경
    * CIFS 프로토콜 볼륨 모니터링 기능이 추가되었습니다.
    * 클라이언트 연결 개수 모니터링이 제거되었습니다.

<a id="may-27-2025"></a>
## 2025. 05. 27. { #may-27-2025 }

<a id="may-27-2025-added-features"></a>
### 신규 기능 추가 { #may-27-2025-added-features }

* Public API 제공
* 볼륨 이름 검색 추가

<a id="may-27-2025-feature-updates"></a>
### 기능 개선/변경 { #may-27-2025-feature-updates }

* CIFS 인증 정보 ID 제약 조건 변경
* 볼륨 복제 기능 변경
    * 복제 방향 변경 즉시 복제가 활성화됩니다.
    * 복제를 중지하거나 복제 방향 변경 시 요청 시점까지의 데이터가 복제된 후 작업이 진행됩니다.

<a id="march-4-2025"></a>
## 2025. 03. 04. { #march-4-2025 }

<a id="march-4-2025-added-features"></a>
### 신규 기능 추가 { #march-4-2025-added-features }

* 볼륨 복제 기능 개선
    * CIFS 프로토콜 볼륨을 복제할 수 있습니다.

<a id="august-27-2024"></a>
## 2024. 08. 27. { #august-27-2024 }

<a id="august-27-2024-added-features"></a>
### 신규 기능 추가 { #august-27-2024-added-features }

* 생성된 볼륨의 서브넷 연결 변경 기능 추가
* 볼륨 복제 기능 개선
    * 조직 내의 다른 프로젝트로 볼륨을 복제할 수 있습니다.
    * 암호화 볼륨을 복제할 수 있습니다.
* 스냅숏 목록에 생성 시점 사용량 표시

<a id="may-28-2024"></a>
## 2024. 05. 28. { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
### 신규 기능 추가 { #may-28-2024-added-features }

* CIFS 프로토콜 추가
    * 윈도우 환경에서도 볼륨을 사용할 수 있습니다.
* 스냅숏 복원 기능 추가
* 스냅숏 복원 히스토리 조회 추가
* 리전 간 볼륨 복제 기능 추가

<a id="march-26-2024"></a>
## 2024. 03. 26. { #march-26-2024 }

<a id="march-26-2024-added-features"></a>
### 신규 기능 추가 { #march-26-2024-added-features }

* 암호화 볼륨 생성 기능 추가

<a id="march-28-2023"></a>
## 2023. 03. 28. { #march-28-2023 }

<a id="march-28-2023-added-features"></a>
### 신규 기능 추가 { #march-28-2023-added-features }

* NAS 모니터링 기능 추가
    * 볼륨에 대한 다양한 지표를 그래프와 함께 확인할 수 있습니다.

<a id="november-23-2022"></a>
## 2022. 11. 23. { #november-23-2022 }

<a id="november-23-2022-added-new-region"></a>
### 신규 리전 추가 { #november-23-2022-added-new-region }

* 한국(평촌) 리전에 NAS 서비스 출시

<a id="july-26-2022"></a>
## 2022. 07. 26. { #july-26-2022 }

<a id="july-26-2022-new-service-release"></a>
### 신규 서비스 출시 { #july-26-2022-new-service-release }

* 한국(판교) 리전에 NAS 서비스 출시
* 프로젝트의 VPC 네트워크를 통한 인스턴스 볼륨 연결 지원