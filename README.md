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
- [delfer/docker-alpine-ftp-server](https://github.com/delfer/docker-alpine-ftp-server)

## 2. 検証手順

WIP
