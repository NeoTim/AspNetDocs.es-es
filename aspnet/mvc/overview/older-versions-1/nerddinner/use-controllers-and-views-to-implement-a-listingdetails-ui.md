---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores y vistas para implementar una interfaz de usuario de listado/detalles . Microsoft Docs
author: rick-anderson
description: Paso 4 muestra cómo agregar un Controlador a la aplicación que aprovecha nuestro modelo para proporcionar a los usuarios una lista de datos / experiencia de navegación de detalles ...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 49c7dc977477a4edbfcfc68b166ae7ea3fa22f2a
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541317"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores y vistas para implementar una interfaz de usuario de lista/detalles

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 4 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 4 muestra cómo agregar un controlador a la aplicación que aprovecha nuestro modelo para proporcionar a los usuarios una lista de datos / detalles de la experiencia de navegación para las cenas en nuestro sitio NerdDinner.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner Paso 4: Controladores y vistas

Con los marcos web tradicionales (ASP clásico, PHP, ASP.NET web Forms, etc.), las direcciones URL entrantes normalmente se asignan a archivos en el disco. Por ejemplo: una solicitud de una URL como "/Products.aspx" o "/Products.php" podría ser procesada por un archivo "Products.aspx" o "Products.php".

Los marcos MVC basados en web asignan direcciones URL al código del servidor de una manera ligeramente diferente. En lugar de asignar direcciones URL entrantes a archivos, en su lugar asignan direcciones URL a métodos en clases. Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, controlar la entrada del usuario, recuperar y guardar datos y determinar la respuesta para enviar de vuelta al cliente (mostrar HTML, descargar un archivo, redirigir a una dirección URL diferente, etc.).

Ahora que hemos creado un modelo básico para nuestra aplicación NerdDinner, nuestro siguiente paso será agregar un controlador a la aplicación que se aprovecha para proporcionar a los usuarios una lista de datos / detalles experiencia de navegación para cenas en nuestro sitio.

### <a name="adding-a-dinnerscontroller-controller"></a>Adición de un controlador DinnersController

Comenzaremos haciendo clic con el botón derecho en la carpeta "Controladores" dentro de nuestro proyecto web, y luego seleccionaremos el comando de menú **Agregar-&gt;Controlador** (también puede ejecutar este comando escribiendo Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Esto abrirá el cuadro de diálogo "Agregar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Nombraremos el nuevo controlador "DinnersController" y haremos clic en el botón "Agregar". A continuación, Visual Studio agregará un archivo de DinnersController.cs en nuestro directorio de controladores:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

También se abrirá la nueva Clase DinnersController en el editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Adición de los métodos de acción Index() y Details() a la clase DinnersController

Queremos permitir a los visitantes que utilizan nuestra aplicación para navegar por una lista de las próximas cenas, y permitirles hacer clic en cualquier cena en la lista para ver detalles específicos al respecto. Para ello, publicaremos las siguientes direcciones URL desde nuestra aplicación:

| **URL** | **Propósito** |
| --- | --- |
| */Cenas/* | Mostrar una lista HTML de las próximas cenas |
| */Dinners/Details/[id]* | Mostrar detalles sobre una cena específica indicada por un parámetro "id" incrustado en la dirección URL, que coincidirá con el DinnerID de la cena en la base de datos. Por ejemplo: /Dinners/Details/2 mostraría una página HTML con detalles sobre la cena cuyo valor DinnerID es 2. |

Publicaremos las implementaciones iniciales de estas direcciones URL agregando dos "métodos de acción" públicos a nuestra clase DinnersController como a continuación:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

A continuación, ejecutaremos la aplicación NerdDinner y usaremos nuestro navegador para invocarlos. Si escribe la URL *"/Dinners/"* se ejecutará nuestro método *Index(),* y se enviará la siguiente respuesta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Si escribes la URL *"/Dinners/Details/2"* se ejecutará nuestro método *Details()* y se enviará la siguiente respuesta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Es posible que se pregunte: ¿cómo sabía ASP.NET MVC para crear nuestra clase DinnersController e invocar esos métodos? Para entender que echemos un vistazo rápido a cómo funciona el enrutamiento.

### <a name="understanding-aspnet-mvc-routing"></a>Comprender ASP.NET MVC Routing

ASP.NET MVC incluye un potente motor de enrutamiento de direcciones URL que proporciona mucha flexibilidad para controlar cómo se asignan las direcciones URL a las clases de controlador. Nos permite personalizar completamente cómo ASP.NET MVC elige qué clase de controlador crear, qué método invocar en él, así como configurar diferentes formas en que las variables se pueden analizar automáticamente desde la dirección URL/Querystring y pasar al método como argumentos de parámetro. Ofrece la flexibilidad de optimizar totalmente un sitio para SEO (optimización de motores de búsqueda), así como para publicar cualquier estructura de URL que queramos desde una aplicación.

De forma predeterminada, los nuevos ASP.NET mvc vienen con un conjunto preconfigurado de reglas de enrutamiento de direcciones URL ya registradas. Esto nos permite comenzar fácilmente en una aplicación sin tener que configurar explícitamente nada. Los registros predeterminados de reglas de enrutamiento se pueden encontrar dentro de la clase "Aplicación" de nuestros proyectos, que podemos abrir haciendo doble clic en el archivo "Global.asax" en la raíz de nuestro proyecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Las reglas de enrutamiento ASP.NET MVC predeterminadas se registran dentro del método "RegisterRoutes" de esta clase:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Las "rutas. La llamada al método MapRoute()" anterior registra una regla de enrutamiento predeterminada que asigna las direcciones URL entrantes a las clases de controlador mediante el formato de dirección URL: "/-controlador-/-acción-/-id-" – donde "controlador" es el nombre de la clase de controlador para crear instancias, "action" es el nombre de un método público para invocarenlo, y "id" es un parámetro opcional incrustado dentro de la dirección URL que se puede pasar como argumento al método. El tercer parámetro que se pasa a la llamada al método "MapRoute()" es un conjunto de valores predeterminados que se van a utilizar para los valores controller/action/id en caso de que no estén presentes en la dirección URL (Controlador de la dirección URL (Controlador de la dirección URL "Home", Action -"Index", Id-"").

A continuación se muestra una tabla que muestra cómo se asignan una variedad de direcciones URL mediante la regla de ruta predeterminada "<em>/ .</em>

| **URL** | **Clase de controlador** | **Método de acción** | **Parámetros aprobados** |
| --- | --- | --- | --- |
| */Cenas/Detalles/2* | DinnersController | Detalles(id) | id.2 |
| */Cenas/Editar/5* | DinnersController | Editar(id) | id-5 |
| */Dinners/Create* | DinnersController | Crear() | N/D |
| */Cenas* | DinnersController | Index() | N/D |
| */home* | HomeController | Index() | N/D |
| */* | HomeController | Index() | N/D |

Las tres últimas filas muestran los valores predeterminados (Controlador , Inicio, Acción, Índice, Id , ""). Dado que el método "Index" se registra como el nombre de acción predeterminado si no se especifica uno, las direcciones URL "/Dinners" y "/Home" hacen que el método de acción Index() se invoque en sus clases Controller. Dado que el controlador "Home" se registra como el controlador predeterminado si no se especifica uno, la dirección URL "/" hace que se cree HomeController y se invoque el método de acción Index() en él.

Si no le gustan estas reglas de enrutamiento de DIRECCIONes PREDETERMINADAs, la buena noticia es que son fáciles de cambiar, simplemente edítelas dentro del método RegisterRoutes anterior. Para nuestra aplicación NerdDinner, sin embargo, no vamos a cambiar ninguna de las reglas de enrutamiento de URL predeterminadas, en su lugar, solo las usaremos tal como está.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Uso de DinnerRepository de nuestro DinnersController

Ahora reemplacemos nuestra implementación actual de los métodos de acción DinnersController's Index() y Details() por implementaciones que utilizan nuestro modelo.

Usaremos la clase DinnerRepository que creamos anteriormente para implementar el comportamiento. Comenzaremos agregando una instrucción "using" que hace referencia al espacio de nombres "NerdDinner.Models" y, a continuación, declararemos una instancia de nuestro DinnerRepository como un campo en nuestra clase DinnerController.

Más adelante en este capítulo presentaremos el concepto de "Inserción de Dependencia" y mostraremos otra manera para que nuestros Controladores obtengan una referencia a un DinnerRepository que permita mejores pruebas unitarias, pero por ahora solo crearemos una instancia de nuestro DinnerRepository en línea como a continuación.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Ahora estamos listos para generar una respuesta HTML usando nuestros objetos de modelo de datos recuperados.

### <a name="using-views-with-our-controller"></a>Uso de vistas con nuestro controlador

Si bien es posible escribir código dentro de nuestros métodos de acción para ensamblar HTML y luego usar el método auxiliar *Response.Write()* para enviarlo de vuelta al cliente, ese enfoque se vuelve bastante difícil de controlar rápidamente. Un enfoque mucho mejor es que solo realicemos la lógica de aplicaciones y datos dentro de nuestros métodos de acción DinnersController y, a continuación, pasemos los datos necesarios para representar una respuesta HTML a una plantilla de "vista" independiente que sea responsable de generar la representación HTML de la misma. Como veremos en un momento, una plantilla de "vista" es un archivo de texto que normalmente contiene una combinación de marcado HTML y código de representación incrustado.

Separar nuestra lógica de controlador de nuestra representación de vista trae varios grandes beneficios. En particular, ayuda a aplicar una clara "separación de preocupaciones" entre el código de la aplicación y el formato de la interfaz de usuario/código de representación. Esto hace que sea mucho más fácil probar la lógica de la aplicación de prueba unitaria de forma aislada de la lógica de representación de la interfaz de usuario. Facilita la modificación posterior de las plantillas de representación de la interfaz de usuario sin tener que realizar cambios en el código de la aplicación. Y puede facilitar que los desarrolladores y diseñadores colaboren juntos en proyectos.

Podemos actualizar nuestra clase DinnersController para indicar que queremos usar una plantilla de vista para devolver una respuesta de interfaz de usuario HTML cambiando las firmas de método de nuestros dos métodos de acción de tener un tipo de valor devuelto de "void" para tener en su lugar un tipo de valor devuelto de "ActionResult". A continuación, podemos llamar al método auxiliar *View()* en la clase base Controller para devolver un objeto "ViewResult" como se muestra a continuación:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma del método auxiliar *View()* que estamos usando anteriormente se ve como la siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

El primer parámetro para el método auxiliar *View()* es el nombre del archivo de plantilla de vista que queremos usar para representar la respuesta HTML. El segundo parámetro es un objeto de modelo que contiene los datos que la plantilla de vista necesita para representar la respuesta HTML.

Dentro de nuestro método de acción Index() llamamos al método auxiliar *View()* e indicamos que queremos representar una lista HTML de cenas utilizando una plantilla de vista "Index". Estamos pasando la plantilla de vista una secuencia de Dinner objetos para generar la lista de:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Dentro de nuestro método de acción Details() intentamos recuperar un objeto Dinner utilizando el identificador proporcionado dentro de la URL. Si se encuentra una cena válida, llamamos al método auxiliar *View(),* lo que indica que queremos usar una plantilla de vista "Detalles" para representar el objeto Dinner recuperado. Si se solicita una cena no válida, representamos un mensaje de error útil que indica que la cena no existe mediante una plantilla de vista "NotFound" (y una versión sobrecargada del método auxiliar *View()* que solo toma el nombre de la plantilla):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Ahora implementemos las plantillas de vista "NotFound", "Details" e "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementación de la plantilla de vista "NotFound"

Comenzaremos implementando la plantilla de vista "NotFound", que muestra un mensaje de error descriptivo que indica que no se puede encontrar la cena solicitada.

Crearemos una nueva plantilla de vista colocando nuestro cursor de texto dentro de un método de acción del controlador, y luego haga clic con el botón derecho y elija el comando de menú "Agregar vista" (también podemos ejecutar este comando escribiendo Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Esto abrirá un cuadro de diálogo "Agregar vista" como a continuación. De forma predeterminada, el cuadro de diálogo rellenará previamente el nombre de la vista que se va a crear para que coincida con el nombre del método de acción en el que se encontraba el cursor cuando se inició el cuadro de diálogo (en este caso "Detalles"). Dado que primero queremos implementar la plantilla "NotFound", reemplazaremos este nombre de vista y lo estableceremos en "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Cuando hagamos clic en el botón "Agregar", Visual Studio creará una nueva plantilla de vista "NotFound.aspx" para nosotros dentro del directorio "Vistas-Cenas" (que también creará si el directorio aún no existe):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

También abrirá nuestra nueva plantilla de vista "NotFound.aspx" dentro del editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Las plantillas de vista tienen por defecto dos "regiones de contenido" donde podemos agregar contenido y código. El primero nos permite personalizar el "título" de la página HTML enviada de vuelta. El segundo nos permite personalizar el "contenido principal" de la página HTML enviada de vuelta.

Para implementar nuestra plantilla de vista "NotFound" agregaremos contenido básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

A continuación, podemos probarlo dentro del navegador. Para ello vamos a solicitar la URL *"/Dinners/Details/9999".* Esto hará referencia a una cena que actualmente no existe en la base de datos y hará que nuestro método de acción DinnersController.Details() represente nuestra plantilla de vista "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Una cosa que notarás en la captura de pantalla anterior es que nuestra plantilla de vista básica ha heredado un montón de HTML que rodea el contenido principal de la pantalla. Esto se debe a que nuestra plantilla de vista está utilizando una plantilla de "página maestra" que nos permite aplicar un diseño coherente en todas las vistas del sitio. Discutiremos cómo funcionan más las páginas maestras en una parte posterior de este tutorial.

### <a name="implementing-the-details-view-template"></a>Implementación de la plantilla de vista "Detalles"

Ahora vamos a implementar la plantilla de vista "Detalles", que generará HTML para un único modelo de cena.

Haremos esto colocando nuestro cursor de texto dentro del método de acción Detalles, y luego haga clic con el botón derecho y elija el comando de menú "Agregar vista" (o presione Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Aparecerá el cuadro de diálogo "Agregar vista". Mantendremos el nombre de vista predeterminado ("Detalles"). También seleccionaremos la casilla de verificación "Crear una vista fuertemente tipada" en el cuadro de diálogo y seleccionaremos (mediante el menú desplegable del cuadro combinado) el nombre del tipo de modelo que estamos pasando del controlador a la vista. Para esta vista estamos pasando un objeto Dinner (el nombre completo de este tipo es: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A diferencia de la plantilla anterior, donde elegimos crear una "Vista vacía", esta vez elegiremos automáticamente "scaffold" la vista usando una plantilla "Detalles". Podemos indicar esto cambiando el menú desplegable "Ver contenido" en el cuadro de diálogo anterior.

"Scaffolding" generará una implementación inicial de nuestra plantilla de vista de detalles basada en el objeto Dinner que le estamos pasando. Esto proporciona una manera fácil para nosotros para comenzar rápidamente en nuestra implementación de plantilla de vista.

Cuando hacemos clic en el botón "Agregar", Visual Studio creará un nuevo archivo de plantilla de vista "Details.aspx" para nosotros dentro de nuestro directorio "Vistas-Cenas":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

También se abrirá nuestra nueva plantilla de vista "Details.aspx" dentro del editor de código. Contendrá una implementación inicial de scaffolding de una vista de detalles basada en un modelo Dinner. El motor de scaffolding utiliza la reflexión de .NET para examinar las propiedades públicas expuestas en la clase que la ha pasado y agregará el contenido adecuado en función de cada tipo que encuentre:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Podemos solicitar la URL *"/Dinners/Details/1"* para ver cómo se ve esta implementación de andamio "detalles" en el navegador. Usando esta URL mostrará una de las cenas que agregamos manualmente a nuestra base de datos cuando la creamos por primera vez:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Esto nos pone en marcha rápidamente y nos proporciona una implementación inicial de nuestra vista Details.aspx. A continuación, podemos ir y ajustarlo para personalizar la interfaz de usuario a nuestra satisfacción.

Cuando examinamos la plantilla Details.aspx más de cerca, encontraremos que contiene HTML estático, así como código de representación incrustado. &lt;%&gt; % de código nuggets ejecutar código &lt;cuando se&gt; representa la plantilla de vista y % % % de código nuggets ejecutar el código contenido en ellos y, a continuación, representar el resultado en la secuencia de salida de la plantilla.

Podemos escribir código dentro de nuestra vista que tiene acceso al objeto de modelo "Dinner" que se pasó desde nuestro controlador mediante una propiedad "Model" fuertemente tipada. Visual Studio nos proporciona el código-intellisense completo al acceder a esta propiedad "Modelo" dentro del editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos a hacer algunos ajustes para que la fuente de nuestra plantilla de vista de detalles final se vea como a continuación:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Cuando accedamos a la URL *"/Dinners/Details/1"* de nuevo, ahora se representará como a continuación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementación de la plantilla de vista "index"

Ahora vamos a implementar la plantilla de vista "Index", que generará una lista de las próximas cenas. Para ello, colocaremos nuestro cursor de texto dentro del método de acción Index y, a continuación, haremos clic con el botón derecho y elegiremos el comando de menú "Agregar vista" (o presionaremos Ctrl-M, Ctrl-V).

Dentro del cuadro de diálogo "Agregar vista" mantendremos la plantilla de vista denominada "Index" y seleccionaremos la casilla de verificación "Crear una vista fuertemente tipada". Esta vez elegiremos generar automáticamente una plantilla de vista "Lista", y seleccionaremos "NerdDinner.Models.Dinner" como el tipo de modelo pasado a la vista (que porque hemos indicado que estamos creando un andamio "Lista" hará que el cuadro de diálogo Agregar vista suponga que estamos pasando una secuencia de objetos Dinner de nuestro controlador a la vista):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio creará un nuevo archivo de plantilla de vista "Index.aspx" para nosotros dentro de nuestro directorio "Vistas". "Scaffold" una implementación inicial dentro de ella que proporciona una lista de tabla HTML de las cenas que pasamos a la vista.

Cuando ejecutamos la aplicación y accedemos a la URL *"/Dinners/"* representará nuestra lista de cenas así:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solución de tabla anterior nos da un diseño similar a la cuadrícula de nuestros datos de la cena, que no es exactamente lo que queremos para nuestro consumidor frente a la lista de cenas. Podemos actualizar la plantilla de vista Index.aspx y modificarla para &lt;enumerar&gt; menos columnas de datos y usar un elemento ul para representarlos en lugar de una tabla con el código siguiente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Estamos usando la palabra clave "var" dentro de la instrucción foreach anterior a medida que recorremos cada cena en nuestro modelo. Aquellos que no están familiarizados con C- 3.0 podrían pensar que usar "var" significa que el objeto de cena está enlazado en tiempo de próyunto en tiempo de 7. En su lugar, significa que el compilador está utilizando la inferencia de tipos con&lt;&gt;la propiedad "Model" fuertemente tipada (que es de tipo "Cena Enumerable") y compilar la variable local "cena" como un tipo Dinner, lo que significa que obtenemos intellisense completo y comprobamos en tiempo de compilación dentro de bloques de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Cuando pulsamos actualizar en la URL */Dinners* en nuestro navegador nuestra vista actualizada ahora se ve como la siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Esto se ve mejor, pero aún no está del todo. Nuestro último paso es permitir a los usuarios finales hacer clic en Cenas individuales en la lista y ver detalles sobre ellas. Implementaremos esto mediante la representación de elementos de hipervínculo HTML que se vinculan a la Details método de acción en nuestro DinnersController.

Podemos generar estos hipervínculos dentro de nuestra vista de índice de una de dos maneras. La primera es crear &lt;manualmente&gt; HTML un elemento &lt;como&gt; el siguiente, donde incrustamos % % bloques dentro del &lt;elemento&gt; HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Un enfoque alternativo que podemos usar es aprovechar el método auxiliar integrado "Html.ActionLink()" dentro &lt;de&gt; ASP.NET MVC que admite la creación mediante programación de un HTML un elemento que se vincula a otro método de acción en un controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

El primer parámetro para el método auxiliar Html.ActionLink() es el texto de vínculo que se va a mostrar (en este caso el título de la cena), el segundo parámetro es el nombre de acción Controller al que queremos generar el vínculo (en este caso el método Details) y el tercer parámetro es un conjunto de parámetros para enviar a la acción (implementado como un tipo anónimo con nombre de propiedad/valores). En este caso, estamos especificando el parámetro "id" de la cena a la que queremos vincular, y debido a que la regla de enrutamiento de URL predeterminada en ASP.NET MVC es "-Controlador-/'Acción'/'id'" el método auxiliar Html.ActionLink() generará el siguiente resultado:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Para nuestra vista Index.aspx usaremos el enfoque del método auxiliar Html.ActionLink() y tendremos cada cena en el vínculo de lista a la dirección URL de detalles adecuada:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Y ahora cuando llegamos a la URL */Dinners* nuestra lista de cenas se ve como la siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Cuando hagamos clic en cualquiera de las Cenas de la lista navegaremos para ver detalles al respecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Nombres basados en convenciones y la estructura de directorios de las vistas

ASP.NET las aplicaciones MVC de forma predeterminada usan una estructura de nomenclatura de directorios basada en convenciones al resolver plantillas de vista. Esto permite a los desarrolladores evitar tener que calificar completamente una ruta de acceso de ubicación al hacer referencia a vistas desde dentro de un Controller clase. De forma predeterminada, ASP.NET MVC buscará el\[archivo de\* plantilla de vista dentro del directorio *-Views ControllerName] debajo de la aplicación.

Por ejemplo, hemos estado trabajando en la clase DinnersController, que hace referencia explícita a tres plantillas de vista: "Index", "Details" y "NotFound". ASP.NET MVC buscará de forma predeterminada estas vistas en el directorio *de* la aplicación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Observe anteriormente cómo hay actualmente tres clases de controlador dentro del proyecto (DinnersController, HomeController y AccountController – las dos últimas se agregaron de forma predeterminada cuando creamos el proyecto) y hay tres subdirectorios (uno para cada controlador) dentro del directorio de vistas.

Las vistas a las que se hace referencia desde los controladores Inicio y Cuentas resolverán automáticamente sus plantillas de vista desde los directorios correspondientes *de* las aplicaciones de inicio y *de cuenta.* El subdirectorio *"Vistas" y "Compartido"* proporciona una manera de almacenar las plantillas de vista que se vuelven a usar en varios controladores dentro de la aplicación. Cuando ASP.NET MVC intenta resolver una plantilla de vista, primero se comprobará dentro del directorio específico del *\[controlador* de vistas, y si no puede encontrar la plantilla de vista allí buscará en el directorio de las *vistas de* shared.

Cuando se trata de nombrar plantillas de vista individuales, la guía recomendada es hacer que la plantilla de vista comparta el mismo nombre que el método de acción que hizo que se representara. Por ejemplo, encima de nuestro método de acción "Index" está utilizando la vista "Index" para representar el resultado de la vista, y el método de acción "Details" usa la vista "Details" para representar sus resultados. Esto hace que sea fácil ver rápidamente qué plantilla está asociada a cada acción.

Los desarrolladores no necesitan especificar explícitamente el nombre de la plantilla de vista cuando la plantilla de vista tiene el mismo nombre que el método de acción que se invoca en el controlador. En su lugar, podemos pasar el objeto de modelo al método auxiliar "View()" (sin especificar el nombre de la vista) y ASP.NET MVC deducirá automáticamente que queremos usar la plantilla de vista *ActionName] de "Views\[\[ControllerName]* en el disco para representarlo.

Esto nos permite limpiar un poco nuestro código de controlador, y evitar duplicar el nombre dos veces en nuestro código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

El código anterior es todo lo que se necesita para implementar una agradable cena lista / detalles experiencia para el sitio.

#### <a name="next-step"></a>siguiente paso

Ahora tenemos una agradable experiencia de navegación de la cena construida.

Ahora habilitemos la compatibilidad con la edición de formularios de datos CRUD (Create, Read, Update, Delete).

> [!div class="step-by-step"]
> [Anterior](build-a-model-with-business-rule-validations.md)
> [Siguiente](provide-crud-create-read-update-delete-data-form-entry-support.md)
