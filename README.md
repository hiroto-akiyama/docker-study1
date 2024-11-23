# docker-study1

## Node.js をベースにした React.js 開発環境

このサンプルは React.js(Next.js) を起動させるためのサンプルです。  
データベース等を持たないコーポレートサイトのような静的サイトをイメージしたサンプルです。

### Dockerfile

<pre>
# Node.jsの公式イメージをDockerHubからダウンロード
FROM node:22-bullseye

# インストールされているソフトウェアを更新し、追加でソフトウェアをインストール
RUN apt update && apt install -y less man-db sudo

# コンテナを起動させるためのユーザー「node」を追加
ARG USERNAME=node
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
</pre>

### docker-compose.yml

<pre>
version: '3.8'

services:
  app: # コンテナごとにdocker-compose内名前を付ける
    build: . # Dockerfileのパスを指定
    container_name: next-app # コンテナの名前を付ける
    # ttyとstdin_openはコンテナを起動させておくための設定
    tty: true 
    stdin_open: true
    # WSLとコンテナのフォルダの共有設定
    volumes:
      - ../:/workspace:cached
    # このコンテナにアクセスするポート番号のマッピング
    ports:
      - '3000:3000'
</pre>

### devcontainer.js

<pre>
{
  "name": "Node.js project",
  "dockerComposeFile": "./docker-compose.yml",
  "service": "app",
  "workspaceFolder": "/workspace",
  "customizations": {
    "vscode": {
      "extensions": []
    }
  },
  "remoteUser": "node"
}
</pre>

### Setup Next.js

表示 → コマンドパレット → 開発コンテナー:コンテナーで再度開く

<pre>
npx create-next-app
✔ What is your project named? … next-app
✔ Would you like to use TypeScript? … No / Yes
✔ Would you like to use ESLint? … No / Yes
✔ Would you like to use Tailwind CSS? … No / Yes
✔ Would you like your code inside a `src/` directory? … No / Yes
✔ Would you like to use App Router? (recommended) … No / Yes
✔ Would you like to use Turbopack for next dev? … No / Yes
✔ Would you like to customize the import alias (@/* by default)? … No / Yes
✔ What import alias would you like configured? … @/*
</pre>

Next.js のインストールが完了したら next-app フォルダに移動して、yarn dev で開発サーバーを起動。
※yarn 以外にも npm や pnpm などがありますが各自のお好みで

```
cd next-app
yarn dev
```

http://localhost:3000

にアクセスすると Next.js のデフォルトページが表示されます。

### 既に Next.js がインストールされているプロジェクトを Clone した場合

git clone でローカルにソースコードをダウンロードしたらアプリケーションのフォルダを開き  
yarn install で必要なパッケージをインストールする

```
cd next-app
yarn install
yarn dev
```
