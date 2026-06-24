# Laravel Devcontainer

Laravelの利用を前提としたDevcontainer環境スケルトンのリポジトリです。
VS Code の Dev Containers 拡張機能で開くと、PHP / Web サーバー / DB がセットアップされた状態でLaravel開発を始められます。

## 特徴

- PHP 8.5-fpm + Composer + Node.js/npm を含む開発コンテナ
- Nginx によるWebサーバー（Laravelのpublic配下を直接配信）
- MariaDB 10（UTF8MB4 + 日本語ロケール設定済み）
- VS Code拡張機能（Intelephense, Laravel Extra Intellisense, EditorConfig）を自動インストール
- PHP設定は日本語環境（タイムゾーン: `Asia/Tokyo`、`mbstring.language = Japanese`）向けに調整済み

## 構成

| サービス | イメージ / ベース | 役割 | ポート |
| --- | --- | --- | --- |
| `app` | `php:8.5-fpm` | PHP実行環境（Composer, Node.js/npm同梱） | - |
| `nginx` | `nginx:alpine` | Webサーバー（`app`へFastCGIでリバースプロキシ） | `8000:80` |
| `db` | `mariadb:10` | データベース | - |

`devcontainer.json` では `5173`（Vite想定）もポートフォワード対象に設定されています。

## ディレクトリ構成

```
.devcontainer/
├── devcontainer.json   # Dev Containers設定（VS Code拡張、ポート転送など）
├── docker-compose.yml  # app/nginx/dbの3コンテナ定義
├── Dockerfile          # appコンテナ（PHP-FPM）のビルド定義
├── mariadb/
│   └── my.cnf           # MariaDBの文字コード・ロケール設定
├── nginx/
│   └── default.conf     # NginxのLaravel用ルーティング設定
└── php/
    └── php.ini           # PHPの開発向け設定
```

## 前提条件

- Docker（Docker Desktop など）
- VS Code + [Dev Containers拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## 使い方

1. VS Codeでこのリポジトリを開き、コマンドパレットから **「Reopen in Container」** を実行する。
2. Laravel本体がまだ存在しない場合は、コンテナ内のターミナルで作成する。
   ```bash
   composer create-project laravel/laravel .
   ```
3. `.env` のDB接続設定を以下のように指定する。
   ```env
   DB_CONNECTION=mysql
   DB_HOST=db
   DB_PORT=3306
   DB_DATABASE=mariadb
   DB_USERNAME=mariadb
   DB_PASSWORD=mariadb
   ```
4. フロントエンドのビルドが必要な場合はインストール・起動する。
   ```bash
   npm install
   npm run dev
   ```
5. ブラウザで [http://localhost:8000](http://localhost:8000) にアクセスして動作を確認する。

## 注意点

- `docker-compose.yml` のDB名・ユーザー名・パスワード（いずれも`mariadb`）はサンプル値です。スケルトンとして別プロジェクトに使い回す際は、本番相当の用途に使う前に適宜変更してください。
