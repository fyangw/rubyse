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

### Ruby on Rails
コマンドラインでRailsをインストールする。
```
gem install rails
```
下記のメッセージが表示されたら、インストールが正常に完了した。
```
Done installing documentation for rack, concurrent-ruby, sprockets, zeitwerk, tzinfo, i18n, activesupport, nokogiri, crass, loofah, rails-html-sanitizer, rails-dom-testing, rack-test, erubi, builder, actionview, actionpack, sprockets-rails, thor, method_source, railties, mimemagic, marcel, activemodel, activerecord, globalid, activejob, activestorage, actiontext, mini_mime, mail, actionmailer, actionmailbox, websocket-extensions, websocket-driver, nio4r, actioncable, rails after 142 seconds
38 gems installed
```

リファレンス
https://guides.rubyonrails.org/getting_started.html
