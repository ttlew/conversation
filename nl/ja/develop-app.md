---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# クライアント・アプリケーションの構築

作業ダイアログが用意できました。ここで、ユーザーと対話し、{{site.data.keyword.conversationfull}} サービスと通信するアプリケーションを開発します。
{: shortdesc}

このチュートリアルのコード例は、Node.js Watson SDK を使用して JavaScript で書かれていますが、他の言語も使用できます。ご使用のプログラミング言語の Watson [SDK ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} をインストールして、SDK 文書で詳細情報を確認してください。
{: tip}

## Conversation サービスのセットアップ

このセクションで作成するアプリケーション例は、コグニティブ・パーソナル・アシスタントの機能をいくつか実装します。アプリケーション・コードは {{site.data.keyword.conversationshort}} ワークスペースに接続し、そこでコグニティブ処理 (ユーザー・インテントの検出など) が行われます。

この例に進む前に、必要な {{site.data.keyword.conversationshort}} ワークスペースを以下のようにしてセットアップする必要があります。

1.  ワークスペース用 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON ファイル</a> をダウンロードします。
1.  {{site.data.keyword.conversationshort}} サービスのインスタンスに [ワークスペースをインポート](configure-workspace.html#creating-workspaces)します。

## サービス情報の取得

{{site.data.keyword.conversationshort}} サービスの REST API にアクセスするには、アプリケーションが {{site.data.keyword.Bluemix}} で認証され、適切な {{site.data.keyword.conversationshort}} ワークスペースに接続できる必要があります。また、サービス資格情報とワークスペース ID をコピーして、アプリケーション・コードに貼り付ける必要があります。

ワークスペースからサービス資格情報とワークスペース ID にアクセスするには、![メニュー](images/Menu_16.png) メニューを選択し、**「デプロイ」**を選択して、**「資格情報」**タブに移動します。

Bluemix ダッシュボードからサービス資格情報にアクセスすることもできます。

## Conversation サービスとの通信

{{site.data.keyword.conversationshort}} サービスとの対話はシンプルです。次の Node.js 例を見てみましょう。この例は、サービスに接続し、単一メッセージを送信して、出力をコンソールに記録します。

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}

最初のステップは、{{site.data.keyword.conversationshort}} サービス用のラッパーを作成することが目的です。

このラッパーは、入力の送信先と出力の受信元として使用するオブジェクトであり、サービスです。サービス・ラッパーを作成するときは、サービス・キーからの認証資格情報だけでなく、使用する {{site.data.keyword.conversationshort}} API のバージョンも指定します。

この Node.js 例では、ラッパーは変数 `conversation` に保管されている `ConversationV1` のインスタンスです。他の言語用の Watson SDK も、サービス・ラッパーをインスタンス化するための同等のメカニズムを備えています。

サービス・ラッパーを作成した後、それを使用して {{site.data.keyword.conversationshort}} サービスにメッセージを送信します。この例では、メッセージは空です。目的はダイアログ内の conversation_start ノードをトリガーすることだけなので、入力テキストは必要ありません。

`node <filename.js>` コマンドを使用して、このアプリケーション例を実行します。

**注:** `npm install watson-developer-cloud` を使用して Node.js Watson SDK がインストールされていることを確認してください。

すべてが正常に機能していれば、{{site.data.keyword.conversationshort}} サービスはダイアログからの出力を返し、その出力は次のようにコンソールに記録されます。

```
Welcome to the Conversation example!
```
{: screen}

この出力は、{{site.data.keyword.conversationshort}} サービスと正常に通信できて、ダイアログ内の conversation_start ノードで指定されたウェルカム・メッセージを受け取ったことを示しています。これで、ユーザー・インターフェースを追加して、ユーザー入力を処理できるようにすることができるようになりました。

## インテントを検出するためのユーザー入力の処理

ユーザー入力を処理するためには、アプリケーションにユーザー・インターフェースを追加する必要があります。この例では、いろいろなことをシンプルにしておき、標準入出力を使用します。そのためには、Node.js prompt-sync モジュールを使用できます。(prompt-sync は、`npm install prompt-sync` を使用してインストールできます。)

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}

このバージョンのアプリケーションも、従来と同じように始まっています。つまり、{{site.data.keyword.conversationshort}} サービスに空のメッセージを送信して会話を開始します。

`processResponse()` 関数はダイアログで検出されたインテントを出力テキストとともに表示するようになり、ユーザー入力の次のラウンドのプロンプトを出します。

それでも、以下に示すように、まだ何かが正しくありません。

```
Welcome to the Conversation example!
>> hello
Detected intent: #hello
Welcome to the Conversation example!
>> what time is it?
Detected intent: #time
Welcome to the Conversation example!
>> goodbye
Detected intent: #goodbye
Welcome to the Conversation example!
>>
```
{: screen}

{{site.data.keyword.conversationshort}} サービスは正しいインテントを検出しているにもかかわらず、会話の中のどの順番でも、conversation_start ノードからのウェルカム・メッセージ (`Welcome to the Conversation example!`) が返されています。

こうなっているのは、{{site.data.keyword.conversationshort}} サービスがステートレスだからです。状態情報を維持する処理はアプリケーションが担います。状態を維持するための処理はまだ何もしていないので、{{site.data.keyword.conversationshort}} サービスはユーザー入力の各ラウンドを新しい会話の中の 1 番目と見なして、conversation_start ノードをトリガーするのです。

## 状態の維持

会話の状態情報は、*context* を使用して維持されます。コンテキストは、アプリケーションと {{site.data.keyword.conversationshort}} サービスの間でやり取りされる JSON オブジェクトです。会話の中のある順番から次の順番までコンテキストを維持する処理は、アプリケーションが担います。

コンテキストには、ユーザーとの会話ごとの固有 ID のほかに、会話の中の順番ごとにインクリメントされるカウンターも含まれます。前のバージョンの例では、コンテキストが保持されませんでした。つまり、入力の各ラウンドは新しい会話の始まりのように見えるということです。この状態は、毎回コンテキストを保存して、それを {{site.data.keyword.conversationshort}} サービスに送り返すことによって修正できます。

会話の中の場所を維持することに加えて、アプリケーションと {{site.data.keyword.conversationshort}} サービスの間でやり取りする他の必要データを保管する目的でも、コンテキストを使用できます。こうしたデータとして、会話の最後まで維持する永続データ (顧客の名前やアカウント番号など) や、追跡する他のデータ (オプション設定の現在の状況など) を含めることができます。

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
    var newMessageFromUser = prompt('>> ');
    // Send back the context to maintain state.
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}

前の例から変った点は、次に示すように、会話の各ラウンドで、前のラウンドで受け取った `response.context` オブジェクトを送り返すようになったということだけです。

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}

これにより、ある順番から次の順番までコンテキストが維持されるようになるので、次のように、{{site.data.keyword.conversationshort}} サービスが各順番を最初の順番と見なさなくなります。

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

大分改善されました。{{site.data.keyword.conversationshort}} サービスはインテントを正しく認識していて、ダイアログはインテントごとに正しい出力テキストを返しています (提供される場合)。

しかし、それ以外のことは起こっていません。時間を尋ねても答が返ってきません。「goodbye」と言っても会話が終わりません。これらのインテントについては、アプリが追加のアクションを取る必要があるからです。

## アプリ・アクションの実装

出力テキストをユーザーに表示することに加え、検出されたインテントに基づいてアプリケーションがアクションを実行する必要があるときにシグナル通知する目的でも、ダイアログは応答 JSON 内の `output` オブジェクトを使用します。

これらのアクション・フラグは `action` プロパティーを使用して送信され、このプロパティーはダイアログで応答 JSON の一部として定義されます。ダイアログは、アプリケーションが何かをする必要があると判断すると、`action` の値を適切な値 (`display_time` か `end_conversation` のどちらか) に設定します。

`output` は単なる JSON オブジェクトであり、必要な場合は、有効であればどのようなコンテンツでもそれに追加できるということを覚えておいてください。より複雑なアプリケーションでは、複数のアクション・フラグを含む配列を使用することもできます。

この例では、単一のアクション・フラグをサポートするキーと値の単純なペアを使用します。アプリケーション・コードは、応答に含まれる `action` プロパティーの値を検査した後に、指定された何らかのアクションを実行する必要があります。(検出されたインテントが正しく識別されていることを確認できたので、このバージョンではさらに、検出されたインテントの表示を削除します。)

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // User said goodbye, so we're done.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Display the output from dialog, if any.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      input: { text: newMessageFromUser },
      // Send back the context to maintain state.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}

processResponse() 関数は、{{site.data.keyword.conversationshort}} サービスから受け取った `output` オブジェクトの `action` プロパティーの値を検査するようになりました。値が `display_time` か `end_conversation` のどちらかであれば、アプリケーションはアプリケーション・アクションを実行します。

```
Welcome to the Conversation example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

成功です! アプリケーションは {{site.data.keyword.conversationshort}} サービスを使用して、自然言語入力の中のインテントを識別し、適切な応答を表示して、要求されたアクションを実行するようになりました。

もちろん、実用アプリケーションの場合は、Web チャット・ウィンドウなど、より洗練されたユーザー・インターフェースを使用することになります。また、より複雑なアクションを実装し、おそらく、顧客データベースなどのビジネス・システムと統合されることになります。しかし、その場合でも、アプリケーションが {{site.data.keyword.conversationshort}} サービスと対話する方法の基本的原則は変わりません。

より複雑ないくつかの例については、「Navigation」ペインでサンプル・アプリをご覧ください。
