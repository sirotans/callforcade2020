# callforcode2020

# Call for Code 2020 とは

一言で言うと、「コードの力で世界を救おう」と言う活動と理解しています。
2018年から開始して3年目であり、今年のテーマは気候変動対策、またはCOVID-19対策です。
コンテストがあり、優勝者には賞金ももらえます。チームでの参加も可能です。応募は2020/7/31まで。ご興味のある方はこちらから。

さて、そうは言っても「何から始めたら？」「何するのかイメージがわかない」と言う方のために、スターターキットというものが用意されています。スターターキットの中身は、COVID-19対策向けサンプルアプリとその作り方です。本記事では試しに作ってみた足跡を書いていきます。
なお、無料で試せます。

# スターターキット

早速スターターキットにアクセスすると、う、英語・・・、と止まりそうになりますが、チュートリアル自体はシンプルな英語なので、苦手な方もgoogle翻訳やWatsonの翻訳デモなどを使えば理解はできると思います。
逆にシンプルすぎて説明が足りていないのでは？と感じるところもあり。

本記事では自分が不明だったところを補足して記載しますので、元のチュートリアルを見ながら、わかりづらいところを本記事をご覧いただく形が良いのではないかと思います。

サンプルアプリは以下の3つです。
- Crisis Communication: チャットボット。コロナウィルスについて答えてくれます(ただし英語)
- Remote Education: オンライン学習サイト
- Community corporation: 物資がどこにあるのかを見つけて案内するモバイルアプリ

ここでは最初のチャットボットをやってみます。
手順はこちらから。
下の方にチュートリアルが4つあります。まず、「Create a COVID crisis chatbot and connect it to data sources」をやってみます。

出来上がりのイメージはこちらです。

# まずは準備

このスターターキットは、IBM Cloudのクラウド環境とPCでやることが前提になっています。
準備として、IBM Cloudのアカウントが必要になりますが、ライトアカウントという無料登録が可能なアカウントがあります。
登録方法は先ほどのCall for Codeのサイトや、こちらの記事をご参考に。
それから後で必要になるので、このスターターキットをgithubからダウンロードしておきましょう。
早速作ってみる

チャットボットはWatson Assistantを使います。
Watson Assistantについて詳しくはこちらやこちらの記事をご参照ください。
以下は手順です。手順の詳細や画面キャプチャはチュートリアルをご覧ください。なお、以下の数字はチュートリアルでの数字に対応しています。
Create your chatbot by setting up a Watson Assistant instance

    IBMクラウドのカタログから、Watson Assistantを選び、以下項目を埋めて「作成」をクリック
    ・地域: どこでも良いですが、「東京」を選択します
    ・プラン: ライト
    ・サービス名: 任意の名前
    ・リソースグループ: Default

    「Watson Assistantの起動」をクリック

    「Create Assistant」をクリック

    以下を記載して「Create assistant」をクリック
    ・Name: COVID Crisis Communication などの任意の名前

    「Add dialog skill」をクリック。そして「Import skill」タブをクリックして、「Choose JSON File」をクリック、「skill-CDC-COVID-FAQ.json」ファイルを選択して「Import」をクリック。このファイルは先ほどのgithubからダウンロードしたディレクトリの以下にあります。
    <ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/assistant/skill-CDC-COVID-FAQ.json

    Assistantのページに戻り、右上のActionメニュー(⋮)から「Settings」をクリック

    左のメニューで「API Details」を選択し、以下をメモ(後で使う)
    ・Assistant ID
    ・Assistant URL
    ・Api Key

    右上のXを押して閉じ、アシスタント画面に戻り、右下の「Preview Link」ボタンを押すと、「Try it out and share the link」の下にURLが表示され、クリックするとチャットボットのテスト画面が現れます。

    質問してみましょう。英語onlyですが・・・
    例)
    ・What is COVID-19?
    ・Can you tell me about symptoms

とりあえず動いたので、お疲れ様でした。

# 外部のデータソースと連携します

ここまででチャットボットらしきものは動いていると思います。ただ、これだと刻一刻と変わるコロナウィルスの状況・データには対応できないので、外部のデータソースと連携させます。
ここではWatson Discoveryというコグニティブなテキスト検索サービスと連携します。
Watson Discoveryについて詳しくはこちらやこちらの記事をご参照ください。

以下は手順です。手順の詳細や画面キャプチャはチュートリアルをご覧ください。
Integrate your chatbot with data sources

    IBM Cloudのカタログから、Discoveryをクリック

    以下項目を埋めて「作成」をクリック。作成完了に数分かかります。
    ・地域: どこでも良いですが、「東京」を選択
    ・プラン: ライト
    ・サービス名: 任意の名前
    ・リソースグループ: Default

    Watson Discoveryが作成されると、「資格情報」にAPI keyとURLが表示されるのでメモ。「Watson Discoveryの起動」をクリック

    Watson Discovery Newsをクリック。これは世界中のニュース記事を集めてデータソースとしているサービスです。プルダウンからJapaneseも選べますが、ここではEnglishのままで。

    左上にどれくらいのニュースがデータソースになっているかの数字が出ています(執筆当時の2020/4では約13万件)。右上の「API」アイコンをクリックして、Collection IDとEnvironment IDをメモ

# サーバーレスを立ち上げる

今度はサーバーレスを立ち上げます。IBM CloudではIBM Cloud Functionsというサービスがあります。
Create Cloud Functions

    IBM Cloudのカタログから、IBM Cloud Functionsをクリック。カタログから検索する場合は、左メニューの下の方の「価格プラン」に「ライト」のチェックを外して検索しましょう。ライトプランのサービスではありませんが、ライトアカウントでも作成はできます。おそらく課金もされていないはず・・・。
    「作成の開始」をクリック
    「Create Action」をクリック
    「Action Name」に任意の名前を入れます。「Runtime」は、Node.js 10 を選択

    「Code」の内容を、action/covid-webhook.jsの中身をコピペします。githubでも良いですが、先ほどダウンロードした以下にもあります。
    <ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/webhook/action/covid-webhook.js

    コードの説明。COVID-19 APIか、Watson Discovery APIをコールできる。type=apiとセットすると、COVID-19 APIをコールする

    type=api and country=US とセットしてAPIコールすると、アメリカの統計情報が戻る。ちなみに日本はcountry=Japan

    ここではサーバーレスでWatson Discovery APIをコールします。左メニューの「Parameters」を選択し、以下を記入します。値は" "で囲う必要があります。
    ・api_key: "Watson Discoveryのメニューでメモしたもの(「Integrate your chatbot with data sources」手順3)"
    ・url: "同じくメモしたもの(手順3)"
    ・collection_id: "同じくメモしたもの(手順5)"
    ・env_id: "同じくメモしたenvironment_id(手順5)"

    左メニュー「Endpoint」を選択し、「Web Action」→「Enable as Web Action」にチェック

    URLをメモ

# webhookでWatson Assistantとデータソースを連携
Integrate data sources via a Watson Assistant webhook

    先ほど(「Create your chatbot by setting up a Watson Assistant instance」手順4.)の作成済みAssistantの画面に戻ります。
    作成済みのダイヤログスキルをクリックし、左メニューから「Options」をクリック
    その下の「Webhooks」をクリック。URLのところに、先ほどの手順10.でメモしたURLを貼り付けし、URLの最後に.jsonをつける(忘れずに)
    左メニューから「Dialog」をクリック
    どれでも良いようなんですが、ここでは画面キャプチャにしたがって、下の方にある「US Interaction Level」をクリック
    右上の「Customize」をクリック
    WebhooksのトグルをONにして、Save

    パラメータを追加とありますが、とりあえず画面キャプチャと同様のまま
    ・type:"api"
    ・country:"US"

    右上の「Try it」で、外部データソースと連携したチャットボットのテストができます。
    「How many cases in the US?」と聞いてみてください。

お疲れ様でした！

# チャットボットをWebに公開してみる

長くなってきましたが、、、これで最後です。
これだけだと、自分しか見えないので、Web公開します。
チュートリアルのIntegrate a COVID-19 crisis communication chatbot on a websiteに手順があります。

準備として、Watson Assistantのcredential情報をメモします。
画面左上「リソース・リスト」→Services→作成済みWatson Assistantサービス→サービス資格情報→資格情報を表示
Configure the application

    githubからダウンロード済みのディレクトリに移動

