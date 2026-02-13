# WordPress に Unity 製ゲームを埋め込む検証

## 1. 環境構築

Docker Compose を使って WordPress の実行環境および開発用の FTP 接続環境を構築する。

### 構成

- **wordpress**: WordPress 本体の実行環境
- **db**: WordPress のデータを保存する MySQL データベース
- **ftp**: WordPress サーバー上のファイルにアクセスするための FTP サーバー

### FTP サーバーコンテナが必要な理由

WordPress イメージには FTP サーバー機能が含まれていない。

FTP サーバーコンテナ（`alpine-ftp-server`）を起動し、<br>
WordPress コンテナのボリューム（`wp_data`）をマウントすることで本番環境をシミュレートする。

### コマンド

`compose.yaml` を配置したディレクトリでコマンドを実行する。

#### 起動

```bash
docker compose up -d
```

#### 停止

```bash
# 通常の停止
docker compose down

# ボリュームを削除したい場合
docker compose down -v
```

#### コンテナの状態を確認

```bash
docker compose ps
```

### 参考資料

- [wordpress - Official Image | Docker Hub](https://hub.docker.com/_/wordpress)
- [delfer/docker-alpine-ftp-server | GitHub](https://github.com/delfer/docker-alpine-ftp-server)

## 2. 検証手順

### ① Unity Editor でゲームを WebGL ビルドする

> Unity には、サーバーの設定方法に影響する主な設定が 2 つあります。
>
> - **Compression Format**: ビルドステップで Unity がどのようにファイルを圧縮するかを決定します。
> - **Decompression Fallback**: ビルドがブラウザーで実行されるときに、ダウンロードしたファイルを Unity がどのように処理するかを決定します。
>
> (参照: [WebGL アプリケーションのデプロイ | Unity Documentation](https://docs.unity3d.com/ja/2022.3/Manual/webgl-deploying.html) )

検証段階では以下の設定でよい。

- Compression Format: Disabled
- Decompression Fallback: OFF

本番環境でパフォーマンスを考慮する場合、以下の設定にするとよい（と思う）。

- Compression Format: gzip
- Decompression Fallback: ON

### ② サーバーにビルドファイルを FTP でアップロードする

Filezilla Client で「ファイル > サイトマネージャー」から次のように設定する。

- 一般タブ
  - ホスト: localhost
  - ポート: 21
  - プロトコル: FTP
  - 暗号化: 使用可能なら FTP over TLS を使用
  - ユーザー: myuser
  - パスワード: mypass
- 転送設定タブ
  - 転送モード: パッシブ

### ③ ブログのページにアップロードしたビルドファイルを埋め込む

カスタム HTML ブロックを使って、ビルドファイルの `index.html` を iframe 要素に埋め込む。スタイルの調整は必要。

```html
<iframe
  src="/games/watermelon_webgl/index.html"
  allowfullscreen>
</iframe>
```
