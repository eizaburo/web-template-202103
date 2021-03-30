## 説明

2021年3月時点でのHTML記述方法の参考テンプレート。
Web制作に必要な要素を散りばめてあるつもり。これをベースに各種レイアウトを作ればよいかなと。

## 完成イメージ

画像はunsplashからAPIで取得してるため随時変わります。

![image](./images/readme_img002.png)

## 主な要素

* 企業サイト等での最低要素を想定
* レスポンシブ
* flex-boxレイアウト
* IE11対応

## 注意点

contact.html中のbase_urlを自身のGoogle Apps Script（API）のURLに変更する必要があります。
セットアップ方法は[こちら](https://qiita.com/zaburo/private/a92ec920c83090f454a1)をご覧ください。

## API側のコード

APIはPOSTにてemail, title, contentが処理できれば何でもいいですが、ここではGoogle Apps Script(GAS)で下記のコードが実装されていることを想定しています。
コードは下記の通りです。スプレッドシートのコードエディタに記述します。
パラメータを受け取りシート1に記録時間を追加して保存しています。

```js
//POST時処理
function doPost(e) {

  //値の取得
  var email = e.parameter.email;
  var title = e.parameter.title;
  var content = e.parameter.content;

  //スプレッドシート取得
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName('シート1');

  //最終行に追加
  sheet.appendRow([email, title, content, new Date()]);

  //response
  ContentService.createTextOutput("success");

}
```

>エラー処理などは一切していません。