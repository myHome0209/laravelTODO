# Laravel開発メモ

#### memo
- FontAwesome install  
Project直下のpackage.jsonへ下記記載  
```
    "dependencies": { 
            "font-awesome": "^4.7.0" 
    }
```
記載後に  
```
npm install
```  
resources/sass/app.scssへ  
```
// Font Awesome  
@import '~font-awesome/scss/font-awesome';
```

- sortable  
```
composer require rutorika/sortable
```

- login機能の実装  
- URLインスタンス→現在のURLに関する情報へアクセスできる  
`app/Providers/AppServiceProvider.php`へ  
```
use Illuminate\Routing\UrlGenerator
～
public function boot(UrlGenerator $url){
    $url->forceScheme('https');
}

```
- 認証機能のコマンド
```
php artisan make:auth
```
> `view already exists. Do you want to replace it? (yes/no) [no]:`  
> 上記出た場合はno　(view/layouts/app.blade.phpが既に作成されている場合)

- Userテーブルの作成
```
php artisan migrate
```

- headerにログイン/ログアウト用のリンクを追加  
> ログイン中の場合の処理(Laravel7まで)
```
@auth
～
@endauth
```
> ログイン中の場合の処理(Laravel7以降)
```
@auth
～
@endif
```
#### csrfの仕組み  
```
・クロスサイトリクエストフォージェリの略

・認証済みのユーザになりすまして不正コマンドを実行する悪意のある攻撃

・ざっくりいえば、攻撃者が用意したトラップサイトを踏ませ、他人のブラウザのcookieに書かれているセッションIDを取得し、
　そのIDを使用して特定のWebアプリへアクセスすることで、元のセッションデータの持ち主がリクエストしたように偽装する事。
　
・対策の一例、
    1.フォーム入力画面を作成時にPHPの暗号を生成して、その結果をsessionへセット
    2.sessionへセットしたデータはhtmlの<form></form>内にtokenという名前でセットし、他の入力データと一緒に送信
    3.入力画面から送信されてきたデータ内のtokenという名前が入っているデータを取り出し、それと入力画面作成時にsessionへセットしたデータを比較
    4.送信されたtokenと入力画面作成時にsessionへセットしたデータが同じであれば成功

・laravelの@csrfの流れ
    1.暗号作成時、その暗号を「/storage/framework/sessions/暗号の値」へファイルとして保存  
    2.@csrfで<form>タグなどのリクエスト内に_tokenという名前で暗号の値をセット  
    3.「/storage/framework/sessions/暗号の値」へファイル保存された値、2.でセットされた値を比較  
    ※上記より、Laravelではsessionを利用していない  
```





