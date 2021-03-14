---
title: "Ruby SE 2 開発環境の準備"
date: 2021-03-13 11:50:00 -0000
categories: ja
---

## Ruby開発環境の準備

### Ruby
Rubyのインストーラーをダンロードし、インストールする。
https://rubyinstaller.org/downloads/
今の最新バージョンはRuby+Devkit 3.0.0-1 (x64)

### SQLite3
SQLite3のプリコンパイルバージョンをダンロードし、解凍する。
https://www.sqlite.org/download.html
今の最新バージョンはsqlite-dll-win64-x64-3350000.zipとsqlite-tools-win32-x86-3350000.zip

### Node JS
Nodeのインストーラーをダウンロードし、インストールする。
https://nodejs.org/en/download/
Windows 64bit msi

### Yarn
Yarnのパッケージをダウンロードし、解凍する。
```bash
npm install -g yarn
```

### Ruby on Rails

```bash
#リポが遅ければ、ミラーを設定すれば解決できる
gem sources --remove https://rubygems.org/
gem sources -a http://gems.ruby-china.org/
gem sources -a https://mirrors.ustc.edu.cn/rubygems/
gem sources -u
```

コマンドラインでRailsをインストールする。
```bash
gem install rails
```
下記のメッセージが表示されたら、インストールが正常に完了した。
```
Done installing documentation for rack, concurrent-ruby, sprockets, zeitwerk, tzinfo, i18n, activesupport, nokogiri, crass, loofah, rails-html-sanitizer, rails-dom-testing, rack-test, erubi, builder, actionview, actionpack, sprockets-rails, thor, method_source, railties, mimemagic, marcel, activemodel, activerecord, globalid, activejob, activestorage, actiontext, mini_mime, mail, actionmailer, actionmailbox, websocket-extensions, websocket-driver, nio4r, actioncable, rails after 142 seconds
38 gems installed
```

sqlite3-rubyのインストール
```bash
gem install sqlite3-ruby
```



リファレンス
https://guides.rubyonrails.org/getting_started.html
