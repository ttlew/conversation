---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-25"

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

# Definindo intenções

***Intenções*** são propósitos ou objetivos expressos em uma entrada do cliente, como responder a uma pergunta ou processar um pagamento de conta. Ao reconhecer a intenção expressa na entrada de um cliente, o serviço {{site.data.keyword.conversationshort}} pode escolher o fluxo de diálogo correto para responder a isso.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VG0YykNcfv8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Limites de intenção
{: #intent-limits}

O número de intenções e exemplos que você pode criar depende de seu plano de serviço {{site.data.keyword.conversationshort}}:

| Plano de Serviço | Intenções por área de trabalho | Exemplos por área de trabalho |
|------------------|----------------------:|-----------------------:|
| Standard/Premium |                 2.000 |                 25.000 |
| Lite             |                    25 |                 25.000 |

## Criando intenções
{: #creating-intents}

Use a ferramenta {{site.data.keyword.conversationshort}} para criar intenções.

1.  Na ferramenta do {{site.data.keyword.conversationshort}}, abra a sua área de trabalho e, em seguida, selecione a guia **Intenções** na barra de navegação. Se **Intenções** não estiver visível, use o menu ![Menu](images/Menu_16.png) para abrir a página.
1.  Selecione **Criar novo**.
1.  No campo **Nome da intenção**, digite um nome descritivo para a intenção.
    - O nome de intenção pode conter letras (em Unicode), números, sublinhados, hifens e pontos.
    - O nome não pode ser composto de `..` ou qualquer outra cadeia de somente pontos.
    - Os nomes das intenções não podem conter espaços e não devem exceder 128 caracteres. A seguir estão exemplos de nomes de intenção:
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    O conjunto de ferramentas inclui automaticamente o caractere `#` nos nomes de intenção, então você não precisa incluir um.
    {: tip}

    É possível selecionar **Criar** para salvar seu nome de intenção sem incluir exemplos. Também é possível selecionar o campo exemplo do usuário ou usar a tecla tab para avançar e incluir exemplos.

1.  No campo **Exemplo do usuário**, digite o texto de um exemplo do usuário para a intenção. Um exemplo pode ser qualquer sequência de até 1024 caracteres de comprimento. Os seguintes podem ser exemplos para a intenção `#pay_bill`:
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    Se você tiver definido ou planeja definir entidades que correspondem a esta intenção, consulte as entidades ou seus sinônimos associados em alguns dos exemplos. Fazer isso ajuda a estabelecer um relacionamento entre a intenção e as entidades.

    > **Importante:** os nomes das intenções e o texto de exemplo podem ser expostos em URLs quando um aplicativo interage com o serviço. Não inclua informações sensíveis ou pessoais nestes artefatos.

    Pressione Enter ou selecione **+** para salvar o exemplo.
1.  Repita o mesmo processo para incluir mais exemplos. É possível usar a tecla tab entre cada exemplo. Forneça pelo menos 5 exemplos para cada intenção. Quanto mais exemplos você fornecer, mais preciso seu aplicativo pode ser.

    ![Captura de tela mostrando a definição de intenção](images/define_intent.png)
1.  Quando você tiver concluído a inclusão de exemplos, selecione **Pronto** para concluir a criação da intenção.

### Resultados

A intenção que você criou é incluída na guia Intenções e o sistema começa a treinar a si mesmo com os novos dados.

## Editando intenções

É possível selecionar qualquer intenção na lista para abri-la para edição. É possível fazer as seguintes mudanças:

- Renomear a intenção.
- Excluir a intenção.
- Incluir, editar ou excluir exemplos.
- Mover um exemplo para uma intenção diferente.

É possível usar a tecla tab do nome da intenção para cada exemplo, editando os exemplos, se você escolher.

Para mover um exemplo, selecione o exemplo selecionando a caixa de seleção e, em seguida, selecione **Mover para**.

  ![Captura de tela mostrando como mover um exemplo](images/move_example.png)

## Importando intenções e exemplos

Se você tiver um grande número de intenções e exemplos, você pode achar mais fácil importá-los de um arquivo CSV (Valor Separado por Vírgula) do que defini-los um por um na ferramenta {{site.data.keyword.conversationshort}}.

1.  Colete as intenções e exemplos em um arquivo CSV ou exporte-os de uma planilha para um arquivo CSV. O formato obrigatório para cada linha no arquivo é o seguinte:

    ```
    <example>,<intent>
    ```
    {: screen}

    em que `<example>` é o texto de um exemplo do usuário e `<intent>` é o nome da intenção que você deseja que corresponda ao exemplo. Por exemplo:

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    > **Importante:** Salve o arquivo CSV com codificação UTF-8 e nenhuma marca de ordem de byte (BOM).

1.  Na ferramenta do {{site.data.keyword.conversationshort}}, abra a sua área de trabalho e, em seguida, selecione a guia **Intenções** na barra de navegação. Se **Intenções** não estiver visível, use o menu ![Menu](images/Menu_16.png) para abrir a página.

1.  Selecione ![Importar](images/importGA.png) e, em seguida, arraste um arquivo ou navegue para selecionar um arquivo de seu computador. O arquivo é validado e importado e o sistema começa a treinar com os novos dados.

    > **Importante:** O tamanho máximo do arquivo CSV é 10MB. Se o seu arquivo CSV for maior, considere dividi-lo em múltiplos arquivos e importá-los separadamente.

### Resultados

É possível visualizar as intenções importadas e os exemplos correspondentes na guia Intenções. Você pode precisar atualizar a página para ver as novas intenções e exemplos.

## Exportando intenções
{: #export_intents}

É possível exportar um número de intenções para um arquivo CSV, você pode então importá-las e reutilizá-las para outro aplicativo de Conversação.

1.  Na guia Intenções, selecione ![Ícone exportar](images/ExportIcon.png)

    ![Opções de Exportação e Exclusão](images/ExportIntent1.png)

1.  Selecione as intenções desejadas e clique no botão **Exportar**.
![Botões de seleção de entidade para Exportação e Exclusão](images/ExportIntent2.png)

## Excluindo intenções
{: #delete_intents}

É possível selecionar várias intenções para exclusão.

**IMPORTANTE**: ao excluir intenções, você também exclui todos os exemplos associados e esses itens não podem ser recuperados posteriormente. Todos os nós de diálogo que fazem referência a essas intenções devem ser atualizados manualmente para que não façam referencia ao conteúdo excluído.

1.  Na guia Intenções, selecione ![Ícone excluir](images/DeleteIcon.png)

    ![Opções de Exportação e Exclusão](images/ExportIntent3.png)

1.  Selecione as intenções que você deseja excluir e clique no botão **Excluir**. **Nota**: O recurso de exclusão suporta a exclusão em massa de intenções.

## Testando suas intenções
{: #testing-your-intents}

Depois de ter concluído a criação de novas intenções, é possível testar o sistema para ver se ele reconhece suas intenções como você espera.

1.  Na ferramenta {{site.data.keyword.conversationshort}}, selecione o ícone ![Pergunte ao Watson](images/ask_watson.png).

1.  No painel Experimente, insira uma pergunta ou outra cadeia de texto e pressione Enter para ver qual intenção é reconhecida. Se a intenção incorreta é reconhecida, você pode melhorar seu modelo incluindo esse texto como um exemplo para a intenção correta.

    Se tiver feito mudanças recentemente em sua área de trabalho, você poderá ver uma mensagem indicando que o sistema ainda está treinando. Se você vir essa mensagem, aguarde até que o treinamento seja concluído antes de testar:
    {: tip}

    ![Captura de tela mostrando mensagem de treinamento em andamento](images/training.png)

    A resposta indica qual intenção foi reconhecida de sua entrada.

    ![Captura de tela de intenções de teste](images/test_intents.png)

1.  Se o sistema não reconhecer a intenção correta, será possível corrigi-lo. Para corrigir a intenção reconhecida, selecione a intenção exibida e, em seguida, selecione a intenção correta na lista. Após o envio de sua correção, o sistema inicia automaticamente um novo treinamento para incorporar os novos dados.

    ![Captura de tela da correção de uma intenção reconhecida](images/correct_intent.png)

1.  Se a entrada não estiver relacionada ao seu aplicativo, será possível indicar isso. Selecione a intenção exibida e escolha **Marcar como irrelevante**.

    ![Captura de tela marcar como irrelevante](images/irrelevant.png)

Se suas intenções não estão sendo corretamente reconhecidas, considere fazer os seguintes tipos de mudanças:

- Inclua o texto não reconhecido como um exemplo para a intenção correta.
- Mova exemplos existentes de uma intenção para outra.
- Considere se suas intenções são muito semelhantes e redefina-as conforme apropriado.

## Pontuação absoluta e Marcar como irrelevante

Desde fevereiro de 2017, há um novo algoritmo para pontuar a confiança da intenção e retornar intenções. É possível também marcar entradas como "irrelevantes". Essas mudanças podem requerer que você [atualizar para sua área de trabalho ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](upgrading.html){: new_window}.

### Pontuação absoluta

Agora pontuamos cada confiança da intenção nela mesma, não em relação a outras intenções. Isso permite a flexibilidade de ter diversas intenções retornadas. Também significa que o sistema não pode retornar uma intenção de modo algum. Se o sistema tiver baixa confiança (menos de 0,2) que qualquer intenção relacione à entrada do usuário, o sistema não retornará uma intenção.

Conforme as pontuações de confiança da intenção mudam, seus diálogos podem precisar de reestruturação. Por exemplo, se você condicionou seu diálogo com uma intenção que agora tem baixa confiança, a resposta do sistema não será mais correta.

### Marcar como irrelevante
{: #mark-irrelevant}

É possível consultar [idiomas suportados](lang-support.html) para verificar a disponibilidade deste recurso.

Depois de atualizar sua área de trabalho, é possível [testar entrada](#testing-your-intents) no painel Experimente para ver as mudanças. É possível usar "Marcar como irrelevante" para indicar que a entrada não está relacionada ao seu aplicativo.

Se você tiver uma intenção, como #off_topic, para aquelas entradas que estão fora do escopo ou tópico, exclua a intenção e teste sua área de trabalho marcando que as entradas são irrelevantes.

> **Importante**: Entradas que são marcadas como irrelevantes são armazenadas na área de trabalho e são incluídas como parte dos dados de treinamento. Certifique-se de que você deseja fazer essa mudança.
> - As entradas não podem ser acessadas ou mudadas posteriormente no conjunto de ferramentas.
> - A única maneira de remover a identificação "Irrelevante" é usar a mesma entrada no painel Experimente e, em seguida, mudar a intenção.
