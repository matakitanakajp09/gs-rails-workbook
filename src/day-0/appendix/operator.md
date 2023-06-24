# 演算子式

## 代入

```ruby
article = Article.find(1)
articles[0] = 1
article.key = "value"
```

## 自己代入

```ruby
count += 12 # count = count + 12
a ||= 1 #  a が偽か未定義ならば1を代入。初期化時のイディオムの一種。
```

## 多重代入

```ruby
article, tag, keyword = Article.find(1), Tag.last, Keyword.first
sum, average = 0, 0
```

## 範囲式

```ruby
1..20 # 1<=n<=20
1...20 # 1<=n<20
Article.where(started_at: Date.new(2023,4,1)..Date.new(2023,4,30)) # 2023/04/01<=n<=2023/04/30
```

## and

```ruby
tokyo && kyoto
tokyo and kyoto
```

左辺を評価し、結果が偽であった場合はその値(つまり nil か false) を返します。左辺の評価結果が真であった場合には右辺を評価しその結果を返します。

```ruby
nil && false # => nil
false && nil # => false
1 && 2 # => 2
```


## or

```ruby
deploy || die
deploy or die
```

左辺を評価し、結果が真であった場合にはその値を返します。左辺の評価結果が偽であった場合には右辺を評価しその評価結果を返します。

```ruby
1 || 2 # => 1
nil || false # => false
false || nil # => false
```

## not

```ruby
!false
not false
```

## 条件演算子

```ruby
sum == 100 ? "合計が正しい" : "合計が正しくない"
# 同義: if sum == 100 then "合計が正しい" else "合計が正しくない" end
```