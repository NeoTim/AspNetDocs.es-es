---
title: DevOps con ASP.NET Core y Azure
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: casoper
ms.date: 08/07/2018
ms.custom: seodec18
uid: azure/devops/index
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="9e540-103">DevOps con ASP.NET Core y Azure</span><span class="sxs-lookup"><span data-stu-id="9e540-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="9e540-104">[![Imagen de portada](./media/cover-large.png)](https://aka.ms/devopsbook)</span><span class="sxs-lookup"><span data-stu-id="9e540-104">[![Cover Image](./media/cover-large.png)](https://aka.ms/devopsbook)</span></span>

<span data-ttu-id="9e540-105">Por [Cam Soper](https://twitter.com/camsoper) y [Scott Addie](https://twitter.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="9e540-105">By [Cam Soper](https://twitter.com/camsoper) and [Scott Addie](https://twitter.com/scottaddie)</span></span>

<span data-ttu-id="9e540-106">Esta guía está disponible como [e-book descargable en formato PDF](https://aka.ms/devopsbook).</span><span class="sxs-lookup"><span data-stu-id="9e540-106">This guide is available as a [downloadable PDF e-book](https://aka.ms/devopsbook).</span></span>

## <a name="welcome"></a><span data-ttu-id="9e540-107">Pantalla de inicio</span><span class="sxs-lookup"><span data-stu-id="9e540-107">Welcome</span></span> 

<span data-ttu-id="9e540-108">Le damos la bienvenida a la guía de Ciclo de vida de desarrollo de Azure para .NET.</span><span class="sxs-lookup"><span data-stu-id="9e540-108">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="9e540-109">En esta guía le mostraremos los conceptos básicos de creación de un ciclo de vida de desarrollo en torno a Azure con herramientas y procesos de .NET.</span><span class="sxs-lookup"><span data-stu-id="9e540-109">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="9e540-110">Cuando haya terminado con esta guía, podrá aprovechar las ventajas de una cadena de herramientas madura de DevOps.</span><span class="sxs-lookup"><span data-stu-id="9e540-110">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="9e540-111">Destinatarios de esta guía</span><span class="sxs-lookup"><span data-stu-id="9e540-111">Who this guide is for</span></span>

<span data-ttu-id="9e540-112">La guía está dirigida a desarrolladores experimentados de ASP.NET Core (nivel 200 o 300).</span><span class="sxs-lookup"><span data-stu-id="9e540-112">You should be an experienced ASP.NET Core developer (200-300 level).</span></span> <span data-ttu-id="9e540-113">No es necesario que tenga conocimientos de Azure, ya que está incluido en esta introducción.</span><span class="sxs-lookup"><span data-stu-id="9e540-113">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="9e540-114">Esta guía también podría ser útil para ingenieros de DevOps, cuyo trabajo está más relacionado con las operación que con el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="9e540-114">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="9e540-115">Esta guía está destinada a desarrolladores para Windows.</span><span class="sxs-lookup"><span data-stu-id="9e540-115">This guide targets Windows developers.</span></span> <span data-ttu-id="9e540-116">Sin embargo, .NET Core es completamente compatible con Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="9e540-116">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="9e540-117">Para adaptar esta guía para Linux o macOS, mire las llamadas en las que se indican las diferencias para Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="9e540-117">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="9e540-118">Aspectos no tratados en esta guía</span><span class="sxs-lookup"><span data-stu-id="9e540-118">What this guide doesn't cover</span></span>

<span data-ttu-id="9e540-119">Esta guía está centrada en una experiencia de desarrollo continuo de un extremo a otro para desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="9e540-119">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="9e540-120">No es una guía exhaustiva de todo Azure y no se profundiza particularmente en API de .NET para servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="9e540-120">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="9e540-121">El énfasis está en la integración, la implementación, la supervisión y la depuración continuas.</span><span class="sxs-lookup"><span data-stu-id="9e540-121">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="9e540-122">Casi al final de la guía podrá ver recomendaciones para los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="9e540-122">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="9e540-123">En las sugerencias se incluyen servicios de plataformas de Azure que resultan útiles para desarrolladores de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e540-123">Included in the suggestions are Azure platform services that are useful to ASP.NET Core developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="9e540-124">Qué se incluye en esta guía</span><span class="sxs-lookup"><span data-stu-id="9e540-124">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="9e540-125">Herramientas y descargas</span><span class="sxs-lookup"><span data-stu-id="9e540-125">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="9e540-126">Obtenga información sobre dónde adquirir las herramientas que se usan en esta guía.</span><span class="sxs-lookup"><span data-stu-id="9e540-126">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="9e540-127">Implementación en App Service</span><span class="sxs-lookup"><span data-stu-id="9e540-127">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="9e540-128">Obtenga información sobre los distintos métodos para implementar una aplicación ASP.NET Core en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9e540-128">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="9e540-129">Integración e implementación continuas</span><span class="sxs-lookup"><span data-stu-id="9e540-129">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="9e540-130">Cree una solución de implementación e integración continuas de un extremo a otro para su aplicación ASP.NET Core con GitHub, Azure DevOps Services y Azure.</span><span class="sxs-lookup"><span data-stu-id="9e540-130">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, Azure DevOps Services, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="9e540-131">Supervisión y depuración</span><span class="sxs-lookup"><span data-stu-id="9e540-131">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="9e540-132">Use las herramientas de Azure para supervisar la aplicación, solucionar problemas y ajustarla.</span><span class="sxs-lookup"><span data-stu-id="9e540-132">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="9e540-133">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9e540-133">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="9e540-134">Otras rutas de aprendizaje para los desarrolladores de ASP.NET Core que están aprendiendo sobre Azure.</span><span class="sxs-lookup"><span data-stu-id="9e540-134">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="additional-introductory-reading"></a><span data-ttu-id="9e540-135">Lecturas de introducción adicionales</span><span class="sxs-lookup"><span data-stu-id="9e540-135">Additional introductory reading</span></span>

<span data-ttu-id="9e540-136">Si se trata de su primer contacto con la informática en nube, en estos artículos aprenderá los conceptos básicos.</span><span class="sxs-lookup"><span data-stu-id="9e540-136">If this is your first exposure to cloud computing, these articles explain the basics.</span></span>

* [<span data-ttu-id="9e540-137">¿Qué es la informática en la nube?</span><span class="sxs-lookup"><span data-stu-id="9e540-137">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="9e540-138">Ejemplos de informática en la nube</span><span class="sxs-lookup"><span data-stu-id="9e540-138">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="9e540-139">¿Qué es una IaaS?</span><span class="sxs-lookup"><span data-stu-id="9e540-139">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="9e540-140">¿Qué es un PaaS?</span><span class="sxs-lookup"><span data-stu-id="9e540-140">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
