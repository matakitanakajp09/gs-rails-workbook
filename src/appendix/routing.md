# ルーティング

> 参考：[Rails のルーティング](https://railsguides.jp/routing.html)

## ルーティングの基本

ルーティングは、リクエストされたURLに対して、どのコントローラのどのアクションを呼び出すかを決定する仕組みです。

Railsでは、`config/routes.rb`にルーティングを記述します。


その際に使用するメソッドは、`Rails.application.routes.draw`です。
ルーティング定義をラップするRails.application.routes.draw do ... endブロックは、ルーターDSLのスコープを確定するのに不可欠です。

```ruby
Rails.application.routes.draw do
  # ここにルーティングを記述する
end
```

※ `config/routes/*`にルーティングを記述することで、ファイルを分割することができます。今回の講義ではこちらの方法でルーティングを記述していきます。

```console
/workspace/
└── config
    └── routes
        ├── admin
        │   ├── authors.rb
        │   └── categories.rb
        │   ...
        ...
        ├── user
        │   ├── top.rb
        │   ...
```

### 基本的な例

```ruby
Rails.application.routes.draw do
  root "articles#index"
  get 'articles/:id', to: 'articles#show'
  post "articles", to: "articles#create"
end
```

### ルーティングの対応表

基本的にルーティングは、HTTPメソッドとパスに対して、コントローラーとアクションを紐付けます。

例えば、`Article`に関するリソースを扱う場合、以下のようなルーティングを定義することができます。

```ruby
Rails.application.routes.draw do
  get 'articles', to: 'articles#index', as: 'articles'
  get 'articles/new', to: 'articles#new', as: 'new_article'
  post 'articles', to: 'articles#create'
  get 'articles/:id', to: 'articles#show', as: 'article'
  get 'articles/:id/edit', to: 'articles#edit', as: 'edit_article'
  patch 'articles/:id', to: 'articles#update'
  put 'articles/:id', to: 'articles#update'
  delete 'articles/:id', to: 'articles#destroy'
end
```

上記の定義では、以下のような対応表が作成されます。

| HTTP Verb | Path                  | Controller#Action | Named Route Helper     |
|-----------|-----------------------|-------------------|------------------------|
| GET       | /admin/articles       | articles#index    | articles_path          |
| GET       | /admin/articles/new   | articles#new      | new_article_path       |
| POST      | /admin/articles       | articles#create   | articles_path          |
| GET       | /admin/articles/:id   | articles#show     | article_path(:id)      |
| GET       | /admin/articles/:id/edit | articles#edit  | edit_article_path(:id) |
| PATCH/PUT | /admin/articles/:id   | articles#update   | article_path(:id)      |
| DELETE    | /admin/articles/:id   | articles#destroy  | article_path(:id)      |

※ `:id`は、リクエストされたURLに含まれるIDを表します。

例）`/admin/articles/1`の場合、`:id`は`1`になります。

### recources

CRUDのアクションに対しては、`resources`メソッドを使用することで、一括でルーティングを定義することができます。

上記の対応表を`resources`メソッドを使用して定義すると、以下のようになります。

```ruby
Rails.application.routes.draw do
  resources :articles
end
```

### namespace

`namespace`を使用することで、コントローラーの名前空間を定義することができます。

例えば、`Admin::ArticlesController`の場合、以下のように定義することができます。

Filename: app/config/routes/admin/authors.rb

```ruby
# frozen_string_literal: true

Rails.application.routes.draw do
  # namespace :admin で、コントローラーの名前空間(今回の場合は、Admin::)を定義する
  namespace :admin do
    resources :authors
  end
end
```

| HTTP Verb | Path                     | Controller#Action      | Named Route Helper       |
| --------- | ------------------------ | ---------------------- | ------------------------ |
| GET       | /admin/authors           | admin/authors#index    | admin_authors_path       |
| POST      | /admin/authors           | admin/authors#create   | admin_authors_path       |
| GET       | /admin/authors/new       | admin/authors#new      | new_admin_author_path    |
| GET       | /admin/authors/:id       | admin/authors#show     | admin_author_path(:id)   |
| GET       | /admin/authors/:id/edit  | admin/authors#edit     | edit_admin_author_path(:id) |
| PATCH/PUT | /admin/authors/:id       | admin/authors#update   | admin_author_path(:id)   |
| DELETE    | /admin/authors/:id       | admin/authors#destroy  | admin_author_path(:id)   |
