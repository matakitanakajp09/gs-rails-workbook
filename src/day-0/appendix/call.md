# メソッド呼び出し(super・ブロック付き・yield)

> [メソッド呼び出し(super・ブロック付き・yield)](https://docs.ruby-lang.org/ja/3.2/doc/spec=2fcall.html)

## 関数と定数の呼び出し方法

```ruby
person.calc_age()
person.calc_age
calc_age()
print "Hello World\n"
print
Person.new
Person::new
Person::TEXT_LENGTH
```

|  変数名     |  宣言例  |  命名規則  |
|----------|----------|----------|
|  `インスタンスメソッド`  |  `person.calc_age(); person.calc_age`  |  `.`を使用してメソッドを呼び出すことができます  |
|  `クラスメソッド`  |  `Person.new; Person::new`  |  `::`か`.`を使用してメソッド呼び出すことができます  |
|  `定数`  |  `Person::TEXT_LENGTH`  |  `Person.TEXT_LENGTH`はNG  |

## super

Rubyのsuperは、特殊なキーワードであり、サブクラス（子クラス）からスーパークラス（親クラス）のメソッドを呼び出すために使用されます。

superを呼び出すと、現在のメソッドと同じ名前のスーパークラスのメソッドが実行されます。superは引数を取ることができ、それらの引数はスーパークラスのメソッドに渡されます。

```ruby
class MySample
  def test(arg = nil)
    p arg
  end
end

class Sample < MySample
  def test(arg)
    super("test from inside")
    super(arg)
    super
    p "Called sample class"
  end
end

Sample.new.test("test from argument")
#=> "test from inside"
#=> "test from argument"
#=> "test from argument"
#=> "Called sample class"
```

別サンプル

```ruby
class Animal
  def speak
    "I'm an animal!"
  end
end

class Dog < Animal
  def speak
    "#{super} And I'm a dog!"
  end
end

dog = Dog.new
puts dog.speak
# => "I'm an animal! And I'm a dog!"
```

## ブロック

Rubyにおけるブロックは、メソッドを呼び出す際にコードのブロックを渡すことができる特性を指します。ブロックは、実行される一連の命令をカプセル化したものです。ブロックは {}（ブレース）または do..end を使って定義され、メソッド呼び出しの最後に配置されます。

|  ブロックの特徴  |
|----------|
|  `{}（ブレース）または do..endのことをブロックと呼ぶ`  |
|  `単独では存在できず，関数の引数にしかなれない`  |
|  `ブロックをProc.newでProcオブジェクトに変換すれば，callで呼び出せる`  |
|  `関数の引数にする場合は，最後の引数に & を付与する。そうするとProcオブジェクトに変換される`  |

```ruby
# {}（ブレース）を使用したブロック
[1, 2, 3].each { |num| puts num * 2 }

# do..endを使用したブロック
[1, 2, 3].each do |num|
  puts num * 2
end
```

ブロックの使い分けは、以下のようになります。

- 一行のブロック: 一行のブロックを書くときは、ブレース {} を使用するのが一般的です。これは読みやすさのための慣習です。
- 複数行のブロック: 複数行のブロックを書くときは、do..end を使用するのが一般的です。これも読みやすさのための慣習です。
- 優先度: do..end と {} は異なる優先順位を持っています。{} は do..end よりも高い優先順位を持つため、メソッドチェインの一部としてブロックを使用する場合は {} を使用することが推奨されます。

### 優先度(結合度)の違い

```ruby
names = ['Alice', 'Bob', 'Charlie']

# ブロックをdo..endで定義
puts names.map do |name|
  name.upcase
end.join(', ')

#=> undefined method `join' for nil:NilClass (NoMethodError)

# ブロックを{}で定義
puts names.map {
  |name| name.upcase
}.join(', ')

#=> ALICE, BOB, CHARLIE
```

このコードでは、names配列の各要素を大文字に変換し、その結果をカンマで連結して出力しようとしています。

しかし、do..endを使用した最初の例では、予想外の結果が得られます。このコードはnames.map do..end.join(', ')と解釈され、join(', ')メソッドがmapメソッドの戻り値ではなくmapメソッド自体に対して呼び出されてしまいます。これはdo..endの優先順位が低いためです。

一方、{}を使用した2つ目の例では、期待通りの結果が得られます。このコードはnames.map { |name| name.upcase }.join(', ')と解釈され、join(', ')メソッドがmapメソッドの戻り値に対して呼び出されます。これは{}の優先順位が高いためです。

したがって、メソッドチェインの中でブロックを使用する場合には、{}を使用することが推奨されます。

内部的な優先度として、以下のような流れで処理が行われているため、do..endでエラーが生じます。

|  実行順番  |  do..end  |  {}  |
|----------|----------|----------|
|  `1`  |  `mapメソッド`  |  `mapメソッド` |
|  `2`  |  `joinメソッド`  |  `{}` |
|  `3`  |  `do..end`  |  `joinメソッド` |
|  `結果`  |  `undefined method 'join' for nil:NilClass`  |  `ALICE, BOB, CHARLIE` |
|  `説明`  |  `mapメソッド自体にjoinメソッドが実行された`  |  `mapメソッドの戻り値にjoinメソッドが実行された` |

## ブロック付きメソッド呼び出し

Rubyにおけるブロック付きメソッド呼び出しは、メソッドを呼び出す際にコードのブロックを渡すことができる特性を指します。ブロックは、実行される一連の命令をカプセル化したもので、それはメソッド内で yield を使用して呼び出すことができます。

```ruby
def greet(&block)
  puts "Hello, #{block.call}!"
end

greet { "world" }  # => "Hello, world!"
```

block.call は、ブロックを呼び出すための方法の1つです。他にも、yield を使用する方法があります。

```ruby
def greet(&block)
  puts "Hello, #{yield}!"
end

greet { "world" }  # => "Hello, world!"
```

yieldを使用する場合、blockという変数は不要になるため、以下のように書くこともできます。

```ruby
def greet
  puts "Hello, #{yield}!"
end

greet { "world" }  # => "Hello, world!"
```

### ブロックを使用した例

```ruby
def to_darling
  puts "愛する人に送るメールです"
  # 愛する人に送るメールの内容を書く
  # eg: DarlingMailer.i_love_you.deliver_now
  puts "送信したよ♡"
end

def to_friend
  puts "友達に送るメールです"
  # 友達に送るメールの内容を書く
  # eg: FriendMailer.what_is_up.deliver_now
  puts "送った！"
end

def send_mail
  puts "メールを送信開始します"
  yield
end
```

愛する人にメールを送る場合は、to_darlingメソッドを呼び出すだけで良いので、以下のように書くことができます。

```ruby
send_mail do
  to_darling
end
#=>メールを送信開始します
# 愛する人に送るメールです
# 送信したよ♡
```

友達にメールを送る場合は、to_friendメソッドを呼び出すだけで良いので、以下のように書くことができます。

```ruby
send_mail do
  to_friend
end
#=> メールを送信開始します
# 友達に送るメールです
# 送った！
```

ブロックごとに処理を定義しておけば、メールを送る相手によって、メールの内容を変えることができます。ブロック機能により、メソッドの処理を柔軟に変更することができます。

> [Ruby block/proc/lambdaの使いどころ](https://qiita.com/kidach1/items/15cfee9ec66804c3afd2) <br>
> [Rubyの動かないコード　（初級編）　ブロックとクロージャの性質](https://language-and-engineering.hatenablog.jp/entry/20101118/p1)