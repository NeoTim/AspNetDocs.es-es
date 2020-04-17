---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Creación de rutas personalizadas (VB) Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd12307f685fa064832bb4198df06df2c3f2d3be
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542747"
---
# <a name="creating-custom-routes-vb"></a><span data-ttu-id="d9401-104">Crear rutas personalizadas (VB)</span><span class="sxs-lookup"><span data-stu-id="d9401-104">Creating Custom Routes (VB)</span></span>

<span data-ttu-id="d9401-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d9401-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d9401-106">Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d9401-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="d9401-107">En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="d9401-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="d9401-108">En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d9401-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="d9401-109">Aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax con una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="d9401-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="d9401-110">En ASP.NET aplicaciones MVC, la tabla de rutas predeterminada funcionará perfectamente.</span><span class="sxs-lookup"><span data-stu-id="d9401-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="d9401-111">Sin embargo, es posible que descubra que tiene necesidades de enrutamiento especializadas.</span><span class="sxs-lookup"><span data-stu-id="d9401-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="d9401-112">En ese caso, puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="d9401-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="d9401-113">Imagine, por ejemplo, que está creando una aplicación de blog.</span><span class="sxs-lookup"><span data-stu-id="d9401-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="d9401-114">Es posible que desee controlar las solicitudes entrantes que se ven así:</span><span class="sxs-lookup"><span data-stu-id="d9401-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="d9401-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="d9401-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="d9401-116">Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog que corresponde a la fecha 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="d9401-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="d9401-117">Para manejar este tipo de petición, usted necesita crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="d9401-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="d9401-118">El archivo Global.asax del listado 1 contiene una nueva ruta personalizada, denominada Blog, que controla las solicitudes que se parecen a /Archive/ fecha de*entrada*.</span><span class="sxs-lookup"><span data-stu-id="d9401-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="d9401-119">**Listado 1 - Global.asax (con ruta personalizada)**</span><span class="sxs-lookup"><span data-stu-id="d9401-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="d9401-120">El orden de las rutas que se agregan a la tabla de rutas es importante.</span><span class="sxs-lookup"><span data-stu-id="d9401-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="d9401-121">Nuestra nueva ruta de blog personalizada se agrega antes de la ruta predeterminada existente.</span><span class="sxs-lookup"><span data-stu-id="d9401-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="d9401-122">Si ha invertido el orden, siempre se llamará a la ruta predeterminada en lugar de a la ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="d9401-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="d9401-123">La ruta blog personalizada coincide con cualquier solicitud que comience por /Archive/.</span><span class="sxs-lookup"><span data-stu-id="d9401-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="d9401-124">Por lo tanto, coincide con todas las siguientes direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="d9401-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="d9401-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="d9401-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="d9401-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="d9401-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="d9401-127">/Archivo/manzana</span><span class="sxs-lookup"><span data-stu-id="d9401-127">/Archive/apple</span></span>

<span data-ttu-id="d9401-128">La ruta personalizada asigna la solicitud entrante a un controlador denominado Archive e invoca la acción Entry().</span><span class="sxs-lookup"><span data-stu-id="d9401-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="d9401-129">Cuando se llama al método Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.</span><span class="sxs-lookup"><span data-stu-id="d9401-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="d9401-130">Puede usar la ruta personalizada Blog con el controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="d9401-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="d9401-131">**Listado 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="d9401-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="d9401-132">Observe que el método Entry() del listado 2 acepta un parámetro de tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="d9401-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="d9401-133">El marco MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un DateTime valor automáticamente.</span><span class="sxs-lookup"><span data-stu-id="d9401-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="d9401-134">Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un DateTime, se produce un error (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="d9401-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="d9401-135">**Figura 1 - Error al convertir el parámetro**</span><span class="sxs-lookup"><span data-stu-id="d9401-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="d9401-136">[![Cuadro de diálogo Nuevo proyecto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d9401-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="d9401-137">**Figura 01**: Error al convertir el parámetro ([Haga clic para ver la imagen de tamaño completo](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d9401-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="d9401-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="d9401-138">Summary</span></span>

<span data-ttu-id="d9401-139">El objetivo de este tutorial era demostrar cómo se puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="d9401-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="d9401-140">Ha aprendido a agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="d9401-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="d9401-141">Hemos discutido cómo asignar solicitudes de entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominada Entry().</span><span class="sxs-lookup"><span data-stu-id="d9401-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d9401-142">[Anterior](asp-net-mvc-controller-overview-vb.md)
> [Siguiente](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d9401-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
