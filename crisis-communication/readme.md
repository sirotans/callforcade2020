### やること

ここでは最初のチャットボットをやってみます。
画面キャプチャ付きの手順は[こちら](https://developer.ibm.com/callforcode/getstarted/covid-19/crisis-communication/)から。
下の方にチュートリアルが4つあります。
まず、「Create a COVID crisis chatbot and connect it to data sources」をやってみます。
出来上がりのイメージは[こちら](https://covid-assistant-simple-sirotan.mybluemix.net)です。

1. [チャットボットの作成](https://github.com/sirotans/callforcode2020/edit/master/crisis-communication/lab1)
2. [Webへの公開](https://github.com/sirotans/callforcode2020/edit/master/crisis-communication/lab2)

### アプリの全体像

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
