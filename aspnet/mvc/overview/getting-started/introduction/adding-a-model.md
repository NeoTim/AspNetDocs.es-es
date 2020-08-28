---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Agregar un modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 453a55bd9f16c7b33525de18bc49493d22776f47
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045122"
---
# <a name="adding-a-model"></a><span data-ttu-id="ffbc8-102">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="ffbc8-102">Adding a Model</span></span>

<span data-ttu-id="ffbc8-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ffbc8-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ffbc8-104">En esta sección, agregará algunas clases para administrar películas en una base de datos de.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="ffbc8-105">Estas clases serán la &quot; parte del modelo &quot; de la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="ffbc8-106">Usará una .NET Framework tecnología de acceso a datos conocida como [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="ffbc8-107">El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="ffbc8-108">Code First permite crear objetos de modelo escribiendo clases simples.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="ffbc8-109">(También se conocen como clases POCO, a partir de &quot; objetos CLR antiguos). &quot; Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="ffbc8-110">Si necesita crear primero la base de datos, puede seguir este tutorial para obtener información sobre el desarrollo de aplicaciones MVC y EF.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="ffbc8-111">Después, puede seguir el tutorial de [scaffolding Fizmakens ASP.net](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) , que cubre el primer enfoque de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="ffbc8-112">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="ffbc8-112">Adding Model Classes</span></span>

<span data-ttu-id="ffbc8-113">En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="ffbc8-114">Escriba el nombre de *clase* &quot; Movie &quot; .</span><span class="sxs-lookup"><span data-stu-id="ffbc8-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="ffbc8-115">Agregue las siguientes cinco propiedades a la `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="ffbc8-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="ffbc8-116">Usaremos la `Movie` clase para representar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="ffbc8-117">Cada instancia de un `Movie` objeto se corresponderá con una fila de una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna de la tabla.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="ffbc8-118">Nota: para poder usar System. Data. Entity y la clase relacionada, debe instalar el [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="ffbc8-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="ffbc8-119">Siga el vínculo para obtener más instrucciones.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="ffbc8-120">En el mismo archivo, agregue la siguiente `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="ffbc8-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="ffbc8-121">La `MovieDBContext` clase representa el contexto de la base de datos de Entity Framework Movie, que controla la captura, el almacenamiento y la actualización `Movie` de instancias de clase en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="ffbc8-122">`MovieDBContext`Deriva de la `DbContext` clase base proporcionada por el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="ffbc8-123">Para poder hacer referencia a `DbContext` y, debe `DbSet` Agregar la siguiente `using` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="ffbc8-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="ffbc8-124">Para ello, puede Agregar manualmente la instrucción using o puede mantener el mouse sobre las líneas onduladas rojas, hacer clic `Show potential fixes` y hacer clic en `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="ffbc8-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="ffbc8-125">Nota: `using` se han quitado varias instrucciones no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="ffbc8-126">Visual Studio mostrará las dependencias no usadas en gris.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="ffbc8-127">Para quitar las dependencias no utilizadas, mantenga el mouse sobre las dependencias grises, haga clic `Show potential fixes` y haga clic en **quitar las utilizaciones no utilizadas.**</span><span class="sxs-lookup"><span data-stu-id="ffbc8-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="ffbc8-128">Por último, hemos agregado un modelo (la M en MVC).</span><span class="sxs-lookup"><span data-stu-id="ffbc8-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="ffbc8-129">En la siguiente sección, trabajará con la cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ffbc8-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ffbc8-130">[Anterior](adding-a-view.md)
> [Siguiente](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="ffbc8-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
