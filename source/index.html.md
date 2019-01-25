---
title: APIリファレンス

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://invitation.giftee.co/admin'>eGift invitation 管理画面</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  #- errors

search: true
---

# はじめに

このドキュメントは株式会社ギフティが提供する来店認証システム (eGift invitation)の利用方法を説明するものです。

# 来店認証システムとは

来店認証システムは店舗やイベント会場に来店/来場することでデジタルギフトがその場でもらえるO2Oキャンペーンツールです。
来店/来場した人にだけデジタルギフトが発行されるため景品の在庫リスクは発生せず、同時に来店/来場データの取得も可能です。

![overview](/images/egift-invitation-overview.png)

## 事前準備

- キャンペーンの設定
- 店舗の登録
- スタンプ利用設定
- 贈呈するギフトの設定

などを事前に行いキャンペーンが実施可能な状態にします。

## 1. キャンペーンエントリー
キャンペーンへのエントリー方法は下記の通りです。
エントリーページ(Webページ)からのエントリー
Web APIを使用したエントリー
キャンペーンへエントリーすることで参加者は招待状(ユニークなURL)を取得します。

## 2. 来店/来場
キャンペーンへエントリーした参加者に、エントリー時に取得した招待状を対象の店舗/会場へ持参してもらいます。対象店舗には事前準備にて用意したスタンプが設置してあります。

## 3. 店舗/会場にてアクティベーション
キャンペーン/施策に応じた条件を満たした参加者の持参した招待状へスタンプを押すことでアクティベーションをします。
各店舗ごとのアクティベーション実績は管理画面より確認することができます。
キャンペーン参加者は招待状をアクティベーションしてもらったことで、ギフト取得可能な状態になります。

## 4. ギフト贈呈
アクティベーション済みの招待状からギフト受け取りボタンを押下することで、キャンペーン参加者はギフトが取得できます。
ここでは事前準備の際に設定したギフトが発行されます。

# 用語定義

## キャンペーン

実施する施策各種設定を保持するリソース。
どのようなギフトを発行するか
いくつまでアクティベーション可能にするのか
どのようなユーザーフローにするのか
など各種ビジネスロジックを保持します。
2019年01月現在、ギフティスタッフが内部作成したキャンペーンのIDがAPI利用のために共有されます。

## エントリー
ユーザーがキャンペーンへ参加することを意味します。
エントリーが成功するとキャンペーンへ参加したユーザーへ招待状が付与されます。

## 招待状
アクティベーションすることでギフトが発行可能になるチケット(ユニークなURL)。
スタンプを画面に押下するか、パスコードを入力することでアクティベーションすることが可能になります。

## アクティベーション
店舗/会場等に設置したスタンプを招待状に押下する、もしくは招待状にパスコードを入力することで、招待状がギフト発行な状態になることを意味します。

## スタンプ
弊社が提供する電子スタンプを指します。
店舗や会場に事前に配置しておき、招待状画面へ押下することでアクティベーションすることができます。

## ギフト
実際の商品と引き換えることができるチケットを指します。

# 認証

Web APIを利用するためには認証する必要があります。認証にはHTTP Basic Authを使用します。

## 認証情報を取得する

### 1. eGift invitation 管理画面 アカウントを取得する
現在eGift invitation 管理画面のアカウントはギフティのスタッフのみ発行が可能になっています。
アカウントをまだお持ちでいない場合は、ギフティの担当スタッフへアカウント発行依頼をしてください。

### 2. eGift invitation 管理画面へログインする

![login](/images/login.png)

1で取得したアカウント情報で[eGift invitation 管理画面(本番環境)](https://invitation.giftee.co/admin)へアクセスいただき、ログインします。

<aside class="notice">
尚、検証環境のアカウントは本番環境とは異なりますので、必要な場合は別途担当スタッフまでお問い合わせください。
</aside>

### 3. Web API アクセストークンを発行する

ナビゲーションより`Web API`を選択し、アクセストークン発行画面に遷移します。
![access_token_](/images/access_token_1.png)

</br>

`新規発行`ボタンを選択すると、アクセストークンが新規発行されます。
![access_token_2](/images/access_token_2.png)

<aside class="warning">
こちらのモーダル内に記載されているアクセストークンを必ず控えてください。
</aside>

<aside class="warning">
この画面を閉じてしまった場合、このアクセストークンの情報を再度閲覧することができなくなります。
万が一、アクセストークンを控えずに画面を閉じてしまった場合は、再度アクセストークンを新規発行してください。
</aside>


## 認証方法

[HTTP Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication)を使用して行います。

`Authorization: Basic credentials`

<aside class="notice">
"credentials"には実際のトークンを入れてご利用ください。
</aside>

# リクエスト基本設定

## プロトコル
`HTTPS`を使用します。

## エンドポイント

### 検証環境

`invitation-stg.giftee.co`

### 本番環境

`invitation.giftee.co`

## Content-type

`application/json; charset=utf-8`

## リクエストパラメーター
### GETリクエストパラメーター

URIクエリを使用します。

> GETリクエストの場合は、URIクエリを使用します。

```shell
curl -H "Content-Type: application/json" -u "<credential>" -i
"https://invitation.giftee.co/api/invitations?max_id=xxxx-xxxx-xxxxx-xxxxxx"
```

### GET以外のリクエストパラメーター

リクエストボディを使用します。

> GET以外のリクエストの場合は、リクエストボディを使用します。

```shell
curl -X POST -H "Content-Type: application/json" \
-u "<credential>" \
-d '{"option": "sample"}' \
"https://invitation.giftee.co/api/campaign/sample/invitations"
```

# 招待状

## 参照

```shell
curl -u "<credential>" -i
"https://invitation.giftee.co/api/invitations/xxxx-xxxx-xxxxx-xxxxxx"
```

> レスポンス

```json
{
  "campaign_uid": "sample",
  "option": null,
  "url": "https://invitation.giftee.co/invitations/xxxx-xxxx-xxxxx-xxxxxx",
  "status": "inactive",
  "activated_at": null,
  "activation_type": null,
  "activated_shop_name": null,
  "created_at": "2019-01-01T10:00:00+09:00"
}
```

このエンドポイントを使用することで指定した招待状を取得することができます。

### HTTP Method
`GET`

### End Point

`https://invitation.giftee.co/api/invitations/:id`

### URL Parameter

属性 | 必須 | 詳細
--------- | ------- | -----------
id | true | 招待状を一意に特定する識別子です。

<aside class="notice">
idはURL(https://invitation.giftee.co/invitations/xxxx-xxxx-xxxxx-xxxxxx) のうち、xxxx-xxxx-xxxxx-xxxxxx部分を指します。
</aside>

## 一覧参照

このエンドポイントを使用することで指定したキャンペーンに属する招待状の一覧を取得することができます。発行日時が新しいものから降順で返却されます。

```shell
curl -u "<credential>" -i
"https://invitation.giftee.co/api/campaigns/sample/invitations?count=20&max_id=xxxx-xxxx-xxxxx-xxxxxx"
```

> レスポンス

```json
[
  {
    "campaign_uid": "sample",
    "option": null,
    "url": "https://invitation.giftee.co/invitations/xxxx-xxxx-xxxxx-xxxxxx",
    "status": "inactive",
    "activated_at": null,
    "activation_type": null,
    "activated_shop_name": null,
    "created_at": "2019-01-01T10:00:00+09:00"
  },
  {
    "campaign_uid": "sample",
    "option": null,
    "url": "https://invitation.giftee.co/invitations/yyyy-xxxx-xxxxx-xxxxxx",
    "status": "inactive",
    "activated_at": null,
    "activation_type": null,
    "activated_shop_name": null,
    "created_at": "2019-01-01T10:00:00+09:00"
  }
]
```

### HTTP Method
`GET`

### End Point

`https://invitation.giftee.co/api/campaigns/:id/invitations`

### URL Parameters

属性 | 必須 | 詳細
--------- | ------- | -----------
id | true | キャンペーンを一意に特定する識別子。

<aside class="notice">
URL Parameters id属性はキャンペーンIDです。こちらは事前準備にてギフティスタッフがキャンペーンを作成した後共有されます。
</aside>

### Query Parameters

属性 | 必須 | 詳細
--------- | ------- | -----------
count | false | 返却される招待状の数。(デフォルト値: 20)
max_id | false | 指定した招待状IDよりも前に発行されたもののみが返却されるようになります。
min_id | false | 指定した招待状IDよりもあとに発行された招待状のみが返却されるようになります。


## 発行

このエンドポイントを使用することで指定したキャンペーンの招待状を発行することができます。

```shell
curl -X POST -H "Content-Type: application/json" \
-u "<credential>" \
-d '{"campaign_uid": "sample"}' \
"https://invitation.giftee.co/api/campaigns/:id/invitations"
```

> レスポンス

```json
{
  "campaign_uid": "sample",
  "option": null,
  "url": "https://invitation.giftee.co/invitations/xxxx-xxxx-xxxxx-xxxxxx",
  "status": "inactive",
  "activated_at": null,
  "activation_type": null,
  "activated_shop_name": null,
  "created_at": "2019-01-01T10:00:00+09:00"
}
```

### HTTP Method
`POST`

### End Point

`https://invitation.giftee.co/api/campaigns/:id/invitations`

### URL Parameters

属性 | 必須 | 詳細
--------- | ------- | -----------
id | true | キャンペーンを一意に特定する識別子。

### Body Parameters

属性 | 必須 | 詳細
--------- | ------- | -----------
option | false | 関連情報をエントリーに関連付けて保存することができます。

<aside class="notice">
URL Parameters id属性はキャンペーンIDです。こちらは事前準備にてギフティスタッフがキャンペーンを作成した後共有されます。
</aside>

# 更新履歴

## 1.0.0 - 2019-01-25

- eGift invitation Web APIドキュメント バージョン1.0.0リリース
