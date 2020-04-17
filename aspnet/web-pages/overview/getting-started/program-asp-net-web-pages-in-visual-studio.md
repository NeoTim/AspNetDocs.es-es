---
uid: web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programación ASP.NET páginas Web (Razor) con Visual Studio ( Visual Studio) Microsoft Docs
author: Rick-Anderson
description: En este apéndice se explica cómo puede usar Visual Studio 2010 o Visual Web Developer 2010 Express para programar ASP.NET páginas Web con la sintaxis de Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: e586a116fc48a797bdd40befad5851a548282ce6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542903"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="3e44a-103">Programación ASP.NET páginas Web (Razor) con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e44a-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="3e44a-104"> por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3e44a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3e44a-105">En este artículo se explica cómo puede usar Visual Studio o Visual Web Developer Express para programar sitios web de ASP.NET páginas Web (Razor).</span><span class="sxs-lookup"><span data-stu-id="3e44a-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="3e44a-106">Temas que se abordarán</span><span class="sxs-lookup"><span data-stu-id="3e44a-106">What you'll learn</span></span>
>
> - <span data-ttu-id="3e44a-107">Lo que necesita instalar (si es que algo) para trabajar con ASP.NET páginas Web en su versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="3e44a-108">Cómo agregar compatibilidad con ASP.NET páginas web a Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="3e44a-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="3e44a-109">Cómo usar características en Visual Studio para trabajar con páginas ASP.NET Razor, incluido IntelliSense y el depurador.</span><span class="sxs-lookup"><span data-stu-id="3e44a-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3e44a-110">Versiones de software utilizadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3e44a-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="3e44a-111">ASP.NET Páginas Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3e44a-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="3e44a-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3e44a-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="3e44a-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="3e44a-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="3e44a-114">Este tutorial también funciona con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="3e44a-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="3e44a-115">Puede programar páginas Web ASP.NET con sintaxis Razor mediante WebMatrix o muchos otros editores de código.</span><span class="sxs-lookup"><span data-stu-id="3e44a-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="3e44a-116">También puede usar Microsoft Visual Studio, que es un entorno de desarrollo integrado (IDE) con todas las funciones que proporciona un potente conjunto de herramientas para crear muchos tipos de aplicaciones (no solo sitios web).</span><span class="sxs-lookup"><span data-stu-id="3e44a-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="3e44a-117">Para trabajar con páginas de ASP.NET Razor, puede usar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="3e44a-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="3e44a-118">Dos características particularmente útiles que Visual Studio proporciona para la programación con ASP.NET páginas web de Razor son:</span><span class="sxs-lookup"><span data-stu-id="3e44a-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="3e44a-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="3e44a-119">*IntelliSense*.</span></span> <span data-ttu-id="3e44a-120">La característica IntelliSense integrada en Visual Studio es más completa que IntelliSense en WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3e44a-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="3e44a-121">*Depurador*.</span><span class="sxs-lookup"><span data-stu-id="3e44a-121">*Debugger*.</span></span> <span data-ttu-id="3e44a-122">El depurador le permite solucionar problemas del código deteniendo un programa mientras se está ejecutando, examinando variables y recorriendo el código línea por línea.</span><span class="sxs-lookup"><span data-stu-id="3e44a-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="3e44a-123">Uso de Visual Studio con diferentes versiones de ASP.NET páginas Web</span><span class="sxs-lookup"><span data-stu-id="3e44a-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="3e44a-124">Para desarrollar ASP.NET aplicaciones web en Visual Studio 2017, instale la carga de trabajo de **desarrollo web y ASP.NET.**</span><span class="sxs-lookup"><span data-stu-id="3e44a-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="3e44a-125">Visual Studio 2012 y Visual Studio 2013 incluyen compatibilidad con ASP.NET páginas Web.</span><span class="sxs-lookup"><span data-stu-id="3e44a-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="3e44a-126">(Los paquetes necesarios para admitir ASP.NET páginas Web se instalan al instalar Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="3e44a-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="3e44a-127">Visual Studio 2010 no incluye compatibilidad de forma predeterminada para ASP.NET páginas Web.</span><span class="sxs-lookup"><span data-stu-id="3e44a-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="3e44a-128">Para usar ASP.NET páginas Web con Visual Studio 2010, debe instalar el paquete ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3e44a-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="3e44a-129">Para obtener ASP.NET páginas Web 2, instale ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3e44a-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="3e44a-130">En la tabla siguiente se resume la compatibilidad con ASP.NET páginas Web en diferentes versiones de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="3e44a-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3e44a-131">Visual Studio 2010</span></span> | <span data-ttu-id="3e44a-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3e44a-132">Visual Studio 2012</span></span> | <span data-ttu-id="3e44a-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3e44a-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e44a-134">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="3e44a-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="3e44a-135">Instalar ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3e44a-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="3e44a-136">(Incluido)</span><span class="sxs-lookup"><span data-stu-id="3e44a-136">(Included)</span></span> | <span data-ttu-id="3e44a-137">(Incluido)</span><span class="sxs-lookup"><span data-stu-id="3e44a-137">(Included)</span></span> |
| <span data-ttu-id="3e44a-138">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="3e44a-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="3e44a-139">Actualización a ASP.NET páginas Web 3 a través de NuGet</span><span class="sxs-lookup"><span data-stu-id="3e44a-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="3e44a-140">(Incluido)</span><span class="sxs-lookup"><span data-stu-id="3e44a-140">(Included)</span></span> |

<span data-ttu-id="3e44a-141">Para trabajar con Visual Studio 2010, vea [Instalación de compatibilidad con ASP.NET páginas Web en Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="3e44a-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="3e44a-142">Iniciar Visual Studio desde WebMatrix</span><span class="sxs-lookup"><span data-stu-id="3e44a-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="3e44a-143">Si ha iniciado un proyecto en WebMatrix y desea cambiar a Visual Studio, WebMatrix proporciona un botón para abrir fácilmente el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="3e44a-144">Debe tener Visual Studio instalado en el equipo para que este botón se habilite.</span><span class="sxs-lookup"><span data-stu-id="3e44a-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="3e44a-145">La siguiente imagen muestra el botón en WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3e44a-145">The following image shows the button in WebMatrix.</span></span>

![iniciar Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="3e44a-147">Al hacer clic en el botón, el proyecto se abre en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="3e44a-148">Puede alternar entre WebMatrix y Visual Studio sin ningún problema.</span><span class="sxs-lookup"><span data-stu-id="3e44a-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="3e44a-149">Se le notificará si algún archivo ha cambiado en el otro entorno y necesita volver a cargarse para obtener los últimos cambios.</span><span class="sxs-lookup"><span data-stu-id="3e44a-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="3e44a-150">Creación de ASP.NET sitio de Razor en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e44a-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="3e44a-151">Para crear un sitio web de ASP.NET Razor en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3e44a-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="3e44a-152">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="3e44a-153">En el menú **Archivo** , haga clic en **Nuevo sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="3e44a-153">In the **File** menu, click **New Web Site**.</span></span>

    ![crear un nuevo sitio web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="3e44a-155">En el cuadro de diálogo **Nuevo sitio Web,** seleccione el idioma que desea usar (Visual C o Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="3e44a-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="3e44a-156">Seleccione la plantilla **ASP.NET sitio web (Razor).**</span><span class="sxs-lookup"><span data-stu-id="3e44a-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![sitio de afeitar](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="3e44a-158">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3e44a-158">Click **OK**.</span></span>

<span data-ttu-id="3e44a-159">El nuevo proyecto existe y se rellena con algunas páginas web predeterminadas para ayudarle a empezar.</span><span class="sxs-lookup"><span data-stu-id="3e44a-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="3e44a-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="3e44a-160">Using IntelliSense</span></span>

<span data-ttu-id="3e44a-161">Ahora que ha creado un sitio, puede ver cómo funciona IntelliSense en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="3e44a-162">En el sitio web que acaba de crear, abra el *Default.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="3e44a-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="3e44a-163">Después `<h3>` de las etiquetas `@ServerInfo.` de la página, escriba (incluido el punto).</span><span class="sxs-lookup"><span data-stu-id="3e44a-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="3e44a-164">Observe cómo IntelliSense muestra los `ServerInfo` métodos disponibles para la aplicación auxiliar en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="3e44a-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="3e44a-166">Seleccione `GetHtml` el método de la lista y, a continuación, pulse Intro.</span><span class="sxs-lookup"><span data-stu-id="3e44a-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="3e44a-167">IntelliSense rellena automáticamente el método.</span><span class="sxs-lookup"><span data-stu-id="3e44a-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="3e44a-168">(Al igual que con cualquier método `()` en C-, debe agregar caracteres después del método.) El código completado `GetHtml` para el método es similar al siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3e44a-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="3e44a-169">Presione Ctrl+F5 para ejecutar la página.</span><span class="sxs-lookup"><span data-stu-id="3e44a-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="3e44a-170">Esto es lo que la página se ve como cuando se muestra en un navegador:</span><span class="sxs-lookup"><span data-stu-id="3e44a-170">This is what the page looks like when displayed in a browser:</span></span>

    ![página predeterminada en el navegador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="3e44a-172">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="3e44a-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="3e44a-173">Uso del depurador</span><span class="sxs-lookup"><span data-stu-id="3e44a-173">Using the Debugger</span></span>

1. <span data-ttu-id="3e44a-174">En la parte superior de la *Página Default.cshtml,* después de la línea que comienza con `Page.Title`, agregue la siguiente línea de código:</span><span class="sxs-lookup"><span data-stu-id="3e44a-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="3e44a-175">En el margen gris del editor a la izquierda del código, haga clic al lado de esta nueva línea para agregar un punto de *interrupción.*</span><span class="sxs-lookup"><span data-stu-id="3e44a-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="3e44a-176">Un punto de interrupción es un marcador que indica al depurador que deje de ejecutar el programa en ese momento para que pueda ver lo que está sucediendo.</span><span class="sxs-lookup"><span data-stu-id="3e44a-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![establecer punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="3e44a-178">Quite la llamada `ServerInfo.GetHtml` al método y agregue `@myTime` una llamada a la variable en su lugar.</span><span class="sxs-lookup"><span data-stu-id="3e44a-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="3e44a-179">Esta llamada muestra el valor de hora actual devuelto por la nueva línea de código.</span><span class="sxs-lookup"><span data-stu-id="3e44a-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="3e44a-180">Presione F5 para ejecutar la página en el depurador.</span><span class="sxs-lookup"><span data-stu-id="3e44a-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="3e44a-181">La página se detiene en el punto de interrupción establecido.</span><span class="sxs-lookup"><span data-stu-id="3e44a-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="3e44a-182">La siguiente imagen muestra el aspecto de la página en el editor con el punto de interrupción (en amarillo).</span><span class="sxs-lookup"><span data-stu-id="3e44a-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![punto de interrupción de depuración](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="3e44a-184">En la barra de herramientas Depurar, haga clic en el botón **Paso** a paso (o presione F11) para ejecutar la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="3e44a-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="3e44a-185">Cada vez que haga clic en este botón, avance la ejecución a la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="3e44a-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Entrar en el botón](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="3e44a-187">Examine el valor `myTime` de la variable manteniendo el puntero del mouse sobre ella o inspeccionando los valores que se muestran en las **ventanas Locales** y Pila de **llamadas.**</span><span class="sxs-lookup"><span data-stu-id="3e44a-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="3e44a-188">Visual Studio muestra el valor de la variable.</span><span class="sxs-lookup"><span data-stu-id="3e44a-188">Visual Studio display the value of the variable.</span></span>

    ![mostrar el valor de la hora](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="3e44a-190">Cuando haya terminado de examinar la variable y recorrer el código, presione F5 para continuar ejecutando la página sin detenerse en cada línea.</span><span class="sxs-lookup"><span data-stu-id="3e44a-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="3e44a-191">Cuando haya terminado de recorrer todo el código, el explorador mostrará la página.</span><span class="sxs-lookup"><span data-stu-id="3e44a-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="3e44a-192">Para obtener más información sobre el depurador y cómo depurar código en Visual Studio, vea [Tutorial: depurar páginas web en Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e44a-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="3e44a-193">Uso de Razor en proyectos MVC ASP.NET con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e44a-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="3e44a-194">La sintaxis de Razor también se utiliza ampliamente en ASP.NET proyectos MVC.</span><span class="sxs-lookup"><span data-stu-id="3e44a-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="3e44a-195">MVC es una forma eficaz y basada en patrones para crear sitios web dinámicos.</span><span class="sxs-lookup"><span data-stu-id="3e44a-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="3e44a-196">Si el sitio de páginas Web de ASP.NET se vuelve difícil de mantener, es posible que desee considerar la posibilidad de convertirlo en una aplicación MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3e44a-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="3e44a-197">Para obtener un ejemplo de creación de una aplicación MVC, vea [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="3e44a-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="3e44a-198">Instalación de compatibilidad con páginas Web ASP.NET en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3e44a-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="3e44a-199">En esta sección se muestra cómo instalar Visual Web Developer Express 2010 y las herramientas de páginas web de ASP.NET para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e44a-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="3e44a-200">Si aún no tiene el Instalador de plataforma web, descárguelo desde la siguiente DIRECCIÓN URL:</span><span class="sxs-lookup"><span data-stu-id="3e44a-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="3e44a-201">Ejecute el Instalador de plataforma web.</span><span class="sxs-lookup"><span data-stu-id="3e44a-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="3e44a-202">Haga clic en la pestaña **Productos.**</span><span class="sxs-lookup"><span data-stu-id="3e44a-202">Click the **Products** tab.</span></span>

    ![Ficha Productos WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="3e44a-204">Busque **ASP.NET MVC 4** (para ASP.NET páginas Web 2) y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="3e44a-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="3e44a-205">Estos productos incluyen herramientas de Visual Studio para crear sitios web de ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="3e44a-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opciones de instalación de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="3e44a-207">Haga clic en **Instalar** para completar la instalación.</span><span class="sxs-lookup"><span data-stu-id="3e44a-207">Click **Install** to complete the installation.</span></span>
