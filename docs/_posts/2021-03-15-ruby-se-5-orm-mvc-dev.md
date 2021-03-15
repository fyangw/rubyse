---
title: "Ruby SE 5 ORMとMVCの開発"
date: 2021-03-15 20:40:00 -0000
categories: ja
---
## Ruby SE 5 ORMとMVCの開発

Ruby on RailsのORMはDB操作方法をオブジェクトで実装する。

## 記事一覧ページの作成

### モデルの作成
コマンドラインでモデル作成する。
```bash
rails generate model Article title:string body:text
```
下記メッセージが表示されたらモデルが正常に作成された。
```
      invoke  active_record
      create    db/migrate/20210315124325_create_articles.rb
      create    app/models/article.rb
      invoke    test_unit
      create      test/models/article_test.rb
      create      test/fixtures/articles.yml
```

作成されたデータベース変更ファイルは
db/migrate/<timestamp>_create_articles.rb
```ruby
class CreateArticles < ActiveRecord::Migration[6.0]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :body

      t.timestamps
    end
  end
end
```

データベース・マイグレーションを実行する。
```bash
rails db:migrate
```
下記のメッセージが表示されたらマイグレーションが完了した。
```bash
==  CreateArticles: migrating ===================================
-- create_table(:articles)
   -> 0.0018s
==  CreateArticles: migrated (0.0018s) ==========================
```

irbで機能を確認する。
```ruby
rails console

irb> article = Article.new(title: "Hello Rails", body: "I am on Rails!")
irb> article.save
(0.1ms)  begin transaction
irb> INSERT INTO "articles" ("title", "body", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "Hello Rails"], ["body", "I am on Rails!"], ["created_at", "2020-01-18 23:47:30.734416"], ["updated_at", "2020-01-18 23:47:30.734416"]]
(0.9ms)  commit transaction
=> true

irb> article
=> #<Article id: 1, title: "Hello Rails", body: "I am on Rails!", created_at: "2020-01-18 23:47:30", updated_at: "2020-01-18 23:47:30">

irb> Article.find(1)
=> #<Article id: 1, title: "Hello Rails", body: "I am on Rails!", created_at: "2020-01-18 23:47:30", updated_at: "2020-01-18 23:47:30">

irb> Article.all
=> #<ActiveRecord::Relation [#<Article id: 1, title: "Hello Rails", body: "I am on Rails!", created_at: "2020-01-18 23:47:30", updated_at: "2020-01-18 23:47:30">]>
```

### コントローラーの修正
app/controllers/articles_controller.rbを修正する
```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all #追加
  end
end
```

### ビューの修正
app/views/articles/index.html.erbを修正する。
```html
<h1>Articles</h1>

<% #追加開始 %>
<ul>
  <% @articles.each do |article| %>
    <li>
      <%= article.title %>
    </li>
  <% end %>
</ul>
<% #追加終了 %>
```

### サーバの起動
rails serverでサーバを起動してから、http://localhost:3000/ をブラウザでページを開くことができる。そして、下記のメッセージが表示される。
```bash
rails server

=> Booting Puma
=> Rails 6.1.3 application starting in development
=> Run `bin/rails server --help` for more startup options
*** SIGUSR2 not implemented, signal based restart unavailable!
*** SIGUSR1 not implemented, signal based restart unavailable!
*** SIGHUP not implemented, signal based logs reopening unavailable!
Puma starting in single mode...
* Puma version: 5.2.2 (ruby 3.0.0-p0) ("Fettisdagsbulle")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 12128
* Listening on http://[::1]:3000
* Listening on http://127.0.0.1:3000
Use Ctrl-C to stop
Started GET "/" for ::1 at 2021-03-15 20:55:12 +0800
   (1.0ms)  SELECT sqlite_version(*)
   (0.2ms)  SELECT "schema_migrations"."version" FROM "schema_migrations" ORDER BY "schema_migrations"."version" ASC
Processing by ArticlesController#index as HTML
  Rendering layout layouts/application.html.erb
  Rendering articles/index.html.erb within layouts/application
  Article Load (0.2ms)  SELECT "articles".* FROM "articles"
  ↳ app/views/articles/index.html.erb:4
  Rendered articles/index.html.erb within layouts/application (Duration: 9.8ms | Allocations: 4180)
[Webpacker] Everything's up-to-date. Nothing to do
  Rendered layout layouts/application.html.erb (Duration: 62.0ms | Allocations: 10282)
Completed 200 OK in 82ms (Views: 69.5ms | ActiveRecord: 0.9ms | Allocations: 13927)
```

### Railsサーバの処理フロー
* ブラウザがHTTPプロトコルのリクエストを送信する。 GET http://localhost:3000
* Railsアプリケーションがこのリクエストを受け取る。
* RailsのルーターがルートアクセスをArticlesControllerのindexアクションにマッピングする。
* indexアクションはArticleモデルを使って、DBから全てのarticlesを取得する。
* Railsが自動的にビューのapp/views/articles/index.html.erbを表示する。
* ビューのなかのERBソースコードが計算されてHTMLに出力される。
* サーバがHTMLリスポンスをブラウザに戻す。

## 記事詳細ページの作成
ルーティング・コンフィグレーションの追加
config/routes.rb
```ruby
Rails.application.routes.draw do
  root "articles#index"

  get "/articles", to: "articles#index"
  get "/articles/:id", to: "articles#show" #追加
end
```の
追加された一行の意味は、HTTPアクセスの"GET http://localhost:3000/articles/1" の場合、1は:idの値としてキャプチャーされる。そして、ArticlesControllerのshow actionのparams[:id]に引き渡される。

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end
end
```

showビューにHTML出力の追加
app/views/articles/show.html.erb
```html
<h1><%= @article.title %></h1> #追加

<p><%= @article.body %></p> #追加
```

```bash
app/views/articles/index.html.erb to its page:

<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <a href="/articles/<%= article.id %>">
        <%= article.title %>
      </a>
    </li>
  <% end %>
</ul>
```

```ruby
Rails.application.routes.draw do
  root "articles#index"

  resources :articles
end
```

```ruby
rails routes
      Prefix Verb   URI Pattern                  Controller#Action
        root GET    /                            articles#index
    articles GET    /articles(.:format)          articles#index
 new_article GET    /articles/new(.:format)      articles#new
     article GET    /articles/:id(.:format)      articles#show
             POST   /articles(.:format)          articles#create
edit_article GET    /articles/:id/edit(.:format) articles#edit
             PATCH  /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
```

app/views/articles/index.html.erb:
```
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <a href="<%= article_path(article) %>">
        <%= article.title %>
      </a>
    </li>
  <% end %>
</ul>
```

app/views/articles/index.html.erb becomes:
```
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <%= link_to article.title, article %>
    </li>
  <% end %>
</ul>
```
