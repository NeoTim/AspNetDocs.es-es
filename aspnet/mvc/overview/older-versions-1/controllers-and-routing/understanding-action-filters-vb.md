---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Descripción de los filtros de acción (VB) Microsoft Docs
author: rick-anderson
description: El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o a un controlador completo...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 70ab564bc1217f67874090d2ae6899d9bcd743a1
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542110"
---
# <a name="understanding-action-filters-vb"></a>Descripción de los filtros de acción (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador (o a un controlador completo) que modifica la forma en que se ejecuta la acción.

## <a name="understanding-action-filters"></a>Comprender los filtros de acción

El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador (o a un controlador completo) que modifica la forma en que se ejecuta la acción. El marco de ASP.NET MVC incluye varios filtros de acción:

- OutputCache: este filtro de acción almacena en caché la salida de una acción de controlador durante un período de tiempo especificado.
- HandleError: este filtro de acción controla los errores generados cuando se ejecuta una acción del controlador.
- Autorizar: este filtro de acción le permite restringir el acceso a un usuario o rol determinado.

También puede crear sus propios filtros de acción personalizados. Por ejemplo, es posible que desee crear un filtro de acción personalizado para implementar un sistema de autenticación personalizado. O bien, es posible que desee crear un filtro de acción que modifique los datos de vista devueltos por una acción de controlador.

En este tutorial, aprenderá a crear un filtro de acción desde cero. Creamos un filtro de acción Registro que registra diferentes etapas del procesamiento de una acción en la ventana Salida de Visual Studio.

### <a name="using-an-action-filter"></a>Uso de un filtro de acción

Un filtro de acción es un atributo. Puede aplicar la mayoría de los filtros de acción a una acción de controlador individual o a un controlador completo.

Por ejemplo, el controlador de datos en `Index()` el listado 1 expone una acción denominada que devuelve la hora actual. Esta acción está `OutputCache` decorada con el filtro de acción. Este filtro hace que el valor devuelto por la acción se almacene en caché durante 10 segundos.

**Listado 1 –`Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Si invoca repetidamente `Index()` la acción introduciendo la dirección URL /Data/Index en la barra de direcciones de su navegador y pulsando el botón Actualizar varias veces, verá la misma hora durante 10 segundos. La salida `Index()` de la acción se almacena en caché durante 10 segundos (véase el cuadro 1).

[![Tiempo en caché](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figura 01**: Tiempo en caché[(Haga clic para ver la imagen de tamaño completo)](understanding-action-filters-vb/_static/image3.png)

En el listado 1, se `OutputCache` aplica un único `Index()` filtro de acción (el filtro de acción) al método. Si lo necesita, puede aplicar varios filtros de acción a la misma acción. Por ejemplo, es posible que `OutputCache` `HandleError` desee aplicar los filtros y los filtros de acción a la misma acción.

En el listado `OutputCache` 1, el `Index()` filtro de acción se aplica a la acción. También podría aplicar este `DataController` atributo a la propia clase. En ese caso, el resultado devuelto por cualquier acción expuesta por el controlador se almacenaría en caché durante 10 segundos.

### <a name="the-different-types-of-filters"></a>Los diferentes tipos de filtros

El marco de ASP.NET MVC admite cuatro tipos diferentes de filtros:

1. Filtros de autorización: implementa el `IAuthorizationFilter` atributo.
2. Filtros de acción: implementa el `IActionFilter` atributo.
3. Filtros de resultados: implementa el `IResultFilter` atributo.
4. Filtros de excepción: implementa el `IExceptionFilter` atributo.

Los filtros se ejecutan en el orden indicado anteriormente. Por ejemplo, los filtros de autorización siempre se ejecutan antes de que los filtros de acción y los filtros de excepción siempre se ejecuten después de cualquier otro tipo de filtro.

Los filtros de autorización se utilizan para implementar la autenticación y la autorización para las acciones del controlador. Por ejemplo, el filtro Autorizar es un ejemplo de un filtro Autorización.

Los filtros de acción contienen lógica que se ejecuta antes y después de que se ejecute una acción de controlador. Puede usar un filtro de acción, por ejemplo, para modificar los datos de vista que devuelve una acción de controlador.

Los filtros de resultados contienen lógica que se ejecuta antes y después de ejecutar un resultado de vista. Por ejemplo, es posible que desee modificar un resultado de vista justo antes de que la vista se represente en el explorador.

Los filtros de excepción son el último tipo de filtro que se ejecuta. Puede usar un filtro de excepciones para controlar los errores generados por las acciones del controlador o los resultados de la acción del controlador. También puede usar filtros de excepción para registrar errores.

Cada tipo diferente de filtro se ejecuta en un orden determinado. Si desea controlar el orden en el que se ejecutan los filtros del mismo tipo, puede establecer la propiedad Order de un filtro.

La clase base para todos `System.Web.Mvc.FilterAttribute` los filtros de acción es la clase. Si desea implementar un tipo determinado de filtro, debe crear una clase que herede de la clase Filter base e implemente una o varias de las interfaces IAuthorizationFilter, IActionFilter, IResultFilter o ExceptionFilter.

### <a name="the-base-actionfilterattribute-class"></a>La clase Base ActionFilterAttribute

Para facilitar la implementación de un filtro de acción personalizado, `ActionFilterAttribute` el marco de ASP.NET MVC incluye una clase base. Esta clase implementa `IActionFilter` las `IResultFilter` interfaces y hereda de la `Filter` clase.

La terminología aquí no es del todo consistente. Técnicamente, una clase que hereda de la clase ActionFilterAttribute es un filtro de acción y un filtro de resultados. Sin embargo, en el sentido suelto, la palabra filtro de acción se usa para hacer referencia a cualquier tipo de filtro en el marco de ASP.NET MVC.

La clase Base ActionFilterAttribute tiene los siguientes métodos que puede invalidar:

- OnActionExecuting: se llama a este método antes de ejecutar una acción de controlador.
- OnActionExecuted: se llama a este método después de ejecutar una acción de controlador.
- OnResultExecuting: se llama a este método antes de ejecutar un resultado de acción de controlador.
- OnResultExecuted: se llama a este método después de ejecutar un resultado de acción de controlador.

En la siguiente sección, veremos cómo puede implementar cada uno de estos métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Creación de un filtro de acción de registro

Para ilustrar cómo puede crear un filtro de acción personalizado, crearemos un filtro de acción personalizado que registra las etapas de procesamiento de una acción de controlador en la ventana Salida de Visual Studio. Nuestro `LogActionFilter` está contenido en el listado 2.

**Listado 2 –`ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

En el listado `OnActionExecuting()` `OnActionExecuted()`2, `OnResultExecuted()` los métodos `Log()` , , `OnResultExecuting()`, y todos llaman al método. El nombre del método y los datos `Log()` de ruta actuales se pasan al método. El `Log()` método escribe un mensaje en la ventana Salida de Visual Studio (consulte la figura 2).

[![Escribir en la ventana Salida de Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figura 02**: Escritura en la ventana Salida de Visual Studio (Haga clic para ver la[imagen de tamaño completo)](understanding-action-filters-vb/_static/image6.png)

El controlador de inicio en el listado 3 ilustra cómo puede aplicar el filtro de acción de registro a una clase de controlador completa. Siempre que se invoca cualquiera de las acciones `Index()` expuestas `About()` por el controlador Home, ya sea el método o el método, las etapas de procesamiento de la acción se registran en la ventana de salida de Visual Studio.

**Listado 3 –`Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Resumen

En este tutorial, se le presentaron los filtros de acción de ASP.NET MVC. Ha aprendido acerca de los cuatro tipos diferentes de filtros: filtros de autorización, filtros de acción, filtros de resultados y filtros de excepción. También aprendiste `ActionFilterAttribute` acerca de la clase base.

Por último, aprendió a implementar un filtro de acción simple. Hemos creado un filtro de acción de registro que registra las fases de procesamiento de una acción de controlador en la ventana Salida de Visual Studio.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-routing-overview-vb.md)
> [Siguiente](improving-performance-with-output-caching-vb.md)
