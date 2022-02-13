# Ruby Memo

## [settingslogic](https://github.com/settingslogic/settingslogic)

初心者向けと聞いて。読んでいく  

```yaml
# test.yaml
hoge:
  fuga: piyo
```

```ruby
require 'settingslogic'

class MyClass < Settingslogic
  source "#{File.dirname(__FILE__)}/test.yaml"
end

my_set = MyClass.new
p my_set.hoge.fuga # => piyo
```

`yaml`でいいけどそういうことじゃないので読む。

 - 最後のcommit 2014年で不安になってきた
 - `lib/settingslogic.rb`1ファイル構成でシンプル
 - `Hash`継承
 - `StandardError`継承してる`MissingSetting`
    - 独自エラークラスっぽい
 - `class << self`...なんだっけこれ特異クラス？
    - https://magazine.rubyist.net/articles/0046/0046-SingletonClassForBeginners.html#class--self-って
    - > これはクラスメソッドを定義するイディオムのひとつ
 - `initialize`をまず読む
    - `new`を引数なしで呼ぶと`hash_or_file`と`section`にデフォルト値が入る
      - `section`は`nil`
      - `hash_or_file`には`self.class.source`
        - `MyClass`の親クラスだから`Settingslogic.source`が呼ばれる
        - `self.source`じゃダメなん？
          - だめだった
          ```ruby
          class One
            def self.source
              p 'source'
            end
          end

          class Two < One; end

          t = Two.new
          t.source # => error
          ```
          - 継承ではクラスメソッドは実装されないらしい
          - じゃあどうやってやるかはパッと調べてわからなかったのでTODOとする
