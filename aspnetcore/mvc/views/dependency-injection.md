---
title: Inserción de dependencias en vistas de ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo ASP.NET Core admite la inserción de dependencias en las vistas de MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: dfadafe9ebb5799b45ef68653f20c5fc1a2506b5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025462"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>Inserción de dependencias en vistas de ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core admite la [inserción de dependencias](xref:fundamentals/dependency-injection) en vistas. Esto puede ser útil para servicios específicos de vistas, como la localización o los datos necesarios solamente para rellenar los elementos de vistas. Debe intentar mantener la [separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) entre los controladores y las vistas. La mayoría de los datos que muestran las vistas deben pasarse desde el controlador.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Ejemplo sencillo

Puede insertar un servicio en una vista mediante la directiva `@inject`. Puede pensar que `@inject` es como si agregara una propiedad a la vista y rellenara la propiedad mediante DI.

La sintaxis de `@inject`: `@inject <type> <name>`

Un ejemplo de `@inject` en acción:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Esta vista muestra una lista de instancias `ToDoItem`, junto con un resumen de estadísticas generales. El resumen se rellena a partir de `StatisticsService` insertado. Este servicio está registrado para la inserción de dependencias en `ConfigureServices` en *Startup.cs*:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` realiza algunos cálculos en el conjunto de instancias de `ToDoItem`, al que se accede a través de un repositorio:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

El repositorio de ejemplo usa una colección en memoria. La implementación que se muestra arriba (que funciona en todos los datos en memoria) no se recomienda para conjuntos de datos grandes, con acceso de forma remota.

El ejemplo muestra los datos del modelo enlazado a la vista y del servicio que se inserta en la vista:

![Vista de tareas pendientes que enumera los elementos totales, los elementos completados, la prioridad media y una lista de tareas con sus niveles de propiedad y valores booleanos que indican si se han completado.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Rellenar datos de búsqueda

La inserción de vistas puede ser útil para rellenar opciones en elementos de interfaz de usuario, como por ejemplo, listas desplegables. Imagine un formulario de perfil de usuario que incluye opciones para especificar el sexo, el estado y otras preferencias. Para representar este tipo de formulario mediante un enfoque MVC estándar, necesitaría que el controlador solicitara servicios de acceso a datos para cada uno de estos conjuntos de opciones y, después, rellenar un modelo o `ViewBag` con cada conjunto de opciones para enlazar.

Un enfoque alternativo consiste en insertar servicios directamente en la vista para obtener las opciones. Esto reduce la cantidad de código necesario para el controlador, ya que mueve esta lógica de construcción del elemento de vista a la propia vista. La acción del controlador para mostrar un formulario de edición de perfil solamente necesita pasar el formulario de la instancia de perfil:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

El formulario HTML que se utilizó para actualizar estas preferencias incluye listas desplegables para tres de las propiedades:

![Vista de actualización de perfil, con un formulario que permite la entrada del nombre, el género, el estado y el color favorito.](dependency-injection/_static/updateprofile.png)

Estas listas se rellenan mediante un servicio que se ha insertado en la vista:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` es un servicio de nivel de interfaz de usuario diseñado para proporcionar solo los datos necesarios para este formulario:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> No olvide registrar los tipos que solicite a través de la inserción de dependencias en `Startup.ConfigureServices`. Un tipo no registrado produce una excepción en tiempo de ejecución porque el proveedor de servicios se consulta internamente a través de [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).

## <a name="overriding-services"></a>Reemplazar servicios

Además de insertar nuevos servicios, esta técnica también puede usarse para reemplazar servicios previamente insertados en una página. En la imagen de abajo se muestran todos los campos disponibles en la página usada en el primer ejemplo:

![Menú contextual de IntelliSense de un símbolo @ de tipo que enumera los campos Html, componente, StatsService y URL](dependency-injection/_static/razor-fields.png)

Como puede ver, los campos predeterminados incluyen `Html`, `Component` y `Url` (además de `StatsService` que hemos insertado). Si, por ejemplo, quisiera reemplazar los asistentes de HTML predeterminadas con las suyas propias, puede hacerlo fácilmente mediante `@inject`:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Si quiere ampliar los servicios existentes, simplemente puede usar esta técnica al heredar o encapsular la implementación existente con la suya propia.

## <a name="see-also"></a>Vea también

* Blog de Simon Timms: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/) (Obtener datos de búsqueda en la vista)
