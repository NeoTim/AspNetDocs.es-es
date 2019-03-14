---
title: Proveedores de directivas de autorización personalizado en ASP.NET Core
author: mjrousos
description: Obtenga información sobre cómo usar un IAuthorizationPolicyProvider personalizado en una aplicación ASP.NET Core para generar dinámicamente las directivas de autorización.
ms.author: riande
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: ca57a9fd8e3c11f15fe14bbe4538bc748c4c84b6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039132"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="1a268-103">Proveedores personalizados de directiva de autorización mediante IAuthorizationPolicyProvider en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a268-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="1a268-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="1a268-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="1a268-105">Normalmente, cuando se usa [autorización basada en directivas](xref:security/authorization/policies), las directivas se registran mediante una llamada a `AuthorizationOptions.AddPolicy` como parte de la configuración del servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="1a268-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="1a268-106">En algunos escenarios, puede no ser posible (o deseable) para registrar todas las directivas de autorización de esta manera.</span><span class="sxs-lookup"><span data-stu-id="1a268-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="1a268-107">En esos casos, puede utilizar una personalizada `IAuthorizationPolicyProvider` para controlar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="1a268-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="1a268-108">Ejemplos de escenarios donde un personalizado [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) puede resultar útil incluir:</span><span class="sxs-lookup"><span data-stu-id="1a268-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="1a268-109">Usar un servicio externo para proporcionar evaluación de directivas.</span><span class="sxs-lookup"><span data-stu-id="1a268-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="1a268-110">Con un intervalo amplio de directivas (para los números de sala diferente o edades, por ejemplo), por lo que no tiene sentido agregar cada directiva de autorización individuales con un `AuthorizationOptions.AddPolicy` llamar.</span><span class="sxs-lookup"><span data-stu-id="1a268-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="1a268-111">Creación de directivas en tiempo de ejecución basándose en la información de origen de datos externo (por ejemplo, una base de datos) o determinar los requisitos de autorización de forma dinámica mediante otro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="1a268-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="1a268-112">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) desde el [repositorio de AspNetCore GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="1a268-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="1a268-113">Descargue el archivo ZIP de repositorio aspnet/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="1a268-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="1a268-114">Descomprima el archivo.</span><span class="sxs-lookup"><span data-stu-id="1a268-114">Unzip the file.</span></span> <span data-ttu-id="1a268-115">Navegue hasta la *src/seguridad/samples/CustomPolicyProvider* carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1a268-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="1a268-116">Personalizar la recuperación de directiva</span><span class="sxs-lookup"><span data-stu-id="1a268-116">Customize policy retrieval</span></span>

<span data-ttu-id="1a268-117">Las aplicaciones de ASP.NET Core usan una implementación de la `IAuthorizationPolicyProvider` interfaz para recuperar las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="1a268-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="1a268-118">De forma predeterminada, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) se registra y se utiliza.</span><span class="sxs-lookup"><span data-stu-id="1a268-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="1a268-119">`DefaultAuthorizationPolicyProvider` Devuelve las directivas de la `AuthorizationOptions` proporcionado en un `IServiceCollection.AddAuthorization` llamar.</span><span class="sxs-lookup"><span data-stu-id="1a268-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="1a268-120">Puede personalizar este comportamiento mediante el registro de otro `IAuthorizationPolicyProvider` implementación en la aplicación [inserción de dependencias](xref:fundamentals/dependency-injection) contenedor.</span><span class="sxs-lookup"><span data-stu-id="1a268-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="1a268-121">El `IAuthorizationPolicyProvider` interfaz contiene dos API:</span><span class="sxs-lookup"><span data-stu-id="1a268-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="1a268-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) devuelve una directiva de autorización para un nombre concreto.</span><span class="sxs-lookup"><span data-stu-id="1a268-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="1a268-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) devuelve la directiva de autorización de forma predeterminada (la directiva utilizada para `[Authorize]` atributos sin una directiva especificada).</span><span class="sxs-lookup"><span data-stu-id="1a268-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="1a268-124">Mediante la implementación de estas dos API, puede personalizar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="1a268-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="1a268-125">Parametrizar autorizar el ejemplo de atributo</span><span class="sxs-lookup"><span data-stu-id="1a268-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="1a268-126">Un escenario donde `IAuthorizationPolicyProvider` es útil es personalizado para permitir `[Authorize]` atributos cuyos requisitos dependen de un parámetro.</span><span class="sxs-lookup"><span data-stu-id="1a268-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="1a268-127">Por ejemplo, en [autorización basada en directivas](xref:security/authorization/policies) documentación, un edades ("AtLeast21") se utiliza la directiva como un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="1a268-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="1a268-128">Si las acciones de controlador diferente en una aplicación deben estar disponibles para los usuarios de *diferentes* edades, podría ser útil tener muchas directivas diferentes edades.</span><span class="sxs-lookup"><span data-stu-id="1a268-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="1a268-129">En vez de registrar todas las diferentes edades directivas que la aplicación necesitará en `AuthorizationOptions`, puede generar las directivas de forma dinámica con un personalizado `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="1a268-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="1a268-130">Para hacer con las directivas más fácil, puede anotar las acciones con el atributo de autorización personalizado como `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="1a268-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="1a268-131">Atributos de autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="1a268-131">Custom Authorization Attributes</span></span>

<span data-ttu-id="1a268-132">Las directivas de autorización se identifican por sus nombres.</span><span class="sxs-lookup"><span data-stu-id="1a268-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="1a268-133">Personalizado `MinimumAgeAuthorizeAttribute` descrito anteriormente, debe asignar argumentos en una cadena que puede usarse para recuperar la directiva de autorización correspondiente.</span><span class="sxs-lookup"><span data-stu-id="1a268-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="1a268-134">Puede hacerlo mediante la derivación de `AuthorizeAttribute` y realizar el `Age` encapsulado propiedad el `AuthorizeAttribute.Policy` propiedad.</span><span class="sxs-lookup"><span data-stu-id="1a268-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="1a268-135">Este tipo de atributo tiene un `Policy` cadena según el prefijo codificado de forma rígida (`"MinimumAge"`) y un entero pasa a través del constructor.</span><span class="sxs-lookup"><span data-stu-id="1a268-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="1a268-136">Se puede aplicar a acciones en la misma manera que otras `Authorize` atributos salvo que toma un entero como parámetro.</span><span class="sxs-lookup"><span data-stu-id="1a268-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="1a268-137">IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="1a268-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="1a268-138">Personalizado `MinimumAgeAuthorizeAttribute` facilita a las directivas de autorización de solicitud para todas las edades mínima deseada.</span><span class="sxs-lookup"><span data-stu-id="1a268-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="1a268-139">El siguiente problema para resolver es asegurarse de que las directivas de autorización están disponibles para todos esas diferentes edades.</span><span class="sxs-lookup"><span data-stu-id="1a268-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="1a268-140">Aquí es donde un `IAuthorizationPolicyProvider` es útil.</span><span class="sxs-lookup"><span data-stu-id="1a268-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="1a268-141">Cuando se usa `MinimumAgeAuthorizationAttribute`, los nombres de directiva de autorización seguirá el patrón `"MinimumAge" + Age`, por lo que personalizado `IAuthorizationPolicyProvider` debe generar directivas de autorización por:</span><span class="sxs-lookup"><span data-stu-id="1a268-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="1a268-142">La antigüedad del nombre de la directiva de análisis.</span><span class="sxs-lookup"><span data-stu-id="1a268-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="1a268-143">Uso de `AuthorizationPolicyBuilder` para crear un nuevo `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="1a268-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="1a268-144">Agregar requisitos a la directiva según la edad con `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="1a268-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="1a268-145">En otros escenarios, puede usar `RequireClaim`, `RequireRole`, o `RequireUserName` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="1a268-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="1a268-146">Varios proveedores de directiva de autorización</span><span class="sxs-lookup"><span data-stu-id="1a268-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="1a268-147">Al usar custom `IAuthorizationPolicyProvider` implementaciones, tenga en cuenta que ASP.NET Core solo usa una instancia de `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="1a268-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="1a268-148">Si un proveedor personalizado no es capaz de proporcionar directivas de autorización para todos los nombres de directiva, debe recurrir a un proveedor de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="1a268-148">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="1a268-149">Los nombres de directiva podrían incluirlas que proceden de una directiva predeterminada para `[Authorize]` atributos sin un nombre.</span><span class="sxs-lookup"><span data-stu-id="1a268-149">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="1a268-150">Por ejemplo, considere que una aplicación necesita directivas personalizadas de edad y la recuperación de directivas basado en roles más tradicional.</span><span class="sxs-lookup"><span data-stu-id="1a268-150">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="1a268-151">Este tipo de aplicación podría usar un proveedor de directivas de autorización personalizado que:</span><span class="sxs-lookup"><span data-stu-id="1a268-151">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="1a268-152">Intenta analizar los nombres de directiva.</span><span class="sxs-lookup"><span data-stu-id="1a268-152">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="1a268-153">Las llamadas a un proveedor de directivas diferentes (como `DefaultAuthorizationPolicyProvider`) si el nombre de la directiva no contiene una edad.</span><span class="sxs-lookup"><span data-stu-id="1a268-153">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="1a268-154">Directiva predeterminada</span><span class="sxs-lookup"><span data-stu-id="1a268-154">Default policy</span></span>

<span data-ttu-id="1a268-155">Además de proporcionar directivas de autorización con nombre, un personalizado `IAuthorizationPolicyProvider` debe implementar `GetDefaultPolicyAsync` para proporcionar una directiva de autorización para `[Authorize]` atributos sin un nombre de directiva especificado.</span><span class="sxs-lookup"><span data-stu-id="1a268-155">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="1a268-156">En muchos casos, este atributo de autorización solo requiere un usuario autenticado, por lo que puede realizar la directiva necesaria con una llamada a `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="1a268-156">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="1a268-157">Como ocurre con todos los aspectos de un personalizado `IAuthorizationPolicyProvider`, puede personalizar esto, según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="1a268-157">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="1a268-158">En algunos casos:</span><span class="sxs-lookup"><span data-stu-id="1a268-158">In some cases:</span></span>

* <span data-ttu-id="1a268-159">No se pueden usar las directivas de autorización de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1a268-159">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="1a268-160">Recuperación de la directiva predeterminada se puede delegar en una acción de reserva `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="1a268-160">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="1a268-161">Usar un IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="1a268-161">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="1a268-162">Para usar directivas personalizadas de un `IAuthorizationPolicyProvider`, debe:</span><span class="sxs-lookup"><span data-stu-id="1a268-162">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="1a268-163">Registrar adecuado `AuthorizationHandler` tipos con inserción de dependencias (se describe en [autorización basada en directivas](xref:security/authorization/policies#authorization-handlers)), al igual que con todos los escenarios de autorización basada en directivas.</span><span class="sxs-lookup"><span data-stu-id="1a268-163">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="1a268-164">Registrar personalizado `IAuthorizationPolicyProvider` tipo de colección de servicios de inyección de dependencia de la aplicación (en `Startup.ConfigureServices`) para reemplazar el proveedor de directivas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1a268-164">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="1a268-165">Un completo personalizado `IAuthorizationPolicyProvider` ejemplo está disponible en el [repositorio de GitHub de aspnet/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="1a268-165">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
