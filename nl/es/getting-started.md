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

# Guía de aprendizaje de iniciación
{: #gettingstarted}

En esta breve guía de aprendizaje presentamos una introducción a la herramienta {{site.data.keyword.conversationshort}} y al proceso de crear la primera conversación.
{: shortdesc}

## Antes de empezar
{: #prerequisites}

Si ya ha creado una instancia de servicio, ya cumple con los requisitos previos. Vaya al paso 1.
{: tip}

1.  Vaya al servicio [{{site.data.keyword.conversationshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/catalog/services/conversation/){: new_window} y regístrese para una cuenta de {{site.data.keyword.Bluemix_notm}} gratuita o bien inicie una sesión.
1.  Después de iniciar la sesión, escriba `conversation-tutorial` en el campo **Nombre de servicio** de la página {{site.data.keyword.conversationshort}} y pulse **Crear**.

## Paso 1: Iniciar la herramienta
{: #launch-tool}

Después de crear la instancia de servicio, irá al panel de control de la instancia. Inicie desde aquí la herramienta {{site.data.keyword.conversationshort}}.

Pulse **Gestionar** y luego **Iniciar herramienta**.

![Muestra la página de gestión de IBM Bluemix Watson desde la que se inicia la herramienta](images/gs-launch-tool.png)

Es posible que se le solicite que inicie una sesión en la herramienta por separado. Si es así, utilice las credenciales de IBM Bluemix para iniciar la sesión.

## Paso 2: Crear un espacio de trabajo
{: #create-workspace}

El primer paso en la herramienta {{site.data.keyword.conversationshort}} consiste en crear un espacio de trabajo.

Un [*espacio de trabajo*](configure-workspace.html) es un contenedor para los artefactos que definen el flujo de la conversación.

1.  En la herramienta {{site.data.keyword.conversationshort}}, pulse **Crear**.
1.  Asigne al espacio de trabajo el nombre `Conversation tutorial` y pulse **Crear**. Irá al separador **Intenciones** del nuevo espacio de trabajo.

![Muestra una animación de un usuario que está creando el espacio de trabajo Conversation tutorial](images/gs-create-workspace-animated.gif)

## Paso 3: Crear intenciones
{: #create-intents}

Una [intención](intents.html) representa la finalidad de la entrada de usuario. Puede considerar las intenciones como las acciones que pueden desear llevar a cabo los usuarios con la aplicación.

Para este ejemplo, vamos a simplificar y vamos a definir solo dos intenciones: uno para decir hola y uno para decir adiós.

1.  Asegúrese de que está en el separador Intenciones. (Debería estar allí si acaba de crear el espacio de trabajo.)
1.  Pulse **Crear nueva**.
1.  Asigne a la intención el nombre `hello`.
1.  Escriba `hello` como **Ejemplo de usuario** y pulse Intro.

   Los *ejemplos* indican al servicio {{site.data.keyword.conversationshort}} los tipos de entrada de usuario que desea comparar con la intención. Cuantos más ejemplos proporcione, más precisa será la forma en que el servicio reconocerá las intenciones de los usuarios.
1.  Añadir cuatro ejemplos más y pulse **Listo** para terminar de crear la intención #hello:
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

   ![Muestra una animación de un usuario que está creando una intención #hello con expresiones de ejemplo.](images/gs-add-intents-animated.gif)

1.  Cree otra intención denominada #goodbye con estos cinco ejemplos:
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

Ha creado dos intenciones, #hello y #goodbye, y ha proporcionado entrada de usuario de ejemplo para formar el servicio {{site.data.keyword.watson}} para que reconozca estas intenciones en la entrada de usuario.

![Muestra la página Intenciones con las intenciones #goodbye y #hello](images/gs-add-intents-result.png)

## Paso 4: Crear un diálogo
{: #build-dialog}

Un [diálogo](dialog-build.html) define el flujo de la conversación en forma de árbol lógico. Cada nodo del árbol tiene una condición que lo activa, en función de la entrada de usuario.

Vamos a crear un diálogo sencillo que maneje nuestras intenciones #hello y #goodbye, cada una con un solo nodo.

### Adición de un nodo de inicio

1.  En la herramienta {{site.data.keyword.conversationshort}}, pulse el separador **Diálogo**.
1.  Pulse **Crear**. Verá dos nodos:
    - **Welcome**: Contiene un saludo que se muestra a los usuarios la primera vez que interactúan con el servicio.
    - **Anything else**: Contiene frases que se utilizan para responder a los usuarios cuando no se reconoce la información que especifican.

    ![Muestra el árbol del diálogo con los nodos Welcome y Anything else](images/gs-add-dialog-node-animated-cover.png)
1.  Pulse el nodo **Welcome** para abrirlo en la vista de edición.
1.  Sustituya la respuesta predeterminada por el texto `Welcome to the Conversation tutorial!`.

    ![Muestra el nodo Welcome abierto en la vista de edición](images/gs-edit-welcome-node.png)
1.  Pulse ![Cerrar](images/close.png) para cerrar la vista de edición.

Ha creado un nodo de diálogo que se activa mediante la condición `welcome`, que es una condición especial que indica que el usuario ha iniciado una nueva conversación. El nodo especifica que cuando se inicia una nueva conversación, el sistema debe responder con el mensaje de bienvenida.

### Prueba del nodo de inicio

Puede probar el diálogo en cualquier momento para verificarlo. Vamos a probarlo ahora.

- Pulse el icono ![Preguntar a Watson](images/ask_watson.png) para abrir el panel "Pruébelo". Debería ver el mensaje de bienvenida.

    ![Prueba del nodo de diálogo](images/gs-tryitout-welcome-node.png)

### Adición de nodos para manejar intenciones

Ahora vamos a añadir nodos para manejar nuestras intenciones entre el nodo `Welcome` y el nodo `Anything else`.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) del nodo **Welcome** y seleccione **Añadir nodo debajo**.
1.  Escriba `#hello` en el campo **Especificar una condición** de este nodo. Luego seleccione la opción **#hello**.
1.  Añada la respuesta, `Good day to you`.
1.  Pulse ![Cerrar](images/close.png) para cerrar la vista de edición.

   ![Muestra una animación de un usuario que está añadiendo un nodo hello al diálogo](images/gs-add-dialog-node-animated.gif)
1.  Pulse el icono Más ![Más opciones](images/kabob.png) en este nodo y luego seleccione **Añadir nodo debajo** para crear un nodo igual. En el nodo de igual, especifique `#goodbye` como condición y `OK. See you later!` como respuesta.

    ![Adición de nodos para intenciones](images/gs-add-dialog-nodes-result.png)

### Prueba de reconocimiento de intención

Ha creado un diálogo sencillo para reconocer y responder a las entradas hello y goodbye. Vamos a ver si funcionan.

1.  Pulse el icono ![Preguntar a Watson](images/ask_watson.png) para abrir el panel "Pruébelo". Aparece este mensaje de bienvenida tranquilizador.
1.  En la parte inferior del panel, escriba `Hello` y pulse Intro. La salida indica que se ha reconocido la intención #hello y aparece la respuesta adecuada (`Good day to you`).
1.  Intente la entrada siguiente:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

   ![Muestra una animación del usuario que está probando el diálogo en el panel Pruébelo](images/gs-test-dialog-animated.gif)

{{site.data.keyword.watson}} reconoce las intenciones incluso cuando la entrada no coincide exactamente con los ejemplos que ha incluido. El diálogo utiliza intenciones para identificar la finalidad de la entrada de usuario, independientemente de los términos precisos utilizados, y responde en la forma que especifique.

### Resultado de crear un diálogo

Eso es todo. Ha creado una conversación sencilla con dos intenciones y un diálogo para reconocerlas.

## Paso 5: Revisar el espacio de trabajo de ejemplo
{: #review-sample-workspace}

Abra el espacio de trabajo de ejemplo para ver intenciones similares a las que acaba de crear y muchas más, y para ver cómo se utilizan en un diálogo complejo.

1.  Vuelva a la página espacios de trabajo.
   Puede pulsar el botón ![Botón Volver a espacios de trabajo](images/workspaces-button.png) en el menú de navegación.
1.  En el mosaico del espacio de trabajo **Car Dashboard - Sample**, pulse el botón **Editar ejemplo**.

    ![Muestra el mosaico de ejemplo de Car Dashboard en la página Espacios de trabajo](images/gs-workspace-car-sample.png)

## Qué hacer a continuación
{: #next-steps}

Esta guía de aprendizaje se basa en un ejemplo sencillo. Para una aplicación real, deberá algunas intenciones más interesantes, algunas entidades y un diálogo más complejo.

- Pruebe la [guía de aprendizaje](tutorial.html) avanzada para añadir entidades y aclarar una finalidad de usuario.
- Para [desplegar](deploy.html) el espacio de trabajo, conéctelo a una interfaz de usuario frontal, red social o canal de mensajería.
- Consulte las [apps de ejemplo](sample-applications.html).
