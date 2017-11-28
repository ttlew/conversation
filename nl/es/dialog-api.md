---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-21"

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

# Modificación de un diálogo mediante la API

La API REST de {{site.data.keyword.conversationshort}} permite modificar el diálogo mediante programación, sin utilizar la herramienta {{site.data.keyword.conversationshort}}. Puede utilizar la API /dialog_nodes para crear, suprimir o modificar nodos del diálogo.

Recuerde que el diálogo es un árbol de nodos interconectados y para que sea válido debe cumplir ciertas reglas. Esto significa que cualquier cambio que realice en un nodo del diálogo podría tener efectos en cascada en otros nodos o en la estructura del diálogo. Antes de utilizar la API /dialog_nodes para modificar el diálogo, asegúrese de comprender cómo los cambios afectarán al resto del diálogo.

Un diálogo válido siempre satisface los siguientes criterios:

- Cada nodo del diálogo tiene un ID exclusivo (propiedad `dialog_node`).
- Un nodo hijo es consciente de su nodo padre (propiedad `parent`). Sin embargo, un nodo padre no es consciente de sus hijos.
- Un nodo es consciente de su hermano inmediatamente anterior, si lo hay (propiedad `previous_sibling`). Esto significa que todos los hermanos de un determinado padre forman una lista enlazada, en la que cada nodo apunta al nodo anterior.
- Sólo un hijo de un determinado padre puede ser el primer hermano (lo que significa que su `previous_sibling` es nulo).
- Un nodo no puede apuntar a un hermano anterior que sea hijo de otro padre.
- Dos nodos no pueden apuntar al mismo hermano anterior.
- Un nodo puede especificar otro nodo para que sea el siguiente en ejecutarse (propiedad `next_step`).
- Un nodo no puede ser su propio padre ni su propio hermano.
- Un nodo de tipo `response_condition` no puede tener hijos.

En los ejemplos siguientes se muestra cómo diversas modificaciones pueden provocar cambios en cascada.

## Creación de un nodo
{: #create-node}

Supongamos que tenemos el siguiente árbol de diálogo sencillo:

![Diálogo de ejemplo](images/dialog_api_1.png)

Podemos crear un nodo nuevo realizando una solicitud POST a /dialog_nodes con el siguiente cuerpo:

```json
{
  "dialog_node": "node_8"
}
```

Ahora el diálogo tiene este aspecto:

![Diálogo de ejemplo 2](images/dialog_api_2.png)

Como **node_8** se ha creado sin especificar un valor para `parent` o `previous_sibling`, ahora es el primer nodo del diálogo. Observe que, además de crear **node_8**, el servicio también ha modificado **node_1** de modo que su propiedad `previous_sibling` apunte al nodo nuevo.

Puede crear un nodo en otro lugar del diálogo especificando el padre y el hermano anterior:

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

Los valores que especifique para `parent` y `previous_node` deben ser válidos:

- Ambos valores deben hacer referencia a nodos existentes.
- El padre especificado debe ser el mismo que el del hermano anterior (o `null`, si el hermano anterior no tiene padre).
- El padre no puede ser un nodo de tipo `response_condition`.

El diálogo resultante tiene este aspecto:

![Diálogo de ejemplo 3](images/dialog_api_3.png)

Además de crear **node_9**, el servicio actualiza automáticamente la propiedad `previous_sibling` de *node_6* para que apunte al nuevo nodo.

## Cómo mover un nodo a otro padre
{: #change-parent}

Vamos a mover **node_5** a otro padre con el método POST /dialog_nodes/node_5 con el siguiente cuerpo:

```json
{
  "parent": "node_1"
}
```

El valor que se especifique para `parent` debe ser válido:
- Debe hacer referencia a un nodo existente.
- No debe hacer referencia al nodo que se está modificando (un nodo no puede ser su propio padre).
- No debe hacer referencia a un descendiente del nodo que se está modificando.
- No debe hacer referencia a un nodo de tipo `response_condition`.

Esto da lugar a la siguiente estructura modificada:

![Diálogo de ejemplo 4](images/dialog_api_4.png)

Aquí han sucedido varias cosas:
- Cuando **node_5** se ha movido a su nuevo padre, **node_7** se ha movido con él (porque el `parent` de **node_7** no ha cambiado). Cuando se mueve un nodo, todos los descendientes de dicho nodo permanecen junto a él.
- Puesto que no se ha especificado un valor `previous_sibling` para **node_5**, ahora es el primer hermano bajo **node_1**.
- La propiedad `previous_sibling` de **node_4** se ha actualizado a `node_5`.
- La propiedad `previous_sibling` de **node_9** se ha actualizado a `null`, porque ahora es el primer hermano bajo **node_2**.

## Reordenación de hermanos
{: #change-sibling}

Ahora convertiremos **node_5** en el segundo hermano en lugar del primero. Lo hacemos con el método POST /dialog_nodes/node_5 con el siguiente cuerpo:

```json
{
  "previous_sibling": "node_4"
}
```

Cuando se modifica `previous_sibling`, el nuevo valor debe ser válido:
- Debe hacer referencia a un nodo existente
- No debe hacer referencia al nodo que se está modificando (un nodo no puede ser su propio hermano)
- Debe hacer referencia a un hijo del mismo padre (todos los hermanos deben tener el mismo padre)

La estructura cambia del siguiente modo:

![Diálogo de ejemplo 5](images/dialog_api_5.png)

Observe de nuevo que **node_7** permanece con su padre. Además, **node_4** se ha modificado de modo que su `previous_sibling` es `null`, porque ahora es el primer hermano.

## Supresión de un nodo
{: #delete-node}

Ahora vamos a suprimir **node_1**, utilizando el método DELETE /dialog_nodes/node_1.

El resultado es el siguiente:

![Diálogo de ejemplo 6](images/dialog_api_6.png)

Observe que **node_1**, **node_4**, **node_5** y **node_7** se han suprimido. Cuando se suprime un nodo, también se suprimen todos los descendientes de dicho nodo. Por lo tanto, si suprime un nodo base, realmente suprime una rama entera del árbol del diálogo. Cualquier otra referencia al nodo suprimido (como las referencias `next_step`) se cambian por `null`.

Además, **node_2** se actualiza para que apunte a **node_8** como su nuevo hermano anterior.

## Cambio del nombre de un nodo
{: #rename-node}

Por último, vamos a cambiar el nombre de **node_2** con el método POST /dialog_nodes/node_2 con el siguiente cuerpo:

```json
{
  "dialog_node": "node_X"
}
```

![Diálogo de ejemplo 7](images/dialog_api_7.png)

La estructura del diálogo no ha cambiado, pero una vez más varios nodos se han modificado para reflejar el nombre cambiado:

- Las propiedades `parent` de **node_9** y **node_6**
- La propiedad `previous_sibling` de **node_3**

Cualquier otra referencia al nodo suprimido (como las referencias `next_step`) también se han modificado.
