---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-06"

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

# Actualización de espacios de trabajo

El servicio Conversation añade y actualiza periódicamente sus características. Aunque algunos de estos cambios se aplican automáticamente a su espacio de trabajo, las actualizaciones que tienen un gran impacto requieren una actualización manual.
{: shortdesc}

Si tiene una instancia que se creó antes del 3 de febrero de 2017 y su espacio de trabajo tiene una actualización disponible, se muestra el icono de actualización (![icono de actualización](images/upgrade.png)) para indicar que hay una actualización disponible. Seleccione este icono para abrir el diálogo Actualizar espacio de trabajo.

Cuando actualiza el espacio de trabajo, la última versión de la API se habilita en la herramienta y el panel "Pruébelo" empieza a utilizar las últimas características.

Después de actualizar un espacio de trabajo, no puede revertir el espacio de trabajo a una versión anterior.

## Actualización del espacio de trabajo
Para no obstaculizar la funcionalidad del espacio de trabajo:

1.  [Duplique el espacio de trabajo](configure-workspace.html#exporting-and-copying-workspaces).
2.  Actualice el espacio de trabajo duplicado.
3.  Realice pruebas para detectar si hay problemas en el espacio de trabajo actualizado.

Cuando termine de comprobar si hay problemas, aplique la actualización a la aplicación cambiando la llamada de API de mensajes para que utilice la versión **2017-02-03** o posterior.

## Cambio en las acciones Ir a
El proceso de las acciones **Ir a** ha cambiado con el release del 3 de febrero de 2017.

Anteriormente, si iba a la condición de un nodo y ni dicho nodo ni ninguno de sus nodos iguales tenían una condición que se evaluara como verdadera, el sistema saltaba al nodo de nivel raíz y buscaba un nodo cuya condición coincidiera con la entrada. En algunas situaciones este proceso creaba un bucle, que impedía el progreso del diálogo.

Con el nuevo proceso, si ni el nodo de destino ni sus iguales se evalúan como verdaderos, la ronda del diálogo finaliza. Cualquier respuesta generada se devuelve al usuario, y se devuelve un mensaje de error a la aplicación:

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

La siguiente entrada de usuario se maneja en el nivel raíz del diálogo.

Esta actualización puede cambiar el comportamiento de su diálogo si tiene acciones **Ir a** destinadas a nodos cuyas condiciones son falsas.

Si desea restaurar el modelo de proceso antiguo, añada un nodo igual final con la condición `true`. En la respuesta, utilice una acción **Ir a** destinada a la condición del primer nodo en el nivel raíz del árbol del diálogo.
