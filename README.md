#Sana 紗凪
ShangriLa Anime API Server for Twitter Data

## Sana Server システム概要

### 説明

* アニメに関するTwitterのデータを返すREST形式のAPIサーバーのリファレンス実装です。
* ShangriLa Anime API のサブセットです。

### サーバーシステム要件

* Ruby 2.2+
* フレームワーク Sinatra

### インストール

#### DB
* MySQL、もしくはMySQL互換サーバーのインストール
* anime_admin_development データベース作成
* 必要なDDLの投入

#### sana batch

* 実際にTwitterのデータを取り扱うにはTwitterから定期的にデータを取得するバッチを実行する必要があります

#### API Server

```
bundle install
```

### 起動方法

```
bundle exec ruby sana.rb
```

## V1 API リファレンス

### エンドポイント

http://api.moemoe.tokyo/anime/v1

### 認証

V1では認証を行いません。


### レートリミット

なし

### GET /anime/v1/twitter/follwer/status

リクエストで指定されたアニメ公式アカウントの最新のフォロワー数を返却します

#### Request Parameter

| Property     | Value               |description|Sample|
| :------------ | :------------------ |:--------|:-------|
| accounts    |String|対象のTwitterアカウントをカンマ区切りにしたもの|usagi_anime,kinmosa_anime|


#### Response Body

| Property     | Value               |description|Sample|
| :------------ | :------------------ |:--------|:-------|
| Request twitter_account 1|follwer Object||"usagi_anime": {..}|
| Request twitter_account 2|follwer Object||"kinmosa_anime": {..}|
| Request twitter_account X|follwer Object||"lovelive_staff": {..}|

##### follwer Object

| Property     | Value               |description|Sample|
| :------------ | :------------------ |:--------|:-------|
| follwer    |Number|フォロワー数|12500|
| updated_at   |Number|データの更新日時 UNIX TIMESTAMP|1446132941|

#### レスポンス例

```
curl -v http://api.moemoe.tokyo/anime/v1/twitter/follwer/status?accounts=usagi_anime,kinmosa_anime,aldnoahzero | jq .

{
  "aldnoahzero": {
    "updated_at": 1432364949,
    "follower": 51055
  },
  "usagi_anime": {
    "updated_at": 1411466007,
    "follower": 51345
  },
  "kinmosa_anime": {
    "updated_at": 1432364961,
    "follower": 57350
  }
}
```



### GET /anime/v1/twitter/follwer/history

リクエストで指定されたアニメ公式アカウントのフォロワー数の履歴を返却します。（最大100履歴）

#### Request Parameter


| Property     |Value |Required|description|Sample|
| :------------|:-----|:-------|:----------|:-----|
| account    |String|◯|対象のTwitterアカウント|usagi_anime|
| end_date |Number|-|unixtimestampで指定した日時より過去のデータを取得。指定がない場合は現在日時。 where end_date > updated_at |1446132941|


#### Response Body

| Property     | Value               |description|Sample|
| :------------ | :------------------ |:--------|:-------|
| Array Object|history Object Array|取得できない場合は空の配列|[]|


##### history Object

| Property     | Value               |description|Sample|
| :------------ | :------------------ |:--------|:-------|
| follwer    |Number|フォロワー数|12500|
| updated_at   |Number|データの更新日時 UNIXTIMESTAMP|1446132941|

データは更新日時の昇順でソートされ格納されています。

#### レスポンス例

```
curl -v http://api.moemoe.tokyo/anime/v1/twitter/follwer/history?account=usagi_anime
```