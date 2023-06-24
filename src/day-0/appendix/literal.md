# リテラル

数字の1や文字列"hello world"のようにRubyのプログラムの中に直接記述できる値の事をリテラルといいます。

|  リテラル     |    |    |
|----------|----------|----------|
|  `数値リテラル`  |  `整数`<br>`符号付き整数`<br>`浮動小数点`<br>`16進整数` |  `123`<br>`-123`<br>`123.45`<br>`0xffff`  |
|  `文字列リテラル`  |    |  `"Yuki Tanaka"`<br>`'This is a string expression'`  |
|  `配列式`  |    |  `[1, 2, 3]`<br>`%w(a b c)`<br>`%W(a b c)`  |
|  `ハッシュ式`  |   |  `{ 1 => 2, 2 => 4, 3 => 6}`<br>`{ :a => "A", :b => "B", :c => "C" }`<br>`{ a:"A", b:"B", c:"C" }`<br>`{ "a":"A", 'b':"B", "c":"C" }`  |
|  `シンボル`  |    |  `:class`<br>`:method!`<br>`:andthisis?`<br>`:@var`<br>`:@@var`<br>`:+`  |
|  `%記法`  |    |  `%w!STRING! : 要素が文字列の配列(空白区切り)`<br>`%i!STRING! : 要素がシンボルの配列(空白区切り)`  |


## 数値リテラル

例

```ruby
def main
  default_tax = 0.1
  price = 100
  result = (price * (1 + default_tax)).to_i
  puts "result: #{result}"
end

main
#=> 110
```

## 文字列リテラル

例

```ruby
def main
  name = "Yuki Tanaka"
  puts name
  puts "My name is #{name}." # 式展開
  puts "My name is" + name + "."
end

main
#=> Yuki Tanaka
#=> My name is Yuki Tanaka.
#=> My name is Yuki Tanaka.
```

## 配列式

例

```ruby
def main
  arr = [1, 2, 3]
  for arr in a
    puts a
  end
end

main
#=> 1
# 2
# 3
```

## ハッシュ式

例

```ruby
def main
  hash = {
    :name => "Yuki Tanaka",
    :age => 25,
    :company => "Kidsweekend Inc."
  }
  hash.each do |k, v|
    puts "key: #{k}, value: #{v}"
  end
end

main
#=> key: name, value: Yuki Tanaka
# key: age, value: 25
# key: company, value: Kidsweekend Inc.
```

## シンボル

例

```ruby
def main
  puts :'foo-bar'
  puts :"foo-bar" 
  puts %s{foo-bar}
end

main
#=> :"foo-bar"
#=> :"foo-bar"
#=> :"foo-bar"
```

## %記法

文字列リテラル、コマンド出力、正規表現リテラル、配列式、シンボルでは、 %で始まる形式の記法を用いることができます。

例

```ruby
def main
  %w(foo bar baz) # ['foo', 'bar', 'baz']と同じ
  %i(foo bar baz) # [:foo, :bar, :baz]と同じ
end
```