# Exercise - 自動販売機プログラム

## 自動販売機プログラムの仕様

- 飲み物を自由に設定できる
- 支払い方法は、現金と電子マネーを設定できる

## 完成形

Filename: lib/training/vending_machine.rb

```ruby
# frozen_string_literal: true

module VendingMachine
  class VendingMachine
    attr_accessor :drinks, :payments, :inventories, :person

    def initialize(person:)
      @person = person
      @drinks = Drink.initialize_drinks
      @payments = Payment.initialize_payments
      @inventories = Hash[*@drinks.map do |drink|
        [drink.key.to_sym, drink.inventory]
      end.flatten]
    end

    def buy!
      @person.display
      drink = choose_drink(@person)
      payment = choose_payment(@person)
      payment.transact(@person, drink)
      manage_invetory!(drink)
    end

    def choose_drink(person)
      display_drinks
      drink_inputed = gets
      selected_drink = select_drink(drink_inputed.to_i)
      selected_drink.display(person.name)
      selected_drink
    end

    def display_drinks
      puts "飲み物を番号で選んでください。\n#{format_drinks}"
    end

    def format_drinks
      @drinks.map do |drink|
        "#{drink.id}: #{drink.name}"
      end
    end

    def select_drink(id)
      selected_drink = select_drink_by_id(id)
      return selected_drink unless selected_drink.nil?

      puts "飲み物が見つかりませんでした。終了します"
      exit
    end

    def select_drink_by_id(id)
      @drinks.find { |drink| drink.id == id }
    end

    def choose_payment(person)
      display_payments
      payment_inputed = gets
      selected_payment = select_payment(payment_inputed.to_i)
      selected_payment.display(person.name)
      selected_payment
    end

    def select_payment(id)
      selected_payment = select_payment_by_id(id)
      return selected_payment unless selected_payment.nil?

      puts "支払い方法が見つかりませんでした。終了します"
      exit
    end

    def select_payment_by_id(id)
      @payments.find { |payment| payment.id == id }
    end

    def display_payments
      puts "支払い方法を番号で選んでください。\n#{format_payments}"
    end

    def format_payments
      @payments.map do |payment|
        "#{payment.id}: #{payment.name}"
      end
    end

    def manage_invetory!(drink)
      target_drink_inventory = @inventories[drink.key.to_sym]
      rest_of_inventory = target_drink_inventory - 1
      @inventories[drink.key.to_sym] = rest_of_inventory
      puts "#{drink.name}の在庫：#{rest_of_inventory}個"
      puts "全体の在庫：#{@inventories}"
    end
  end

  class Drink
    attr_accessor :id, :key, :name, :price, :temperature_type, :inventory

    def initialize(id, key, name, price, temperature_type, inventory)
      @id = id
      @key = key
      @name = name
      @price = price
      @temperature_type = temperature_type
      @inventory = inventory
    end

    def self.initialize_drinks
      [
        new(1, "cola", "Cola", 100, "cold", 10),
        new(2, "tea", "お〜いお茶", 150, "hot", 5),
        new(3, "pepsi", "ペプシ", 100, "cold", 7),
        new(4, "oshiruko", "おしるこ", 80, "hot", 9),
        new(5, "water", "水", 130, "cold", 12)
      ]
    end

    def display(person_name)
      puts "#{person_name}が選んだ飲み物は：#{@name}, #{@price}円"
    end
  end

  class Person
    attr_accessor :name, :cash, :e_cash

    def initialize(name = "Noname", cash = 0, e_cash = 0)
      @name = name
      @cash = cash
      @e_cash = e_cash
    end

    def display
      puts "購入者：#{@name}, 現金：#{@cash}円, 電子マネー：#{@e_cash}円"
    end
  end

  class Payment
    TXT_CASH = "cash"
    TXT_E_CASH = "e_cash"

    attr_accessor :id, :name, :payment_type

    def initialize(id, name, payment_type)
      @id = id
      @name = name
      @payment_type = payment_type
    end

    def self.initialize_payments
      [
        new(1, "現金", TXT_CASH),
        new(2, "電子マネー", TXT_E_CASH)
      ]
    end

    def display(person_name)
      puts "#{person_name}が選んだ支払い方法は：#{@name}"
    end

    def transact(person, drink)
      person_money = get_person_money(person)
      drink_price = drink.price

      if can_buy?(person_money, drink_price, drink.inventory)
        change = charge!(person_money, drink_price)
        puts "購入者の#{@name}残高：#{change}円"
      else
        puts "#{@name}もしくは在庫が不足しています。プログラムを終了します"
        exit
      end
    end

    def get_person_money(person)
      case @payment_type
      when TXT_CASH
        person.cash
      when TXT_E_CASH
        person.e_cash
      end
    end

    def can_buy?(person_money, drink_price, drink_inventory)
      person_money >= drink_price && drink_inventory.positive?
    end

    def charge!(person_money, drink_price)
      person_money - drink_price
    end
  end
end

# 呼び出し側
person = VendingMachine::Person.new("Taro", 10, 2000)
vending_machine = VendingMachine::VendingMachine.new(person: person)
vending_machine.buy!
#=> 購入者：Taro, 現金：10円, 電子マネー：2000円
# 飲み物を番号で選んでください。
# ["1: Cola", "2: お〜いお茶", "3: ペプシ", "4: おしるこ", "5: 水"]
# 1
# Taroが選んだ飲み物は：Cola, 100円
# 支払い方法を番号で選んでください。
# ["1: 現金", "2: 電子マネー"]
# 2
# Taroが選んだ支払い方法は：電子マネー
# 購入可能です
# Colaの在庫：9個
# 全体の在庫：{:cola=>9, :tea=>5, :pepsi=>7, :oshiruko=>9, :water=>12}
```

キーワード

> 

## 自動販売機プログラムで使うRubyが定義するメソッド（忘れても良い）

### gets

getsメソッドは、標準入力から文字列を取得するメソッドです。

※標準入力とは、キーボードからの入力や、ファイルからの入力など、プログラムに入力を与えることができるものを指します。

```ruby
drink_inputed = gets

#=> 飲み物を番号で選んでください。
# ["1: Cola", "2: お〜いお茶", "3: ペプシ", "4: おしるこ", "5: 水"]
#
```

1が入力された場合、`drink_inputed`には`1\n`が代入されます。

## Rubyのreturn

プログラミング言語を学ぶと、必ず出てくるのがreturnです。Rubyにおいてもreturnは存在しますが、returnを書かなくてもメソッドの最後に評価した値が返り値となります。

※明示的にreturnを書くこともありますが、基本的には書かなくても良いです。

```ruby
def format_payments
  @payments.map do |payment|
    "#{payment.id}: #{payment.name}"
  end
end
```

明示的にreturnを書く場合は、以下のように書きます。

```ruby
def format_payments
  return @payments.map do |payment|
    "#{payment.id}: #{payment.name}"
  end
end
```

ただ多くの場合は、returnを書かない方がRubyらしい書き方となります。

### 早期リターン

早期リターンとは、メソッドの途中でreturnを書くことです。以下のように、メソッドの途中でreturnを書くことで、メソッドの処理を途中で終了させることができます。いくつかの書き換え方がありますが、どれも同義で、主に可読性を考慮して書き換えます。

```ruby
def print_message(user)
  return puts "ユーザーが存在しません" if user.nil?

  puts "ユーザー名：#{user.name}"
end

# 1. 上記の書き換え
# def print_message(user)
#   if user.nil?
#     puts "ユーザーが存在しません"
#     return
#   end

#   puts "ユーザー名：#{user.name}"
# end

# 2. 上記の書き換え
# def print_message(user)
#   if user.nil?
#     puts "ユーザーが存在しません"
#   else
#     puts "ユーザー名：#{user.name}"
#   end
# end
```

```ruby
def select_drink(id)
  selected_drink = select_drink_by_id(id)
  return selected_drink unless selected_drink.nil?

  puts "飲み物が見つかりませんでした。終了します"
  exit
end
```

## クラスメソッド

Rubyにおけるクラスメソッドは、特定のクラスに関連付けられたメソッドのことを指します。クラスメソッドはそのクラスのインスタンスではなく、クラス自体に対して呼び出します。これらは、通常、そのクラス全体に関連する動作を定義するために使用されます。

クラスメソッドは、メソッド名の前に「self.」を付けることで定義します。ここでの「self」は、そのメソッドが定義されているコンテキスト（この場合はクラス）を参照します。

```ruby
class Drink
  # クラスメソッド
  def self.initialize_drinks
    [
      new(1, "cola", "Cola", 100, "cold", 10),
      new(2, "tea", "お〜いお茶", 150, "hot", 5),
      new(3, "pepsi", "ペプシ", 100, "cold", 7),
      new(4, "oshiruko", "おしるこ", 80, "hot", 9),
      new(5, "water", "水", 130, "cold", 12)
    ]
  end

  # 上記は以下のように書き換えることもできます。
  # class << self
  #   [
  #     new(1, "cola", "Cola", 100, "cold", 10),
  #     new(2, "tea", "お〜いお茶", 150, "hot", 5),
  #     new(3, "pepsi", "ペプシ", 100, "cold", 7),
  #     new(4, "oshiruko", "おしるこ", 80, "hot", 9),
  #     new(5, "water", "水", 130, "cold", 12)
  #   ]
  # end

  # インスタンスメソッド
  def display(person_name)
    puts "#{person_name}が選んだ飲み物は：#{@name}, #{@price}円"
  end
end
```

それぞれのメソッドを呼び出す場合は、以下のように呼び出します。

```ruby
# クラスメソッドの呼び出し
# クラスメソッドは、クラス名.メソッド名で呼び出します。クラスから直接呼び出すことができます。
Drink.initialize_drinks
#=> ドリンク情報が初期化される

# インスタンスメソッドの呼び出し
# インスタンスメソッドは、インスタンスを生成してから呼び出します。
drink = Drink.new(1, "cola", "Cola", 100, "cold", 10)
drink.display("たろう")
#=> たろうが選んだ飲み物は：Cola, 100円
```

## 関数の引数

### デフォルト引数

ソッド定義の中で引数にデフォルトの値を設定することです。これにより、メソッド呼び出し時にその引数を省略した場合、デフォルトの値が自動的に使用されます。

```ruby
def greet(name = "world")
  puts "Hello, #{name}!"
end

greet("Ruby")  # => "Hello, Ruby!"
greet          # => "Hello, world!"
```

### 名前付き引数（またはキーワード引数）

メソッド呼び出し時に引数名（キー）とその値を指定することです。これにより、引数の順序を気にせずにメソッドを呼び出すことができます。また、メソッド定義の可読性も向上します。

```ruby
def person_info(name:, age:, city:)
  puts "#{name} is #{age} years old and lives in #{city}."
end

person_info(name: "Alice", age: 25, city: "Tokyo")
# => "Alice is 25 years old and lives in Tokyo."
```

## メソッド群

自動販売機プログラムで使用されるメソッドについていくつか紹介します。

### map と each

よく使うメソッドとして、`map`と`each`があります。どちらも配列の要素を順番に取り出して、ブロックの処理を実行します。違いは、`map`はブロックの戻り値を配列にして返すことです。`each`はブロックの戻り値を返しません。

```ruby
array = [1, 2, 3]

array.each do |num|
  num * 2
end

#=> [1, 2, 3]

array.map do |num|
  num * 2
end

#=> [2, 4, 6]
```

基本的に`each`は単純な繰り返し処理、`map`は繰り返し処理を行いながら配列を作成したい場合に使います。

### find と select

findは、ブロックの戻り値が真になった最初の要素を返します。selectは、ブロックの戻り値が真になった要素を全て返します。

```ruby
array = [1, 2, 3, 4, 5]

array.find do |num|
  num.even?
end

#=> 2

array.select do |num|
  num.even?
end

#=> [2, 4]
```

### flatten

flattenは、多次元配列を一次元配列にします。

```ruby
array = [[1, 2], [3, 4], [5, 6]]

array.flatten
#=> [1, 2, 3, 4, 5, 6]
```

## 「?」と「!」のつくメソッドのつくり方

Rubyでは、メソッド名の最後に「?」や「!」をつけることができます。これらのメソッドは、Rubyの慣習として、以下のような意味を持ちます。

### ?をつけるメソッド
メソッド名の最後に「?」がついていると、そのメソッドは真偽値（真または偽）を返すことを示しています。これは、ある条件が真であるか偽であるかを問い合わせるメソッドであることを示します。たとえば、RubyのArrayクラスのempty?メソッドは、配列が空であるかどうかを確認します。

```ruby
array = []
puts array.empty?
# => true
```

### !をつけるメソッド
メソッド名の最後に「!」がついていると、そのメソッドが何らかの形で「危険」な動作を行うことを示しています。たとえば、オブジェクトを変更する（つまり、副作用を持つ）メソッドや、エラーを引き起こす可能性があるメソッドなどです。この記号は、メソッドが通常の動作とは異なる何らかの振る舞いをすることを示しています。

```ruby
str = "Hello, world!"
str.gsub!('world', 'Ruby')
puts str  # => "Hello, Ruby!"
```