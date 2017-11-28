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

# Creación de una aplicación cliente

Ya tiene un diálogo de trabajo. Ahora supongamos que desea desarrollar la aplicación que va a interactuar con los usuarios y a comunicarse con el servicio {{site.data.keyword.conversationfull}}.
{: shortdesc}

Los ejemplos de código de esta guía de aprendizaje están escritos en JavaScript utilizando Node.js Watson SDK, pero puede utilizar otros lenguajes. Instale Watson [SDK ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} para su lenguaje de programación y revise la documentación de SDK para obtener más información.
{: tip}

## Configuración del servicio Conversation

La aplicación de ejemplo que se creará en esta sección implementa varias funciones de un asistente personal cognitivo. El código de la aplicación se conectará a un espacio de trabajo de {{site.data.keyword.conversationshort}}, donde tiene lugar el proceso cognitivo (como por ejemplo la detección de intenciones del usuario).

Antes de continuar con este ejemplo, debe configurar el espacio de trabajo de {{site.data.keyword.conversationshort}} necesario:

1.  Descargue el <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">archivo JSON</a> del espacio de trabajo.
1.  [Importe el espacio de trabajo](configure-workspace.html#creating-workspaces) en una instancia del servicio {{site.data.keyword.conversationshort}}.

## Obtención de información del servicio

Para acceder a las API REST del servicio {{site.data.keyword.conversationshort}}, la aplicación debe ser capaz de autenticares con {{site.data.keyword.Bluemix}} y de conectarse al espacio de trabajo de {{site.data.keyword.conversationshort}} adecuado. Tendrá que copiar las credenciales del servicio y el ID del espacio de trabajo y pegarlos en el código de la aplicación.

Para acceder a las credenciales del servicio y al ID del espacio de trabajo desde su espacio de trabajo, seleccione el menú ![Menú](images/Menu_16.png), seleccione **Desplegar** y luego vaya al separador **Credenciales**.

También puede acceder a las credenciales del servicio desde el panel de control de Bluemix.

## Comunicación con el servicio Conversation

Interactuar con el servicio {{site.data.keyword.conversationshort}} es sencillo. Echemos un vistazo a un ejemplo de Node.js que se conecta al servicio, envía un único mensaje único y registra la salida en la consola:

```javascript
// Ejemplo 1: configura el derivador del servicio, envía el mensaje inicial y
// recibe respuesta.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el derivador del servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir por nombre de usuario de la clave de servicio
  password: 'PASSWORD', // sustituir por contraseña de la clave de servicio
  path: { workspace_id: 'WORKSPACE_ID' }, // sustituir por ID de espacio de trabajo
  version_date: '2016-07-11'
});

// Iniciar conversación con mensaje vacío.
conversation.message({}, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  // Mostrar la salida del diálogo, si la hay.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}

El primer paso consiste en crear un derivador para el servicio {{site.data.keyword.conversationshort}}.

El derivador es un objeto que utilizará para enviar información de entrada al servicio y para recibir la salida de este. Cuando cree el derivador del servicio, especifique las credenciales de autenticación de la clave de servicio, así como la versión de la API de {{site.data.keyword.conversationshort}} que está utilizando.

En este ejemplo de Node.js, el derivador es una instancia de `ConversationV1`, almacenada en la variable `conversation`. Los Watson SDK de otros lenguajes proporcionan mecanismos equivalentes para crear instancias de un derivador de servicio.

Después de crear el derivador del servicio, lo utilizaremos que enviar un mensaje al servicio {{site.data.keyword.conversationshort}}. En este ejemplo, el mensaje está vacío; sólo queremos activar el nodo conversation_start en el diálogo, de modo que no necesitamos ningún texto de entrada.

Utilice el mandato `node <filename.js>` para ejecutar la aplicación de ejemplo.

**Nota:** Asegúrese de haber instalado Node.js Watson SDK mediante `npm install watson-developer-cloud`.

Si todo funciona según lo esperado, el servicio {{site.data.keyword.conversationshort}} devuelve la salida del diálogo, que luego se registra en la consola:

```
Welcome to the Conversation example!
```
{: screen}

Esta salida nos indica que nos hemos comunicado correctamente con el servicio {{site.data.keyword.conversationshort}} y hemos recibido el mensaje especificado por el nodo conversation_start en el diálogo. Ahora podemos añadir una interfaz de usuario, lo que permitirá procesar entrada de usuario.

## Proceso de información de entada de usuario para detectar intenciones

Para poder procesar la entrada de usuario, debemos añadir una interfaz de usuario a nuestra aplicación. En este ejemplo, vamos a simplificar y utilizaremos una entrada y salida estándares. Para ello utilizaremos el módulo prompt-sync de Node.js. (Puede instalar prompt-sync con `npm install prompt-sync`.)

```javascript
// Ejemplo 2: añade información de entrada del usuario y detecta intenciones.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el derivador del servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir por nombre de usuario de la clave de servicio
  password: 'PASSWORD', // sustituir por contraseña de la clave de servicio
  path: { workspace_id: 'WORKSPACE_ID' }, // sustituir por ID de espacio de trabajo
  version_date: '2016-07-11'
});

// Iniciar conversación con mensaje vacío.
conversation.message({}, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  // Si se detecta una intención, registrarla en la consola.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Mostrar la salida del diálogo, si la hay.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Solicitar la siguiente ronda de información de entrada.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}

Esta versión de la aplicación empieza igual que antes: enviando un mensaje vacío al servicio {{site.data.keyword.conversationshort}} para iniciar la conversación.

Ahora la función `processResponse()` muestra las intenciones que detecta el diálogo junto con el texto de salida, y luego solicita la siguiente ronda de información de entrada de usuario.

Pero aún hay algo que no es correcto:

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

El servicio {{site.data.keyword.conversationshort}} detecta las intenciones correctas, y cada ronda de la conversación devuelve el mensaje de bienvenida procedente del nodo conversation_start (`Welcome to the Conversation example!`).

Esto se debe a que el servicio {{site.data.keyword.conversationshort}} no tiene estado; es responsabilidad de la aplicación mantener la información de estado. Puesto que todavía no se está haciendo nada para mantener el estado, el servicio {{site.data.keyword.conversationshort}} ve cada ronda de información de entrada de usuario como la primera ronda de una nueva conversación, por lo que activa el nodo conversation_start.

## Mantenimiento del estado

La información de estado de la conversación se mantiene mediante un *contexto*. El contexto es un objeto JSON que se transfiere entre la aplicación y el servicio {{site.data.keyword.conversationshort}} y viceversa. Es responsabilidad de la aplicación mantener el contexto entre una ronda de la conversación y la siguiente.

El contexto incluye un identificador exclusivo para cada conversación con un usuario, así como un contador que se incrementa con cada ronda de la conversación. La versión anterior del ejemplo no preservaba el contexto, lo que significa que cada ronda de entrada parecía el principio de una nueva conversación. Podemos arreglarlo guardando el contexto y enviándolo de nuevo al servicio {{site.data.keyword.conversationshort}} cada vez.

Además de mantener nuestro lugar en la conversación, el contexto también se puede utilizar para almacenar cualquier dato que desee transferir entre la aplicación y el servicio {{site.data.keyword.conversationshort}} y viceversa. Esto puede incluir datos permanente que desea mantener durante toda la conversación (como el nombre de un cliente o un número de cuenta), o cualquier otra información de la que desee realizar un seguimiento (como, por ejemplo, el estado actual de los valores de las opciones).

```javascript
// Ejemplo 3: mantiene el estado.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir por nombre de usuario de la clave de servicio
  password: 'PASSWORD', // sustituir por contraseña de la clave de servicio
  path: { workspace_id: 'WORKSPACE_ID' }, // sustituir por ID de espacio de trabajo
  version_date: '2016-07-11'
});

// Iniciar conversación con mensaje vacío.
conversation.message({}, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  // Si se detecta una intención, registrarla en la consola.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Mostrar la salida del diálogo, si la hay.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Solicitar la siguiente ronda de información de entrada.
    var newMessageFromUser = prompt('>> ');
    // Enviar el contexto para mantener el estado.
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}

El único cambio con respecto al ejemplo anterior es que, con cada ronda de la conversación, ahora enviamos el objeto `response.context` que hemos recibido en la ronda anterior:

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}

Esto garantiza que se mantiene el contexto entre una ronda y la siguiente, de modo que el servicio {{site.data.keyword.conversationshort}} ya no piensa que cada ronda es la primera:

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

¡Ahora estamos avanzando! El servicio {{site.data.keyword.conversationshort}} reconoce correctamente nuestras intenciones y el diálogo devuelve el texto de salida correcto (cuando se proporciona) para cada intención.

Sin embargo, no sucede nada más. Cuando preguntamos la hora, no objetemos respuesta, y cuando decimos adiós, la conversación no termina. Esto se debe a que estas intenciones requieren que la app lleve a cabo acciones adicionales.

## Implementación de acciones de la app

Además del texto de salida que se muestra al usuario, nuestro diálogo utiliza el objeto `output` en el JSON de la respuesta para indicar cuándo la aplicación debe llevar a cabo una acción, en función de las intenciones detectadas.

Estos distintivos de acción se envían mediante la propiedad `action`, que nuestro diálogo define como parte del JSON de respuesta. Cuando el diálogo determina que la aplicación debe hacer algo, establece el valor de `action` en el valor adecuado, ya sea `display_time` o `end_conversation`.

Tenga en cuenta que `output` es simplemente un objeto JSON, y puede añadir cualquier contenido válido que desee. Para una aplicación más compleja, puede utilizar una matriz con varios distintivos de acción.

Pero, en nuestro ejemplo, utilizando un sencillo par de clave/valor que da soporte a un solo distintivo de acción. Nuestro código de aplicación debe comprobar el valor de la propiedad `action` en la respuesta y llevar a cabo la acción especificada. (Esta versión también elimina la visualización de las intenciones detectadas, ahora que estamos seguros de que se han definido correctamente.)

```javascript
// Ejemplo 4: implementa acciones de la app.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir por nombre de usuario de la clave de servicio
  password: 'PASSWORD', // sustituir por contraseña de la clave de servicio
  path: { workspace_id: 'WORKSPACE_ID' }, // sustituir por ID de espacio de trabajo
  version_date: '2016-07-11'
});

// Iniciar conversación con mensaje vacío.
conversation.message({}, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  var endConversation = false;

  // Comprobar los distintivos de la acción.
  if (response.output.action === 'display_time') {
    // El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // El usuario se ha despedido, así que hemos terminado.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Mostrar la salida del diálogo, si la hay.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      input: { text: newMessageFromUser },
      // Enviar el contexto para mantener el estado.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}

Ahora la función processResponse() comprueba el valor de la propiedad `action` del objeto `output` recibido del servicio {{site.data.keyword.conversationshort}}. Si el valor es `display_time` o `end_conversation`, la aplicación lleva a cabo la acción adecuada.

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

Correcto. Ahora la aplicación utiliza el servicio {{site.data.keyword.conversationshort}} para identificar las intenciones en la entrada de lenguaje natural, muestra las respuestas adecuadas e implementa las acciones solicitadas.

Por supuesto, una aplicación real utilizaría una interfaz de usuario más sofisticadas, como por ejemplo una ventana de conversación web. Y también implementaría acciones más complejas, posiblemente integrando una base de datos de clientes u otros sistemas empresariales. Pero los principios básicos sobre cómo la aplicación interactúa con el servicio {{site.data.keyword.conversationshort}} seguirían siendo los mismos.

Para ver ejemplos más complejos, consulte las apps de ejemplo del panel de navegación.
