# インスタグラムで入手できる情報を整理

## Instagram_Graph_API  
**概要**  
https://developers.facebook.com/docs/instagram-api/?translation  
ビジネス・クリエイターアカウントのみ  

**できる**  
コメント情報の取得  
ハッシュタグ検索(人気上位と最近)  
他のユーザの画像・動画・アルバム(公開コンテンツのみ)  

**できない**  
位置情報

**他のアカウントについて**  
business_discoveryを用いて取得することができる  
https://developers.facebook.com/docs/instagram-api/guides/business-discovery/?translation  
フォロワー数、投稿数、メディア

**ハッシュタグ検索について**  
1., ハッシュタグidを検索  
```
/ig_hashtag_search?user_id=<user_id>&q=<query>
```
2., 取得したidを元に検索  

```
<hashtag_id>/top_media?user_id=<user_id>&fields=media_url,media_type,id,permalink,caption
```

**インサイトについて**  
※連携アカウントのみ有効  
※100人以上のフォロワーアカウントのみ有効  
https://developers.facebook.com/docs/instagram-api/guides/insights/?translation  
時系列に取れそう  
→過去2年間のある30日間の期間を取得可能  
メディア単位とユーザ単位  
期間も指定できる  
https://developers.facebook.com/docs/instagram-api/reference/ig-user/insights  
国、都市、年齢、性別、インプレッション、フォロワー  

## Instagram_基本_API  
**概要**
https://developers.facebook.com/docs/instagram-basic-display-api  
個人アカウントのみ  
**できる**  
プロフィール  
画像・動画・アルバム(認証後なので、非公開データも取得可能なはず)  

**できない**  
位置情報([ソース](https://www.instagram.com/developer/changelog/))  
投稿に対するいいね！数とコメントの数  
フォロワー数  
投稿されたコメントの内容  
ユーザーの写真と自己紹介文  

**他のユーザの取得について**
```
ユーザーが認証ウィンドウでアプリにこれらのアクセス許可を付与してからでなければ、アプリがユーザーのデータにアクセスすることはできません。

アプリが開発モードである間は、Instagramテスターアカウントのデータにのみアクセスできます。アプリをライブモードに切り替えてテスター以外のアカウントのデータにアクセスするには、その前にビジネス認証プロセスを完了する必要があります。

APIを使用するには、まず認証ウィンドウを取得し、それをアプリユーザーに対して表示します。アプリユーザーはそのウィンドウによって認証を受け、そのアプリに対して特定のアクセス許可を付与することにより、アプリがユーザーのデータにアクセスできるようにします。認証されたら、そのウィンドウから認証コードが指定されてアプリにリダイレクトされます。コードをキャプチャし、それを短期Instagramユーザーアクセストークンと交換します。短期トークンを入手したら、それを使用して、ユーザーがアプリにアクセスを許可したデータのUserエンドポイントとMediaエンドポイントに対してクエリを実行することができます。また、トークンを長期トークンに交換することもできます。
```
要約すると、アプリ利用の承認が必要かつ、利用先の承認が必要  


# アプリレビューについて
https://developers.facebook.com/docs/app-review#business-verification
