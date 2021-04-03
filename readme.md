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

## 完全に動作させるための注意点

### API側の実装

完全に実行させるためにはcontact.html中のbase_urlを動作するAPIのURLに変更する必要があります。
APIはPOSTにてemail, title, contentが処理できれば何でもいいですが、ここではGoogle Apps Script(GAS)で下記のコードが実装されていることを想定しています。
コードは下記の通りです。スプレッドシートのコードエディタに記述します。
パラメータを受け取りシート1に記録時間を追加して保存しています。
セットアップ方法等については[こちら](https://qiita.com/zaburo/private/a92ec920c83090f454a1)をご覧ください。

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
  return ContentService.createTextOutput("success");

}
```

>エラー処理などは一切していません。

## 既知の問題

* 標準ではfetch()はIE11では動きません（CDN等で補助ライブラリを読み込めば動きます）。
* GASは標準でresponseをリダイレクト先で表示するので、302が必ず帰ってくる。
* fetch()でno-corsモードにすると、レスポンス内容がopaqueとなり、status含めうまく取得できません（エラー処理に限界があります）。