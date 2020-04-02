### 早速作ってみる
チャットボットはWatson Assistantを使います。
Watson Assistantについて詳しくは[こちら](https://www.ibm.com/watson/jp-ja/developercloud/conversation.html)や[こちらの記事](https://qiita.com/ishida330/items/666ced65a04243ce286c)をご参照ください。
以下は手順です。手順の詳細や画面キャプチャは[チュートリアル](https://developer.ibm.com/tutorials/crisis-communication-chatbot-watson-assistant-webhook-integration-discovery-covid-data/)をご覧ください。なお、以下の数字はチュートリアルでの数字に対応しています。

#### Create your chatbot by setting up a Watson Assistant instance
1. IBMクラウドのカタログから、Watson Assistantを選び、以下項目を埋めて「作成」をクリック
- 地域: どこでも良いですが、「東京」を選択します
- プラン: ライト
- サービス名: 任意の名前
- リソースグループ: Default

2. 「Watson Assistantの起動」をクリック
3. 「Create Assistant」をクリック
4. 以下を記載して「Create assistant」をクリック
- Name: COVID Crisis Communication などの任意の名前

5. 「Add dialog skill」をクリック。そして「Import skill」タブをクリックして、「Choose JSON File」をクリック、「skill-CDC-COVID-FAQ.json」ファイルを選択して「Import」をクリック。このファイルは先ほどのgithubからダウンロードしたディレクトリの以下にあります。
<ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/assistant/skill-CDC-COVID-FAQ.json

6. Assistantのページに戻り、右上のActionメニュー(⋮)から「Settings」をクリック
7. 左のメニューで「API Details」を選択し、以下をメモ(後で使う)
- Assistant ID
- Assistant URL
- Api Key

8. 右上のXを押して閉じ、アシスタント画面に戻り、右下の「Preview Link」ボタンを押すと、「Try it out and share the link」の下にURLが表示され、クリックするとチャットボットのテスト画面が現れます。
9. 質問してみましょう。英語onlyですが・・・
例)
- What is COVID-19?
- Can you tell me about symptoms

とりあえず動いたので、お疲れ様でした。

### 外部のデータソースと連携する
ここまででチャットボットらしきものは動いていると思います。ただ、これだと刻一刻と変わるコロナウィルスの状況・データには対応できないので、外部のデータソースと連携させます。
ここではWatson Discoveryというコグニティブなテキスト検索サービスと連携します。
Watson Discoveryについて詳しくは[こちら](https://www.ibm.com/watson/jp-ja/developercloud/discovery.html)や[こちらの記事](https://qiita.com/ishida330/items/b823d7c5b55806f04242)をご参照ください。

以下は手順です。手順の詳細や画面キャプチャは[チュートリアル](https://developer.ibm.com/tutorials/crisis-communication-chatbot-watson-assistant-webhook-integration-discovery-covid-data/)をご覧ください。

#### Integrate your chatbot with data sources
1. IBM Cloudのカタログから、Discoveryをクリック
2. 以下項目を埋めて「作成」をクリック。**作成完了に数分かかります。**
- 地域: どこでも良いですが、「東京」を選択
- プラン: ライト
- サービス名: 任意の名前
- リソースグループ: Default

3. Watson Discoveryが作成されると、「資格情報」にAPI keyとURLが表示されるのでメモ。「Watson Discoveryの起動」をクリック
4. Watson Discovery Newsをクリック。これは世界中のニュース記事を集めてデータソースとしているサービスです。プルダウンからJapaneseも選べますが、ここではEnglishのままで。
5. 左上にどれくらいのニュースがデータソースになっているかの数字が出ています(執筆当時の2020/4では約13万件)。右上の「API」アイコンをクリックして、Collection IDとEnvironment IDをメモ

### サーバーレスを立ち上げる
今度はサーバーレスを立ち上げます。IBM CloudではIBM Cloud Functionsというサービスがあります。

#### Create Cloud Functions
1. IBM Cloudのカタログから、IBM Cloud Functionsをクリック。カタログから検索する場合は、左メニューの下の方の「価格プラン」に「ライト」のチェックを外して検索しましょう。ライトプランのサービスではありませんが、ライトアカウントでも作成はできます。おそらく課金もされていないはず・・・。
2. 「作成の開始」をクリック
3. 「Create Action」をクリック
4. 「Action Name」に任意の名前を入れます。「Runtime」は、Node.js 10 を選択
5. 「Code」の内容を、action/covid-webhook.jsの中身をコピペします。[github](https://github.com/Call-for-Code/Solution-Starter-Kit-Communication-2020/blob/master/starter-kit/webhook/action/covid-webhook.js)でも良いですが、先ほどダウンロードした以下にもあります。
<ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/webhook/action/covid-webhook.js

6. コードの説明。COVID-19 APIか、Watson Discovery APIをコールできる。type=apiとセットすると、COVID-19 APIをコールする
7. type=api and country=US とセットしてAPIコールすると、アメリカの統計情報が戻る。ちなみに日本はcountry=Japan
8. ここではサーバーレスでWatson Discovery APIをコールします。左メニューの「Parameters」を選択し、以下を記入します。**値は" "で囲う必要があります。**
- api_key: "Watson Discoveryのメニューでメモしたもの(「Integrate your chatbot with data sources」手順3)"
- url: "同じくメモしたもの(手順3)"
- collection_id: "同じくメモしたもの(手順5)"
- env_id: "同じくメモしたenvironment_id(手順5)"
9. 左メニュー「Endpoint」を選択し、「Web Action」→「Enable as Web Action」にチェック
10. URLをメモ

### webhookでWatson Assistantとデータソースを連携

#### Integrate data sources via a Watson Assistant webhook
1. 先ほど(「Create your chatbot by setting up a Watson Assistant instance」手順4.)の作成済みAssistantの画面に戻ります。
2. 作成済みのダイヤログスキルをクリックし、左メニューから「Options」をクリック
3. その下の「Webhooks」をクリック。URLのところに、先ほどの手順10.でメモしたURLを貼り付けし、**URLの最後に.jsonをつける(忘れずに)**
4. 左メニューから「Dialog」をクリック
5. どれでも良いようなんですが、ここでは画面キャプチャにしたがって、下の方にある「US Interaction Level」をクリック
6. 右上の「Customize」をクリック
7. WebhooksのトグルをONにして、Save
8. パラメータを追加とありますが、とりあえず画面キャプチャと同様のまま
・type:"api"
・country:"US"

9. 右上の「Try it」で、外部データソースと連携したチャットボットのテストができます。
「How many cases in the US?」と聞いてみてください。

お疲れ様でした！

一休みしたら、[Webへ公開](https://github.com/sirotans/callforcode2020/blob/master/crisis-communication/lab2.md)してみましょう。
