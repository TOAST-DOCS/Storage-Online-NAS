## Storage > NAS > API ガイド

<a id="nas_api_common"></a>
## NAS API 共通情報

<a id="nas_api_common.endpoint"></a>
### API エンドポイント

NASAPIは`nasv1`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| リージョン | エンドポイント | 
| --- | --- |
| 韓国(パンギョ)リージョン | https://kr1-api-nas-infrastructure.nhncloudservice.com |
| 韓国(ピョンチョン)リージョン | https://kr2-api-nas-infrastructure.nhncloudservice.com |


<a id="nas_api_common.authentication"></a>
### 認証及び権限

NASは、API呼び出し時の認証/認可のためにIaaSトークンを使用します。IaaSトークンは、NHN CloudのOpenStackベースのインフラサービス(IaaS)で使用する認証トークンです。IaaSトークンの発行及び使用に関する詳細は、[IaaSトークン](/nhncloud/ja/public-api/iaas-token/)を参照してください。

<a id="nas_api_common.response"></a>
### レスポンス共通情報

NASAPIが提供する共通レスポンス情報の説明です。全てのAPIレスポンスは`header`オブジェクトを通じてリクエスト結果を伝達します。

| 名前 | 形式 | 説明 |
| --- | --- | --- |
| header | Object | ヘッダオブジェクト |
| header.isSuccessful | Boolean | リクエストの成否(`true`または`false`) |
| header.resultCode | Integer | HTTPステータスコードに該当する結果コード<br>- `200`:成功 <br>- `201`:リソース作成成功<br>- `202`:リクエストが正常に受信されたが、まだ処理されていない状態<br>- `400`:有効ではない値でリクエストされた<br>- `401`:権限、認証またはトークン関連エラー <br>- `404`:リクエストしたリソースが見つからない<br>- `405`:リクエストしたURLが指定したHTTPメソッドをサポートしていない<br>- `5XX`:クライアントのリクエストは有効ですがサーバーが処理に失敗する |
| header.resultMessage | String | リクエスト処理結果に関するメッセージ |

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

> [参考]
> APIレスポンスにガイドに記載されていないフィールドが表示される場合があります。このようなフィールドは、NHN Cloudの内部用途に使用され、事前告知なしに変更される可能性があるため、使用しないでください。

<a id="volume"></a>
## ボリューム

<a id="volume.list"></a>
### ボリューム一覧表示

ボリューム一覧を照会します。

```
GET  /v1/volumes
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| sizeGb | String | Query | - | ボリュームサイズ |
| maxSizeGb | String | Query | - | ボリューム最大サイズ |
| minSizeGb | String | Query | - | ボリューム最小サイズ |
| name | String | Query | - | ボリューム名 |
| nameContains | String | Query | - | ボリューム名に含まれる文字列 |
| subnetId | String | Query | - | サブネットのインターフェースを持つボリューム |
| limit | String | Query | - | 1ページに表示するリソース数 |
| page | String | Query | - | 照会するページ |
| sort | String | Query | - | ソート基準となるフィールド名<br>`{key}:{direction}`の形で記述します。例：`name:asc`, `created_at:desc`<br>使用可能なkey値: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| paging | Body | Object | ページ情報 |
| paging.limit | Body | Integer | 1ページに表示されるリソース数 |
| paging.page | Body | Integer | 現在ページ番号 |
| paging.totalCount | Body | Integer | 全体数 |
| volumes | Body | List | ボリュームオブジェクトリスト |
| volumes.id | Body | String | ボリュームID |
| volumes.name | Body | String | ボリューム名 |
| volumes.status | Body | String | ボリュームの状態 |
| volumes.description | Body | String | ボリュームの説明 |
| volumes.sizeGb | Body | Integer | ボリュームサイズ(GB) |
| volumes.projectId | Body | String | ボリュームが属するプロジェクトID |
| volumes.tenantId | Body | String | ボリュームが属するテナントID |
| volumes.acl | Body | List | ボリュームACLリスト |
| volumes.encryption | Body | Object | ボリューム暗号化情報 |
| volumes.encryption.enabled | Body | Boolean | ボリュームの暗号化が有効かどうか |
| volumes.encryption.keys | Body | List | ボリューム暗号化キー情報 |
| volumes.interfaces | Body | List | ボリュームインターフェースオブジェクトリスト |
| volumes.interfaces.id | Body | String | インターフェースID |
| volumes.interfaces.path | Body | String | インターフェースパス |
| volumes.interfaces.status | Body | String | インターフェース状態 |
| volumes.interfaces.subnetId | Body | String | インターフェースのサブネットID |
| volumes.interfaces.tenantId | Body | String | インターフェースのテナントID |
| volumes.mirrors | Body | List | ボリューム複製設定オブジェクトリスト |
| volumes.mirrors.id | Body | String | 複製設定ID |
| volumes.mirrors.role | Body | String | 複製ロール<br>- `SOURCE`:ソースボリューム<br>- `DESTINATION`:対象ボリューム |
| volumes.mirrors.status | Body | String | 複製設定状態<br>- `INITIALIZED`:設定完了<br>- `UPDATING`:設定変更中<br>- `DELETING`:設定削除中<br>- `PENDING`:設定作成中 |
| volumes.mirrors.direction | Body | String | 複製方向 <br>- `FORWARD`:ソースボリューム→複製ボリューム <br>- `REVERSE`:複製ボリューム→ソースボリューム |
| volumes.mirrors.directionChangedAt | Body | String | 複製方向変更時刻 |
| volumes.mirrors.dstProjectId | Body | String | 複製対象ボリュームのプロジェクトID |
| volumes.mirrors.dstRegion | Body | String | 複製対象ボリュームリージョン |
| volumes.mirrors.dstTenantId | Body | String | 複製対象ボリュームテナントID |
| volumes.mirrors.dstVolumeId | Body | String | 複製対象ボリュームID |
| volumes.mirrors.dstVolumeName | Body | String | 複製対象ボリューム名 |
| volumes.mirrors.srcProjectId | Body | String | ソースボリュームのプロジェクトID |
| volumes.mirrors.srcRegion | Body | String | ソースボリュームリージョン |
| volumes.mirrors.srcTenantId | Body | String | ソースボリュームテナントID |
| volumes.mirrors.srcVolumeId | Body | String | ソースボリュームID |
| volumes.mirrors.srcVolumeName | Body | String | ソースボリューム名 |
| volumes.mirrors.createdAt | Body | String | 複製作成時刻 |
| volumes.mountProtocol | Body | Object | ボリュームマウントプロトコル |
| volumes.mountProtocol.cifsAuthIds | Body | List | ボリュームCIFS認証IDリスト |
| volumes.mountProtocol.protocol | Body | String | ボリュームマウントプロトコル |
| volumes.snapshotPolicy | Body | Object | ボリュームスナップショット設定オブジェクト |
| volumes.snapshotPolicy.maxScheduledCount | Body | Integer | スナップショット最大保存数 |
| volumes.snapshotPolicy.reservePercent | Body | Integer | スナップショット容量比率 |
| volumes.snapshotPolicy.schedule | Body | Object | スナップショット自動作成オブジェクト |
| volumes.snapshotPolicy.schedule.time | Body | String | スナップショット自動作成時間 |
| volumes.snapshotPolicy.schedule.timeOffset | Body | String | スナップショット自動作成基準タイムゾーン |
| volumes.snapshotPolicy.schedule.weekdays | Body | List | スナップショット自動作成曜日<br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |
| volumes.createdAt | Body | String | ボリューム作成時刻 |
| volumes.updatedAt | Body | String | ボリューム変更時刻 |

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
### ボリューム作成

新しいボリュームを作成します。

> [参考] CIFSプロトコル使用
> CIFSプロトコルを使用するためには、CIFS認証情報を作成する必要があります。認証情報はプロジェクト単位で管理され、CIFSボリュームごとにアクセスを許可するCIFS認証情報を登録する必要があります。
> CIFS認証情報はコンソールの **Storage > NAS > CIFS認証情報管理**ウィンドウから作成できます。


<!-- -->

> [参考]暗号化キーストア設定
> 暗号化ボリュームを作成すると、暗号化に使用する共通鍵がNHN Cloud Secure Key Managerサービスのキーストアに保存されます。したがって、暗号化ボリュームを作成するためには、事前にSecure Key Managerサービスで[キーストアを作成](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ja/getting-started/#_1)する必要があります。[キーストアのIDを確認](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ja/getting-started/#_2)し、暗号化キーストア設定に入力します。
> 作成したキーストアIDはコンソールの **Storage > NAS > 暗号化キーストア設定** ウィンドウで入力できます。暗号化ボリュームを作成すると、設定したキーストアに共通鍵が保存されます。 NASサービスによってキーストアに保存された共通鍵は暗号化ボリューム使用中には削除できません。暗号化ボリュームを削除すると、共通鍵も一緒に削除されます。
> キーストアIDを変更すると、その後に作成する暗号化ボリュームの共通鍵が変更されたキーストアに保存されます。既存キーストアに保存された共通鍵は維持されます。


```
POST  /v1/volumes
X-Auth-Token: {token-id}
```

<br>

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume | Body | Object | O | ボリューム作成リクエストオブジェクト |
| volume.acl | Body | List | - | ボリューム作成時設定するACLリスト<br>IPまたはCIDR形式で入力できます。 |
| volume.description | Body | String | - | ボリュームの説明 |
| volume.encryption | Body | Object | - | ボリューム作成時暗号化設定オブジェクト |
| volume.encryption.enabled | Body | Boolean | - | 暗号化設定有効かどうか<br>暗号化キーストアが設定された後、該当フィールドを`true`に設定すると、暗号化が有効になります。 |
| volume.interfaces | Body | List | - | ボリュームにアクセスするインターフェースリスト |
| volume.interfaces.subnetId | Body | String | - | ボリュームインターフェースのサブネットID |
| volume.mountProtocol | Body | Object | - | ボリュームを作成する際のプロトコル設定オブジェクト |
| volume.mountProtocol.cifsAuthIds | Body | List | - | CIFS認証IDリスト<br>NFSプロトコル選択時入力不要 |
| volume.mountProtocol.protocol | Body | String | O | ボリュームをマウントする際のプロトコル指定<br>`nfs`, `cifs`のいずれかを選択できます。 |
| volume.name | Body | String | O | ボリューム名 |
| volume.sizeGb | Body | Integer | O | ボリュームサイズ(GB)<br>ボリュームは、最小300GBから最大10,000GBまで、100GB単位で設定できます。 |
| volume.snapshotPolicy | Body | Object | - | ボリュームスナップショット設定オブジェクト |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | スナップショット最大保存数<br>30個まで設定可能で、最大保存数に達する作成されたスナップショット中のうち、先に作成されたスナップショットがと自動的に削除されます。 |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | スナップショット容量比率 |
| volume.snapshotPolicy.schedule | Body | Object | - | スナップショット自動作成オブジェクト<br>`null`の場合、スナップショット自動作成が設定されません。 |
| volume.snapshotPolicy.schedule.time | Body | String | - | スナップショット自動作成時間 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | スナップショット自動作成基準タイムゾーン |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | スナップショット自動作成曜日。 <br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |

<details>
  <summary>リクエスト例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volume | Body | Object | ボリュームオブジェクト |
| volume.id | Body | String | ボリュームID |
| volume.name | Body | String | ボリューム名 |
| volume.status | Body | String | ボリュームの状態 |
| volume.description | Body | String | ボリュームの説明 |
| volume.sizeGb | Body | Integer | ボリュームサイズ(GB) |
| volume.projectId | Body | String | ボリュームが属するプロジェクトID |
| volume.tenantId | Body | String | ボリュームが属するテナントID |
| volume.acl | Body | List | ボリュームACLリスト |
| volume.encryption | Body | Object | ボリューム暗号化情報 |
| volume.encryption.enabled | Body | Boolean | ボリュームの暗号化が有効かどうか |
| volume.encryption.keys | Body | List | ボリューム暗号化キー情報 |
| volume.interfaces | Body | List | ボリュームインターフェースオブジェクトリスト |
| volume.interfaces.id | Body | String | インターフェースID |
| volume.interfaces.path | Body | String | インターフェースパス |
| volume.interfaces.status | Body | String | インターフェースの状態 |
| volume.interfaces.subnetId | Body | String | インターフェースのサブネットID |
| volume.interfaces.tenantId | Body | String | インターフェースのテナントID |
| volume.mirrors | Body | List | ボリューム複製設定オブジェクトリスト |
| volume.mirrors.id | Body | String | 複製設定ID |
| volume.mirrors.role | Body | String | 複製役割<br>- `SOURCE`:ソースボリューム<br>- `DESTINATION`:対象ボリューム |
| volume.mirrors.status | Body | String | 複製設定状態<br>- `INITIALIZED`:設定完了<br>- `UPDATING`:設定変更中<br>- `DELETING`:設定削除中<br>- `PENDING`:設定作成中 |
| volume.mirrors.direction | Body | String | 複製方向 <br>- `FORWARD`:ソースボリューム -> 複製ボリューム<br>- `REVERSE`:複製ボリューム -> ソースボリューム |
| volume.mirrors.directionChangedAt | Body | String | 複製方向変更時刻 |
| volume.mirrors.dstProjectId | Body | String | 複製対象ボリュームのプロジェクトID |
| volume.mirrors.dstRegion | Body | String | 複製対象ボリュームリージョン |
| volume.mirrors.dstTenantId | Body | String | 複製対象ボリュームテナントID |
| volume.mirrors.dstVolumeId | Body | String | 複製対象ボリュームID |
| volume.mirrors.dstVolumeName | Body | String | 複製対象ボリューム名 |
| volume.mirrors.srcProjectId | Body | String | ソースボリュームのプロジェクトID |
| volume.mirrors.srcRegion | Body | String | ソースボリュームリージョン |
| volume.mirrors.srcTenantId | Body | String | ソースボリュームテナントID |
| volume.mirrors.srcVolumeId | Body | String | ソースボリュームID |
| volume.mirrors.srcVolumeName | Body | String | ソースボリューム名 |
| volume.mirrors.createdAt | Body | String | 複製作成時刻 |
| volume.mountProtocol | Body | Object | ボリュームマウントプロトコル |
| volume.mountProtocol.cifsAuthIds | Body | List | ボリュームCIFS認証IDリスト |
| volume.mountProtocol.protocol | Body | String | ボリュームマウントプロトコル |
| volume.snapshotPolicy | Body | Object | ボリュームスナップショット設定オブジェクト |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | スナップショット最大保存数 |
| volume.snapshotPolicy.reservePercent | Body | Integer | スナップショット容量比率 |
| volume.snapshotPolicy.schedule | Body | Object | スナップショット自動作成オブジェクト |
| volume.snapshotPolicy.schedule.time | Body | String | スナップショット自動作成時間 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | スナップショット自動作成基準タイムゾーン |
| volume.snapshotPolicy.schedule.weekdays | Body | List | スナップショット自動作成曜日<br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |
| volume.createdAt | Body | String | ボリューム作成時刻 |
| volume.updatedAt | Body | String | ボリューム変更時刻 |

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
### ボリューム削除

指定したボリュームを削除します。

```
DELETE  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | 削除するボリュームID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="volume.view"></a>
### ボリューム表示

指定したボリュームの詳細情報を返します。

```
GET   /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | 照会するボリュームID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volume | Body | Object | ボリュームオブジェクト |
| volume.id | Body | String | ボリュームID |
| volume.name | Body | String | ボリューム名 |
| volume.status | Body | String | ボリュームの状態 |
| volume.description | Body | String | ボリュームの説明 |
| volume.sizeGb | Body | Integer | ボリュームサイズ(GB) |
| volume.projectId | Body | String | ボリュームが属するプロジェクトID |
| volume.tenantId | Body | String | ボリュームが属するテナントID |
| volume.acl | Body | List | ボリュームACLリスト |
| volume.encryption | Body | Object | ボリューム暗号化情報 |
| volume.encryption.enabled | Body | Boolean | ボリュームの暗号化が有効かどうか |
| volume.encryption.keys | Body | List | ボリューム暗号化キー情報 |
| volume.interfaces | Body | List | ボリュームインターフェースオブジェクトリスト |
| volume.interfaces.id | Body | String | インターフェースID |
| volume.interfaces.path | Body | String | インターフェースパス |
| volume.interfaces.status | Body | String | インターフェースの状態 |
| volume.interfaces.subnetId | Body | String | インターフェースのサブネットID |
| volume.interfaces.tenantId | Body | String | インターフェースのテナントID |
| volume.mirrors | Body | List | ボリューム複製設定オブジェクトリスト |
| volume.mirrors.id | Body | String | 複製設定ID |
| volume.mirrors.role | Body | String | 複製役割<br>- `SOURCE`:ソースボリューム<br>- `DESTINATION`:対象ボリューム |
| volume.mirrors.status | Body | String | 複製設定状態<br>- `INITIALIZED`:設定完了<br>- `UPDATING`:設定変更中<br>- `DELETING`:設定削除中<br>- `PENDING`:設定作成中 |
| volume.mirrors.direction | Body | String | 複製方向 <br>- `FORWARD`:ソースボリューム -> 複製ボリューム<br>- `REVERSE`:複製ボリューム -> ソースボリューム |
| volume.mirrors.directionChangedAt | Body | String | 複製方向変更時刻 |
| volume.mirrors.dstProjectId | Body | String | 複製対象ボリュームのプロジェクトID |
| volume.mirrors.dstRegion | Body | String | 複製対象ボリュームリージョン |
| volume.mirrors.dstTenantId | Body | String | 複製対象ボリュームテナントID |
| volume.mirrors.dstVolumeId | Body | String | 複製対象ボリュームID |
| volume.mirrors.dstVolumeName | Body | String | 複製対象ボリューム名 |
| volume.mirrors.srcProjectId | Body | String | ソースボリュームのプロジェクトID |
| volume.mirrors.srcRegion | Body | String | ソースボリュームリージョン |
| volume.mirrors.srcTenantId | Body | String | ソースボリュームテナントID |
| volume.mirrors.srcVolumeId | Body | String | ソースボリュームID |
| volume.mirrors.srcVolumeName | Body | String | ソースボリューム名 |
| volume.mirrors.createdAt | Body | String | 複製作成時刻 |
| volume.mountProtocol | Body | Object | ボリュームマウントプロトコル |
| volume.mountProtocol.cifsAuthIds | Body | List | ボリュームCIFS認証IDリスト |
| volume.mountProtocol.protocol | Body | String | ボリュームマウントプロトコル |
| volume.snapshotPolicy | Body | Object | ボリュームスナップショット設定オブジェクト |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | スナップショット最大保存数 |
| volume.snapshotPolicy.reservePercent | Body | Integer | スナップショット容量比率 |
| volume.snapshotPolicy.schedule | Body | Object | スナップショット自動作成オブジェクト |
| volume.snapshotPolicy.schedule.time | Body | String | スナップショット自動作成時間 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | スナップショット自動作成基準タイムゾーン |
| volume.snapshotPolicy.schedule.weekdays | Body | List | スナップショット自動作成曜日<br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |
| volume.createdAt | Body | String | ボリューム作成時刻 |
| volume.updatedAt | Body | String | ボリューム変更時刻 |

<br>

<a id="volume.change_settings"></a>
### ボリュームの設定変更

指定したボリュームの設定を変更します。

> [注意]
> 複製設定されたボリュームのサイズを変更するにはソースボリュームと対象ボリュームの両方を変更する必要があります。ソースボリュームと対象ボリュームのサイズが異なる場合、複製に失敗する可能性があります。

```
PATCH  /v1/volumes/{volume_id}
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| volume | Body | Object | O | ボリューム作成リクエストオブジェクト |
| volume.acl | Body | List | - | ボリューム作成時に設定するACLのリスト<br>IPまたはCIDR形式で入力できます。 |
| volume.description | Body | String | - | ボリュームの説明 |
| volume.mountProtocol | Body | Object | - | ボリュームを作成する際のプロトコル設定オブジェクト |
| volume.mountProtocol.cifsAuthIds | Body | List | - | CIFS認証IDリスト |
| volume.mountProtocol.protocol | Body | String | - | すでに作成されたボリュームのプロトコルは変更できません。<br>`cifsAuthIds`フィールドを変更する場合は該当フィールドに`cifs`を明示する必要があります。 |
| volume.sizeGb | Body | Integer | - | ボリュームサイズ(GB)<br>ボリュームは、最小300GBから最大10,000GBまで、100GB単位で設定できます。 |
| volume.snapshotPolicy | Body | Object | - | ボリュームスナップショット設定オブジェクト |
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | スナップショット最大保存数<br>30個まで設定可能で、最大保存数に達する作成されたスナップショット中のうち、先に作成されたスナップショットがと自動的に削除されます。 |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | スナップショット容量比率 |
| volume.snapshotPolicy.schedule | Body | Object | - | スナップショット自動作成オブジェクト<br>`null`の場合、スナップショット自動作成が設定されません。 |
| volume.snapshotPolicy.schedule.time | Body | String | - | スナップショット自動作成時間 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | スナップショット自動作成基準タイムゾーン |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | スナップショット自動作成曜日。<br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |

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

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="volume.connect_interface"></a>
### ボリュームにインターフェース接続

指定したボリュームのインターフェースを設定します。
設定されたアドレス及びサブネットからボリュームにアクセス可能です。アクセス可能なIP設定はアクセス制御(ACL)設定で別途設定する必要があります。

```
POST  /v1/volumes/{volume_id}/interfaces
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| interface | Body | Object | O | インターフェース設定オブジェクト |
| interface.subnetId | Body | String | O | インターフェースサブネット指定 |

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| interface | Body | Object | 作成されたインターフェース情報オブジェクト |
| interface.id | Body | String | 作成されたインターフェースID |
| interface.path | Body | String | 作成されたインターフェースパス |
| interface.status | Body | String | 作成されたインターフェースの状態 |
| interface.subnetId | Body | String | 作成されたインターフェースのサブネットID |
| interface.tenantId | Body | String | 作成されたインターフェースのテナントID |

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
### ボリュームのインターフェース削除

指定したボリュームの指定したインターフェースを削除します。

```
DELETE  /v1/volumes/{volume_id}/interfaces/{interface_id}
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| interface\_id | URL | String | O | 削除するインターフェースID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="volume.view_snapshot_restore_history"></a>
### スナップショット復元履歴を表示

指定したボリュームのスナップショット復元履歴リストを返します。

```
GET  /v1/volumes/{volume_id}/restore-histories
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| limit | String | Query | X | 1ページに表示するリソース数 |
| page | String | Query | X | 照会するページ |
| sort | String | Query | X | ソート基準となるフィールド名<br>`{key}:{direction}`の形で記述します。例：`snapshotId:asc`, `requestedAt:desc`<br>使用可能なkey値: `snapshotId`, `snapshotName`, `requestedAt`, `restoredAt`, `requestedUser`, `requestedIp`, `result` |

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
### ボリューム使用状況表示

指定したボリュームの使用状況を返します。

```
GET  /v1/volumes/{volume_id}/usage
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| usage | Body | Object | ボリューム使用状況オブジェクト |
| usage.snapshotReserveGb | Body | Integer | ボリュームでスナップショットのために予約したスペースサイズ |
| usage.usedGb | Body | Integer | ボリューム使用量 |

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
    "snapshotReserveGb": 30,
    "usedGb": 2
  }
}
```

</details>

<br>

<a id="snapshots"></a>
## Snapshots

<a id="snapshots.list"></a>
### スナップショットリスト表示

スナップショットリストを照会します。

```
GET  /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| snapshots | Body | List | スナップショット情報オブジェクトリスト |
| snapshots.id | Body | String | スナップショットID |
| snapshots.name | Body | String | スナップショット名 |
| snapshots.size | Body | Integer | スナップショットサイズ |
| snapshots.type | Body | String | スナップショットタイプ<br>- `NORMAL`:ユーザーによって作成されたスナップショット<br>- `SCHEDULED`:スナップショット自動作成によって作成されたスナップショット<br>- `MIRROR`:複製により作成されたスナップショット |
| snapshots.preserved | Body | Boolean | システムによって削除不可に設定されたスナップショットかどうか |
| snapshots.createdAt | Body | String | スナップショット作成時刻 |

<details><summary>レスポンス例</summary>

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 201,
    "resultMessage": "Created"
  },
  "snapshots": [
    {
      "createdAt": "2025-04-01T09:34:27+00:00",
      "id": "8151fe33-0edc-11f0-b0e3-d039eaa3e920",
      "name": "TEST-SNAPSHOT-1",
      "preserved": false,
      "size": 3112960,
      "type": "NORMAL"
    }
  ]
}
```
</details>

<br>

<a id="snapshots.create"></a>
### スナップショット作成

指定したボリュームのスナップショットを作成します。

```
POST  /v1/volumes/{volume_id}/snapshots
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| snapshot | Body | Object | O | スナップショット作成オブジェクト |
| snapshot.name | Body | String | O | スナップショット名 |

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| snapshot | Body | Object | スナップショット情報オブジェクト |
| snapshot.id | Body | String | スナップショットID |
| snapshot.name | Body | String | スナップショット名 |
| snapshot.size | Body | Integer | スナップショットサイズ |
| snapshot.type | Body | String | スナップショットタイプ<br>- `NORMAL`:ユーザーによって作成されたスナップショット<br>- `SCHEDULED`:スナップショット自動作成によって作成されたスナップショット<br>- `MIRROR`:複製により作成されたスナップショット |
| snapshot.preserved | Body | Boolean | システムによって削除不可に設定されたスナップショットかどうか |
| snapshot.createdAt | Body | String | スナップショット作成時刻 |

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
    "createdAt": "2025-04-01T09:34:27+00:00",
    "id": "8151fe33-0edc-11f0-b0e3-d039eaa3e920",
    "name": "TEST-SNAPSHOT-1",
    "preserved": false,
    "size": 3112960,
    "type": "NORMAL"
  }
}
```

</details>

<br>

<a id="snapshots.delete"></a>
### スナップショット削除

指定したボリュームのスナップショットを削除します。

```
DELETE  /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| snapshot\_id | URL | String | O | スナップショットID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="snapshots.view"></a>
### スナップショット表示

指定したスナップショットの詳細情報を返します。

```
GET  /v1/volumes/{volume_id}/snapshots/{snapshot_id}
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| snapshot\_id | URL | String | O | スナップショットID |
| showReclaimableSpace | Query | Boolean | - | スナップショット削除時に確保される容量を示す`reclaimableSpace`項目を表示するかどうか |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| snapshot | Body | List | スナップショット情報オブジェクト |
| snapshot.id | Body | String | スナップショットID |
| snapshot.name | Body | String | スナップショット名 |
| snapshot.size | Body | Integer | スナップショットサイズ |
| snapshot.type | Body | String | スナップショットタイプ<br>- `NORMAL`:ユーザーによって作成されたスナップショット<br>- `SCHEDULED`:スナップショット自動作成によって作成されたスナップショット<br>- `MIRROR`:複製により作成されたスナップショット |
| snapshot.preserved | Body | Boolean | システムによって削除不可に設定されたスナップショットかどうか |
| snapshot.createdAt | Body | String | スナップショット作成時刻 |

<br>

<a id="snapshots.restore"></a>
### スナップショット復元

指定したスナップショットでボリュームを復元します。

```
POST  /v1/volumes/{volume_id}/snapshots/{snapshot_id}/restore
X-Auth-Token: {token-id}
```

#### リクエスト

リクエスト本文は必要ありません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| snapshot\_id | URL | String | O | スナップショットID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication"></a>
## ボリューム複製設定

<a id="replication.setup"></a>
### 複製設定

指定したボリュームの複製を設定します。
複製対象プロジェクトごとに選択可能なリージョン範囲は、以下の表で確認できます。

| 対象プロジェクト | 選択可能なリージョン |
| ------- | --------- |
| 組織内の同一プロジェクト | 他のリージョン |
| 組織内他のプロジェクト | 全てのリージョン |

<br>

> [注意]
> 複製対象ボリュームサイズはソースボリュームと同じサイズに設定する必要があります。ソースボリュームと対象ボリュームのサイズが異なる場合、複製に失敗する可能性があります。

<!-- -->

> [参考]
> 複製対象ボリュームに暗号化を設定するには、ソースボリュームとは別の(複製対象ボリュームが属するプロジェクトまたはリージョン)暗号化キーストア設定が必要です。

<!-- -->

> [参考] 
> ソースボリュームがCIFSプロトコルを使用している場合、対象ボリュームもCIFSプロトコルを使用する必要があります。このため、ソースボリュームとは別のCIFS認証情報を作成してリクエスト本文`cifsAuthIds`フィールドに入力する必要があります。


```
POST  /v1/volumes/{volume_id}/volume-mirrors
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | 原本ボリュームID |
| volumeMirror | Body | Object | O | ボリューム複製設定リクエストオブジェクト |
| volumeMirror.dstRegion | Body | String | O | 複製対象ボリュームのリージョン |
| volumeMirror.dstTenantId | Body | String | O | 複製対象ボリュームのテナントID |
| volumeMirror.dstVolume | Body | Object | O | 複製対象ボリューム作成リクエストオブジェクト |
| volumeMirror.dstVolume.acl | Body | List | - | ボリューム作成時に設定するACLリスト<br>IPまたはCIDR形式で入力できます。 |
| volumeMirror.dstVolume.description | Body | String | - | ボリュームの説明 |
| volumeMirror.dstVolume.encryption | Body | Object | - | ボリューム作成時の暗号化設定オブジェクト |
| volumeMirror.dstVolume.encryption.enabled | Body | Boolean | - | 暗号化設定が有効かどうか<br>暗号化キーストアが設定された後、該当フィールドを`true`に設定すると暗号化が有効になります。 |
| volumeMirror.dstVolume.interfaces | Body | List | - | ボリュームにアクセスするインターフェースリスト |
| volumeMirror.dstVolume.interfaces.subnetId | Body | String | - | ボリュームインターフェースのサブネットID |
| volumeMirror.dstVolume.mountProtocol | Body | Object | - | ボリューム作成時のプロトコル設定オブジェクト |
| volumeMirror.dstVolume.mountProtocol.cifsAuthIds | Body | List | - | CIFS認証IDリスト<br>NFSプロトコル選択時入力不要 |
| volumeMirror.dstVolume.mountProtocol.protocol | Body | String | O | ボリュームをマウントする際のプロトコル指定<br>`nfs`, `cifs`のいずれかを選択できます。 |
| volumeMirror.dstVolume.name | Body | String | O | ボリューム名 |
| volumeMirror.dstVolume.sizeGb | Body | Integer | O | ボリュームサイズ(GB)<br>ボリュームは、最小300GBから最大10,000GBまで、100GB単位で設定できます。 |
| volumeMirror.dstVolume.snapshotPolicy | Body | Object | - | ボリュームスナップショット設定オブジェクト |
| volumeMirror.dstVolume.snapshotPolicy.maxScheduledCount | Body | Integer | - | スナップショット最大保存数<br>30個まで設定可能で、最大保存数に達すると、自動的に作成されたスナップショットのうち、最初に作成されたスナップショットが削除されます。 |
| volumeMirror.dstVolume.snapshotPolicy.reservePercent | Body | Integer | - | スナップショット容量比率 |
| volumeMirror.dstVolume.snapshotPolicy.schedule | Body | Object | - | スナップショット自動作成オブジェクト<br>`null`の場合、スナップショット自動作成が設定されません。 |
| volumeMirror.dstVolume.snapshotPolicy.schedule.time | Body | String | - | スナップショット自動作成時間 |
| volumeMirror.dstVolume.snapshotPolicy.schedule.timeOffset | Body | String | - | スナップショット自動作成基準タイムゾーン |
| volumeMirror.dstVolume.snapshotPolicy.schedule.weekdays | Body | List | - | スナップショット自動作成曜日。<br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object | ヘッダオブジェクト |
| volumeMirror | Body | Object | 複製設定作成オブジェクト |
| volumeMirror.id | Body | String | 複製設定ID |
| volumeMirror.role | Body | String | 複製役割<br>- `SOURCE`:ソースボリューム<br>- `DESTINATION`:対象ボリューム |
| volumeMirror.status | Body | String | 複製設定状態<br>- `INITIALIZED`:設定完了<br>- `UPDATING`:設定変更中<br>- `DELETING`:設定削除中<br>- `PENDING`:設定作成中 |
| volumeMirror.direction | Body | String | 複製方向 <br>- `FORWARD`:ソースボリューム -> 複製ボリューム<br>- `REVERSE`:複製ボリューム -> ソースボリューム |
| volumeMirror.directionChangedAt | Body | String | 複製方向変更時刻 |
| volumeMirror.dstProjectId | Body | String | 複製対象ボリュームのプロジェクトID |
| volumeMirror.dstRegion | Body | String | 複製対象ボリュームリージョン |
| volumeMirror.dstTenantId | Body | String | 複製対象ボリュームテナントID |
| volumeMirror.dstVolumeId | Body | String | 複製対象ボリュームID |
| volumeMirror.dstVolumeName | Body | String | 複製対象ボリューム名 |
| volumeMirror.srcProjectId | Body | String | ソースボリュームのプロジェクトID |
| volumeMirror.srcRegion | Body | String | ソースボリュームリージョン |
| volumeMirror.srcTenantId | Body | String | ソースボリュームテナントID |
| volumeMirror.srcVolumeId | Body | String | ソースボリュームID |
| volumeMirror.srcVolumeName | Body | String | ソースボリューム名 |
| volumeMirror.createdAt | Body | String | 複製作成時刻 |

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
}
```

</details>

<br>

<a id="replication.disable"></a>
### 複製設定の解除

指定したボリュームの複製設定を解除します。

```
DELETE  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| volume\_mirror\_id | URL | String | O | 複製設定ID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication.change_direction"></a>
### 複製方向の変更

ソースボリュームと対象ボリュームの複製方向を変更します。

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/invert-direction
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| volume\_mirror\_id | URL | String | O | 複製設定ID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication.start"></a>
### 複製開始

ソースボリュームから対象ボリュームへの複製を開始します。

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/start
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| volume\_mirror\_id | URL | String | O | 複製設定ID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>

<a id="replication.status"></a>
### 複製状態の確認

最近の複製状態を返します。

```
GET  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stat
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| volume\_mirror\_id | URL | String | O | 複製設定ID |

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
| volumeMirrorStat.status | Body | String | 複製設定状態<br>- `ACTIVE`:複製有効化状態<br>- `UPDATING`:設定変更中<br>- `DELETING`:設定削除中<br>- `PENDING`:設定作成中 <br>- `HALT`:複製停止状態<br>- `RETRIEVE FAILED`:一時的な情報取得失敗 |

<br>

<a id="replication.stop"></a>
### 複製停止

ソースボリュームから対象ボリュームへの複製を停止します。

```
POST  /v1/volumes/{volume_id}/volume-mirrors/{volume_mirror_id}/stop
X-Auth-Token: {token-id}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| X-Auth-Token | Header | String | O | トークンID |
| volume\_id | URL | String | O | ボリュームID |
| volume\_mirror\_id | URL | String | O | 複製設定ID |

#### レスポンス

レスポンス本文にはヘッダフィールド以外の内容は含まれません。

<br>
