---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Usar Signalr con Web Apps en Azure App Service | Microsoft Docs
author: bradygaster
description: En este documento se describe cómo configurar una aplicación Signalr que se ejecuta en Microsoft Azure. Versiones de software usadas en el tutorial Visual Studio 2013 o vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450187"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="de9a7-104">Usar SignalR con Web Apps en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de9a7-104">Using SignalR with Web Apps in Azure App Service</span></span>

<span data-ttu-id="de9a7-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="de9a7-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="de9a7-106">En este documento se describe cómo configurar una aplicación Signalr que se ejecuta en Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="de9a7-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="de9a7-107">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="de9a7-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="de9a7-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="de9a7-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="de9a7-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="de9a7-109">.NET 4.5</span></span>
> - <span data-ttu-id="de9a7-110">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="de9a7-110">SignalR version 2</span></span>
> - <span data-ttu-id="de9a7-111">Azure SDK 2,3 para Visual Studio 2013 o 2012</span><span class="sxs-lookup"><span data-stu-id="de9a7-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="de9a7-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="de9a7-112">Questions and comments</span></span>
>
> <span data-ttu-id="de9a7-113">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="de9a7-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="de9a7-114">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), en [stackoverflow.com](http://stackoverflow.com/)o en los [foros de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="de9a7-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="de9a7-115">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="de9a7-115">Table of Contents</span></span>

- [<span data-ttu-id="de9a7-116">Introducción</span><span class="sxs-lookup"><span data-stu-id="de9a7-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="de9a7-117">Implementación de una aplicación Web de Signalr en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de9a7-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="de9a7-118">Habilitación de WebSockets en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de9a7-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="de9a7-119">Usar el backplane Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="de9a7-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="de9a7-120">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="de9a7-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="de9a7-121">Introducción</span><span class="sxs-lookup"><span data-stu-id="de9a7-121">Introduction</span></span>

<span data-ttu-id="de9a7-122">ASP.NET Signalr se puede usar para incorporar un nuevo nivel de interactividad entre los servidores y los clientes Web o de .NET.</span><span class="sxs-lookup"><span data-stu-id="de9a7-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="de9a7-123">Cuando se hospeda en Azure, las aplicaciones de Signalr pueden aprovechar el entorno de alta disponibilidad, escalable y de rendimiento que proporciona en la nube.</span><span class="sxs-lookup"><span data-stu-id="de9a7-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="de9a7-124">Implementación de una aplicación Web de Signalr en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de9a7-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="de9a7-125">Signalr no agrega complicaciones particulares a la implementación de una aplicación en Azure frente a la implementación en un servidor local.</span><span class="sxs-lookup"><span data-stu-id="de9a7-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="de9a7-126">Una aplicación que usa Signalr se puede hospedar en Azure sin realizar cambios en la configuración ni en otras opciones (aunque para la compatibilidad con WebSockets, consulte [habilitación de WebSockets en Azure App Service](#websocket) a continuación). En este tutorial, implementará la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) en Azure.</span><span class="sxs-lookup"><span data-stu-id="de9a7-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="de9a7-127">**Requisitos previos**</span><span class="sxs-lookup"><span data-stu-id="de9a7-127">**Prerequisites**</span></span>

- <span data-ttu-id="de9a7-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="de9a7-128">Visual Studio 2013.</span></span> <span data-ttu-id="de9a7-129">Si no tiene Visual Studio, Visual Studio 2013 Express para web se incluye en la instalación del SDK de Azure.</span><span class="sxs-lookup"><span data-stu-id="de9a7-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="de9a7-130">[Azure sdk 2,3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [Azure SDK 2,3 para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="de9a7-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="de9a7-131">Para completar este tutorial, deberá tener una suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="de9a7-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="de9a7-132">Puede [activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)o [registrarse para obtener una suscripción de prueba](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="de9a7-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="de9a7-133">Implementación de una aplicación Web de Signalr en Azure</span><span class="sxs-lookup"><span data-stu-id="de9a7-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="de9a7-134">Complete el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md)o descargue el proyecto terminado de la [Galería de código](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="de9a7-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="de9a7-135">En Visual Studio, seleccione **compilar**, **publicar chat de signalr**.</span><span class="sxs-lookup"><span data-stu-id="de9a7-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="de9a7-136">En el cuadro de diálogo "publicar web", seleccione "sitios web de Windows Azure".</span><span class="sxs-lookup"><span data-stu-id="de9a7-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Seleccionar sitios web de Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="de9a7-138">Si no ha iniciado sesión en el cuenta de Microsoft, haga clic en **iniciar sesión...** en el cuadro de diálogo "seleccionar sitio web existente" e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="de9a7-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Seleccionar sitio web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Iniciar sesión en Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="de9a7-141">En el cuadro de diálogo "seleccionar sitio web existente", haga clic en **nuevo**.</span><span class="sxs-lookup"><span data-stu-id="de9a7-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nuevo sitio web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="de9a7-143">En el cuadro de diálogo "crear sitio en Windows Azure", escriba un nombre de aplicación único.</span><span class="sxs-lookup"><span data-stu-id="de9a7-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="de9a7-144">Seleccione la región más cercana a usted en la lista desplegable región.</span><span class="sxs-lookup"><span data-stu-id="de9a7-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="de9a7-145">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="de9a7-145">Click **Create**.</span></span>

    ![Creación de sitio en Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="de9a7-147">En el cuadro de diálogo "publicar web", haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="de9a7-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publicar sitio](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="de9a7-149">Cuando la aplicación haya completado la publicación, la aplicación Signalr chat hospedada en Azure App Service Web Apps se abrirá en un explorador.</span><span class="sxs-lookup"><span data-stu-id="de9a7-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Abrir el sitio en un explorador](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="de9a7-151">Habilitación de WebSockets en Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="de9a7-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="de9a7-152">WebSockets debe habilitarse explícitamente en la aplicación web para usarse en una aplicación de Signalr. de lo contrario, se usarán otros protocolos (consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="de9a7-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="de9a7-153">Para usar WebSockets en Azure App Service Web Apps, habilítelo en la sección configuración de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="de9a7-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="de9a7-154">Para ello, abra la aplicación web en [Azure portal de administración](https://manage.windowsazure.com/)y seleccione Configurar.</span><span class="sxs-lookup"><span data-stu-id="de9a7-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Pestaña Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="de9a7-156">En la parte superior de la página Configuración, asegúrese de que se usa .NET 4,5 para la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="de9a7-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Configuración de la versión 4,5 de .NET Framework](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="de9a7-158">En la página Configuración, en la configuración de **WebSockets** , seleccione **activado**.</span><span class="sxs-lookup"><span data-stu-id="de9a7-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Configuración de WebSockets: activado](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="de9a7-160">En la parte inferior de la página Configuración, seleccione **Guardar** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="de9a7-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Guardar configuración](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="de9a7-162">Usar el backplane Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="de9a7-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="de9a7-163">Si usa varias instancias para la aplicación web y los usuarios de esas instancias necesitan interactuar entre sí (por ejemplo, los mensajes de chat creados en una instancia pueden llegar a los usuarios conectados a otras instancias), el [Azure Redis cache backplane](../performance/scaleout-with-redis.md) debe implementarse en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="de9a7-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="de9a7-164">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="de9a7-164">Next Steps</span></span>

<span data-ttu-id="de9a7-165">Para obtener más información sobre Web Apps en Azure App Service, consulte [información general sobre Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="de9a7-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
