# Day1 ハンズオン

Day1のハンズオンページです。

## 目次

- ハンズオンの準備
- ハンズオンの前に
  - Web開発の全体像
  - MVCとルーティングについて
  - データベースについて
- ハンズオン
  - Day0の振り返り
  - Day1で使用するソースコードの共有
  - User側の記事詳細画面を実装する
- Day2に向けて
  - Rails開発でよく使うファイル場所やコマンド
  - 課題について

## ハンズオンの準備

### git pull

Day1は、`lesson-day-1`ブランチを使用します。お使いの環境にソースコードを取り込んでください。

[command]

作業中の場合は、一旦コミットするか、stashしてからpullしてください。

```console
git pull origin lesson-day-1
```

### git commitが必要な場合

[command]

```console
git add -p
git commit -m "commit message"
```

### git stashが必要な場合

[command]

```consoe
git stash
```

※ VSCode Remote Containerに接続前提で進みます。

### データベースの再作成

[command]

db/migrate以下のファイルを追加しましたので、データベースを再作成してください。

```console
bin/rails db:reset
```

`bin/rails db:reset`は、以下のコマンドをまとめて実行しています。

| コマンド | 内容 |
|:--|:--|
| bin/rails db:drop | データベースのドロップ |
| bin/rails db:create | データベースの作成 |
| bin/rails db:schema:load | スキーマのロード |
| bin/rails db:seed | シードデータのロード。db/seeds.rbに記述されたコードが実行され、データベースに初期データが投入される |

### 動作確認

[command]

ターミナルで以下のコマンドを実行し、Railsサーバーを起動してください。

```console
bin/rails s
```

もう一つターミナルを開き、以下のコマンドを実行してください。

```console
sh entrypoint.local.sh
```

[output]

ブラウザで以下のURLにアクセスしてください。

[http://localhost:3000/admin](http://localhost:3000/admin)

## ハンズオンの前に

### Web開発の全体像

[Ruby on Railsを用いたWeb開発の全体像](../day-0/web-development-overview.md)のページを参照ください。

### MVCとルーティングについて

- [MVCとルーティング](../appendix/mvc-r.md)
- [ルーティングについて](../appendix/routing.md)

### データベースについて

- [データベース構造と設計](../appendix/database-design.md)
- [MyCMSのER図](../day-0/goal.md)

## ハンズオン

### Day0の振り返り

[タグ管理](../day-0/cms-tag.md)のページを使用する

### Day1で使用するソースコードの共有

lesson-day-1ブランチの内容は、以下の通りです。

#### Admin側

- 記事管理機能
  - 記事一覧ページ
  - 記事詳細ページ
  - 記事作成ページ
  - 記事編集ページ
- 著者管理機能
  - 著者一覧ページ
  - 著者詳細ページ
  - 著者作成ページ
  - 著者編集ページ
- 連載管理機能
  - 連載一覧ページ
  - 連載詳細ページ
  - 連載作成ページ
  - 連載編集ページ
- カテゴリ管理機能
  - カテゴリ一覧ページ
  - カテゴリ詳細ページ
  - カテゴリ作成ページ
  - カテゴリ編集ページ
- タグ管理機能
  - タグ一覧ページ
  - タグ詳細ページ
  - タグ作成ページ
  - タグ編集ページ
- イチオシ管理機能
  - イチオシ一覧ページ
  - イチオシ詳細ページ
  - イチオシ作成ページ
  - イチオシ編集ページ
- バナー管理機能
  - バナー一覧ページ
  - バナー詳細ページ
  - バナー作成ページ
  - バナー編集ページ

#### User側

- トップページ
- カテゴリ詳細ページ
- 連載一覧ページ
- 連載詳細ページ

### User側の記事詳細画面を実装する

まずは、User側の記事詳細に使用するコントローラーを実装します。

[command]

```bash
touch app/controllers/user/articles_controller.rb
```

[code]

Filename: app/controllers/user/articles_controller.rb

```ruby
# frozen_string_literal: true

class User::ArticlesController < User::ApplicationController
  def show
    @article = Article.preload(:author).find(params[:id])

    render "/user/error_404" and return unless @article.present?
  end
end
```

次に、User側の記事詳細に使用するビューを実装します。

[command]

```bash
mkdir -p app/views/user/articles && touch app/views/user/articles/show.html.erb
```

[code]

Filename: app/views/user/articles/show.html.erb

```erb
<main id="kiji-main">
  <div class="container max-width-980">
    <div class="article-main">
      <h1><%= @article&.title %></h1>
      <div class="article-main-author">
        <div class="article-main-author-img flex-center">
          <div class="flex-center">
            <%= @article&.author&.name[0] %>
          </div>
        </div>
        <div class="article-main-author-box">
          <div class="article-main-author-box-name">
            <%= @article&.author&.name %>
          </div>
          <div class="article-main-author-box-note">
            <time datetime="<%= @article&.published_at %>">
              <%= l(@article&.published_at, format: :article_show) if @article&.published_at.present? %>
            </time>
          </div>
        </div>
      </div>
      <div class="article-main-thumbnail">
      </div>
      <div class="article-main-content mt-3">
        <%= @article&.content %>
      </div>
    </div>
    <div class="footer-breadcrumb">
      <ul class="breadcrumb">
        <li>
          <%= link_to user_top_path do %>
            <%= t ".breadcrumb.top" %>
          <% end %>
        </li>
        <li>
          <%= link_to article_path(id: @article[:id]) do %>
            <%= @article&.title %>
          <% end %>
        </li>
      </ul>
    </div>
  </div>
</main>
```

### 管理画面から記事を作成する

管理画面に移動して記事を作成してみます。

## Day2に向けて

### Rails開発でよく使うファイル場所やコマンド

### Gemfile

Gemとは、Rubyで使えるライブラリのことです。Railsでは、GemfileにGemを記述することで、Gemを使用することができます。

RailsはGemのエコシステムが充実しているため、Gemを使用することで、簡単に機能を追加することができると言われています。

Filename: Gemfile

#### よく使うコマンド集

- [Rails開発でよく使うコマンド](../appendix/rails-dev-command.md)

#### CSSを変更したい場合

※今回CSSにふれませんが、以下にCSSのファイルが置かれています。

Directory: app/assets/stylesheets

#### JavaScriptを変更したい場合

※今回JavaScriptに触れませんが、以下にJavaScriptのファイルが置かれています。

Directory: app/javascript

#### binding.pry

デバックする際に役に立つ機能を紹介します。画面にアクセスした時に、どんな値が入っているかを確認したい場合など使用します。

[code]

`Admin::TagsController#index`に`binding.pry`を追加します。

Filename: app/controllers/admin/tags_controller.rb

```ruby
# frozen_string_literal: true

class Admin::TagsController < Admin::ApplicationController
  def index
    @tags = Tag.all
    binding.pry
  end
end
```

[output]

画面にアクセスすると、Railsサーバーのコンソールを見ると、`binding.pry`を追加した箇所で処理が止まっています。

```bash
From: /workspace/app/controllers/admin/tags_controller.rb:6 Admin::TagsController#index:

    4: def index
    5:   @tags = Tag.all
 => 6:   binding.pry
    7: end
```

[command]

以下を実行すると、`@tags`に入っている値を確認することができます。

```bash
@tags
```

[output]

```bash
[1] pry(#<Admin::TagsController>)> @tags
=>   Tag Load (2.7ms)  SELECT "tags".* FROM "tags"
  ↳ app/controllers/admin/tags_controller.rb:6:in `index'
[#<Tag:0x0000ffff7b0f0d40
  id: "0fdd7c38-8f11-4965-b872-6ac55e437012",
  name: "ポエム",
  created_at: Sun, 09 Jul 2023 01:24:47.937235000 JST +09:00,
  updated_at: Sun, 09 Jul 2023 01:24:47.937235000 JST +09:00>,
 #<Tag:0x0000ffff7b0f0c50
  id: "0e431892-e797-4649-a596-be21b21374ce",
  name: "非認知能力",
  created_at: Sun, 09 Jul 2023 01:24:47.942586000 JST +09:00,
  updated_at: Sun, 09 Jul 2023 01:24:47.942586000 JST +09:00>]
[2] pry(#<Admin::TagsController>)> 
```

#### bin/rails c

実際にRailsコンソールを起動しながら、コードを実行してみます。

#### bin/rails routes

実際にルーティングを確認しましょう。

### 課題

実際に自分で手を動かしてみるのが一番身につきますので、以下の課題をやってみましょう。

- サンプルで用意したソースコードの復習
  - 不具合があれば皆さんに共有お願いします！そして修正してみてください！

- Day3に向けて実装したい機能を考えてみましょう。余裕があれば実装してください！
  - フロントエンドの実装でも良いですし、
  - サーバーサイドの実装でも良いです
  - 実装の方針が分からなければ、ぜひ聞いてください！
