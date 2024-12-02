Dockerを使用してPostgreSQLとMongoDBの環境を構築し、それらをGoogle Cloud Platform（GCP）にデプロイする手順を以下にまとめます。

## 1. DockerでのPostgreSQLとMongoDBの設定方法

### 1.1 PostgreSQLの設定

Dockerを利用すると、PostgreSQLの環境を迅速に構築できます。以下は基本的な手順です。

1. **Dockerイメージの取得**:
   ```bash
   docker pull postgres
   ```
   PostgreSQLの公式イメージを取得します。

2. **コンテナの起動**:
   ```bash
   docker run --name my_postgres -e POSTGRES_PASSWORD=your_password -p 5432:5432 -d postgres
   ```
   * `--name`: コンテナの名前を指定します。
   * `-e POSTGRES_PASSWORD`: PostgreSQLのスーパーユーザーのパスワードを設定します。
   * `-p`: ホストとコンテナのポートをマッピングします。
   * `-d`: バックグラウンドでコンテナを実行します。

3. **データの永続化**:
   データを永続化するために、ボリュームをマウントします。
   ```bash
   docker run --name my_postgres -e POSTGRES_PASSWORD=your_password -p 5432:5432 -v /path/to/your/data:/var/lib/postgresql/data -d postgres
   ```
   `/path/to/your/data`をホスト上のデータ保存先に置き換えてください。

詳細な手順や設定については、以下のリソースが参考になります。

* [DockerでPostgreSQLを立てて、DBの操作してみる](https://book.st-hakky.com/hakky/try-postgres-on-docker/)
* [DockerでPostgreSQLを使う方法](https://itc.tokyo/sql/postgresql-docker/)

### 1.2 MongoDBの設定

MongoDBも同様にDockerで簡単にセットアップできます。

1. **Dockerイメージの取得**:
   ```bash
   docker pull mongo
   ```
   MongoDBの公式イメージを取得します。

2. **コンテナの起動**:
   ```bash
   docker run --name my_mongo -p 27017:27017 -d mongo
   ```
   * `--name`: コンテナの名前を指定します。
   * `-p`: ホストとコンテナのポートをマッピングします。
   * `-d`: バックグラウンドでコンテナを実行します。

3. **データの永続化**:
   データを永続化するために、ボリュームをマウントします。
   ```bash
   docker run --name my_mongo -p 27017:27017 -v /path/to/your/data:/data/db -d mongo
   ```
   `/path/to/your/data`をホスト上のデータ保存先に置き換えてください。

これらの手順により、ローカル環境でPostgreSQLとMongoDBのコンテナを立ち上げることができます。

## 2. DockerイメージをGCPのCompute Engineにデプロイする方法

GCPのCompute Engine（GCE）を使用して、Dockerコンテナをデプロイする手順は以下の通りです。

1. **GCEインスタンスの作成**:
   GCPコンソールから新しいCompute Engineインスタンスを作成します。
   * マシンタイプやゾーンなどの設定を行います。

2. **Dockerのインストール**:
   作成したインスタンスにSSHで接続し、Dockerをインストールします。
   ```bash
   sudo apt-get update
   sudo apt-get install docker.io
   ```

3. **Dockerコンテナのデプロイ**:
   Docker Hubから必要なイメージをプルし、コンテナを起動します。
   ```bash
   sudo docker pull your_image_name
   sudo docker run --name your_container_name -p host_port:container_port -d your_image_name
   ```

詳細な手順や設定については、以下のリソースが参考になります。

* [GCP（GCE）に手軽にDockerコンテナをデプロイする](https://qiita.com/aki3061/items/61c703a01ec786b155c8)
* [GCPのCompute EngineにDockerコンテナをデプロイする](https://zenn.dev/kwst/articles/9d95185a0f8875)

これらの手順を踏むことで、GCP上にDockerコンテナをデプロイし、PostgreSQLやMongoDBの環境を構築することが可能です。
