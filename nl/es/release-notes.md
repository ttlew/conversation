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

# Notas del release

## Versiones de la API del servicio
{: shortdesc}

Las solicitudes de API requieren un parámetro de versión que toma una fecha en el formato `version=AAAA-MM-DD`. Siempre que cambiamos la API de forma que sea compatible con versiones anteriores, ponemos a su disposición una versión menor de la API.

Envíe el parámetro version con cada solicitud de API. El servicio utiliza la versión de la API correspondiente a la fecha que especifique, o la versión más reciente anterior a dicha fecha. No se utiliza como valor predeterminado la fecha actual. Especifique una fecha que coincida con una versión que sea compatible con su app y no la modifique hasta que la app esté lista para una versión posterior.

La versión actual es `2017-05-26`.

## Características beta

IBM pondrá a su disposición servicios, características y soporte de idiomas clasificados como beta (antes se denominaban _experimentales_). Estos servicios pueden ser inestables, pueden cambiar con frecuencia y pueden dejar de recibir soporte tras un aviso con poca antelación. Las características beta solo reciben soporte mediante nuestro foro de {{site.data.keyword.Bluemix_notm}}.

## Modelos actualizados
{: #updated-models}

Los algoritmos de {{site.data.keyword.conversationshort}} se pueden perfeccionar y actualizar periódicamente en función de comentarios, mejoras científicas y otros factores, a fin de mejorar continuamente el rendimiento. Las actualizaciones del modelo se comunicarán en estas notas del release.

Los modelos que ha formado no se verán afectados de inmediato, pero los modelos caducados se actualizarán al modelo actual, si todavía no lo ha hecho, transcurridos 60 días desde la disponibilidad de un nuevo modelo.

> **Nota:** Esta declaración sobre actualización se aplica solo a características e idiomas disponibles a nivel general (GA).

## Cambios
{: #change-log}

Están disponibles las siguientes nuevas características y cambios en el servicio.

### 26 de octubre de 2017
{: #26October2017}

- **Actualizaciones para chino simplificado** - El soporte del idioma se ha mejorado para el chino simplificado. Esto incluye mejoras en la clasificación de intenciones utilizando inclusiones de palabras a nivel de carácter y la disponibilidad de entidades del sistema. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](release-notes.html#updated-models) para obtener más información.
- **Actualizaciones para español** - Se han realizado mejoras en la clasificación de intenciones en español para conjuntos de datos de gran tamaño.

### 11 de octubre de 2017
{: #11October2017}

- **Actualizaciones para coreano ** - El soporte del idioma se ha mejorado para el coreano. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](release-notes.html#updated-models) para obtener más información.

### 3 de octubre de 2017
{: #3October2017}

- **Entidades definidas por patrón (Beta)**: Ahora puede definir patrones específicos para una entidad, utilizando expresiones regulares. Esto le puede ayudar a identificar las entidades que siguen un determinado patrón, por ejemplo números de referencia o números de pieza, números de teléfono o direcciones de correo electrónico. Consulte [Entidades definidas por patrón](entities.html#pattern-entities) para ver detalles adicionales.
    - Puede añadir sinónimos o patrones para un solo valor de entidad; no puede añadir ambos.
    - Para cada valor de entidad, puede haber un máximo de 5 patrones.
    - Cada patrón (expresión regular) está limitado a 128 caracteres.
    - La importación o exportación mediante un archivo CSV no da soporte a patrones actualmente.
    - La API REST no da soporte al acceso directo a patrones, pero puede recuperar o modificar patrones utilizando el punto final `/values`.

- **Coincidencia aproximada filtrada por diccionario (solo inglés)**: Ahora dispone de una versión mejorada de la coincidencia aproximada para entidades, en inglés. Esta mejora evita la captura de algunas palabras comunes válidas en inglés como coincidencias aproximadas para una entidad determinada. Por ejemplo, la coincidencia aproximada no hará coincidir el valor de entidad `like` con `hike` ni con `bike`, que son palabras válidas en inglés, pero seguirá coincidiendo con ejemplos como `lkie` u `oike`.

### 27 de septiembre de 2017
{: #27September2017}

- **Actualizaciones en el generador de condiciones**: El control que se muestra para ayudarle a definir una condición en un nodo de diálogo se ha actualizado. Las mejoras incluyen soporte para listar nombres de variables de contexto después de escribir $ para empezar a añadir una variable de contexto.

### 31 de agosto de 2017
{: #31August2017}

- **Reversión de la sección Mejorar** - La medida de tiempo medio de conversación, y los filtros correspondientes, se han eliminado temporalmente de la página Visión general de la sección Mejorar. Esta eliminación impedirá que el cálculo de determinadas medidas de tiempo medio de conversación y el gráfico de conversaciones en el tiempo muestren información imprecisa. IBM lamenta eliminar esta función de la herramienta, pero se compromete a garantizar que se comunica información precisa a los usuarios.
- **Nombres de nodos del diálogo** - Ahora puede asignar cualquier nombre a un nodo del diálogo; no es necesario que sea exclusivo. Y por lo tanto puede cambiar el nombre de un nodo sin que ello afecte al modo en que se hace referencia al nodo internamente. El nombre que especifique se guarda como un atributo de título del nodo en el archivo JSON del espacio de trabajo y el sistema utiliza un ID exclusivo que se guarda en el atributo de nombre para hacer referencia al nodo.

### 23 de agosto de 2017
{: #23August2017}

- **Actualizaciones en coreano, japonés e italiano** - Se ha mejorado el soporte de los idiomas coreano, japonés e italiano. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](release-notes.html#updated-models) para obtener más información.

### 10 de agosto de 2017
{: #10August2017}

- **Normalización de acentos:** En un entorno conversacional, puede que los usuarios utilicen o no acentos cuando interactúan con el servicio Conversation. Por lo tanto, se ha actualizado el algoritmo de modo que las versiones acentuadas y no acentuadas de las palabras se traten de la misma manera para la detección de intenciones y el reconocimiento de entidades.

  Sin embargo, para algunos idiomas, como el español, algunos acentos puede alterar el significado de la entidad. Por lo tanto, para la detección de entidades, aunque la entidad original pueda tener implícitamente un acento, el servicio también puede considerar como coincidencia la versión no acentuada de la misma entidad, pero con una puntuación de confianza ligeramente inferior.

  Por ejemplo, para la palabra "barrió", que lleva acento y corresponde al pretérito indefinido del verbo "barrer", el servicio también puede considerar como coincidencia la palabra "barrio", pero con un nivel de confianza ligeramente inferior.

  El sistema proporcionará la mayor puntuación de confianza en entidades con coincidencias exactas. Por ejemplo, `barrio` no se detectará si `barrió` está en el conjunto de datos de formación; y `barrió` no se detectará si `barrio` está en el conjunto de datos de formación.

  Se espera que forme el sistema con los caracteres y acentos adecuados. Por ejemplo, si espera `barrió` como respuesta, debe colocar `barrió` en el conjunto de datos de formación.

  Aunque no lleven acento, lo mismo se aplica a las palabras que utilizan, por ejemplo, la letra `ñ` frente a la letra `n`, como "uña" frente a "una". En este caso la letra `ñ` no es simplemente una `n` con virgulilla, sino que es una letra específica del español.

  Puede habilitar la coincidencia aproximada si cree que los clientes no utilizarán los acentos correctamente o cometerán errores ortográficos en algunas palabras (por ejemplo, colocando una `n` en lugar de una `ñ`), o bien puede excluirlas de forma explícita de los ejemplos de formación.

  **Nota:** La normalización de acentos está habilitada para portugués, español, francés y checo.

- **Distintivo opt-out del espacio de trabajo:** Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte a un distintivo opt-out para espacios de trabajo. Este distintivo indica que los datos de formación del espacio de trabajo, como intenciones y entidades, no serán utilizados por IBM para realizar mejoras en el servicio general. Para obtener más información, vea la [Consulta de API![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}

### 7 de agosto de 2017
{: #7August2017}

- **Interpretación de `siguiente` y `última` fecha** - El servicio Conversation trata las fechas "última" ("last") y "siguiente" ("next") como si hicieran referencia al último o al siguiente día más inmediato, que puede estar dentro de la misma semana o en la anterior. Consulte el tema sobre [entidades del sistema](system-entities.html#sys-datetime) para obtener más información.

### 3 de agosto de 2017
{: #3August2017}

- **Coincidencia aproximada para idiomas adicionales (Beta)** - La coincidencia aproximada para entidades ahora está disponible para más idiomas, tal como se indica en el tema [Idiomas soportados](lang-support.html).
- **Coincidencia parcial (Beta - solo inglés)** - Ahora la coincidencia aproximada sugerirá automáticamente sinónimos basados en subseries que aparecen en entidades definidas por el usuario y asignará una puntuación de confianza inferior en comparación con la coincidencia de entidad exacta. Consulte [Coincidencia aproximada](entities.html#fuzzy-matching) para ver detalles.

### 28 de julio de 2017
{: #28July2017}

- Cuando establece preferencias bidireccionales para la herramienta, ahora puede especificar la dirección de la interfaz gráfica de usuario.
- El esquema de colores de la herramienta se ha actualizada para adaptarla a otros servicios y productos Watson.

### 19 de julio de 2017
{: #19July2017}

- Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte al acceso a nodos del diálogo. Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14 de julio de 2017
{: #14July2017}

- La funcionalidad de ranuras de los diálogos se ha mejorado. Por ejemplo, se ha añadido la propiedad *slot_in_focus* que puede utilizar para definir una condición que solo se aplica a una ranura. Consulte [Obtención de información con ranuras](/docs/services/conversation/dialog-build.html#slots) para ver detalles.

### 12 de julio de 2017
{: #12July2017}

- **Soporte de checo** - Se ha incorporado soporte para el idioma checo; consulte el tema [Idiomas soportados](lang-support.html) para ver más detalles.

### 11 de julio de 2017
{: #11July2017}

- **Probar en Slack** - Puede utilizar la nueva herramienta **Probar en Slack** para desplegar rápidamente su espacio de trabajo como usuario de bot de Slack para realizar pruebas. Esta herramienta sólo está disponible para la región EE.UU. Sur de {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte [Prueba de Slack](test-deploy.html).
- **Actualizaciones para árabe** - Se ha mejorado el soporte del idioma árabe para incluir puntuación absoluta por intención y la posibilidad de marcar intenciones como irrelevantes; consulte el tema [Idiomas soportados](lang-support.html) para ver más detalles. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](release-notes.html#updated-models) para obtener más información.

### 23 de junio de 2017
{: #23June2017}

- **Actualizaciones para coreano** - Se ha mejorado el soporte del idioma coreano; consulte el tema [Idiomas soportados](lang-support.html) para ver más detalles. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](release-notes.html#updated-models) para obtener más información.

### 22 de junio de 2017
{: #22June2017}

- **Incorporación de ranuras** - Ahora es más fácil recopilar varios fragmentos de información de un usuario en un solo nodo mediante la adición de ranuras. Anteriormente, se tenían que crear varios nodos de diálogo para que cubrieran todas las combinaciones posibles de formas en que los usuarios pueden proporcionar información. Con las ranuras, puede configurar un solo nodo que guarde cualquier información proporcionada por el usuario y que solicite detalles necesarios que el usuario no ha proporcionado. Consulte [Obtención de información con ranuras](dialog-build.html#slots) para ver más detalles.
- **Árbol de diálogo simplificado** - Se ha rediseñado el árbol de diálogo para facilitar su uso. La vista del árbol es más compacta para que resulte más fácil ver en qué punto se encuentra uno. Y los enlaces entre nodos se representan de forma que facilitan la comprensión de las relaciones entre los nodos.

### 21 de junio de 2017
{: #21June2017}

- **Soporte de árabe** - Ahora el soporte del idioma árabe está disponible a nivel general. Para obtener más información, consulte [Configuración de idiomas bidireccionales](lang-support.html#configuring-bi-directional).
- **Actualizaciones de idiomas** - Los algoritmos del servicio {{site.data.keyword.conversationshort}} se han actualizado para mejorar el soporte general de idiomas. Consulte el tema sobre [Idiomas soportados](lang-support.html) para obtener más información.

### 16 de junio de 2017
{: #16June2017}

- **Recomendaciones (solo usuarios de las versiones Beta - Premium)** - El panel Mejorar también incluye una página **Recomendaciones** que sugiere maneras de mejorar el sistema analizando las conversaciones que los usuarios tienen con el chatbot y teniendo en cuenta los datos de formación y la certeza de respuestas actuales del sistema.

### 14 de junio de 2017
{: #14June2017}

- **Coincidencia aproximada para idiomas adicionales (Beta)** - La coincidencia aproximada para entidades ahora está disponible para más idiomas, tal como se indica en el tema [Idiomas soportados](lang-support.html). Puede activar la coincidencia aproximada por entidad para mejorar la capacidad del servicio para reconocer términos de entrada de usuario con una sintaxis parecida a la entidad, pero sin necesidad de que la coincidencia sea exacta. La característica es capaz de correlacionar la entrada de usuario con la entidad adecuada correspondiente, a pesar de que contenga errores ortográficos o ligeras diferencias sintácticas. Por ejemplo, si define giraffe como sinónimo para una entidad animal y la entrada de usuario contiene los términos giraffes o girafe, la coincidencia aproximada es capaz de correlacionar correctamente el término con la entidad animal. Consulte [Coincidencia aproximada](entities.html#fuzzy-matching) para ver detalles.

### 13 de junio de 2017
{: #13June2017}

- **Conversaciones de usuario** - El panel Mejorar ahora incluye una página **Conversaciones de usuario** que proporciona una lista de interacciones del usuario con el chatbot que se puede filtrar por palabra clave, intención, entidad o número de días. Puede abrir conversaciones individuales para corregir intenciones o para añadir valores de entidad o sinónimos.
- **Cambio de expresiones regulares (Regex)** - Las expresiones regulares que reciben soporte de funciones de SpEL como find, matches, extract, replaceFirst, replaceAll y split se han modificado. Ya no se permite un grupo de construcciones de expresiones regulares, incluidas las construcciones look-ahead, look-behind, possessive repetition y backreference. Este cambio responde a la necesidad de evitar exposiciones de seguridad en la biblioteca de expresiones regulares original.

### 12 de junio de 2017
{: #12June2017}

- El número máximo de espacios de trabajo que puede crear con el plan **Lite** (antes denominado plan Gratuito) ha pasado de 3 a 5.
- Ahora puede asignar cualquier nombre a un nodo del diálogo; no es necesario que sea exclusivo. Y por lo tanto puede cambiar el nombre de un nodo sin que ello afecte al modo en que se hace referencia al nodo internamente. El nombre que especifique se trata como un alias y el sistema utiliza su propio identificador interno para hacer referencia al nodo.
- Ya no se puede cambiar el idioma de un espacio de trabajo después de crearlo editando los detalles del espacio de trabajo. Si necesita cambiar el idioma, puede exportar el espacio de trabajo como un archivo JSON, actualizar la propiedad de idioma, y luego importar el archivo JSON como un nuevo espacio de trabajo.

### 6 de junio de 2017
{: #6June2017}

- **Información** - Dispone de una nueva página *Más información sobre {{site.data.keyword.watson}} {{site.data.keyword.conversationshort}}* que ofrece información de iniciación y enlaces con la documentación del servicio y otros recursos que le resultarán útiles. Para abrir la página, pulse el icono ![i para información](images/info.png) en la cabecera de la página.
- **Exportación y supresión masivas** - Ahora puede exportar simultáneamente varias intenciones o entidades a un archivo CSV, para luego poderlas importar y reutilizar para otra aplicación de {{site.data.keyword.conversationshort}}. También puede seleccionar simultáneamente varias entidades o intenciones para suprimirlas simultáneamente.
- **Actualizaciones para coreano ** - Se han actualizado cuatro señaladores de coreano para dar soporte al lenguaje informal. IBM sigue trabajando para mejorar el reconocimiento y la clasificación de entidades.
- **Soporte de emojis** - Los emojis que se añaden a ejemplos de intenciones, o como valores de entidad, ahora se clasificarán y extraerán correctamente. **Nota**: Solo los emojis incluidos en los datos de formación se identificarán correctamente; es posible que el soporte de emojis no clasifique correctamente emojis parecidos con distintos tones de colores u otras variaciones.
- **Lematización de entidades (solo versiones Beta - inglés)** - La característica beta de coincidencia aproximada reconoce entidades y las compara en función de la forma del lema del valor de la entidad. Por ejemplo, esta característica reconoce correctamente 'bananas' como similar a 'banana' y 'run' como similar a 'running' ya que comparten un lexema común. Para obtener más información, consulte [Coincidencia aproximada](entities.html#fuzzy-matching).
- **Progreso de la importación de espacios de trabajo** - Cuando importa un espacio desde un archivo JSON, se muestra inmediatamente un mosaico correspondiente al espacio de trabajo, en el que puede ver información sobre el progreso de la importación.
- **Reducción del tiempo de formación** - Ahora se forman varios modelos en paralelo, lo que reduce significativamente el tiempo de formación para espacios de trabajo de gran tamaño.

### 26 de mayo de 2017
{: #26May2017}

- Ahora la versión de API actual es `2017-05-26`. Esta versión presenta los cambios siguientes:
    - El esquema de objetos ErrorResponse se ha modificado. Este cambio afecta a todos los puntos finales y métodos. Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - El esquema interno utilizado para representar nodos del diálogo en el archivo JSON del espacio de trabajo exportado ha cambiado. Si utiliza la API `2017-05-26` para importar un espacio de trabajo que se ha exportado utilizando una versión anterior, es posible que algunos nodos del diálogo no se importen correctamente. Para obtener mejores resultados, importe siempre un espacio de trabajo utilizando la misma versión que utilizó para exportarlo.

### 25 de mayo de 2017
{: #25May2017}

- Ahora puede gestionar variables de contexto en el panel "Pruébelo". Pulse el enlace **Gestionar contexto** para abrir un nuevo panel donde puede establecer y comprobar los valores de las variables de contexto cuando prueba el diálogo. Consulte [Prueba del diálogo](dialog-test.html) para obtener más información.

### 16 de mayo de 2017
{: #16May2017}

- Ahora dispone de un espacio de trabajo de ejemplo denominado **Car Dashboard** cuando abre la herramienta. Para utilizar el ejemplo como punto de partida para su propio espacio de trabajo, edite el espacio de trabajo. Si desea utilizarlo para varios espacios de trabajo, duplíquelo. El espacio de trabajo de ejemplo no cuenta en el total de espacios de trabajo de la suscripción, a menos que lo utilice.
- Ahora resulta más sencillo navegar por la herramienta. Las opciones del menú de navegación se muestran en el lateral de la página principal en lugar de en la parte superior. En la parte superior de la página, se muestran los enlaces de la ruta de navegación para mostrarle dónde está. Ahora puede conmutar entre instancias del servicio desde la página Espacios de trabajo. Para hasta allí rápidamente, pulse **Volver a espacios de trabajo** en el menú de navegación. Si tiene varias instancias del servicio, se muestra el nombre de la instancia actual. Puede pulsar el enlace **Cambiar** que hay junto a la instancia para elegir otra instancia.
- Cuando se crea un diálogo, ahora se añaden automáticamente a la misma dos nodos: 1) un nodo **Welcome** en la parte superior del árbol del diálogo que contiene el saludo que se mostrará al usuario y 2) un nodo **Anything else** en la parte inferior del árbol que detecta las consultas de los usuarios que no reconocen otros nodos del diálogo y responde a las mismas. Consulte [Creación de un diálogo](dialog-create.html) para obtener más detalles.
- Cuando pruebe un diálogo en el panel "Pruébelo", ahora puede encontrar y volver a enviar una expresión de prueba reciente pulsando la tecla Subir para recorrer entradas anteriores.
- Ahora dispone de soporte experimental del idioma coreano para 5 entidades del sistema (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`). Existen problemas conocidos sobre algunas de las entidades numéricas y soporte limitado para entradas en lenguaje informal.
- Dispone de una página Visión en el separador Mejorar. La página proporciona un resumen de las interacciones con el bot. Puede ver la cantidad de tráfico durante un periodo de tiempo determinado, así como las intenciones y entidades que se han reconocido con mayor frecuencia en las conversaciones de los usuarios. Para obtener más información, consulte [Utilización de la página Visión general](logs_oview.html).

### 27 de abril de 2017
{: #27April2017}

- Las siguientes entidades del sistema están ahora disponibles como características beta solo en inglés:
    - sys-location: Reconoce referencias a ubicaciones, como pueblos, ciudades y países, en expresiones de usuario.
    - sys-person: Reconoce referencias a nombres de personas, nombre y apellido, en expresiones de usuario.

    Para obtener más información, vea la [Consulta de entidades del sistema](system-entities.html).
- La coincidencia aproximada para entidades es una característica beta que ahora está disponible en inglés. Puede activar la coincidencia aproximada por entidad para mejorar la capacidad del servicio para reconocer términos de entrada de usuario con una sintaxis parecida a la entidad, pero sin necesidad de que la coincidencia sea exacta. La característica es capaz de correlacionar la entrada de usuario con la entidad adecuada correspondiente, a pesar de que contenga errores ortográficos o ligeras diferencias sintácticas. Por ejemplo, si define **giraffe** como sinónimo para una entidad animal y la entrada de usuario contiene los términos *giraffes* o *girafe*, la coincidencia aproximada es capaz de correlacionar correctamente el término con la entidad animal. Consulte [Definición de entidades](entities.html#fuzzy-matching) y busque "Coincidencia aproximada" para obtener detalles.

### 18 de abril de 2017
{: #18April2017}

- Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte al acceso a los siguientes recursos:
    - entidades
    - valores de entidad
    - sinónimos de valores de entidad
    - registros

    Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
- El comportamiento del método `POST` /messages ha modificado el manejo de entidades e intenciones especificadas como parte de la entrada del mensaje:
    - Si especifica intenciones en la entrada, el servicio utiliza las intenciones que especifica, pero utiliza el proceso de lenguaje natural para detectar entidades en la entrada de usuario.
    - Si especifica entidades en la entrada, el servicio utiliza las entidades que especifica, pero utiliza el proceso de lenguaje natural para detectar intenciones en la entrada de usuario.

        El comportamiento no ha cambiado para los mensajes que especifican tanto intenciones como entidades ni para los mensajes que no especifican ninguna de ellas.
- La opción para marcar la entrada de usuario como irrelevante ahora está disponible para todos los idiomas soportados. Esta es una característica beta.
- Un nuevo separador Credenciales proporciona una ubicación única en la que puede encontrar toda la información que necesita para conectar la aplicación a un espacio de trabajo (como credenciales de servicio y el ID del espacio de trabajo), así como otras opciones de despliegue. Para acceder al separador Credenciales correspondiente a su espacio de trabajo, pulse el icono ![Menú](images/Menu_16.png) y seleccione **Credenciales**.

### 9 de marzo de 2017
{: #9March2017}

Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte al acceso a los siguientes recursos:

- espacios de trabajo
- intenciones
- ejemplos
- contraejemplos

Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7 de marzo de 2017
{: #7March2017}

- El uso de `.` o `..` como nombre de intención ocasiona problemas y ya no recibe soporte.

    No puede cambiar el nombre ni suprimir una intención con este nombre; para cambiar el nombre, exporte las intenciones a un archivo, cambie el nombre de la intención en el archivo e importar el archivo actualizado en el espacio de trabajo.

    Los clientes de pago pueden ponerse en contacto con el equipo de soporte para un cambio de base de datos.

### 1 de marzo de 2017
{: #1March2017}

- Ahora las entidades del sistema están habilitadas en alemán.

### 22 de febrero de 2017
{: #22February2017}

- Ahora los mensajes están limitados a 2.048 caracteres.

### 3 de febrero de 2017
{: #3February2017}

- En este release, hemos cambiado la forma en que se puntúan las intenciones y hemos añadido la posibilidad de marcar una entrada como irrelevante para la aplicación. [Para obtener más información, consulte Definición de intenciones](intents.html#mark-irrelevant) y busque "Marcar como irrelevante".
    - Estas características están disponibles en la herramienta mediante una [actualización del espacio de trabajo](upgrading.html).
    - Estas características están disponibles para su aplicación mediante un cambio en la llamada de la API de mensajes para que utilice **2017-02-03.**
- El proceso de las acciones **Ir a** se ha modificado para evitar los bucles que se pueden producir bajo determinadas circunstancias.

    Consulte [Acciones Ir a](dialog-build.html#jump-to) para obtener detalles.

### 11 de enero de 2017
{: #11January2017}

- En este release, puede personalizar los títulos de los nodos en el diálogo.

### 22 de diciembre de 2016
{: #22December2016}

- En este release, los nodos del diálogo muestran una nueva sección para `título de nodo`. Ahora se puede personalizar el `título de nodo`. Cuando está contraído, el `título de nodo` muestra la `condición de nodo` del nodo del diálogo. Si no hay ninguna `condición de nodo`, se muestra como título "Nodo sin título".

### 19 de diciembre de 2016
{: #19December2016}

Se han realizado cambios en el generador de diálogos para que resulte más intuitivo:

- Una vista de edición mayor facilita la consulta de todos los detalles de un nodo cuando trabaja en el mismo.
- Un nodo puede contener varias respuestas, cada una activada por una condición distinta. Para obtener más información, consulte [Varias respuestas](dialog-build.html#responses).

### 5 de diciembre de 2016
{: #5December2016}

- Se da soporte a nuevos idiomas, todos en modalidad experimental: alemán, chino tradicional, chino simplificado y neerlandés.
- Están disponibles dos nuevas entidades del sistema: @sys-date y @sys-time. Para obtener más información, consulte [Entidades del sistema](system-entities.html).

### 21 de octubre de 2016
{: #21October2016}

- Ahora el servicio {{site.data.keyword.conversationshort}} proporciona entidades del sistema, que son entidades comunes que puede utilizar en cualquier caso de uso. Para obtener más información, consulte [Definición de entidades](entities.html) y busque "Habilitación de entidades del sistema".
- Ahora puede ver un historial de conversaciones con usuarios en la página Mejorar. Puede utilizarlo para comprender el comportamiento de su bot. Para obtener más información, consulte [Mejora de la comprensión](logs.html).
- Ahora puede importar entidades desde un archivo de valores separados por comas (CSV), lo que le resultará de ayuda si tiene un gran número de entidades. Para obtener más información, consulte [Definición de entidades](entities.html) y busque "Importación de entidades".

### 20 de septiembre de 2016
{: #20September2016}

**Nueva versión**: 2016-09-20

Para aprovechar los cambios de una nueva versión, cambie el valor del parámetro `version` por la nueva fecha. Si no está listo para actualizar a esta versión, no cambie la fecha de versión.

- Versión **2016-09-20**: `dialog_stack` ha pasado de una matriz de series a una matriz de objetos JSON.

### 29 de agosto de 2016
{: #29August2016}

- Puede mover nodos del diálogo de una rama a otra, como hermanos o iguales. Para obtener más información, consulte [Creación de un diálogo](dialog-build.html) y busque "Mover un nodo del diálogo".
- Puede expandir la ventana del editor de JSON.
- Puede ver los registros del chat de las conversaciones del bot para comprender mejor su comportamiento. Puede filtrar por intenciones, entidades, fecha y hora. Para obtener más información, consulte [Mejora de la comprensión](logs.html)

### 11 de julio de 2016
{: #21July2016}

- Este release de disponibilidad general (GA) le permite trabajar con entidades y diálogos para crear un bot completamente funcional.

### 18 de mayo de 2016
{: #18May2016}

- Este release experimental de {{site.data.keyword.conversationshort}} incorpora la interfaz de usuario y le permite trabajar con espacios de trabajo, intenciones y ejemplos.
