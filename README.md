# Jipcode

Jipcodeは郵便番号から住所を検索する機能を提供します。
郵便番号と対応する住所のデータは日本郵便の公式サイトで配布されているものを用いています。

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'jipcode'
```

And then execute:

```shell
$ bundle install
```

Or install it yourself as:

```shell
$ gem install jipcode
```

## Usage

### 検索
郵便番号は1個の番号につき複数の住所が結びつくことがあります。
そのため次のように検索結果は住所情報を含むHashの配列で返ります。
このHashは郵便番号(`:zipcode`)、都道府県(`:prefecture`)、市区町村(`:city`)、町域番地(`:town`)の値を持ちます。

```ruby
Jipcode.locate('1510051')
# => [{zipcode: '1510051', prefecture: '東京都', city: '渋谷区', town: '千駄ヶ谷'}]
```

#### 都道府県コード
次のようにオプションで都道府県コードを検索結果に含めることもできます。

```ruby
Jipcode.locate('1510051', prefecture_code: true)
# => [{:zipcode=>"1510051", :prefecture=>"東京都", :city=>"渋谷区", :town=>"千駄ヶ谷", :prefecture_code=>13}]
```

#### 全角カナ

加えてオプションで、都道府県カナ、市区町村カナ、町域番地カナを検索結果に含めることもできます。

```ruby
Jipcode.locate('1510051', prefecture_code: true, kana: true)
=begin
=>
[{:zipcode=>"1510051",
  :prefecture=>"東京都",
  :city=>"渋谷区",
  :town=>"千駄ヶ谷",
  :prefecture_code=>13,
  :prefecture_kana=>"トウキョウト",
  :city_kana=>"シブヤク",
  :town_kana=>"センダガヤ"}]
=end
```

例外として、大口事業所個別番号は番地のカナが`:town_kana`に含まれず、町域までとなります。

```ruby
Jipcode.locate('0608614', prefecture_code: true, kana: true)
=begin
[{:zipcode=>"0608614",
  :prefecture=>"北海道",
  :city=>"札幌市中央区",
  :town=>"大通西５丁目地下鉄大通駅西側コンコース内",
  :prefecture_code=>1,
  :prefecture_kana=>"ホッカイドウ",
  :city_kana=>"サッポロシチュウオウク",
  :town_kana=>"オオドオリニシ"}]
=end
```

### 更新
日本郵便の郵便番号データは月末に更新されています。
jipcodeではこれを毎月取り込んでいます。

更新を反映したい時は`bundle update jipcode`してください。

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the Jipcode project’s codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/rinkei/jipcode/blob/master/CODE_OF_CONDUCT.md).
