### Ginza.rb 第80回 はっぴょうかい☆
- - -

### 1行 in の有効的な使いみち

---

#### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* Rails 歴 約 2年
* 趣味で Ruby にパッチを投げたりしてます
  * Ruby 2.7 に [Time#floor](https://bugs.ruby-lang.org/issues/15653) / [Time#ceil](https://bugs.ruby-lang.org/issues/15772) を追加したり

---

### 今日話すこと
### Ruby 2.7 で追加された
### 1行 in について

---

#### 1行 in とは
- - -

* Ruby 2.7 で実験的にパターンマッチが実装された       <!-- .element: class="fragment" -->
* 1行 in はパターンマッチを1行で書くための構文       <!-- .element: class="fragment" -->
  * マッチに失敗したら例外 NoMatchingPatternError が発生する
* 今回はパターンマッチはわからなくても OK       <!-- .element: class="fragment" -->
* [Ruby2.7の(実験的)新機能「パターンマッチ」で遊ぶ - メドピア開発者ブログ](https://tech.medpeer.co.jp/entry/2019/05/13/090000)           <!-- .element: class="fragment" -->

↓ ↓ ↓

>>>

* パターンマッチは case に渡した値を in の条件で判定する構文

```ruby
user = { id: 1, name: "homu", age: 14 }
case user
in { name: /^h/, age: (..20) }   # age が 20 以下
  p "マッチした！"
end
```

* 条件が 1つの場合に1行で書くことができる    <!-- .element: class="fragment" -->

```ruby
user = { id: 1, name: "homu", age: 14 }
user in { name: /^h/, age: (..20) }
p "マッチした！"
```
<!-- .element: class="fragment" -->

* マッチに失敗したら例外 NoMatchingPatternError    <!-- .element: class="fragment" -->


---

## 今日は
## 1行 in の有効的な使い方
## について話します

---

* パターンマッチでは任意のキーの要素をキャプチャできる

```ruby
user = { id: 1, name: "homu", age: 14 }

case user
in { name:, age: (..20) }
  # name の値が name という名前の変数に代入される
  # その変数を参照することができる
  p name    # => "homu"
end
```

↓↓↓

>>>

* これは 1行 in でも可能

```ruby
user = { id: 1, name: "homu", age: 14 }

# name の値が name という名前の変数に代入される
user in { name:, age: (..20) }

# その変数を参照できる
p name     # => "homu"
```

↓↓↓

>>>

* そもそもパターンを書かずに変数にキャプチャだけできる

```ruby
user = { name: "homu", age: 14 }

# name と age の値がそれぞれ同名の変数に代入できる
user in { name:, age: }   # 条件は書かない
p name     # => "homu"
p age      # => 14
```

## これって JavaScript にあるような分割代入では…？🤔         <!-- .element: class="fragment" -->

---

# そもそも

---

* in には変数名だけを渡すこともできる        <!-- .element: class="fragment" -->

```ruby
# user の値を変数 user2 に代入
user in user2
user2  # => user
```
<!-- .element: class="fragment" -->


* これを利用すると…     <!-- .element: class="fragment" -->
* メソッドチェーンの最後に代入式を書いたりできる！！     <!-- .element: class="fragment" -->

```ruby
(1..10).to_a.shuffle.select { _1.even? }.map { _1 * 2 }.map { _1.to_s } in result
p result
# => ["20", "16", "12", "4", "8"]
```
<!-- .element: class="fragment" -->

---

# 代入演算子では？？？？

---

## 実は 1行 in は
## 左辺値を右辺に代入する     <!-- .element: class="fragment" -->
## 新しい代入演算子だったんだよ！！！！     <!-- .element: class="fragment" -->
## ΩΩΩ<な、なんだってー!?      <!-- .element: class="fragment" -->

---

#### まとめ
- - -

* 1行 in は代入演算子            <!-- .element: class="fragment" -->
* = 演算子とは違い分割代入ができる          <!-- .element: class="fragment" -->
* まだ実験的な機能なので今後仕様が変わる可能性があるので注意          <!-- .element: class="fragment" -->
* パターンマッチについて知りたい人はここを読んでね           <!-- .element: class="fragment" -->
  * [Ruby2.7の(実験的)新機能「パターンマッチ」で遊ぶ - メドピア開発者ブログ](https://tech.medpeer.co.jp/entry/2019/05/13/090000)

---

#### おまけ
- - -

* ActiveRecord でパターンマッチできる gem を作った
* [activerecord-pattern_match](https://github.com/osyo-manga/activerecord-pattern_match)

```ruby
user = User.create name: "homu", age: 14

# 分割代入できる
user in { id:, name:, age:, created_at: }
p id, name, age, created_at
# => 1
#    "homu"
#    14
#    2020-01-25 06:03:00.368396 UTC
```


---

## ご清聴
## ありがとうございました
