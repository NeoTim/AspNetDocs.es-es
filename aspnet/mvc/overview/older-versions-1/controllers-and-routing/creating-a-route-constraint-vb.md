---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Crear una restricción de ruta (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo puede controlar cómo las solicitudes del explorador coinciden con las rutas mediante la creación de restricciones de ruta con expresiones regulares.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486775"
---
# <a name="creating-a-route-constraint-vb"></a><span data-ttu-id="7193c-103">Crear una restricción de ruta (VB)</span><span class="sxs-lookup"><span data-stu-id="7193c-103">Creating a Route Constraint (VB)</span></span>

<span data-ttu-id="7193c-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="7193c-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="7193c-105">En este tutorial, Stephen Walther muestra cómo puede controlar cómo las solicitudes del explorador coinciden con las rutas mediante la creación de restricciones de ruta con expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="7193c-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>

<span data-ttu-id="7193c-106">Las restricciones de ruta se usan para restringir las solicitudes del explorador que coinciden con una ruta determinada.</span><span class="sxs-lookup"><span data-stu-id="7193c-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="7193c-107">Puede usar una expresión regular para especificar una restricción de ruta.</span><span class="sxs-lookup"><span data-stu-id="7193c-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="7193c-108">Por ejemplo, Imagine que ha definido la ruta en la lista 1 en el archivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="7193c-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="7193c-109">**Lista 1: global. asax. VB**</span><span class="sxs-lookup"><span data-stu-id="7193c-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="7193c-110">La lista 1 contiene una ruta denominada product.</span><span class="sxs-lookup"><span data-stu-id="7193c-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="7193c-111">Puede usar la ruta del producto para asignar solicitudes del explorador al ProductController incluido en la lista 2.</span><span class="sxs-lookup"><span data-stu-id="7193c-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="7193c-112">**Lista 2-Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="7193c-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="7193c-113">Tenga en cuenta que la acción Details () expuesta por el controlador del producto acepta un único parámetro denominado productId.</span><span class="sxs-lookup"><span data-stu-id="7193c-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="7193c-114">Este parámetro es un parámetro entero.</span><span class="sxs-lookup"><span data-stu-id="7193c-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="7193c-115">La ruta definida en la enumeración 1 coincidirá con cualquiera de las siguientes direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="7193c-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="7193c-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="7193c-116">/Product/23</span></span>
- <span data-ttu-id="7193c-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="7193c-117">/Product/7</span></span>

<span data-ttu-id="7193c-118">Desafortunadamente, la ruta también coincidirá con las siguientes direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="7193c-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="7193c-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="7193c-119">/Product/blah</span></span>
- <span data-ttu-id="7193c-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="7193c-120">/Product/apple</span></span>

<span data-ttu-id="7193c-121">Dado que la acción Details () espera un parámetro de entero, la realización de una solicitud que contenga algo distinto de un valor entero producirá un error.</span><span class="sxs-lookup"><span data-stu-id="7193c-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="7193c-122">Por ejemplo, si escribe la dirección URL/Product/Apple en el explorador, se mostrará la página de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="7193c-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>

<span data-ttu-id="7193c-123">[![el cuadro de diálogo nuevo proyecto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7193c-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="7193c-124">**Figura 01**: visualización de una página de explosión ([haga clic para ver la imagen de tamaño completo](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7193c-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>

<span data-ttu-id="7193c-125">Lo que realmente desea hacer es que solo coincida con las direcciones URL que contengan un número entero de productId adecuado.</span><span class="sxs-lookup"><span data-stu-id="7193c-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="7193c-126">Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta.</span><span class="sxs-lookup"><span data-stu-id="7193c-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="7193c-127">La ruta del producto modificada de la lista 3 contiene una restricción de expresión regular que solo coincide con enteros.</span><span class="sxs-lookup"><span data-stu-id="7193c-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="7193c-128">**Lista 3: global. asax. VB**</span><span class="sxs-lookup"><span data-stu-id="7193c-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="7193c-129">La expresión regular \d + coincide con uno o varios enteros.</span><span class="sxs-lookup"><span data-stu-id="7193c-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="7193c-130">Esta restricción hace que la ruta del producto coincida con las siguientes direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="7193c-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="7193c-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="7193c-131">/Product/3</span></span>
- <span data-ttu-id="7193c-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="7193c-132">/Product/8999</span></span>

<span data-ttu-id="7193c-133">Pero no las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="7193c-133">But not the following URLs:</span></span>

- <span data-ttu-id="7193c-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="7193c-134">/Product/apple</span></span>
- <span data-ttu-id="7193c-135">/Product</span><span class="sxs-lookup"><span data-stu-id="7193c-135">/Product</span></span>

<span data-ttu-id="7193c-136">Estas solicitudes del explorador se controlarán mediante otra ruta o, si no hay ninguna ruta coincidente, se devolverá un error *de recurso no encontrado* .</span><span class="sxs-lookup"><span data-stu-id="7193c-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7193c-137">[Anterior](creating-custom-routes-vb.md)
> [Siguiente](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7193c-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
