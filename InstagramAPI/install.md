# インスタグラム調査メモ

**参考になったサイト**
https://arrown-blog.com/instagram-graph-api/#Instagram_Graph_API
https://irokoto.co.jp/blog/20200606/post-11


**前提条件**
```
Facebookページを取得
Instaアカウントを取得
FacebookページとInstaアカウントを連携
```

**1., FacebookページでAppを作りInstagram APIをプロダクトに登録する**
既に作成されている場合は管理者や開発者に含めてもらう  
![サンプル](https://user-images.githubusercontent.com/18533877/105624230-70ab9b80-5e63-11eb-880c-69abc4e4142c.png)

登録すると、左のメニューバーに表示される  
![サンプル](https://user-images.githubusercontent.com/18533877/105624262-d3049c00-5e63-11eb-8b9b-7af6b7235cbc.png)

アプリのIDとApp Secretが必要になるので設定ページからメモしておく  
![図1](https://user-images.githubusercontent.com/18533877/105624236-84570200-5e63-11eb-86ca-d2c3b97bbda2.png)


**2., トークンを取得する**
ここからが本題、3段階+αのトークンを取得していく  

***1段階目***
グラフAPIのページで、アクセス許可に関するトークンを取得する  
Facebookアプリ -> 今回開発するアプリ名  
ユーザーまたはページ -> ユーザートークン  
アクセス許可 -> 下の許可を追加より以下を選択する  
```
pages_show_list,
business_management,
instagram_basic,
instagram_manage_comments,
instagram_manage_insights,
pages_read_engagement,
pages_manage_posts,
public_profile
```
選択したら`Generate Access Token`を押すことで上のText boxに1段階目トークンが出現するのでメモる  
![サンプル](https://user-images.githubusercontent.com/18533877/105623354-6a65f100-5e5c-11eb-9c55-18359b5a4c8a.png)


***2段階目***
グラフAPIに以下を投げる
```
oauth/access_token?grant_type=fb_exchange_token&client_id=<アプリID>&client_secret=<App Secret>&fb_exchange_token=<1段階目トークン>
```
上手くいくとJSONレスポンスが返却されるので2段階目トークンをメモる
```
{
  "access_token": "2段階目トークン",
  "token_type": "bearer",
  "expires_in": 5184000
}

```

***3段階目***
2段階目トークンを用いて、FacebookIDをグラフAPIから取得する  
```
me?access_token=<2段階目トークン>
```
上手くいくとJSONレスポンスが返却されるのでFacebookIDをメモる  
```
{
  "name": "自身の名前",
  "id": "FacebookID"
}
```
FacebookIDと2段階目トークンを用いて3段階目トークンをグラフAPIから取得する
```
<FacebookID>/accounts?access_token=<2段階目トークン>
```
上手くいくとJSONレスポンスが返却される
```
{
  "data": [
    {
      "access_token": "3段階目トークン",
      ---他ページ情報
    }
    ]
}
```

**3., Instagram情報を取得**
3段階目トークンをグラフAPIのアクセストークン欄に入力し、InstagramIDをグラフAPIから取得する
```
me?fields=instagram_business_account
```
上手くいくとJSONレスポンスが返却される
```
{
  "instagram_business_account": {
    "id": "InstagramID"
  },
  "id": ""
}
```
InstagramID, 3段階目アクセストークン, Instagramアカウント名を用いてデータを取得する
```
<InstagramID>?fields=business_discovery.username(<Instagramaアカウント名>){id,followers_count,media_count,ig_id,media{caption,media_url,media_type,like_count,comments_count,timestamp,id}}&access_token=<3段階目アクセストークン>
```

# トークンの永続化
トークンデバッガーページより2段階目トークンを入力  
https://developers.facebook.com/tools/debug/accesstoken/  
下の方に`アクセストークンを延長`ボタンがあるのでクリック　　
もう一度、グラフAPIのページに戻り、2段階目トークンをグラフAPIのアクセストークン欄に入力し以下をAPIに投げる
```
me/accounts
```
上手くいくとJSONレスポンスが返却される
```
{
  "data": [
    {
      "access_token": "3段階目永続化トークン",
      ---他ページ情報
    }
    ]
}
```
