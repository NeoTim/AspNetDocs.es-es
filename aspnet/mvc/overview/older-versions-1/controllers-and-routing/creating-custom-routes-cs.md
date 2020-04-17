---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Creación de rutas personalizadas (C-) Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: b66ccc5e0cd4f6d7e5884394c2b7555b76382d3d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542760"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="4cc4e-104">Crear rutas personalizadas (C#)</span><span class="sxs-lookup"><span data-stu-id="4cc4e-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="4cc4e-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4cc4e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4cc4e-106">Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="4cc4e-107">En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="4cc4e-108">En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="4cc4e-109">Aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax con una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="4cc4e-110">Para muchas aplicaciones mvc ASP.NET simples, la tabla de rutas predeterminada funcionará perfectamente.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="4cc4e-111">Sin embargo, es posible que descubra que tiene necesidades de enrutamiento especializadas.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="4cc4e-112">En ese caso, puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="4cc4e-113">Imagine, por ejemplo, que está creando una aplicación de blog.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="4cc4e-114">Es posible que desee controlar las solicitudes entrantes que se ven así:</span><span class="sxs-lookup"><span data-stu-id="4cc4e-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="4cc4e-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="4cc4e-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="4cc4e-116">Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog que corresponde a la fecha 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="4cc4e-117">Para manejar este tipo de petición, usted necesita crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="4cc4e-118">El archivo Global.asax del listado 1 contiene una nueva ruta personalizada, denominada Blog, que controla las solicitudes que se parecen a /Archive/ fecha de*entrada*.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="4cc4e-119">**Listado 1 - Global.asax (con ruta personalizada)**</span><span class="sxs-lookup"><span data-stu-id="4cc4e-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="4cc4e-120">El orden de las rutas que se agregan a la tabla de rutas es importante.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="4cc4e-121">Nuestra nueva ruta de blog personalizada se agrega antes de la ruta predeterminada existente.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="4cc4e-122">Si ha invertido el orden, siempre se llamará a la ruta predeterminada en lugar de a la ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="4cc4e-123">La ruta blog personalizada coincide con cualquier solicitud que comience por /Archive/.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="4cc4e-124">Por lo tanto, coincide con todas las siguientes direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="4cc4e-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="4cc4e-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="4cc4e-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="4cc4e-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="4cc4e-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="4cc4e-127">/Archivo/manzana</span><span class="sxs-lookup"><span data-stu-id="4cc4e-127">/Archive/apple</span></span>

<span data-ttu-id="4cc4e-128">La ruta personalizada asigna la solicitud entrante a un controlador denominado Archive e invoca la acción Entry().</span><span class="sxs-lookup"><span data-stu-id="4cc4e-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="4cc4e-129">Cuando se llama al método Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="4cc4e-130">Puede usar la ruta personalizada Blog con el controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="4cc4e-131">**Listado 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="4cc4e-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="4cc4e-132">Observe que el método Entry() del listado 2 acepta un parámetro de tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="4cc4e-133">El marco MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un DateTime valor automáticamente.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="4cc4e-134">Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un DateTime, se produce un error (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="4cc4e-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="4cc4e-135">**Figura 1 - Error al convertir el parámetro**</span><span class="sxs-lookup"><span data-stu-id="4cc4e-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="4cc4e-136">[![Cuadro de diálogo Nuevo proyecto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4cc4e-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="4cc4e-137">**Figura 01**: Error al convertir el parámetro ([Haga clic para ver la imagen de tamaño completo](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4cc4e-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="4cc4e-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="4cc4e-138">Summary</span></span>

<span data-ttu-id="4cc4e-139">El objetivo de este tutorial era demostrar cómo se puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="4cc4e-140">Ha aprendido a agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="4cc4e-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="4cc4e-141">Hemos discutido cómo asignar solicitudes de entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominada Entry().</span><span class="sxs-lookup"><span data-stu-id="4cc4e-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4cc4e-142">[Anterior](aspnet-mvc-controllers-overview-cs.md)
> [Siguiente](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4cc4e-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
