---
title: "Ruby SE 5 ORMとMVCの開発"
date: 2021-03-15 20:40:00 -0000
categories: ja
---
## Ruby SE 5 ORMとMVCの開発

Ruby on RailsのORMはDB操作方法をオブジェクトで実装する。

* [モデルの作成](#モデルの作成)
  * [railsコマンドでモデルの作成](#railsコマンドでモデルの作成)
  * [データベース・マイグレーション・スクリプト](#データベース・マイグレーション・スクリプト)
  * [railsコマンドでデータベース・マイグレーションの実行](#railsコマンドでデータベース・マイグレーションの実行)
  * [irbでモデル新規挿入、ID検索、一覧取得機能の確認](#irbでモデル新規挿入、ID検索、一覧取得機能の確認)
* [一覧ページの作成](#一覧ページの作成)
  * [一覧コントローラーの修正](#一覧コントローラーの修正)
  * [一覧ビューの修正](#一覧ビューの修正)
  * [railsコマンドでサーバの起動](#railsコマンドでサーバの起動)
  * [Railsサーバの処理フロー](#Railsサーバの処理フロー)
* [詳細ページの作成](#詳細ページの作成)
  * [詳細ページルーティングの追加](#詳細ページルーティングの追加)
  * [詳細ページコントローラーの修正](#詳細ページコントローラーの修正)
  * [詳細ページビューの追加](#詳細ページビューの追加)
  * [一覧ページの詳細ページへの遷移リンクの修正](#一覧ページの詳細ページへの遷移リンクの修正)
* [リソースアクションの追加](#リソースアクションの追加)
  * [railsコマンドでroutesの確認](#railsコマンドでroutesの確認)
  * [一覧ページリンクの_pathの利用](#一覧ページリンクの_pathの利用)
  * [一覧ページリンクのlink_toの利用](#一覧ページリンクのlink_toの利用)

## モデルの作成

### railsコマンドでモデルの作成
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

### データベース・マイグレーション・スクリプト
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

### railsコマンドでデータベース・マイグレーションの実行
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

### irbでモデル新規挿入、ID検索、一覧取得機能の確認
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

## 一覧ページの作成

### 一覧コントローラーの修正
app/controllers/articles_controller.rbを修正する
```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all #追加
  end
end
```

### 一覧ビューの修正
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

### railsコマンドでサーバの起動
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

## 詳細ページの作成

### 詳細ページルーティングの追加
ルーティング・コンフィグレーションの追加
config/routes.rb
```ruby
Rails.application.routes.draw do
  root "articles#index"

  get "/articles", to: "articles#index"
  get "/articles/:id", to: "articles#show" #追加
end
```
追加された一行の意味は、HTTPアクセスの"GET http://localhost:3000/articles/1" の場合、1は:idの値としてキャプチャーされる。そして、ArticlesControllerのshow actionのparams[:id]に引き渡される。


### 詳細ページコントローラーの修正
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

### 詳細ページビューの追加
showビューにHTML出力の追加
app/views/articles/show.html.erbの追加
```html
<h1><%= @article.title %></h1> #追加

<p><%= @article.body %></p> #追加
```

### 一覧ページの詳細ページへの遷移リンクの修正
app/views/articles/index.html.erbの修正
```html
<h1>Articles</h1>

<% # 追加開始 %>
<ul>
  <% @articles.each do |article| %>
    <li>
      <a href="/articles/<%= article.id %>">
        <%= article.title %>
      </a>
    </li>
  <% end %>
</ul>
<% # 追加終了 %>
```

## リソースアクションの追加
config/route.rbの追加
```ruby
Rails.application.routes.draw do
  root "articles#index"

  resources :articles #追加
end
```

### railsコマンドでroutesの確認
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

### 一覧ページリンクの_pathの利用
app/views/articles/index.html.erbの修正、リンク<a/>タグの追加
```html
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

### 一覧ページリンクのlink_toの利用
app/views/articles/index.html.erbの修正、リンクの追加
```html
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <%= link_to article.title, article %>
    </li>
  <% end %>
</ul>
```

## 新規ページの作成

### コントローラーにnewとcreateアクション追加の修正
app/controllers/articles_controller.rbのshowメソッドの下にnewとcreateメソッドを追加する。
```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end
  # 追加開始
  def new
    @article = Article.new
  end

  def create
    @article = Article.new(title: "...", body: "...")

    if @article.save
      redirect_to @article
    else
      render :new
    end
  end
  # 追加終了
end
```

### newビューの追加及びフォーム・ビルダーの利用
app/views/articles/new.html.erb新規追加、form関連はフォーム・ビルダーで作成する。
```html
<h1>New Article</h1>

<%= form_with model: @article do |form| %>
  <div>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```

上記のerbは下記のhtmlに変換される。
```html
<form action="/articles" accept-charset="UTF-8" method="post">
  <input type="hidden" name="authenticity_token" value="...">

  <div>
    <label for="article_title">Title</label><br>
    <input type="text" name="article[title]" id="article_title">
  </div>

  <div>
    <label for="article_body">Body</label><br>
    <textarea name="article[body]" id="article_body"></textarea>
  </div>

  <div>
    <input type="submit" name="commit" value="Create Article" data-disable-with="Create Article">
  </div>
</form>
```

### パラメータの利用
app/controllers/articles_controller.rb 
```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new
    end
  end

  private
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

### 項目のチェックとエラーメッセージ
app/models/article.rb
```ruby
class Article < ApplicationRecord
  validates :title, presence: true
  validates :body, presence: true, length: { minimum: 10 }
end
```

app/views/articles/new.html.erb
```html
<h1>New Article</h1>

<%= form_with model: @article do |form| %>
  <div>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
    <% @article.errors.full_messages_for(:title).each do |message| %>
      <div><%= message %></div>
    <% end %>
  </div>

  <div>
    <%= form.label :body %><br>
    <%= form.text_area :body %><br>
    <% @article.errors.full_messages_for(:body).each do |message| %>
      <div><%= message %></div>
    <% end %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```

```ruby
  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new
    end
  end
```

```html
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <%= link_to article.title, article %>
    </li>
  <% end %>
</ul>

<%= link_to "New Article", new_article_path %>
```
