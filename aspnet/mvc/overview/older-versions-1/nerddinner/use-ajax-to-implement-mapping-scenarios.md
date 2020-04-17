---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usar AJAX para implementar escenarios de asignación ? Microsoft Docs
author: rick-anderson
description: El paso 11 muestra cómo integrar el soporte de mapeo AJAX en nuestra aplicación NerdDinner, permitiendo a los usuarios que están creando, editando o viendo cenas para ver el l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f2e2640eb421d5ee8006915f46cbe1090b8d21ad
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542591"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Usar AJAX para implementar escenarios de asignación

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 11 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 11 muestra cómo integrar la compatibilidad con mapas AJAX en nuestra aplicación NerdDinner, lo que permite a los usuarios que están creando, editando o viendo cenas ver gráficamente la ubicación de la cena.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner Paso 11: Integración de un mapa AJAX

Ahora haremos que nuestra aplicación sea un poco más emocionante visualmente mediante la integración de la compatibilidad con mapas AJAX. Esto permitirá a los usuarios que están creando, editando o viendo cenas ver gráficamente la ubicación de la cena.

### <a name="creating-a-map-partial-view"></a>Creación de una vista parcial de mapa

Vamos a utilizar la funcionalidad de mapeo en varios lugares dentro de nuestra aplicación. Para mantener nuestro código DRY encapsularemos la funcionalidad de mapa común dentro de una sola plantilla parcial que podemos reutilizar en varias acciones y vistas del controlador. Vamos a nombrar esta vista parcial "map.ascx" y crearla dentro del directorio de las vistas de la cena.

Podemos crear el map.ascx parcial haciendo clic con el botón derecho en&gt;el directorio de las vistas y seleccionando el comando de menú Agregar- Vista. Nombraremos la vista "Map.ascx", la comprobaremos como una vista parcial e indicaremos que vamos a pasarle una clase de modelo "Dinner" fuertemente tipada:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Al hacer clic en el botón "Añadir" se creará nuestra plantilla parcial. A continuación, actualizaremos el archivo Map.ascx para que tenga el siguiente contenido:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La &lt;primera&gt; referencia de script apunta a la biblioteca de asignación de Microsoft Virtual Earth 6.2. La &lt;segunda&gt; referencia de script apunta a un archivo map.js que crearemos en breve que encapsulará nuestra lógica de mapeo Javascript común. El &lt;elemento div id"theMap"&gt; es el contenedor HTML que Virtual Earth usará para hospedar el mapa.

A continuación, tenemos&gt; un bloque de script incrustado &lt;que contiene dos funciones de JavaScript específicas de esta vista. La primera función utiliza jQuery para conectar una función que se ejecuta cuando la página está lista para ejecutar el script del lado cliente. Llama a una función auxiliar LoadMap() que definiremos dentro de nuestro archivo de script Map.js para cargar el control de mapa de tierra virtual. La segunda función es un controlador de eventos de devolución de llamada que agrega un pin al mapa que identifica una ubicación.

Observe cómo estamos usando &lt;un&gt; bloque % % del lado del servidor dentro del bloque de script del lado cliente para incrustar la latitud y longitud de la cena que queremos asignar en el JavaScript. Esta es una técnica útil para generar valores dinámicos que pueden ser utilizados por el script del lado cliente (sin requerir una llamada AJAX independiente al servidor para recuperar los valores, lo que lo hace más rápido). Los &lt;bloques&gt; % % se ejecutarán cuando la vista se esté representando en el servidor, por lo que la salida del HTML acabará con valores de JavaScript incrustados (por ejemplo: var latitude á 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creación de una biblioteca de utilidades Map.js

Ahora vamos a crear el archivo Map.js que podemos usar para encapsular la funcionalidad de JavaScript para nuestro mapa (e implementar los métodos LoadMap y LoadPin anteriores). Podemos hacer esto haciendo clic con el botón derecho en el directorio&gt;de scripts dentro de nuestro proyecto, y luego elegir el comando de menú "Agregar- Nuevo elemento", seleccionar el elemento JScript y asignarle el nombre "Map.js".

A continuación se muestra el código JavaScript que agregaremos al archivo Map.js que interactuará con Virtual Earth para mostrar nuestro mapa y agregar le agregan ubicaciones para nuestras cenas:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integración del mapa con crear y editar formularios

Ahora integraremos el soporte de mapas con nuestros escenarios de creación y edición existentes. La buena noticia es que esto es bastante fácil de hacer, y no requiere que cambiemos cualquiera de nuestro código de controlador. Dado que nuestras vistas Crear y editar comparten una vista parcial común de "DinnerForm" para implementar la interfaz de usuario del formulario de cena, podemos agregar el mapa en un solo lugar y hacer que nuestros escenarios de creación y edición lo usen.

Todo lo que necesitamos hacer es abrir la vista parcial de la vista parcial de la vista parcial de la lista de vistas, las cenas y la configuración de la cena, y actualizarla para incluir nuestro nuevo mapa parcial. A continuación se muestra cómo se verá el DinnerForm actualizado una vez que se agregue el mapa (nota: los elementos del formulario HTML se omiten del fragmento de código a continuación para mayor brevedad):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

El DinnerForm parcial anterior toma un objeto de tipo "DinnerFormViewModel" como su tipo de modelo (porque necesita un Dinner objeto, así como un SelectList para rellenar la lista desplegable de países). Nuestro map partial solo necesita un objeto de tipo "Dinner" como su tipo de modelo, por lo que cuando representamos el mapa parcial estamos pasando solo la subpropiedad Dinner de DinnerFormViewModel:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La función JavaScript que hemos agregado al parcial utiliza jQuery para adjuntar un evento "blur" al cuadro de texto HTML "Address". Probablemente hayas oído hablar de eventos de "enfoque" que se desencadenan cuando un usuario hace clic o pestaña en un cuadro de texto. Lo contrario es un evento "blur" que se activa cuando un usuario sale de un cuadro de texto. El controlador de eventos anterior borra los valores de cuadro de texto de latitud y longitud cuando esto sucede y, a continuación, traza la nueva ubicación de dirección en nuestro mapa. Un controlador de eventos de devolución de llamada que definimos en el archivo map.js actualizará los cuadros de texto de longitud y latitud de nuestro formulario utilizando los valores devueltos por la tierra virtual en función de la dirección que le dimos.

Y ahora cuando ejecutemos nuestra aplicación de nuevo y hagamos clic en la pestaña "Cena de host" veremos un mapa predeterminado que se muestra junto con nuestros elementos de formulario de cena estándar:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Cuando escribimos una dirección y, a continuación, tabulamos, el mapa se actualizará dinámicamente para mostrar la ubicación y nuestro controlador de eventos rellenará los cuadros de texto de latitud/longitud con los valores de ubicación:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si guardamos la nueva cena y luego la abrimos de nuevo para editarla, encontraremos que la ubicación del mapa se muestra cuando se carga la página:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Cada vez que se cambia el campo de dirección, el mapa y las coordenadas de latitud/longitud se actualizarán.

Ahora que el mapa muestra la ubicación de la cena, también podemos cambiar los campos de formulario Latitud y Longitud de ser cuadros de texto visibles para que en su lugar sean elementos ocultos (ya que el mapa los actualiza automáticamente cada vez que se introduce una dirección). Para ello, pasaremos de usar la aplicación auxiliar HTML Html.TextBox() a usar el método auxiliar Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Y ahora nuestros formularios son un poco más fáciles de usar y evitan mostrar la latitud/longitud sin procesar (sin dejar de almacenarlos con cada cena en la base de datos):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integración del mapa con la vista de detalles

Ahora que tenemos el mapa integrado con nuestros escenarios de crear y editar, vamos a integrarlo también con nuestro escenario De detalles. Todo lo que necesitamos &lt;hacer es llamar a % Html.RenderPartial("map"); %&gt; en la vista Detalles.

A continuación se muestra el aspecto del código fuente de la vista Detalles completo (con integración de mapas):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Y ahora, cuando un usuario navega a una URL /Dinners/Details/[id] verá detalles sobre la cena, la ubicación de la cena en el mapa (completa con un push-pin que cuando se pasa el ratón muestra el título de la cena y la dirección de la misma), y tienen un enlace AJAX a RSVP para ello:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementación de la búsqueda de ubicación en nuestra base de datos y repositorio

Para finalizar nuestra implementación de AJAX, agreguemos un mapa a la página principal de la aplicación que permite a los usuarios buscar gráficamente cenas cerca de ellos.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Comenzaremos implementando soporte dentro de nuestra capa de base de datos y repositorio de datos para realizar de manera eficiente una búsqueda de radio basada en ubicación para cenas. Podríamos usar las nuevas [características geoespaciales de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) para implementar esto, o alternativamente podemos [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) usar un enfoque de función SQL que Gary Dryden discutió en el artículo aquí: y Rob Conery blogueado sobre el uso con LINQ to SQL aquí:[http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar esta técnica, abriremos el "Explorador de servidores" en Visual Studio, seleccionaremos la base de datos NerdDinner y, a continuación, haremos clic con el botón derecho en el subnodo "funciones" debajo de él y elegiremos crear una nueva "función con valores escalares":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

A continuación, pegaremos la siguiente función DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

A continuación, crearemos una nueva función con valores de tabla en SQL Server SQL Server que llamaremos "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Esta función de tabla "NearestDinners" utiliza la función auxiliar DistanceBetween para devolver todas las cenas dentro de 100 millas de la latitud y longitud que le suministramos:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para llamar a esta función, primero abriremos el diseñador LINQ to SQLLINQ to SQL haciendo doble clic en el archivo NerdDinner.dbml dentro de nuestro directorio de modelos:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

A continuación, arrastraremos las funciones NearestDinners y DistanceBetween al diseñador LINQ to SQLLINQ to SQL, lo que hará que se agreguen como métodos en nuestra clase LINQ to SQL NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

A continuación, podemos exponer un método de consulta "FindByLocation" en nuestra clase DinnerRepository que usa la función NearestDinner para devolver las próximas cenas que se encuentran dentro de 100 millas de la ubicación especificada:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementación de un método de acción de búsqueda AJAX basado en JSON

Ahora implementaremos un método de acción de controlador que aprovecha el nuevo método de repositorio FindByLocation() para devolver una lista de datos de cena que se pueden usar para rellenar un mapa. Haremos que este método de acción devuelva los datos de dinner en un formato JSON (JavaScript Object Notation) para que se pueda namanipular fácilmente con JavaScript en el cliente.

Para implementar esto, crearemos una nueva clase "SearchController" haciendo clic con el botón derecho en el directorio de controladores y eligiendo el comando de menú Agregar controlador.&gt; A continuación, implementaremos un método de acción "SearchByLocation" dentro de la nueva clase SearchController como se muestra a continuación:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

El SearchController SearchByLocation método de acción llama internamente el FindByLocation método en DinnerRepository para obtener una lista de cenas cercanas. Sin embargo, en lugar de devolver los objetos Dinner directamente al cliente, devuelve objetos JsonDinner. La clase JsonDinner expone un subconjunto de propiedades Dinner (por ejemplo: por razones de seguridad no revela los nombres de las personas que tienen RSVP'd para una cena). También incluye una propiedad RSVPCount que no existe en Dinner y que se calcula dinámicamente contando el número de objetos RSVP asociados a una cena determinada.

A continuación, estamos utilizando el método auxiliar Json() en la clase base Controller para devolver la secuencia de cenas mediante un formato de cable basado en JSON. JSON es un formato de texto estándar para representar estructuras de datos simples. A continuación se muestra un ejemplo de cómo se ve una lista con formato JSON de dos objetos JsonDinner cuando se devuelve desde nuestro método de acción:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Llamar al método AJAX basado en JSON mediante jQuery

Ahora estamos listos para actualizar la página principal de la aplicación NerdDinner para usar el SearchByLocation método de acción SearchByLocation de SearchController. Para ello, abriremos la plantilla de vista /Views/Home/Index.aspx y la actualizaremos para que tenga &lt;&gt; un cuadro de texto, un botón de búsqueda, nuestro mapa y un elemento div denominado dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

A continuación, podemos agregar dos funciones de JavaScript a la página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La primera función JavaScript carga el mapa cuando se carga la página por primera vez. La segunda función JavaScript conecta un controlador de eventos de clic de JavaScript en el botón de búsqueda. Cuando se pulsa el botón llama a la función JavaScript FindDinnersGivenLocation() que añadiremos a nuestro archivo Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Esta función FindDinnersGivenLocation() llama a map. Find() en el Virtual Earth Control para centrarlo en la ubicación ingresada. Cuando se devuelve el servicio de mapa de tierra virtual, el mapa. El método Find() invoca el método de devolución de llamada callbackUpdateMapDinners que lo pasamos como argumento final.

El método callbackUpdateMapDinners() es donde se realiza el trabajo real. Utiliza el método auxiliar $.post() de jQuery para realizar una llamada AJAX al método de acción SearchByLocation() de nuestro SearchController, pasándole la latitud y longitud del mapa recién centrado. Define una función en línea a la que se llamará cuando se complete el método auxiliar $.post() y los resultados de la cena con formato JSON devueltos desde el método de acción SearchByLocation() se pasarán mediante una variable denominada "dinners". A continuación, hace un foreach sobre cada cena devuelta, y utiliza la latitud y longitud de la cena y otras propiedades para agregar un nuevo pin en el mapa. También añade una entrada para la cena a la lista HTML de cenas a la derecha del mapa. A continuación, conecta un evento hover tanto para los pulsadores como para la lista HTML para que los detalles sobre la cena se muestren cuando un usuario pasa el cursor sobre ellos:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Y ahora cuando ejecutemos la aplicación y visitemos la página de inicio se nos presentará un mapa. Cuando introduzcamos el nombre de una ciudad el mapa mostrará las próximas cenas cerca de ella:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Al pasar el ratón sobre una cena se mostrarán detalles al respecto.

Al hacer clic en el título de la cena, ya sea en la burbuja o en el lado derecho de la lista HTML, nos navegará a la cena, que luego podemos opcionalmente RSVP para:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>siguiente paso

Ahora hemos implementado toda la funcionalidad de la aplicación de nuestra aplicación NerdDinner. Veamos ahora cómo podemos habilitar las pruebas unitarias automatizadas de la misma.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Siguiente](enable-automated-unit-testing.md)
