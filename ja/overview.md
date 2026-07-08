<a id="storage-nas-overview"></a>
## Storage > NAS > 概要 { #storage-nas-overview }

NASサービスを使用すると、インスタンスに共有ストレージを接続してデータを簡単に共有できます。

<a id="features"></a>
## 特徴 { #features }

<a id="features.sharing"></a>
### 共有 { #features.sharing }

ボリュームを1つ以上のインスタンスにマウントして使用できます。
サポートするプロトコル：NFS v3(Linux), CIFS(Windows)

<a id="features.convenient"></a>
### 利便性 { #features.convenient }

ファイルレベルのボリュームをマウントするため、別途のファイルシステム構成が必要ありません。

<a id="features.flexible"></a>
### 柔軟性 { #features.flexible }

ボリュームを使用中でもボリューム容量の拡張および縮小が可能です。

<a id="features.secure"></a>
###セキュリティ

プロジェクトのネットワークを介してボリュームにアクセスするため、他のプロジェクトのネットワークと隔離されています。
XTS-AES-256アルゴリズムで暗号化してデータを安全に保管できます。

<a id="features.secure"></a>
### セキュリティ { #features.secure }

<!-- TODO: translate body -->

<a id="glossary"></a>
## 用語 { #glossary }

<a id="glossary.NAS"></a>
### NAS(network-attached storage) { #glossary.NAS }

コンピュータネットワークに接続されているファイルレベルの記憶装置を意味し、他のクライアントからのデータアクセスを制御できます。

<a id="glossary.volume"></a>
### ボリューム { #glossary.volume }

NASでデータを保管する論理的な記憶領域を意味します。
インスタンスはNASのボリュームをマウントしてデータの保存と読み取りができます。

<a id="glossary.snapshots"></a>
### スナップショット { #glossary.snapshots }

ボリュームの読み取り専用コピーで、ボリュームのバックアップを意味します。
スナップショットを使用すると、特定時点のデータに復元できます。
NASサービスはユーザーが1日1回自動作成する時点を指定できます。
ただし、スナップショットの保存はボリュームの記憶領域を消費するため、必要がない場合は使用しないことがあります。
