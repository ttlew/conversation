---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-27"

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

# Notas sobre a liberação

## Versão da API de Serviço
{: shortdesc}

As solicitações de API requerem um parâmetro version que use uma data no formato `version=YYYY-MM-DD`. Sempre que mudamos a API de uma maneira incompatível com versões anteriores, lançamos uma nova versão secundária da API.

Envie o parâmetro version com cada solicitação de API. O serviço usa a versão da API para a data que você especificar ou a versão mais recente antes dessa data. Não padronize com a data atual. Em vez disso, especifique uma data que corresponda a uma versão que seja compatível com seu aplicativo e não a mude até que o aplicativo esteja pronto para uma versão mais recente.

A versão atual é `2017-05-26`.

## Recursos beta

A IBM irá liberar serviços, recursos e suporte ao idioma que são classificados como beta (anteriormente _experimental_). Esses serviços podem ser instáveis, podem mudar frequentemente e podem ser descontinuados mediante aviso com pouca antecedência. Os recursos Beta serão suportados apenas por meio do nosso fórum do {{site.data.keyword.Bluemix_notm}}.

## Modelos atualizados
{: #updated-models}

Os algoritmos do {{site.data.keyword.conversationshort}} podem ser refinados e atualizados periodicamente com base no feedback, em aprimoramentos científicos e em fatores adicionais, a fim de aprimorar continuamente o desempenho. Atualizações para o modelo serão comunicadas nessas notas sobre a liberação.

Os modelos existentes que você treinou não serão afetados imediatamente, mas os modelos expirados serão atualizados para o modelo atual, se você ainda não tiver feito isso, após 60 dias de um novo modelo se tornar disponível.

> **Nota:** esta instrução de atualização se aplica apenas aos idiomas e recursos Disponíveis de Modo Geral (GA).

## Mudanças
{: #change-log}

Os novos recursos e mudanças a seguir para o serviço estão disponíveis.

### 26 de outubro de 2017
{: #26October2017}

- **Atualizações para Chinês Simplificado** - o suporte ao idioma foi aprimorado para chinês simplificado. Isso inclui melhorias na classificação de intenção usando incorporações de palavras no nível de caractere e a disponibilidade de entidades do sistema. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](release-notes.html#updated-models) para obter mais informações.
- **Atualizações para Espanhol** - foram feitas melhorias na classificação da intenção em espanhol, para conjuntos de dados muito grandes.

### 11 de outubro de 2017
{: #11October2017}

- **Atualizações para Coreano** - o suporte ao idioma foi aprimorado para coreano. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](release-notes.html#updated-models) para obter mais informações.

### 3 de outubro de 2017
{: #3October2017}

- **Entidades definidas por padrão (Beta)**: agora é possível definir padrões específicos para uma entidade, usando expressões regulares. Isso pode ajudá-lo a identificar entidades que seguem um padrão definido, por exemplo de SKU ou números de peça, números de telefone ou endereços de e-mail. Consulte [Entidades definidas por padrão](entities.html#pattern-entities) para obter detalhes adicionais.
    - É possível incluir sinônimos ou padrões para um único valor de entidade; você não pode incluir ambos.
    - Para cada valor de entidade, pode haver um máximo de até 5 padrões.
    - Cada padrão (expressão regular) é limitado a 128 caracteres.
    - A importação ou exportação por meio de um arquivo CSV não suporta padrões atualmente.
    - A API de REST não suporta acesso direto aos padrões, mas é possível recuperar ou modificar padrões usando o terminal `/values`.

- **Correspondência difusa filtrada por dicionário (somente inglês)**: uma versão melhorada da correspondência difusa para entidades está agora disponível, para o inglês. Esta melhoria evita a captura de algumas palavras comuns válidas em inglês como correspondências difusas para uma determinada entidade. Por exemplo, a correspondência difusa não corresponderá o valor da entidade `like` a `hike` ou `bike`, que são palavras válidas em inglês, mas continuará correspondendo exemplos como `lkie` ou `oike`.

### 27 de setembro de 2017
{: #27September2017}

- **Atualizações do construtor de condição**: o controle que é exibido para ajudá-lo a definir uma condição em um nó de diálogo foi atualizado. Os aprimoramentos incluem suporte para listar nomes de variável de contexto disponíveis após você inserir o $ para começar a incluir uma variável de contexto.

### 31 de agosto de 2017
{: #31August2017}

- **Retrocesso da seção Melhorar** - a métrica do tempo médio de conversa e filtros correspondentes, estão sendo removidos temporariamente da página Visão Geral da seção Melhorar. Essa remoção evitará que o cálculo de determinadas métricas faça com que a métrica de tempo médio de conversa e o gráfico de conversas ao longo do tempo, exibam informações imprecisas. A IBM lamenta a remoção da funcionalidade da ferramenta, mas está comprometida em assegurar que informações precisas sejam comunicadas aos usuários.
- **Nomes do nó de diálogo** - agora é possível designar qualquer nome a um nó de diálogo; ele não precisa ser exclusivo. E será possível mudar posteriormente o nome do nó sem impactar a forma como o nó será referenciado internamente. O nome especificado é salvo como um atributo de título do nó no arquivo JSON da área de trabalho e o sistema usa um ID exclusivo que é armazenado no atributo de nome para referenciar o nó.

### 23 de agosto de 2017
{: #23August2017}

- **Atualizações para coreano, japonês e italiano** - O suporte ao idioma foi aprimorado para coreano, japonês e italiano. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](release-notes.html#updated-models) para obter mais informações.

### 10 de agosto de 2017
{: #10August2017}

- **Normalização do acento:** em uma configuração de conversação, os usuários podem ou não usar acentos enquanto interagem com o serviço de Conversa. Dessa forma, foi feita uma atualização no algoritmo para que as versões acentuadas e não acentuadas de palavras sejam tratadas da mesma forma para detecção de intenção e reconhecimento de entidade.

  No entanto, para alguns idiomas, como o espanhol, alguns acentos podem alterar o significado da entidade. Dessa forma, para a detecção de entidade, embora a entidade original possa ter implicitamente um acento, o serviço também poderá corresponder à versão não acentuada da mesma entidade, mas com uma pontuação de confiança ligeiramente mais baixa.

  Por exemplo, para a palavra "barrió", que tem um acento e corresponde ao passado do verbo "barrer" (varrer), o serviço também pode corresponder à palavra "barrio" (vizinhança), mas com uma confiança ligeiramente mais baixa.

  O sistema fornecerá a maior pontuação de confiança em entidades com correspondências exatas. Por exemplo, `barrio` não será detectado se `barrió` estiver no conjunto de treinamento, e `barrió` não será detectado se `barrio` estiver no conjunto de treinamento.

  Você deverá treinar o sistema com os caracteres e acentos apropriados. Por exemplo, se você estiver esperando `barrió` como uma resposta, coloque `barrió` no conjunto de treinamento.

  Embora não seja um acento, o mesmo se aplica às palavras que usam, por exemplo, a letra espanhola `ñ` vs. a letra `n`, como "uña" vs. "una". Nesse caso, a letra `ñ` não é apenas um `n` com um acento; ela é uma letra exclusiva, específica do espanhol.

  Se você achar que os seus clientes não usarão os acentos apropriados ou usarão palavras erradas (incluindo, por exemplo, a colocação de um `n` em vez de um `ñ`), ative a correspondência difusa ou inclua-os explicitamente nos exemplos de treinamento.

  **Nota:** A normalização de acento está ativada para português, espanhol, francês e tcheco.

- **Sinalização de opção por não participar da área de trabalho:** a API de REST do {{site.data.keyword.conversationshort}} agora suporta uma sinalização de opção por não participar para áreas de trabalho. Essa sinalização indica que os dados de treinamento da área de trabalho como intenções e entidades não devem ser usados pela IBM para melhorias gerais do serviço. Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}

### 7 de agosto de 2017
{: #7August2017}

- Interpretação de **`Next` e `last` data** - o serviço de Conversa trata a "última" e a "próxima" datas como referência ao último ou próximo dia mais imediato referenciado, que pode estar na mesma semana ou em uma semana anterior. Consulte o tópico [entidades do sistema](system-entities.html#sys-datetime) para obter informações adicionais.

### 3 de agosto de 2017
{: #3August2017}

- **Correspondência difusa para idiomas adicionais (Beta)** - A correspondência difusa para entidades adicionais agora está disponível para idiomas adicionais, conforme notado no tópico [Idiomas suportados](lang-support.html).
- **Correspondência parcial (Beta - apenas inglês)** - a correspondência difusa agora irá sugerir automaticamente sinônimos baseados em subsequências presentes em entidades definidas pelo usuário e designará uma pontuação de confiança inferior quando comparada à correspondência de entidade exata. Veja [Correspondência difusa](entities.html#fuzzy-matching) para obter detalhes.

### 28 de julho de 2017
{: #28July2017}

- Ao configurar as preferências bidirecionais para o conjunto de ferramentas, agora é possível especificar a direção da interface gráfica com o usuário.
- O esquema de cores do conjunto de ferramentas foi atualizado para ficar consistente com outros serviços e produtos do Watson.

### 19 de julho de 2017
{: #19July2017}

- A API de REST do {{site.data.keyword.conversationshort}} agora suporta o acesso a nós de diálogo. Para obter mais informações, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14 de julho de 2017
{: #14July2017}

- A funcionalidade dos slots de diálogos foi aprimorada. Por exemplo, foi incluída uma propriedade *slot_in_focus* que pode ser usada para definir uma condição que se aplica a um único slot. Consulte [Reunindo informações com slots](/docs/services/conversation/dialog-build.html#slots) para obter detalhes.

### 12 de julho de 2017
{: #12July2017}

- **Suporte para Tcheco** - foi introduzido o suporte ao idioma tcheco; consulte o tópico [Idiomas Suportados](lang-support.html) para obter detalhes adicionais.

### 11 de julho de 2017
{: #11July2017}

- **Testar no Slack** - é possível usar a nova ferramenta **Testar no Slack** para implementar rapidamente sua área de trabalho como um usuário robô do Slack para propósitos de teste. Essa ferramenta está disponível apenas para a região Sul dos EUA do {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, consulte [Testando no Slack](test-deploy.html).
- **Atualizações para árabe** - o suporte ao idioma árabe foi aprimorado para incluir a pontuação absoluta por intenção e a capacidade de marcar intenções como irrelevante; consulte o tópico [Idiomas Suportados](lang-support.html) para obter detalhes adicionais. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](release-notes.html#updated-models) para obter mais informações.

### 23 de junho de 2017
{: #23June2017}

- **Atualizações para Coreano** - o suporte ao idioma coreano foi aprimorado; consulte o tópico [Idiomas Suportados](lang-support.html) para obter detalhes adicionais. Observe que os modelos de aprendizado de serviço do {{site.data.keyword.conversationshort}} podem ter sido atualizados como parte desse aprimoramento, e quando você treinar o seu modelo, todas as mudanças serão aplicadas; veja [Modelos atualizados](release-notes.html#updated-models) para obter mais informações.

### 22 de junho de 2017
{: #22June2017}

- **Apresentando os slots** - agora é mais fácil coletar várias partes de informações de um usuário em um único nó incluindo slots. Anteriormente, você precisava criar vários nós de diálogo para cobrir todas as combinações possíveis de maneiras que os usuários poderiam fornecer as informações. Com os slots, é possível configurar um nó único que salva quaisquer informações que o usuário forneça e solicita quaisquer detalhes necessários que o usuário não forneça. Consulte [Reunindo informações com slots](dialog-build.html#slots) para obter mais detalhes.
- **Árvore de diálogo simplificada** - a árvore de diálogo foi reprojetada para melhorar sua usabilidade. A visualização em árvore está mais compacta, portanto, é mais fácil ver onde você está nela. E os links entre nós são representados de uma maneira que torna mais fácil entender os relacionamentos entre os nós.

### 21 de junho de 2017
{: #21June2017}

- **Suporte árabe** - o suporte ao idioma para árabe agora está disponível de maneira geral. Para obter detalhes, consulte [Configurando idiomas bidirecionais](lang-support.html#configuring-bi-directional).
- **Atualizações de idioma** - os algoritmos de serviço do {{site.data.keyword.conversationshort}} foram atualizados para melhorar o suporte ao idioma geral. Consulte o tópico [Idiomas Suportados](lang-support.html) para obter detalhes.

### 16 de junho de 2017
{: #16June2017}

- **Recomendações (Beta - apenas usuários Premium)** - o painel Melhorar também inclui uma página **Recomendações** que recomenda maneiras de melhorar seu sistema analisando as conversas que os usuários têm com seu robô de bate-papo e levando em conta os dados de treinamento e a certeza de resposta atuais de seu sistema.

### 14 de junho de 2017
{: #14June2017}

- **Correspondência difusa para idiomas adicionais (Beta)** - A correspondência difusa para entidades adicionais agora está disponível para idiomas adicionais, conforme notado no tópico [Idiomas suportados](lang-support.html). Será possível ativar a correspondência difusa por entidade para melhorar a capacidade do serviço para reconhecer termos na entrada do usuário com sintaxe que é semelhante à entidade, sem exigir uma correspondência exata. O recurso é capaz de mapear a entrada do usuário para a entidade correspondente, apesar da presença de erros de ortografia ou de pequenas diferenças sintáticas. Por exemplo, se você definir girafa como um sinônimo para uma entidade animal e a entrada do usuário contiver os termos girafas ou girafa, a correspondência difusa será capaz de mapear o termo para a entidade animal corretamente. Veja [Correspondência difusa](entities.html#fuzzy-matching) para obter detalhes.

### 13 de junho de 2017
{: #13June2017}

- **Conversas do usuário** - o painel Melhorar agora inclui uma página **Conversas do Usuário**, que fornece uma lista de interações do usuário com seu robô de bate-papo que podem ser filtradas por palavra-chave, intenção, entidade ou número de dias. É possível abrir conversas individuais para corrigir intenções ou para incluir valores de entidade ou sinônimos.
- **Mudança de regex** - as expressões regulares que são suportadas por funções SpEL como find, matches, extract, replaceFirst, replaceAll e split mudaram. Um grupo de construções de expressões regulares não é mais permitido, incluindo as construções look-ahead, look-behind, de repetição possessiva e backreference. Essa mudança foi necessária para evitar uma exposição de segurança na biblioteca de expressões comuns original.

### 12 de junho de 2017
{: #12June2017}

- O número máximo de áreas de trabalho que você pode criar com o plano **Lite** (anteriormente chamado de plano Grátis) foi mudado de 3 para 5.
- Agora é possível designar qualquer nome para um nó de diálogo; ele não precisa ser exclusivo. E será possível mudar posteriormente o nome do nó sem impactar a forma como o nó será referenciado internamente. O nome que você especifica é tratado como um alias e o sistema usa seu próprio identificador interno para referenciar o nó.
- Não é mais possível mudar o idioma de uma área de trabalho depois de criá-lo editando os detalhes da área de trabalho. Se você precisar mudar o idioma, poderá exportar a área de trabalho como um arquivo JSON, atualizar a propriedade de idioma e, em seguida, importar o arquivo JSON como uma nova área de trabalho.

### 6 de junho de 2017
{: #6June2017}

- **Saiba** - está disponível uma nova página *Aprenda sobre o {{site.data.keyword.watson}} {{site.data.keyword.conversationshort}}* que fornece as informações de introdução e os links para a documentação de serviço e outros recursos úteis. Para abrir a página, clique no ícone ![i para informações.](images/info.png) no cabeçalho da página.
- **Exportação e exclusão em massa** - agora é possível exportar simultaneamente inúmeras intenções ou entidades para um arquivo CSV, para que você possa, então, importar e reutilizá-las em outro aplicativo {{site.data.keyword.conversationshort}}. Também é possível selecionar simultaneamente inúmeras entidades ou intenções para exclusão em massa.
- **Atualizações para Coreano** - tokenizers coreanos foram atualizados para tratar do suporte ao idioma informal. A IBM continua trabalhando em melhorias para o reconhecimento e a classificação de entidade.
- **Suporte ao emoji** - os emojis incluídos em exemplos de intenção ou como valores de entidade agora serão classificados/extraídos corretamente. **Nota**: apenas emojis que foram incluídos em seus dados de treinamento serão identificados de maneira correta e consistente; o suporte a emojis não pode classificar corretamente emojis semelhantes com tons de cores diferentes ou outras variações.
- **Stemming de entidade (Beta - apenas inglês)** - o recurso beta de correspondência difusa reconhece entidades e as corresponde com base no formato raiz do valor da entidade. Por exemplo, esse recurso reconhece corretamente 'bananas' como sendo semelhante a 'banana' e 'executar' sendo semelhante a 'executando' pois eles compartilham um formato raiz comum. Para obter mais informações, consulte [Correspondência Difusa](entities.html#fuzzy-matching).
- **Progresso de importação da área de trabalho** - ao importar uma área de trabalho de um arquivo JSON, imediatamente é exibido um ladrilho para a área de trabalho, no qual as informações sobre o progresso da importação são exibidas.
- **Tempo de treinamento reduzido** - múltiplos modelos agora são treinados em paralelo, o que reduz significativamente o tempo de treinamento para áreas de trabalho grandes.

### 26 de maio de 2017
{: #26May2017}

- A versão de API atual agora é `2017-05-26`. Esta versão introduz as mudanças a seguir:
    - O esquema de objetos ErrorResponse mudou. Essa mudança afeta todos os terminais e métodos. Para obter informações, veja a Referência de API [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - O esquema interno usado para representar os nós de diálogo no JSON da área de trabalho exportado mudou. Se você usar a API `2017-05-26` para importar uma área de trabalho que foi exportada usando uma versão anterior, alguns nós de diálogo poderão não ser importados corretamente. Para obter melhores resultados, sempre importe uma área de trabalho usando a mesma versão que foi usada para exportá-la.

### 25 de maio de 2017
{: #25May2017}

- Agora é possível gerenciar variáveis de contexto na área de janela "Experimente". Clique no link **Gerenciar contexto** para abrir uma nova área de janela na qual é possível configurar e verificar os valores de variáveis de contexto à medida que você testa o diálogo. Consulte [Testando seu diálogo](dialog-test.html) para obter informações adicionais.

### 16 de maio de 2017
{: #16May2017}

- Uma área de trabalho de amostra **Painel do Carro** agora está disponível quando você abre a ferramenta. Para usar a amostra como um ponto de início para sua própria área de trabalho, edite a área de trabalho. Se você desejar usá-lo para múltiplas áreas de trabalho, duplique-o como alternativa. A área de trabalho de amostra não considera o total de sua área de trabalho de assinatura, a menos que você a utilize.
- Agora é mais fácil navegar pela ferramenta. As opções do menu de navegação estão disponíveis no lado da página principal em vez de na parte superior. Na parte superior da página, há links de Trilha de Navegação que mostram onde você está. Agora é possível alternar entre as instâncias de serviço da página Áreas de Trabalho. Para chegar lá rapidamente, clique em **Voltar para áreas de trabalho** no menu de navegação. Se você tiver várias instâncias de serviço, o nome da instância atual será exibido. É possível clicar no link **Mudar** ao lado dele para escolher outra instância.
- Ao criar um diálogo, dois nós agora são incluídos nele para você: 1) um nó **Bem-vindo** na parte superior da árvore de diálogo que contém a saudação a ser exibida para o usuário e 2) um nó **Qualquer outra coisa** na parte inferior da árvore que captura quaisquer consultas do usuário que não sejam reconhecidas por outros nós no diálogo e responde a elas. Consulte [Criando um diálogo](dialog-create.html) para obter mais detalhes.
- Quando você está testando um diálogo na área de janela "Experimente", agora é possível localizar e reenviar uma elocução de teste recente pressionando a tecla Para Cima para percorrer suas entradas anteriores.
- Agora está disponível o suporte ao idioma coreano experimental para 5 entidades do sistema (`@sys-data`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`). Existem problemas conhecidos para algumas das entidades numéricas e suporte limitado para a entrada de linguagem informal.
- Uma página Visão Geral está disponível na guia Melhorar. A página fornece um resumo de interações com seu robô. Será possível ver a quantia de tráfego para um determinado período de tempo, bem como as intenções e entidades que foram reconhecidas mais frequentemente nas conversas do usuário. Para obter informações adicionais, consulte [Usando a página Visão Geral](logs_oview.html).

### 27 de abril de 2017
{: #27April2017}

- As entidades do sistema a seguir agora estão disponíveis como recursos beta apenas em inglês:
    - sys-location: reconhece referências a locais como vilas, cidades e países em elocuções do usuário.
    - sys-person: reconhece referências a nomes de pessoas, primeiro e último, em elocuções do usuário.

    Para obter mais informações, consulte a [Referência de entidades do sistema](system-entities.html).
- A correspondência difusa para entidades é um recurso beta que agora está disponível em inglês. Será possível ativar a correspondência difusa por entidade para melhorar a capacidade do serviço para reconhecer termos na entrada do usuário com sintaxe que é semelhante à entidade, sem exigir uma correspondência exata. O recurso é capaz de mapear a entrada do usuário para a entidade correspondente, apesar da presença de erros de ortografia ou de pequenas diferenças sintáticas. Por exemplo, se você definir **girafa** como um sinônimo para uma entidade animal e a entrada do usuário contiver os termos *girafas* ou *girafa*, a correspondência difusa será capaz de mapear o termo para a entidade animal corretamente. Consulte [Definindo Entidades](entities.html#fuzzy-matching) e procure por "Correspondência Difusa" para obter detalhes.

### 18 de abril de 2017
{: #18April2017}

- A API de REST do {{site.data.keyword.conversationshort}} agora suporta o acesso aos recursos a seguir:
    - entidades
    - valores de entidade
    - sinônimos de valor da entidade
    - Logs

    Para obter informações, veja a Referência de API [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
- O comportamento do método `POST` /messages mudou a manipulação de entidades e intenções especificadas como parte da entrada da mensagem:
    - Se você especificar intenções na entrada, o serviço usará as intenções especificadas, mas usará processamento de língua natural para detectar entidades na entrada do usuário.
    - Se você especificar entidades na entrada, o serviço usará as entidades especificadas, mas usará processamento de língua natural para detectar intenções na entrada do usuário.

        O comportamento não foi mudado para mensagens que especificam intenções e entidades, ou para mensagens que não especificam nenhuma das duas.
- A opção para marcar a entrada do usuário como irrelevante agora está disponível para todos os idiomas suportados. Este é um recurso beta.
- Uma nova guia Credenciais fornece um único local no qual você pode localizar todas as informações de que precisa para conectar seu aplicativo a uma área de trabalho (como as credenciais de serviço e o ID da área de trabalho), bem como outras opções de implementação. Para acessar a guia Credenciais para sua área de trabalho, clique no ícone ![Menu](images/Menu_16.png) e selecione **Credenciais**.

### 9 de março de 2017
{: #9March2017}

A API de REST do {{site.data.keyword.conversationshort}} agora suporta o acesso aos recursos a seguir:

- áreas de trabalho
- intenções
- exemplos
- contra-exemplos

Para obter informações, veja a Referência de API [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7 de março de 2017
{: #7March2017}

- O uso de `.` ou `..` como um nome de intenção causa problemas e não é mais suportado.

    Não é possível renomear ou excluir uma intenção com este nome; para mudar o nome, exporte suas intenções para um arquivo, renomeie a intenção no arquivo e importe o arquivo atualizado em sua área de trabalho.

    Os clientes pagantes podem entrar em contato com o suporte para uma mudança do banco de dados.

### 1 de março de 2017
{: #1March2017}

- Entidades do sistema agora são ativadas em alemão.

### 22 de fevereiro de 2017
{: #22February2017}

- As mensagens agora estão limitadas a 2.048 caracteres.

### 3 de fevereiro de 2017
{: #3February2017}

- Nesta liberação, nós mudamos como as intenções são pontuadas e incluímos a capacidade de marcar a entrada como irrelevante para seu aplicativo. [Para obter detalhes, consulte Definindo intenções](intents.html#mark-irrelevant) e procure por "Marcar como irrelevante".
    - Esses recursos estão disponíveis no conjunto de ferramentas [fazendo upgrade de sua área de trabalho](upgrading.html).
    - Esses recursos estão disponíveis para seu aplicativo mudando a chamada de API da mensagem para usar **2017-02-03.**
- O processamento de ações **Ir para** foi mudado para evitar loops que possam ocorrer sob determinadas condições.

    Consulte [Ir para Ações](dialog-build.html#jump-to) para obter detalhes.

### 11 de janeiro de 2017
{: #11January2017}

- Nesta liberação, é possível customizar títulos de nós no diálogo.

### 22 de dezembro de 2016
{: #22December2016}

- Nesta liberação, nós de diálogo exibem uma nova seção para `node title`. A capacidade de customizar o `node title` não está disponível. Quando reduzido, o `node title` exibe a `node condition` do nó de diálogo. Se não houver uma `node condition`, "Nó Sem Título" será exibido como o título.

### 19 de dezembro de 2016
{: #19December2016}

Várias mudanças tornam o construtor de diálogo mais fácil e mais intuitivo para usar:

- Uma visualização de edição maior torna mais fácil visualizar todos os detalhes de um nó à medida que você trabalha nele.
- Um nó pode conter várias respostas, cada uma acionada por uma condição separada. Para obter mais informações, consulte [Múltiplas respostas](dialog-build.html#responses).

### 5 de dezembro de 2016
{: #5December2016}

- Novos idiomas são suportados, todos no modo Experimental: alemão, chinês tradicional, chinês simplificado e holandês.
- Duas novas entidades do sistema estão disponíveis: @sys-data e @sys-time. Para obter detalhes, consulte [Entidades do Sistema](system-entities.html).

### 21 de outubro de 2016
{: #21October2016}

- O serviço do {{site.data.keyword.conversationshort}} agora fornece entidades do sistema, que são entidades comuns que podem ser usadas em qualquer caso de uso. Para obter detalhes, consulte [Definindo Entidades](entities.html) e procure por "Ativando entidades do sistema".
- Agora é possível visualizar um histórico de conversas com usuários na página Melhorar. É possível usar isso para entender o comportamento de seu robô. Para obter detalhes, consulte [Melhorando o entendimento](logs.html).
- Agora é possível importar entidades de um arquivo CSV (Valor Separado por Vírgula), que ajuda quando você tem um grande número de entidades. Para obter detalhes, consulte [Definindo Entidades](entities.html) e procure por "Importando entidades".

### 20 de setembro de 2016
{: #20September2016}

**Nova versão**: 2016-09-20

Para aproveitar as mudanças em uma nova versão, mude o valor do parâmetro `version` para a nova data. Se você não estiver pronto para atualizar para esta versão, não mude sua data da versão.

- Versão **2016-09-20**: `dialog_stack` foi mudado de uma matriz de sequências para uma matriz de objetos JSON.

### 29 de agosto de 2016
{: #29August2016}

- É possível mover os nós de diálogo de uma ramificação para outra, como irmãos ou de mesmo nível. Para obter detalhes, consulte [Construindo um diálogo](dialog-build.html) e procure por "Movendo um nó de diálogo".
- É possível expandir a janela do editor JSON.
- É possível visualizar os logs de bate-papo de conversas de seu robô para ajudá-lo a entender seu comportamento. É possível filtrar por intenções, entidades, data e horário. Para obter detalhes, consulte [Melhorando o entendimento](logs.html)

### 11 de julho de 2016
{: #21July2016}

- Esta liberação de Disponibilidade Geral permite trabalhar com entidades e diálogos para criar um robô totalmente funcional.

### 18 de maio de 2016
{: #18May2016}

- Esta liberação Experimental do {{site.data.keyword.conversationshort}} apresenta a interface com o usuário e permite trabalhar com áreas de trabalho, intenções e exemplos.
