---
title: "Ruby SE 5 ウェブプログラミングとMVCフレームワーク"
date: 2021-03-13 23:20:00 -0000
categories: ja
---
## Ruby SE 5 ウェブプログラミングとMVCフレームワーク

各言語を比較しながら、RubyウェブプログラミングとMVCフレームワークをリストアップする。
* ウェブ、HTTPとHTML
* Restful、JSON、シングルページとアプリ
* MVCのデザインパタン
* MVCフレームワーク

### 人気な言語とMVCフレームワークの比較

主に比較となる言語はCOBOL、VB(.net)、Java 8、JavaScript、Python 3。

機能         |Ruby (Rails)          | COBOL     | VB(.net MVVP)          | Java (Spring MVC)             | JavaScript (Vue)             | Python 3 (Flask)
-------------|-------------|------------|------------------|---------------------|-------------------------|------------------------------------
モデル       | ActiveRecord | - | -       | iBatis | - | SQLAlchemy
ビュー     | ActiveView    |  ''       | -   | -       | - | -
コントローラー | ActiveController |  ''       | -   | Controller       | - | Flask

### ウェブプログラミング、HTTPとHTMLとは
* ウェブプログラミングはブラウザでサイトコンテンツの閲覧と情報送信が目的の開発である。
* ブラウザはHTTPプロトコルでサイトサーバと通信する。HTTPプロトコルをサポートするサイトサーバの開発がフレームワークとサーバソフトがすでに開発できているから、あまり新規開発する必要がない。
* コンテンツ自身は各サイトのメインの開発仕事になる。HTMLはコンテンツのテキストフォーマットであるため、理解する必要がある。
* また、動的なコンテンツを提供するため、プログラミングで動的にHTMLのテキストコンテンツの作成が必要になる。

デモのdemo.html
```html
<html>
  <body>
    <h1>Hello!</h1>
    <p>Hello World!</p>
  </body>
</html>
```

下記の手順で、ウェブブラウザでこのドキュメントを表示することができる。
* ファイルをローカルコンピュータに格納する。例えば c:\home\user\rubyse\day5webmvc\demo.html ブラウザでファイルを開くことができる。
* ローカルコンピュータにWebサーバを立ち上げることができる。http://localhost:8080/demo.html
* クラウドのホストサーバを購入すれば、世界中にウェブを公開することができる。

### Restful、JSON、シングルページとアプリ
* ダイナミックなウェブページは、ブラウザあるいはアプリ中に画面処理のモジュール（JavaScript/Native Android iOS）、そしてサーバ上動くデータ提供するモジュールに構成される。
* ブラウザの画面処理とサーバのデータ提供処理の間に、最も適切な処理パタンはRestfulで、データフォーマットはJSONである。
* HTTPプロトコルは各URLに対してPUT GET POST DELETEのメソッドが定義されている。この四つのメソッドはちょうどデータエンティティのCreate 新規作成、Read 取得、Update 更新、Delete 削除にマッピングできる。この仕組みはRestfulである。
* 異なる言語の間、接続と操作のプロトコルのほか、通信フォーマットが必要である。よく使われているのはJSONとXMLだ。

デモのuser.json
```json
{
  "id": 1,
  "name": "Frank"
}
```

### Ruby on Rails

フォルダ 	| 目的
---------	|-------
app/	| アプリケーションのコントローラー、モデル、ビュー、ヘルパー、メーラー、チャネル、ジョブ、アサート
bin/	| アプリケーションの起動、設定、更新、配布
config/	| ルート、DBなどのコンフィグレーション
config.ru	| Rackコンフィグレーション
db/	| DBスキーマとマイグレーション
Gemfile | GEMファイル
Gemfile.lock	| Bundler gem制御ロック
lib/	| 拡張モジュール
log/	| ログ
package.json	| npmとyarnのコンフィグレーション
public/	| 静的なファイルとアサート
Rakefile	| Rakeファイル
README.md	| 説明ファイル
storage/	| Active Storageファイル
test/	| ユニットテスト
tmp/	| キャッシュとPIDファイル
vendor/	| サードパーティー
.gitignore	| 
.ruby-version	| バージョンファイル

### Rails Router
ルートコンフィグレーションの追加
config/routers.rb
```ruby
Rails.application.routes.draw do
  get "/articles", to: "articles#index" #ルート・コンフィグレーションの追加
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

シェルでコントローラーの追加
```bash
$ bin/rails　generate controller Articles index --skip-routes
```
```bash
dev\ruby\blog>rails generate controller Articles index --skip-routes
      create  app/controllers/articles_controller.rb
      invoke  erb
      create    app/views/articles
      create    app/views/articles/index.html.erb
      invoke  test_unit
      create    test/controllers/articles_controller_test.rb
      invoke  helper
      create    app/helpers/articles_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/articles.scss
```

app/controllers/articles_controller.rb
```ruby
class ArticlesController < ApplicationController
  def index
  end
end
```

app/views/articles/index.html.erb
```html
<h1>Hello, Rails!</h1>
```
