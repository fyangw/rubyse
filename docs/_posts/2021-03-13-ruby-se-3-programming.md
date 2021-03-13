---
title: "Ruby SE プログラミングの基本知識"
date: 2021-03-13 13:23:00 -0000
categories: ja
---

## Ruby プログラミング

各言語を比較しながら、Rubyプログラミングの基本知識をリストアップする。

主に比較となる言語はCOBOL、VB(.net)、Java、Python。


機能         |Ruby          | COBOL     | VB                | Java8               | JavaScript             | Python3
-------------|-------------|------------|-------------------|--------------------|-------------------------|---------
数式         |  `1+2*3`     |  `1+2*3`  | `1+2*3`           |   `1+2*3`           |  `1+2*3`               |  `1+2*3`   
文字列       |  `"abc"`     |  `"abc"`  | `"abc"`           |   `"abc"`           |`"ab"`,`'ab'`          |`"ab"`,`'ab'`,`"""ab"""`,`'''ab'''`
変数         |  `i`         |  `i`      | `Dim i as Integer`|`int i = 0;`         |  `i=0;`               |  `i=0`
配列初期化   |`a=[1,2,3]`    |  ``       | `Dim a`          |`int[] a={1,2,3};`   |  `a=[1,2,3];`          |  `a=[1,2,3]`
配列利用     |`a[0]`         |  ``       | `a(0)`           |`a[0]`               |  `a[0]`                |  `a[0]`
条件制御フロー | `if...`       |  ``     | `If...Then...Else`|`if(...){...}else{}` |`if(...){...}else{...}`|  `if...: else: ...`  
ロープ制御フロー| `for...in...[do]...end`,`.each do \|e\|...end`|  ``    | `For...Next`      |`for(...){...}`      | `for(...){...}`       |  `for ...`  
関数制御フロー| `def ...`      |  ''     | `Function ...`      |``      | `function...(...){...}`       |  `def ...: ...`  



### Ruby Demo
```ruby
a = [1, 2, 3]
sum = 0
a.each do |e|
  sum = sum + e
end
print sum
```

```ruby
a = [1, 2, 3]
sum = 0
for i in 0..2
  sum = sum + a[i]
end
print sum
```



リファレンス
https://docs.microsoft.com/ja-jp/dotnet/visual-basic/programming-guide/language-features/
