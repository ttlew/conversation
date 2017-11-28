---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-30"

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

# Cómo trabajar con conversaciones
{: #logs_convo}

Para abrir una lista de interacciones entre los usuarios y el bot, seleccione **Conversaciones de usuario** en la barra de navegación. Si no ve **Conversaciones de usuarios**, utilice el menú ![Menú](images/Menu_16.png) para abrir la página.
{: shortdesc}

Cuando se abre la página **Conversaciones de usuarios**, la vista predeterminada muestra los resultados del último día, con el resultado más reciente el primero. Están disponibles la intención principal (#intent) y los valores de entidades reconocidas (@entity) utilizadas en el mensaje y el texto del mensaje. Para las intenciones que no se reconocen, el valor mostrado es *Irrelevante*. Si una entidad no se reconoce, o no se ha proporcionado, el valor mostrado es *No se han encontrado entidades*. ![Página predeterminada de registros](images/logs_page1.png)

## Límites del registro
{: #log-limits}

El periodo de tiempo durante el que se retienen mensajes del chat dependen del plan de servicio de {{site.data.keyword.conversationshort}}:

  Plan de servicio                         | Retención del registro del chat
  ------------------------------------ | ------------------------------------
  Premium                              | Últimos 90 días
  Estándar                             | Últimos 30 días
  Gratuito                                 | Últimos 7 días

## Filtrado de expresiones

Es importante señalar que la página **Conversaciones de usuarios** muestra el número total de *expresiones* entre los usuarios y el bot. Una expresión es un único mensaje que el usuario envía al bot. Cada conversación puede estar formada por varias expresiones. Por lo tanto, el número de resultados de esta página **Conversaciones de usuarios** es distinto del número de conversaciones que se muestra en la página [Visión general](logs_oview.html).

Puede filtrar las expresiones por *Sentencias de usuario de búsqueda*, *Intenciones*, *Entidades* y *Últimos* n *días*:

*Sentencias de usuario de búsqueda* - Escriba una palabra en la barra de búsqueda. Se realizará una búsqueda de las entradas de los usuarios, pero no de las respuestas del bot.

*Intenciones* - Seleccione el menú desplegable y escriba una intención en el campo de entrada o bien seleccione en la lista con información. Puede seleccionar más de una intención, lo cual filtra los resultados utilizando cualquiera de las intenciones seleccionadas, incluidas las marcadas como *Irrelevante*.

![Menú desplegable de intenciones](images/intents_filter.png)

*Entidades* - Seleccione el menú desplegable y escriba una entidad en el campo de entrada o bien seleccione en la lista con información. Puede seleccionar más de una entidad, lo cual filtra los resultados por cualquiera de las entidades seleccionadas. Si filtra por intención *y* entidad, los resultados incluirán los mensajes que tienen ambos valores. También puede filtrar por los resultados con *No han encontrado entidades*.

![Menú desplegable de entidades](images/entities_filter.png)

Las expresiones tardar un tiempo en actualizarse. Deje pasar al menos 30 tras la interacción de un usuario con el bot antes de intentar filtrar según ese contenido.

## Visualización de una expresión individual
Puede expandir cada entrada de expresión para ver qué ha dicho el usuario en la conversación completa y cómo ha respondido el bot. Para ello, seleccione **Abrir conversación**. Irá automáticamente a la expresión que ha seleccionado dentro de esa conversación.

![Panel Abrir conversación](images/open_convo.png)

A continuación, puede elegir optar por la clasificación o clasificaciones correspondientes a la expresión que ha seleccionado.

![Panel Abrir conversación con clasificaciones](images/open_convo_classes.png)

## Corrección de una intención

1.  Para corregir una intención, seleccione el icono de edición ![Editar](images/edit_icon.png) que hay junto a la intención seleccionada.
1.  En la lista proporcionada, seleccione la intención correcta para esta entrada.
    - Empiece escribiendo en el campo de entrada y se filtrará la lista de intenciones.
    - También puede elegir **Marcar como irrelevante** en este menú. (Para obtener más información, consulte "[Marcar como irrelevante](intents.html#mark-irrelevant)"). O bien puede elegir **No formar en intención**, que no guarda esta expresión como ejemplo para la formación.

    ![Seleccionar intención](images/select_intent.png)
1.  Seleccione **Guardar**.

    ![Guardar intención](images/save_intent.png)

## Adición de un valor de entidad o sinónimo

1.  Para añadir un valor de entidad o sinónimo, seleccione el icono de edición ![Editar](images/edit_icon.png) que hay junto a la entidad elegida.
1.  Seleccione **Añadir entidad**.

    ![Añadir entidad](images/add_entity.png)
1.  Ahora seleccione una palabra o frase en la entrada de usuario subrayada.

    ![Seleccionar entidad](images/select_entity.png)
1.  Elija una entidad a la que se añadirá la frase resaltada como valor.
    - Empiece escribiendo en el campo de entrada y se filtrará la lista de entidades y valores.
    - Para añadir la frase resaltada como sinónimo de un valor existente, elija "@entity:value" en la lista desplegable.

    ![Añadir palabra de entidad](images/add_entity_word.png)
1.  Seleccione **Guardar**.

    ![Guardar entidad](images/add_entity_save.png)
