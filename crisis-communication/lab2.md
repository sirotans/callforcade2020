
### チャットボットをWebに公開してみる
今のままだと自分しか見えないので、Web公開します。
チュートリアルの[Integrate a COVID-19 crisis communication chatbot on a website](https://developer.ibm.com/tutorials/create-a-covid-19-chatbot-embedded-on-a-website/)に手順があります。

準備として、Watson Assistantのcredential情報をメモします。
画面左上「リソース・リスト」→Services→作成済みWatson Assistantサービス→サービス資格情報→資格情報を表示

##### Configure the application
1. githubからダウンロード済みのディレクトリに移動

```
$ cd <ダウンロードフォルダ>/Solution-Starter-Kit-Communication-2020-master/starter-kit/covid-simple
```

2. ファイルコピー

```
$ cp .env.example .env
```

3. .envファイル編集。以下は私の環境での例です。

```
# Environment variables
ASSISTANT_URL=https://gateway-tok.watsonplatform.net/assistant/api  #東京の場合
ASSISTANT_ID=XXXXX #最初の方(Create your chatbot by setting up a Watson Assistant instanceの7.)でメモした内容

# IBM Cloud connection parameters
ASSISTANT_IAM_APIKEY=YYYYY #準備でメモした内容
ASSISTANT_IAM_URL=https://api.jp-tok.assistant.watson.cloud.ibm.com/instances/ZZZZZ #準備でメモした内容
```

4. 上記例のように、.envファイルに「ASSISTANT_ID」も追加。「Create your chatbot by setting up a Watson Assistant instance」の7.でメモした内容です。

##### Running locally
PCでテスト稼動します。

1. 前提条件のインストール

```
$ npm install
```

2. アプリ起動

```
$ npm start
```

3. ブラウザで localhost:3000 を開いてみましょう。チャットボットが動いていればOKです。


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

3. covid-simpleディレクトリにあるmanifest.ymlの中身を編集して、「name」の値をユニークなものします。

```
applications:
- name: covid-assistant-simple-sirotan #例です
  command: npm start
  path: .
  memory: 256M
  instances: 1
```

4. アプリをデプロイします。

```
$ ibmcloud app push
```

もしCloud Foundry CLIがインストールされていないよ、というエラーが出たら、インストールします。

```
$ ibmcloud cf install
```

5. IBM Cloudのメニュー→リソース・リスト→Cloud Foundryアプリ　にアプリができていると思います。クリックして画面右上の「アプリURLにアクセス」をクリック。チャットボットが表示されていたら完成です。


[ここまでやった全体像を確認してみましょう](https://github.com/sirotans/callforcode2020/blob/master/crisis-communication/readme.md#アプリの全体像)
