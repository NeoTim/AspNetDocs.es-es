---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: 'de #7 la iteración: agregar la funcionalidad de Ajax (C-) Microsoft Docs'
author: rick-anderson
description: En la séptima iteración, mejoramos la capacidad de respuesta y el rendimiento de nuestra aplicación mediante la adición de compatibilidad con Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 12d7348382bc55af049567922bd6963970a9421b
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542318"
---
# <a name="iteration-7--add-ajax-functionality-c"></a>Iteración n.º 7: Agregar funcionalidad de Ajax (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> En la séptima iteración, mejoramos la capacidad de respuesta y el rendimiento de nuestra aplicación mediante la adición de compatibilidad con Ajax.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación de administración de contactos ASP.NET MVC (C-)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto (nombres, números de teléfono y direcciones de correo electrónico) para una lista de personas.

Creamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de iteración múltiple es permitirle comprender el motivo de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, creamos Contact Manager de la manera más sencilla posible. Agregamos compatibilidad con operaciones básicas de base de datos: Crear, leer, actualizar y eliminar (CRUD).

- Iteración #2 - Hacer que la aplicación se vea bien. En esta iteración, mejoramos el aspecto de la aplicación modificando la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos validación básica del formulario. Evitamos que las personas envíen un formulario sin completar los campos de formulario obligatorios. También validamos direcciones de correo electrónico y números de teléfono.

- Iteración #4: haga que la aplicación se acople libremente. En esta cuarta iteración, aprovechamos varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, refactorizamos nuestra aplicación para usar el patrón Repository y el patrón Dependency Injection.

- Iteración #5: crear pruebas unitarias. En la quinta iteración, hacemos que nuestra aplicación sea más fácil de mantener y modificar agregando pruebas unitarias. Nos burlamos de nuestras clases de modelo de datos y creamos pruebas unitarias para nuestros controladores y lógica de validación.

- #6 de iteración: use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- de #7 iteración: agregue la funcionalidad DebAjax. En la séptima iteración, mejoramos la capacidad de respuesta y el rendimiento de nuestra aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta iteración de la aplicación Contact Manager, refactorizamos nuestra aplicación para hacer uso de Ajax. Aprovechando Ajax, hacemos que nuestra aplicación sea más receptiva. Podemos evitar representar una página completa cuando necesitamos actualizar solo una región determinada en una página.

Vamos a refactorizar nuestra vista de índice para que no tengamos que volver a mostrar toda la página cada vez que alguien selecciona un nuevo grupo de contacto. En su lugar, cuando alguien haga clic en un grupo de contactos, solo actualizaremos la lista de contactos y dejaremos el resto de la página en paz.

También cambiaremos el funcionamiento de nuestro enlace de eliminación. En lugar de mostrar una página de confirmación independiente, mostraremos un cuadro de diálogo de confirmación de JavaScript. Si confirma que desea eliminar un contacto, se realiza una operación HTTP DELETE en el servidor para eliminar el registro de contacto de la base de datos.

Además, aprovecharemos jQuery para agregar efectos de animación a nuestra vista de índice. Mostraremos una animación cuando la nueva lista de contactos se obtenga del servidor.

Por último, aprovecharemos la compatibilidad con ASP.NET framework AJAX para administrar el historial del explorador. Crearemos puntos de historial cada vez que realicemos una llamada ajax para actualizar la lista de contactos. De esa manera, los botones de retroceso y avance del navegador funcionarán.

## <a name="why-use-ajax"></a>¿Por qué Ajax?

El uso de Ajax tiene muchos beneficios. En primer lugar, agregar la funcionalidad de Ajax a una aplicación da como resultado una mejor experiencia de usuario. En una aplicación web normal, toda la página debe volver a publicarse en el servidor cada vez que un usuario realiza una acción. Cada vez que realiza una acción, el explorador se bloquea y el usuario debe esperar hasta que se recupere y vuelva a mostrar toda la página.

Esto sería una experiencia inaceptable en el caso de una aplicación de escritorio. Pero, tradicionalmente, vivíamos con esta mala experiencia de usuario en el caso de una aplicación web porque no sabíamos que podíamos hacerlo mejor. Pensamos que era una limitación de las aplicaciones web cuando, en realidad, era sólo una limitación de nuestra imaginación.

En una aplicación Ajax, no es necesario detener la experiencia del usuario solo para actualizar una página. En su lugar, puede realizar una solicitud asincrónica en segundo plano para actualizar la página. No se fuerza al usuario a esperar mientras se actualiza parte de la página.

Aprovechando Ajax, también puede mejorar el rendimiento de su aplicación. Considere cómo funciona la aplicación Contact Manager en este momento sin la funcionalidad de Ajax. Al hacer clic en un grupo de contactos, se debe volver a mostrar toda la vista de índice. La lista de contactos y la lista de grupos de contactos deben recuperarse del servidor de bases de datos. Todos estos datos deben pasarse a través del cable desde el servidor web al navegador web.

Sin embargo, después de agregar la funcionalidad de Ajax a nuestra aplicación, podemos evitar volver a mostrar toda la página cuando un usuario hace clic en un grupo de contactos. Ya no necesitamos tomar los grupos de contacto de la base de datos. Tampoco es necesario insertar toda la vista de índice a través del cable. Aprovechando Ajax, reducimos la cantidad de trabajo que nuestro servidor de bases de datos debe realizar y reducimos la cantidad de tráfico de red requerido por nuestra aplicación.

## <a name="don-t-be-afraid-of-ajax"></a>No tengas miedo de Ajax

Algunos desarrolladores evitan el uso de Ajax porque se preocupan por los navegadores de nivel inferior. Quieren asegurarse de que sus aplicaciones web seguirán funcionando cuando se accede a ella mediante un navegador que no es compatible con JavaScript. Dado que Ajax depende de JavaScript, algunos desarrolladores evitan el uso de Ajax.

Sin embargo, si tiene cuidado con la forma de implementar Ajax, puede crear aplicaciones que funcionen con exploradores de nivel superior y de nivel inferior. Nuestra aplicación Contact Manager funcionará con navegadores que admitan JavaScript y navegadores que no lo hagan.

Si utiliza la aplicación Contact Manager con un navegador compatible con JavaScript, tendrá una mejor experiencia de usuario. Por ejemplo, al hacer clic en un grupo de contactos, solo se actualizará la región de la página que muestra los contactos.

Si, por otro lado, utiliza la aplicación Contact Manager con un navegador que no admite JavaScript (o que tiene JavaScript deshabilitado), tendrá una experiencia de usuario un poco menos deseable. Por ejemplo, al hacer clic en un grupo de contactos, toda la vista de índice debe volver a publicarse en el explorador para mostrar la lista coincidente de contactos.

## <a name="adding-the-required-javascript-files"></a>Adición de los archivos JavaScript necesarios

Necesitaremos usar tres archivos JavaScript para agregar funcionalidad Ajax a nuestra aplicación. Los tres de estos archivos se incluyen en la carpeta Scripts de una nueva aplicación ASP.NET MVC.

Si tiene previsto utilizar Ajax en varias páginas de la aplicación, tiene sentido incluir los archivos JavaScript necesarios en la página maestra de vista de la aplicación s. De este modo, los archivos JavaScript se incluirán automáticamente en todas las páginas de la aplicación.

Agregue las siguientes opciones &lt;de&gt; JavaScript dentro de la etiqueta head de la página maestra de vista:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactorización de la vista de índice para usar Ajax

Vamos s comenzar modificando nuestra vista de índice para que al hacer clic en un grupo de contactos sólo actualiza la región de la vista que muestra los contactos. El cuadro rojo de la figura 1 contiene la región que queremos actualizar.

[![Actualización de solo contactos](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: Actualización de solo contactos[(Haga clic para ver la imagen de tamaño completo)](iteration-7-add-ajax-functionality-cs/_static/image2.png)

El primer paso es separar la parte de la vista que queremos actualizar de forma asincrónica en un parcial independiente (ver control de usuario). La sección de la vista de índice que muestra la tabla de contactos se ha movido al parcial del listado 1.

**Listado 1 - Vistas-Contacto-ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Observe que el parcial en el listado 1 tiene un modelo diferente de la vista de índice. El *atributo Inherits* en &lt;la&gt; directiva % - Page % especifica&lt;que&gt; el parcial hereda de la ViewUserControl Group clase.

La vista de índice actualizada se encuentra en el listado 2.

**Listado 2 - Vistas-Contacto-Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Hay dos cosas que debe notar acerca de la vista actualizada en el listado 2. En primer lugar, observe que todo el contenido movido al parcial se reemplaza por una llamada a Html.RenderPartial(). Se llama al método Html.RenderPartial() cuando se solicita por primera vez la vista Index para mostrar el conjunto inicial de contactos.

En segundo lugar, observe que el Html.ActionLink() utilizado para mostrar grupos de contactos se ha reemplazado por un Ajax.ActionLink(). Se llama a Ajax.ActionLink() con los siguientes parámetros:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

El primer parámetro representa el texto que se va a mostrar para el vínculo, el segundo parámetro representa los valores de ruta y el tercer parámetro representa las opciones de Ajax. En este caso, usamos la opción UpdateTargetId &lt;&gt; Ajax para apuntar a la etiqueta div HTML que queremos actualizar una vez completada la solicitud de Ajax. Queremos actualizar &lt;la&gt; etiqueta div con la nueva lista de contactos.

El método Index() actualizado del controlador Contact se encuentra en el listado 3.

**Listado 3 - Controllers-ContactController.cs (método Index)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

La acción index() actualizada devuelve condicionalmente una de dos cosas. Si una solicitud Ajax invoca la acción Index(), el controlador devuelve un parcial. De lo contrario, la acción Index() devuelve una vista completa.

Observe que la acción Index() no necesita devolver tantos datos cuando se invoca mediante una solicitud Ajax. En el contexto de una solicitud normal, la acción Index devuelve una lista de todos los grupos de contactos y el grupo de contactos seleccionado. En el contexto de una solicitud Ajax, la acción Index() devuelve solo el grupo seleccionado. Ajax significa menos trabajo en su servidor de base de datos.

Nuestra vista de índice modificada funciona en el caso de los navegadores de nivel superior y bajo nivel. Si hace clic en un grupo de contactos y su navegador admite JavaScript, solo se actualiza la región de la vista que contiene la lista de contactos. Si, por otro lado, su navegador no es compatible con JavaScript, se actualiza toda la vista.

Nuestra vista de índice actualizada tiene un problema. Al hacer clic en un grupo de contactos, el grupo seleccionado no se resalta. Dado que la lista de grupos se muestra fuera de la región que se actualiza durante una solicitud de Ajax, el grupo correcto no se resalta. Solucionaremos este problema en la siguiente sección.

## <a name="adding-jquery-animation-effects"></a>Adición de efectos de animación jQuery

Normalmente, al hacer clic en un vínculo de una página web, puede utilizar la barra de progreso del explorador para detectar si el explorador está recuperando activamente el contenido actualizado. Al realizar una solicitud Ajax, por otro lado, la barra de progreso del navegador no muestra ningún progreso. Esto puede poner nerviosos a los usuarios. ¿Cómo sabes si el navegador se ha congelado?

Hay varias maneras de indicar a un usuario que se está realizando el trabajo mientras se realiza una solicitud ajax. Un enfoque es mostrar una animación simple. Por ejemplo, puede desvanecer una región cuando comienza una solicitud ajax y desvanecerse en la región cuando se completa la solicitud.

Usaremos la biblioteca jQuery que se incluye con el marco de Microsoft ASP.NET MVC, para crear los efectos de animación. La vista de índice actualizada se encuentra en el listado 4.

**Listado 4 - Vistas-Contacto-Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Observe que la vista de índice actualizada contiene tres nuevas funciones de JavaScript. Las dos primeras funciones usan jQuery para desvanecerse y desvanecerse en la lista de contactos al hacer clic en un nuevo grupo de contactos. La tercera función muestra un mensaje de error cuando una solicitud Ajax produce un error (por ejemplo, tiempo de espera de red).

La primera función también se encarga de resaltar el grupo seleccionado. Se agrega un atributo seleccionado de clase al elemento primario (el elemento LI) del elemento en el que se ha clic. Una vez más, jQuery facilita la selección del elemento correcto y la adición de la clase CSS.

Estos scripts están vinculados a los vínculos de grupo con la ayuda del parámetro Ajax.ActionLink() AjaxOptions. La llamada al método Ajax.ActionLink() actualizada tiene este aspecto:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Adición de compatibilidad con el historial del navegador

Normalmente, al hacer clic en un vínculo para actualizar una página, se actualiza el historial del navegador. De esta manera, puede hacer clic en el botón Atrás del navegador para retroceder en el tiempo al estado anterior de la página. Por ejemplo, si hace clic en el grupo de contactos Amigos y, a continuación, hace clic en el grupo Contacto empresarial, puede hacer clic en el botón Atrás del explorador para volver al estado de la página cuando se seleccionó el grupo de contactos Amigos.

Desafortunadamente, realizar una solicitud Ajax no actualiza el historial del navegador automáticamente. Si hace clic en un grupo de contactos y la lista de contactos coincidentes se recupera con una solicitud de Ajax, el historial del explorador no se actualiza. No puede utilizar el botón Atrás del explorador para volver a un grupo de contactos después de seleccionar un nuevo grupo de contactos.

Si desea que los usuarios puedan utilizar el botón Atrás del navegador después de realizar solicitudes Ajax, entonces debe realizar un poco más de trabajo. Debe aprovechar la funcionalidad de administración del historial del explorador integrada en el marco de trabajo de ASP.NET AJAX.

ASP.NET historial del navegador AJAX, debe hacer tres cosas:

1. Habilite el historial del explorador estableciendo la propiedad enableBrowserHistory en true.
2. Guarde los puntos de historial cuando cambie el estado de una vista llamando al método addHistoryPoint().
3. Reconstruya el estado de la vista cuando se genere el evento navigate.

La vista de índice actualizada se encuentra en el listado 5.

**Listado 5 - Vistas-Contacto-Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

En el listado 5, el historial del navegador está habilitado en la función pageInit(). La función pageInit() también se utiliza para configurar el controlador de eventos para el evento navigate. El evento navigate se genera siempre que el botón Forward o Back del explorador hace que cambie el estado de la página.

Se llama al método beginContactList() al hacer clic en un grupo de contactos. Este método crea un nuevo punto de historial llamando al método addHistoryPoint(). El identificador del grupo de contactos en el que se ha clic se agrega al historial.

El identificador de grupo se recupera de un atributo expando en el vínculo del grupo de contactos. El vínculo se representa con la siguiente llamada a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

El último parámetro pasado a Ajax.ActionLink() agrega un atributo expando denominado groupid al vínculo (en minúsculas para compatibilidad con XHTML).

Cuando un usuario pulsa el botón Atrás o Adelante del navegador, se provoca el evento navigate y se llama al método navigate(). Este método actualiza los contactos que se muestran en la página para que coincidan con el estado de la página que corresponde al punto de historial del explorador pasado al método de navegación.

## <a name="performing-ajax-deletes"></a>Realización de eliminaciones de Ajax

Actualmente, para eliminar un contacto, usted necesita hacer clic en el link de la cancelación y después haga clic el botón de la cancelación visualizado en la página de confirmación de la cancelación (véase el cuadro 2). Esto parece una gran cantidad de solicitudes de página para hacer algo simple como eliminar un registro de base de datos.

[![La página de confirmación de eliminación](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: La página de confirmación de eliminación[(Haga clic para ver la imagen de tamaño completo)](iteration-7-add-ajax-functionality-cs/_static/image4.png)

Es tentador omitir la página de confirmación de eliminación y eliminar un contacto directamente de la vista de índice. Debe evitar esta tentación porque al adoptar este enfoque se abre la aplicación a los agujeros de seguridad. En general, no desea realizar una operación HTTP GET al invocar una acción que modifica el estado de la aplicación web. Al realizar una eliminación, desea realizar una POST HTTP, o mejor aún, una operación HTTP DELETE.

El vínculo Eliminar se encuentra en el parcial ContactList. En el listado 6 se incluye una versión actualizada del parcial ContactList.

**Listado 6 - Vistas-Contacto-ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

El vínculo Delete se representa con la siguiente llamada al método Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() no es una parte estándar del marco de ASP.NET MVC. Ajax.ImageActionLink() es un método auxiliar personalizado incluido en el proyecto Contact Manager.

El parámetro AjaxOptions tiene dos propiedades. En primer lugar, la propiedad Confirm se utiliza para mostrar un cuadro de diálogo de confirmación de JavaScript emergente. En segundo lugar, el HttpMethod propiedad se utiliza para realizar una operación HTTP DELETE.

El listado 7 contiene una nueva acción AjaxDelete() que se ha agregado al controlador Contact.

**Listado 7 - Controllers-ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

La acción AjaxDelete() está decorada con un atributo AcceptVerbs. Este atributo impide que la acción se invoque excepto por cualquier operación HTTP que no sea una operación HTTP DELETE. En particular, no puede invocar esta acción con http get.

Después de eliminar el registro de base de datos, debe mostrar la lista actualizada de contactos que no contiene el registro eliminado. El método AjaxDelete() devuelve el error ContactList y la lista actualizada de contactos.

## <a name="summary"></a>Resumen

En esta iteración, agregamos la funcionalidad de Ajax a nuestra aplicación Contact Manager. Utilizamos Ajax para mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación.

En primer lugar, hemos refactorizado la vista de índice para que al hacer clic en un grupo de contactos no se actualice toda la vista. En su lugar, al hacer clic en un grupo de contactos solo se actualiza la lista de contactos.

A continuación, usamos efectos de animación jQuery para desvanecerse y desvanecerse en la lista de contactos. Agregar animación a una aplicación Ajax se puede utilizar para proporcionar a los usuarios de la aplicación el equivalente de una barra de progreso del explorador.

También hemos añadido soporte de historial del navegador a nuestra aplicación Ajax. Hemos permitido a los usuarios hacer clic en los botones Atrás y Adelante del navegador para cambiar el estado de la vista de índice.

Por último, hemos creado un vínculo de eliminación que admite operaciones HTTP DELETE. Al realizar eliminaciones de Ajax, permitimos a los usuarios eliminar registros de base de datos sin necesidad de que el usuario solicite una página de confirmación de eliminación adicional.

> [!div class="step-by-step"]
> [Anterior](iteration-6-use-test-driven-development-cs.md)
> [Siguiente](iteration-1-create-the-application-vb.md)
