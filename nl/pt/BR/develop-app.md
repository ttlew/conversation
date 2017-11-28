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

# Construindo um aplicativo cliente

Então você tem um diálogo de trabalho. Agora você deseja desenvolver o aplicativo que irá interagir com seus usuários e se comunicar com o serviço do {{site.data.keyword.conversationfull}}.
{: shortdesc}

Os exemplos de código neste tutorial são gravados em JavaScript usando o SDK Watson Node.js, mas você pode usar outras linguagens. Instale o Watson [SDK ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} para sua linguagem de programação e revise a documentação do SDK para obter mais informações.
{: tip}

## Configurando o serviço de Conversa

O aplicativo de exemplo que criaremos nesta seção implementa várias funções de um assistente pessoal cognitivo. O código do aplicativo irá se conectar a uma área de trabalho do {{site.data.keyword.conversationshort}}, em que o processamento cognitivo (como a detecção de intenções de usuário) ocorre.

Antes de continuar com este exemplo, é necessário configurar a área de trabalho do {{site.data.keyword.conversationshort}} necessária:

1.  Faça download do <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">arquivo JSON</a> da área de trabalho.
1.  [Importe a área de trabalho](configure-workspace.html#creating-workspaces) para uma instância do serviço do {{site.data.keyword.conversationshort}}.

## Obtendo informações de serviço

Para acessar as APIs REST do serviço do {{site.data.keyword.conversationshort}}, seu aplicativo precisa ser capaz de autenticar-se no {{site.data.keyword.Bluemix}} e de se conectar à área de trabalho correta do {{site.data.keyword.conversationshort}}. Será necessário copiar as credenciais de serviço e o ID da área de trabalho e colá-los no código do aplicativo.

Para acessar as credenciais de serviço e o ID da área de trabalho de sua área de trabalho, selecione o menu ![Menu](images/Menu_16.png), escolha **Implementar** e, em seguida, acesse a guia **Credenciais**.

Também é possível acessar as credenciais de serviço de seu painel do Bluemix.

## Comunicando-se com o serviço de Conversa

Interagir com o serviço do {{site.data.keyword.conversationshort}} é simples. Vamos dar uma olhada em um exemplo de Node.js que se conecta ao serviço, envia uma única mensagem e registra a saída no console:

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

A primeira etapa é criar um wrapper para o serviço do {{site.data.keyword.conversationshort}}.

O wrapper é um objeto que você usará para enviar entrada para o serviço e para receber a saída dele. Ao criar o wrapper de serviço, especifique as credenciais de autenticação da chave de serviço, bem como a versão da API do {{site.data.keyword.conversationshort}} que você está usando.

Neste exemplo de Node.js, o wrapper é uma instância do `ConversationV1`, armazenado na variável `conversation`. Os SDKs do Watson para outros idiomas fornecem mecanismos equivalentes para instanciar um wrapper de serviço.

Depois de criar o wrapper de serviço, nós o usamos para enviar uma mensagem para o serviço do {{site.data.keyword.conversationshort}}. Neste exemplo, a mensagem está vazia; nós só queremos acionar o nó conversation_start no diálogo, então não precisamos de nenhum texto de entrada.

Use o comando `node <filename.js>` para executar o aplicativo de exemplo.

**Nota:** certifique-se de que você tenha instalado o SDK Watson Node.js usando `npm install watson-developer-cloud`.

Supondo que tudo funciona conforme o esperado, o serviço do {{site.data.keyword.conversationshort}} retorna a saída do diálogo, que é então registrada no console:

```
Welcome to the Conversation example!
```
{: screen}

Essa saída nos informa que nos comunicamos com sucesso com o serviço do {{site.data.keyword.conversationshort}} e recebemos a mensagem de boas-vindas especificada pelo nó conversation_start no diálogo. Agora podemos incluir uma interface com o usuário, tornando possível processar a entrada do usuário.

## Processando a entrada do usuário para detectar intenções

Para podermos processar a entrada do usuário, precisamos incluir uma interface com o usuário em nosso aplicativo. Para este exemplo, vamos manter as coisas simples e usar a entrada e a saída padrão. Podemos usar o módulo de sincronização de prompt Node.js para fazer isso. (É possível instalar a sincronização de prompt usando `npm install prompt-sync`).

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

Esta versão do aplicativo começa da mesma maneira que antes: enviando uma mensagem vazia para o serviço do {{site.data.keyword.conversationshort}} para iniciar a conversa.

A função `processResponse ()` agora exibe qualquer intenção detectada pelo diálogo junto com o texto de saída e, então, solicita a próxima rodada de entrada do usuário.

Mas algo ainda não está certo:

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

O serviço do {{site.data.keyword.conversationshort}} é detectar as intenções corretas, no entanto, cada rodada da conversa retorna a mensagem de boas-vindas do nó conversation_start (`Welcome to the Conversation example!`).

Isso está acontecendo porque o serviço do {{site.data.keyword.conversationshort}} está sem estado; é responsabilidade do aplicativo manter as informações de estado. Como nós ainda não estamos fazendo nada para manter o estado, o serviço do {{site.data.keyword.conversationshort}} vê cada rodada de entrada do usuário como a primeira rodada de uma nova conversa, acionando o nó conversation_start.

## Mantendo o estado

As informações de estado para sua conversa são mantidas usando o *contexto*. O contexto é um objeto JSON que é transmitido e retornado entre seu aplicativo e o serviço do {{site.data.keyword.conversationshort}}. É responsabilidade de seu aplicativo manter o contexto de uma rodada de conversa para a próxima.

O contexto inclui um identificador exclusivo para cada conversa com um usuário, bem como um contador que é incrementado com cada rodada da conversa. A versão anterior do exemplo não preservava o contexto, o que significa que cada rodada de entrada parecia ser o início de uma nova conversa. Podemos consertar isso salvando o contexto e enviando-o de volta ao serviço do {{site.data.keyword.conversationshort}} toda vez.

Além de manter nossa posição na conversa, o contexto também pode ser usado para armazenar quaisquer outros dados que você deseje transmitir e receber entre seu aplicativo e o serviço do {{site.data.keyword.conversationshort}}. Isso pode incluir dados persistentes que você deseja manter durante a conversa (como nome ou o número da conta de um cliente) ou quaisquer outros dados que você deseje rastrear (como o status atual de configurações de opção).

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

A única mudança do exemplo anterior é que, com cada rodada da conversa, agora enviamos de volta o objeto `response.context` que recebemos na rodada anterior:

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}

Isso assegura que o contexto seja mantido de uma vez para a outra, para que o serviço do {{site.data.keyword.conversationshort}} não pense mais que cada vez é a primeira:

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

Agora estamos fazendo progresso! O serviço do {{site.data.keyword.conversationshort}} está reconhecendo corretamente nossas intenções e o diálogo está retornando o texto de saída correto (quando fornecido) para cada intenção.

No entanto, nada mais está acontecendo. Quando perguntamos o horário, ficamos sem resposta; e quando dizemos adeus, a conversa não termina. Isso ocorre porque essas intenções requerem que ações adicionais sejam executadas pelo aplicativo.

## Implementando ações do aplicativo

Além do texto de saída a ser exibido para o usuário, o nosso diálogo usa o objeto de `output` no JSON de resposta para sinalizar quando o aplicativo precisa realizar uma ação, com base nas intenções detectadas.

Essas sinalizações de ação são enviadas usando a propriedade `action`, que o nosso diálogo define como parte do JSON de resposta. Quando o diálogo determina que o aplicativo precisa fazer algo, ele configura o valor de `action` com o valor apropriado, `display_time` ou `end_conversation`.

Lembre-se de que `output` é apenas um objeto JSON e você pode incluir nele qualquer conteúdo válido que desejar. Para um aplicativo mais complexo, você pode usar uma matriz com várias sinalizações de ação.

Mas, em nosso exemplo, estamos usando um par de chave/valor simples que suporta uma única sinalização de ação. Nosso código do aplicativo precisa verificar o valor da propriedade `action` na resposta e, em seguida, executar qualquer ação especificada. (Essa versão também remove a exibição de intenções detectadas, agora que temos certeza de que elas estão sendo identificadas corretamente.)

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

A função processResponse() agora verifica o valor da propriedade `action` do objeto `output` recebido do serviço do {{site.data.keyword.conversationshort}}. Se o valor for `display_time` ou `end_conversation`, o aplicativo realizará a ação apropriada.

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

Êxito! O aplicativo agora usa o serviço do {{site.data.keyword.conversationshort}} para identificar as intenções na entrada da língua natural, exibe as respostas apropriadas e implementa as ações solicitadas.

Claro, um aplicativo real usará uma interface com o usuário mais sofisticada, como uma janela de bate-papo da web. E ele implementará ações mais complexas, possivelmente integrando com um banco de dados de clientes ou outros sistemas de negócios. Mas os princípios básicos de como o aplicativo interage com o serviço do {{site.data.keyword.conversationshort}} continuarão os mesmos.

Para obter alguns exemplos mais complexos, dê uma olhada nos aplicativos de Amostra na área de janela de navegação.
