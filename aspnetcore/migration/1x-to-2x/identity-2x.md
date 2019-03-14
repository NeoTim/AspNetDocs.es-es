---
title: Migrar de autenticación e identidad a ASP.NET Core 2.0
author: scottaddie
description: En este artículo se describe los pasos más comunes para migrar la autenticación de ASP.NET Core 1.x y la identidad a ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d28b4af483c7ec9d6cff6db3e2f1693e765d4202
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063022"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="ebcd4-103">Migrar de autenticación e identidad a ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="ebcd4-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="ebcd4-104">Por [Scott Addie](https://github.com/scottaddie) y [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="ebcd4-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="ebcd4-105">ASP.NET Core 2.0 tiene un nuevo modelo para la autenticación y [identidad](xref:security/authentication/identity) que simplifica la configuración mediante el uso de servicios.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="ebcd4-106">Las aplicaciones de ASP.NET Core 1.x que utilizan la autenticación o Identity se pueden actualizar para usar el nuevo modelo, tal como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="ebcd4-107">Middleware de autenticación y servicios</span><span class="sxs-lookup"><span data-stu-id="ebcd4-107">Authentication Middleware and services</span></span>
<span data-ttu-id="ebcd4-108">En los proyectos de 1.x, la autenticación se configura a través de middleware.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="ebcd4-109">Se invoca un método de middleware para cada esquema de autenticación que desea admitir.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="ebcd4-110">El siguiente ejemplo de 1.x configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

<span data-ttu-id="ebcd4-111">En los 2.0 proyectos, la autenticación se configura a través de servicios.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="ebcd4-112">Cada esquema de autenticación está registrado en el `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="ebcd4-113">El `UseIdentity` método se reemplaza por `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="ebcd4-114">El siguiente ejemplo 2.0 configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="ebcd4-115">El `UseAuthentication` método agrega un componente de middleware de autenticación único que es responsable de la autenticación automática y la administración de las solicitudes de autenticación remota.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="ebcd4-116">Reemplaza todos los componentes de middleware individuales con un componente de middleware única y común.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="ebcd4-117">A continuación se muestran 2.0 instrucciones de migración para cada esquema de autenticación principales.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="ebcd4-118">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="ebcd4-118">Cookie-based authentication</span></span>
<span data-ttu-id="ebcd4-119">Seleccione una de las dos opciones siguientes y realice los cambios necesarios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="ebcd4-120">Uso de cookies con identidad</span><span class="sxs-lookup"><span data-stu-id="ebcd4-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="ebcd4-121">Reemplace `UseIdentity` con `UseAuthentication` en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="ebcd4-122">Invocar el `AddIdentity` método en el `ConfigureServices` método para agregar los servicios de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="ebcd4-123">Si lo desea, invocar el `ConfigureApplicationCookie` o `ConfigureExternalCookie` método en el `ConfigureServices` método ajustar la configuración de la cookie de identidad.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="ebcd4-124">Usar cookies sin identidad</span><span class="sxs-lookup"><span data-stu-id="ebcd4-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="ebcd4-125">Reemplace el `UseCookieAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="ebcd4-126">Invocar el `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="ebcd4-127">Autenticación de portador JWT</span><span class="sxs-lookup"><span data-stu-id="ebcd4-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="ebcd4-128">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ebcd4-129">Reemplace el `UseJwtBearerAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ebcd4-130">Invocar el `AddJwtBearer` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="ebcd4-131">Este fragmento de código no usa la identidad, por lo que se debe establecer el esquema predeterminado pasando `JwtBearerDefaults.AuthenticationScheme` a la `AddAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="ebcd4-132">Autenticación de OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="ebcd4-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="ebcd4-133">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="ebcd4-134">Reemplace el `UseOpenIdConnectAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ebcd4-135">Invocar el `AddOpenIdConnect` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="ebcd4-136">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="ebcd4-136">Facebook authentication</span></span>
<span data-ttu-id="ebcd4-137">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ebcd4-138">Reemplace el `UseFacebookAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ebcd4-139">Invocar el `AddFacebook` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="ebcd4-140">Autenticación con Google</span><span class="sxs-lookup"><span data-stu-id="ebcd4-140">Google authentication</span></span>
<span data-ttu-id="ebcd4-141">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ebcd4-142">Reemplace el `UseGoogleAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ebcd4-143">Invocar el `AddGoogle` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="ebcd4-144">Autenticación de Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="ebcd4-144">Microsoft Account authentication</span></span>
<span data-ttu-id="ebcd4-145">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ebcd4-146">Reemplace el `UseMicrosoftAccountAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ebcd4-147">Invocar el `AddMicrosoftAccount` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="ebcd4-148">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="ebcd4-148">Twitter authentication</span></span>
<span data-ttu-id="ebcd4-149">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ebcd4-150">Reemplace el `UseTwitterAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ebcd4-151">Invocar el `AddTwitter` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="ebcd4-152">Esquemas de autenticación predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="ebcd4-152">Setting default authentication schemes</span></span>
<span data-ttu-id="ebcd4-153">En la versión 1.x, el `AutomaticAuthenticate` y `AutomaticChallenge` propiedades de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) clase base se pensada para establecerse en un esquema de autenticación único.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="ebcd4-154">No había una buena manera para exigir esto.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="ebcd4-155">Se han quitado en 2.0, estas dos propiedades como propiedades en cada `AuthenticationOptions` instancia.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="ebcd4-156">Se pueden configurar en el `AddAuthentication` llamada al método dentro de la `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="ebcd4-157">En el fragmento de código anterior, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="ebcd4-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="ebcd4-158">Como alternativa, use una versión sobrecargada de la `AddAuthentication` método para establecer más de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="ebcd4-159">En el siguiente ejemplo de método sobrecargado, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="ebcd4-160">También se puede especificar el esquema de autenticación dentro de la persona `[Authorize]` atributos o las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="ebcd4-161">Definir un esquema predeterminado de 2.0 si se cumple una de las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="ebcd4-162">Desea que el usuario para iniciar sesión automáticamente</span><span class="sxs-lookup"><span data-stu-id="ebcd4-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="ebcd4-163">Usa el `[Authorize]` las directivas de autorización o el atributo sin especificar esquemas</span><span class="sxs-lookup"><span data-stu-id="ebcd4-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="ebcd4-164">Una excepción a esta regla es la `AddIdentity` método.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="ebcd4-165">Este método agrega las cookies para usted y establece el valor predeterminado autenticarse y Desafíe a esquemas para la cookie de aplicación `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="ebcd4-166">Además, Establece el esquema predeterminado de inicio de sesión a la cookie externa `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="ebcd4-167">Usar las extensiones de autenticación de HttpContext</span><span class="sxs-lookup"><span data-stu-id="ebcd4-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="ebcd4-168">El `IAuthenticationManager` interfaz es el punto de entrada principal en el sistema de autenticación 1.x.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="ebcd4-169">Se ha reemplazado por un nuevo conjunto de `HttpContext` métodos de extensión en el `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="ebcd4-170">Por ejemplo, los proyectos de 1.x referencia un `Authentication` propiedad:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="ebcd4-171">En los 2.0 proyectos, importe el `Microsoft.AspNetCore.Authentication` espacio de nombres y elimine el `Authentication` referencias de propiedad:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="ebcd4-172">Autenticación de Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="ebcd4-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="ebcd4-173">Hay dos variaciones de autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="ebcd4-174">El host sólo permite que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="ebcd4-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="ebcd4-175">El host permite tanto anónimos y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="ebcd4-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="ebcd4-176">La primera variación que se ha descrito anteriormente se ve afectada por los cambios de 2.0.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="ebcd4-177">La segunda variación que se ha descrito anteriormente se ve afectada por los cambios de 2.0.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="ebcd4-178">Por ejemplo, puede permitir a los usuarios anónimos en su aplicación en IIS o [HTTP.sys](xref:fundamentals/servers/httpsys) pero autorizando a los usuarios en el nivel de controlador de capas.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-178">As an example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="ebcd4-179">En este escenario, establezca el esquema predeterminado en `IISDefaults.AuthenticationScheme` en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="ebcd4-180">En consecuencia, no se pudo establecer el esquema predeterminado impide que la solicitud authorize al desafío de trabajo.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="ebcd4-181">Instancias de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="ebcd4-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="ebcd4-182">Un efecto secundario de los 2.0 cambios es el cambio a denominado opciones en lugar de las instancias de las opciones de cookie.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="ebcd4-183">Se quita la capacidad para personalizar los nombres de esquema de cookie de identidad.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="ebcd4-184">Por ejemplo, use los proyectos de 1.x [inserción del constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para pasar un `IdentityCookieOptions` parámetro en *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="ebcd4-185">El esquema de autenticación externo cookie se obtiene acceso desde la instancia proporcionada:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="ebcd4-186">La inyección de constructor mencionados anteriormente se convierte en innecesaria en los 2.0 proyectos y el `_externalCookieScheme` se puede eliminar el campo:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="ebcd4-187">El `IdentityConstants.ExternalScheme` constante se puede usar directamente:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="ebcd4-188">Agregar propiedades de navegación IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="ebcd4-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="ebcd4-189">Las propiedades de la base de navegación de Entity Framework (EF) Core `IdentityUser` POCO (objeto CRL estándar) se han quitado.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="ebcd4-190">Si el proyecto de 1.x usa estas propiedades, agregarlos manualmente al proyecto 2.0:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="ebcd4-191">Para evitar duplicadas claves externas al ejecutar migraciones de EF Core, agregue lo siguiente a su `IdentityDbContext` clase `OnModelCreating` método (después de que el `base.OnModelCreating();` llamar):</span><span class="sxs-lookup"><span data-stu-id="ebcd4-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="ebcd4-192">Reemplace GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="ebcd4-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="ebcd4-193">El método sincrónico `GetExternalAuthenticationSchemes` quitó una versión asincrónica.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="ebcd4-194">los proyectos de 1.x tienen el siguiente código *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="ebcd4-195">Este método aparece en *Login.cshtml* demasiado:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="ebcd4-196">En los 2.0 proyectos, utilice el `GetExternalAuthenticationSchemesAsync` método:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="ebcd4-197">En *Login.cshtml*, `AuthenticationScheme` propiedad accede en la `foreach` bucle cambia a `Name`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="ebcd4-198">Cambio de propiedad ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="ebcd4-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="ebcd4-199">Un `ManageLoginsViewModel` objeto se usa en el `ManageLogins` acción de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="ebcd4-200">En de los proyectos de 1.x, el objeto `OtherLogins` propiedad de valor devuelto es de tipo `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="ebcd4-201">Este tipo de valor devuelto requiere una importación de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="ebcd4-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="ebcd4-202">En los 2.0 proyectos, se cambia el tipo de valor devuelto a `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="ebcd4-203">Este nuevo tipo de valor devuelto es necesario sustituir la `Microsoft.AspNetCore.Http.Authentication` importar con un `Microsoft.AspNetCore.Authentication` importar.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="ebcd4-204">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ebcd4-204">Additional resources</span></span>
<span data-ttu-id="ebcd4-205">Para obtener más detalles y explicación, consulte el [discusión Auth 2.0](https://github.com/aspnet/Security/issues/1338) problema en GitHub.</span><span class="sxs-lookup"><span data-stu-id="ebcd4-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
