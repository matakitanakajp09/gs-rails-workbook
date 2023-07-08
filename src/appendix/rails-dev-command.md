# Railsでよく使われるコマンド

## bin/rails generate || bin/rails g

### bin/rails g migration

マイグレーションファイルを作成します。マイグレーションは、データベースの構造を変更するためのファイルです。Railsでは、マイグレーションを使ってデータベースの構造を変更します。

- db/migrate/にマイグレーションファイルを作成します。

[command]

```console
bin/rails g migration CreateTags
```

```console
/workspace# bin/rails g migration CreateTags
      invoke  active_record
      create    db/migrate/20230708053757_create_tags.rb
```

[output]

コマンドを実行すると以下のファイルが作成されます。

Filename: db/migrate/20230708053757_create_tags.rb

```ruby
# frozen_string_literal: true

class CreateTags < ActiveRecord::Migration[7.0]
  def change
    create_table :tags do |t|

      t.timestamps
    end
  end
end
```

テーブルに定義したいカラムを追加します。タグ名を管理するためにnameカラムを追加したい場合は、以下のように記述します。

```ruby
# frozen_string_literal: true

class CreateTags < ActiveRecord::Migration[7.0]
  def change
    create_table :tags, id: :uuid do |t|
      t.string :name, null: false, comment: "タグ名"
      t.timestamps
    end
  end
end
```

マイグレーションしたい場合は、`bin/rails db:migrate`を実行します。

### bin/rails g model

`bin/rails g model`は、モデルを作成するコマンドです。モデルは、データベースのテーブルと対応するクラスです。モデルを作成すると、以下のファイルが作成されます。

- app/models/にモデルファイルを作成します。
- db/migrate/にマイグレーションファイルを作成します。
- test/models/にテストファイルを作成します。
- test/fixtures/にテストデータを作成します。

[command]

```console
bin/rails g model Tag
```

モデル作成時に、カラムを指定することもできます。

[command]

```console
bin/rails g model Tag name:string
```

### bin/rails g controller

`bin/rails g controller`は、コントローラを作成するコマンドです。コントローラは、アクションと呼ばれるメソッドを定義するクラスです。コントローラを作成すると、以下のファイルが作成されます。

- app/controllers/にコントローラファイルを作成します。
- app/views/にビューファイルを作成します。
- test/controllers/にテストファイルを作成します。
- app/helpers/にヘルパーファイルを作成します。
- test/fixtures/にテストデータを作成します。
- config/routes.rbにルーティングを追加します。
- app/assets/stylesheets/にCSSファイルを作成します。
- app/assets/javascripts/にJavaScriptファイルを作成します。

[command]

```console
bin/rails g controller Tags
```

名前空間を指定することもできます。

```console
bin/rails g controller Admin::Tags
```

## bin/rails server || bin/rails s

Railsの開発用サーバー(Puma)を起動します。Railsの開発中によく使うコマンドで、ブラウザからアクセスすることができます。

[command]

```console
bin/rails server
```

短縮形である`bin/rails s`でも起動できます。

```console
bin/rails s
```

### ポート番号を指定する

デフォルトでは、Railsの開発用サーバーは3000番ポートで起動します。ポート番号を変更する場合は、`-p`オプションを使います。

[command]

```console
bin/rails server -p 3001
```

### ホスト名を指定する

デフォルトでは、Railsの開発用サーバーは`localhost`のみからのアクセスを受け付けます。ホスト名を変更する場合は、`-b`オプションを使います。

[command]

```console
bin/rails s -b 0.0.0.0
```

## bin/rails console || bin/rails c

Railsのコンソールを起動します。開発中によく使うコマンドで、Railsのコードを実行したり、データベースの操作を行ったりすることができます。

[command]

```console
bin/rails console
```

短縮形である`bin/rails c`でも起動できます。

```console
bin/rails c
```

### 環境ごとのコンソールを起動する

webアプリケーションの開発では、開発環境と本番環境で動作が異なることがあります。Railsでは、開発環境と本番環境で動作を変えることができます。開発環境のコンソールを起動するには、`-e`オプションを使います。

```console
# 環境を指定してコンソールを起動する
bin/rails c -e production
bin/rails c -e staging
```

環境を指定しない場合は、開発環境のコンソールが起動します。

```console
# 環境を指定しない場合は、開発環境のコンソールが起動する
bin/rails c

# 以下のコマンドと同じ
bin/rails c -e development
```

## bin/rails routes

Railsのルーティングを確認することができます。

以下のコマンドでは、定義されている全てのルーティングを確認することができます。

[command]

```console
bin/rails routes
```

### コントローラーごとにルーティングを確認する

`--controller` or　`-c`オプションをつけると、コントローラーごとにルーティングを確認することができます。

[command]

```console
bin/rails routes --controller tags
bin/rails routes -c tags
```

### ルーティングの一部を確認する

`--grep` or　`-g`オプションをつけると、指定した文字列を含むルーティングを確認することができます。

[command]

```console
bin/rails routes --grep tag
bin/rails routes -g tag
```

`grep`コマンドを使っても同じことができます。

```console
# bin/rails routes -g tagとほとんど同じ
bin/rails routes | grep tag
```

## bin/rails db:migrate:status

データベースのマイグレーションの状態を確認することができます。

RAILS_ENVを指定しない場合は、開発環境のマイグレーションの状態が確認できます。

[command]

```console
bin/rails db:migrate:status

# 上記のコマンドの結果は以下のコマンドと同じ
bin/rails db:migrate:status RAILS_ENV=development
```

[output]

```console
database: gs_expansion_rails_sample_db

 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230521111713  Create active storage tablesactive storage
   up     20230521112655  Create users
   up     20230521113755  Create user registrations
   up     20230521113756  Create user database authentications
   up     20230521113757  Create user account lockings
   up     20230521113758  Create user account trackings
   up     20230521113759  Create user password reset requests
   up     20230522112655  Create admins
   up     20230522113755  Create admin registrations
   up     20230522113756  Create admin database authentications
   up     20230522113757  Create admin account lockings
   up     20230522113758  Create admin account trackings
   up     20230522113759  Create admin password reset requests
   up     20230529075027  Create authors
   up     20230529085435  Create current status enum
   up     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```

### 環境ごとのマイグレーションの状態を確認する

特定の環境に対してマイグレーションのステータスを確認したい場合は、環境変数RAILS_ENVを使用して環境を設定することができます。

```console
RAILS_ENV=production bin/rails db:migrate:status
bin/rails db:migrate:status RAILS_ENV=production
```

※ 前後ろどちらで実行しても実行結果は同じです。

## bin/rails db:migrate

データベースのマイグレーションを実行します。RAILS_ENVを指定しない場合は、開発環境のマイグレーションが実行されます。

開発環境以外で誤ってマイグレーションを実行してしまっても、その環境のマイグレーションに影響はありません(RAILS_ENVが分けられている前提)。

[command]

```console
bin/rails db:migrate

# 上記のコマンドの結果は以下のコマンドと同じ
bin/rails db:migrate RAILS_ENV=development
```

### 環境ごとにマイグレーションを実行する

特定の環境に対してマイグレーションを実行したい場合は、環境変数RAILS_ENVを使用して環境を設定することができます。

```console
RAILS_ENV=production bin/rails db:migrate
bin/rails db:migrate RAILS_ENV=production
```

※ 前後ろどちらで実行しても実行結果は同じです。

## bin/rails db:rollback

データベースのマイグレーションをロールバックします。RAILS_ENVを指定しない場合は、開発環境のマイグレーションがロールバックされます。

ロールバックは、一つ前のマイグレーションをロールバックすることができます。

```console
bin/rails db:rollback

# 上記のコマンドの結果は以下のコマンドと同じ
bin/rails db:rollback RAILS_ENV=development
```

#### 例1

例えば、以下のようなマイグレーション([status])があった場合、`bin/rails db:rollback`を実行すると、`20230529113755  Create action text tablesaction text`がロールバックされます。

[status]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```

[command]

```console
bin/rails db:rollback
```

[output]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230529085530  Create categories
   down     20230529113755  Create action text tablesaction text
```

#### 例2

また以下のようなマイグレーション([status])があった場合、`bin/rails db:rollback`を実行すると、`20230529085530  Create categories`がロールバックされます。

[status]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230529085530  Create categories
   down     20230529113755  Create action text tablesaction text
```

[command]

```console
bin/rails db:rollback
```

[output]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   down     20230529085530  Create categories
   down     20230529113755  Create action text tablesaction text
```

### まとめてロールバックする

`bin/rails db:rollback STEP=2`のようにSTEPオプションを指定することで、複数のマイグレーションをまとめてロールバックすることができます。

[status]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```

[command]

```console
bin/rails db:rollback STEP=2
```

[output]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   down     20230529085530  Create categories
   down     20230529113755  Create action text tablesaction text
```

### 特定のマイグレーションのみロールバックする

`bin/rails db:migrate:down VERSION=20230529085530`のようにVERSIONオプションを指定することで、特定のマイグレーションのみロールバックすることができます。

[status]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```

[command]

```console
bin/rails db:migrate:down VERSION=20230529085530
```

[output]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   down     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```

## bin/rails db:migrate:up

データベースのマイグレーションを実行します。RAILS_ENVを指定しない場合は、開発環境のマイグレーションが実行されます。

ロールバックしたマイグレーションを再度実行することができます。以下のように、`bin/rails db:migrate:up VERSION=20230529085530`のようにVERSIONオプションを指定することで、特定のマイグレーションのみ実行することができます。

[status]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   down     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```

[command]

```console
bin/rails db:migrate:up VERSION=20230529085530
```

[output]

```console
 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20230529085530  Create categories
   up     20230529113755  Create action text tablesaction text
```
