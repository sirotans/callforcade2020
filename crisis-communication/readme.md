### やること




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