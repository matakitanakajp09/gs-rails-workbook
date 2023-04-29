# インストール

> このページは、講座準備に関するページです。


## Docker Desktopをインストールする

まずはDocker Desktopをインストールします。以下Docker公式ページからインストールしてください。

[Docker Desktopをインストール](https://www.docker.com/products/docker-desktop/)

## Visual Studio Codeをインストールする

今回Ruby, Railsを記述するためのテキストエディタとして、Visual Studio Codeを使用します。以下ダウンロードページからダウンロードをお願いします。

[Visual Studio Codeをインストール](https://code.visualstudio.com/download)

またVisual Studio Codeの拡張機能である「Visual Studio Code Dev Containers」を使用します。
Dockerに慣れている方、ローカルにRuby, Railsの環境構築ができる方に関しては、Visual Studio Codeでなくても問題ありません。
普段お使いのエディアをご使用ください。

[Visual Studio Code Dev Containersを始める](https://code.visualstudio.com/docs/devcontainers/containers)

## 講義用のリポジトリをクローンする

講義用リポジトリをクローンします。

```bash
git clone 
```

データベース接続用設定ファイルをコピーします。

```bash
cp config/database.sample.yml config/database.yml
```

Railsプロジェクトからデータベースを作成します。

```bash
bin/rails db:create
```

データベースの状況を確認すると、データベースを作成しましょうと言われました。

```bash
bin/rails db:migrate:status
rails aborted!
ActiveRecord::NoDatabaseError: We could not find your database: dev_gs_rails_workbook_db. Which can be found in the database configuration file located at config/database.yml.

To resolve this issue:

- Did you create the database for this app, or delete it? You may need to create your database.
- Has the database name changed? Check your database.yml config has the correct database name.

To create your database, run:

        bin/rails db:create


Caused by:
PG::ConnectionBad: FATAL:  database "dev_gs_rails_workbook_db" does not exist

Tasks: TOP => db:migrate:status
(See full trace by running task with --trace)
```

Railsを起動します。

```bash
sh entrypoint.local.sh
```

[http://localhost:3000/](http://localhost:3000/)にアクセスすると、db:mirateコマンドを叩くように言われたので

```bash
Migrations are pending. To resolve this issue, run:
        bin/rails db:migrate RAILS_ENV=development
```

Railsプロジェクトからデータベースをマイグレーションします。

```bash
bin/rails db:migrate
```