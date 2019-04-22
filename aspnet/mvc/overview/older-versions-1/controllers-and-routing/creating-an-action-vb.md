---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Creación de una acción (VB) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar una nueva acción a un controlador ASP.NET MVC. Obtenga información sobre los requisitos para un método que se trata de una acción.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f8eeeaa9bd77c0259f680198e57ade8d49cd06b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59382627"
---
# <a name="creating-an-action-vb"></a><span data-ttu-id="d6f70-104">Crear una acción (VB)</span><span class="sxs-lookup"><span data-stu-id="d6f70-104">Creating an Action (VB)</span></span>

<span data-ttu-id="d6f70-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d6f70-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d6f70-106">Obtenga información sobre cómo agregar una nueva acción a un controlador ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d6f70-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="d6f70-107">Obtenga información sobre los requisitos para un método que se trata de una acción.</span><span class="sxs-lookup"><span data-stu-id="d6f70-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="d6f70-108">El objetivo de este tutorial es explicar cómo puede crear una nueva acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="d6f70-109">Conozca los requisitos de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="d6f70-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="d6f70-110">También aprenderá cómo impedir que un método que se expone como una acción.</span><span class="sxs-lookup"><span data-stu-id="d6f70-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="d6f70-111">Agregar una acción a un controlador</span><span class="sxs-lookup"><span data-stu-id="d6f70-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="d6f70-112">Agregar una nueva acción a un controlador mediante la adición de un nuevo método al controlador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="d6f70-113">Por ejemplo, el controlador en el listado 1 contiene una acción denominada Index() y una acción denominada SayHello().</span><span class="sxs-lookup"><span data-stu-id="d6f70-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="d6f70-114">Ambos métodos se exponen como acciones.</span><span class="sxs-lookup"><span data-stu-id="d6f70-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="d6f70-115">**Listing 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6f70-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="d6f70-116">Con el fin de exponerse al universo como una acción, un método debe cumplir ciertos requisitos:</span><span class="sxs-lookup"><span data-stu-id="d6f70-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="d6f70-117">El método debe ser público.</span><span class="sxs-lookup"><span data-stu-id="d6f70-117">The method must be public.</span></span>
- <span data-ttu-id="d6f70-118">El método no puede ser un método estático.</span><span class="sxs-lookup"><span data-stu-id="d6f70-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="d6f70-119">El método no puede ser un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="d6f70-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="d6f70-120">El método no puede ser un constructor de captador o establecedor.</span><span class="sxs-lookup"><span data-stu-id="d6f70-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="d6f70-121">El método no puede tener tipos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="d6f70-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="d6f70-122">El método no es un método de la clase base del controlador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="d6f70-123">El método no puede contener **ref** o **out** parámetros.</span><span class="sxs-lookup"><span data-stu-id="d6f70-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="d6f70-124">Tenga en cuenta que no hay ninguna restricción en el tipo de valor devuelto de una acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="d6f70-125">Una acción de controlador puede devolver una cadena, un valor DateTime, una instancia de la clase Random, o void.</span><span class="sxs-lookup"><span data-stu-id="d6f70-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="d6f70-126">El marco ASP.NET MVC convertirá cualquier tipo de valor devuelto no es un resultado de acción en una cadena y procesar la cadena en el explorador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="d6f70-127">Cuando se agrega a cualquier método que no infrinja estos requisitos a un controlador, el método se expone como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="d6f70-128">Tenga cuidado aquí.</span><span class="sxs-lookup"><span data-stu-id="d6f70-128">Be careful here.</span></span> <span data-ttu-id="d6f70-129">Cualquier persona conectada a Internet puede invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="d6f70-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="d6f70-130">No, por ejemplo, cree una acción de controlador DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="d6f70-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="d6f70-131">Impide que se invoca un método público</span><span class="sxs-lookup"><span data-stu-id="d6f70-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="d6f70-132">Si necesita crear un método público en una clase de controlador y no desea exponer el método como una acción de controlador, a continuación, puede impedir que el método que se invoca mediante el uso de la &lt;NonAction&gt; atributo.</span><span class="sxs-lookup"><span data-stu-id="d6f70-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="d6f70-133">Por ejemplo, el controlador en el listado 2 contiene un método público denominado CompanySecrets() decorada con el &lt;NonAction&gt; atributo.</span><span class="sxs-lookup"><span data-stu-id="d6f70-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="d6f70-134">**Listing 2 - Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6f70-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="d6f70-135">Si se intenta invocar la acción del controlador CompanySecrets() escribiendo /Work/CompanySecrets en la barra de direcciones del explorador, a continuación, obtendrá el mensaje de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="d6f70-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="d6f70-136">[![Invocar un método NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6f70-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="d6f70-137">**Figura 01**: Invocar un método NonAction ([haga clic aquí para ver imagen en tamaño completo](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d6f70-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6f70-138">[Anterior](creating-a-controller-vb.md)
> [Siguiente](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d6f70-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
