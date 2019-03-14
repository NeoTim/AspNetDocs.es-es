---
title: Publicar un ASP.NET Core SignalR aplicación a aplicación Web de Azure
author: bradygaster
description: Publicar un ASP.NET Core SignalR aplicación a aplicación Web de Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57027432"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="763dd-103">Publicar un ASP.NET Core SignalR aplicación a una aplicación Web de Azure</span><span class="sxs-lookup"><span data-stu-id="763dd-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="763dd-104">[Azure Web App](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) plataforma de servicio para hospedar aplicaciones web, incluido ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="763dd-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="763dd-105">Este artículo se refiere a la publicación de una aplicación ASP.NET Core SignalR desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="763dd-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="763dd-106">Visite [SignalR service para Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) para obtener más información sobre el uso de SignalR en Azure.</span><span class="sxs-lookup"><span data-stu-id="763dd-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="763dd-107">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="763dd-107">Publish the app</span></span>

<span data-ttu-id="763dd-108">Visual Studio proporciona herramientas integradas para la publicación en una aplicación Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="763dd-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="763dd-109">Puede usar el usuario de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones en Azure.</span><span class="sxs-lookup"><span data-stu-id="763dd-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="763dd-110">Este artículo trata la publicación con las herramientas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="763dd-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="763dd-111">Para publicar una aplicación mediante la CLI de Azure, consulte [publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="763dd-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="763dd-112">Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="763dd-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="763dd-113">Confirme que **crear nuevo** está activada en la **elegir un destino de publicación** cuadro de diálogo y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="763dd-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Seleccionar destino de la publicación](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="763dd-115">Escriba la siguiente información en el **crear App Service** cuadro de diálogo y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="763dd-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="763dd-116">Elemento</span><span class="sxs-lookup"><span data-stu-id="763dd-116">Item</span></span> | <span data-ttu-id="763dd-117">Descripción</span><span class="sxs-lookup"><span data-stu-id="763dd-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="763dd-118">**Nombre de la aplicación**</span><span class="sxs-lookup"><span data-stu-id="763dd-118">**App name**</span></span> | <span data-ttu-id="763dd-119">Un nombre único de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="763dd-119">A unique name of the app.</span></span> |
| <span data-ttu-id="763dd-120">**Suscripción**</span><span class="sxs-lookup"><span data-stu-id="763dd-120">**Subscription**</span></span> | <span data-ttu-id="763dd-121">La suscripción de Azure que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="763dd-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="763dd-122">**Grupo de recursos**</span><span class="sxs-lookup"><span data-stu-id="763dd-122">**Resource Group**</span></span> | <span data-ttu-id="763dd-123">El grupo de recursos relacionados a la que pertenece la aplicación.</span><span class="sxs-lookup"><span data-stu-id="763dd-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="763dd-124">**Plan de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="763dd-124">**Hosting Plan**</span></span> | <span data-ttu-id="763dd-125">El plan de precios para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="763dd-125">The pricing plan for the web app.</span></span> |

![Crear servicio de aplicaciones](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="763dd-127">Visual Studio realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="763dd-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="763dd-128">Crea un perfil de publicación que contiene la configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="763dd-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="763dd-129">Crea o utiliza un archivo *Azure Web App* con los detalles proporcionados.</span><span class="sxs-lookup"><span data-stu-id="763dd-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="763dd-130">Publica la aplicación.</span><span class="sxs-lookup"><span data-stu-id="763dd-130">Publishes the app.</span></span>
* <span data-ttu-id="763dd-131">Inicia un explorador, con la aplicación web publicada cargada.</span><span class="sxs-lookup"><span data-stu-id="763dd-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="763dd-132">Tenga en cuenta el formato de la dirección URL de la aplicación se *{nombre de la aplicación}. azurewebsites NET*.</span><span class="sxs-lookup"><span data-stu-id="763dd-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="763dd-133">Por ejemplo, una aplicación denominada `SignalRChattR` tiene una dirección URL similar a `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="763dd-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="763dd-134">Si se produce un error HTTP 502.2, consulte [versión de vista previa de la implementación de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/index) para resolverlo.</span><span class="sxs-lookup"><span data-stu-id="763dd-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="763dd-135">Configuración de aplicación web de SignalR</span><span class="sxs-lookup"><span data-stu-id="763dd-135">Configure SignalR web app</span></span>

<span data-ttu-id="763dd-136">ASP.NET Core SignalR aplicaciones que se publican como una aplicación Web de Azure debe tener [afinidad ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado.</span><span class="sxs-lookup"><span data-stu-id="763dd-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="763dd-137">[WebSockets](xref:fundamentals/websockets) debe habilitarse para permitir que el transporte de WebSockets funcione.</span><span class="sxs-lookup"><span data-stu-id="763dd-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="763dd-138">En el portal de Azure, vaya a **configuración de la aplicación** para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="763dd-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="763dd-139">Establecer **WebSockets** a **en**y compruebe **afinidad ARR** es **en**.</span><span class="sxs-lookup"><span data-stu-id="763dd-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Configuración de Azure Web app en Azure portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="763dd-141">WebSockets y otros transportes [están limitados según el Plan de App Service](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="763dd-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="763dd-142">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="763dd-142">Related resources</span></span>

* [<span data-ttu-id="763dd-143">Publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="763dd-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="763dd-144">Publicar una aplicación ASP.NET Core en Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="763dd-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="763dd-145">Hospedar e implementar aplicaciones de la versión preliminar de ASP.NET Core en Azure</span><span class="sxs-lookup"><span data-stu-id="763dd-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
