# ActiveRecordについて

ActiveRecordとは、RailsのORM(Object Relational Mapping)のことです。データベースのテーブルと、Rubyのオブジェクトをマッピングすることで、データベースの操作をRubyのコードで行うことができます。

例えば、`show`アクションのコードを見てみましょう。

```ruby
def show
  @author = Author.find(params[:id])

  # `http://localhost:3000/admin/authors/1`というURLにアクセスした場合、
  # `params[:id]`は`1`という値になります。
  # そのため、`Author.find(params[:id])`というコードは、
  # `authors`テーブルから`id`が`1`のレコードを取得するという意味になります。
  # つまり、Article.find("1")となります。
end
```

- `Author.find(params[:id])`というコードがありますが、これは、`authors`テーブルから`id`が`params[:id]`のレコードを取得するという意味です。

- `find`メソッドは、`ActiveRecord::Base`クラスのメソッドです。`Author`モデルは、`ActiveRecord::Base`クラスを継承しているので、`find`メソッドを使用することができます。`find`メソッドは、指定された`id`のレコードを取得することができます。

- params[:id]は、URLのパスに含まれる`:id`という名前のパラメータの値を取得することができます。例えば、`http://localhost:3000/admin/authors/1`というURLの場合、`:id`の値は`1`になります。

## save, updateメソッド

saveメソッドは、レコードを保存するメソッドです。`create`メソッドや`update`メソッドを実行すると、内部で`save`メソッドが実行されます。

```ruby
def create
  @author = Author.new(create_params)
  # 以下の部分
  if @author.save
    flash.now.notice = t("admin.create.success")
    redirect_to admin_authors_path
  else
    flash.now.alert = t("admin.create.failed")
    render :new, status: :unprocessable_entity
  end
end
```

レコード保存関連のメソッドまとめ

戻り値が重要です。true/falseで判定したい場合は、saveメソッドを使用する。失敗した時に例外を投げたい場合は、ビックリマークが使用されているsave!やupdate!メソッドを使用します。

| メソッド名 | 使用例 | 戻り値 | 備考 |
|---|---|---|---|
| save | `user.save` | 成功時は`true`、失敗時は`false` | レコードを保存します。バリデーションに失敗した場合でも例外を投げません。 |
| save! | `user.save!` | 成功時は`true` | レコードを保存します。バリデーションに失敗すると例外(ActiveRecord::RecordInvalid)を投げます。 |
| create | `User.create(name: 'Alice')` | 新規作成したオブジェクト | 新規レコードを生成し、成功時に保存します。バリデーションに失敗した場合でも例外を投げません。 |
| create! | `User.create!(name: 'Alice')` | 新規作成したオブジェクト | 新規レコードを生成し、保存します。バリデーションに失敗すると例外(ActiveRecord::RecordInvalid)を投げます。 |
| update | `user.update(name: 'Bob')` | 成功時は`true`、失敗時は`false` | レコードを更新します。バリデーションに失敗した場合でも例外を投げません。 |
| update! | `user.update!(name: 'Bob')` | 成功時は`true` | レコードを更新します。バリデーションに失敗すると例外(ActiveRecord::RecordInvalid)を投げます。 |
