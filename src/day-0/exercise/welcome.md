# Exercise

いくつか題材を元に、Rubyの基礎を学びたいと思います。

- [じゃんけんプログラム](day-0/exercise/exercise-janken.md)
- [自動販売機プログラム](day-0/exercise/exercise-vending-machine.md)

各ページで書き方の解説をしていますが、別途、[Appendix](../appendix/welcome.md)にもまとめていますので、そちらも参照してください。

## 環境構築

[環境構築](../day-0/how-to-setup.md)がまだの方は、先にこちらを行ってください。

## 環境構築後の準備

Remote Containerを起動し、`/workspace`に移動します。

```bash
pwd
#=> /workspace
```

Ruby事前学習用に、`lib/training`ディレクトリを作成し、`janken.rb`と`vending_machine.rb`を作成します。

```bash
mkdir -p lib/training && touch lib/training/janken.rb && touch lib/training/vending_machine.rb
```

実行するには、以下のコマンドを実行します。

```bash
ruby lib/training/janken.rb
```

例えば、`janken.rb`の中身を以下のように書き換えてみてください。

Filename: `lib/training/janken.rb`

```ruby
puts "Hello World!"
```

実行すると、以下のように出力されます。

```bash
ruby lib/training/janken.rb 
#=> Hello World!
```

本講義資料は、サンプルコードベースを元に学習することを想定しているので、ご自身の環境で実行しながら学習を進めてください。