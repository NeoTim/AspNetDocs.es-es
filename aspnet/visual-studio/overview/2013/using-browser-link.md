---
uid: visual-studio/overview/2013/using-browser-link
title: Usar el vínculo del explorador en Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505207"
---
# <a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="61d50-102">Usar el vínculo del explorador en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="61d50-102">Using Browser Link in Visual Studio 2013</span></span>

<span data-ttu-id="61d50-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="61d50-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="61d50-104">El vínculo del explorador es una característica nueva de Visual Studio 2013 que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores Web.</span><span class="sxs-lookup"><span data-stu-id="61d50-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="61d50-105">Puede usar el vínculo del explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.</span><span class="sxs-lookup"><span data-stu-id="61d50-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="61d50-106">Actualización del explorador</span><span class="sxs-lookup"><span data-stu-id="61d50-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="61d50-107">Ver el panel de vínculos del explorador</span><span class="sxs-lookup"><span data-stu-id="61d50-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="61d50-108">Habilitación del vínculo del explorador para archivos HTML estáticos</span><span class="sxs-lookup"><span data-stu-id="61d50-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="61d50-109">Deshabilitando vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="61d50-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="61d50-110">¿Cómo funciona?</span><span class="sxs-lookup"><span data-stu-id="61d50-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="61d50-111">Actualización del explorador</span><span class="sxs-lookup"><span data-stu-id="61d50-111">Browser Refresh</span></span>

<span data-ttu-id="61d50-112">Con la actualización del explorador, puede actualizar varios exploradores que están conectados a Visual Studio a través del vínculo del explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="61d50-113">Para usar la actualización del explorador, cree primero una aplicación ASP.NET con cualquiera de las plantillas de proyecto.</span><span class="sxs-lookup"><span data-stu-id="61d50-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="61d50-114">Depure la aplicación presionando F5 o haciendo clic en el icono de flecha de la barra de herramientas:</span><span class="sxs-lookup"><span data-stu-id="61d50-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="61d50-115">También puede usar la lista desplegable para seleccionar un explorador específico para la depuración.</span><span class="sxs-lookup"><span data-stu-id="61d50-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="61d50-116">Para depurar con varios exploradores, seleccione **examinar con**.</span><span class="sxs-lookup"><span data-stu-id="61d50-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="61d50-117">En el cuadro de diálogo **examinar con** , mantenga presionada la tecla Ctrl para seleccionar más de un explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="61d50-118">Haga clic en **examinar** para depurar con los exploradores seleccionados.</span><span class="sxs-lookup"><span data-stu-id="61d50-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="61d50-119">El vínculo del explorador también funciona si inicia un explorador desde fuera de Visual Studio y navega a la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61d50-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="61d50-120">Los controles de vínculo del explorador se encuentran en la lista desplegable con el icono de flecha circular.</span><span class="sxs-lookup"><span data-stu-id="61d50-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="61d50-121">El icono de flecha es el botón **Actualizar** .</span><span class="sxs-lookup"><span data-stu-id="61d50-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="61d50-122">Para ver qué exploradores están conectados, mantenga el mouse sobre el botón de **actualización** durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="61d50-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="61d50-123">Los exploradores conectados se muestran en una ventana de información sobre herramientas.</span><span class="sxs-lookup"><span data-stu-id="61d50-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="61d50-124">Para actualizar los exploradores conectados, haga clic en el botón **Actualizar** o presione Ctrl + Alt + Entrar.</span><span class="sxs-lookup"><span data-stu-id="61d50-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="61d50-125">Por ejemplo, en la captura de pantalla siguiente se muestra un proyecto de ASP.NET, que se creó con la plantilla de proyecto de MVC 5.</span><span class="sxs-lookup"><span data-stu-id="61d50-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="61d50-126">Puede ver la aplicación que se ejecuta en dos exploradores en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="61d50-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="61d50-127">En la parte inferior, el proyecto está abierto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="61d50-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="61d50-128">En Visual Studio, cambio el encabezado &lt;H1&gt; de la Página principal:</span><span class="sxs-lookup"><span data-stu-id="61d50-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="61d50-129">Cuando hago clic en el botón **Actualizar** , el cambio apareció en las dos ventanas del explorador:</span><span class="sxs-lookup"><span data-stu-id="61d50-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="61d50-130">**Notas**</span><span class="sxs-lookup"><span data-stu-id="61d50-130">**Notes**</span></span>

- <span data-ttu-id="61d50-131">Para habilitar el vínculo del explorador, establezca `debug=true` en el elemento&gt;de la [compilación de&lt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) en el archivo Web. config del proyecto.</span><span class="sxs-lookup"><span data-stu-id="61d50-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="61d50-132">La aplicación debe ejecutarse en localhost.</span><span class="sxs-lookup"><span data-stu-id="61d50-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="61d50-133">La aplicación debe tener como destino .NET 4,0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="61d50-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="61d50-134">Ver el panel de vínculos del explorador</span><span class="sxs-lookup"><span data-stu-id="61d50-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="61d50-135">El panel vínculo del explorador muestra información acerca de las conexiones de vínculo del explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="61d50-136">Para ver el panel, seleccione el menú desplegable vínculo del explorador (la flecha pequeña situada junto al botón **Actualizar** ).</span><span class="sxs-lookup"><span data-stu-id="61d50-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="61d50-137">A continuación, haga clic en **Panel vínculo de explorador**.</span><span class="sxs-lookup"><span data-stu-id="61d50-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="61d50-138">En el panel se muestran los exploradores conectados y la dirección URL en la que se ha navegado por cada explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="61d50-139">En la sección de **requisitos previos** se muestran los pasos necesarios para habilitar el vínculo del explorador para ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="61d50-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="61d50-140">Por ejemplo, la captura de pantalla siguiente muestra un proyecto donde "debug" se establece en false en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="61d50-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="61d50-141">Habilitación del vínculo del explorador para archivos HTML estáticos</span><span class="sxs-lookup"><span data-stu-id="61d50-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="61d50-142">Para habilitar el vínculo del explorador para los archivos HTML estáticos, agregue lo siguiente al archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="61d50-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="61d50-143">Por motivos de rendimiento, quite esta configuración al publicar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="61d50-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="61d50-144">Deshabilitando vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="61d50-144">Disabling Browser Link</span></span>

<span data-ttu-id="61d50-145">El vínculo del explorador está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="61d50-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="61d50-146">Hay varias maneras de deshabilitarlo:</span><span class="sxs-lookup"><span data-stu-id="61d50-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="61d50-147">En el menú desplegable vínculo del explorador, desactive **Habilitar vínculo de explorador**.</span><span class="sxs-lookup"><span data-stu-id="61d50-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="61d50-148">En el archivo Web. config, agregue una clave denominada "vs: EnableBrowserLink" con el valor "false" en la sección appSettings.</span><span class="sxs-lookup"><span data-stu-id="61d50-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="61d50-149">En el archivo Web. config, establezca debug en false.</span><span class="sxs-lookup"><span data-stu-id="61d50-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="61d50-150">¿Cómo funciona?</span><span class="sxs-lookup"><span data-stu-id="61d50-150">How Does It Work?</span></span>

<span data-ttu-id="61d50-151">El vínculo del explorador usa [signalr](../../../signalr/index.md) para crear un canal de comunicación entre Visual Studio y el explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="61d50-152">Cuando el vínculo del explorador está habilitado, Visual Studio actúa como un servidor de Signalr al que se pueden conectar varios clientes (exploradores).</span><span class="sxs-lookup"><span data-stu-id="61d50-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="61d50-153">El vínculo del explorador también registra un módulo HTTP con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61d50-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="61d50-154">Este módulo inserta un script de &lt;especial&gt; referencias en cada solicitud de página del servidor.</span><span class="sxs-lookup"><span data-stu-id="61d50-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="61d50-155">Puede ver las referencias del script seleccionando "ver código fuente" en el explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="61d50-156">Los archivos de origen no se modifican.</span><span class="sxs-lookup"><span data-stu-id="61d50-156">Your source files are not modified.</span></span> <span data-ttu-id="61d50-157">El módulo HTTP inserta las referencias de script dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="61d50-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="61d50-158">Dado que el código del explorador es todo JavaScript, funciona en todos los exploradores [compatibles con signalr](../../../signalr/overview/getting-started/supported-platforms.md), sin necesidad de ningún complemento del explorador.</span><span class="sxs-lookup"><span data-stu-id="61d50-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
