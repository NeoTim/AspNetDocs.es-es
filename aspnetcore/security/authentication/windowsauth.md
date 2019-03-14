---
title: Configurar la autenticación de Windows en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo configurar la autenticación de Windows en ASP.NET Core, usar IIS Express, IIS y HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/25/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 15fc41efba77f88fc8129f875b85836ac1b5f886
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031942"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="a1a8a-103">Configurar la autenticación de Windows en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1a8a-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="a1a8a-104">Por [Scott Addie](https://twitter.com/Scott_Addie) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a1a8a-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a1a8a-105">[Autenticación de Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) puede configurarse para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index) o [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="a1a8a-106">Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="a1a8a-107">Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o cuentas de Windows para identificar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="a1a8a-108">Autenticación de Windows se adapta mejor a los entornos de intranet donde los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="a1a8a-109">Habilitar la autenticación de Windows en una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1a8a-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="a1a8a-110">El **aplicación Web** plantilla disponible a través de Visual Studio o la CLI de .NET Core puede configurarse para admitir la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1a8a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1a8a-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="a1a8a-112">Usa la plantilla de aplicación de autenticación de Windows para un nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="a1a8a-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="a1a8a-113">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-113">In Visual Studio:</span></span>

1. <span data-ttu-id="a1a8a-114">Cree un nuevo **aplicación Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-114">Create a new **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="a1a8a-115">Seleccione **aplicación Web** en la lista de plantillas.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-115">Select **Web Application** from the list of templates.</span></span>
1. <span data-ttu-id="a1a8a-116">Seleccione el **Cambiar autenticación** y seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-116">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="a1a8a-117">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-117">Run the app.</span></span> <span data-ttu-id="a1a8a-118">El nombre de usuario aparece en la interfaz de usuario de la aplicación representada.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-118">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="a1a8a-119">Configuración manual de un proyecto existente</span><span class="sxs-lookup"><span data-stu-id="a1a8a-119">Manual configuration for an existing project</span></span>

<span data-ttu-id="a1a8a-120">Las propiedades del proyecto le permiten habilitar la autenticación de Windows y deshabilitar la autenticación anónima:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-120">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="a1a8a-121">Haga clic en el proyecto en Visual Studio **el Explorador de soluciones** y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-121">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="a1a8a-122">Seleccione la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-122">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="a1a8a-123">Desactive la casilla de verificación **habilitar la autenticación anónima**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-123">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="a1a8a-124">Seleccione la casilla de verificación **habilitar la autenticación de Windows**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-124">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="a1a8a-125">Como alternativa, se pueden configurar las propiedades en el `iisSettings` nodo de la *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-125">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a1a8a-126">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="a1a8a-126">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a1a8a-127">Use la **Windows autenticación** plantilla de aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-127">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="a1a8a-128">Ejecute el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando con el `webapp` argumento (aplicación Web de ASP.NET Core) y `--auth Windows` cambiar:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-128">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="a1a8a-129">Cuando se modifica un proyecto existente, compruebe que el archivo de proyecto incluye una referencia de paquete para el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) **o** el [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-129">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="a1a8a-130">Habilitar la autenticación de Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-130">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="a1a8a-131">IIS usa el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-131">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="a1a8a-132">Autenticación de Windows se configura para IIS a través de la *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-132">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="a1a8a-133">Las siguientes secciones muestran cómo:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-133">The following sections show how to:</span></span>

* <span data-ttu-id="a1a8a-134">Proporcionar una variable local *web.config* archivo que activa la autenticación de Windows en el servidor cuando se implementa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-134">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="a1a8a-135">Use el Administrador de IIS para configurar el *web.config* archivos de una aplicación de ASP.NET Core que ya se ha implementado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-135">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="a1a8a-136">Configuración de IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-136">IIS configuration</span></span>

<span data-ttu-id="a1a8a-137">Si aún no lo ha hecho, habilite IIS para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-137">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="a1a8a-138">Para obtener más información, consulta <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-138">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="a1a8a-139">Habilite el servicio de rol IIS para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-139">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="a1a8a-140">Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-140">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="a1a8a-141">Middleware de integración de IIS está configurado para autenticar automáticamente las solicitudes de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-141">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="a1a8a-142">Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: Las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-142">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="a1a8a-143">El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-143">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="a1a8a-144">Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-144">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="a1a8a-145">Crear un nuevo sitio IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-145">Create a new IIS site</span></span>

<span data-ttu-id="a1a8a-146">Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-146">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="a1a8a-147">Habilitar la autenticación de Windows para la aplicación en IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-147">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="a1a8a-148">Use **cualquier** de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-148">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="a1a8a-149">[Configuración de desarrollo antes de publicar la aplicación](#development-side-configuration-with-a-local-webconfig-file) (*recomendado*)</span><span class="sxs-lookup"><span data-stu-id="a1a8a-149">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="a1a8a-150">Configuración del servidor después de publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a1a8a-150">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="a1a8a-151">Configuración de desarrollo con un archivo local web.config</span><span class="sxs-lookup"><span data-stu-id="a1a8a-151">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="a1a8a-152">Realice los pasos siguientes **antes** [publicar e implementar el proyecto](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-152">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="a1a8a-153">Agregue el siguiente *web.config* archivo a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-153">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="a1a8a-154">Cuando se publica el proyecto mediante el SDK (sin el `<IsTransformWebConfigDisabled>` propiedad establecida en `true` en el archivo de proyecto), publicado *web.config* archivo incluye la `<location><system.webServer><security><authentication>` sección.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-154">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="a1a8a-155">Para obtener más información sobre la `<IsTransformWebConfigDisabled>` propiedad, vea <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-155">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="a1a8a-156">Configuración del servidor con el Administrador de IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-156">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="a1a8a-157">Realice los pasos siguientes **después** [publicar e implementar el proyecto](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-157">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="a1a8a-158">En el Administrador de IIS, seleccione el sitio IIS en el **sitios** nodo de la **conexiones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-158">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="a1a8a-159">Haga doble clic en **autenticación** en el **IIS** área.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-159">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="a1a8a-160">Seleccione **autenticación anónima**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-160">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="a1a8a-161">Seleccione **deshabilitar** en el **acciones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-161">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="a1a8a-162">Seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-162">Select **Windows Authentication**.</span></span> <span data-ttu-id="a1a8a-163">Seleccione **habilitar** en el **acciones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-163">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="a1a8a-164">Cuando se realizan estas acciones, el Administrador de IIS modifica la aplicación *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-164">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="a1a8a-165">Un `<system.webServer><security><authentication>` se agrega el nodo con la configuración actualizada para `anonymousAuthentication` y `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-165">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="a1a8a-166">El `<system.webServer>` sección agregada a la *web.config* archivo por el Administrador de IIS está fuera de la aplicación `<location>` sección agregada por el SDK de .NET Core cuando se publique la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-166">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="a1a8a-167">Dado que se agrega la sección fuera de la `<location>` nodo, la configuración se hereda por cualquier [las aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-applications) a la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-167">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="a1a8a-168">Para impedir la herencia, mover agregado `<security>` sección dentro de la `<location><system.webServer>` sección suministradas por el SDK.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-168">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="a1a8a-169">Cuando se usa el Administrador de IIS para agregar la configuración de IIS, solo afecta a la aplicación *web.config* archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-169">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="a1a8a-170">Una implementación posterior de la aplicación podría sobrescribir la configuración en el servidor si la copia del servidor de *web.config* se sustituye por el proyecto *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-170">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="a1a8a-171">Use **cualquier** de los métodos siguientes para administrar la configuración:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-171">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="a1a8a-172">Use el Administrador de IIS para restablecer la configuración en el *web.config* archivo después de que el archivo se sobrescribe en la implementación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-172">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="a1a8a-173">Agregar un *archivo web.config* a la aplicación localmente con la configuración.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-173">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="a1a8a-174">Para obtener más información, consulte el [configuración del desarrollo](#development-side-configuration-with-a-local-webconfig-file) sección.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-174">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="a1a8a-175">Publicar e implementar el proyecto a la carpeta del sitio IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-175">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="a1a8a-176">Con Visual Studio o la CLI de .NET Core, publicar e implementar la aplicación en la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-176">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="a1a8a-177">Para obtener más información sobre el hospedaje con IIS, publicación e implementación, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-177">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="a1a8a-178">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="a1a8a-178">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="a1a8a-179">Inicie la aplicación para comprobar que funciona la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-179">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="a1a8a-180">Habilitar la autenticación de Windows con HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a1a8a-180">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="a1a8a-181">Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir los escenarios Auto-hospedados en Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-181">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="a1a8a-182">El ejemplo siguiente configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-182">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="a1a8a-183">HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-183">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="a1a8a-184">La autenticación de modo usuario no se admite con Kerberos y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-184">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="a1a8a-185">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-185">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="a1a8a-186">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-186">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="a1a8a-187">HTTP.sys no se admite en Nano Server versión 1709 o posterior.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-187">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="a1a8a-188">Para usar autenticación de Windows y HTTP.sys con Nano Server, use un [contenedor de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-188">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="a1a8a-189">Para obtener más información sobre Server Core, vea [¿qué es la opción de instalación Server Core en Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="a1a8a-189">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="a1a8a-190">Trabajar con la autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="a1a8a-190">Work with Windows Authentication</span></span>

<span data-ttu-id="a1a8a-191">Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-191">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="a1a8a-192">Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-192">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="a1a8a-193">No permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="a1a8a-193">Disallow anonymous access</span></span>

<span data-ttu-id="a1a8a-194">Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-194">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="a1a8a-195">Si el sitio IIS (o HTTP.sys) está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-195">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="a1a8a-196">Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-196">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="a1a8a-197">Permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="a1a8a-197">Allow anonymous access</span></span>

<span data-ttu-id="a1a8a-198">Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-198">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="a1a8a-199">El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requiera autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-199">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="a1a8a-200">El `[AllowAnonymous]` invalidaciones de atributo `[Authorize]` atributo uso dentro de las aplicaciones que permite el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-200">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="a1a8a-201">Consulte [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso del atributo.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-201">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="a1a8a-202">En ASP.NET Core 2.x, el `[Authorize]` atributo requiere configuración adicional en *Startup.cs* Desafíe a las solicitudes anónimas para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-202">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="a1a8a-203">La configuración recomendada varía ligeramente según el servidor web que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-203">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="a1a8a-204">De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="a1a8a-205">El [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".</span><span class="sxs-lookup"><span data-stu-id="a1a8a-205">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="a1a8a-206">IIS</span><span class="sxs-lookup"><span data-stu-id="a1a8a-206">IIS</span></span>

<span data-ttu-id="a1a8a-207">Si usa IIS, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-207">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="a1a8a-208">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a1a8a-208">HTTP.sys</span></span>

<span data-ttu-id="a1a8a-209">Si usa HTTP.sys, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="a1a8a-209">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="a1a8a-210">Suplantación</span><span class="sxs-lookup"><span data-stu-id="a1a8a-210">Impersonation</span></span>

<span data-ttu-id="a1a8a-211">ASP.NET Core no implementa la suplantación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-211">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="a1a8a-212">Las aplicaciones se ejecutan con la identidad de la aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-212">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="a1a8a-213">Si necesita realizar explícitamente una acción en nombre de un usuario, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) en un [software intermedio alineado terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-213">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="a1a8a-214">Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-214">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="a1a8a-215">`RunImpersonated` no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-215">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="a1a8a-216">Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-216">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="a1a8a-217">Transformaciones de notificaciones</span><span class="sxs-lookup"><span data-stu-id="a1a8a-217">Claims transformations</span></span>

<span data-ttu-id="a1a8a-218">Al hospedar con el modo en proceso IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> no se llama internamente para inicializar un usuario.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-218">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a1a8a-219">Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-219">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a1a8a-220">Para obtener más información y un ejemplo de código que activa las transformaciones de notificaciones cuando se hospedan en proceso, consulte <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="a1a8a-220">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
