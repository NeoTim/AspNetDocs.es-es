---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de la versión para ASP.NET y Web Tools 2013.1 para Visual Studio 2012 Microsoft Docs
author: rick-anderson
description: Este documento describe la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d4aced4e77a150d52358c2d2513ff79e6594a9de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543579"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="c4038-103">Notas de la versión de ASP.NET and Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c4038-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="c4038-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c4038-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c4038-105">Este documento describe la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c4038-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="c4038-106">Contenido</span><span class="sxs-lookup"><span data-stu-id="c4038-106">Contents</span></span>

- [<span data-ttu-id="c4038-107">Notas de instalación</span><span class="sxs-lookup"><span data-stu-id="c4038-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="c4038-108">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="c4038-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="c4038-109">Nuevas características en ASP.NET y Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c4038-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="c4038-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="c4038-110">Bootstrap</span></span>](#bootstrap)
    - <span data-ttu-id="c4038-111">[Templates](#templates) (Plantillas [C++])</span><span class="sxs-lookup"><span data-stu-id="c4038-111">[Templates](#templates)</span></span>

        - [<span data-ttu-id="c4038-112">ASP.NET MVC 5 plantilla</span><span class="sxs-lookup"><span data-stu-id="c4038-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="c4038-113">ASP.NET plantilla de Web API 2</span><span class="sxs-lookup"><span data-stu-id="c4038-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="c4038-114">Plantillas de elementos</span><span class="sxs-lookup"><span data-stu-id="c4038-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="c4038-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c4038-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="c4038-116">andamios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4038-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="c4038-117">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="c4038-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="c4038-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="c4038-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="c4038-119">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="c4038-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="c4038-120">andamios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4038-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="c4038-121">MVC y Web API Scaffolding - HTTP 404, Error no encontrado</span><span class="sxs-lookup"><span data-stu-id="c4038-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="c4038-122">Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding</span><span class="sxs-lookup"><span data-stu-id="c4038-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="c4038-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="c4038-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="c4038-124">Ver el archivo cshtml con Browse With o F5 provoca un error de servidor</span><span class="sxs-lookup"><span data-stu-id="c4038-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="c4038-125">Url Rewrite y Tilde(')</span><span class="sxs-lookup"><span data-stu-id="c4038-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - <span data-ttu-id="c4038-126">[Templates](#templateissue) (Plantillas [C++])</span><span class="sxs-lookup"><span data-stu-id="c4038-126">[Templates](#templateissue)</span></span>

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="c4038-127">Notas de la instalación</span><span class="sxs-lookup"><span data-stu-id="c4038-127">Installation Notes</span></span>

<span data-ttu-id="c4038-128">[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET y Web Tools 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c4038-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="c4038-129">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="c4038-129">Software Requirements</span></span>

<span data-ttu-id="c4038-130">Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="c4038-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="c4038-131">Nuevas características en ASP.NET y Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c4038-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="c4038-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="c4038-132">Bootstrap</span></span>

<span data-ttu-id="c4038-133">Al aplicar scaffolding MVC 5 controladores y vistas, el marcado de las vistas usa [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="c4038-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="c4038-134">Plantillas</span><span class="sxs-lookup"><span data-stu-id="c4038-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="c4038-135">ASP.NET MVC 5 plantilla</span><span class="sxs-lookup"><span data-stu-id="c4038-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="c4038-136">Agregamos una nueva plantilla MVC 5.</span><span class="sxs-lookup"><span data-stu-id="c4038-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="c4038-137">Hace referencia a los últimos paquetes NuGet de MVC 5 y puede usar scaffolding para agregar controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="c4038-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="c4038-138">ASP.NET plantilla de Web API 2</span><span class="sxs-lookup"><span data-stu-id="c4038-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="c4038-139">Hemos añadido una nueva plantilla de Web API 2.</span><span class="sxs-lookup"><span data-stu-id="c4038-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="c4038-140">Hace referencia a los paquetes NuGet de Web API 2 más recientes y puede usar scaffolding para agregar controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="c4038-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="c4038-141">Plantillas de elementos</span><span class="sxs-lookup"><span data-stu-id="c4038-141">Item Templates</span></span>

<span data-ttu-id="c4038-142">Agregamos nuevas plantillas de elementos para vistas MVC 5, páginas web (Razor 3) y controladores de Web API 2.</span><span class="sxs-lookup"><span data-stu-id="c4038-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="c4038-143">Instalan los paquetes NuGet relacionados en el proyecto mientras agregan nuevos elementos.</span><span class="sxs-lookup"><span data-stu-id="c4038-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="c4038-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c4038-144">Entity Framework 6</span></span>

<span data-ttu-id="c4038-145">Cuando se aplica a un scaffolding a un controlador MVC o Web API mediante Entity Framework, usamos Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c4038-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="c4038-146">Para obtener más información acerca de Entity Framework, vea el Historial de [versiones](https://msdn.com/data/jj574253)de Entity Framework .</span><span class="sxs-lookup"><span data-stu-id="c4038-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="c4038-147">También puede descargar e instalar Entity Framework 6 Tools para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c4038-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="c4038-148">Consulte [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="c4038-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="c4038-149">andamios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4038-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="c4038-150">ASP.NET Scaffolding es un marco de generación de código para ASP.NET aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="c4038-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="c4038-151">Facilita la adición de código reutilizable al proyecto que interactúa con un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c4038-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="c4038-152">En versiones anteriores de Visual Studio, el scaffolding se limitaba a ASP.NET proyectos MVC.</span><span class="sxs-lookup"><span data-stu-id="c4038-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="c4038-153">Con esta actualización, ahora puede usar scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="c4038-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="c4038-154">Esta actualización no admite la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms agregando dependencias MVC al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4038-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="c4038-155">La compatibilidad con la generación de páginas para formularios Web Forms se agregará en una actualización futura.</span><span class="sxs-lookup"><span data-stu-id="c4038-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="c4038-156">Cuando se usa scaffolding, nos aseguramos de que todas las dependencias necesarias están instaladas en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4038-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="c4038-157">Por ejemplo, si comienza con un proyecto de formularios Web Forms ASP.NET y, a continuación, usa scaffolding para agregar un controlador de API web, los paquetes NuGet y las referencias necesarios se agregan automáticamente al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4038-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="c4038-158">Para agregar scaffolding MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento con scaffolding** y seleccione **dependencias MVC 5** en la ventana de diálogo.</span><span class="sxs-lookup"><span data-stu-id="c4038-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="c4038-159">Hay dos opciones para scaffolding MVC; Mínimo y Completo.</span><span class="sxs-lookup"><span data-stu-id="c4038-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="c4038-160">Si selecciona Mínimo, solo se agregan al proyecto los paquetes NuGet y las referencias de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c4038-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="c4038-161">Si selecciona la opción Completo, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto MVC.</span><span class="sxs-lookup"><span data-stu-id="c4038-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="c4038-162">La compatibilidad con controladores asincrónicos de scaffolding utiliza las nuevas características asincrónicas de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c4038-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="c4038-163">Para obtener más información y tutoriales, vea [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4038-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="c4038-164">Estos tutoriales muestran scaffolding con Visual Studio 2013, pero también son aplicables a ASP.NET y Web Tools 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c4038-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="c4038-165">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="c4038-165">Razor Editor</span></span>

<span data-ttu-id="c4038-166">Con esta actualización, Visual Studio 2012 ahora admite herramientas y edición de Razor 3.</span><span class="sxs-lookup"><span data-stu-id="c4038-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="c4038-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="c4038-167">NuGet 2.7</span></span>

<span data-ttu-id="c4038-168">NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en las notas de la versión de [NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="c4038-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="c4038-169">Esta versión de NuGet elimina la necesidad de que los usuarios permitan explícitamente nuGet restaurar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="c4038-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="c4038-170">Al instalar NuGet 2.7, los usuarios consienten implícitamente la restauración automática de los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="c4038-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="c4038-171">Los usuarios pueden excluirse explícitamente de la restauración de paquetes a través de la configuración de NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4038-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="c4038-172">Este cambio simplifica el funcionamiento de la restauración de paquetes.</span><span class="sxs-lookup"><span data-stu-id="c4038-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="c4038-173">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="c4038-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="c4038-174">andamios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4038-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="c4038-175">MVC y Web API Scaffolding - HTTP 404, Error no encontrado</span><span class="sxs-lookup"><span data-stu-id="c4038-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="c4038-176">Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quede en un estado incoherente.</span><span class="sxs-lookup"><span data-stu-id="c4038-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="c4038-177">Algunos de los cambios realizados serán scaffolding se revertirán, pero otros cambios, como los paquetes NuGet instalados, no se revertirán.</span><span class="sxs-lookup"><span data-stu-id="c4038-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="c4038-178">Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 al navegar a elementos con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c4038-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="c4038-179">Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccione MVC 5 Dependencias (mínimas o completas).</span><span class="sxs-lookup"><span data-stu-id="c4038-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="c4038-180">Este proceso agregará todos los cambios necesarios al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4038-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="c4038-181">Para corregir este error para Web API:</span><span class="sxs-lookup"><span data-stu-id="c4038-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="c4038-182">Agregue la siguiente clase WebApiConfig al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4038-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="c4038-183">Configure WebApiConfig.Register en\_el método Application Start en Global.asax de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="c4038-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="c4038-184">Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding</span><span class="sxs-lookup"><span data-stu-id="c4038-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="c4038-185">Si Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding con Entity Framework (como Web API 2 Controller con acciones, mediante Entity Framework), es posible que Visual Studio Express no haya podido cargar la imagen nativa de un ensamblado dependiente de System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="c4038-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="c4038-186">Para corregir este problema, configure Visual Studio Express para que funcione con la imagen MSIL de System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="c4038-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="c4038-187">Abra el símbolo del sistema en el modo Administrador.</span><span class="sxs-lookup"><span data-stu-id="c4038-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="c4038-188">Ir a %Archivos de programa%-Microsoft Visual Studio 11.0-Common7-IDE o %ProgramFiles(x86)%-Microsoft Visual Studio 11.0-Common7-IDE (para Windows de 64 bits).</span><span class="sxs-lookup"><span data-stu-id="c4038-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="c4038-189">Abra VWDExpress.exe.config en un editor de texto.</span><span class="sxs-lookup"><span data-stu-id="c4038-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="c4038-190">Agregue la siguiente &lt;línea&gt;/&lt;&gt; en el elemento de tiempo de ejecución de configuración:</span><span class="sxs-lookup"><span data-stu-id="c4038-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="c4038-191">Reinicie Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="c4038-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="c4038-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="c4038-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="c4038-193">Ver el archivo cshtml con Browse With o F5 provoca un error de servidor</span><span class="sxs-lookup"><span data-stu-id="c4038-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="c4038-194">Al crear un proyecto MVC 5 en Visual Studio 2012 (o abrir en Visual Studio 2012 un proyecto MVC 5 que se creó en Visual Studio 2013) e intentar ver un archivo cshtml mediante Examinar con o F5, recibirá un error que indica : Error de **servidor en la aplicación '/'**.</span><span class="sxs-lookup"><span data-stu-id="c4038-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="c4038-195">El servidor intenta navegar a`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="c4038-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="c4038-196">Para resolver este problema, cambie la configuración **de Acción de inicio** del proyecto a Página **específica**.</span><span class="sxs-lookup"><span data-stu-id="c4038-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="c4038-197">No es necesario proporcionar un valor para la página.</span><span class="sxs-lookup"><span data-stu-id="c4038-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="c4038-198">Después de realizar este cambio, al seleccionar F5 se navega a la raíz de la aplicación (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="c4038-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="c4038-199">Este comportamiento no es el mismo que el comportamiento de los proyectos MVC 5 en Visual Studio 2013, donde la configuración **Página actual** inicia la página abierta.</span><span class="sxs-lookup"><span data-stu-id="c4038-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="c4038-200">Url Rewrite y Tilde(')</span><span class="sxs-lookup"><span data-stu-id="c4038-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="c4038-201">Después de actualizar a ASP.NET Razor 3 o ASP.NET MVC 5, es posible que la notación de tilde (o) ya no funcione correctamente si está utilizando reescrituras de URL.</span><span class="sxs-lookup"><span data-stu-id="c4038-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="c4038-202">La reescritura de url afecta a la notación &lt;tilde()) &lt;en&gt;elementos HTML como A/&gt;, &lt;SCRIPT/&gt;, LINK/ y, como resultado, la tilde ya no se asigna al directorio raíz.</span><span class="sxs-lookup"><span data-stu-id="c4038-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="c4038-203">Por ejemplo, si vuelve **a**escribir las solicitudes de &lt; **asp.net/content** asp.net , el atributo&gt; href en A href-"/content/"/ se resuelve en **/content/content/** en lugar de **/**.</span><span class="sxs-lookup"><span data-stu-id="c4038-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="c4038-204">Para suprimir este cambio, puede establecer el contexto **WasUrlRewritten\_** de IIS en false en cada página Web o en Application **\_BeginRequest** en Global.asax.</span><span class="sxs-lookup"><span data-stu-id="c4038-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="c4038-205">Plantillas</span><span class="sxs-lookup"><span data-stu-id="c4038-205">Templates</span></span>

<span data-ttu-id="c4038-206">Al crear proyectos de ASP.NET MVC con Visual Studio 2012 en Windows 8.1 o Windows Server 2012 R2, Visual Studio muestra un mensaje de error que indica "Error al configurar Web [url] para ASP.NET 4.5."</span><span class="sxs-lookup"><span data-stu-id="c4038-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![error de configuración](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="c4038-208">Verá este error porque Visual Studio 2012 no habilita la característica ASP.NET 4.5 cuando se instala en esas versiones de Windows.</span><span class="sxs-lookup"><span data-stu-id="c4038-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="c4038-209">Para habilitar ASP.NET 4.5, siga los pasos descritos en [Activar o desactivar](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)las características de Windows .</span><span class="sxs-lookup"><span data-stu-id="c4038-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![activar o desactivar las características de Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="c4038-211">Como alternativa, puede habilitar ASP.NET 4.5 a través de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="c4038-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="c4038-212">Abra el símbolo del sistema en el modo Administrador.</span><span class="sxs-lookup"><span data-stu-id="c4038-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="c4038-213">Ejecute el siguiente comando para habilitar ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c4038-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
