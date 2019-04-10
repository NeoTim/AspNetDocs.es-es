---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Crear una restricción de ruta (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther demuestra cómo puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de las restricciones de ruta con expresiones regulares.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 8c7b2274ff396f222382488ed877599e86ae5b99
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59412689"
---
# <a name="creating-a-route-constraint-vb"></a><span data-ttu-id="2a2e2-103">Crear una restricción de ruta (VB)</span><span class="sxs-lookup"><span data-stu-id="2a2e2-103">Creating a Route Constraint (VB)</span></span>

<span data-ttu-id="2a2e2-104">by [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2a2e2-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="2a2e2-105">En este tutorial, Stephen Walther demuestra cómo puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de las restricciones de ruta con expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="2a2e2-106">Utilice las restricciones de ruta para restringir las solicitudes del explorador que coinciden con una ruta determinada.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="2a2e2-107">Puede usar una expresión regular para especificar una restricción de ruta.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="2a2e2-108">Por ejemplo, imagine que ha definido la ruta en el listado 1 en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

**<span data-ttu-id="2a2e2-109">Listing 1 - Global.asax.vb</span><span class="sxs-lookup"><span data-stu-id="2a2e2-109">Listing 1 - Global.asax.vb</span></span>**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="2a2e2-110">Listado 1 contiene una ruta con el nombre de producto.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="2a2e2-111">Puede usar la ruta de producto para asignar las solicitudes del explorador a ProductController contenido en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

**<span data-ttu-id="2a2e2-112">Listado 2 - Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="2a2e2-112">Listing 2 - Controllers\ProductController.vb</span></span>**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="2a2e2-113">Tenga en cuenta que la acción Details() expuesta por el controlador del producto acepta un solo parámetro denominado productId.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="2a2e2-114">Este parámetro es un entero.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="2a2e2-115">La ruta definida en el listado 1 coincide con cualquiera de las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="2a2e2-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="2a2e2-116">/ Product/23</span><span class="sxs-lookup"><span data-stu-id="2a2e2-116">/Product/23</span></span>
- <span data-ttu-id="2a2e2-117">/ Product/7</span><span class="sxs-lookup"><span data-stu-id="2a2e2-117">/Product/7</span></span>

<span data-ttu-id="2a2e2-118">Por desgracia, la ruta también coincidirá con las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="2a2e2-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="2a2e2-119">/ Product/bLa, bla</span><span class="sxs-lookup"><span data-stu-id="2a2e2-119">/Product/blah</span></span>
- <span data-ttu-id="2a2e2-120">/ Product/apple</span><span class="sxs-lookup"><span data-stu-id="2a2e2-120">/Product/apple</span></span>

<span data-ttu-id="2a2e2-121">Dado que la acción Details() espera un parámetro entero, que realiza una solicitud que contiene un valor distinto de un valor entero se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="2a2e2-122">Por ejemplo, si escribe la dirección URL /Product/apple en el explorador, a continuación, obtendrá la página de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


[![T<span data-ttu-id="2a2e2-123">el cuadro de diálogo nuevo proyecto]</span><span class="sxs-lookup"><span data-stu-id="2a2e2-123">he New Project dialog box]</span></span>(creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

<span data-ttu-id="2a2e2-124">**Figura 01**: Puede ver una página explode ([haga clic aquí para ver imagen en tamaño completo](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2a2e2-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="2a2e2-125">Lo realmente desea es que solo coinciden con las direcciones URL que contienen un productId entero adecuado.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="2a2e2-126">Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="2a2e2-127">La ruta de producto modificada en el listado 3 contiene una restricción de expresión regular que coincida solo con enteros.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

**<span data-ttu-id="2a2e2-128">Listing 3 - Global.asax.vb</span><span class="sxs-lookup"><span data-stu-id="2a2e2-128">Listing 3 - Global.asax.vb</span></span>**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="2a2e2-129">La expresión regular de \d+ coincide con uno o más números enteros.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="2a2e2-130">Esta restricción hace que la ruta de productos para que coincida con las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="2a2e2-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="2a2e2-131">/ Product/3</span><span class="sxs-lookup"><span data-stu-id="2a2e2-131">/Product/3</span></span>
- <span data-ttu-id="2a2e2-132">/ Product/8999</span><span class="sxs-lookup"><span data-stu-id="2a2e2-132">/Product/8999</span></span>

<span data-ttu-id="2a2e2-133">Pero no las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="2a2e2-133">But not the following URLs:</span></span>

- <span data-ttu-id="2a2e2-134">/ Product/apple</span><span class="sxs-lookup"><span data-stu-id="2a2e2-134">/Product/apple</span></span>
- <span data-ttu-id="2a2e2-135">O producto</span><span class="sxs-lookup"><span data-stu-id="2a2e2-135">/Product</span></span>

<span data-ttu-id="2a2e2-136">Estas solicitudes de explorador se controlarán mediante otra ruta o, si no hay ninguna ruta coincidente, un *no se encontró el recurso* , se devolverá el error.</span><span class="sxs-lookup"><span data-stu-id="2a2e2-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a2e2-137">[Anterior](creating-custom-routes-vb.md)
> [Siguiente](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2a2e2-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
