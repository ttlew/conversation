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

# 建置用戶端應用程式

您具有一個工作中對話。現在要開發與使用者互動並與 {{site.data.keyword.conversationfull}} 服務通訊的應用程式。
{: shortdesc}

本指導教學中的程式碼範例是使用 Node.js Watson SDK 以 JavaScript 撰寫的，但您可以使用其他語言。請安裝您的程式設計語言所適用的 Watson [SDK ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}，並檢閱 SDK 文件以取得相關資訊。
{: tip}

## 設定 Conversation 服務

我們在本節建立的範例應用程式會實作認知個人助理的數個功能。應用程式碼將連接至執行認知處理（例如偵測使用者目的）的 {{site.data.keyword.conversationshort}} 工作區。

繼續此範例之前，您需要設定必要的 {{site.data.keyword.conversationshort}} 工作區：

1.  下載 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON 檔案</a>工作區。
1.  [匯入工作區](configure-workspace.html#creating-workspaces)至 {{site.data.keyword.conversationshort}} 服務實例。

## 取得服務資訊

若要存取 {{site.data.keyword.conversationshort}} 服務 REST API，您的應用程式需要可以向 {{site.data.keyword.Bluemix}} 進行鑑別，並連接至正確的 {{site.data.keyword.conversationshort}} 工作區。您需要複製服務認證及工作區 ID，並將它們貼入至應用程式碼。

若要從工作區中存取服務認證及工作區 ID，請選取 ![功能表](images/Menu_16.png) 功能表，選擇**部署**，然後移至**認證**標籤。

您也可以從 Bluemix 儀表板中存取服務認證。

## 與 Conversation 服務通訊

與 {{site.data.keyword.conversationshort}} 服務互動十分簡單。請查看用於連接至服務、傳送單一訊息，以及將輸出記載到主控台的 Node.js 範例：

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

第一個步驟是建立 {{site.data.keyword.conversationshort}} 服務的封套。

封套是一個物件，您將用來將輸入傳送至服務並接收來自服務的輸出。在建立服務封套時，請指定來自服務金鑰的鑑別認證，以及所使用的 {{site.data.keyword.conversationshort}} API 版本。

在此 Node.js 範例中，封套是儲存在變數 `conversation` 的 `ConversationV1` 實例。其他語言的 Watson SDK 提供用於實例化服務封套的對等機制。

建立服務封套之後，我們會用它來傳送訊息至 {{site.data.keyword.conversationshort}} 服務。在此範例中，訊息是空的；我們只要在對話中觸發 conversation_start 節點，因此不需要任何輸入文字。

使用 `node <filename.js>` 指令，以執行範例應用程式。

**附註：**請確定您已使用 `npm install watson-developer-cloud` 來安裝 Node.js Watson SDK。

假設所有作業都如預期般運作，則 {{site.data.keyword.conversationshort}} 服務會傳回對話的輸出，然後將其記載到主控台：

```
Welcome to the Conversation example!
```
{: screen}

此輸出告訴我們，我們已順利與 {{site.data.keyword.conversationshort}} 服務通訊，並在對話中接收到 conversation_start 節點所指定的歡迎訊息。我們現在可以新增使用者介面，將它用來處理使用者輸入。

## 處理使用者輸入以偵測目的

為了可以處理使用者輸入，我們需要將使用者介面新增至應用程式。在此範例中，我們將簡化作業，並使用標準輸入及輸出。我們可以使用 Node.js 提示同步模組來執行這項作業（您可以使用 `npm install prompt-sync` 來安裝提示同步模組）。

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

此版本應用程式的開始方式像之前一樣：將空白訊息傳送至 {{site.data.keyword.conversationshort}} 服務以開始交談。

`processResponse()` 函數現在會顯示對話所偵測到的任何目的以及輸出文字，然後提示輸入下一輪的使用者輸入。

但仍有問題：

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

{{site.data.keyword.conversationshort}} 服務偵測到正確的目的，但每次交談都會傳回來自 conversation_start 節點的歡迎訊息 (`Welcome to the Conversation example!`)。

發生此情況是因為 {{site.data.keyword.conversationshort}} 服務為無狀態；應用程式會負責維護狀態資訊。因為我們尚未執行任何動作來維護狀態，所以 {{site.data.keyword.conversationshort}} 服務會將每一輪的使用者輸入都視為新交談的第一次，而觸發 conversation_start 節點。

## 維護狀態

交談的狀態資訊是使用*環境定義* 進行維護。環境定義是在應用程式與 {{site.data.keyword.conversationshort}} 服務之間來回傳遞的 JSON 物件。應用程式負責維護從某次交談到下一次交談的環境定義。

環境定義包括每一個使用者交談的唯一 ID，以及每一次交談時遞增的計數器。舊版範例不會保留環境定義，這表示每一輪的輸入都會是新交談的開始。修正這種情況的方法是儲存環境定義，並且每一次都將它送回至 {{site.data.keyword.conversationshort}} 服務。

除了在交談中維護我們的位置之外，環境定義還可以用來儲存您要在應用程式與 {{site.data.keyword.conversationshort}} 服務之間來回傳遞的任何其他資料。這可以包括您要在整個交談中維護的持續資料（例如客戶的名稱或帳號），或您要追蹤的任何其他資料（例如選項設定的現行狀態）。

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

前一個範例的唯一變更是，在每一輪的交談中，我們現在會送回在前一輪中收到的 `response.context` 物件：

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}

這可確保將環境定義從某一次維護到下一次，因此，{{site.data.keyword.conversationshort}} 服務不會再認為每一次都是第一次：

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

我們現在已有所進展！{{site.data.keyword.conversationshort}} 服務會正確地辨識我們的目的，且對話會傳回每一個目的的正確輸出文字（如果提供的話）。

不過，不會發生任何其他情況。當我們詢問時間時，未收到任何回答；當我們說再見時，並未結束交談。這是因為那些目的需要應用程式採取其他動作。

## 實作應用程式動作

除了要向使用者顯示的輸出文字之外，我們的對話還會使用回應 JSON 中的 `output` 物件，根據偵測到的目的來發出應用程式需要執行動作的信號。

這些動作旗標是使用 `action` 內容進行傳送，在其中，我們的對話定義為回應 JSON 的一部分。對話在判斷應用程式需要執行某項作業時會將 `action` 值設為適當的值，可以是 `display_time` 或 `end_conversation`。

請謹記，`output` 只是 JSON 物件，您可以在其中新增任何想要的有效內容。針對較複雜的應用程式，您可以使用具有多個動作旗標的陣列。

但在我們的範例中，我們使用的是支援單一動作旗標的簡單鍵值組。我們的應用程式碼需要檢查回應中的 `action` 內容值，然後執行任何指定的動作（此版本也會移除偵測到之目的的顯示，現在我們確定將會正確地識別這些目的）。

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

processResponse() 函數現在會檢查接收自 {{site.data.keyword.conversationshort}} 服務之 `output` 物件的 `action` 內容值。如果值是 `display_time` 或 `end_conversation`，應用程式會執行適當的動作。

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

成功！應用程式現在會使用 {{site.data.keyword.conversationshort}} 服務來識別自然語言輸入中的目的、顯示適當的回應，以及實作所要求的動作。

當然，實際的應用程式會使用更複雜的使用者介面（例如網路聊天視窗）。它會實作更複雜的動作，並可能會與客戶資料庫或其他商務系統整合。但是，應用程式如何與 {{site.data.keyword.conversationshort}} 服務互動的基本原則仍會相同。

如需一些更複雜的範例，請查看「導覽」窗格中的「範例」應用程式。
