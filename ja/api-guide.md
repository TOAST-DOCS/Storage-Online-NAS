```markdown
## Storage > NAS > APIガイド

<a id="nas_api_common"></a>
## NAS API共通情報

<a id="nas_api_common.endpoint"></a>
### APIエンドポイント

NAS APIは`nasv1`タイプエンドポイントを使用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| リージョン | エンドポイント |
| --- | --- |
| 韓国(パンギョ)リージョン | https://kr1-api-nas-infrastructure.nhncloudservice.com |
| 韓国(ピョンチョン)リージョン | https://kr2-api-nas-infrastructure.nhncloudservice.com |
| 韓国(光州)リージョン | https://kr3-api-nas-infrastructure.nhncloudservice.com |


<a id="nas_api_common.authentication"></a>
### 認証及び権限

NASは、API呼び出し時の認証/認可のためにIaaSトークンを使用します。IaaSトークンは、NHN CloudのOpenStackベースのインフラサービス(IaaS)で使用する認証トークンです。
IaaSトークンの発行及び使用に関する詳細は、[IaaSトークン](/nhncloud/ja/public-api/iaas-token/)を参照してください。

<a id="nas_api_common.response"></a>
### レスポンス共通情報

NAS APIが提供する共通レスポンス情報の説明です。全てのAPIレスポンスは`header`オブジェクトでリクエスト結果を伝達します。

| 名前 | 形式 | 説明 |
| --- | --- | --- |
| header | Object | ヘッダオブジェクト |
| header.isSuccessful | Boolean | リクエストの成否(`true`または`false`) |
| header.resultCode | Integer | HTTPステータスコードに該当する結果コード<br>- `200`:成功 <br>- `201`:リソース作成成功<br>- `202`:リクエストが正常に受信されたが、まだ処理されていない状態<br>- `400`:有効ではない値でリクエストされた<br>- `401`:権限、認証またはトークン関連エラー <br>- `404`:リクエストしたリソースが見つからない<br>- `405`:リクエストしたURLが指定したHTTPメソッドをサポートしていない<br>- `5XX`:クライアントのリクエストは有効ですがサーバーが処理に失敗する |
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

> [参考]
> APIレスポンスにガイドに記載されていないフィールドが表示される場合があります。これらのフィールドは、NHN Cloudの内部用途に使用され、事前告知なしに変更される可能性があるため、使用しないでください。

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
| sizeGb | Query | String | - | ボリュームサイズ |
| maxSizeGb | Query | String | - | ボリューム最大サイズ |
| minSizeGb | Query | String | - | ボリューム最小サイズ |
| name | Query | String | - | ボリューム名 |
| nameContains | Query | String | - | ボリューム名に含まれる文字列 |
| subnetId | Query | String | - | サブネットのインターフェースを持つボリューム |
| limit | Query | String | - | 1ページに表示するリソース数 |
| page | Query | String | - | 照会するページ |
| sort | Query | String | - | ソート基準となるフィールド名<br>`{key}:{direction}`の形で記述します。例：`name:asc`, `created_at:desc`<br>使用可能なkey値: `id`, `name`, `sizeGb`, `createdAt`, `updatedAt` |

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
| volumes.mirrors.direction | Body | String | 複製方向<br>- `FORWARD`:ソースボリューム → 複製ボリューム <br>- `REVERSE`:複製ボリューム → ソースボリューム |
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
> CIFSプロトコルを使用するには、CIFS認証情報を生成する必要があります。認証情報はプロジェクト単位で管理され、CIFSボリュームごとにアクセスを許可するCIFS認証情報を登録する必要があります。
> CIFS認証情報はコンソールの **Storage > NAS > CIFS認証情報管理**ウィンドウから作成できます。


<!-- -->

> [参考]暗号化キーストア設定
> 暗号化ボリュームを作成すると、暗号化に使用する共通鍵がNHN Cloud Secure Key Managerサービスのキーストアに保存されます。したがって、暗号化ボリュームを作成するには、事前にSecure Key Managerサービスで[キーストアを作成](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ko/getting-started/#_1)する必要があります。[キーストアのIDを確認](https://docs.nhncloud.com/ja/Security/Secure%20Key%20Manager/ko/getting-started/#_2)し、暗号化キーストア設定に入力します。
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
| volume.snapshotPolicy.maxScheduledCount | Body | Integer | - | スナップショット最大保存数<br>30個まで設定可能で、最大保存数に達すると、作成されたスナップショット中のうち、最初に作成されたスナップショットが自動的に削除されます。 |
| volume.snapshotPolicy.reservePercent | Body | Integer | - | スナップショット容量比率 |
| volume.snapshotPolicy.schedule | Body | Object | - | スナップショット自動作成オブジェクト<br>`null`の場合、スナップショット自動作成が設定されません。 |
| volume.snapshotPolicy.schedule.time | Body | String | - | スナップショット自動作成時間 |
| volume.snapshotPolicy.schedule.timeOffset | Body | String | - | スナップショット自動作成基準タイムゾーン |
| volume.snapshotPolicy.schedule.weekdays | Body | List | - | スナップショット自動作成曜日<br>空白のリストは毎日を意味し、曜日を0(日曜日)から6(土曜日)までの数字のリストで指定します。 |

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
| volume.mirrors.direction | Body | String | 複製方向<br>- `FORWARD`:ソースボリューム -> 複製ボリューム<br>- `REVERSE`:複製ボリューム -> ソースボリューム |
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

リクエス