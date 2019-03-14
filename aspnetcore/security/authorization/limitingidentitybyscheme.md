---
title: Autorizar con un esquema específico en ASP.NET Core
author: rick-anderson
description: En este artículo se explica cómo limitar la identidad de un esquema específico cuando se trabaja con varios métodos de autenticación.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059472"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="94044-103">Autorizar con un esquema específico en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94044-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="94044-104">En algunos escenarios, como aplicaciones de página única (SPA), es habitual usar varios métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="94044-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="94044-105">Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación de portador JWT para las solicitudes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94044-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="94044-106">En algunos casos, la aplicación puede tener varias instancias de un controlador de autenticación.</span><span class="sxs-lookup"><span data-stu-id="94044-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="94044-107">Por ejemplo, dos controladores de la cookie donde uno contiene una identidad básica y el otro se crea cuando se ha desencadenado una autenticación multifactor (MFA).</span><span class="sxs-lookup"><span data-stu-id="94044-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="94044-108">MFA se puede desencadenar porque el usuario solicitó una operación que requiere una seguridad adicional.</span><span class="sxs-lookup"><span data-stu-id="94044-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94044-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94044-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="94044-110">Un esquema de autenticación se llama cuando el servicio de autenticación se configura durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="94044-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="94044-111">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="94044-111">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="94044-112">En el código anterior, se han agregado dos controladores de autenticación: una para las cookies y otra de portador.</span><span class="sxs-lookup"><span data-stu-id="94044-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="94044-113">Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad se establezca en esa identidad.</span><span class="sxs-lookup"><span data-stu-id="94044-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="94044-114">Si no se desea ese comportamiento, puede deshabilitarlo mediante la invocación de la forma sin parámetros de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="94044-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94044-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94044-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="94044-116">Se denominan esquemas de autenticación cuando el middleware de autenticación se configuran durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="94044-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="94044-117">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="94044-117">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="94044-118">En el código anterior, se han agregado dos middleware de autenticación: una para las cookies y otra de portador.</span><span class="sxs-lookup"><span data-stu-id="94044-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="94044-119">Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad se establezca en esa identidad.</span><span class="sxs-lookup"><span data-stu-id="94044-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="94044-120">Si no se desea ese comportamiento, puede deshabilitarla estableciendo el `AuthenticationOptions.AutomaticAuthenticate` propiedad `false`.</span><span class="sxs-lookup"><span data-stu-id="94044-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="94044-121">Seleccionar el esquema con el atributo Authorize</span><span class="sxs-lookup"><span data-stu-id="94044-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="94044-122">En el momento de la autorización, la aplicación indica el controlador que se usará.</span><span class="sxs-lookup"><span data-stu-id="94044-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="94044-123">Seleccione el controlador con el que va a autorizar la aplicación pasando una lista delimitada por comas de esquemas de autenticación `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="94044-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="94044-124">El `[Authorize]` atributo especifica el esquema de autenticación o combinaciones que puede utilizar independientemente de si se ha configurado un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="94044-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="94044-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="94044-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94044-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94044-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94044-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94044-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="94044-128">En el ejemplo anterior, los controladores de la cookie y el portador ejecutan y tienen la oportunidad de crear y agregar una identidad para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="94044-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="94044-129">Al especificar un único esquema, se ejecuta el controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="94044-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94044-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94044-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94044-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94044-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="94044-132">En el código anterior, se ejecuta solo el controlador con el esquema de "Bearer".</span><span class="sxs-lookup"><span data-stu-id="94044-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="94044-133">Se omiten las identidades basada en cookies.</span><span class="sxs-lookup"><span data-stu-id="94044-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="94044-134">Seleccionar la combinación de directivas</span><span class="sxs-lookup"><span data-stu-id="94044-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="94044-135">Si desea especificar los esquemas deseados en [directiva](xref:security/authorization/policies), puede establecer el `AuthenticationSchemes` al agregar la directiva de colección:</span><span class="sxs-lookup"><span data-stu-id="94044-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="94044-136">En el ejemplo anterior, la directiva de "Over18" solo se ejecuta con la identidad del creado por el controlador "Bearer".</span><span class="sxs-lookup"><span data-stu-id="94044-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="94044-137">Usar la directiva estableciendo el `[Authorize]` del atributo `Policy` propiedad:</span><span class="sxs-lookup"><span data-stu-id="94044-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="94044-138">Usar varios esquemas de autenticación</span><span class="sxs-lookup"><span data-stu-id="94044-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="94044-139">Algunas aplicaciones que necesite admitir varios tipos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="94044-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="94044-140">Por ejemplo, la aplicación puede autenticar a los usuarios de Azure Active Directory y de una base de datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="94044-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="94044-141">Otro ejemplo es una aplicación que autentica a los usuarios de Active Directory Federation Services y Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="94044-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="94044-142">En este caso, la aplicación debe aceptar un token de portador JWT desde varios emisores.</span><span class="sxs-lookup"><span data-stu-id="94044-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="94044-143">Agregue todos los esquemas de autenticación que desea Aceptar.</span><span class="sxs-lookup"><span data-stu-id="94044-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="94044-144">Por ejemplo, el siguiente código en `Startup.ConfigureServices` agrega dos esquemas de autenticación de portador JWT con distintos emisores:</span><span class="sxs-lookup"><span data-stu-id="94044-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="94044-145">Solo una autenticación de portador JWT se registra con el esquema de autenticación predeterminado `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="94044-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="94044-146">Autenticación adicional que se tiene que estar registrada con un esquema de autenticación único.</span><span class="sxs-lookup"><span data-stu-id="94044-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="94044-147">El siguiente paso es actualizar la directiva de autorización de forma predeterminada para aceptar ambos esquemas de autenticación.</span><span class="sxs-lookup"><span data-stu-id="94044-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="94044-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="94044-148">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="94044-149">Como se invalida la directiva de autorización de forma predeterminada, es posible usar el `[Authorize]` atributo en los controladores.</span><span class="sxs-lookup"><span data-stu-id="94044-149">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="94044-150">El controlador, a continuación, acepta las solicitudes con el token JWT emitido por el emisor de primer o segundo.</span><span class="sxs-lookup"><span data-stu-id="94044-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
