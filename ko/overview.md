<a id="storage-nas-overview"></a>
## Storage > NAS > 개요 { #storage-nas-overview }

NAS 서비스를 사용하면 인스턴스에 공유 스토리지를 연결하여 데이터를 쉽게 공유할 수 있습니다.

<a id="features"></a>
## 특징 { #features }

<a id="features.sharing"></a>
### 공유 { #features.sharing }

볼륨을 하나 이상의 인스턴스에 마운트하여 사용할 수 있습니다.
지원하는 프로토콜: NFS v3(Linux), CIFS(Windows)

<a id="features.convenient"></a>
### 편리성 { #features.convenient }

별도의 파일 시스템 구성 없이 네트워크 파일 시스템을 통해 볼륨을 마운트하고, 파일 단위 작업을 수행할 수 있습니다.

<a id="features.flexible"></a>
### 유연성 { #features.flexible }

볼륨을 사용하는 중에도 저장소 용량을 확장하거나 축소할 수 있습니다.

<a id="features.secure"></a>
### 보안성 { #features.secure }

볼륨은 프로젝트의 네트워크를 통해서만 접근할 수 있어, 다른 프로젝트의 네트워크와 격리됩니다.
XTS-AES-256 알고리즘으로 암호화하여 데이터를 안전하게 보관할 수 있습니다.

<a id="glossary"></a>
## 용어 { #glossary }

<a id="glossary.NAS"></a>
### NAS(network-attached storage) { #glossary.NAS }

NAS는 네트워크를 통해 접근할 수 있는 파일 기반 저장 장치입니다. NAS 볼륨은 로컬 디스크처럼 마운트해 파일을 저장하거나 불러올 수 있으며, 여러 서버 간 데이터 공유에도 활용할 수 있습니다. 접근 제어나 인증 같은 기본적인 보안 기능도 함께 제공합니다.

<a id="glossary.volume"></a>
### 볼륨 { #glossary.volume }

볼륨은 NAS의 논리적인 저장 공간으로, 인스턴스에 마운트하여 데이터를 저장하거나 읽을 수 있습니다.

<a id="glossary.snapshots"></a>
### 스냅숏 { #glossary.snapshots }

스냅숏은 볼륨의 읽기 전용 복사본으로, 특정 시점의 데이터를 백업 및 복원하는 데 사용됩니다.
사용자는 하루 1회 자동 생성 시점을 지정할 수 있으며, 생성된 스냅숏을 통해 해당 시점의 상태로 데이터를 복원할 수 있습니다.
스냅숏은 볼륨의 저장 공간을 사용하므로, 필요하지 않은 경우 생성을 비활성화할 수 있습니다.
