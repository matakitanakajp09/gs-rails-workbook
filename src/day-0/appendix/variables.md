# 変数と定数

Rubyはいくつかの変数を持ちます。

|  変数名     |  宣言例  |  命名規則  |
|----------|----------|----------|
|  `ローカル変数`  |  `name = "Yuki Tanaka"; age = 25`  |    |
|  `インスタンス変数`  |  `@name = "Yuki Tanaka"; @age = 25`  |  `@`で始まります  |
|  `クラス変数`  |  `@@name = "Yuki Tanaka"; @@age = 25`  |  `@@`で始まります  |
|  `グローバル変数`  |  `$name = "Yuki Tanaka"; $age = 25`  |  `$`で始まります  |
|  `定数`  |  `NAME = "Yuki Tanaka"; AGE = 25`  |  アルファベット大文字([A-Z])で始まる識別子  |


本ページで登場するクラスについては[クラス/メソッドの定義](def.md)で詳しく確認します。

## ローカル変数

ローカル変数のスコープは、宣言した位置からその変数が宣言されたブロック、メソッド定義、またはクラス/モジュール定義の終りまでです。

```ruby

# (A)の部分はスコープに入らない
3.times do |t|
  p defined?(v)          # (A)
  v = "Hello World #{t}" # ここ(宣言開始)から
  p v                    # ここ(ブロックの終わり)までが v のスコープ
end

# => nil
# "Hello World 0"
# nil
# "Hello World 1"
# nil
# "Hello World 2"
```

宣言は、実行されなかったとしても宣言とみなされます。

```ruby
v = "Hello World!" unless true  # 代入は行われないが宣言は有効
p defined?(v)                   # => "local-variable"
p v                             # => nil
```

## インスタンス変数

インスタンス変数は、クラスのインスタンス（オブジェクト）ごとに独立した値を保持するために使用されます。インスタンス変数の名前は、`@`で始まります。

```ruby
@name
@articles
```

例えば、GPT3.5-turboをAPIでコールするときには、以下のような記述をします。インスタンス化された時に、`@client`というインスタンス変数が定義されます。この例では、インスタンス変数に`OpenAI::Clientインスタンス`が格納され、`OpenAI::Client`のインスタンス関数である`chat`をコールしています。

```ruby
# frozen_string_literal: true

# [!!!COUTION]そのままコピペしても動きません。雰囲気を掴んでください
class OpenAIGptClient
  # newで自動的に呼ばれるオブジェクト初期化メソッド
  def initialize
    # インスタンス変数
    @client = OpenAI::Client.new(
      access_token: "Input access token",
      organization_id: "Input organization_id"
    )
  end

  # インスタンス関数
  def process
    @client.chat(
      {
        :parameters => {
          :model => "gpt-3.5-turbo",
          :messages => [],
          :max_tokens => 1024,
        }
      }
    )
  end
end

# 呼び出し方の例
@gpt_client = OpenAIGptClient.new
@gpt_client.process
```

例：オブジェクトごとに異なる設定を保持する

```ruby
class Circle
  def initialize(radius, color = 'red')
    @radius = radius
    @color = color
  end

  def area
    Math::PI * @radius * @radius
  end

  def describe
    puts "この円は#{@color}色で、面積は#{area}です。"
  end
end

circle1 = Circle.new(5)
circle1.describe  #=> "この円はred色で、面積は78.53981633974483です。"

circle2 = Circle.new(3, 'blue')
circle2.describe  #=> "この円はblue色で、面積は28.274333882308138です。"
```

## クラス変数

クラス変数は、クラス内のすべてのインスタンスで共有される変数です。そのため、クラス全体で状態を保持したり、情報を共有する際に役立ちます。

例：ユニークなIDをインスタンスに割り当てる

```ruby
# frozen_string_literal: true

class Item
  # クラス変数
  @@next_id = 1

  def initialize(name)
    @name = name # インスタンス変数
    @id = @@next_id # インスタンス変数@idに、クラス変数@@next_idを代入
    @@next_id += 1 # クラス変数をカウントアップ
  end

  def display_id
    puts "ID: #{@id}"
  end
end

item1 = Item.new("Item1")
item1.display_id  #=> "ID: 1"

item2 = Item.new("Item2")
item2.display_id #=> "ID: 2"
```

## グローバル変数

グローバル変数は、プログラム全体でアクセスできる変数で、$で始まる名前が付けられます。ただし、グローバル変数は使用を避けるべきです。なぜなら、コードの可読性と保守性を低下させ、デバッグが困難になるからです。代わりに、オブジェクト指向プログラミングの原則に従って、クラスやモジュールを使用することが推奨されます。

```ruby
$default_tax_rate = 0.1

def calculate_price(price)
  price * (1 + $default_tax_rate)
end

puts calculate_price(100)  #=> 110.0
```

## 定数

定数は、プログラムの実行中に変更されない値を保持するために使用されます。定数の名前は、すべて大文字で表記し、単語間にアンダースコアを使用します。

```ruby
class MathConstants
  PI = 3.141592653589793
  E = 2.718281828459045
end

def calculate_area_of_circle(radius)
  MathConstants::PI * radius * radius
end

puts calculate_area_of_circle(5)  #=> 78.53981633974483
```
