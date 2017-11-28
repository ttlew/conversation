---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-10"

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

# 概説チュートリアル
{: #gettingstarted}

この簡潔なチュートリアルでは、{{site.data.keyword.conversationshort}} ツールを紹介し、最初の会話を作成するプロセスを示します。{: shortdesc}

## 始める前に
{: #prerequisites}

サービス・インスタンスを既に作成している場合は、ここで示す前提条件が満たされており、準備ができています。ステップ 1 に進んでください。
{: tip}

1.  [{{site.data.keyword.conversationshort}} サービス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.{DomainName}/catalog/services/conversation/){: new_window} に移動し、無料 {{site.data.keyword.Bluemix_notm}} アカウントの登録を行うか、またはログインします。
1.  ログインした後、{{site.data.keyword.conversationshort}} ページの**「サービス名」**フィールドに `conversation-tutorial` と入力し、**「作成」**をクリックします。

## ステップ 1: ツールを起動する
{: #launch-tool}

サービス・インスタンスを作成した後、インスタンスのダッシュボードが表示されます。ここから  {{site.data.keyword.conversationshort}} ツールを起動します。

**「管理」**、**「ツールの起動 (Launch tool)」**の順にクリックします。

![ツールを起動する、IBM Bluemix Watson の「管理」ページを表示](images/gs-launch-tool.png)

ツールに別途ログインするよう促すプロンプトが出される場合があります。その場合は、IBM Bluemix 資格情報を入力してログインしてください。

## ステップ 2: ワークスペースを作成する
{: #create-workspace}

{{site.data.keyword.conversationshort}} ツールでの最初のステップとして、ワークスペースを作成します。

[*ワークスペース*](configure-workspace.html) とは、会話フローを定義する成果物のコンテナーです。

1.  {{site.data.keyword.conversationshort}} ツールで**「Create」**をクリックします。
1.  ワークスペースに`「Conversation tutorial」`という名前を付け、**「Create」**をクリックします。新規ワークスペースの**「Intents」**タブが表示されます。

![ユーザーが「Conversation tutorial」ワークスペースを作成するアニメーションを表示。](images/gs-create-workspace-animated.gif)

## ステップ 3: インテントを作成する
{: #create-intents}

[インテント](intents.html)は、ユーザーの入力の目的を表します。インテントは、ユーザーがアプリケーションを使用して実行したいアクションと見なすことができます。

この例では、分かりやすくするために 2 つのインテントのみを定義します。「こんにちは」と言うインテントと、「さようなら」と言うインテントです。

1.  「Intents」タブが表示されていることを確認します。(ワークスペースを作成したところであれば、既に表示されているはずです。)
1.  **「新規作成 (Create new)」**をクリックします。
1.  インテントに `hello` という名前を付けます。
1.  **「User example」**として `こんにちは` と入力し、Enter キーを押します。

   *サンプル* によって、どのようなユーザー入力をインテントと一致させるのかを {{site.data.keyword.conversationshort}} サービスに指示します。サンプルが多ければ多いほど、サービスはユーザーのインテントをいっそう正確に認識できるようになります。
1.  さらに 4 つのサンプルを追加し、**「Done」**をクリックして、#hello インテントの作成を終了します。
    - `おはよう`
    - `おはようございます`
    - `こんにちは`
    - `やあ`

   ![ユーザーが発話サンプルを追加して #hello インテントを作成するアニメーションを表示。](images/gs-add-intents-animated.gif)

1.  以下の 5 つのサンプルを追加して、#goodbye という名前の別のインテントを作成します。
    - `バイバイ`
    - `元気でね`
    - `さようなら`
    - `終わりました`
    - `またね`

ここまでで、#hello と #goodbye という 2 つのインテントを作成し、ユーザー入力サンプルを提供することで、ユーザーの入力に含まれるインテントを認識できるように {{site.data.keyword.watson}} がトレーニングされるようにしました。

![#goodbye インテントと #hello インテントがリストされた「Intents」ページを表示](images/gs-add-intents-result.png)

## ステップ 4: 対話を作成する
{: #build-dialog}

[対話](dialog-build.html)とは、論理ツリー形式で会話の流れを定義したものです。ツリーの各ノードには、ユーザーの入力に基づいてトリガーとなる、条件があります。

これから、#hello インテントと #goodbye インテントを処理するシンプルな対話を作成します。それぞれのインテントにはノードが 1 つだけあります。

### 開始ノードの追加

1.  {{site.data.keyword.conversationshort}} ツールで、**「Dialog」**タブをクリックします。
1.  **「Create」**をクリックします。 2 つのノードが表示されます。
    - **Welcome**: ユーザーがボットとのやり取りを開始するときに表示される、あいさつが入ります。
    - **Anything else**: ユーザーの入力が認識されないときにユーザーへの応答に使用するフレーズが入ります。

    ![Welcome ノードと Anything else ノードからなる対話ツリーを表示](images/gs-add-dialog-node-animated-cover.png)
1.  **Welcome** ノードをクリックして、編集ビューで開きます。
1.  デフォルトの応答を、`Conversation チュートリアルへようこそ!` というテキストに置き換えます。

    ![編集ビューで開いた Welcome ノードを表示](images/gs-edit-welcome-node.png)
1.  ![Close](images/close.png) をクリックして、編集ビューを閉じます。

`welcome` 条件がトリガーとなる対話ノードを作成しました。この条件は、ユーザーが新規会話を開始したことを示す特別な条件です。このノードは、新規会話が開始されたらシステムがウェルカム・メッセージを使用して応答するよう指定しています。

### 開始ノードのテスト

いつでも対話をテストして、対話を検証することができます。これからテストします。

- ![Ask Watson](images/ask_watson.png) アイコンをクリックして、「Try it out」ペインを開きます。作成したウェルカム・メッセージが表示されるはずです。

    ![対話ノードのテスト](images/gs-tryitout-welcome-node.png)

### インテントを処理するノードの追加

`Welcome` ノードと `Anything else` ノードの間に、インテントを処理するためのノードを追加します。

1.  **Welcome** ノードの詳細アイコン ![詳細オプション](images/kabob.png) をクリックし、**「Add node below」**を選択します。
1.  このノードの **「Enter a condition」**フィールドに `#hello` と入力します。それから、**#hello** オプションを選択します。
1.  応答として `良い一日を。` を追加します。
1.  ![Close](images/close.png) をクリックして、編集ビューを閉じます。

   ![ユーザーが hello ノードを対話に追加するアニメーションを表示。](images/gs-add-dialog-node-animated.gif)
1.  このノードの詳細アイコン ![詳細オプション](images/kabob.png) をクリックし、**「Add node below」**を選択してピア・ノードを作成します。ピア・ノードで、`#goodbye` を条件に指定し、`またお会いしましょう!` を応答に指定します。

    ![インテントのノードを追加](images/gs-add-dialog-nodes-result.png)

### インテントの認識のテスト

hello 入力と goodbye 入力の両方を認識して応答する、単純な対話を作成しました。これから、正しく機能することを確認します。

1.  ![Ask Watson](images/ask_watson.png) アイコンをクリックして、「Try it out」ペインを開きます。例のウェルカム・メッセージが表示されます。
1.  ペインの下部で `こんにちは` と入力し、Enter キーを押します。#hello インテントが認識されたことが出力に示され、該当する応答 (`良い一日を。`) が表示されます。
1.  以下の入力を試してください。
    - `バイバイ`
    - `やあ`
    - `またね`
    - `おはよう`
    - `さよなら`

   ![ユーザーが「Try it out」パネルで対話をテストするアニメーションを表示](images/gs-test-dialog-animated.gif)

{{site.data.keyword.watson}} は、入力内容が、提供されていたサンプルと完全に一致していなくても、インテントを認識できます。対話ではインテントにより、使用される厳密な言葉遣いに関係なくユーザーの入力の目的を識別します。そして、指定された方法で応答します。

### 対話の作成の結果

これで完了です。2 つのインテントとそれらを認識する対話からなる、単純な会話を作成しました。

## ステップ 5: サンプル・ワークスペースを確認する
{: #review-sample-workspace}

サンプル・ワークスペースを開いて、先ほど作成したのと同様のインテントや、その他多数のインテントを参照し、それらが複雑な対話でどのように使用されているかを確認します。

1.  「Workspaces」ページに戻ります。ナビゲーション・メニューから ![「Back to workspaces」ボタン](images/workspaces-button.png) ボタンをクリックすることができます。
1.  **「Car Dashboard - Sample」**ワークスペース・タイルで、**「Edit sample」**ボタンをクリックします。

    ![「Workspaces」ページで車のダッシュボード・サンプルを表示](images/gs-workspace-car-sample.png)

## 次の作業
{: #next-steps}

ここで示したチュートリアルは、単純なサンプルで作成されています。実際のアプリケーションの場合は、より興味深いインテント、いくつかのエンティティー、より複雑な対話を定義する必要があります。

- 上級の[チュートリアル](tutorial.html)を試用して、エンティティーを追加し、ユーザーの目的を明確化します。
- ワークスペースをフロントエンド・ユーザー・インターフェース、ソーシャル・メディア、またはメッセージング・チャネルに接続して[デプロイ](deploy.html)します。
- [サンプル・アプリ](sample-applications.html)をチェックアウトします。
