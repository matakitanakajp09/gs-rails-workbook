# プログラム・文・式

プログラムは、式を並べたものです。まずは恒例の `Hello World!`を表示させましょう。

```ruby
print "hello world!\n"  #=> Hello World!
```

```ruby
puts "Hello World!\n"  #=> Hello World!
```

上記は同じ出力結果になります。違う書き方だけど同じような出力結果になる

## 式

```ruby
true
false
1+2+3+4+5*3
filter_articles()
if true then puts "ok" else "ng" end
```

Rubyの式

|  項目                           ||
|--------------------------------|-----|
| 式                             |  [変数と定数](variables.md)  |
|                                |  [リテラル](literal.md)  |
|                                |  [演算子式](operator.md)  |
|                                |  [制御構文](control.md)  |
|                                |  [メソッド呼び出し(super・ブロック付き・yield)](call.md)  |
|                                |  [クラス/メソッドの定義](def.md)  |
|  式のグルーピング                 |  (), {}, do end  |
|  式の評価                        |  式は評価されると値(評価値)が決まり、その値を返します。<br>ただし、return、break、nextといったものは値を返しません。これらは評価された時点で制御が移ってしまいます  |
|  空の式                          |  nilを返します。  |
|  メソッドの引数に指定できるか否か    |  メソッドの引数に指定できない式を「文」と呼び分ける場合があります。  |
|  文：メソッドの引数に指定できない式  |  and, or, not, if/unless/rescue修飾式, ...  |
