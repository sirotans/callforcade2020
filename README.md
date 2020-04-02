### Call for Code 2020 とは
一言で言うと、「コードの力で世界を救おう」と言う活動と理解しています。
2018年から開始して3年目であり、今年のテーマは気候変動対策、またはCOVID-19対策です。
コンテストがあり、優勝者には賞金ももらえます。チームでの参加も可能です。応募は2020/7/31まで。ご興味のある方は[こちら](https://developer.ibm.com/jp/callforcode/)から。

さて、そうは言っても「何から始めたら？」「何するのかイメージがわかない」と言う方のために、[スターターキット](https://developer.ibm.com/callforcode/getstarted/covid-19/)というものが用意されています。スターターキットの中身は、COVID-19対策向けサンプルアプリとその作り方です。本記事では試しに作ってみた足跡を書いていきます。
なお、無料で試せます。

### スターターキット
早速[スターターキット](https://developer.ibm.com/callforcode/getstarted/covid-19/)にアクセスすると、う、英語・・・、と止まりそうになりますが、チュートリアル自体はシンプルな英語なので、苦手な方もgoogle翻訳や[Watsonの翻訳デモ](https://language-translator-demo.ng.bluemix.net/)などを使えば理解はできると思います。
逆にシンプルすぎて説明が足りていないのでは？と感じるところもあり。

本記事では自分が不明だったところを補足して記載しますので、元のチュートリアルを見ながら、わかりづらいところを本記事をご覧いただく形が良いのではないかと思います。

サンプルアプリは以下の3つです。
- Crisis Communication: チャットボット。コロナウィルスについて答えてくれます(ただし英語)
- Remote Education: オンライン学習サイト
- Community corporation: 物資がどこにあるのかを見つけて案内するモバイルアプリ

ここでは最初のチャットボットをやってみます。
手順は[こちら](https://developer.ibm.com/callforcode/getstarted/covid-19/crisis-communication/)から。
下の方にチュートリアルが4つあります。まず、「Create a COVID crisis chatbot and connect it to data sources」をやってみます。

出来上がりのイメージは[こちら](https://covid-assistant-simple-sirotan.mybluemix.net)です。


### まずは準備
このスターターキットは、IBM Cloudのクラウド環境とPCでやることが前提になっています。
準備として、IBM Cloudのアカウントが必要になりますが、ライトアカウントという無料登録が可能なアカウントがあります。
登録方法は先ほどの[Call for Codeのサイト](https://developer.ibm.com/jp/callforcode/)や、[こちらの記事](https://qiita.com/kmht/items/e77137a4af657777a7f9)をご参考に。
それから後で必要になるので、この[スターターキット](https://github.com/Call-for-Code/Solution-Starter-Kit-Communication-2020)をgithubからダウンロードしておきましょう。


### 早速作ってみる
チャットボットはWatson Assistantを使います。
Watson Assistantについて詳しくは[こちら](https://www.ibm.com/watson/jp-ja/developercloud/conversation.html)や[こちらの記事](https://qiita.com/ishida330/items/666ced65a04243ce286c)をご参照ください。
以下は手順です。手順の詳細や画面キャプチャは[チュートリアル](https://developer.ibm.com/tutorials/crisis-communication-chatbot-watson-assistant-webhook-integration-discovery-covid-data/)をご覧ください。なお、以下の数字はチュートリアルでの数字に対応しています。

##### Create your chatbot by setting up a Watson Assistant instance
1. IBMクラウドのカタログから、Watson Assistantを選び、以下項目を埋めて「作成」をクリック
・地域: どこでも良いですが、「東京」を選択します
・プラン: ライト
・サービス名: 任意の名前
・リソースグループ: Default

2. 「Watson Assistantの起動」をクリック
3. 「Create Assistant」をクリック
4. 以下を記載して「Create assistant」をクリック
・Name: COVID Crisis Communication などの任意の名前

5. 「Add dialog skill」をクリック。そして「Import skill」タブをクリックして、「Choose JSON File」をクリック、「skill-CDC-COVID-FAQ.json」ファイルを選択して「Import」をクリック。このファイルは先ほどのgithubからダウンロードしたディレクトリの以下にあります。
<ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/assistant/skill-CDC-COVID-FAQ.json

6. Assistantのページに戻り、右上のActionメニュー(⋮)から「Settings」をクリック
7. 左のメニューで「API Details」を選択し、以下をメモ(後で使う)
・Assistant ID
・Assistant URL
・Api Key

8. 右上のXを押して閉じ、アシスタント画面に戻り、右下の「Preview Link」ボタンを押すと、「Try it out and share the link」の下にURLが表示され、クリックするとチャットボットのテスト画面が現れます。
9. 質問してみましょう。英語onlyですが・・・
例)
・What is COVID-19?
・Can you tell me about symptoms

とりあえず動いたので、お疲れ様でした。

### 外部のデータソースと連携します
ここまででチャットボットらしきものは動いていると思います。ただ、これだと刻一刻と変わるコロナウィルスの状況・データには対応できないので、外部のデータソースと連携させます。
ここではWatson Discoveryというコグニティブなテキスト検索サービスと連携します。
Watson Discoveryについて詳しくは[こちら](https://www.ibm.com/watson/jp-ja/developercloud/discovery.html)や[こちらの記事](https://qiita.com/ishida330/items/b823d7c5b55806f04242)をご参照ください。

以下は手順です。手順の詳細や画面キャプチャは[チュートリアル](https://developer.ibm.com/tutorials/crisis-communication-chatbot-watson-assistant-webhook-integration-discovery-covid-data/)をご覧ください。

##### Integrate your chatbot with data sources
1. IBM Cloudのカタログから、Discoveryをクリック
2. 以下項目を埋めて「作成」をクリック。**作成完了に数分かかります。**
・地域: どこでも良いですが、「東京」を選択
・プラン: ライト
・サービス名: 任意の名前
・リソースグループ: Default

3. Watson Discoveryが作成されると、「資格情報」にAPI keyとURLが表示されるのでメモ。「Watson Discoveryの起動」をクリック
4. Watson Discovery Newsをクリック。これは世界中のニュース記事を集めてデータソースとしているサービスです。プルダウンからJapaneseも選べますが、ここではEnglishのままで。
5. 左上にどれくらいのニュースがデータソースになっているかの数字が出ています(執筆当時の2020/4では約13万件)。右上の「API」アイコンをクリックして、Collection IDとEnvironment IDをメモ

### サーバーレスを立ち上げる
今度はサーバーレスを立ち上げます。IBM CloudではIBM Cloud Functionsというサービスがあります。

##### Create Cloud Functions
1. IBM Cloudのカタログから、IBM Cloud Functionsをクリック。カタログから検索する場合は、左メニューの下の方の「価格プラン」に「ライト」のチェックを外して検索しましょう。ライトプランのサービスではありませんが、ライトアカウントでも作成はできます。おそらく課金もされていないはず・・・。
2. 「作成の開始」をクリック
3. 「Create Action」をクリック
4. 「Action Name」に任意の名前を入れます。「Runtime」は、Node.js 10 を選択
5. 「Code」の内容を、action/covid-webhook.jsの中身をコピペします。[github](https://github.com/Call-for-Code/Solution-Starter-Kit-Communication-2020/blob/master/starter-kit/webhook/action/covid-webhook.js)でも良いですが、先ほどダウンロードした以下にもあります。
<ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/webhook/action/covid-webhook.js

6. コードの説明。COVID-19 APIか、Watson Discovery APIをコールできる。type=apiとセットすると、COVID-19 APIをコールする
7. type=api and country=US とセットしてAPIコールすると、アメリカの統計情報が戻る。ちなみに日本はcountry=Japan
8. ここではサーバーレスでWatson Discovery APIをコールします。左メニューの「Parameters」を選択し、以下を記入します。**値は" "で囲う必要があります。**
・api_key: "Watson Discoveryのメニューでメモしたもの(「Integrate your chatbot with data sources」手順3)"
・url: "同じくメモしたもの(手順3)"
・collection_id: "同じくメモしたもの(手順5)"
・env_id: "同じくメモしたenvironment_id(手順5)"
9. 左メニュー「Endpoint」を選択し、「Web Action」→「Enable as Web Action」にチェック
10. URLをメモ

### webhookでWatson Assistantとデータソースを連携

##### Integrate data sources via a Watson Assistant webhook
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

### チャットボットをWebに公開してみる

長くなってきましたが、、、これで最後です。
これだけだと、自分しか見えないので、Web公開します。
チュートリアルの[Integrate a COVID-19 crisis communication chatbot on a website](https://developer.ibm.com/tutorials/create-a-covid-19-chatbot-embedded-on-a-website/)に手順があります。

準備として、Watson Assistantのcredential情報をメモします。
画面左上「リソース・リスト」→Services→作成済みWatson Assistantサービス→サービス資格情報→資格情報を表示

##### Configure the application
1. githubからダウンロード済みのディレクトリに移動

```
$ cd <ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/covid-simple
```

2.ファイルコピー

```
$ cp .env.example .env
```

3..envファイル編集。以下は私の環境での例です。

```
# Environment variables
ASSISTANT_URL=https://gateway-tok.watsonplatform.net/assistant/api  #東京の場合
ASSISTANT_ID=XXXXX #最初の方(Create your chatbot by setting up a Watson Assistant instanceの7.)でメモした内容

# IBM Cloud connection parameters
ASSISTANT_IAM_APIKEY=YYYYY #準備でメモした内容
ASSISTANT_IAM_URL=https://api.jp-tok.assistant.watson.cloud.ibm.com/instances/ZZZZZ #準備でメモした内容
```

4.上記例のように、.envファイルに「ASSISTANT_ID」も追加。「Create your chatbot by setting up a Watson Assistant instance」の7.でメモした内容です。

##### Running locally
PCでテスト稼動します。

1. 前提条件のインストール

```
$ npm install
```

2.アプリ起動

```
$ npm start
```

3.ブラウザで localhost:3000 を開いてみましょう。チャットボットが動いていればOKです。


##### Deploy to IBM Cloud as a Cloud Foundry application

1. IBM Cloud CLIでログインします。未インストールの方は[こちら](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started#overview)から。**インストールには数分かかります。**


```
$ ibmcloud login
```

2. Cloud Foundry CLIを設定します

```
$ ibmcloud target --cf
```

結果がこんな風になっていればOKです。

```
ターゲットの Cloud Foundry (https://api.us-south.cf.cloud.ibm.com)
ターゲットの組織 XXX
ターゲットのスペース dev
                            
API エンドポイント:      https://cloud.ibm.com   
地域:                    us-south   
ユーザー:                XXX   
アカウント:              XXX's Account (XXXXX)   
リソース・グループ:      Default   
CF API エンドポイント:   https://api.us-south.cf.cloud.ibm.com (API バージョン: 2.146.0)   
組織:                    XXX  
スペース:                dev   
```

3.covid-simpleディレクトリにあるmanifest.ymlの中身を編集して、「name」の値をユニークなものします。

```
applications:
- name: covid-assistant-simple-sirotan #例です
  command: npm start
  path: .
  memory: 256M
  instances: 1
```

4.アプリをデプロイします。

```
$ ibmcloud app push
```

もしCloud Foundry CLIがインストールされていないよ、というエラーが出たら、インストールします。

```
$ ibmcloud cf install
```

5.IBM Cloudのメニュー→リソース・リスト→Cloud Foundryアプリ　にアプリができていると思います。クリックして画面右上の「アプリURLにアクセス」をクリック。チャットボットが表示されていたら完成です。


### ここまでの全体像

![alt](https://developer.ibm.com/developer/tutorials/create-a-covid-19-chatbot-embedded-on-a-website/images/covid-website-diagram.png)


1. ユーザーがWebサイトでチャットボット(COVID-19 FAQ)に対して質問
2. Webサーバー(Node.js)が、Watson Assistantサービスにコール
3. Watson Assistantは自然言語解析や機械学習を利用して、質問からユーザーのentity(目的語)やintent(意図)を抽出
4. COVID-19 FAQは、信頼できるCDC(Centers for Disease Control and Prevention)のデータをソースとしています
5. Watson Assistantが、サーバーレスアプリ(IBM Cloud Function)を呼び出し
6. IBM Cloud Functionは、Watson Discoveryをコール
7. Watson Discoveryは、関連する記事を検索し、関連性の高い記事とともに応答
8. Watson Assistantが、サーバーレスアプリ(IBM Cloud Function)を呼び出し
9. IBM Cloud Functionは、統計情報を取り出すCOVID-19 APIをコール
10. Watson Assistantがユーザーへ返答
11. Webサーバー(Node.js)がチャットの返答を表示


### まとめ
ここまでお疲れ様でした。
これはあくまでサンプルアプリなので、カスタマイズしたりしてご自身のアプリ作成の参考にしていただければと思います。
他のチュートリアルも余裕があれば、やってみたいと思います。

以上
