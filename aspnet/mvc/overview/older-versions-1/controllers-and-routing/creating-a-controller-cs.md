---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Creación de un controlador (C#) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther demuestra cómo puede agregar un controlador a una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fe880775a5d477aefcf0fe6cedb3e719760ed90
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036492"
---
<a name="creating-a-controller-c"></a><span data-ttu-id="1b804-103">Crear un controlador (C#)</span><span class="sxs-lookup"><span data-stu-id="1b804-103">Creating a Controller (C#)</span></span>
====================
<span data-ttu-id="1b804-104">by [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="1b804-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="1b804-105">En este tutorial, Stephen Walther demuestra cómo puede agregar un controlador a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1b804-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="1b804-106">El objetivo de este tutorial es explicar cómo se puede crear controladores de MVC de ASP.NET nuevo.</span><span class="sxs-lookup"><span data-stu-id="1b804-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="1b804-107">Aprenda a crear controladores mediante la opción de menú Agregar controlador de Visual Studio y mediante la creación de un archivo de clase a mano.</span><span class="sxs-lookup"><span data-stu-id="1b804-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="1b804-108">Mediante el agregar la opción de menú del controlador</span><span class="sxs-lookup"><span data-stu-id="1b804-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="1b804-109">Es la manera más fácil de crear un nuevo controlador secundario en la carpeta controladores en la ventana Explorador de soluciones de Visual Studio y seleccione el **Add, controlador** opción de menú (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="1b804-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="1b804-110">Al seleccionar esta opción de menú se abre el **Agregar controlador** cuadro de diálogo (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="1b804-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="1b804-111">[![El cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1b804-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="1b804-112">**Figura 01**: Agregar un nuevo controlador ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="1b804-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>


<span data-ttu-id="1b804-113">[![El cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1b804-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="1b804-114">**Figura 02**: El cuadro de diálogo Agregar controlador ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="1b804-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>


<span data-ttu-id="1b804-115">Tenga en cuenta que la primera parte del nombre del controlador se resalta en el **Agregar controlador** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="1b804-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="1b804-116">Cada nombre de controlador debe finalizar con el sufijo *controlador*.</span><span class="sxs-lookup"><span data-stu-id="1b804-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="1b804-117">Por ejemplo, puede crear un controlador denominado *ProductController* pero no un controlador denominado *producto*.</span><span class="sxs-lookup"><span data-stu-id="1b804-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="1b804-118">Si crea un controlador que falta el *controlador* sufijo, a continuación, no podrá invocar el controlador.</span><span class="sxs-lookup"><span data-stu-id="1b804-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="1b804-119">No hace esto, he en vano incontables horas de mi vida después cometer este error.</span><span class="sxs-lookup"><span data-stu-id="1b804-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="1b804-120">**Listado 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="1b804-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="1b804-121">Siempre debe crear los controladores en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="1b804-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="1b804-122">En caso contrario, deberá se infringen las convenciones de ASP.NET MVC y otros desarrolladores tendrán un tiempo más difícil comprender la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1b804-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="1b804-123">Métodos de acción de scaffolding</span><span class="sxs-lookup"><span data-stu-id="1b804-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="1b804-124">Cuando se crea un controlador, tiene la opción para generar automáticamente los métodos de acción Create, Update y los detalles (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="1b804-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="1b804-125">Si selecciona esta opción, se genera la clase de controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="1b804-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="1b804-126">[![Creación automática de los métodos de acción](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1b804-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="1b804-127">**Figura 03**: Creación automática de los métodos de acción ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1b804-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>


<span data-ttu-id="1b804-128">**Listado 2 - Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="1b804-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="1b804-129">Estos métodos generados son los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="1b804-129">These generated methods are stub methods.</span></span> <span data-ttu-id="1b804-130">Debe agregar la lógica real para crear, actualizar y mostrar los detalles de un cliente.</span><span class="sxs-lookup"><span data-stu-id="1b804-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="1b804-131">Sin embargo, los métodos de código auxiliar proporcionan un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="1b804-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="1b804-132">Creación de una clase de controlador</span><span class="sxs-lookup"><span data-stu-id="1b804-132">Creating a Controller Class</span></span>

<span data-ttu-id="1b804-133">El controlador de MVC de ASP.NET es simplemente una clase.</span><span class="sxs-lookup"><span data-stu-id="1b804-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="1b804-134">Si lo prefiere, puede omitir el scaffolding de controlador adecuado de Visual Studio y crear una clase de controlador a mano.</span><span class="sxs-lookup"><span data-stu-id="1b804-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="1b804-135">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="1b804-135">Follow these steps:</span></span>

1. <span data-ttu-id="1b804-136">Haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, nuevo elemento** y seleccione el **clase** plantilla (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="1b804-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="1b804-137">Nombre de la nueva clase PersonController.cs y haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="1b804-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="1b804-138">Modifique el archivo resultante de la clase para que la clase hereda de la clase base System.Web.Mvc.Controller (consulte el listado 3).</span><span class="sxs-lookup"><span data-stu-id="1b804-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="1b804-139">[![Crear una nueva clase](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1b804-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="1b804-140">**Figura 04**: Crear una nueva clase ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="1b804-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>


<span data-ttu-id="1b804-141">**Listing 3 - Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="1b804-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="1b804-142">El controlador en el listado 3 expone una acción denominada Index() que devuelve la cadena "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="1b804-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="1b804-143">Puede invocar esta acción de controlador mediante la ejecución de la aplicación y solicitar una dirección URL similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1b804-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="1b804-144">El servidor de desarrollo de ASP.NET utiliza un número de puerto aleatorio (por ejemplo, 40071).</span><span class="sxs-lookup"><span data-stu-id="1b804-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="1b804-145">Cuando escriba una dirección URL para invocar un controlador, deberá proporcionar el número de puerto de la derecha.</span><span class="sxs-lookup"><span data-stu-id="1b804-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="1b804-146">Puede determinar el número de puerto desplazando el puntero del mouse sobre el icono para el servidor de desarrollo de ASP.NET en el área de notificación de Windows (esquina inferior derecha de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="1b804-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="1b804-147">[Anterior](adding-dynamic-content-to-a-cached-page-cs.md)
> [Siguiente](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1b804-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
