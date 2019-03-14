---
title: Detección de cambios con tokens de cambio en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar tokens de cambio para realizar el seguimiento de los cambios.
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/change-tokens
ms.openlocfilehash: 7ad580a7e999a4eae006ce5dd07cca0cbdbe9ab6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030292"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="c9966-103">Detección de cambios con tokens de cambio en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9966-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="c9966-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c9966-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c9966-105">Un *token de cambio* es un bloque de creación de bajo nivel y uso general que se usa para realizar el seguimiento de los cambios.</span><span class="sxs-lookup"><span data-stu-id="c9966-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="c9966-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/change-tokens/sample/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9966-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/change-tokens/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="c9966-107">Interfaz IChangeToken</span><span class="sxs-lookup"><span data-stu-id="c9966-107">IChangeToken interface</span></span>

<span data-ttu-id="c9966-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga notificaciones que indican que se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="c9966-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="c9966-109">`IChangeToken` reside en el espacio de nombres [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="c9966-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="c9966-110">En el caso de las aplicaciones que no usan el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), haga referencia al paquete NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="c9966-110">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="c9966-111">`IChangeToken` tiene dos propiedades:</span><span class="sxs-lookup"><span data-stu-id="c9966-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="c9966-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indica si el token genera devoluciones de llamada de forma proactiva.</span><span class="sxs-lookup"><span data-stu-id="c9966-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="c9966-113">Si `ActiveChangedCallbacks` se establece en `false`, nunca se llama a una devolución de llamada y la aplicación debe sondear `HasChanged` en busca de cambios.</span><span class="sxs-lookup"><span data-stu-id="c9966-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="c9966-114">También es posible que un token nunca se cancele si no se producen cambios o si se elimina o deshabilita el agente de escucha de cambios subyacente.</span><span class="sxs-lookup"><span data-stu-id="c9966-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="c9966-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) obtiene un valor que indica si se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="c9966-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="c9966-116">La interfaz tiene un método, [RegisterChangeCallback(Acción&lt;Objeto&gt;, Objeto)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), que registra una devolución de llamada que se invoca cuando el token ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="c9966-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="c9966-117">`HasChanged` se debe establecer antes de que se invoque la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="c9966-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="c9966-118">Clase ChangeToken</span><span class="sxs-lookup"><span data-stu-id="c9966-118">ChangeToken class</span></span>

<span data-ttu-id="c9966-119">`ChangeToken` es una clase estática que se usa para propagar notificaciones que indican que se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="c9966-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="c9966-120">`ChangeToken` reside en el espacio de nombres [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="c9966-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="c9966-121">En el caso de las aplicaciones que no usan el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), haga referencia al paquete NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="c9966-121">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="c9966-122">El método [OnChange(Función&lt;IChangeToken&gt;, Acción)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) de `ChangeToken` registra una `Action` que se llama cada vez que cambia el token:</span><span class="sxs-lookup"><span data-stu-id="c9966-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="c9966-123">`Func<IChangeToken>` genera el token.</span><span class="sxs-lookup"><span data-stu-id="c9966-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="c9966-124">Se llama a `Action` cuando cambia el token.</span><span class="sxs-lookup"><span data-stu-id="c9966-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="c9966-125">`ChangeToken` tiene una sobrecarga [OnChange&lt;TState&gt;(Función&lt;IChangeToken&gt;, Acción&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) que toma un parámetro `TState` adicional que se pasa a la `Action` de consumidor de token.</span><span class="sxs-lookup"><span data-stu-id="c9966-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="c9966-126">`OnChange` devuelve una interfaz [IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="c9966-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="c9966-127">Al llamar a [Dispose](/dotnet/api/system.idisposable.dispose) se detiene la escucha del token de futuras modificaciones y se liberan sus recursos.</span><span class="sxs-lookup"><span data-stu-id="c9966-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="c9966-128">Ejemplos de uso de tokens de cambio en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9966-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="c9966-129">Los tokens de cambio se usan en áreas principales de ASP.NET Core para la supervisión de cambios en los objetos:</span><span class="sxs-lookup"><span data-stu-id="c9966-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="c9966-130">Para supervisar los cambios en los archivos, el método [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) de [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) crea un `IChangeToken` para los archivos especificados o la carpeta que se va a supervisar.</span><span class="sxs-lookup"><span data-stu-id="c9966-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="c9966-131">Se pueden agregar tokens `IChangeToken` a las entradas de caché para desencadenar expulsiones de caché al producirse un cambio.</span><span class="sxs-lookup"><span data-stu-id="c9966-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="c9966-132">Para los cambios de `TOptions`, la implementación predeterminada [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) tiene una sobrecarga que acepta una o varias instancias de [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1).</span><span class="sxs-lookup"><span data-stu-id="c9966-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="c9966-133">Cada instancia devuelve un `IChangeToken` para registrar una devolución de llamada de notificación de cambio a fin de realizar el seguimiento de los cambios en las opciones.</span><span class="sxs-lookup"><span data-stu-id="c9966-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="c9966-134">Supervisión de los cambios de configuración</span><span class="sxs-lookup"><span data-stu-id="c9966-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="c9966-135">De forma predeterminada, las plantillas de ASP.NET Core usan [archivos de configuración de JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* y *appsettings.Production.json*) para cargar parámetros de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c9966-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="c9966-136">Estos archivos se configuran mediante el método de extensión [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) de [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder), que acepta un parámetro `reloadOnChange` (ASP.NET Core 1.1 y versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="c9966-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="c9966-137">`reloadOnChange` indica si la configuración se debe recargar en los cambios de archivo.</span><span class="sxs-lookup"><span data-stu-id="c9966-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="c9966-138">Vea esta configuración en el método de conveniencia [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) de [WebHost](/dotnet/api/microsoft.aspnetcore.webhost):</span><span class="sxs-lookup"><span data-stu-id="c9966-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="c9966-139">La configuración basada en archivo se representa por medio de [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="c9966-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="c9966-140">`FileConfigurationSource` usa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) para supervisar los archivos.</span><span class="sxs-lookup"><span data-stu-id="c9966-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="c9966-141">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) proporciona de forma predeterminada `IFileMonitor`, que usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) para supervisar los cambios del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="c9966-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="c9966-142">En la aplicación de ejemplo se muestran dos implementaciones para supervisar los cambios de configuración.</span><span class="sxs-lookup"><span data-stu-id="c9966-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="c9966-143">Si cambia el archivo *appsettings.json* o la versión del entorno del archivo, cada implementación ejecuta código personalizado.</span><span class="sxs-lookup"><span data-stu-id="c9966-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="c9966-144">La aplicación de ejemplo escribe un mensaje en la consola.</span><span class="sxs-lookup"><span data-stu-id="c9966-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="c9966-145">El `FileSystemWatcher` de un archivo de configuración puede desencadenar varias devoluciones de llamada de token para un único cambio del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="c9966-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="c9966-146">La implementación del ejemplo protege contra este problema mediante la comprobación del hash de archivo en los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="c9966-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="c9966-147">La comprobación del hash de archivo garantiza que al menos uno de los archivos de configuración ha cambiado antes de ejecutar el código personalizado.</span><span class="sxs-lookup"><span data-stu-id="c9966-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="c9966-148">En el ejemplo se usa el hash de archivo SHA1 (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="c9966-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="c9966-149">Se implementa un reintento con una interrupción exponencial.</span><span class="sxs-lookup"><span data-stu-id="c9966-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="c9966-150">El reintento aparece porque se puede producir un bloqueo de archivos que impida temporalmente calcular un hash nuevo en uno de los archivos.</span><span class="sxs-lookup"><span data-stu-id="c9966-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="c9966-151">Token de cambio de inicio simple</span><span class="sxs-lookup"><span data-stu-id="c9966-151">Simple startup change token</span></span>

<span data-ttu-id="c9966-152">Registre una devolución de llamada de `Action` de consumidor de token para las notificaciones de cambio en el token de recarga de configuración (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="c9966-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="c9966-153">`config.GetReloadToken()` proporciona el token.</span><span class="sxs-lookup"><span data-stu-id="c9966-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="c9966-154">La devolución de llamada es el método `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="c9966-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="c9966-155">El `state` de la devolución de llamada se usa para pasar el `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="c9966-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="c9966-156">Esto es útil para determinar el archivo JSON de configuración *appsettings* correcto que se va a supervisar, *appsettings.&lt;Entorno&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="c9966-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="c9966-157">Se usa el hash de archivo para evitar que se ejecute varias veces la instrucción `WriteConsole` debido a varias devoluciones de llamada de token cuando el archivo de configuración solo ha cambiado una vez.</span><span class="sxs-lookup"><span data-stu-id="c9966-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="c9966-158">Este sistema se ejecuta siempre que la aplicación esté en ejecución y el usuario no lo puede deshabilitar.</span><span class="sxs-lookup"><span data-stu-id="c9966-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="c9966-159">Supervisión de los cambios de configuración como servicio</span><span class="sxs-lookup"><span data-stu-id="c9966-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="c9966-160">En el ejemplo se implementa lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9966-160">The sample implements:</span></span>

* <span data-ttu-id="c9966-161">La supervisión del token de inicio básico.</span><span class="sxs-lookup"><span data-stu-id="c9966-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="c9966-162">La supervisión como servicio.</span><span class="sxs-lookup"><span data-stu-id="c9966-162">Monitoring as a service.</span></span>
* <span data-ttu-id="c9966-163">Un mecanismo para habilitar y deshabilitar la supervisión.</span><span class="sxs-lookup"><span data-stu-id="c9966-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="c9966-164">En el ejemplo se establece una interfaz `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="c9966-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="c9966-165">El constructor de la clase implementada, `ConfigurationMonitor`, registra una devolución de llamada para las notificaciones de cambio:</span><span class="sxs-lookup"><span data-stu-id="c9966-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="c9966-166">`config.GetReloadToken()` proporciona el token.</span><span class="sxs-lookup"><span data-stu-id="c9966-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="c9966-167">`InvokeChanged` es el método de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="c9966-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="c9966-168">El elemento `state` de esta instancia es una referencia a la instancia de `IConfigurationMonitor` que se usa para tener acceso al estado de supervisión.</span><span class="sxs-lookup"><span data-stu-id="c9966-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that is used to access the monitoring state.</span></span> <span data-ttu-id="c9966-169">Se usan dos propiedades:</span><span class="sxs-lookup"><span data-stu-id="c9966-169">Two properties are used:</span></span>

* <span data-ttu-id="c9966-170">`MonitoringEnabled` indica si la devolución de llamada debe ejecutar su código personalizado.</span><span class="sxs-lookup"><span data-stu-id="c9966-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="c9966-171">`CurrentState` describe el estado de supervisión actual para su uso en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="c9966-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="c9966-172">El método `InvokeChanged` es similar al enfoque anterior, excepto en que:</span><span class="sxs-lookup"><span data-stu-id="c9966-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="c9966-173">No ejecuta su código, a menos que `MonitoringEnabled` sea `true`.</span><span class="sxs-lookup"><span data-stu-id="c9966-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="c9966-174">Anota el `state` actual en su salida de `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="c9966-174">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="c9966-175">Una instancia de `ConfigurationMonitor` se registra como servicio en `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c9966-175">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="c9966-176">En la página Index se ofrece al usuario el control sobre la supervisión de la configuración.</span><span class="sxs-lookup"><span data-stu-id="c9966-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="c9966-177">La instancia de `IConfigurationMonitor` se inserta en `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="c9966-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="c9966-178">Un botón habilita y deshabilita la supervisión:</span><span class="sxs-lookup"><span data-stu-id="c9966-178">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="c9966-179">Cuando se desencadena `OnPostStartMonitoring`, se habilita la supervisión y se borra el estado actual.</span><span class="sxs-lookup"><span data-stu-id="c9966-179">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="c9966-180">Cuando se desencadena `OnPostStopMonitoring`, se deshabilita la supervisión y se establece el estado para reflejar que no se está realizando la supervisión.</span><span class="sxs-lookup"><span data-stu-id="c9966-180">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="c9966-181">Supervisión de los cambios de archivos en caché</span><span class="sxs-lookup"><span data-stu-id="c9966-181">Monitoring cached file changes</span></span>

<span data-ttu-id="c9966-182">El contenido de los archivos se puede almacenar en caché en memoria mediante [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="c9966-182">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="c9966-183">El almacenamiento en caché en memoria se describe en el tema [Cache in-memory](xref:performance/caching/memory) (Almacenamiento en caché en memoria).</span><span class="sxs-lookup"><span data-stu-id="c9966-183">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="c9966-184">Sin realizar pasos adicionales, como la implementación que se describe a continuación, si los datos de origen cambian, se devuelven datos *obsoletos* (no actualizados) de la caché.</span><span class="sxs-lookup"><span data-stu-id="c9966-184">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="c9966-185">Si no se tiene en cuenta el estado de un archivo de origen en caché cuando se renueva un período de [vencimiento variable](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration), se pueden crear datos en caché obsoletos.</span><span class="sxs-lookup"><span data-stu-id="c9966-185">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="c9966-186">En cada solicitud de los datos se renueva el período de vencimiento variable, pero el archivo nunca se vuelve a cargar en la caché.</span><span class="sxs-lookup"><span data-stu-id="c9966-186">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="c9966-187">Las características de la aplicación que usen el contenido en caché del archivo están sujetas a la posible recepción de contenido obsoleto.</span><span class="sxs-lookup"><span data-stu-id="c9966-187">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="c9966-188">El uso de tokens de cambio en un escenario de almacenamiento en caché de archivos evita contenido de archivo obsoleto en la caché.</span><span class="sxs-lookup"><span data-stu-id="c9966-188">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="c9966-189">En la aplicación de ejemplo se muestra una implementación del enfoque.</span><span class="sxs-lookup"><span data-stu-id="c9966-189">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="c9966-190">En el ejemplo se usa `GetFileContent` para:</span><span class="sxs-lookup"><span data-stu-id="c9966-190">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="c9966-191">Devolver el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="c9966-191">Return file content.</span></span>
* <span data-ttu-id="c9966-192">Implementar un algoritmo de reintento con interrupción exponencial para casos en los que un bloqueo de archivo impide temporalmente que se lea un archivo.</span><span class="sxs-lookup"><span data-stu-id="c9966-192">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="c9966-193">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="c9966-193">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="c9966-194">Se crea un `FileService` para administrar las búsquedas de archivos en caché.</span><span class="sxs-lookup"><span data-stu-id="c9966-194">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="c9966-195">La llamada al método `GetFileContent` del servicio intenta obtener el contenido de archivo de la caché en memoria y devolverlo al autor de la llamada (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="c9966-195">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="c9966-196">Si el contenido en caché no se encuentra mediante la clave de caché, se realizan las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="c9966-196">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="c9966-197">El contenido del archivo se obtiene mediante `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="c9966-197">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="c9966-198">Se obtiene un token de cambio del proveedor de archivos con [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="c9966-198">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="c9966-199">La devolución de llamada del token se desencadena cuando se modifica el archivo.</span><span class="sxs-lookup"><span data-stu-id="c9966-199">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="c9966-200">El contenido del archivo se almacena en caché con un período de [vencimiento variable](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration).</span><span class="sxs-lookup"><span data-stu-id="c9966-200">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="c9966-201">El token de cambio se adjunta con [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) para expulsar la entrada de caché si el archivo cambia mientras está almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="c9966-201">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="c9966-202">El `FileService` se registra en el contenedor de servicios junto con el servicio de almacenamiento en caché (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="c9966-202">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="c9966-203">El modelo de página carga el contenido del archivo mediante el servicio (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c9966-203">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="c9966-204">Clase CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="c9966-204">CompositeChangeToken class</span></span>

<span data-ttu-id="c9966-205">Para representar una o varias instancias de `IChangeToken` en un solo objeto, use la clase [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).</span><span class="sxs-lookup"><span data-stu-id="c9966-205">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="c9966-206">En el token compuesto, `HasChanged` notifica `true` si algún token representado `HasChanged` es `true`.</span><span class="sxs-lookup"><span data-stu-id="c9966-206">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="c9966-207">En el token compuesto, `ActiveChangeCallbacks` notifica `true` si algún token representado `ActiveChangeCallbacks` es `true`.</span><span class="sxs-lookup"><span data-stu-id="c9966-207">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="c9966-208">Si se producen varios eventos de cambio simultáneos, la devolución de llamada de cambio compuesto se invoca exactamente una vez.</span><span class="sxs-lookup"><span data-stu-id="c9966-208">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9966-209">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c9966-209">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
