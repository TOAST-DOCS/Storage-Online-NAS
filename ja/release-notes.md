<!-- pre-align:aligned sig=41a352a073ca -->

<a id="storage-nas-release-notes"></a>
## Storage > NAS > リリースノート { #storage-nas-release-notes }

<a id="august-25-2026"></a>
## 2026. 08. 25. { #august-25-2026 }

<a id="august-25-2026-added-features"></a>
### 新規機能追加 { #august-25-2026-added-features }

* NAS リソースのアクセス権限の細分化
    * Infrastructure NAS ADMIN ロールが追加され、NAS リソースの作成、変更、削除の権限が提供されます。
    * Infrastructure ADMIN を除く既存の基本インフラロールでは、NAS リソースの作成、変更、削除が制限されます。

<a id="august-25-2026-feature-updates"></a>
### 機能改善/変更 { #august-25-2026-feature-updates }

* ボリュームステータス表示の改善
    * ボリュームのステータスが失敗またはエラーの場合、その原因を確認できるよう詳細な理由が併せて提供されます。

<a id="may-27-2026"></a>
## 2026. 05. 27. { #may-27-2026 }

<a id="may-27-2026-added-features"></a>
### 新機能追加 { #may-27-2026-added-features }

* ボリューム使用状況へのスナップショット使用量情報の追加
    * 全体スナップショット使用量
    * スナップショット予約容量内の使用量
    * 予約容量超過スナップショット使用量

<a id="november-25-2025"></a>
## 2025. 11. 25. { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
### 機能改善/変更 { #november-25-2025-feature-updates }

* モニタリング機能の変更
    * CIFSプロトコルボリュームのモニタリング機能が追加されました。
    * クライアント接続数のモニタリングが削除されました。

<a id="may-27-2025"></a>
## 2025. 05. 27. { #may-27-2025 }

<a id="may-27-2025-added-features"></a>
### 新機能追加 { #may-27-2025-added-features }

* Public API 提供
* ボリューム名検索の追加

<a id="may-27-2025-feature-updates"></a>
### 機能改善/変更 { #may-27-2025-feature-updates }

* CIFS認証情報 ID の制約条件変更
* ボリューム複製機能の変更
    * 複製方向変更後、直ちに複製が有効化されます。
    * 複製を停止するか、複製方向を変更する場合、リクエスト時点までのデータが複製された後に処理が進みます。

<a id="march-4-2025"></a>
## 2025. 03. 04. { #march-4-2025 }

<a id="march-4-2025-added-features"></a>
### 新機能追加 { #march-4-2025-added-features }

* ボリューム複製機能の改善
    * CIFS プロトコルボリュームを複製できます。

<a id="august-27-2024"></a>
## 2024. 08. 27. { #august-27-2024 }

<a id="august-27-2024-added-features"></a>
### 新機能追加 { #august-27-2024-added-features }

* 作成されたボリュームのサブネット接続変更機能を追加
* ボリューム複製機能の改善
    * 組織内の別のプロジェクトにボリュームを複製できます。
    * 暗号化ボリュームを複製できます。
* スナップショット一覧に作成時点の使用量を表示

<a id="may-28-2024"></a>
## 2024. 05. 28. { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
### 新機能追加 { #may-28-2024-added-features }

* CIFS プロトコルの追加
    * Windows 環境でもボリュームを使用できます。
* スナップショット復元機能の追加
* スナップショット復元履歴照会の追加
* リージョン間ボリューム複製機能の追加

<a id="march-26-2024"></a>
## 2024. 03. 26. { #march-26-2024 }

<a id="march-26-2024-added-features"></a>
### 新機能追加 { #march-26-2024-added-features }

* 暗号化ボリューム作成機能を追加しました。

<a id="march-28-2023"></a>
## 2023. 03. 28. { #march-28-2023 }

<a id="march-28-2023-added-features"></a>
### 新機能追加 { #march-28-2023-added-features }

* NAS モニタリング機能を追加
    * ボリュームに関するさまざまな指標をグラフとともに確認できます。

<a id="november-23-2022"></a>
## 2022. 11. 23. { #november-23-2022 }

<a id="november-23-2022-added-new-region"></a>
### 新しいリージョンの追加 { #november-23-2022-added-new-region }

* 韓国(坪村)リージョンにNASサービスをリリース

<a id="july-26-2022"></a>
## 2022. 07. 26. { #july-26-2022 }

<a id="july-26-2022-new-service-release"></a>
### 新規サービスリリース { #july-26-2022-new-service-release }

* 韓国(板橋)リージョンにて NAS サービスをリリース
* プロジェクトの VPC ネットワークを通じたインスタンスボリューム接続をサポート
