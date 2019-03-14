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
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="ba118-103">Inserción de dependencias en vistas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba118-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="ba118-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ba118-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ba118-105">ASP.NET Core admite la [inserción de dependencias](xref:fundamentals/dependency-injection) en vistas.</span><span class="sxs-lookup"><span data-stu-id="ba118-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="ba118-106">Esto puede ser útil para servicios específicos de vistas, como la localización o los datos necesarios solamente para rellenar los elementos de vistas.</span><span class="sxs-lookup"><span data-stu-id="ba118-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="ba118-107">Debe intentar mantener la [separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) entre los controladores y las vistas.</span><span class="sxs-lookup"><span data-stu-id="ba118-107">You should try to maintain [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="ba118-108">La mayoría de los datos que muestran las vistas deben pasarse desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="ba118-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="ba118-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ba118-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="ba118-110">Ejemplo sencillo</span><span class="sxs-lookup"><span data-stu-id="ba118-110">A Simple Example</span></span>

<span data-ttu-id="ba118-111">Puede insertar un servicio en una vista mediante la directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="ba118-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="ba118-112">Puede pensar que `@inject` es como si agregara una propiedad a la vista y rellenara la propiedad mediante DI.</span><span class="sxs-lookup"><span data-stu-id="ba118-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="ba118-113">La sintaxis de `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="ba118-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="ba118-114">Un ejemplo de `@inject` en acción:</span><span class="sxs-lookup"><span data-stu-id="ba118-114">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="ba118-115">Esta vista muestra una lista de instancias `ToDoItem`, junto con un resumen de estadísticas generales.</span><span class="sxs-lookup"><span data-stu-id="ba118-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="ba118-116">El resumen se rellena a partir de `StatisticsService` insertado.</span><span class="sxs-lookup"><span data-stu-id="ba118-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="ba118-117">Este servicio está registrado para la inserción de dependencias en `ConfigureServices` en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ba118-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="ba118-118">`StatisticsService` realiza algunos cálculos en el conjunto de instancias de `ToDoItem`, al que se accede a través de un repositorio:</span><span class="sxs-lookup"><span data-stu-id="ba118-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="ba118-119">El repositorio de ejemplo usa una colección en memoria.</span><span class="sxs-lookup"><span data-stu-id="ba118-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="ba118-120">La implementación que se muestra arriba (que funciona en todos los datos en memoria) no se recomienda para conjuntos de datos grandes, con acceso de forma remota.</span><span class="sxs-lookup"><span data-stu-id="ba118-120">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="ba118-121">El ejemplo muestra los datos del modelo enlazado a la vista y del servicio que se inserta en la vista:</span><span class="sxs-lookup"><span data-stu-id="ba118-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Vista de tareas pendientes que enumera los elementos totales, los elementos completados, la prioridad media y una lista de tareas con sus niveles de propiedad y valores booleanos que indican si se han completado.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="ba118-123">Rellenar datos de búsqueda</span><span class="sxs-lookup"><span data-stu-id="ba118-123">Populating Lookup Data</span></span>

<span data-ttu-id="ba118-124">La inserción de vistas puede ser útil para rellenar opciones en elementos de interfaz de usuario, como por ejemplo, listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="ba118-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="ba118-125">Imagine un formulario de perfil de usuario que incluye opciones para especificar el sexo, el estado y otras preferencias.</span><span class="sxs-lookup"><span data-stu-id="ba118-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="ba118-126">Para representar este tipo de formulario mediante un enfoque MVC estándar, necesitaría que el controlador solicitara servicios de acceso a datos para cada uno de estos conjuntos de opciones y, después, rellenar un modelo o `ViewBag` con cada conjunto de opciones para enlazar.</span><span class="sxs-lookup"><span data-stu-id="ba118-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="ba118-127">Un enfoque alternativo consiste en insertar servicios directamente en la vista para obtener las opciones.</span><span class="sxs-lookup"><span data-stu-id="ba118-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="ba118-128">Esto reduce la cantidad de código necesario para el controlador, ya que mueve esta lógica de construcción del elemento de vista a la propia vista.</span><span class="sxs-lookup"><span data-stu-id="ba118-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="ba118-129">La acción del controlador para mostrar un formulario de edición de perfil solamente necesita pasar el formulario de la instancia de perfil:</span><span class="sxs-lookup"><span data-stu-id="ba118-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="ba118-130">El formulario HTML que se utilizó para actualizar estas preferencias incluye listas desplegables para tres de las propiedades:</span><span class="sxs-lookup"><span data-stu-id="ba118-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Vista de actualización de perfil, con un formulario que permite la entrada del nombre, el género, el estado y el color favorito.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="ba118-132">Estas listas se rellenan mediante un servicio que se ha insertado en la vista:</span><span class="sxs-lookup"><span data-stu-id="ba118-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="ba118-133">`ProfileOptionsService` es un servicio de nivel de interfaz de usuario diseñado para proporcionar solo los datos necesarios para este formulario:</span><span class="sxs-lookup"><span data-stu-id="ba118-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="ba118-134">No olvide registrar los tipos que solicite a través de la inserción de dependencias en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ba118-134">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ba118-135">Un tipo no registrado produce una excepción en tiempo de ejecución porque el proveedor de servicios se consulta internamente a través de [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span><span class="sxs-lookup"><span data-stu-id="ba118-135">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="ba118-136">Reemplazar servicios</span><span class="sxs-lookup"><span data-stu-id="ba118-136">Overriding Services</span></span>

<span data-ttu-id="ba118-137">Además de insertar nuevos servicios, esta técnica también puede usarse para reemplazar servicios previamente insertados en una página.</span><span class="sxs-lookup"><span data-stu-id="ba118-137">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="ba118-138">En la imagen de abajo se muestran todos los campos disponibles en la página usada en el primer ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ba118-138">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menú contextual de IntelliSense de un símbolo @ de tipo que enumera los campos Html, componente, StatsService y URL](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="ba118-140">Como puede ver, los campos predeterminados incluyen `Html`, `Component` y `Url` (además de `StatsService` que hemos insertado).</span><span class="sxs-lookup"><span data-stu-id="ba118-140">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="ba118-141">Si, por ejemplo, quisiera reemplazar los asistentes de HTML predeterminadas con las suyas propias, puede hacerlo fácilmente mediante `@inject`:</span><span class="sxs-lookup"><span data-stu-id="ba118-141">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="ba118-142">Si quiere ampliar los servicios existentes, simplemente puede usar esta técnica al heredar o encapsular la implementación existente con la suya propia.</span><span class="sxs-lookup"><span data-stu-id="ba118-142">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="ba118-143">Vea también</span><span class="sxs-lookup"><span data-stu-id="ba118-143">See Also</span></span>

* <span data-ttu-id="ba118-144">Blog de Simon Timms: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/) (Obtener datos de búsqueda en la vista)</span><span class="sxs-lookup"><span data-stu-id="ba118-144">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
