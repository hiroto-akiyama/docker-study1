# Node.jsの公式イメージをDockerHubからダウンロード
FROM node:22-bullseye

# インストールされているソフトウェアを更新し、追加でソフトウェアをインストール
RUN apt update && apt install -y less man-db sudo

# コンテナを起動させるためのユーザー「node」を追加
ARG USERNAME=node
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
