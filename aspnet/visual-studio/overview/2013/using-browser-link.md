---
uid: visual-studio/overview/2013/using-browser-link
title: Mediante el vínculo de explorador en Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395100"
---
# <a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="05455-102">Mediante el vínculo de explorador en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="05455-102">Using Browser Link in Visual Studio 2013</span></span>

<span data-ttu-id="05455-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="05455-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="05455-104">Vínculo de explorador es una característica nueva en Visual Studio 2013 que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web.</span><span class="sxs-lookup"><span data-stu-id="05455-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="05455-105">Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, que es útil para probar varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="05455-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="05455-106">Actualizar del explorador</span><span class="sxs-lookup"><span data-stu-id="05455-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="05455-107">Ver el panel de vínculos de explorador</span><span class="sxs-lookup"><span data-stu-id="05455-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="05455-108">Habilitar vínculo con exploradores para archivos HTML estáticos</span><span class="sxs-lookup"><span data-stu-id="05455-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="05455-109">Deshabilitar el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="05455-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="05455-110">¿Cómo funciona?</span><span class="sxs-lookup"><span data-stu-id="05455-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="05455-111">Actualizar del explorador</span><span class="sxs-lookup"><span data-stu-id="05455-111">Browser Refresh</span></span>

<span data-ttu-id="05455-112">Con sólo actualizar del explorador, puede actualizar varios exploradores que están conectados a Visual Studio a través del vínculo de explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="05455-113">Para usar actualizar del explorador, cree primero una aplicación de ASP.NET, mediante cualquiera de las plantillas de proyecto.</span><span class="sxs-lookup"><span data-stu-id="05455-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="05455-114">Depurar la aplicación presionando F5 o haga clic en el icono de flecha en la barra de herramientas:</span><span class="sxs-lookup"><span data-stu-id="05455-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="05455-115">También puede usar la lista desplegable para seleccionar un explorador específico para la depuración.</span><span class="sxs-lookup"><span data-stu-id="05455-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="05455-116">Para depurar con varios exploradores, seleccione **explorar con**.</span><span class="sxs-lookup"><span data-stu-id="05455-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="05455-117">En el **explorar con** cuadro de diálogo, mantenga presionada la tecla CTRL para seleccionar más de un explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="05455-118">Haga clic en **examinar** para depurar con los exploradores seleccionados.</span><span class="sxs-lookup"><span data-stu-id="05455-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="05455-119">Vínculo de explorador también funciona si se ejecutará un explorador desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05455-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="05455-120">Los controles de vínculo de explorador se encuentran en la lista desplegable con el icono de flecha circular.</span><span class="sxs-lookup"><span data-stu-id="05455-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="05455-121">El icono de flecha es el **actualizar** botón.</span><span class="sxs-lookup"><span data-stu-id="05455-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="05455-122">Para ver qué exploradores se conectan, mantenga el mouse sobre el **actualizar** botón durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="05455-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="05455-123">Los exploradores conectados se muestran en una ventana de información sobre herramientas.</span><span class="sxs-lookup"><span data-stu-id="05455-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="05455-124">Para actualizar los exploradores conectados, haga clic en el **actualizar** botón o presione CTRL + ALT + ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="05455-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="05455-125">Por ejemplo, la captura de pantalla siguiente muestra un proyecto ASP.NET, que he creado mediante la plantilla de proyecto de MVC 5.</span><span class="sxs-lookup"><span data-stu-id="05455-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="05455-126">Puede ver la aplicación en ejecución en los dos exploradores en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="05455-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="05455-127">En la parte inferior, el proyecto está abierto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05455-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="05455-128">En Visual Studio, he cambiado el &lt;h1&gt; encabezado de la página principal:</span><span class="sxs-lookup"><span data-stu-id="05455-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="05455-129">Cuando hace clic en el **actualizar** button, el cambio aparece en dos ventanas del explorador:</span><span class="sxs-lookup"><span data-stu-id="05455-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

**<span data-ttu-id="05455-130">Notas</span><span class="sxs-lookup"><span data-stu-id="05455-130">Notes</span></span>**

- <span data-ttu-id="05455-131">Para habilitar el vínculo de explorador, establezca `debug=true` en el [ &lt;compilación&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elemento en el archivo Web.config del proyecto.</span><span class="sxs-lookup"><span data-stu-id="05455-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="05455-132">La aplicación debe estar ejecutando en el host local.</span><span class="sxs-lookup"><span data-stu-id="05455-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="05455-133">La aplicación debe tener como destino .NET 4.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="05455-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="05455-134">Ver el panel de vínculos de explorador</span><span class="sxs-lookup"><span data-stu-id="05455-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="05455-135">El panel de vínculos de explorador muestra información sobre las conexiones de vínculo de explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="05455-136">Para ver el panel, seleccione el menú desplegable de vínculo de explorador (la flecha pequeña situada junto a la **actualizar** botón).</span><span class="sxs-lookup"><span data-stu-id="05455-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="05455-137">A continuación, haga clic en **panel vínculo de explorador**.</span><span class="sxs-lookup"><span data-stu-id="05455-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="05455-138">El panel enumera los exploradores conectados y la dirección URL a la que ha navegado cada explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="05455-139">El **requisitos previos** sección muestra los pasos necesarios para habilitar el vínculo de explorador para ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="05455-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="05455-140">Por ejemplo, la captura de pantalla siguiente muestra un proyecto donde "debug" se establece en false en el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="05455-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="05455-141">Habilitar vínculo con exploradores para archivos HTML estáticos</span><span class="sxs-lookup"><span data-stu-id="05455-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="05455-142">Para habilitar el vínculo de explorador para archivos HTML estáticos, agregue lo siguiente al archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="05455-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="05455-143">Por motivos de rendimiento, quite esta configuración al publicar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="05455-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="05455-144">Deshabilitar el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="05455-144">Disabling Browser Link</span></span>

<span data-ttu-id="05455-145">Vínculo de explorador está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="05455-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="05455-146">Hay varias maneras para deshabilitarlo:</span><span class="sxs-lookup"><span data-stu-id="05455-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="05455-147">En el menú desplegable de vínculo de explorador, desactive la opción **habilitar Browser Link**.</span><span class="sxs-lookup"><span data-stu-id="05455-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="05455-148">En el archivo Web.config, agregue una clave denominada "vs: EnableBrowserLink" con el valor "false" en la sección appSettings.</span><span class="sxs-lookup"><span data-stu-id="05455-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="05455-149">En el archivo Web.config, establezca la depuración en false.</span><span class="sxs-lookup"><span data-stu-id="05455-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="05455-150">¿Cómo funciona?</span><span class="sxs-lookup"><span data-stu-id="05455-150">How Does It Work?</span></span>

<span data-ttu-id="05455-151">Vínculo de explorador utiliza [SignalR](../../../signalr/index.md) para crear un canal de comunicación entre Visual Studio y el explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="05455-152">Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a.</span><span class="sxs-lookup"><span data-stu-id="05455-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="05455-153">Vínculo de explorador también registra un módulo HTTP con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="05455-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="05455-154">Este módulo inyecta especial &lt;script&gt; referencias en cada solicitud de página desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="05455-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="05455-155">Puede ver las referencias de script, seleccione "Ver origen" en el explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="05455-156">No se modifican los archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="05455-156">Your source files are not modified.</span></span> <span data-ttu-id="05455-157">El módulo HTTP inserta dinámicamente las referencias de script.</span><span class="sxs-lookup"><span data-stu-id="05455-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="05455-158">Dado que el código del lado del explorador es JavaScript todas, funciona en todos los exploradores que [SignalR admite](../../../signalr/overview/getting-started/supported-platforms.md), sin necesidad de cualquier complemento del explorador.</span><span class="sxs-lookup"><span data-stu-id="05455-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
