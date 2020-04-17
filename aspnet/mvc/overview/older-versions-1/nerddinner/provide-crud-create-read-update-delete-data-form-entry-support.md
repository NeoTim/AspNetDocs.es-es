---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Proporcionar CRUD (Crear, Leer, Actualizar, Eliminar) Soporte de Entrada de Formularios de Datos Microsoft Docs
author: rick-anderson
description: El paso 5 muestra cómo llevar nuestra clase DinnersController más lejos al habilitar la compatibilidad para editar, crear y eliminar cenas con él también.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 2b75a7eda8bce4baa25d92626639f4d904eb363a
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542630"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Proporcionar la compatibilidad con entradas de formularios de datos CRUD (crear, leer, actualizar y eliminar)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 5 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 5 muestra cómo llevar nuestra clase DinnersController más lejos al habilitar la compatibilidad para editar, crear y eliminar cenas con él también.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner Paso 5: Crear, Actualizar, Eliminar Escenarios de Formulario

Hemos introducido controladores y vistas, y hemos cubierto cómo usarlos para implementar una experiencia de listado/detalles para cenas en el sitio. Nuestro siguiente paso será llevar nuestra clase DinnersController más lejos y habilitar el soporte para editar, crear y eliminar cenas con él también.

### <a name="urls-handled-by-dinnerscontroller"></a>URLs manejadas por DinnersController

Anteriormente agregamos métodos de acción a DinnersController que implementaban la compatibilidad con dos direcciones URL: */Dinners* y */Dinners/Details/[id]*.

| **URL** | **Verbo** | **Propósito** |
| --- | --- | --- |
| */Cenas/* | GET | Muestra una lista HTML de las próximas cenas. |
| */Dinners/Details/[id]* | GET | Muestre detalles sobre una cena específica. |

Ahora agregaremos métodos de acción para implementar tres direcciones URL adicionales: */Dinners/Edit/[id]*, */Dinners/Create*y */Dinners/Delete/[id]*. Estas direcciones URL permitirán la compatibilidad con la edición de cenas existentes, la creación de nuevas cenas y la eliminación de cenas.

Admitiremos interacciones de verboHTTP GET y HTTP POST con estas nuevas direcciones URL. Las solicitudes HTTP GET a estas url mostrarán la vista HTML inicial de los datos (un formulario rellenado con los datos de la cena en el caso de "editar", un formulario en blanco en el caso de "crear", y una pantalla de confirmación de eliminación en el caso de "eliminar"). Las solicitudes HTTP POST a estas direcciones URL guardarán/actualizarán/eliminarán los datos de la cena en nuestro DinnerRepository (y de allí a la base de datos).

| **URL** | **Verbo** | **Propósito** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | Mostrar un formulario HTML editable rellenado con datos de la cena. |
| POST | Guarde los cambios de formulario para una cena determinada en la base de datos. |
| */Dinners/Create* | GET | Mostrar un formulario HTML vacío que permite a los usuarios definir nuevas cenas. |
| POST | Cree una nueva cena y guárdela en la base de datos. |
| */Dinners/Delete/[id]* | GET | Pantalla de confirmación de eliminación. |
| POST | Elimina la cena especificada de la base de datos. |

### <a name="edit-support"></a>Editar soporte

Comencemos implementando el escenario de "editar".

#### <a name="the-http-get-edit-action-method"></a>El método de acción de edición HTTP-GET

Comenzaremos implementando el comportamiento HTTP "GET" de nuestro método de acción de edición. Este método se invocará cuando se solicite la dirección URL */Dinners/Edit/[id].* Nuestra implementación tendrá el siguiente aspecto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

El código anterior utiliza el DinnerRepository para recuperar un Dinner objeto. A continuación, representa una plantilla de vista mediante el Dinner objeto. Dado que no hemos pasado explícitamente un nombre de plantilla al método auxiliar *View(),* usará la ruta de acceso predeterminada basada en convenciones para resolver la plantilla de vista: /Views/Dinners/Edit.aspx.

Ahora vamos a crear esta plantilla de vista. Haremos esto haciendo clic con el botón derecho en el método Edit y seleccionando el comando de menú contextual "Agregar vista":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Dentro del cuadro de diálogo "Agregar vista" indicaremos que estamos pasando un objeto Dinner a nuestra plantilla de vista como su modelo, y elegiremos auto-scaffold una plantilla "Editar":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio agregará un nuevo archivo de plantilla de vista "Edit.aspx" para nosotros dentro del directorio "Vistas-Cenas". También abrirá la nueva plantilla de vista "Edit.aspx" dentro del editor de código, rellenada con una implementación inicial de scaffolding "Editar" como se muestra a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos a hacer algunos cambios en el andamio "editar" predeterminado generado, y actualizar la plantilla de vista de edición para tener el contenido siguiente (que elimina algunas de las propiedades que no queremos exponer):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Cuando ejecutemos la aplicación y solicitemos la URL *"/Dinners/Edit/1"* veremos la siguiente página:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

El marcado HTML generado por nuestra vista se ve como el siguiente. Es HTML estándar: &lt;con&gt; un elemento de formulario que realiza un HTTP POST a la &lt;dirección URL */Dinners/Edit/1* cuando se inserta el botón "Guardar" tipo de entrada "enviar"/&gt; . Se &lt;ha producido un elemento&gt; HTML input type-"text"/ para cada propiedad editable:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() y Html.TextBox() Html Helper Métodos

Nuestra plantilla de vista "Edit.aspx" utiliza varios métodos "Html Helper": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() y Html.ValidationMessage(). Además de generar marcado HTML para nosotros, estos métodos auxiliares proporcionan compatibilidad integrada con el control de errores y la validación.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() método auxiliar

El método auxiliar Html.BeginForm() es &lt;&gt; lo que genera el elemento de formulario HTML en nuestro marcado. En nuestra edit.aspx plantilla de vista se dará cuenta de que estamos aplicando una instrucción "using" de C- al usar este método. La llave abierta indica el principio &lt;&gt; del contenido del formulario, y la llave de &lt;cierre&gt; es lo que indica el final del elemento /form:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Como alternativa, si encuentra que el enfoque de instrucción "using" no es natural para un escenario como este, puede utilizar una combinación Html.BeginForm() y Html.EndForm() (que hace lo mismo):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Llamar a Html.BeginForm() sin ningún parámetro hará que genere un elemento de formulario que hace un HTTP-POST en la dirección URL de la solicitud actual. Es por eso que nuestra vista Editar genera un * &lt;elemento de acción de&gt; formulario "/Dinners/Edit/1" method ."post".* Alternativamente, podríamos haber pasado parámetros explícitos a Html.BeginForm() si quisiéramos publicar en una URL diferente.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() método auxiliar

Nuestra vista Edit.aspx utiliza el método auxiliar Html.TextBox() para generar &lt;elementos de tipo de entrada "text"/:&gt;

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

El método Html.TextBox() anterior toma un único parámetro, que se utiliza &lt;para especificar los atributos id/name del elemento de tipo de entrada "text"/&gt; a la salida, así como la propiedad del modelo desde el que rellenar el valor del cuadro de texto. Por ejemplo, el objeto Dinner que pasamos a la vista Edit tenía un valor de propiedad "Title" de "Futuros .NET", por lo que nuestro html.TextBox("Title") de llamada al método: * &lt;id de entrada "Título" nombre "Título" tipo "texto" valor ".NET Futuros" /&gt;*. .

Como alternativa, podemos utilizar el primer parámetro Html.TextBox() para especificar el id/name del elemento y, a continuación, pasar explícitamente el valor que se usará como segundo parámetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

A menudo queremos realizar un formato personalizado en el valor que se genera. El método estático String.Format() integrado en .NET es útil para estos escenarios. Nuestra plantilla de vista Edit.aspx está usando esto para dar formato al valor EventDate (que es de tipo DateTime) para que no muestre segundos para la hora:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un tercer parámetro para Html.TextBox() se puede utilizar opcionalmente para generar atributos HTML adicionales. En el fragmento de código que se muestra a continuación se muestra cómo representar un atributo &lt;de tamaño adicional&gt; "30" y un atributo class-"mycssclass" en el elemento de tipo de entrada "text"/. Tenga en cuenta cómo escapamos del nombre@" character because "del atributo de clase mediante una " class" es una palabra clave reservada en C-:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementación del método de acción de edición HTTP-POST

Ahora tenemos implementada la versión HTTP-GET de nuestro método de acción Edit. Cuando un usuario solicita la dirección URL */Dinners/Edit/1,* recibe una página HTML como la siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Al pulsar el botón "Guardar" se produce una publicación de formulario &lt;en&gt; la dirección URL */Dinners/Edit/1* y se envían los valores de formulario de entrada HTML mediante el verbo HTTP POST. Ahora vamos a implementar el comportamiento HTTP POST de nuestro método de acción de edición, que controlará el guardado de la cena.

Comenzaremos agregando un método de acción "Editar" sobrecargado a nuestro DinnersController que tiene un atributo "AcceptVerbs" en él que indica que controla escenarios HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Cuando el atributo [AcceptVerbs] se aplica a los métodos de acción sobrecargados, ASP.NET MVC controla automáticamente la distribución de solicitudes al método de acción adecuado en función del verbo HTTP entrante. Las solicitudes HTTP POST a las direcciones URL */Dinners/Edit/[id]* irán al método Edit anterior, mientras que todas las demás solicitudes de verbo HTTP a `[AcceptVerbs]` direcciones URL */Dinners/Edit/[id]* irán al primer método Edit que implementamos (que no tenía un atributo).

| **Tema secundario: ¿Por qué diferenciarse a través de verbos HTTP?** |
| --- |
| Usted podría preguntar – ¿por qué estamos usando una sola URL y diferenciando su comportamiento a través del verbo HTTP? ¿Por qué no tener dos direcciones URL independientes para controlar la carga y guardar los cambios de edición? Por ejemplo: /Dinners/Edit/[id] para mostrar el formulario inicial y /Dinners/Save/[id] para controlar el mensaje de formulario para guardarlo? La desventaja con la publicación de dos direcciones URL independientes es que en los casos en los que publicamos en /Dinners/Save/2, y luego necesitamos volver a mostrar el formulario HTML debido a un error de entrada, el usuario final terminará teniendo la DIRECCIÓN URL /Dinners/Save/2 en la barra de direcciones de su navegador (ya que esa era la url en la que se publicó el formulario). Si el usuario final marca esta página remostrada en su lista de favoritos del navegador, o copia/pega la URL y la envía por correo electrónico a un amigo, terminará guardando una URL que no funcionará en el futuro (ya que esa URL depende de los valores de publicación). Al exponer una sola URL (como: /Dinners/Edit/[id]) y diferenciar el procesamiento de la misma por verbo HTTP, es seguro para los usuarios finales marcar la página de edición y/o enviar la URL a otros. |

#### <a name="retrieving-form-post-values"></a>Recuperación de valores de publicación de formulario

Hay una variedad de maneras en que podemos acceder a los parámetros de formulario publicados dentro de nuestro método HTTP POST "Editar". Un enfoque sencillo es usar la propiedad Request en la clase base Controller para tener acceso a la colección de formularios y recuperar los valores publicados directamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

El enfoque anterior es un poco detallado, sin embargo, especialmente una vez que agregamos lógica de control de errores.

Un mejor enfoque para este escenario es aprovechar el método auxiliar *UpdateModel()* integrado en la clase base Controller. Admite la actualización de las propiedades de un objeto que se pasa mediante los parámetros de formulario entrantes. Utiliza la reflexión para determinar los nombres de propiedad en el objeto y, a continuación, convierte y asigna automáticamente valores a ellos en función de los valores de entrada enviados por el cliente.

Podríamos usar el método UpdateModel() para simplificar nuestra acción de edición HTTP-POST usando este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Ahora podemos visitar la URL */Dinners/Edit/1,* y cambiar el título de nuestra cena:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Al hacer clic en el botón "Guardar", realizaremos una publicación de formulario en nuestra acción Editar y los valores actualizados se conservarán en la base de datos. A continuación, se remos redirigidos a la URL de detalles para la cena (que mostrará los valores recién guardados):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Manejo de errores de edición

Nuestra implementación HTTP-POST actual funciona bien, excepto cuando hay errores.

Cuando un usuario comete un error al editar un formulario, necesitamos asegurarnos de que el formulario se vuelve a mostrar con un mensaje de error informativo que los guía para solucionarlo. Esto incluye los casos en los que un usuario final publica una entrada incorrecta (por ejemplo: una cadena de fecha con formato incorrecto), así como los casos en los que el formato de entrada es válido pero hay una infracción de la regla de negocio. Cuando se producen errores, el formulario debe conservar los datos de entrada que el usuario introdujo originalmente para que no tengan que rellenar sus cambios manualmente. Este proceso debe repetirse tantas veces como sea necesario hasta que el formulario se complete correctamente.

ASP.NET MVC incluye algunas características integradas agradables que facilitan el control de errores y la revisualización de formularios. Para ver estas características en acción, vamos a actualizar nuestro método de acción Editar con el siguiente código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

El código anterior es similar a nuestra implementación anterior, excepto que ahora estamos envolviendo un bloque de control de errores try/catch alrededor de nuestro trabajo. Si se produce una excepción al llamar a UpdateModel(), o cuando intentamos guardar DinnerRepository (que generará una excepción si el objeto Dinner que intentamos guardar no es válido debido a una infracción de la regla dentro de nuestro modelo), se ejecutará nuestro bloque de control de errores de captura. Dentro de él, recorremos en bucle las infracciones de reglas que existen en el objeto Dinner y las agregamos a un objeto ModelState (que trataremos en breve). A continuación, volvemos a mostrar la vista.

Para ver este trabajo, volvamos a ejecutar la aplicación, editemos una cena y cámbiela para que tenga un título vacío, un EventDate de "BOGUS" y utilice un número de teléfono del Reino Unido con un valor de país de EE. UU. Cuando pulsamos el botón "Guardar" nuestro método HTTP POST Edit no podrá guardar la cena (porque hay errores) y volverá a mostrar el formulario:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nuestra aplicación tiene una experiencia de error decente. Los elementos de texto con la entrada no válida se resaltan en rojo y los mensajes de error de validación se muestran al usuario final sobre ellos. El formulario también conserva los datos de entrada que el usuario introdujo originalmente, para que no tengan que rellenar nada.

¿Cómo, se podría preguntar, ocurrió esto? ¿Cómo se resaltaron los cuadros de texto Title, EventDate y ContactPhone en rojo y sabían generar los valores de usuario introducidos originalmente? ¿Y cómo se mostraron los mensajes de error en la lista de la parte superior? La buena noticia es que esto no ocurrió por arte de magia - más bien fue porque usamos algunas de las características integradas ASP.NET MVC que facilitan la validación de entradas y escenarios de control de errores.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Comprender ModelState y los métodos auxiliares HTML de validación

Las clases de controlador tienen una colección de propiedades "ModelState" que proporciona una manera de indicar que existen errores con un objeto de modelo que se pasa a una vista. Las entradas de error dentro de la colección ModelState identifican el nombre de la propiedad model con el problema (por ejemplo: "Title", "EventDate" o "ContactPhone") y permiten especificar un mensaje de error descriptivo (por ejemplo: "Title is required").

El método auxiliar *UpdateModel()* rellena automáticamente la colección ModelState cuando encuentra errores al intentar asignar valores de formulario a las propiedades del objeto de modelo. Por ejemplo, la propiedad EventDate del objeto Dinner es de tipo DateTime. Cuando el método UpdateModel() no pudo asignarle el valor de cadena "BOGUS" en el escenario anterior, el método UpdateModel() agregó una entrada a la colección ModelState que indicaba que se había producido un error de asignación con esa propiedad.

Los desarrolladores también pueden escribir código para agregar explícitamente entradas de error en la colección ModelState como estamos haciendo a continuación dentro de nuestro bloque de control de errores "catch", que está rellenando la colección ModelState con entradas basadas en las infracciones de regla activas en el objeto Dinner:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integración de Html Helper con ModelState

Los métodos auxiliares HTML, como Html.TextBox() - comprueban la colección ModelState al representar la salida. Si existe un error para el elemento, representan el valor introducido por el usuario y una clase de error CSS.

Por ejemplo, en nuestra vista "Editar" estamos utilizando el método auxiliar Html.TextBox() para representar el EventDate de nuestro objeto Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Cuando la vista se representó en el escenario de error, el método Html.TextBox() comprobó la colección ModelState para ver si había errores asociados con la propiedad "EventDate" de nuestro objeto Dinner. Cuando determinó que había un error, representaba la entrada del usuario enviada ("BOGUS") &lt;como el valor, y&gt; agregó una clase de error css al tipo de entrada "textbox"/ marcado que generó:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Puede personalizar la apariencia de la clase de error css para que se vea como desee. La clase de error CSS predeterminada – "input-validation-error" – se define en la hoja de estilos *.css* de contenido y se ve como a continuación:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Esta regla CSS es lo que causó que nuestros elementos de entrada no válidos se resaltaran como abajo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Método auxiliar Html.ValidationMessage()

El método auxiliar Html.ValidationMessage() se puede utilizar para generar el mensaje de error ModelState asociado a una propiedad de modelo determinada:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Las salidas de código anteriores: * &lt;span&gt; class-"field-validation-error"&lt;El valor 'BOGUS' no es válido /span&gt;*

El método auxiliar Html.ValidationMessage() también admite un segundo parámetro que permite a los desarrolladores invalidar el mensaje de texto de error que se muestra:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Las salidas de código anteriores: * &lt;span class-"field-validation-error"&gt;\*&lt;/span&gt; * en lugar del texto de error predeterminado cuando hay un error para la propiedad EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Método auxiliar Html.ValidationSummary()

El método auxiliar Html.ValidationSummary() se puede utilizar para representar &lt;un&gt;&lt;mensaje&gt;&lt;de&gt; error de resumen, acompañado de una lista ul li/ /ul de todos los mensajes de error detallados de la colección ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

El método auxiliar Html.ValidationSummary() toma un parámetro de cadena opcional, que define un mensaje de error de resumen para mostrar encima de la lista de errores detallados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Opcionalmente, puede usar CSS para invalidar el aspecto de la lista de errores.

#### <a name="using-a-addruleviolations-helper-method"></a>Uso de un método auxiliar AddRuleViolations

Nuestra implementación inicial de http-POST Edit utilizó una instrucción foreach dentro de su bloque catch para recorrer en bucle las infracciones de regla del objeto Dinner y agregarlas a la colección ModelState del controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos hacer que este código sea un poco más limpio agregando una clase "ControllerHelpers" al proyecto NerdDinner e implementar un método de extensión "AddRuleViolations" dentro de él que agrega un método auxiliar a la clase ModelStateDictionary de ASP.NET MVC. Este método de extensión puede encapsular la lógica necesaria para rellenar ModelStateDictionary con una lista de errores RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

A continuación, podemos actualizar nuestro método de acción de edición HTTP-POST para usar este método de extensión para rellenar la colección ModelState con nuestras infracciones de regla de cena.

#### <a name="complete-edit-action-method-implementations"></a>Implementaciones completas del método de acción de edición

El código siguiente implementa toda la lógica del controlador necesaria para nuestro escenario de edición:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Lo bueno de nuestra implementación de edición es que ni nuestra clase Controller ni nuestra plantilla View tienen que saber nada acerca de la validación específica o las reglas de negocio que se aplican por nuestro modelo de cena. Podemos agregar reglas adicionales a nuestro modelo en el futuro y no tenemos que realizar ningún cambio de código en nuestro controlador o vista para que sean compatibles. Esto nos proporciona la flexibilidad para evolucionar fácilmente nuestros requisitos de aplicación en el futuro con un mínimo de cambios de código.

### <a name="create-support"></a>Crear soporte

Hemos terminado de implementar el comportamiento "Editar" de nuestra clase DinnersController. Ahora vamos a implementar el soporte "Crear" en él, lo que permitirá a los usuarios agregar nuevas cenas.

#### <a name="the-http-get-create-action-method"></a>El método de acción HTTP-GET Create

Comenzaremos implementando el comportamiento HTTP "GET" de nuestro método de acción create. Se llamará a este método cuando alguien visite la dirección URL */Dinners/Create.* Nuestra implementación tiene el siguiente aspecto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

El código anterior crea un nuevo objeto Dinner y asigna su propiedad EventDate para que sea de una semana en el futuro. A continuación, representa una vista que se basa en el nuevo Dinner objeto. Dado que no hemos pasado explícitamente un nombre al método auxiliar *View(),* usará la ruta de acceso predeterminada basada en convenciones para resolver la plantilla de vista: /Views/Dinners/Create.aspx.

Ahora vamos a crear esta plantilla de vista. Podemos hacer esto haciendo clic con el botón derecho en el método de acción Crear y seleccionando el comando de menú contextual "Agregar vista". Dentro del cuadro de diálogo "Agregar vista" indicaremos que estamos pasando un objeto Dinner a la plantilla de vista y elegiremos auto-scaffold una plantilla "Crear":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio guardará una nueva vista basada en scaffold "Create.aspx" en el directorio "-Views-Dinners" y la abrirá dentro del IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos a hacer algunos cambios en el archivo de andamio "crear" predeterminado que se generó para nosotros, y modificarlo para que se vea como a continuación:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Y ahora, cuando ejecutemos nuestra aplicación y accedamos a la URL *"/Dinners/Create"* dentro del navegador, se hará la interfaz de usuario como la siguiente desde nuestra implementación de crear acción:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementación del método de acción CREATE HTTP-POST

Tenemos implementada la versión HTTP-GET de nuestro método de acción Create. Cuando un usuario hace clic en el botón "Guardar", realiza una publicación de &lt;&gt; formulario en la dirección URL */Dinners/Create* y envía los valores del formulario de entrada HTML mediante el verbo HTTP POST.

Ahora vamos a implementar el comportamiento HTTP POST de nuestro método de acción create. Comenzaremos agregando un método de acción "Create" sobrecargado a nuestro DinnersController que tiene un atributo "AcceptVerbs" que indica que controla escenarios HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Hay una variedad de maneras en que podemos acceder a los parámetros de formulario publicados dentro de nuestro método "Crear" habilitado para HTTP-POST.

Un enfoque consiste en crear un nuevo objeto Dinner y, a continuación, usar el método auxiliar *UpdateModel()* (como hicimos con la acción Editar) para rellenarlo con los valores de formulario publicados. A continuación, podemos agregarlo a nuestro DinnerRepository, conservarlo en la base de datos y redirigir al usuario a nuestra acción Detalles para mostrar la cena recién creada utilizando el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternativamente, podemos usar un enfoque donde tenemos nuestro método de acción Create() tomar un objeto Dinner como parámetro de método. ASP.NET MVC, a continuación, creará automáticamente una instancia de un nuevo Dinner objeto para nosotros, rellenar sus propiedades mediante las entradas de formulario y pasarlo a nuestro método de acción:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nuestro método de acción anterior comprueba que el objeto Dinner se ha rellenado correctamente con los valores de entrada de formulario comprobando la propiedad ModelState.IsValid. Esto devolverá false si hay problemas de conversión de entrada (por ejemplo: una cadena de "BOGUS" para la propiedad EventDate) y si hay algún problema, nuestro método de acción vuelve a mostrar el formulario.

Si los valores de entrada son válidos, el método de acción intenta agregar y guardar la nueva Dinner en DinnerRepository. Ajusta este trabajo dentro de un bloque try/catch y vuelve a mostrar el formulario si hay alguna infracción de la regla de negocio (lo que haría que el método dinnerRepository.Save() generara una excepción).

Para ver este comportamiento de control de errores en acción, podemos solicitar la dirección URL */Dinners/Create* y rellenar los detalles sobre una nueva cena. La entrada o los valores incorrectos harán que el formulario de creación se vuelva a mostrar con los errores resaltados como se muestra a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Observe cómo nuestro formulario Crear respeta exactamente las mismas reglas de validación y negocio que nuestro formulario Editar. Esto se debe a que nuestras reglas de validación y de negocio se definieron en el modelo y no se incrustaron en la interfaz de usuario o el controlador de la aplicación. Esto significa que más adelante podemos cambiar/evolucionar nuestras reglas de validación o de negocio en un solo lugar y hacer que se apliquen a lo largo de nuestra aplicación. No tendremos que cambiar ningún código dentro de nuestros métodos de acción Editar o Crear para respetar automáticamente las nuevas reglas o modificaciones a las existentes.

Cuando corrijamos los valores de entrada y hagamos clic en el botón "Guardar" de nuevo, nuestra adición a DinnerRepository se realizará correctamente y se agregará una nueva cena a la base de datos. A continuación, se remos redirigidos a la URL */Dinners/Details/[id]* – donde se nos presentarán detalles sobre la cena recién creada:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Eliminar soporte

Ahora agreguemos soporte "Eliminar" a nuestro DinnersController.

#### <a name="the-http-get-delete-action-method"></a>El método de acción HTTP-GET Delete

Comenzaremos implementando el comportamiento HTTP GET de nuestro método de acción delete. Se llamará a este método cuando alguien visite la dirección URL */Dinners/Delete/[id].* A continuación se muestra la implementación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

El método de acción intenta recuperar la cena que se va a eliminar. Si la cena existe, representa una vista basada en el objeto Dinner. Si el objeto no existe (o ya se ha eliminado), devuelve una vista que representa la plantilla de vista "NotFound" que creamos anteriormente para nuestro método de acción "Detalles".

Podemos crear la plantilla de vista "Eliminar" haciendo clic con el botón derecho en el método de acción Eliminar y seleccionando el comando de menú contextual "Agregar vista". Dentro del cuadro de diálogo "Agregar vista" indicaremos que estamos pasando un objeto Dinner a nuestra plantilla de vista como su modelo, y elegiremos crear una plantilla vacía:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio agregará un nuevo archivo de plantilla de vista "Delete.aspx" para nosotros dentro de nuestro directorio "Vistas". Agregaremos algo de CÓDIGO HTML y código a la plantilla para implementar una pantalla de confirmación de eliminación como la siguiente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

El código anterior muestra el título de la cena &lt;&gt; que se va a eliminar y genera un elemento de formulario que hace un POST en la dirección URL /Dinners/Delete/[id] si el usuario final hace clic en el botón "Eliminar" dentro de él.

Cuando ejecutamos nuestra aplicación y accedemos a la URL *"/Dinners/Delete/[id]"* para un objeto Dinner válido, representa la interfaz de usuario como la siguiente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tema secundario: ¿Por qué estamos haciendo un POST?** |
| --- |
| Usted podría preguntar – ¿por qué hemos &lt;&gt; pasado por el esfuerzo de crear un formulario dentro de nuestra pantalla de confirmación Eliminar? ¿Por qué no utilizar simplemente un hipervínculo estándar para vincular a un método de acción que realiza la operación de eliminación real? La razón es porque queremos tener cuidado de protegernos contra los rastreadores web y motores de búsqueda que descubren nuestras URL y sin darse cuenta haciendo que los datos se eliminen cuando siguen los enlaces. Las direcciones URL basadas en HTTP-GET se consideran "seguras" para que accedan/rastreen, y se supone que no deben seguir las HTTP-POST. Una buena regla es asegurarse de que siempre coloca operaciones destructivas o de modificación de datos detrás de las solicitudes HTTP-POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementación del método de acción de eliminación HTTP-POST

Ahora tenemos implementada la versión HTTP-GET de nuestro método de acción Delete, que muestra una pantalla de confirmación de eliminación. Cuando un usuario final hace clic en el botón "Eliminar", realizará una publicación de formulario en la dirección URL */Dinners/Dinner/[id].*

Ahora vamos a implementar el comportamiento HTTP "POST" del método de acción delete usando el código siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versión HTTP-POST de nuestro método de acción Delete intenta recuperar el objeto de cena que se va a eliminar. Si no puede encontrarlo (porque ya se ha eliminado) representa nuestra plantilla "NotFound". Si encuentra la cena, la elimina de DinnerRepository. A continuación, representa una plantilla "Eliminado".

Para implementar la plantilla "Eliminado" haremos clic con el botón derecho en el método de acción y elegiremos el menú contextual "Agregar vista". Nombraremos nuestra vista "Eliminado" y haremos que sea una plantilla vacía (y no tomaremos un objeto de modelo fuertemente tipado). A continuación, le agregaremos contenido HTML:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Y ahora cuando ejecutamos nuestra aplicación y accedemos a la URL *"/Dinners/Delete/[id]"* para un objeto Dinner válido, representará nuestra pantalla de confirmación de eliminación de la cena como a continuación:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Al hacer clic en el botón "Eliminar" se realizará un HTTP-POST a la URL */Dinners/Delete/[id],* que eliminará la cena de nuestra base de datos, y mostrará nuestra plantilla de vista "Eliminado":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Seguridad de enlace de modelos

Hemos discutido dos maneras diferentes de usar las características de enlace de modelos integradas de ASP.NET MVC. El primero utiliza el método UpdateModel() para actualizar las propiedades de un objeto de modelo existente y el segundo mediante ASP.NET compatibilidad de MVC para pasar objetos de modelo como parámetros de método de acción. Ambas técnicas son muy potentes y extremadamente útiles.

Este poder también trae consigo la responsabilidad. Es importante ser siempre paranoico acerca de la seguridad al aceptar cualquier entrada de usuario, y esto también es cierto cuando se enlazan objetos para formar la entrada. Debe tener cuidado de codificar siempre HTML cualquier valor introducido por el usuario para evitar ataques de inyección de HTML y JavaScript, y tener cuidado con los ataques de inyección SQL (tenga en cuenta: estamos usando LINQ to SQL para nuestra aplicación, que codifica automáticamente los parámetros para evitar estos tipos de ataques). Nunca debe confiar solo en la validación del lado cliente y emplear siempre la validación del lado del servidor para protegerse contra los piratas informáticos que intentan enviarle valores falsos.

Un elemento de seguridad adicional para asegurarse de que piensa en el uso de las características de enlace de ASP.NET MVC es el ámbito de los objetos que está enlazando. En concreto, desea asegurarse de comprender las implicaciones de seguridad de las propiedades que permite enlazar y asegurarse de que solo permite que se actualicen las propiedades que realmente debe actualizar un usuario final.

De forma predeterminada, el método UpdateModel() intentará actualizar todas las propiedades del objeto de modelo que coincidan con los valores de parámetro de formulario entrantes. Del mismo modo, los objetos pasados como parámetros de método de acción también de forma predeterminada pueden tener todas sus propiedades establecidas a través de parámetros de formulario.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloqueo de la unión por uso

Puede bloquear la directiva de enlace por uso proporcionando una "lista de inclusión" explícita de propiedades que se pueden actualizar. Esto se puede hacer pasando un parámetro de matriz de cadena adicional al método UpdateModel() como a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Los objetos pasados como parámetros de método de acción también admiten un atributo [Bind] que permite especificar una "lista de inclusión" de propiedades permitidas como se muestra a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloqueo de la unión sobre una base de tipo

También puede bloquear las reglas de enlace por tipo. Esto le permite especificar las reglas de enlace una vez y, a continuación, hacer que se apliquen en todos los escenarios (incluidos UpdateModel y escenarios de parámetros de método de acción) en todos los controladores y métodos de acción.

Puede personalizar las reglas de enlace por tipo agregando un atributo [Bind] a un tipo o registrándolo en el archivo Global.asax de la aplicación (útil para escenarios en los que no es propietario del tipo). A continuación, puede usar las propiedades Include y Exclude del atributo Bind para controlar qué propiedades son enlazables para la clase o interfaz determinada.

Usaremos esta técnica para la clase Dinner en nuestra aplicación NerdDinner y le agregaremos un atributo [Bind] que restringe la lista de propiedades enlazables a lo siguiente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Tenga en cuenta que no estamos permitiendo que la colección RSVPs se manipule a través de enlace, ni estamos permitiendo que las propiedades DinnerID o HostedBy se establezcan a través de binding. Por razones de seguridad, solo manipularemos estas propiedades concretas utilizando código explícito dentro de nuestros métodos de acción.

### <a name="crud-wrap-up"></a>Envoltura CRUD

ASP.NET MVC incluye una serie de características integradas que ayudan a implementar escenarios de registro de formularios. Usamos una variedad de estas características para proporcionar compatibilidad con la interfaz de usuario CRUD en la parte superior de nuestro DinnerRepository.

Estamos utilizando un enfoque centrado en el modelo para implementar nuestra aplicación. Esto significa que toda nuestra lógica de validación y regla de negocio se define dentro de nuestra capa de modelo, y no dentro de nuestros controladores o vistas. Ni nuestra clase Controller ni nuestras plantillas View saben nada acerca de las reglas de negocio específicas que aplica nuestra clase de modelo Dinner.

Esto mantendrá nuestra arquitectura de aplicaciones limpia y facilitará la prueba. Podemos agregar reglas de negocio adicionales a nuestra capa de modelo en el futuro y *no tener que realizar ningún cambio* de código en nuestro controller o View para que sean compatibles. Esto nos va a proporcionar una gran agilidad para evolucionar y cambiar nuestra aplicación en el futuro.

Nuestro DinnersController ahora habilita los listados/detalles de la cena, así como crear, editar y eliminar soporte. El código completo de la clase se puede encontrar a continuación:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>siguiente paso

Ahora tenemos soporte básico de CRUD (Crear, Leer, Actualizar y Eliminar) implementado dentro de nuestra clase DinnersController.

Veamos ahora cómo podemos usar ViewData y ViewModel clases para habilitar la interfaz de usuario aún más rica en nuestros formularios.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Siguiente](use-viewdata-and-implement-viewmodel-classes.md)
