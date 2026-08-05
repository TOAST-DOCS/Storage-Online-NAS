<a id="storage-nas-terraform-user-guide"></a>
## Storage > NAS > Terraform使用ガイド { #storage-nas-terraform-user-guide }

このドキュメントでは、Terraformを使用してNHN Cloud NASサービスを使用する方法を説明します。

<a id="terraform"></a>
## Terraform { #terraform }

Terraformは、インフラを簡単に構築し、安全に変更し、効率的に構成を管理できるオープンソースツールです。基本的な使用法は、[ユーザーガイド > NHN Cloud > Terraform使用ガイド](/nhncloud/ja/terraform-guide/)を参照してください。

<a id="terraform-resource-dependency"></a>
### リソースの依存関係 { #terraform-resource-dependency }

一般的に各リソースは独立していますが、他の特定のリソースに依存関係を持つ場合もあります。リソースのラベルで他のリソースの情報を参照すると、Terraformは自動的に依存関係を設定します。
例えば、`volume1`ボリュームに接続される`interface1`インターフェイスは次のように表現できます。

```hcl
# ボリュームリソース
resource "nhncloud_nas_storage_volume_v1" "volume1" {
  name = "volume1"
  size_gb = 300

  mount_protocol {
    protocol = "nfs"
  }
}

# インターフェースリソース
resource "nhncloud_nas_storage_volume_interface_v1" "interface1" {
  volume_id = nhncloud_nas_storage_volume_v1.volume1.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
}
```

!!! tip "ヒント"
    明示的なリソース依存関係の指定方法については、[Terraform の Resource dependencies](https://developer.hashicorp.com/terraform/tutorials/configuration-language/dependencies) を参照してください。

<a id="terraform-resources-nas"></a>
## Resources { #terraform-resources-nas }

<a id="terraform-resources-create-volume"></a>
### ボリュームの作成 { #terraform-resources-create-volume }

!!! tip "参考: CIFSプロトコルの使用"
    CIFSプロトコルを使用するには、CIFS認証情報を作成する必要があります。認証情報はプロジェクト単位で管理され、CIFSボリュームごとにアクセスを許可するCIFS認証情報を登録する必要があります。
    CIFS認証情報は、コンソールの**Storage > NAS > CIFS認証情報管理**画面で作成できます。


<!-- -->

!!! tip "参考: 暗号化キーストア設定"
    暗号化ボリュームを作成すると、暗号化に使用する共通鍵がNHN Cloud Secure Key Managerサービスのキーストアに保存されます。したがって、暗号化ボリュームを作成するには、事前にSecure Key Managerサービスで[キーストアを作成](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ja/getting-started/#_1)する必要があります。[キーストアのIDを確認](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ja/getting-started/#_2)し、暗号化キーストア設定に入力します。
    作成したキーストアIDは、コンソールの**Storage > NAS > 暗号化キーストア設定**画面で入力できます。暗号化ボリュームを作成すると、設定したキーストアに共通鍵が保存されます。キーストアに保存された共通鍵は、暗号化ボリュームの使用中は削除できません。暗号化ボリュームを削除すると、共通鍵も一緒に削除されます。
    キーストアIDを変更すると、それ以降に作成する暗号化ボリュームの共通鍵は変更されたキーストアに保存されます。既存のキーストアに保存された共通鍵は維持されます。


```hcl
# NFSプロトコルの空のNASボリュームの作成
resource "nhncloud_nas_storage_volume_v1" "volume_01" {
  name = "nas_volume_01"
  size_gb = 300
  mount_protocol {
    protocol = "nfs"
  }
}

# CIFSプロトコルの空のNASボリュームの作成
resource "nhncloud_nas_storage_volume_v1" "volume_02" {
  name = "nas_volume_02"
  size_gb = 300
  mount_protocol {
    protocol = "cifs"
    cifs_auth_ids = ["auth_id"]
  }
}

# ACL、暗号化設定などの設定を含めたボリュームの作成
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

| 名前 | タイプ | 必須 | 変更可能 | 説明 |
| --- | --- | --- | --- | --- |
| region | String | N | - | 作成するボリュームのリージョン<br>デフォルト値はプロバイダー設定ファイルに設定されたリージョン |
| name | String | Y | - | ボリューム名 |
| description | String | N | O | ボリューム説明 |
| size_gb | Integer | Y | O | ボリュームサイズ(GB)<br>ボリュームは最小300GBから最大10,000GBまで、100GB単位で設定できます。 |
| acl | List | N | O | ボリューム作成時に設定するACL一覧<br>IPまたはCIDR形式で入力できます。 |
| encryption | Object | N | - | ボリューム作成時の暗号化設定オブジェクト |
| encryption.enabled | Boolean | N | - | 暗号化設定の有効化有無<br>暗号化キーストアが設定された後、該当フィールドを`true`に設定すると暗号化が有効になります。 |
| mount_protocol | Object | N | - | ボリューム作成時のプロトコル設定オブジェクト |
| mount_protocol.cifs_auth_ids | List(String) | N | O | CIFS認証ID一覧<br>NFSプロトコル選択時は入力不要 |
| mount_protocol.protocol | String | Y | - | ボリュームマウント時のプロトコル指定<br>`nfs`、`cifs`のいずれかを選択できます。 |
| snapshot_policy | Object | N | - | ボリュームスナップショット設定オブジェクト |
| snapshot_policy.max_scheduled_count | Integer | N | O | スナップショット最大保存数<br>30個まで設定可能であり、最大保存数に達すると、自動的に作成されたスナップショットの中で一番最初に作られたスナップショットが削除されます。 |
| snapshot_policy.reserve_percent | Integer | N | O | スナップショット容量の割合 |
| snapshot_policy.schedule | Object | N | - | スナップショット自動作成オブジェクト<br>`null`の場合、スナップショット自動作成は設定されません。 |
| snapshot_policy.schedule.time | String | N | O | スナップショット自動作成時間 |
| snapshot_policy.schedule.time_offset | String | N | O | スナップショット自動作成基準タイムゾーン |
| snapshot_policy.schedule.weekdays | List | N | O | スナップショット自動作成曜日<br>空のリストは毎日を意味し、曜日は0(日曜日)から6(土曜日)までの数字のリストで指定します。 |

<a id="terraform-resources-connect-interface"></a>
### ボリュームにインターフェースを接続する { #terraform-resources-connect-interface }

```hcl
data "nhncloud_networking_vpcsubnet_v2" "default_subnet" {
  ...
}

resource "nhncloud_nas_storage_volume_interface_v1" "nas_interface_01" {
  volume_id = nhncloud_nas_storage_volume_v1.volume_01.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
}
```

| 名前 | タイプ | 必須 | 変更可能 | 説明 |
| --- | --- | --- | --- | --- |
| region | String | N | - | 接続するボリュームのリージョン<br>デフォルト値はプロバイダー設定ファイルに設定されたリージョン |
| volume_id | String | Y | - | 接続するボリュームのID |
| subnet_id | String | Y | - | 接続するサブネットID |

<a id="terraform-resources-set-replication"></a>
### レプリケーションの設定 { #terraform-resources-set-replication }

レプリケーション設定リソースを作成すると、対象ボリュームが自動的に作成されます。
レプリケーション設定リソースで`dst_volume`の設定値を変更して対象ボリュームをアップデートできますが、レプリケーション設定リソースを削除しても対象ボリュームは自動的に削除されません。

!!! danger "注意"
    レプリケーション設定リソースの値を変更すると、既存リソースが削除され新しく作成される場合がありますが、既存の対象ボリュームは削除されません。
    残っている対象ボリュームと新しい対象ボリュームの名前が同じ場合、作成が失敗する可能性があるためご注意ください。

<!-- -->

!!! tip "ヒント"
    リソースの削除および更新によって削除されずに残っているターゲットボリュームは、コンソールで個別に管理する必要があります。

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

| 名前 | タイプ | 必須 | 変更可能 | 説明 |
| --- | --- | --- | --- | --- |
| src_region | String | N | - | ソースボリュームのリージョン<br>デフォルト値はプロバイダー設定ファイルに設定されたリージョン |
| src_volume_id | String | Y | - | ソースボリュームのID |
| dst_region | String | Y | - | レプリケーション対象ボリュームのリージョン |
| dst_tenant_id | String | Y | - | レプリケーション対象ボリュームのテナントID |
| dst_volume | Object | Y | - | レプリケーション対象ボリューム作成リクエストオブジェクト |
| dst_volume.acl | List | N | O | ボリューム作成時に設定するACL一覧<br>IPまたはCIDR形式で入力できます。 |
| dst_volume.description | String | N | O | ボリューム説明 |
| dst_volume.encryption | Object | N | - | ボリューム作成時の暗号化設定オブジェクト |
| dst_volume.encryption.enabled | Boolean | N | - | 暗号化設定の有効化有無<br>暗号化キーストアが設定された後、該当フィールドを`true`に設定すると暗号化が有効になります。 |
| dst_volume.mount_protocol | Object | N | - | ボリューム作成時のプロトコル設定オブジェクト |
| dst_volume.mount_protocol.cifs_auth_ids | List | N | O | CIFS認証ID一覧<br>NFSプロトコル選択時は入力不要 |
| dst_volume.mount_protocol.protocol | String | Y | - | ボリュームマウント時のプロトコル指定<br>`nfs`、`cifs`のいずれかを選択できます。 |
| dst_volume.name | String | Y | - | ボリューム名 |
| dst_volume.size_gb | Integer | Y | O | ボリュームサイズ(GB)<br>ボリュームは最小300GBから最大10,000GBまで、100GB単位で設定できます。 |
| dst_volume.snapshot_policy | Object | N | - | ボリュームスナップショット設定オブジェクト |
| dst_volume.snapshot_policy.max_scheduled_count | Integer | N | O | スナップショット最大保存数<br>30個まで設定可能であり、最大保存数に達すると、自動的に作成されたスナップショットの中で一番最初に作られたスナップショットが削除されます。 |
| dst_volume.snapshot_policy.reserve_percent | Integer | N | O | スナップショット容量の割合 |
| dst_volume.snapshot_policy.schedule | Object | N | O | スナップショット自動作成オブジェクト<br>`null`の場合、スナップショット自動作成は設定されません。 |
| dst_volume.snapshot_policy.schedule.time | String | N | O | スナップショット自動作成時間 |
| dst_volume.snapshot_policy.schedule.time_offset | String | N | O | スナップショット自動作成基準タイムゾーン |
| dst_volume.snapshot_policy.schedule.weekdays | List | N | O | スナップショット自動作成曜日<br>空のリストは毎日を意味し、曜日は0(日曜日)から6(土曜日)までの数字のリストで指定します。 |

<a id="reference"></a>
## 参考サイト { #reference }

Terraform - [https://www.terraform.io/](https://www.terraform.io/)
Terraform Registry - [https://registry.terraform.io/](https://registry.terraform.io/)
