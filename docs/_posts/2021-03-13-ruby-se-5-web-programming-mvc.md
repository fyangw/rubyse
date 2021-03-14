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
  id: 1,
  name: "Frank"
}
```

### Ruby

```ruby

```
