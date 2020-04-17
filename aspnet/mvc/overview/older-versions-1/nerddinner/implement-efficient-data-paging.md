---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar la paginación eficiente de datos ( Efficient Data Paging ) Microsoft Docs
author: rick-anderson
description: El paso 8 muestra cómo agregar soporte de paginación a nuestra URL /Dinners para que en lugar de mostrar 1000s de cenas a la vez, solo mostraremos 10 próximas cenas en...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: a833553fe44b62b136f7eb55c7e00eca0b0462c6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541330"
---
# <a name="implement-efficient-data-paging"></a>Implementar la paginación de datos eficaz

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 8 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 8 muestra cómo agregar soporte de paginación a nuestra URL /Dinners para que en lugar de mostrar 1000 s de cenas a la vez, solo mostraremos 10 próximas cenas a la vez - y permitiremos a los usuarios finales paginar hacia atrás y reenviar a través de toda la lista de una manera amigable SEO.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-8-paging-support"></a>NerdDinner Paso 8: Soporte de paginación

Si nuestro sitio tiene éxito, tendrá miles de próximas cenas. Tenemos que asegurarnos de que nuestra interfaz de usuario se escala para manejar todas estas cenas y permite a los usuarios examinarlas. Para habilitar esto, agregaremos soporte de paginación a nuestra URL */Dinners* para que en lugar de mostrar 1000 s de cenas a la vez, solo mostraremos 10 próximas cenas a la vez - y permitiremos a los usuarios finales paginar hacia atrás y reenviar a través de toda la lista de una manera amigable seO.

### <a name="index-action-method-recap"></a>Index() Action Method Recap

El método de acción Index() dentro de nuestra clase DinnersController actualmente tiene el siguiente aspecto:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Cuando se realiza una solicitud a la dirección URL */Dinners,* recupera una lista de todas las próximas cenas y, a continuación, representa una lista de todas ellas:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Comprender IQueryable&lt;T&gt;

*IQueryable&lt;&gt; T* es una interfaz que se introdujo con LINQ como parte de .NET 3.5. Permite escenarios de "ejecución diferida" eficaces que podemos aprovechar para implementar la compatibilidad con la paginación.

En nuestro DinnerRepository estamos devolviendo&lt;&gt; una secuencia de cena IQueryable de nuestro método FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

El objeto&lt;IQueryable Dinner&gt; devuelto por nuestro método FindUpcomingDinners() encapsula una consulta para recuperar objetos Dinner de nuestra base de datos mediante LINQ to SQLLINQ to SQL. Es importante destacar que no ejecutará la consulta en la base de datos hasta que intentemos acceder/iterar los datos de la consulta, o hasta que llamemos al método ToList() en ella. El código que llama a nuestro método FindUpcomingDinners() puede optar opcionalmente por&lt;agregar&gt; operaciones/filtros "encadenados" adicionales al objeto IQueryable Dinner antes de ejecutar la consulta. LINQ to SQLLINQ to SQL es lo suficientemente inteligente como para ejecutar la consulta combinada en la base de datos cuando se solicitan los datos.

Para implementar la lógica de paginación podemos actualizar nuestro método de acción Index() de DinnersController para que&lt;&gt; aplique operadores adicionales "Skip" y "Take" a la secuencia IQueryable Dinner devuelta antes de llamar a ToList() en ella:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

El código anterior omite las primeras 10 próximas cenas en la base de datos y, a continuación, devuelve 20 cenas. LINQ to SQLLINQ to SQL es lo suficientemente inteligente como para construir una consulta SQL optimizada que realiza esta lógica de omisión en la base de datos SQL y no en el servidor web. Esto significa que incluso si tenemos millones de próximas cenas en la base de datos, solo los 10 que queremos serán recuperados como parte de esta solicitud (haciéndolo eficiente y escalable).

### <a name="adding-a-page-value-to-the-url"></a>Adición de un valor de "página" a la URL

En lugar de codificar de forma rígida un intervalo de páginas específico, queremos que nuestras direcciones URL incluyan un parámetro de "página" que indique qué intervalo de cenas está solicitando un usuario.

#### <a name="using-a-querystring-value"></a>Uso de un valor Querystring

El código siguiente muestra cómo podemos actualizar nuestro método de acción Index() para admitir un parámetro de cadena de consulta y habilitar direcciones URL como */Dinners?page-2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

El método de acción Index() anterior tiene un parámetro denominado "page". El parámetro se declara como un entero que acepta valores NULL (que es lo que int? indica). Esto significa que la dirección URL */Dinners?page-2* hará que se pase un valor de "2" como valor de parámetro. La dirección URL */Dinners* (sin un valor de cadena de consulta) hará que se pase un valor nulo.

Multiplicamos el valor de página por el tamaño de página (en este caso 10 filas) para determinar cuántas cenas se deben omitir. Estamos usando el [operador de "coalescing" nulo de C- (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) que es útil cuando se trata de tipos que aceptan valores NULL. El código anterior asigna a la página el valor de 0 si el parámetro page es null.

#### <a name="using-embedded-url-values"></a>Uso de valores de URL incrustados

Una alternativa al uso de un valor de cadena de consulta sería incrustar el parámetro de página dentro de la propia dirección URL real. Por ejemplo: */Dinners/Page/2* o */Dinners/2*. ASP.NET MVC incluye un potente motor de enrutamiento de DIRECCIONes URL que facilita la compatibilidad con escenarios como este.

Podemos registrar reglas de enrutamiento personalizadas que asignan cualquier formato de URL o URL entrante a cualquier clase de controlador o método de acción que deseemos. Todo lo que necesitamos hacer es abrir el archivo Global.asax dentro de nuestro proyecto:

![](implement-efficient-data-paging/_static/image2.png)

Y luego registre una nueva regla de asignación usando el método auxiliar MapRoute() como la primera llamada a las rutas. MapRoute() a continuación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Arriba estamos registrando una nueva regla de enrutamiento denominada "UpcomingDinners". Estamos indicando que tiene el formato de url "Dinners/Page/'page'" – donde "page" es un valor de parámetro incrustado dentro de la URL. El tercer parámetro del método MapRoute() indica que debemos asignar direcciones URL que coincidan con este formato con el método de acción Index() en la clase DinnersController.

Podemos usar exactamente el mismo código Index() que teníamos antes con nuestro escenario Querystring, excepto que ahora nuestro parámetro "page" provendrá de la URL y no de la cadena de consulta:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Y ahora cuando ejecutemos la aplicación y escribamos */Dinners* veremos las primeras 10 próximas cenas:

![](implement-efficient-data-paging/_static/image3.png)

Y cuando escribamos */Dinners/Page/1* veremos la siguiente página de cenas:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Adición de la interfaz de usuario de navegación de página

El último paso para completar nuestro escenario de paginación será implementar la interfaz de usuario de navegación "siguiente" y "anterior" dentro de nuestra plantilla de vista para permitir a los usuarios omitir fácilmente los datos de la cena.

Para implementar esto correctamente, necesitaremos saber el número total de cenas en la base de datos, así como cuántas páginas de datos se traduce nalto. A continuación, tendremos que calcular si el valor de "página" solicitado actualmente está al principio o al final de los datos, y mostrar u ocultar la interfaz de usuario "anterior" y "siguiente" en consecuencia. Podríamos implementar esta lógica dentro de nuestro método de acción Index(). Alternativamente, podemos agregar una clase auxiliar a nuestro proyecto que encapsula esta lógica de una manera más reutilizable.

A continuación se muestra una clase auxiliar simple "PaginatedList" que se deriva de la clase de colección List&lt;T&gt; integrada en .NET Framework. Implementa una clase de colección reutilizable que se puede usar para paginar cualquier secuencia de datos IQueryable. En nuestra aplicación NerdDinner haremos que funcione&lt;&gt; sobre los resultados de IQueryable Dinner,&lt;&gt; pero podría&lt;usarse con la misma facilidad en los resultados de IQueryable Product o IQueryable Customer&gt; en otros escenarios de aplicación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Observe anteriormente cómo calcula y, a continuación, expone propiedades como "PageIndex", "PageSize", "TotalCount" y "TotalPages". A continuación, también expone dos propiedades auxiliares "HasPreviousPage" y "HasNextPage" que indican si la página de datos de la colección está al principio o al final de la secuencia original. El código anterior hará que se ejecuten dos consultas SQL ( la primera para recuperar el recuento del número total de objetos Dinner (esto no devuelve los objetos- sino que realiza una instrucción "SELECT COUNT" que devuelve un entero) y la segunda para recuperar solo las filas de datos que necesitamos de nuestra base de datos para la página actual de datos.

A continuación, podemos actualizar nuestro método auxiliar DinnersController.Index() para crear una cena&lt;&gt; PaginatedList a partir de nuestro resultado DinnerRepository.FindUpcomingDinners(), y pasarlo a nuestra plantilla de vista:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

A continuación, podemos actualizar la plantilla de vista de Vista&lt;de Vista S/Cenas-Index.aspx para heredar de&lt;&gt;&gt;ViewPage NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; en lugar de ViewPage&lt;IEnumerable Dinner y, a continuación, agregar el siguiente código en la parte inferior de nuestra plantilla de vista para mostrar u ocultar la interfaz de usuario de navegación siguiente y anterior:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Observe anteriormente cómo estamos utilizando el método auxiliar Html.RouteLink() para generar nuestros hipervínculos. Este método es similar al método auxiliar Html.ActionLink() que hemos utilizado anteriormente. La diferencia es que estamos generando la URL usando la regla de enrutamiento "UpcomingDinners" que configuramos dentro de nuestro archivo Global.asax. Esto garantiza que vamos a generar direcciones URL a nuestro método de acción Index() que tienen el formato: */Dinners/Page/'page',* donde el valor de la página es una variable que estamos proporcionando anteriormente en función del PageIndex actual.

Y ahora cuando ejecutemos nuestra aplicación de nuevo veremos 10 cenas a la vez en nuestro navegador:

![](implement-efficient-data-paging/_static/image5.png)

También &lt; &lt; &lt; tenemos &gt; &gt; &gt; y navegación interfaz de usuario en la parte inferior de la página que nos permite saltar hacia adelante y hacia atrás sobre nuestros datos utilizando URL accesibles para el motor de búsqueda:

![](implement-efficient-data-paging/_static/image6.png)

| **Tema secundario: Comprender las implicaciones&lt;de IQueryable T&gt;** |
| --- |
| IQueryable&lt;&gt; T es una característica muy eficaz que permite una variedad de interesantes escenarios de ejecución diferida (como la paginación y las consultas basadas en composición). Al igual que con todas las características de gran alcance, desea tener cuidado con la forma en que lo utiliza y asegúrese de que no se abusa. Es importante reconocer que devolver un&lt;resultado&gt; de IQueryable T desde el repositorio permite que el código de llamada se anexe en métodos de operador encadenados y, por lo tanto, participar en la ejecución final de la consulta. Si no desea proporcionar código de llamada esta capacidad,&lt;debe&gt; devolver IList T o IEnumerable&lt;T&gt; resultados - que contienen los resultados de una consulta que ya se ha ejecutado. Para escenarios de paginación, esto requeriría insertar la lógica de paginación de datos real en el método de repositorio al que se llama. En este escenario, podríamos actualizar nuestro método FinderUpcomingDinners() finder para tener una&lt; &gt; firma que devolvió un PaginatedList: PaginatedList Dinner FindUpcomingDinners(int pageIndex, int pageSize) - O devolver&lt;una&gt; cena&lt;&gt;IList , y usar un param "totalCount" out para devolver el recuento total de Dinners: IList Dinner FindUpcomingDinners(int pageIndex, int pageSize, out |

### <a name="next-step"></a>siguiente paso

Veamos ahora cómo podemos agregar soporte de autenticación y autorización a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Siguiente](secure-applications-using-authentication-and-authorization.md)
