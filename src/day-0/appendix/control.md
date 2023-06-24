# 制御構文

## 条件分岐

### if

```ruby
if sum >= 100
  print "合計が100を超えている"
else
  print "合計が100を超えていない"
end
```

Rubyでは、`if`、`elsif`、`else`を使う

```ruby
if sum >= 100
  print "合計が100を超えている"
elsif sum >= 200
  print "合計が200を超えている"
else
  print "合計が100を超えていない"
end
```

if 修飾子。こんな書き方もできます。

```ruby
print "合計が100を超えている" if sum >= 100
```

### unless

```ruby
unless sum >= 100
  print "合計が100を超えている"
else
  print "合計が100を超えていない"
end
```

unless 修飾子。こんな書き方もできます。

```ruby
print "合計が99以下" unless sum >= 100
```

### case

case は一つの式に対する一致判定による分岐を行います。when 節で指定された値と最初の式を評価した結果とを演算子 === を用いて比較して、一致する場合には when 節の本体を評価します。

例1

```ruby
case $age
when 0 .. 2
  "baby"
when 3 .. 6
  "little child"
when 7 .. 12
  "child"
when 13 .. 18
  "youth"
else
  "adult"
end
```

`if`を使って書き直すと以下になります。

```ruby
if $age >= 0 && $age <= 2
  "baby"
elsif $age >= 3 && $age <= 6
  "little child"
elsif $age >= 7 && $age <= 12
  "child"
elsif $age >= 13 && $age <= 18
  "youth"
else
  "adult"
end
```

例2

```ruby
case $status
when "opened", "accepting"
  "Can apply"
when "closed", "suspended"
  "Cannot apply"
else
  "Not permitted"
end
```

`if`を使って書き直すと以下になります。

```ruby
if $status === "opened" || $status === "accepting"
  "Can apply"
elsif $status === "closed" || $status === "suspended"
  "Cannot apply"
else
  "Not permitted"
end
```