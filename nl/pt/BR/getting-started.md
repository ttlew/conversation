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

# Tutorial de introdução
{: #gettingstarted}

Neste tutorial curto, introduzimos a ferramenta do {{site.data.keyword.conversationshort}} e passamos pelo processo de criação de sua primeira conversa.
{: shortdesc}

## Antes de iniciar
{: #prerequisites}

Se você já criou uma instância de serviço, você está pronto com esses pré-requisitos. Vá para a Etapa 1.
{: tip}

1.  Acesse o serviço [{{site.data.keyword.conversationshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/catalog/services/conversation/){: new_window} e inscreva-se para uma conta grátis do {{site.data.keyword.Bluemix_notm}} ou efetue login.
1.  Depois de efetuar login, digite `conversation-tutorial` no campo **Nome do Serviço** da página {{site.data.keyword.conversationshort}} e clique em **Criar**.

## Etapa 1: Ativar a ferramenta
{: #launch-tool}

Depois de criar a instância de serviço, você entrará no painel da instância. Ative a ferramenta {{site.data.keyword.conversationshort}} desde aqui.

Clique em **Gerenciar**, em seguida **Ativar ferramenta**.

![Mostra a página IBM Bluemix Watson Manage em que você ativa a ferramenta](images/gs-launch-tool.png)

Você pode ser solicitado a efetuar login na ferramenta separadamente. Nesse caso, forneça as credenciais do IBM Bluemix para efetuar login.

## Etapa 2: Criar uma área de trabalho
{: #create-workspace}

Sua primeira etapa na ferramenta {{site.data.keyword.conversationshort}} é criar uma área de trabalho.

Uma [*área de trabalho*](configure-workspace.html) é um contêiner para os artefatos que define o fluxo de conversa.

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique em **Criar**.
1.  Dê à sua área de trabalho o nome `Conversation tutorial` e clique em **Criar**. Você entrará na guia **Intenções** de sua nova área de trabalho.

![Mostra uma animação de um usuário criando uma área de trabalho do tutorial de Conversa.](images/gs-create-workspace-animated.gif)

## Etapa 3: Criar intenções
{: #create-intents}

Uma [intenção](intents.html) representa o propósito de uma entrada do usuário. É possível pensar em intenções como as ações que seus usuários podem querer executar com seu aplicativo.

Para este exemplo, vamos manter as coisas simples e definir apenas duas intenções: uma para dizer olá e uma para dizer adeus.

1.  Certifique-se de estar na guia Intenções. (Você já deve estar lá se você acabou de criar a área de trabalho.)
1.  Clique em **Criar novo**.
1.  Nomeie a intenção `hello`.
1.  Digite `hello` como um **Exemplo do usuário** e pressione Enter.

   *Exemplos* informam ao serviço {{site.data.keyword.conversationshort}} quais tipos de entrada do usuário você deseja corresponder à intenção. Quanto mais exemplos você fornecer, mais preciso o serviço pode ser em reconhecer as intenções dos usuários.
1.  Inclua mais quatro exemplos e clique em **Concluído** para concluir a criação da intenção #hello:
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

   ![Mostra uma animação de um usuário criando uma intenção #hello com elocuções de exemplo.](images/gs-add-intents-animated.gif)

1.  Crie outra intenção chamada #goodbye com esses cinco exemplos:
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

Você criou duas intenções, #hello e #goodbye e forneceu a entrada do usuário de exemplo para treinar o {{site.data.keyword.watson}} para reconhecer essas intenções na entrada de seus usuários.

![Mostra a página Intenções listando as intenções #goodbye e #hello](images/gs-add-intents-result.png)

## Etapa 4: Construir um diálogo
{: #build-dialog}

Um [diálogo](dialog-build.html) define o fluxo de sua conversa na forma de uma árvore lógica. Cada nó da árvore tem uma condição que o aciona, com base na entrada do usuário.

Vamos criar um diálogo simples que manipula nossas intenções #hello e #goodbye, cada uma com um nó único.

### Incluindo um nó inicial

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique na guia **Diálogo**.
1.  Clique em **Criar**. Você verá dois nós:
    - **Bem-vindo**: contém uma saudação que é exibida para os usuários quando eles se relacionam com o robô inicialmente.
    - **Qualquer outra coisa**: contém frases que são usadas para responder aos usuários quando a entrada deles não é reconhecida.

    ![Mostra a árvore de diálogo com os nós Bem-vindo e Qualquer outra coisa](images/gs-add-dialog-node-animated-cover.png)
1.  Clique no nó **Bem-vindo** para abri-lo na visualização de edição.
1.  Substitua a resposta padrão com o texto `Welcome to the Conversation tutorial!`.

    ![Mostra o nó Bem-vindo aberto na visualização de edição](images/gs-edit-welcome-node.png)
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

Você criou um nó diálogo que é acionado pela condição `welcome`, que é uma condição especial que indica que o usuário iniciou uma nova conversa. Seu nó especifica que quando uma nova conversa inicia, o sistema deve responder com a mensagem de boas-vindas.

### Testando o nó inicial

É possível testar seu diálogo a qualquer momento para verificar o diálogo. Vamos testá-lo agora.

- Clique no ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela "Experimente". Você deverá ver sua mensagem de boas-vindas.

    ![Testando o nó de diálogo](images/gs-tryitout-welcome-node.png)

### Incluindo nós para manipular intenções

Agora vamos incluir nós para manipular nossas intenções entre o nó `Welcome` e o nó `Anything else`.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **Bem-vindo** e, em seguida, selecione **Incluir nó abaixo**.
1.  Digite `#hello` no campo **Insira uma condição** desse nó. Em seguida, selecione a opção **#hello**.
1.  Inclua a resposta, `Good day to you.`
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

   ![Mostra uma animação de um usuário incluindo um nó hello para o diálogo.](images/gs-add-dialog-node-animated.gif)
1.  Clique no ícone Mais ![Mais opções](images/kabob.png) nesse nó e, em seguida, selecione **Incluir nó abaixo** para criar um nó de mesmo nível. No nó de mesmo nível, especifique `#goodbye` como a condição, e `OK. See you later!` como a resposta.

    ![Incluindo nós para intenções](images/gs-add-dialog-nodes-result.png)

### Testando o reconhecimento de intenção

Você construiu um diálogo simples para reconhecer e responder a ambas as entradas, olá e adeus. Vamos ver o quão bem ele funciona.

1.  Clique no ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela "Experimente". Há aquela mensagem de boas-vindas tranquilizadora.
1.  Na parte inferior da área de janela, digite `Hello` e pressione Enter. A saída indica que a intenção #hello foi reconhecida e a resposta apropriada (`Good day to you.`) aparece.
1.  Tente a seguinte entrada:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

   ![Mostra uma animação do usuário testando o diálogo no painel Experimente](images/gs-test-dialog-animated.gif)

O {{site.data.keyword.watson}} pode reconhecer suas intenções mesmo quando sua entrada não corresponde exatamente aos exemplos incluídos. O diálogo utiliza intenções para identificar o propósito da entrada do usuário independentemente do texto exato usado e então responde da maneira que você especificar.

### Resultado da construção de um diálogo

É isso. Você criou uma conversa simples com duas intenções e um diálogo para reconhecê-las.

## Etapa 5: Revisar a área de trabalho de amostra
{: #review-sample-workspace}

Abra a área de trabalho de amostra para ver as intenções semelhantes àquelas que você acabou de criar e muitas mais, e ver como elas são usadas em um diálogo complexo.

1.  Volte para a página Áreas de Trabalho.
   É possível clicar no botão ![Botão voltar para as áreas de trabalho](images/workspaces-button.png) no menu de navegação.
1.  No quadro da área de trabalho **Painel do carro - Amostra**, clique no botão **Editar amostra**.

    ![Mostra o quadro amostra de painel do carro na página Áreas de trabalho](images/gs-workspace-car-sample.png)

## O que fazer em seguida
{: #next-steps}

Este tutorial é construído em torno de um exemplo simples. Para um aplicativo real, você precisará definir algumas intenções mais interessantes, algumas entidades e um diálogo mais complexo.

- Tente o [tutorial](tutorial.html) avançado para incluir entidades e esclarecer o propósito de um usuário.
- [Implemente](deploy.html) sua área de trabalho conectando-a a uma interface com o usuário de front-end, mídia social ou um canal de mensagens.
- Veja os [aplicativos de amostra](sample-applications.html).
