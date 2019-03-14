---
title: Empezar a trabajar con componentes de Razor
author: guardrex
description: Obtenga información sobre cómo empezar a trabajar con componentes de Razor mediante la creación y modificación de un proyecto de componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036392"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="6e012-103">Empezar a trabajar con componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="6e012-103">Get started with Razor Components</span></span>

<span data-ttu-id="6e012-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6e012-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e012-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e012-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e012-106">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="6e012-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="6e012-107">Para crear su primer proyecto de componentes de Razor en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6e012-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="6e012-108">Seleccione **archivo** > **nuevo proyecto** > **Web** > **aplicación Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6e012-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="6e012-109">Asegúrese de que **.NET Core** y **ASP.NET Core 3.0** están seleccionados en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="6e012-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="6e012-110">Elija la **Razor componentes** plantilla y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6e012-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Cuadro de diálogo nueva aplicación](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="6e012-112">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e012-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="6e012-113">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="6e012-113">Congratulations!</span></span> <span data-ttu-id="6e012-114">Acaba de ejecutar su primera aplicación de los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="6e012-114">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6e012-115">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="6e012-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="6e012-116">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="6e012-116">Prerequisites:</span></span>

* [<span data-ttu-id="6e012-117">Obtener una vista previa de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="6e012-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="6e012-118">Para crear su primer proyecto de Razor componentes desde un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="6e012-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="6e012-119">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6e012-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="6e012-120">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="6e012-120">Congratulations!</span></span> <span data-ttu-id="6e012-121">Acaba de ejecutar su primera aplicación de los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="6e012-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="6e012-122">Proyecto de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="6e012-122">Razor Components project</span></span>

<span data-ttu-id="6e012-123">La solución creada por la plantilla de Razor componentes contiene dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="6e012-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="6e012-124">*WebApplication1.Server* &ndash; el proyecto de servidor es un proyecto de ASP.NET Core configurado para hospedar la aplicación de los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="6e012-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="6e012-125">*WebApplication1.App* &ndash; el proyecto de interfaz de usuario web del lado cliente que usa componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="6e012-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="6e012-126">La lógica de la interfaz de usuario en el *WebApplication1.App* proyecto está separado del resto de la aplicación debido a una limitación técnica en ASP.NET Core 3.0 Preview 2.</span><span class="sxs-lookup"><span data-stu-id="6e012-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="6e012-127">La extensión de archivo Razor (*.cshtml*) utilizado para los componentes de Razor se usa también para las vistas de páginas de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="6e012-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="6e012-128">Actualmente, los componentes de Razor y MVC o páginas de Razor tienen modelos de compilación diferentes, por lo que los archivos de Razor de componentes de Razor se mantienen separados.</span><span class="sxs-lookup"><span data-stu-id="6e012-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="6e012-129">En una vista previa futura, tenemos previsto introducir una nueva extensión de archivo para los componentes de Razor (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="6e012-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="6e012-130">Componentes, las páginas y las vistas se hospedará *en el mismo proyecto*.</span><span class="sxs-lookup"><span data-stu-id="6e012-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="6e012-131">Cuando se ejecuta la aplicación, están disponibles las fichas en la barra lateral de varias páginas:</span><span class="sxs-lookup"><span data-stu-id="6e012-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="6e012-132">Página principal</span><span class="sxs-lookup"><span data-stu-id="6e012-132">Home</span></span>
* <span data-ttu-id="6e012-133">Contador</span><span class="sxs-lookup"><span data-stu-id="6e012-133">Counter</span></span>
* <span data-ttu-id="6e012-134">Recuperar datos</span><span class="sxs-lookup"><span data-stu-id="6e012-134">Fetch data</span></span>

<span data-ttu-id="6e012-135">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="6e012-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6e012-136">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Razor Components proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="6e012-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="6e012-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6e012-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="6e012-138">Una solicitud para `/counter` en el explorador, según lo especificado por el `@page` directiva en la parte superior, hace que el componente de contador representar su contenido.</span><span class="sxs-lookup"><span data-stu-id="6e012-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="6e012-139">Los componentes se representan en una representación en memoria del árbol de representación que puede usarse para actualizar la interfaz de usuario de una manera flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="6e012-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="6e012-140">Cada vez que el **Click me** está seleccionado:</span><span class="sxs-lookup"><span data-stu-id="6e012-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="6e012-141">El `onclick` desencadena el evento.</span><span class="sxs-lookup"><span data-stu-id="6e012-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="6e012-142">Se llama al método `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="6e012-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="6e012-143">El `currentCount` se incrementa.</span><span class="sxs-lookup"><span data-stu-id="6e012-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="6e012-144">El componente se representa de nuevo.</span><span class="sxs-lookup"><span data-stu-id="6e012-144">The component is rendered again.</span></span>

<span data-ttu-id="6e012-145">El tiempo de ejecución compara el contenido nuevo para el contenido anterior y solo aplica el contenido cambiado a Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="6e012-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="6e012-146">Agregar un componente a otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="6e012-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="6e012-147">Parámetros del componente se especifican mediante atributos o contenido secundario.</span><span class="sxs-lookup"><span data-stu-id="6e012-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="6e012-148">Por ejemplo, un componente de contador se puede agregar a la página principal de la aplicación agregando un `<Counter />` elemento para el componente del índice.</span><span class="sxs-lookup"><span data-stu-id="6e012-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="6e012-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6e012-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="6e012-150">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e012-150">Run the app.</span></span> <span data-ttu-id="6e012-151">La página principal tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="6e012-151">The homepage has its own counter.</span></span>

<span data-ttu-id="6e012-152">Para agregar un parámetro para el componente de contador, actualice el componente `@functions` bloque:</span><span class="sxs-lookup"><span data-stu-id="6e012-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="6e012-153">Agregue una propiedad para `IncrementAmount` decorada con el `[Parameter]` atributo.</span><span class="sxs-lookup"><span data-stu-id="6e012-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="6e012-154">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6e012-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="6e012-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6e012-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="6e012-156">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="6e012-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="6e012-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6e012-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="6e012-158">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e012-158">Run the app.</span></span> <span data-ttu-id="6e012-159">La página principal tiene su propio contador que se incrementa en diez cada vez que el **Click me** botón está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="6e012-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e012-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="6e012-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
