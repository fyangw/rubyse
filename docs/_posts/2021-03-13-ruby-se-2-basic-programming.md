---
title: "Ruby SE 2 プログラミングの基本知識"
date: 2021-03-13 13:23:00 -0000
categories: ja
---

## Ruby SE 2 プログラミングの基本知識

各言語を比較しながら、Rubyプログラミングの基本知識をリストアップする。

主に比較となる言語はCOBOL、VB(.net)、Java、Python。


機能         |Ruby          | COBOL     | VB                | Java 8              | JavaScript             | Python 3
-------------|-------------|------------|-------------------|--------------------|-------------------------|---------
数式         |  `1+2*3`     |  `1+2*3`  | `1+2*3`           |   `1+2*3`           |  `1+2*3`               |  `1+2*3`   
文字列       |  `"abc"`     |  `"abc"`  | `"abc"`           |   `"abc"`           |`"ab"`,`'ab'`          |`"ab"`,`'ab'`,`"""ab"""`,`'''ab'''`
変数         |  `i`         |  `i`      | `Dim i as Integer`|`int i = 0;`         |  `i=0;`               |  `i=0`
配列初期化   |`a=[1,2,3]`    |  ``       | `Dim a`          |`int[] a={1,2,3};`   |  `a=[1,2,3];`          |  `a=[1,2,3]`
配列利用     |`a[0]`         |  ``       | `a(0)`           |`a[0]`               |  `a[0]`                |  `a[0]`
条件制御フロー | `if...elsif...end`       |  ``     | `If...Then...Else`|`if(...){...}else{}` |`if(...){...}else{...}`|  `if...: else: ...`  
ロープ制御フロー| `for...in...[do]...end`,`.each do \|e\|...end`|  ``    | `For...Next`      |`for(...){...}`      | `for(...){...}`       |  `for ...`  
関数制御フロー| `def ...`      |  ''     | `Function ...`      |``      | `function...(...){...}`       |  `def ...: ...`  



### Ruby Demo
```ruby
a = [1, 2, 3]
s = 0
a.each do |e|
  s = s + e
end
print s
```

```ruby
a = [1, 2, 3]
s = 0
for i in 0..2
  s = s + a[i]
end
print s
```

### COBOL

```cobol
```

### VB
```vb
Dim a = [1, 2, 3]
Dim s = 0
For i = 0 To 2
    s = s + a(1)
Next
Print s
```

### Java 8
```Java
public class Demo {
    public static void main(String[] args) {
        int[] a = {1, 2, 3};
        int s = 0;
        for (int i = 0; i < a.length; i ++) {
            s += a[i];
        }
        System.out.println(s);
    }
}
```

### JavaScript
```JavaScript
a = [1, 2, 3];
s = 0;
for (i = 0; i < a.length; i ++) {
    s += a[i];
}
console.log(s);
```

### Python 3
```python
a = [1, 2, 3]
s = 0
for i in range(0, 2):
  s = s + a[i]
print (s)
```


リファレンス
https://docs.microsoft.com/ja-jp/dotnet/visual-basic/programming-guide/language-features/
