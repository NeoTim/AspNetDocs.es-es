---
uid: config-builder
title: Generadores de configuración de ASP.NET
author: rick-anderson
description: Obtenga información sobre cómo obtener datos de configuración de orígenes que no sean web.config valores de orígenes externos.
ms.author: riande
ms.date: 7/17/2020
msc.type: content
ms.openlocfilehash: 1f95efcceb2ecf33fece12174cecf65cd8b27675
ms.sourcegitcommit: 000cbcd1de66fe8a766f203ef2d6f009e4435f53
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2020
ms.locfileid: "86424805"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="a3b8a-103">Generadores de configuración de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a3b8a-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="a3b8a-104">Por [Stephen Molloy](https://github.com/StephenMolloy) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a3b8a-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a3b8a-105">Los generadores de configuración proporcionan un mecanismo moderno y ágil para que las aplicaciones de ASP.NET obtengan valores de configuración de orígenes externos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="a3b8a-106">Generadores de configuración:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-106">Configuration builders:</span></span>

* <span data-ttu-id="a3b8a-107">Están disponibles en .NET Framework 4.7.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="a3b8a-108">Proporcionar un mecanismo flexible para leer los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="a3b8a-109">Solucione algunas de las necesidades básicas de las aplicaciones cuando se mueven a un contenedor y a un entorno centrado en la nube.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="a3b8a-110">Se puede usar para mejorar la protección de los datos de configuración mediante el dibujo de orígenes no disponibles anteriormente (por ejemplo, Azure Key Vault y variables de entorno) en el sistema de configuración de .NET.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="a3b8a-111">Generadores de configuración de clave/valor</span><span class="sxs-lookup"><span data-stu-id="a3b8a-111">Key/value configuration builders</span></span>

<span data-ttu-id="a3b8a-112">Un escenario común que pueden controlar los generadores de configuraciones es proporcionar un mecanismo básico de sustitución de clave-valor para las secciones de configuración que siguen un patrón de clave y valor.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="a3b8a-113">El concepto .NET Framework de ConfigurationBuilders no se limita a secciones o patrones de configuración específicos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="a3b8a-114">Sin embargo, muchos de los generadores de configuración de `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionan dentro del patrón de clave-valor.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="a3b8a-115">Configuración de los generadores de configuración de clave/valor</span><span class="sxs-lookup"><span data-stu-id="a3b8a-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="a3b8a-116">La siguiente configuración se aplica a todos los generadores de configuración de clave y valor en `Microsoft.Configuration.ConfigurationBuilders` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="a3b8a-117">Mode</span><span class="sxs-lookup"><span data-stu-id="a3b8a-117">Mode</span></span>

<span data-ttu-id="a3b8a-118">Los generadores de configuración usan un origen externo de la información de clave y valor para rellenar los elementos de clave y valor seleccionados del sistema de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="a3b8a-119">En concreto, `<appSettings/>` las `<connectionStrings/>` secciones y reciben un tratamiento especial de los generadores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="a3b8a-120">Los generadores funcionan en tres modos:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-120">The builders work in three modes:</span></span>

* <span data-ttu-id="a3b8a-121">`Strict`: El modo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-121">`Strict` - The default mode.</span></span> <span data-ttu-id="a3b8a-122">En este modo, el generador de configuración solo funciona en secciones de configuración con clave y valor bien conocidas.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="a3b8a-123">`Strict`el modo enumera cada clave de la sección.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="a3b8a-124">Si se encuentra una clave coincidente en el origen externo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="a3b8a-125">Los generadores de configuración reemplazan el valor de la sección de configuración resultante por el valor del origen externo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="a3b8a-126">`Greedy`: Este modo está estrechamente relacionado con el `Strict` modo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="a3b8a-127">En lugar de limitarse a las claves que ya existen en la configuración original:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="a3b8a-128">Los generadores de configuración agregan todos los pares clave-valor del origen externo a la sección de configuración resultante.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="a3b8a-129">`Expand`: Opera en el XML sin formato antes de analizarlo en un objeto de sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="a3b8a-130">Puede considerarse como una expansión de tokens en una cadena.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="a3b8a-131">Cualquier parte de la cadena XML sin formato que coincida con el patrón `${token}` es candidata para la expansión de tokens.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="a3b8a-132">Si no se encuentra ningún valor correspondiente en el origen externo, no se cambia el token.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="a3b8a-133">Los generadores de este modo no se limitan a `<appSettings/>` las `<connectionStrings/>` secciones y.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="a3b8a-134">El siguiente marcado de *web.config* habilita [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en `Strict` modo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="a3b8a-135">En el código siguiente `<appSettings/>` se lee y `<connectionStrings/>` se muestra en el archivo de *web.config* anterior:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="a3b8a-136">El código anterior establecerá los valores de propiedad en:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="a3b8a-137">Valores del archivo de *web.config* si las claves no se establecen en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="a3b8a-138">Los valores de la variable de entorno, si se establece.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="a3b8a-139">Por ejemplo, contendrá `ServiceID` :</span><span class="sxs-lookup"><span data-stu-id="a3b8a-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="a3b8a-140">"Valor de ServiceID desde web.config", si no se establece la variable de entorno `ServiceID` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="a3b8a-141">El valor de la `ServiceID` variable de entorno, si se establece.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="a3b8a-142">En la imagen siguiente se muestran las `<appSettings/>` claves o los valores del conjunto de archivos *web.config* anterior en el editor de entorno:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor de entorno](config-builder/static/env.png)

<span data-ttu-id="a3b8a-144">Nota: es posible que tenga que salir y reiniciar Visual Studio para ver los cambios en las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="a3b8a-145">Control de prefijos</span><span class="sxs-lookup"><span data-stu-id="a3b8a-145">Prefix handling</span></span>

<span data-ttu-id="a3b8a-146">Los prefijos de clave pueden simplificar la configuración de claves porque:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="a3b8a-147">La configuración .NET Framework es compleja y está anidada.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="a3b8a-148">Los orígenes de clave/valor externos suelen ser básicos y planos por naturaleza.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="a3b8a-149">Por ejemplo, las variables de entorno no están anidadas.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="a3b8a-150">Use cualquiera de los métodos siguientes para insertar `<appSettings/>` y `<connectionStrings/>` en la configuración mediante variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="a3b8a-151">Con `EnvironmentConfigBuilder` en el modo predeterminado `Strict` y los nombres de clave adecuados en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="a3b8a-152">El código y el marcado anteriores tienen este enfoque.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="a3b8a-153">Con este enfoque **no** puede tener claves con el mismo nombre en `<appSettings/>` y `<connectionStrings/>` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="a3b8a-154">Use dos `EnvironmentConfigBuilder` s en `Greedy` modo con prefijos distintos y `stripPrefix` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="a3b8a-155">Con este enfoque, la aplicación puede leer `<appSettings/>` y `<connectionStrings/>` sin necesidad de actualizar el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="a3b8a-156">En la sección siguiente, [stripPrefix](#stripprefix), se muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="a3b8a-157">Use dos `EnvironmentConfigBuilder` s en `Greedy` modo con prefijos distintos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="a3b8a-158">Con este enfoque no puede tener nombres de clave duplicados, ya que los nombres de clave deben ser diferentes en el prefijo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="a3b8a-159">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="a3b8a-160">Con el marcado anterior, se puede usar el mismo origen de clave y valor sin formato para rellenar la configuración de dos secciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="a3b8a-161">En la imagen siguiente se muestran las `<appSettings/>` `<connectionStrings/>` claves y los valores del conjunto de archivos *web.config* anterior en el editor de entorno:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor de entorno](config-builder/static/prefix.png)

<span data-ttu-id="a3b8a-163">El código siguiente lee las `<appSettings/>` `<connectionStrings/>` claves y los valores que contiene el archivo de *web.config* anterior:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="a3b8a-164">El código anterior establecerá los valores de propiedad en:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="a3b8a-165">Valores del archivo de *web.config* si las claves no se establecen en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="a3b8a-166">Los valores de la variable de entorno, si se establece.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="a3b8a-167">Por ejemplo, con el archivo de *web.config* anterior, las claves/valores de la imagen del editor del entorno anterior y el código anterior, se establecen los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="a3b8a-168">Clave</span><span class="sxs-lookup"><span data-stu-id="a3b8a-168">Key</span></span>              | <span data-ttu-id="a3b8a-169">Valor</span><span class="sxs-lookup"><span data-stu-id="a3b8a-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="a3b8a-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="a3b8a-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="a3b8a-171">AppSetting_ServiceID de variables de env</span><span class="sxs-lookup"><span data-stu-id="a3b8a-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="a3b8a-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="a3b8a-172">AppSetting_default</span></span>            | <span data-ttu-id="a3b8a-173">AppSetting_default valor de env</span><span class="sxs-lookup"><span data-stu-id="a3b8a-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="a3b8a-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="a3b8a-174">ConnStr_default</span></span>         | <span data-ttu-id="a3b8a-175">ConnStr_default Val from env</span><span class="sxs-lookup"><span data-stu-id="a3b8a-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="a3b8a-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="a3b8a-176">stripPrefix</span></span>

<span data-ttu-id="a3b8a-177">`stripPrefix`: booleano, su valor predeterminado es `false` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="a3b8a-178">El marcado XML anterior separa la configuración de la aplicación de las cadenas de conexión, pero requiere que todas las claves del archivo *web.config* usen el prefijo especificado.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="a3b8a-179">Por ejemplo, el prefijo `AppSetting` debe agregarse a la `ServiceID` clave ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="a3b8a-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="a3b8a-180">Con `stripPrefix` , el prefijo no se utiliza en el archivo de *web.config* .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="a3b8a-181">El prefijo es necesario en el origen del generador de configuraciones (por ejemplo, en el entorno). Se prevé que la mayoría de los desarrolladores usarán `stripPrefix` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="a3b8a-182">Normalmente, las aplicaciones quitan el prefijo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="a3b8a-183">En el siguiente *web.config* se quita el prefijo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="a3b8a-184">En el archivo de *web.config* anterior, la `default` clave está en `<appSettings/>` y `<connectionStrings/>` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="a3b8a-185">En la imagen siguiente se muestran las `<appSettings/>` `<connectionStrings/>` claves y los valores del conjunto de archivos *web.config* anterior en el editor de entorno:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor de entorno](config-builder/static/prefix.png)

<span data-ttu-id="a3b8a-187">El código siguiente lee las `<appSettings/>` `<connectionStrings/>` claves y los valores que contiene el archivo de *web.config* anterior:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="a3b8a-188">El código anterior establecerá los valores de propiedad en:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="a3b8a-189">Valores del archivo de *web.config* si las claves no se establecen en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="a3b8a-190">Los valores de la variable de entorno, si se establece.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="a3b8a-191">Por ejemplo, con el archivo de *web.config* anterior, las claves/valores de la imagen del editor del entorno anterior y el código anterior, se establecen los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="a3b8a-192">Clave</span><span class="sxs-lookup"><span data-stu-id="a3b8a-192">Key</span></span>              | <span data-ttu-id="a3b8a-193">Valor</span><span class="sxs-lookup"><span data-stu-id="a3b8a-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="a3b8a-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="a3b8a-194">ServiceID</span></span>           | <span data-ttu-id="a3b8a-195">AppSetting_ServiceID de variables de env</span><span class="sxs-lookup"><span data-stu-id="a3b8a-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="a3b8a-196">default</span><span class="sxs-lookup"><span data-stu-id="a3b8a-196">default</span></span>            | <span data-ttu-id="a3b8a-197">AppSetting_default valor de env</span><span class="sxs-lookup"><span data-stu-id="a3b8a-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="a3b8a-198">default</span><span class="sxs-lookup"><span data-stu-id="a3b8a-198">default</span></span>         | <span data-ttu-id="a3b8a-199">ConnStr_default Val from env</span><span class="sxs-lookup"><span data-stu-id="a3b8a-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="a3b8a-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="a3b8a-200">tokenPattern</span></span>

<span data-ttu-id="a3b8a-201">`tokenPattern`: String, el valor predeterminado es.`@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="a3b8a-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="a3b8a-202">El `Expand` comportamiento de los generadores busca en el XML sin formato los tokens que se parezcan a `${token}` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="a3b8a-203">La búsqueda se realiza con la expresión regular predeterminada `@"\$\{(\w+)\}"` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="a3b8a-204">El conjunto de caracteres que coincide `\w` es más estricto que XML y muchos orígenes de configuración permiten.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="a3b8a-205">Use `tokenPattern` cuando haya más caracteres de `@"\$\{(\w+)\}"` los necesarios en el nombre del token.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="a3b8a-206">`tokenPattern`String@</span><span class="sxs-lookup"><span data-stu-id="a3b8a-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="a3b8a-207">Permite a los desarrolladores cambiar la expresión regular que se usa para la coincidencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="a3b8a-208">No se realiza ninguna validación para asegurarse de que se trata de una expresión regular correcta y no peligrosa.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="a3b8a-209">Debe contener un grupo de capturas.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-209">It must contain a capture group.</span></span> <span data-ttu-id="a3b8a-210">La expresión regular completa debe coincidir con el token completo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="a3b8a-211">La primera captura debe ser el nombre del token que se va a buscar en el origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="a3b8a-212">Generadores de configuración en Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="a3b8a-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="a3b8a-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="a3b8a-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="a3b8a-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="a3b8a-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="a3b8a-215">Es la más sencilla de los generadores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="a3b8a-216">Lee los valores del entorno.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-216">Reads values from the environment.</span></span>
* <span data-ttu-id="a3b8a-217">No tiene ninguna opción de configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="a3b8a-218">El `name` valor del atributo es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="a3b8a-219">**Nota:** En un entorno de contenedor de Windows, las variables que se establecen en tiempo de ejecución solo se insertan en el entorno de proceso EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="a3b8a-220">Las aplicaciones que se ejecutan como un servicio o un proceso sin punto de entrada no recogen estas variables a menos que se inserten de otro modo a través de un mecanismo del contenedor.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="a3b8a-221">En [IIS](https://github.com/Microsoft/iis-docker/pull/41) / el caso de los contenedores basados en[ASP.net](https://github.com/Microsoft/aspnet-docker)de IIS, la versión actual de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) lo controla solo en *DefaultAppPool* .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="a3b8a-222">Otras variantes de contenedor basadas en Windows pueden necesitar desarrollar su propio mecanismo de inyección para los procesos que no son de punto de entrada.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="a3b8a-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="a3b8a-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="a3b8a-224">Nunca almacene contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="a3b8a-225">Los secretos de producción no deben usarse para el desarrollo o las pruebas.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="a3b8a-226">Este generador de configuración proporciona una característica similar a [ASP.net Core administrador de secretos](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a3b8a-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="a3b8a-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) se puede usar en proyectos de .NET Framework, pero debe especificarse un archivo de secretos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="a3b8a-228">Como alternativa, puede definir la `UserSecretsId` propiedad en el archivo de proyecto y crear el archivo de secretos sin formato en la ubicación correcta para la lectura.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="a3b8a-229">Para mantener las dependencias externas fuera del proyecto, el archivo secreto tiene un formato XML.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="a3b8a-230">El formato XML es un detalle de implementación y no se debe confiar en el formato.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="a3b8a-231">Si necesita compartir un *secrets.jsen* el archivo con proyectos de .net Core, considere la posibilidad de usar [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span><span class="sxs-lookup"><span data-stu-id="a3b8a-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span></span> <span data-ttu-id="a3b8a-232">El `SimpleJsonConfigBuilder` formato de .net Core también debe considerarse un detalle de implementación sujeto a cambios.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="a3b8a-233">Atributos de configuración para `UserSecretsConfigBuilder` :</span><span class="sxs-lookup"><span data-stu-id="a3b8a-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="a3b8a-234">`userSecretsId`: Es el método preferido para identificar un archivo de secretos XML.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="a3b8a-235">Funciona de forma similar a .NET Core, que usa una `UserSecretsId` propiedad de proyecto para almacenar este identificador.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="a3b8a-236">La cadena debe ser única, por lo que no es necesario que sea un GUID.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="a3b8a-237">Con este atributo, `UserSecretsConfigBuilder` busca en una ubicación local conocida ( `%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml` ) un archivo de secretos que pertenezca a este identificador.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="a3b8a-238">`userSecretsFile`: Un atributo opcional que especifica el archivo que contiene los secretos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="a3b8a-239">El `~` carácter se puede usar en el inicio para hacer referencia a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="a3b8a-240">Este atributo o el `userSecretsId` atributo son obligatorios.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="a3b8a-241">Si se especifican ambos, `userSecretsFile` tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="a3b8a-242">`optional`: booleano, valor predeterminado `true` : impide una excepción si no se encuentra el archivo de secretos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="a3b8a-243">El `name` valor del atributo es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="a3b8a-244">El archivo de secretos tiene el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="a3b8a-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="a3b8a-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="a3b8a-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) Lee los valores almacenados en el [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="a3b8a-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="a3b8a-247">`vaultName`es obligatorio (el nombre del almacén o un URI para el almacén).</span><span class="sxs-lookup"><span data-stu-id="a3b8a-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="a3b8a-248">Los otros atributos permiten controlar el almacén al que se va a conectar, pero solo son necesarios si la aplicación no se está ejecutando en un entorno que funcione con `Microsoft.Azure.Services.AppAuthentication` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="a3b8a-249">La biblioteca de autenticación de servicios de Azure se usa para seleccionar automáticamente la información de conexión del entorno de ejecución si es posible.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="a3b8a-250">Puede invalidar la recogida automática de información de conexión proporcionando una cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="a3b8a-251">`vaultName`-Requerido si `uri` no se proporciona.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="a3b8a-252">Especifica el nombre del almacén en la suscripción de Azure desde el que se van a leer pares de clave y valor.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="a3b8a-253">`connectionString`: Una cadena de conexión que puede usar [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="a3b8a-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="a3b8a-254">`uri`: Se conecta a otros proveedores de Key Vault con el `uri` valor especificado.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="a3b8a-255">Si no se especifica, Azure ( `vaultName` ) es el proveedor de almacén.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="a3b8a-256">`version`-Azure Key Vault proporciona una característica de control de versiones para secretos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="a3b8a-257">Si `version` se especifica, el generador solo recupera los secretos que coinciden con esta versión.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="a3b8a-258">`preloadSecretNames`: De forma predeterminada, este generador consulta **todos** los nombres de clave en el almacén de claves cuando se inicializa.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="a3b8a-259">Para evitar la lectura de todos los valores de clave, establezca este atributo en `false` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="a3b8a-260">Si se establece en, se `false` leerán los secretos de uno en uno.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="a3b8a-261">Leer secretos de uno en uno puede ser útil si el almacén permite el acceso "Get" pero no "list".</span><span class="sxs-lookup"><span data-stu-id="a3b8a-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="a3b8a-262">**Nota:** Cuando `Greedy` se usa el modo, `preloadSecretNames` debe ser `true` (el valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="a3b8a-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="a3b8a-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="a3b8a-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="a3b8a-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) es un generador de configuración básico que usa los archivos de un directorio como origen de valores.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="a3b8a-265">El nombre de un archivo es la clave y el contenido es el valor.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="a3b8a-266">Este generador de configuración puede ser útil cuando se ejecuta en un entorno de contenedor organizado.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="a3b8a-267">Los sistemas como Docker Swarm y Kubernetes proporcionan `secrets` a sus contenedores de Windows orquestados en función de la clave por archivo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="a3b8a-268">Detalles del atributo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-268">Attribute details:</span></span>

* <span data-ttu-id="a3b8a-269">`directoryPath` - Requerido.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-269">`directoryPath` - Required.</span></span> <span data-ttu-id="a3b8a-270">Especifica una ruta de acceso en la que buscar valores.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="a3b8a-271">Docker para Windows secretos se almacenan de forma predeterminada en el directorio *C:\ProgramData\Docker\secrets*</span><span class="sxs-lookup"><span data-stu-id="a3b8a-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="a3b8a-272">`ignorePrefix`-Se excluyen los archivos que comienzan con este prefijo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="a3b8a-273">Se establece en "ignore." de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="a3b8a-274">`keyDelimiter`-El valor predeterminado es `null` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="a3b8a-275">Si se especifica, el generador de configuración atraviesa varios niveles del directorio y crea nombres de clave con este delimitador.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="a3b8a-276">Si este valor es `null` , el generador de configuración solo busca en el nivel superior del directorio.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="a3b8a-277">`optional`-El valor predeterminado es `false` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="a3b8a-278">Especifica si el generador de configuración debe producir errores si el directorio de origen no existe.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="a3b8a-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="a3b8a-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="a3b8a-280">Nunca almacene contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="a3b8a-281">Los secretos de producción no deben usarse para el desarrollo o las pruebas.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="a3b8a-282">Los proyectos de .NET Core usan frecuentemente archivos JSON para la configuración.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="a3b8a-283">El generador de [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) permite usar archivos JSON de .net Core en el .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="a3b8a-284">Este generador de configuración proporciona una asignación básica desde un origen de valor o clave sin formato en áreas de clave y valor específicas de la configuración de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="a3b8a-285">Este generador de configuración **no** proporciona configuraciones jerárquicas.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="a3b8a-286">El archivo de copia de seguridad JSON es similar a un diccionario, no a un objeto jerárquico complejo.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="a3b8a-287">Se puede usar un archivo jerárquico de varios niveles.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="a3b8a-288">Este proveedor `flatten` es la profundidad anexando el nombre de la propiedad en cada nivel usando `:` como un delimitador.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="a3b8a-289">Detalles del atributo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-289">Attribute details:</span></span>

* <span data-ttu-id="a3b8a-290">`jsonFile` - Requerido.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-290">`jsonFile` - Required.</span></span> <span data-ttu-id="a3b8a-291">Especifica el archivo JSON del que se va a leer.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="a3b8a-292">El `~` carácter se puede usar en el inicio para hacer referencia a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="a3b8a-293">`optional`-Boolean, el valor predeterminado es `true` .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="a3b8a-294">Evita producir excepciones si no se encuentra el archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="a3b8a-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="a3b8a-296">`Flat` es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-296">`Flat` is the default.</span></span> <span data-ttu-id="a3b8a-297">Cuando `jsonMode` es `Flat` , el archivo JSON es un origen de valor o clave plana único.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="a3b8a-298">`EnvironmentConfigBuilder`Y `AzureKeyVaultConfigBuilder` también son orígenes de clave/valor sin formato.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="a3b8a-299">Cuando `SimpleJsonConfigBuilder` se configura en el `Sectional` modo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="a3b8a-300">El archivo JSON se divide conceptualmente solo en el nivel superior en varios diccionarios.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="a3b8a-301">Cada uno de los diccionarios solo se aplica a la sección de configuración que coincide con el nombre de la propiedad de nivel superior asociado a ellos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="a3b8a-302">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="configuration-builders-order"></a><span data-ttu-id="a3b8a-303">Orden de los generadores de configuración</span><span class="sxs-lookup"><span data-stu-id="a3b8a-303">Configuration builders order</span></span>

<span data-ttu-id="a3b8a-304">Consulte [orden de ejecución de ConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) en el repositorio de github de [ASPNET/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) .</span><span class="sxs-lookup"><span data-stu-id="a3b8a-304">See [ConfigurationBuilders Order of Execution](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) in the [aspnet/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) GitHub repository.</span></span>

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="a3b8a-305">Implementar un generador de configuración de clave/valor personalizado</span><span class="sxs-lookup"><span data-stu-id="a3b8a-305">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="a3b8a-306">Si los generadores de configuración no satisfacen sus necesidades, puede escribir uno personalizado.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-306">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="a3b8a-307">La `KeyValueConfigBuilder` clase base controla los modos de sustitución y la mayoría de los prefijos.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-307">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="a3b8a-308">Un proyecto de implementación solo necesita:</span><span class="sxs-lookup"><span data-stu-id="a3b8a-308">An implementing project need only:</span></span>

* <span data-ttu-id="a3b8a-309">Herede de la clase base e implemente un origen básico de pares clave-valor a través de `GetValue` y `GetAllValues` :</span><span class="sxs-lookup"><span data-stu-id="a3b8a-309">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="a3b8a-310">Agregue el [Microsoft.Configuration.ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-310">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="a3b8a-311">La `KeyValueConfigBuilder` clase base proporciona gran parte del comportamiento coherente y de trabajo en los generadores de configuración de clave/valor.</span><span class="sxs-lookup"><span data-stu-id="a3b8a-311">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3b8a-312">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a3b8a-312">Additional resources</span></span>

* [<span data-ttu-id="a3b8a-313">Repositorio de los generadores de configuración de GitHub</span><span class="sxs-lookup"><span data-stu-id="a3b8a-313">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="a3b8a-314">Autenticación entre servicios en Azure Key Vault mediante .NET</span><span class="sxs-lookup"><span data-stu-id="a3b8a-314">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
