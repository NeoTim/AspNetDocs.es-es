---
title: Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core
author: rick-anderson
description: Aprenda a crear perfiles de publicación en Visual Studio y usarlos para administrar implementaciones de aplicaciones ASP.NET Core en diversos destinos.
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: e1e8f99be18d6f395a146bda805f71c46cd0346d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57058622"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="846c2-103">Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="846c2-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="846c2-104">Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="846c2-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="846c2-105">Para la versión 1.1 de este tema, descargue [Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core (versión 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="846c2-105">For the 1.1 version of this topic, download [Visual Studio publish profiles for ASP.NET Core app deployment (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="846c2-106">Este documento se centra en el uso de Visual Studio 2017 o versiones posteriores para crear y usar perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-106">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="846c2-107">Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="846c2-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="846c2-108">Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre la publicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="846c2-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="846c2-109">El siguiente archivo de proyecto se creó con el comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="846c2-109">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="846c2-110">El atributo `<Project>` del elemento `Sdk` lleva a cabo las siguientes tareas:</span><span class="sxs-lookup"><span data-stu-id="846c2-110">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="846c2-111">Importa el archivo de propiedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al comienzo.</span><span class="sxs-lookup"><span data-stu-id="846c2-111">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="846c2-112">Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.</span><span class="sxs-lookup"><span data-stu-id="846c2-112">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="846c2-113">La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="846c2-113">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="846c2-114">El SDK de `Microsoft.NET.Sdk.Web` depende de:</span><span class="sxs-lookup"><span data-stu-id="846c2-114">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="846c2-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="846c2-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="846c2-116">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="846c2-116">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="846c2-117">Lo que hace que se importen las propiedades y los destinos siguientes:</span><span class="sxs-lookup"><span data-stu-id="846c2-117">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="846c2-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="846c2-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="846c2-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="846c2-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="846c2-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="846c2-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="846c2-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="846c2-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="846c2-122">Los destinos de publicación importan el conjunto adecuado de destinos en función del método de publicación usado.</span><span class="sxs-lookup"><span data-stu-id="846c2-122">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="846c2-123">Cuando se carga un proyecto en Visual Studio o MSBuild, se llevan a cabo las siguientes acciones generales:</span><span class="sxs-lookup"><span data-stu-id="846c2-123">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="846c2-124">Compilación del proyecto</span><span class="sxs-lookup"><span data-stu-id="846c2-124">Build project</span></span>
* <span data-ttu-id="846c2-125">Cálculo de los archivos para la publicación</span><span class="sxs-lookup"><span data-stu-id="846c2-125">Compute files to publish</span></span>
* <span data-ttu-id="846c2-126">Publicación de los archivos en el destino</span><span class="sxs-lookup"><span data-stu-id="846c2-126">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="846c2-127">Cálculo de los elementos del proyecto</span><span class="sxs-lookup"><span data-stu-id="846c2-127">Compute project items</span></span>

<span data-ttu-id="846c2-128">Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos).</span><span class="sxs-lookup"><span data-stu-id="846c2-128">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="846c2-129">El atributo `item type` determina cómo se procesa el archivo.</span><span class="sxs-lookup"><span data-stu-id="846c2-129">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="846c2-130">De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="846c2-130">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="846c2-131">Después se compilan los archivos de la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="846c2-131">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="846c2-132">La lista de elementos `Content` contiene archivos que se van a publicar, además de los resultados de compilación.</span><span class="sxs-lookup"><span data-stu-id="846c2-132">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="846c2-133">De forma predeterminada, los archivos que coinciden con el patrón `wwwroot/**` se incluyen en el elemento `Content`.</span><span class="sxs-lookup"><span data-stu-id="846c2-133">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="846c2-134">El `wwwroot/\*\*` [patrón global](https://gruntjs.com/configuring-tasks#globbing-patterns) coincide con los archivos de la carpeta *wwwroot* y de las subcarpetas **y**.</span><span class="sxs-lookup"><span data-stu-id="846c2-134">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="846c2-135">Para agregar explícitamente un archivo a la lista de publicación, agregue el archivo directamente en el archivo *.csproj*, como se muestra en [Archivos de inclusión](#include-files).</span><span class="sxs-lookup"><span data-stu-id="846c2-135">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="846c2-136">Al seleccionar el botón **Publicar** en Visual Studio o al publicar desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="846c2-136">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="846c2-137">Se calculan los elementos/propiedades (los archivos que se deben compilar).</span><span class="sxs-lookup"><span data-stu-id="846c2-137">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="846c2-138">**Solo Visual Studio**: Se han restaurado los paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="846c2-138">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="846c2-139">(la restauración debe ser explícita por parte del usuario en la CLI).</span><span class="sxs-lookup"><span data-stu-id="846c2-139">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="846c2-140">Se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="846c2-140">The project builds.</span></span>
* <span data-ttu-id="846c2-141">Se calculan los elementos de publicación (los archivos que se deben publicar).</span><span class="sxs-lookup"><span data-stu-id="846c2-141">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="846c2-142">Se publica el proyecto (los archivos calculados se copian en el destino de publicación).</span><span class="sxs-lookup"><span data-stu-id="846c2-142">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="846c2-143">Cuando hace referencia a un proyecto de ASP.NET Core `Microsoft.NET.Sdk.Web` en el archivo de proyecto, se coloca un archivo *app_offline.htm* en la raíz del directorio de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="846c2-143">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="846c2-144">Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación.</span><span class="sxs-lookup"><span data-stu-id="846c2-144">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="846c2-145">Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm) (Referencia de configuración del módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="846c2-145">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="846c2-146">Publicación básica de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="846c2-146">Basic command-line publishing</span></span>

<span data-ttu-id="846c2-147">La publicación de línea de comandos funciona en todas las plataformas admitidas de .NET Core y no se requiere Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="846c2-147">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="846c2-148">En los ejemplos siguientes, se ejecuta el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el directorio del proyecto (que contiene el archivo *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="846c2-148">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="846c2-149">Si no se encuentra en la carpeta del proyecto, pase de forma explícita la ruta de acceso del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="846c2-149">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="846c2-150">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="846c2-150">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="846c2-151">Ejecute los comandos siguientes para crear y publicar una aplicación web:</span><span class="sxs-lookup"><span data-stu-id="846c2-151">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="846c2-152">El comando [dotnet publish](/dotnet/core/tools/dotnet-publish) produce una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="846c2-152">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="846c2-153">La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`,</span><span class="sxs-lookup"><span data-stu-id="846c2-153">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="846c2-154">El valor predeterminado de `$(Configuration)` es *Debug*.</span><span class="sxs-lookup"><span data-stu-id="846c2-154">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="846c2-155">En el ejemplo anterior, el elemento `<TargetFramework>` es `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="846c2-155">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="846c2-156">`dotnet publish -h` muestra información de ayuda de la publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="846c2-157">El siguiente comando especifica una compilación `Release` y el directorio de publicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="846c2-158">El comando [dotnet publish](/dotnet/core/tools/dotnet-publish) llama a MSBuild, que invoca el destino `Publish`.</span><span class="sxs-lookup"><span data-stu-id="846c2-158">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="846c2-159">Todos los parámetros pasados a `dotnet publish` se pasan a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="846c2-159">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="846c2-160">El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="846c2-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="846c2-161">El parámetro `-o` se asigna a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="846c2-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="846c2-162">Se pueden pasar propiedades de MSBuild mediante cualquiera de los siguientes formatos:</span><span class="sxs-lookup"><span data-stu-id="846c2-162">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="846c2-163">El comando siguiente publica una compilación `Release` en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="846c2-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="846c2-164">El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="846c2-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="846c2-165">Confirme que la aplicación publicada para la implementación no se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="846c2-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="846c2-166">Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="846c2-167">No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.</span><span class="sxs-lookup"><span data-stu-id="846c2-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="846c2-168">Publicar los perfiles</span><span class="sxs-lookup"><span data-stu-id="846c2-168">Publish profiles</span></span>

<span data-ttu-id="846c2-169">En esta sección se usa Visual Studio 2017 o versiones posteriores para crear un perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-169">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="846c2-170">Una vez creado el perfil, es posible realizar la publicación desde Visual Studio o la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="846c2-170">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="846c2-171">Los perfiles de publicación pueden simplificar el proceso de publicación, y puede existir cualquier número de perfiles.</span><span class="sxs-lookup"><span data-stu-id="846c2-171">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="846c2-172">Cree un perfil de publicación en Visual Studio mediante la selección de una de las rutas de acceso siguientes:</span><span class="sxs-lookup"><span data-stu-id="846c2-172">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="846c2-173">Haga clic con el botón derecho en el Explorador de soluciones y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="846c2-173">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="846c2-174">Seleccione **Publicar {NOMBRE DEL PROYECTO}** en el menú **Compilar**.</span><span class="sxs-lookup"><span data-stu-id="846c2-174">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="846c2-175">Aparece la pestaña **Publicar** de la página de funcionalidades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-175">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="846c2-176">Si el proyecto no contiene ningún perfil de publicación, se muestra la página siguiente:</span><span class="sxs-lookup"><span data-stu-id="846c2-176">If the project lacks a publish profile, the following page is displayed:</span></span>

![La pestaña Publicar de la página de funcionalidades de la aplicación](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="846c2-178">Cuando se seleccione **Carpeta**, especifique una ruta de acceso a la carpeta para almacenar los recursos publicados.</span><span class="sxs-lookup"><span data-stu-id="846c2-178">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="846c2-179">La carpeta predeterminada es *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="846c2-179">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="846c2-180">Haga clic en el botón **Crear perfil** para terminar.</span><span class="sxs-lookup"><span data-stu-id="846c2-180">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="846c2-181">Una vez que se crea un perfil de publicación, la pestaña **Publicar** cambia.</span><span class="sxs-lookup"><span data-stu-id="846c2-181">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="846c2-182">El perfil recién creado aparece en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="846c2-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="846c2-183">Haga clic en **Crear nuevo perfil** para crear otro nuevo perfil.</span><span class="sxs-lookup"><span data-stu-id="846c2-183">Click **Create new profile** to create another new profile.</span></span>

![La pestaña Publicar de la página de funcionalidades de la aplicación que muestra FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="846c2-185">El Asistente para publicación admite los siguientes destinos de publicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-185">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="846c2-186">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="846c2-186">Azure App Service</span></span>
* <span data-ttu-id="846c2-187">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="846c2-187">Azure Virtual Machines</span></span>
* <span data-ttu-id="846c2-188">IIS, FTP, etc. (para cualquier servidor web)</span><span class="sxs-lookup"><span data-stu-id="846c2-188">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="846c2-189">Carpeta</span><span class="sxs-lookup"><span data-stu-id="846c2-189">Folder</span></span>
* <span data-ttu-id="846c2-190">Perfil de importación</span><span class="sxs-lookup"><span data-stu-id="846c2-190">Import Profile</span></span>

<span data-ttu-id="846c2-191">Para más información, consulte [¿Qué opciones de publicación son las adecuadas para mí?](/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="846c2-191">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="846c2-192">Al crear un perfil de publicación con Visual Studio, se crea un archivo MSBuild denominado *Properties/PublishProfiles/{NOMBRE DEL PERFIL}.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="846c2-192">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="846c2-193">Este archivo *.pubxml* es un archivo de MSBuild que contiene la configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-193">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="846c2-194">Este archivo se puede modificar para personalizar el proceso de compilación y publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-194">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="846c2-195">Este archivo se lee durante el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-195">This file is read by the publishing process.</span></span> <span data-ttu-id="846c2-196">`<LastUsedBuildConfiguration>` es especial porque es una propiedad global y no debe estar en ningún archivo importado en la compilación.</span><span class="sxs-lookup"><span data-stu-id="846c2-196">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="846c2-197">Consulte [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: Cómo establecer la propiedad de configuración) para más información.</span><span class="sxs-lookup"><span data-stu-id="846c2-197">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="846c2-198">Al publicar en un destino de Azure, el archivo *.pubxml* contiene el identificador de suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="846c2-198">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="846c2-199">Con ese tipo de destino, no se recomienda agregar este archivo al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="846c2-199">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="846c2-200">Al publicar en un destino que no sea Azure, es seguro insertar el archivo *.pubxml* en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="846c2-200">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="846c2-201">La información confidencial (por ejemplo, la contraseña de publicación) se cifra en cada usuario y máquina.</span><span class="sxs-lookup"><span data-stu-id="846c2-201">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="846c2-202">Este se almacena en el archivo *Properties/PublishProfiles/{NOMBRE DEL PERFIL}.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="846c2-202">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="846c2-203">Como este archivo puede contener información confidencial, no se debe insertar en el repositorio de control del código fuente.</span><span class="sxs-lookup"><span data-stu-id="846c2-203">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="846c2-204">Para información general sobre cómo publicar una aplicación web en ASP.NET Core, consulte [Hospedaje e implementación](xref:host-and-deploy/index),</span><span class="sxs-lookup"><span data-stu-id="846c2-204">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="846c2-205">Las tareas de MSBuild y los destinos necesarios para publicar una aplicación ASP.NET Core son de código abierto en https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="846c2-205">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="846c2-206">`dotnet publish` puede usar perfiles de publicación, de carpeta, Msdeploy y [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="846c2-206">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="846c2-207">Carpeta (funciona entre plataformas)</span><span class="sxs-lookup"><span data-stu-id="846c2-207">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="846c2-208">MSDeploy (actualmente solo funciona en Windows dado que MSDeploy no es multiplataforma):</span><span class="sxs-lookup"><span data-stu-id="846c2-208">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="846c2-209">El paquete de MSDeploy (actualmente solo funciona en Windows dado que MSDeploy no es multiplataforma):</span><span class="sxs-lookup"><span data-stu-id="846c2-209">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="846c2-210">En los ejemplos anteriores, **no** pase `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="846c2-210">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="846c2-211">Para más información, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="846c2-211">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="846c2-212">`dotnet publish` admite las API de Kudu para publicar en Azure desde cualquier plataforma.</span><span class="sxs-lookup"><span data-stu-id="846c2-212">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="846c2-213">La publicación de Visual Studio admite API de Kudu, pero es compatible con WebSDK para la publicación multiplataforma en Azure.</span><span class="sxs-lookup"><span data-stu-id="846c2-213">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="846c2-214">Agregue un perfil de publicación a la carpeta *Properties/PublishProfiles* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="846c2-214">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="846c2-215">Ejecute el comando siguiente para descomprimir el contenido de publicación y publicarlo en Azure mediante las API de Kudu:</span><span class="sxs-lookup"><span data-stu-id="846c2-215">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="846c2-216">Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-216">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="846c2-217">Por ejemplo, al efectuar una publicación con un perfil denominado *FolderProfile*, se puede ejecutar cualquiera de los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="846c2-217">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="846c2-218">Al invocar [dotnet build](/dotnet/core/tools/dotnet-build), se llama a `msbuild` para ejecutar el proceso de compilación y publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-218">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="846c2-219">Llamar a `dotnet build` o `msbuild` es equivalente cuando se pasa un perfil de carpeta.</span><span class="sxs-lookup"><span data-stu-id="846c2-219">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="846c2-220">Al llamar a MSBuild directamente en Windows, se usa la versión .NET Framework de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="846c2-220">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="846c2-221">Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones.</span><span class="sxs-lookup"><span data-stu-id="846c2-221">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="846c2-222">Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta.</span><span class="sxs-lookup"><span data-stu-id="846c2-222">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="846c2-223">Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows).</span><span class="sxs-lookup"><span data-stu-id="846c2-223">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="846c2-224">Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="846c2-224">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="846c2-225">El siguiente perfil de publicación de carpeta se creó con Visual Studio y se publica en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="846c2-225">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="846c2-226">Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`.</span><span class="sxs-lookup"><span data-stu-id="846c2-226">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="846c2-227">Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-227">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="846c2-228">La propiedad de configuración `<LastUsedBuildConfiguration>` es especial y no se debe reemplazar en un archivo importado de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="846c2-228">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="846c2-229">Esta propiedad se puede invalidar desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="846c2-229">This property can be overridden from the command line.</span></span>

<span data-ttu-id="846c2-230">Con la CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="846c2-230">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="846c2-231">Con MSBuild:</span><span class="sxs-lookup"><span data-stu-id="846c2-231">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="846c2-232">Publicar en un punto de conexión de MSDeploy desde la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="846c2-232">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="846c2-233">En el ejemplo siguiente se utiliza una aplicación web ASP.NET Core de ejemplo creada mediante Visual Studio y denominada *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="846c2-233">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="846c2-234">Se agrega un perfil de publicación de aplicaciones de Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="846c2-234">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="846c2-235">Para obtener más información sobre cómo crear un perfil, consulte la sección [Perfiles de publicación](#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="846c2-235">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="846c2-236">Para implementar la aplicación con un perfil de publicación, ejecute el comando `msbuild` desde un **símbolo del sistema para desarrolladores** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="846c2-236">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="846c2-237">El símbolo del sistema se encuentra disponible en la carpeta *Visual Studio* del menú **Inicio**, en la barra de tareas de Windows.</span><span class="sxs-lookup"><span data-stu-id="846c2-237">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="846c2-238">Para facilitar el acceso, puede agregar el símbolo del sistema al menú **Herramientas** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="846c2-238">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="846c2-239">Para más información, consulte [Símbolo del sistema para desarrolladores de Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="846c2-239">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="846c2-240">MSBuild usa la sintaxis de comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="846c2-240">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="846c2-241">{RUTA DE ACCESO}: ruta de acceso al archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-241">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="846c2-242">{PERFIL}: nombre del perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-242">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="846c2-243">{NOMBRE DE USUARIO}: nombre de usuario de MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="846c2-243">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="846c2-244">El valor {NOMBRE DE USUARIO} se encuentra en el perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-244">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="846c2-245">{CONTRASEÑA}: contraseña de MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="846c2-245">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="846c2-246">Puede obtener el valor {CONTRASEÑA} en el archivo *{PERFIL}.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="846c2-246">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="846c2-247">Descargue el archivo *.PublishSettings* desde:</span><span class="sxs-lookup"><span data-stu-id="846c2-247">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="846c2-248">Explorador de soluciones: Seleccione **Ver** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="846c2-248">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="846c2-249">Conéctese con su suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="846c2-249">Connect with your Azure subscription.</span></span> <span data-ttu-id="846c2-250">Abra **App Services**.</span><span class="sxs-lookup"><span data-stu-id="846c2-250">Open **App Services**.</span></span> <span data-ttu-id="846c2-251">Haga clic con el botón derecho en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-251">Right-click the app.</span></span> <span data-ttu-id="846c2-252">Seleccione **Descargar perfil de publicación**.</span><span class="sxs-lookup"><span data-stu-id="846c2-252">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="846c2-253">Azure Portal: Haga clic en **Obtener perfil de publicación**, en el panel **Información general** de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="846c2-253">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="846c2-254">En el ejemplo siguiente se usa un perfil de publicación denominado *AzureWebApp - Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="846c2-254">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="846c2-255">También se puede usar un perfil de publicación ejecutando el comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) de la CLI de .NET Core en un símbolo del sistema de Windows:</span><span class="sxs-lookup"><span data-stu-id="846c2-255">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="846c2-256">El comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) es multiplataforma y permite compilar aplicaciones ASP.NET Core en macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="846c2-256">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="846c2-257">Sin embargo, MSBuild en macOS y Linux no permite implementar una aplicación en Azure ni en otro punto de conexión de MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="846c2-257">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="846c2-258">MSDeploy solo está disponible en Windows.</span><span class="sxs-lookup"><span data-stu-id="846c2-258">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="846c2-259">Establecimiento del entorno</span><span class="sxs-lookup"><span data-stu-id="846c2-259">Set the environment</span></span>

<span data-ttu-id="846c2-260">Incluya la propiedad `<EnvironmentName>` en el perfil de publicación (*.pubxml*) o el archivo del proyecto para configurar el [entorno](xref:fundamentals/environments) de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-260">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="846c2-261">Si necesita transformaciones de *web.config* (por ejemplo, establecer variables de entorno basadas en la configuración, el perfil o el entorno), consulte <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="846c2-261">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="846c2-262">Archivos de exclusión</span><span class="sxs-lookup"><span data-stu-id="846c2-262">Exclude files</span></span>

<span data-ttu-id="846c2-263">Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="846c2-263">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="846c2-264">`msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="846c2-264">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="846c2-265">Por ejemplo, el siguiente elemento `<Content>` excluye todo los archivos de texto (*.txt*) de la carpeta *wwwroot/content* y de todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="846c2-265">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="846c2-266">El marcado anterior se puede agregar a un perfil de publicación o al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="846c2-266">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="846c2-267">Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="846c2-267">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="846c2-268">El siguiente elemento `<MsDeploySkipRules>` excluye todos los archivos de la carpeta *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="846c2-268">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="846c2-269">`<MsDeploySkipRules>` no eliminará del sitio de implementación los destinos *skip*.</span><span class="sxs-lookup"><span data-stu-id="846c2-269">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="846c2-270">Los archivos y carpetas destinados a `<Content>` se eliminan del sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="846c2-270">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="846c2-271">Por ejemplo, suponga que una aplicación web implementada tenía los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="846c2-271">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="846c2-272">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="846c2-272">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="846c2-273">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="846c2-273">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="846c2-274">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="846c2-274">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="846c2-275">Si se agregan los siguientes elementos `<MsDeploySkipRules>`, esos archivos no se eliminarán en el sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="846c2-275">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="846c2-276">Los elementos `<MsDeploySkipRules>` anteriores impiden que se implementen los archivos *omitidos*.</span><span class="sxs-lookup"><span data-stu-id="846c2-276">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="846c2-277">Una vez que se implementan esos archivos, no se eliminarán.</span><span class="sxs-lookup"><span data-stu-id="846c2-277">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="846c2-278">El siguiente elemento `<Content>` elimina los archivos de destino en el sitio de implementación:</span><span class="sxs-lookup"><span data-stu-id="846c2-278">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="846c2-279">El uso de la implementación de línea de comandos con el elemento `<Content>` anterior, produce la siguiente salida:</span><span class="sxs-lookup"><span data-stu-id="846c2-279">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="846c2-280">Archivos de inclusión</span><span class="sxs-lookup"><span data-stu-id="846c2-280">Include files</span></span>

<span data-ttu-id="846c2-281">El siguiente marcado incluye una carpeta *images* fuera del directorio del proyecto a la carpeta *wwwroot/images* del sitio de publicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-281">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="846c2-282">El marcado se puede agregar al archivo *.csproj* o al perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-282">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="846c2-283">Si se agrega al archivo *.csproj*, se incluye en todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="846c2-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="846c2-284">En el siguiente marcado resaltado se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="846c2-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="846c2-285">Copiar un archivo de fuera del proyecto a la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="846c2-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="846c2-286">Excluir la carpeta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="846c2-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="846c2-287">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="846c2-287">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="846c2-288">Vea [WebSDK Readme](https://github.com/aspnet/websdk) (Archivo Léame de WebSDK) para ver más ejemplos de implementación.</span><span class="sxs-lookup"><span data-stu-id="846c2-288">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="846c2-289">Ejecutar un destino antes o después de la publicación</span><span class="sxs-lookup"><span data-stu-id="846c2-289">Run a target before or after publishing</span></span>

<span data-ttu-id="846c2-290">Los destinos `BeforePublish` y `AfterPublish` ejecutan un destino antes o después del destino de publicación.</span><span class="sxs-lookup"><span data-stu-id="846c2-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="846c2-291">Agregue los siguientes elementos al perfil de publicación para registrar mensajes de la consola antes y después de la publicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="846c2-292">Publicación en un servidor mediante un certificado que no es de confianza</span><span class="sxs-lookup"><span data-stu-id="846c2-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="846c2-293">Agregue la propiedad `<AllowUntrustedCertificate>` con un valor de `True` al perfil de publicación:</span><span class="sxs-lookup"><span data-stu-id="846c2-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="846c2-294">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="846c2-294">The Kudu service</span></span>

<span data-ttu-id="846c2-295">Para ver los archivos de la implementación de una aplicación web de Azure App Service, use el [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="846c2-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="846c2-296">Anexe el token `scm` al nombre de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="846c2-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="846c2-297">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="846c2-297">For example:</span></span>

| <span data-ttu-id="846c2-298">Resolución</span><span class="sxs-lookup"><span data-stu-id="846c2-298">URL</span></span>                                    | <span data-ttu-id="846c2-299">Resultado</span><span class="sxs-lookup"><span data-stu-id="846c2-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="846c2-300">Aplicación web</span><span class="sxs-lookup"><span data-stu-id="846c2-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="846c2-301">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="846c2-301">Kudu service</span></span> |

<span data-ttu-id="846c2-302">Seleccione el elemento de menú [Consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) para ver, editar, eliminar o agregar archivos.</span><span class="sxs-lookup"><span data-stu-id="846c2-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="846c2-303">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="846c2-303">Additional resources</span></span>

* <span data-ttu-id="846c2-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica la implementación de aplicaciones web y sitios web en servidores de IIS.</span><span class="sxs-lookup"><span data-stu-id="846c2-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="846c2-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de archivos y características de solicitud para la implementación.</span><span class="sxs-lookup"><span data-stu-id="846c2-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="846c2-306">Publicación de una aplicación web ASP.NET en una máquina virtual de Azure desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="846c2-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
