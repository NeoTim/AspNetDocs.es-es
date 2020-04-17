---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Pasar datos a ver páginas maestras (VB) Microsoft Docs
author: rick-anderson
description: El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de vista. Examinamos dos estrategias para pasar datos a una vista m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: ef65541a5f2297414f923e2f8108a14de67531e5
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542474"
---
# <a name="passing-data-to-view-master-pages-vb"></a>Pasar datos a las páginas maestras de vista (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de vista. Examinamos dos estrategias para pasar datos a una página maestra de vista. En primer lugar, analizamos una solución fácil que da como resultado una aplicación que es difícil de mantener. A continuación, examinamos una solución mucho mejor que requiere un poco más de trabajo inicial, pero da como resultado una aplicación mucho más mantenible.

## <a name="passing-data-to-view-master-pages"></a>Pasar datos para ver páginas maestras

El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de vista. Examinamos dos estrategias para pasar datos a una página maestra de vista. En primer lugar, analizamos una solución fácil que da como resultado una aplicación que es difícil de mantener. A continuación, examinamos una solución mucho mejor que requiere un poco más de trabajo inicial, pero da como resultado una aplicación mucho más mantenible.

### <a name="the-problem"></a>El problema

Imagine que está creando una aplicación de base de datos de películas y desea mostrar la lista de categorías de películas en cada página de la aplicación (consulte la figura 1). Imagine, además, que la lista de categorías de películas se almacena en una tabla de base de datos. En ese caso, tendría sentido recuperar las categorías de la base de datos y representar la lista de categorías de película dentro de una página maestra de vista.

[![Visualización de categorías de película en una página maestra de vista](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: Visualización de categorías de película en una página maestra de vista (Haga clic para ver la[imagen de tamaño completo)](passing-data-to-view-master-pages-vb/_static/image3.png)

Este es el problema. ¿Cómo se recupera la lista de categorías de películas en la página maestra? Es tentador llamar directamente a métodos de las clases de modelo en la página maestra. En otras palabras, es tentador incluir el código para recuperar los datos de la base de datos directamente en la página maestra. Sin embargo, omitir los controladores MVC para tener acceso a la base de datos infringiría la separación limpia de preocupaciones que es una de las principales ventajas de crear una aplicación MVC.

En una aplicación MVC, desea que todas las interacciones entre las vistas MVC y el modelo MVC sean controladas por los controladores MVC. Esta separación de preocupaciones da como resultado una aplicación más mantenible, adaptable y comprobable.

En una aplicación MVC, todos los datos pasados a una vista, incluida una página maestra de vista, se deben pasar a una vista mediante una acción de controlador. Además, los datos deben pasarse aprovechando los datos de vista. En el resto de este tutorial, examino dos métodos para pasar datos de vista a una página maestra de vista.

### <a name="the-simple-solution"></a>La solución simple

Comencemos con la solución más sencilla para pasar datos de vista de un controlador a una página maestra de vista. La solución más sencilla es pasar los datos de vista de la página maestra en todas y cada una de las acciones del controlador.

Considere el controlador en el listado 1. Expone dos acciones `Index()` denominadas y `Details()`. El `Index()` método action devuelve todas las películas de la tabla de base de datos Movies. El `Details()` método action devuelve todas las películas de una categoría de película determinada.

**Listado 1 –`Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Tenga en `Index()` cuenta `Details()` que las acciones y las de la acción agregan dos elementos para ver los datos. La `Index()` acción añade dos claves: categorías y películas. La clave categories representa la lista de categorías de película mostradas por la página maestra de vista. La tecla movies representa la lista de películas mostradas por la página de vista de índice.

La `Details()` acción también agrega dos claves denominadas categorías y películas. La clave de categorías, una vez más, representa la lista de categorías de película mostradas por la página maestra de vista. La clave movies representa la lista de películas de una categoría determinada que se muestra en la página de vista Detalles (consulte la figura 2).

[![La vista Detalles](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: La vista Detalles[(Haga clic para ver la imagen de tamaño completo)](passing-data-to-view-master-pages-vb/_static/image6.png)

La vista de índice se encuentra en el listado 2. Simplemente recorre en iteración la lista de películas representadas por el elemento de películas en los datos de vista.

**Listado 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La página maestra de la vista se encuentra en el listado 3. La página maestra de vista recorre en iteración y representa todas las categorías de película representadas por el elemento de categorías a partir de los datos de vista.

**Listado 3 –`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Todos los datos se pasan a la vista y a la página maestra de la vista a través de los datos de vista. Esa es la forma correcta de pasar datos a la página maestra.

Entonces, ¿qué tiene de malo esta solución? El problema es que esta solución infringe el principio DRY (Don't Repeat Yourself). Todas y cada una de las acciones del controlador deben agregar la misma lista de categorías de película para ver los datos. Tener código duplicado en la aplicación hace que la aplicación sea mucho más difícil de mantener, adaptar y modificar.

### <a name="the-good-solution"></a>La buena solución

En esta sección, examinamos una alternativa, y mejor, solución para pasar datos de una acción de controlador a una página maestra de vista. En lugar de agregar las categorías de película para la página maestra en todas y cada una de las acciones del controlador, agregamos las categorías de película a los datos de vista solo una vez. Todos los datos de vista utilizados por la página maestra de vista se agregan en un controlador de aplicación.

La clase ApplicationController se encuentra en el listado 4.

La clase ApplicationController se encuentra en el listado 4.

**Listado 4 –`Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Hay tres cosas que debe tener en cuenta sobre el controlador de aplicación en el listado 4. En primer lugar, observe que la clase hereda de la clase base System.Web.Mvc.Controller. El controlador de aplicación es una clase de controlador.

En segundo lugar, observe que la clase de controlador de aplicación es una clase MustInherit. Una clase MustInherit es una clase que debe implementarse mediante una clase concreta. Dado que el controlador de aplicación es un MustInherit clase, no se puede invocar ningún método definido en la clase directamente. Si intenta invocar la clase Application directamente, obtendrá un mensaje de error Resource Cannot Be Found.

En tercer lugar, tenga en cuenta que el controlador de aplicación contiene un constructor que agrega la lista de categorías de película para ver los datos. Cada clase de controlador que hereda del controlador de aplicación llama automáticamente al constructor del controlador de aplicación. Cada vez que se llama a cualquier acción en cualquier controlador que hereda del controlador de aplicación, las categorías de película se incluyen en los datos de vista automáticamente.

El Movies controlador en el listado 5 hereda de la Application controlador.

**Listado 5 –`Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

El Movies controlador, al igual que el Home controlador discutido en `Index()` `Details()`la sección anterior, expone dos métodos de acción denominados y . Observe que la lista de categorías de película mostradas por la `Index()` página `Details()` maestra de vista no se agrega para ver los datos en el método o. Dado que el Movies controlador hereda de la Application controlador, la lista de categorías de película se agrega para ver los datos automáticamente.

Tenga en cuenta que esta solución para agregar datos de vista para una página maestra de vista no infringe el principio DRY (Don't Repeat Yourself). El código para agregar la lista de categorías de película para ver datos está contenido en una sola ubicación: el constructor para el controlador de aplicación.

### <a name="summary"></a>Resumen

En este tutorial, se describen dos enfoques para pasar datos de vista de un controlador a una página maestra de vista. En primer lugar, examinamos un enfoque simple, pero difícil de mantener. En la primera sección, se describe cómo puede agregar datos de vista para una página maestra de vista en cada acción de controlador en la aplicación. Llegamos a la conclusión de que este era un mal enfoque porque viola el principio DRY (Don't Repeat Yourself).

A continuación, examinamos una estrategia mucho mejor para agregar datos requeridos por una página maestra de vista para ver los datos. En lugar de agregar los datos de vista en todas y cada una de las acciones del controlador, agregamos los datos de vista solo una vez dentro de un controlador de aplicación. De este modo, puede evitar el código duplicado al pasar datos a una página maestra de vista en una aplicación MVC ASP.NET.

> [!div class="step-by-step"]
> [Anterior](creating-page-layouts-with-view-master-pages-vb.md)
