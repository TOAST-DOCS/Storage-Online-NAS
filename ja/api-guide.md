<!-- machine_translated: true -->

{% include-markdown '../_online-nas-vars.md' %}

<!-- pre-align:aligned sig=06dac106ebf2 -->

{% macro interface_response_table(prefix='', desc_prefix='') -%}
| $[ prefix ]$id | Body | String | $[ desc_prefix ]$インターフェイス ID |
| $[ prefix ]$path | Body | String | $[ desc_prefix ]$インターフェイスパス |
| $[ prefix ]$status | Body | String | $[ desc_prefix ]$インターフェイスステータス |
| $[ prefix ]$subnetId | Body | String | $[ desc_prefix ]$インターフェイスのサブネット ID |
| $[ prefix ]$tenantId | Body | String | $[ desc_prefix ]$インターフェイスのテナント ID |{% endmacro %}
{# end macro interface_response_table #}
{% macro volume_mirror_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | 複製設定 ID |
| $[ prefix ]$role | Body | String | 複製役割<br>- `SOURCE`: ソースボリューム<br>- `DESTINATION`: ターゲットボリューム |
| $[ prefix ]$status | Body | String | 複製設定ステータス<br>- `INITIALIZED`: 設定完了<br>- `UPDATING`: 設定変更中<br>- `DELETING`: 設定削除中<br>- `PENDING`: 設定作成中 |
| $[ prefix ]$direction | Body | String | 複製方向<br>- `FORWARD`: ソースボリューム → ターゲットボリューム<br>- `REVERSE`: ターゲットボリューム → ソースボリューム |
| $[ prefix ]$directionChangedAt | Body | String | 複製方向変更日時 |
| $[ prefix ]$dstProjectId | Body | String | ターゲットボリュームのプロジェクト ID |
| $[ prefix ]$dstRegion | Body | String | ターゲットボリュームのリージョン |
| $[ prefix ]$dstTenantId | Body | String | ターゲットボリュームのテナント ID |
| $[ prefix ]$dstVolumeId | Body | String | ターゲットボリューム ID |
| $[ prefix ]$dstVolumeName | Body | String | ターゲットボリューム名 |
| $[ prefix ]$srcProjectId | Body | String | ソースボリュームのプロジェクト ID |
| $[ prefix ]$srcRegion | Body | String | ソースボリュームのリージョン |
| $[ prefix ]$srcTenantId | Body | String | ソースボリュームのテナント ID |
| $[ prefix ]$srcVolumeId | Body | String | ソースボリューム ID |
| $[ prefix ]$srcVolumeName | Body | String | ソースボリューム名 |
| $[ prefix ]$createdAt | Body | String | 複製作成日時 |{% endmacro %}
{# end macro volume_mirror_response_table #}
{% macro volume_response_table(prefix='') -%}
| $[ prefix ]$id | Body | String | ボリューム ID |
| $[ prefix ]$name | Body | String | ボリューム名 |
| $[ prefix ]$status | Body | String | ボリュームステータス |
| $[ prefix ]$description | Body | String | ボリュームの説明 |
| $[ prefix ]$sizeGb | Body | Integer | ボリュームサイズ (GB) |
| $[ prefix ]$projectId | Body | String | ボリュームが属するプロジェクト ID |
| $[ prefix ]$tenantId | Body | String | ボリュームが属するテナント ID |
| $[ prefix ]$acl | Body | List | ボリューム ACL リスト |
{%- if encryption %}
| $[ prefix ]$encryption | Body | Object | ボリューム暗号化情報 |
| $[ prefix ]$encryption.enabled | Body | Boolean | ボリューム暗号化の有効可否 |
| $[ prefix ]$encryption.keys | Body | List | ボリューム暗号化キー情報 |
{%- endif %}
| $[ prefix ]$interfaces | Body | List | ボリュームインターフェイスオブジェクトリスト |
$[ interface_response_table(prefix + 'interfaces.') ]$
{%- if replication %}
| $[ prefix ]$mirrors | Body | List | ボリューム複製設定オブジェクトリスト |
$[ volume_mirror_response_table(prefix + 'mirrors.') ]$
{%- endif %}
| $[ prefix ]$mountProtocol | Body | Object | ボリュームマウントプロトコル |
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | ボリューム CIFS 認証 ID リスト |
| $[ prefix ]$mountProtocol.protocol | Body | String | ボリュームマウントプロトコル |
| $[ prefix ]$snapshotPolicy | Body | Object | ボリュームスナップショット設定オブジェクト |
| $[ prefix ]$snapshotPolicy.maxScheduledCount | Body | Integer | スナップショット最大保存数 |
| $[ prefix ]$snapshotPolicy.reservePercent | Body | Integer | スナップショット容量割合 |
| $[ prefix ]$snapshotPolicy.schedule | Body | Object | スナップショット自動作成オブジェクト |
| $[ prefix ]$snapshotPolicy.schedule.time | Body | String | スナップショット自動作成時間 |
| $[ prefix ]$snapshotPolicy.schedule.timeOffset | Body | String | スナップショット自動作成基準タイムゾーン |
| $[ prefix ]$snapshotPolicy.schedule.weekdays | Body | List | スナップショット自動作成曜日<br>空のリストは毎日を意味し、曜日は 0 (日曜日) から 6 (土曜日) までの数字のリストで指定します。 |
| $[ prefix ]$createdAt | Body | String | ボリューム作成日時 |
| $[ prefix ]$updatedAt | Body | String | ボリューム更新日時 |{% endmacro %}
{# end macro volume_response_table #}
{% macro volume_request_table(prefix='', method='') -%}
| $[ prefix ]$acl | Body | List | N | ボリューム作成時に設定する ACL リスト<br>IP または CIDR 形式で入力できます。 |
| $[ prefix ]$description | Body | String | N | ボリュームの説明 |
{%- if method == 'post' %}
{%- if encryption %}
| $[ prefix ]$encryption | Body | Object | N | ボリューム作成時の暗号化設定オブジェクト |
| $[ prefix ]$encryption.enabled | Body | Boolean | N | 暗号化設定の有効可否<br>暗号化キーストアが設定された後、該当フィールドを `true` に設定すると暗号化が有効になります。 |
{%- endif %}
{%- endif %}
{%- if method == 'post' %}
| $[ prefix ]$interfaces | Body | List | N | ボリュームにアクセスするインターフェイスリスト |
| $[ prefix ]$interfaces.subnetId | Body | String | N | ボリュームインターフェイスのサブネット ID |
{%- endif %}
| $[ prefix ]$mountProtocol | Body | Object | N | ボリューム作成時のプロトコル設定オブジェクト |
{%- if method == 'post' %}
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | N | CIFS 認証 ID リスト<br>NFS プロトコル選択時は入力不要 |
| $[ prefix ]$mountProtocol.protocol | Body | String | Y | ボリュームマウント時のプロトコル指定<br>`nfs`、`cifs` のいずれかを選択できます。 |
{%- elif method == 'patch' %}
| $[ prefix ]$mountProtocol.cifsAuthIds | Body | List | N | CIFS 認証 ID リスト |
| $[ prefix ]$mountProtocol.protocol | Body | String | N | 既に作成されたボリュームのプロトコルは変更できません。<br>`cifsAuthIds` フィールド変更時は、該当フィールドに `cifs` を明示する必要があります。 |
{%- endif %}
{%- if method == 'post' %}
| $[ prefix ]$name | Body | String | Y | ボリューム名 |
{%- endif %}
| $[ prefix ]$sizeGb | Body | Integer | $[ 'Y' if method == 'post'  else 'N' ]$ | ボリュームサイズ (GB)<br>ボリュームは最小 300 GB から最大 10,000 GB まで、100 GB 単位で設定できます。 |
| $[ prefix ]$snapshotPolicy | Body | Object | N | ボリュームスナップショット設定オブジェクト |
| $[ prefix ]$snapshotPolicy.maxScheduledCount | Body | Integer | N | スナップショット最大保存数<br>30 個まで設定可能で、最大保存数に達すると、自動生成されたスナップショットのうち最も先に生成されたスナップショットが削除されます。 |
| $[ prefix ]$snapshotPolicy.reservePercent | Body | Integer | N | スナップショット容量割合 |
| $[ prefix ]$snapshotPolicy.schedule | Body | Object | N | スナップショット自動作成オブジェクト<br>`null` の場合、スナップショット自動作成は設定されません。 |
| $[ prefix ]$snapshotPolicy.schedule.time | Body | String | N | スナップショット自動作成時間 |
| $[ prefix ]$snapshotPolicy.schedule.timeOffset | Body | String | N | スナップショット自動作成基準タイムゾーン |
| $[ prefix ]$snapshotPolicy.schedule.weekdays | Body | List | N | スナップショット自動作成曜日<br>空のリストは毎日を意味し、曜日は 0 (日曜日) から 6 (土曜日) までの数字のリストで指定します。 |{% endmacro %}
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
{%- if encryption %}
$[ ' ' * indent ]$"encryption": {
$[ ' ' * indent ]$  "enabled": false
$[ ' ' * indent ]$},
{%- endif %}
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
{%- if method == 'post' %}
$[ ' ' * indent ]$"mirrors": []
{% else %}
$[ ' ' * indent ]$"mirrors": [
$[ ' ' * indent ]$  {
$[ volume_mirror_response_json(indent+4) ]$
$[ ' ' * indent ]$  }
$[ ' ' * indent ]$],
{%- endif %}
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
| $[ prefix ]$id | Body | String | スナップショット ID |
| $[ prefix ]$name | Body | String | スナップショット名 |
| $[ prefix ]$size | Body | Integer | スナップショットサイズ |
| $[ prefix ]$type | Body | String | スナップショットタイプ<br>- `NORMAL`: ユーザーが作成したスナップショット<br>- `SCHEDULED`: スナップショット自動作成で作成されたスナップショット<br>- `MIRROR`: 複製で作成されたスナップショット |
| $[ prefix ]$preserved | Body | Boolean | システムが削除不可に設定したスナップショットかどうか |
| $[ prefix ]$createdAt | Body | String | スナップショット作成日時 |{% endmacro %}
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
## Storage > NAS > API ガイド { #storage-nas-api-guide }

この文書は、NHN Cloud NAS が提供する API を使用してボリュームとスナップショットを管理する方法について説明します。

<a id="nas_api_common"></a>
## NAS API 共通情報 { #nas_api_common }

<a id="nas_api_common.endpoint"></a>
### API エンドポイント { #nas_api_common.endpoint }

NASAPIは`nasv1`タイプのエンドポイントを使用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| リージョン | エンドポイント |
| --- | --- |
{% for region in regions %}| $[ region.name ]$ | $[ region.endpoint ]$ |
{% endfor %}
<a id="nas_api_common.authentication"></a>

### 認証及び権限 { #nas_api_common.authentication }

NAS は API 呼び出し時の認証/認可のために IaaS トークンを使用します。IaaS トークンは、NHN Cloud の OpenStack ベースのインフラサービス (IaaS) で使用する認証トークンです。
IaaS トークンの発行および使用方法については、[IaaS トークン]($[ identity_guide_url ]$)を参照してください。

<a id="nas_api_common.response"></a>
### レスポンス共通情報 { #nas_api_common.response }

NASAPIが提供する共通レスポンス情報の説明です。全てのAPIレスポンスは`header`オブジェクトでリクエスト結果を伝達します。

| 名前 | 形式 | 説明 |
| --- | --- | --- |
| header | Object | ヘッダオブジェクト |
| header.isSuccessful | Boolean | リクエストの成否(`true`または`false`) |
| header.resultCode | Integer | HTTP ステータスコードに対応する結果コード<br>- `200`: 成功<br>- `201`: リソース作成成功<br>- `202`: リクエストは正常に受信されましたが、まだ処理されていない状態<br>- `400`: 無効な値でリクエストされました<br>- `401`: 権限、認証またはトークン関連のエラー<br>- `404`: リクエストされたリソースが見つかりません<br>- `405`: リクエストされた URL が指定した HTTP メソッドをサポートしていません<br>- `5XX`: クライアントのリクエストは有効ですが、サーバーが処理に失敗しました |
| header.resultMessage | String | リクエスト処理結果メッセージ |

<details>
  <summary><strong>成功レスポンス</strong></summary>

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
  <summary><strong>失敗レスポンス</strong></summary>

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

!!! tip "ヒント"
    API レスポンスには、ガイドに記載されていないフィールドが含まれる場合があります。これらのフィールドは NHN Cloud 内部用途で使用されており、事前通知なしに変更される可能性があるため、使用しないでください。

<a id="volume"></a>
## ボリューム { #volume }

<a id="volume.list"></a>
### ボリューム一覧表示 { #volume.list }

ボリューム一覧を照会します。

```
GET /v1/volumes
X-Auth-Token: {token-id}
```

<a id="volume.list-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| sizeGb | Query | String | N | ボリュームサイズ |
| maxSizeGb | Query | String | N | ボリューム最大サイズ |
| minSizeGb | Query | String | N | ボリューム最小サイズ |
| name | Query | String | N | ボリューム名 |
| nameContains | Query | String | N | ボリューム名に含まれる文字列 |
| subnetId | Query | String | N | サブネットのインターフェースを持つボリューム |
| limit | Query | String | N | 1ページに表示するリソース数 |
| page | Query | String | N | 照会するページ |
| sort | Query | String | N | ソート基準となるフィールド名<br>`{key}:{direction}`の形で記述します。例：`name:asc`, `created_at:desc`<br>使用可能なkey値: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

<a id="volume.list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| paging | Body | Object | ページ情報 |
| paging.limit | Body | Integer | 1ページに表示されるリソース数 |
| paging.page | Body | Integer | 現在ページ番号 |
| paging.totalCount | Body | Integer | 全体数 |
| volumes | Body | List | ボリュームオブジェクトリスト |
$[ volume_response_table('volumes.') ]$

<details>
  <summary>レスポンス例</summary>

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
### ボリューム作成 { #volume.create }

新しいボリュームを作成します。

!!! tip "参考: CIFSプロトコル使用"
    CIFSプロトコルを使用するには、CIFS認証情報を作成する必要があります。認証情報はプロジェクト単位で管理され、CIFSボリュームごとにアクセスを許可するCIFS認証情報を登録する必要があります。
    CIFS認証情報はコンソールの **Storage > NAS > CIFS認証情報管理**ウィンドウから作成できます。


{% if encryption %}
<!-- -->

!!! tip "参考: 暗号化キーストア設定"
    暗号化ボリュームを作成すると、暗号化に使用する共通鍵がNHN Cloud Secure Key Managerサービスのキーストアに保存されます。したがって、暗号化ボリュームを作成するには、事前にSecure Key Managerサービスで[キーストアを作成](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ja/getting-started/#_1)する必要があります。[キーストアのIDを確認](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ja/getting-started/#_2)し、暗号化キーストア設定に入力します。
    作成したキーストアIDはコンソールの **Storage > NAS > 暗号化キーストア設定** ウィンドウで入力できます。暗号化ボリュームを作成すると、設定したキーストアに共通鍵が保存されます。 NASサービスによってキーストアに保存された共通鍵は暗号化ボリューム使用中には削除できません。暗号化ボリュームを削除すると、共通鍵も一緒に削除されます。
    キーストアIDを変更すると、その後に作成する暗号化ボリュームの共通鍵が変更されたキーストアに保存されます。既存キーストアに保存された共通鍵は維持されます。


{% endif %}
```
POST /v1/volumes
X-Auth-Token: {token-id}
```

<br>

<a id="volume.create-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume | Body | Object | Y | ボリューム作成リクエストオブジェクト |
$[ volume_request_table('volume.', 'post') ]$

<details>
  <summary>リクエスト例</summary>

```json
{
  "volume": {
    "acl": [
      "10.0.1.0/24"
    ],
    "description": "NAS for Testing",
{%- if encryption %}
    "encryption": {
      "enabled": true
    },
{%- endif %}
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
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volume | Body | Object | ボリュームオブジェクト |
$[ volume_response_table('volume.') ]$

<details>
  <summary>レスポンス例</summary>

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
### ボリューム削除 { #volume.delete }

指定したボリュームを削除します。

```
DELETE /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | 削除するボリュームID |

<a id="volume.delete-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="volume.view"></a>
### ボリューム表示 { #volume.view }

指定したボリュームの詳細情報を返します。

```
GET /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.view-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | 照会するボリュームID |

<a id="volume.view-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volume | Body | Object | ボリュームオブジェクト |
$[ volume_response_table('volume.') ]$

<br>

<a id="volume.change_settings"></a>
### ボリュームの設定変更 { #volume.change_settings }

指定したボリュームの設定を変更します。

!!! danger "注意"
    複製設定されたボリュームのサイズを変更するにはソースボリュームと対象ボリュームの両方を変更する必要があります。ソースボリュームと対象ボリュームのサイズが異なる場合、複製に失敗する可能性があります。

```
PATCH /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

<a id="volume.change_settings-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| volume | Body | Object | Y | ボリューム設定変更リクエストオブジェクト |
$[ volume_request_table('volume.', 'patch') ]$

<details>
  <summary>リクエスト例</summary>

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
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="volume.connect_interface"></a>
### ボリュームにインターフェース接続 { #volume.connect_interface }

指定したボリュームのインターフェイスを設定します。
設定されたアドレスおよびサブネットからボリュームにアクセスできます。アクセス可能な IP はアクセス制御 (ACL) で別途設定する必要があります。

```
POST /v1/volumes/{volume_id}/interfaces
X-Auth-Token: {token-id}
```

<a id="volume.connect_interface-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| interface | Body | Object | Y | インターフェース設定オブジェクト |
| interface.subnetId | Body | String | Y | インターフェースサブネット指定 |

<details>
  <summary>リクエスト例</summary>

```json
{
  "interface":{
    "subnetId":"3e5b4d63-d143-420a-9263-208a447a2a3f"
  }
}
```

</details>

<a id="volume.connect_interface-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| interface | Body | Object | 作成されたインターフェース情報オブジェクト |
$[ interface_response_table('interface.', '作成された ') ]$

<details>
  <summary>レスポンス例</summary>

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
### ボリュームのインターフェース削除 { #volume.delete_interface }

指定したボリュームの指定したインターフェースを削除します。

```
DELETE /v1/volumes/{volume_id}/interfaces/{interface_id}
X-Auth-Token: {token-id}
```

<a id="volume.delete_interface-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| interface\_id | URL | String | Y | 削除するインターフェースID |

<a id="volume.delete_interface-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="volume.view_snapshot_restore_history"></a>
### スナップショット復元履歴を表示 { #volume.view_snapshot_restore_history }

指定したボリュームのスナップショット復元履歴リストを返します。

```
GET /v1/volumes/{volume_id}/restore-histories
X-Auth-Token: {token-id}
```

<a id="volume.view_snapshot_restore_history-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| limit | Query | String | N | 1ページに表示するリソース数 |
| page | Query | String | N | 照会するページ |
| sort | Query | String | N | ソート基準となるフィールド名<br>`{key}:{direction}`の形で記述します。例：`snapshotId:asc`, `requestedAt:desc`<br>使用可能なkey値: `snapshotId`, `snapshotName`, `requestedAt`, `restoredAt`, `requestedUser`, `requestedIp`, `result` |

<a id="volume.view_snapshot_restore_history-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| paging | Body | Object | ページ情報 |
| paging.limit | Body | Integer | 1ページに表示されるリソース数 |
| paging.page | Body | Integer | 現在ページ番号 |
| paging.totalCount | Body | Integer | 全体数 |
| restoreHistories | Body | List | スナップショット復元履歴オブジェクトリスト |
| restoreHistories.requestedAt | Body | String | スナップショット復元をリクエストした時刻 |
| restoreHistories.requestedIp | Body | String | スナップショット復元をリクエストしたアドレス |
| restoreHistories.requestedUser | Body | String | スナップショット復元をリクエストしたユーザーID |
| restoreHistories.restoredAt | Body | String | スナップショット復元を完了した時刻 |
| restoreHistories.result | Body | String | スナップショット復元結果 |
| restoreHistories.snapshotId | Body | String | 復元対象スナップショットID |
| restoreHistories.snapshotName | Body | String | 復元対象スナップショット名 |
| restoreHistories.volumeId | Body | String | 復元したボリュームのID |

<details>
  <summary>レスポンス例</summary>

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
### ボリューム使用状況表示 { #volume.view_usage }

指定したボリュームの使用状況を返します。

```
GET /v1/volumes/{volume_id}/usage
X-Auth-Token: {token-id}
```

<a id="volume.view_usage-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |

<a id="volume.view_usage-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| usage | Body | Object | ボリューム使用状況オブジェクト |
| usage.snapshotReserveGb | Body | Integer | ボリュームでスナップショットのために予約したスペースサイズ |
| usage.snapshotUsedGb | Body | Integer | スナップショット使用量 |
| usage.snapshotUsedGbInReservedSpace | Body | Integer | スナップショット予約容量内の使用量 |
| usage.snapshotUsedGbInUserSpace | Body | Integer | 予約容量を超過したスナップショット使用量 |
| usage.usedGb | Body | Integer | ボリューム使用量 |
| usage.userDataGb | Body | Integer | ユーザーが実際に記録したデータサイズ |

<details>
  <summary>レスポンス例</summary>

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
## スナップショット { #snapshots }

<a id="snapshots.list"></a>
### スナップショットリスト表示 { #snapshots.list }

スナップショットリストを照会します。

```
GET /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.list-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |

<a id="snapshots.list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| snapshots | Body | List | スナップショット情報オブジェクトリスト |
$[ snapshot_response_table('snapshots.') ]$

<details><summary>レスポンス例</summary>

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
### スナップショット作成 { #snapshots.create }

指定したボリュームのスナップショットを作成します。

```
POST /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

<a id="snapshots.create-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| snapshot | Body | Object | Y | スナップショット作成オブジェクト |
| snapshot.name | Body | String | Y | スナップショット名 |

<details>
  <summary>リクエスト例</summary>

```json
{
  "snapshot": {
    "name": "TEST-SNAPSHOT-1"
  }
}
```

</details>

<a id="snapshots.create-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| snapshot | Body | Object | スナップショット情報オブジェクト |
$[ snapshot_response_table('snapshot.') ]$

<details>
  <summary>レスポンス例</summary>

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
### スナップショット削除 { #snapshots.delete }

指定したボリュームのスナップショットを削除します。

```
DELETE /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.delete-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| snapshot\_id | URL | String | Y | スナップショットID |

<a id="snapshots.delete-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="snapshots.view"></a>
### スナップショット表示 { #snapshots.view }

指定したスナップショットの詳細情報を返します。

```
GET /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

<a id="snapshots.view-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| snapshot\_id | URL | String | Y | スナップショットID |
| showReclaimableSpace | Query | Boolean | N | スナップショット削除時に確保される容量を示す`reclaimableSpace`項目を表示するかどうか |

<a id="snapshots.view-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| snapshot | Body | Object | スナップショット情報オブジェクト |
$[ snapshot_response_table('snapshot.') ]$

<br>

<a id="snapshots.restore"></a>
### スナップショット復元 { #snapshots.restore }

指定したスナップショットでボリュームを復元します。

```
POST /v1/volumes/{volume_id}/snapshots/{snapshot_id}/restore
X-Auth-Token: {token-id}
```

<a id="snapshots.restore-request"></a>
#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| snapshot\_id | URL | String | Y | スナップショットID |

<a id="snapshots.restore-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

{% if replication %}

<a id="replication"></a>
## ボリューム複製設定 { #replication }

<a id="replication.setup"></a>
### 複製設定 { #replication.setup }

指定したボリュームの複製を設定します。
複製対象プロジェクトごとに選択可能なリージョン範囲は、以下の表で確認できます。

| 対象プロジェクト | 選択可能なリージョン |
| ------- | --------- |
| 組織内の同一プロジェクト | 他のリージョン |
| 組織内他のプロジェクト | 全てのリージョン |

<br>

!!! danger "注意"
    複製対象ボリュームサイズはソースボリュームと同じサイズに設定する必要があります。ソースボリュームと対象ボリュームのサイズが異なる場合、複製に失敗する可能性があります。

<!-- -->

!!! tip "ヒント"
    複製先ボリュームに暗号化を設定するには、複製先ボリュームが属するプロジェクトまたはリージョンに、ソースボリュームとは別の暗号化キーストアを設定する必要があります。

<!-- -->

!!! tip "ヒント"
    ソースボリュームが CIFS プロトコルを使用している場合、ターゲットボリュームも CIFS プロトコルを使用する必要があります。そのため、ソースボリュームとは別の CIFS 認証情報を作成し、リクエスト本文の `cifsAuthIds` フィールドに入力する必要があります。

```
POST /v1/volumes/{volume_id}/volume-mirrors
X-Auth-Token: {token-id}
```

<a id="replication.setup-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | 原本ボリュームID |
| volumeMirror | Body | Object | Y | ボリューム複製設定リクエストオブジェクト |
| volumeMirror.dstRegion | Body | String | Y | 複製対象ボリュームのリージョン |
| volumeMirror.dstTenantId | Body | String | Y | 複製対象ボリュームのテナントID |
| volumeMirror.dstVolume | Body | Object | Y | 複製対象ボリューム作成リクエストオブジェクト |
$[ volume_request_table('volumeMirror.dstVolume.', 'post') ]$

<details>
  <summary>リクエスト例</summary>

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
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volumeMirror | Body | Object | 複製設定作成オブジェクト |
$[ volume_mirror_response_table('volumeMirror.') ]$

<details>
  <summary>レスポンス例</summary>

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
### 複製設定の解除 { #replication.disable }

指定したボリュームの複製設定を解除します。

```
DELETE /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}
X-Auth-Token: {token-id}
```

<a id="replication.disable-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| volume\_mirror\_id | URL | String | Y | 複製設定ID |

<a id="replication.disable-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication.change_direction"></a>
### 複製方向の変更 { #replication.change_direction }

ソースボリュームと対象ボリュームの複製方向を変更します。

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/invert-direction
X-Auth-Token: {token-id}
```

<a id="replication.change_direction-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| volume\_mirror\_id | URL | String | Y | 複製設定ID |

<a id="replication.change_direction-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication.start"></a>
### 複製開始 { #replication.start }

ソースボリュームから対象ボリュームへの複製を開始します。

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/start
X-Auth-Token: {token-id}
```

<a id="replication.start-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| volume\_mirror\_id | URL | String | Y | 複製設定ID |

<a id="replication.start-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication.status"></a>
### 複製状態の確認 { #replication.status }

最近の複製状態を返します。

```
GET /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stat
X-Auth-Token: {token-id}
```

<a id="replication.status-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| volume\_mirror\_id | URL | String | Y | 複製設定ID |

<a id="replication.status-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volumeMirrorStat | Body | Object | 複製状態オブジェクト |
| volumeMirrorStat.lastSuccessTransferBytes | Body | Integer | 最近成功した複製で転送されたデータサイズ(Byte) |
| volumeMirrorStat.lastSuccessTransferEndTime | Body | String | 最近成功した複製完了時間 |
| volumeMirrorStat.lastTransferBytes | Body | Integer | 最近実行した複製で転送されたデータサイズ(Byte) |
| volumeMirrorStat.lastTransferEndTime | Body | String | 最近実行した複製完了時間 |
| volumeMirrorStat.lastTransferStatus | Body | String | 最近の複製実行結果 |
| volumeMirrorStat.status | Body | String | 複製設定の状態<br>- `ACTIVE`: 複製アクティブ状態<br>- `UPDATING`: 設定変更中<br>- `DELETING`: 設定削除中<br>- `PENDING`: 設定作成中<br>- `HALT`: 複製停止状態<br>- `RETRIEVE FAILED`: 一時的な情報取得失敗 |

<br>

<a id="replication.stop"></a>
### 複製停止 { #replication.stop }

ソースボリュームから対象ボリュームへの複製を停止します。

```
POST /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stop
X-Auth-Token: {token-id}
```

<a id="replication.stop-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | Y | トークンID |
| volume\_id | URL | String | Y | ボリュームID |
| volume\_mirror\_id | URL | String | Y | 複製設定ID |

<a id="replication.stop-response"></a>
#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>
{%- endif %}
