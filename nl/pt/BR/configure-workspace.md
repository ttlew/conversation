---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-17"

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

# Configurando uma área de trabalho de Conversa

O processamento de língua natural para o serviço {{site.data.keyword.conversationshort}} acontece dentro de uma *área de trabalho*, que é um contêiner para todos os artefatos que definem o fluxo de conversa para um aplicativo.
{: shortdesc}

Uma única instância de serviço {{site.data.keyword.conversationshort}} pode conter várias áreas de trabalho. Uma área de trabalho contém os seguintes tipos de artefatos:

- [**Intenções**](intents.html): uma *intenção* representa o propósito de uma entrada do usuário, como uma pergunta sobre os locais de negócios ou um pagamento de fatura. Você define uma intenção para cada tipo de solicitação do usuário que você quer que seu aplicativo suporte. Na ferramenta, o nome de uma intenção é sempre prefixado com o caractere `#`. Para treinar a área de trabalho para reconhecer suas intenções, você fornece vários exemplos de entrada do usuário e indica para quais intenções elas são mapeadas.
- [**Entidades**](entities.html); Uma *entidade* representa um termo ou objeto que é relevante para suas intenções e que fornece um contexto específico para uma intenção. Por exemplo, uma entidade pode representar uma cidade na qual o usuário deseja localizar um local de negócios ou a quantia de um pagamento de fatura. Na ferramenta, o nome de uma entidade é sempre prefixado com o caractere `@`. Para treinar a área de trabalho para reconhecer suas entidades, você lista os valores possíveis para cada entidade e sinônimos que os usuários podem inserir.
- [**Diálogo**](dialog-build.html): Um *diálogo* é um fluxo de conversa de ramificação que define como seu aplicativo responde quando ele reconhece as intenções e entidades definidas. Você usa o construtor de diálogo na ferramenta para criar conversas com usuários, fornecendo respostas com base nas intenções e entidades que você reconhece na entrada deles.

Conforme você inclui informações, a área de trabalho treina sozinha, portanto, não é preciso fazer nada para iniciar o treinamento.

## Limites da área de trabalho
{: #workspace-limits}

O número de áreas de trabalho que você pode criar em uma única instância de serviço depende do seu plano de serviço {{site.data.keyword.conversationshort}}. A área de trabalho de amostra não conta para seu limite de áreas de trabalho, a menos que você edite ou duplique a amostra:

| Plano de Serviço | Áreas de trabalho por instância de serviço |
|------------------|--------------------------------:|
| Standard/Premium |                              20 |
| Lite*            |                               5 |

*Depois de 30 dias de inatividade, uma área de trabalho Lite não usada pode ser excluída para liberar espaço.

## Criando áreas de trabalho
{: #creating-workspaces}

É possível criar uma área de trabalho do início, usar a área de trabalho de amostra fornecida ou importar uma área de trabalho de um arquivo JSON. Também é possível duplicar uma área de trabalho existente dentro da mesma instância de serviço.

Você usa a ferramenta {{site.data.keyword.conversationshort}} para criar áreas de trabalho. Siga essas etapas para criar uma área de trabalho:

1.  Se a página "Detalhes do Serviço" não estiver aberta, clique em sua instância de serviço do {{site.data.keyword.conversationshort}} no console. (Quando você cria uma instância de serviço, a página "Detalhes do Serviço" é exibida.)

1.  Na página "Detalhes do Serviço", role para baixo para **Conjunto de ferramentas de conversa** e clique em **Ativar ferramenta**.

1.  Execute um dos seguintes procedimentos:
    - Para criar uma área de trabalho do início, clique em **Criar**.
    - Para usar a amostra como um ponto de início para sua própria área de trabalho, edite a área de trabalho de amostra. Se desejar usá-la para múltiplas áreas de trabalho, duplique-a como alternativa.
    - Para importar uma área de trabalho de um arquivo JSON, clique no ícone ![Importar área de trabalho](images/workspace_import.png) e selecione o arquivo JSON do qual você deseja importar.

        **Importante:**

        - O arquivo JSON importado deve usar codificação UTF-8.
        - O tamanho máximo para um arquivo JSON de área de trabalho é 10MB. Se você precisar importar uma área de trabalho maior, considere importar as intenções e entidades separadamente depois de ter importado a área de trabalho. (Também é possível importar áreas de trabalho maiores usando a API de REST. Para obter informações adicionais, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
        - O JSON não pode conter guias, novas linhas ou retornos de linha.

        Especifique os dados que você deseja incluir:

        - Selecione **Tudo (Intenções, Entidades e Diálogo)** se desejar importar uma cópia completa da área de trabalho exportada, incluindo o diálogo.
        - Selecione **Intenções e Entidades** se desejar usar as intenções e entidades da área de trabalho exportada, mas você planeja construir um novo diálogo.

1.  Especifique os detalhes para a nova área de trabalho:
    - **Nome**: um nome com não mais de 64 caracteres de comprimento. Esse valor é necessário.
    - **Descrição**: uma descrição com não mais de 128 caracteres de comprimento.
    - **Idioma**: o idioma da entrada do usuário que a área de trabalho será treinada para entender.

Depois de criar a área de trabalho, ele aparece como um quadrado na página Áreas de Trabalho.

## Exportando e copiando áreas de trabalho
{: #exporting-and-copying-workspaces}

É possível usar a página Áreas de Trabalho para exportar áreas de trabalho e para copiar uma área de trabalho para uma nova área de trabalho.

A exportação de uma área de trabalho salva uma cópia de todos os dados da área de trabalho em um arquivo JSON. Use essa opção se você deseja importar a área de trabalho em uma instância de serviço diferente ou salvar uma cópia dos dados da área de trabalho off-line. Ao importar uma área de trabalho, é possível optar por importar apenas as intenções e entidades, o que pode ser útil se você deseja construir um novo diálogo usando os mesmos dados de treinamento.

A cópia de uma área de trabalho faz uma cópia completa da área de trabalho dentro da mesma instância de serviço. Use essa opção se você deseja adaptar uma área de trabalho existente enquanto preserva a versão original.

- Para exportar uma área de trabalho existente para um arquivo JSON, clique no ícone de menu no quadro da área de trabalho e, em seguida, selecione **Fazer download como JSON**:

    ![Captura de tela mostrando a opção de menu Fazer download como JSON](images/workspace_export.png)
- Para criar uma cópia de uma área de trabalho existente dentro da mesma instância de serviço, clique no ícone de menu no quadro da área de trabalho e, em seguida, selecione **Duplicar**:

    ![Captura de tela mostrando a opção de menu Duplicar](images/workspace_duplicate.png)

    Especifique o nome, a descrição e o idioma para a nova área de trabalho. Todos os dados (incluindo intenções, entidades e diálogo) são incluídos na cópia.

