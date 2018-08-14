# Language Update 2018 - Ruby

author
:   Kazuhiro NISHIYAMA

content-source
:   Learn Languages 2018

date
:   2018/08/26

allotted-time
:   10m

theme
:   lightning-simple

## LLoT 以降のリリースなど

- 2016-08-27 LLoT (前回の Language Update)
- 2016-12-24 2.4.0
- 2017-03-31 2.1 EOL
- 2017-12-25 2.5.0
- 2018-03-31 2.2 EOL

<https://bugs.ruby-lang.org/projects/ruby/wiki/ReleaseEngineering>

## 2.4.0 での変更点

- 速くなりました
- 機能追加されました

といった話はいつものことなので省略

## 2.4.0 での大きな非互換

- Unifying Fixnum and Bignum into Integer
- 詳細は RubyKaigi 2016 の発表を参照
  <http://rubykaigi.org/2016/presentations/tanaka_akr.html>

## Fixnum, Bignum とは?

- 実装の詳細
- Fixnum: 32bit 環境なら 31bit 以下の整数が効率よく扱える
- Bignum: 大きな整数もメモリーの許す限り扱える
- 普通のユーザーは区別する必要がないので Integer に統合

## 影響

- 利点: シンプルになる
  - 教える人にとっても勉強する人によっても良い
- 欠点: 非互換
  - 特に拡張ライブラリーに影響

## 非互換の例

- Sequel の DSL で `add_column :column, Bignum` → `:Bignum`
- 影響のあった拡張ライブラリ : オブジェクトをダンプ/ロードするようなものがほとんどだった
  - ext/json, msgpack, syck, yajl, oj, ox, ruby-gnome2, etc.

## Version Dependencies

- json に対する pessimistic (悲観的な) version dependency `(~> 1.3)`
  - 1.3 以上 2.0 未満という意味
  - 間接的にバージョン制限が入っていることが多かった
- 問題点
  - json 1.x の最新 (当時) 1.8.3 は ruby 2.4 非対応
  - json 2.0.x がバージョン制限で入らない

## 解決

- stdlib なライブラリーはバージョン依存をつけないのが推奨
  <https://www.hsbt.org/diary/20160829.html>
- 結局 json 1.8.5 がリリースされて解決
  <https://www.hsbt.org/diary/20170112.html>
