# Docker基礎

## Dockerとは

- Dockerは「データやプログラムを隔離できる仕組み」
- Docker Engineのうえにコンテナが載る
- コンテナはイメージから作る



## コマンド一般規則

```bash
docker container run -d          penguin --mode=1
docker [コマンド     ] [オプション] [対象]   [引数]
```



## Dockerのコンテナライフサイクル

1. `create` コンテナ作成
2. `start` 起動
   ( `create`, ``)
3. `stop` 停止
4. `rm` 廃棄

* Apache （コンテナのライフサイクル）

```bash
docker run --name xxx(コンテナ名) -d httpd
// -d バックグラウンド実行
docker ps -a
docker stop xxx
docker rm xxx
```

* Apache（通信）

```bash
docker run --name xxx -d -p 8080:80 httpd
// -p [ホスト]:[コンテナ] ポートの設定
docker ps -a
docker stop xxx
docker rm xxx
```

* MySQL

```bash
docker run --name xxx -dit -e MYSQL_ROOT_PASSWORD=myrootpass mysql
// -d バックグラウンド実行 -it キーボード操作
// -e 環境変数
docker stop xxx
docker rm xxx
```

* イメージの削除

```bash
docker image ls
docker image rm xxx(イメージ名)
docker image ls
```

* Dockerネットワーク（コンテナ同士の接続）

```bash
docker network create ネットワーク名 // 作成
docker network ls ネットワーク名 // 一覧表示
docker network rm ネットワーク名 // 削除

docker run name --xxx -dit --net=ネットワーク名 ...
```

* ファイルの移動

```bash
docker cp [コンテナ名]:[コンテナ側のパス] [ホスト側のパス] // コンテナ→ホスト
docker cp [ホスト側のパス] [コンテナ名]:[コンテナ側のパス] // ホスト→コンテナ
```

* ボリューム（ストレージの1領域）のマウント

  * バインドマウント（Docker Engine以外のローカル上にマウント）

  ```bash
  docker run --name xxx -d -p 8080:80 -v [ホスト側のパス]:[コンテナ側のパス] httpd
  ```

  

  * ボリュームマウント（Docker Engine上の領域内にボリュームを作成して使用）

  ```bash
  docker volume create xxx(ボリューム名)
  docker run --name yyy(コンテナ名) -d -p 8089:80 -v [ホスト側パス]:[コンテナ側パス] httpd
  docker volume inspect xxx 	// ボリュームの詳細情報の表示
  docker vokume rm xxx 		// ボリュームの削除
  ```

  



## コンテナのイメージ化

1. commit

   ```bash
   docker commit コンテナ名 作成するイメージ名
   ```

   

2. Dockerfile
   Dockerfileを`build`することで、イメージを作成する。

   * サンプル

   ```dockerfile
   FROM httpd
   COPY index.html /usr/local/apache2/htdocs/
   ```

   ```bash
   docker build -t 作成するイメージ名 材料フォルダのパス
   ```



## Docker Compose 

Docker Composeはコンテナ、ネットワーク、ボリュームを作る

ファイル名：docker-componse.yml

```yml
version: "3"
services:
  コンテナ名1:
  コンテナ名2:
networks: 
  ネットワーク名1:
volumes: 
  ボリューム1:
  ボリューム2:
```

* 実行例

```bash
docker-compose -f [定義ファイルのパス] up [オプション]   // コンテナや周辺環境を作成
docker-compose -f [定義ファイルのパス] down [オプション] // コンテナとネットワークを削除
docker-compose -f [定義ファイルのパス] stop [オプション] // コンテナを停止するコマンド
```





# Docker + Vue.js 開発環境構築

[公式](https://jp.vuejs.org/v2/cookbook/dockerize-vuejs-app.html)

```dockerfile
FROM node:lts-alpine

# 静的コンテンツを配信するシンプルな http サーバをインストールする
RUN npm install -g http-server

# カレントワーキングディレクトリとして 'app' フォルダを指定する
WORKDIR /app

# `package.json` と `package-lock.json` （あれば）を両方コピーする
COPY package*.json ./

# プロジェクトの依存ライブラリをインストールする
RUN npm install

# カレントワーキングディレクトリ(つまり 'app' フォルダ)にプロジェクトのファイルやフォルダをコピーする
COPY . .

# 本番向けに圧縮しながらアプリケーションをビルドする
RUN npm run build

EXPOSE 8080
CMD [ "http-server", "dist" ]
```



