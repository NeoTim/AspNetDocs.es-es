---
title: Migrar la configuración a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar la configuración de un proyecto de MVC de ASP.NET a un proyecto de ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048242"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="c79f7-103">Migrar la configuración a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79f7-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="c79f7-104">Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c79f7-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c79f7-105">En el artículo anterior, comenzamos a [migrar un proyecto de MVC de ASP.NET a ASP.NET Core MVC](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="c79f7-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="c79f7-106">En este artículo, nos migrar la configuración.</span><span class="sxs-lookup"><span data-stu-id="c79f7-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="c79f7-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c79f7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="c79f7-108">Parámetros de configuración</span><span class="sxs-lookup"><span data-stu-id="c79f7-108">Setup configuration</span></span>

<span data-ttu-id="c79f7-109">ASP.NET Core ya no usa el *Global.asax* y *web.config* archivos que utilizan versiones anteriores de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c79f7-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="c79f7-110">En versiones anteriores de ASP.NET, la lógica de inicio de la aplicación se ha puesto en un `Application_StartUp` método dentro de *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="c79f7-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="c79f7-111">Más adelante, en ASP.NET MVC, un *Startup.cs* archivo se incluyó en la raíz del proyecto; y, en el se llamó cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c79f7-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="c79f7-112">ASP.NET Core ha adoptado este enfoque completamente mediante la colocación de toda la lógica de inicio en el *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="c79f7-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="c79f7-113">El *web.config* archivo también se ha sustituido en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c79f7-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="c79f7-114">Configuración de ahora se puede configurar, como parte del procedimiento de inicio de aplicación se describe en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c79f7-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="c79f7-115">Configuración todavía puede usar archivos XML, pero normalmente los proyectos de ASP.NET Core colocará los valores de configuración en un archivo con formato JSON, como *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c79f7-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="c79f7-116">Sistema de configuración de ASP.NET Core también puede acceder fácilmente las variables de entorno, lo que pueden proporcionar un [ubicación más segura y sólida](xref:security/app-secrets) para valores específicos del entorno.</span><span class="sxs-lookup"><span data-stu-id="c79f7-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="c79f7-117">Esto es especialmente cierto para los secretos, como las cadenas de conexión y las claves de API que no deben comprobarse en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="c79f7-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="c79f7-118">Consulte [configuración](xref:fundamentals/configuration/index) para obtener más información sobre la configuración en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c79f7-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="c79f7-119">En este artículo, hemos empezado con el proyecto de ASP.NET Core parcialmente migrado desde [el artículo anterior](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="c79f7-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="c79f7-120">En la instalación de configuración, agregue el siguiente constructor y propiedad a la *Startup.cs* archivo se encuentra en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="c79f7-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="c79f7-121">Tenga en cuenta que en este momento, el *Startup.cs* archivo no se compilará, medida necesitamos agregar lo siguiente `using` instrucción:</span><span class="sxs-lookup"><span data-stu-id="c79f7-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="c79f7-122">Agregar un *appsettings.json* archivo a la raíz del proyecto mediante la plantilla de elemento adecuado:</span><span class="sxs-lookup"><span data-stu-id="c79f7-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Agregar AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="c79f7-124">Migrar la configuración de web.config</span><span class="sxs-lookup"><span data-stu-id="c79f7-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="c79f7-125">Nuestro proyecto de ASP.NET MVC incluye la cadena de conexión de base de datos necesarios en *web.config*, en el `<connectionStrings>` elemento.</span><span class="sxs-lookup"><span data-stu-id="c79f7-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="c79f7-126">En nuestro proyecto de ASP.NET Core, vamos a almacenar esta información en el *appsettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="c79f7-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="c79f7-127">Abra *appsettings.json*y tenga en cuenta que ya incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c79f7-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="c79f7-128">En la línea resaltada descrita anteriormente, cambie el nombre de la base de datos **_CHANGE_ME** en el nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c79f7-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="c79f7-129">Resumen</span><span class="sxs-lookup"><span data-stu-id="c79f7-129">Summary</span></span>

<span data-ttu-id="c79f7-130">ASP.NET Core coloca toda la lógica de inicio de la aplicación en un solo archivo, en el que los servicios necesarios y las dependencias pueden definirse y configuradas.</span><span class="sxs-lookup"><span data-stu-id="c79f7-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="c79f7-131">Reemplaza el *web.config* archivo con una característica de configuración flexible que puede utilizar una variedad de formatos de archivo, como JSON, así como las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="c79f7-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
