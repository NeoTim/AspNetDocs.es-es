---
title: Autenticar a los usuarios con WS-Federation en ASP.NET Core
author: chlowell
description: Este tutorial muestra cómo utilizar WS-Federation en una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065102"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="c0740-103">Autenticar a los usuarios con WS-Federation en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0740-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="c0740-104">Este tutorial muestra cómo habilitar usuarios iniciar sesión con un proveedor de autenticación de WS-Federation, como Active Directory Federation Services (ADFS) o [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="c0740-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="c0740-105">Usa la aplicación de ejemplo de ASP.NET Core 2.0 se describe en [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c0740-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c0740-106">Para las aplicaciones de ASP.NET Core 2.0, proporciona compatibilidad con WS-Federation [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="c0740-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="c0740-107">Este componente se porta desde [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) y comparte muchos de los mecanismos de dicho componente.</span><span class="sxs-lookup"><span data-stu-id="c0740-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="c0740-108">Sin embargo, los componentes se diferencian en un par de formas importantes.</span><span class="sxs-lookup"><span data-stu-id="c0740-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="c0740-109">De forma predeterminada, el middleware nueva:</span><span class="sxs-lookup"><span data-stu-id="c0740-109">By default, the new middleware:</span></span>

* <span data-ttu-id="c0740-110">No permitir inicios de sesión no solicitados.</span><span class="sxs-lookup"><span data-stu-id="c0740-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="c0740-111">Esta característica del protocolo WS-Federation es vulnerable a ataques XSRF.</span><span class="sxs-lookup"><span data-stu-id="c0740-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="c0740-112">Sin embargo, puede habilitarse con el `AllowUnsolicitedLogins` opción.</span><span class="sxs-lookup"><span data-stu-id="c0740-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="c0740-113">No se comprueba cada formulario post para los mensajes de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="c0740-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="c0740-114">Solo las solicitudes a la `CallbackPath` se comprueba el inicio de sesión complementos. `CallbackPath` el valor predeterminado es `/signin-wsfed` , pero puede cambiarse a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="c0740-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="c0740-115">Esta ruta de acceso se puede compartir con otros proveedores de autenticación habilitando la [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) opción.</span><span class="sxs-lookup"><span data-stu-id="c0740-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="c0740-116">Registrar la aplicación con Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0740-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="c0740-117">Servicios de federación de Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0740-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="c0740-118">Abra el servidor **para usuario autenticado entidad Asistente para agregar confianza** desde la consola de administración de AD FS:</span><span class="sxs-lookup"><span data-stu-id="c0740-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Agregar el Asistente para la relación de confianza para usuario autenticado: Pantalla de inicio](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="c0740-120">Elija esta opción escribir manualmente los datos:</span><span class="sxs-lookup"><span data-stu-id="c0740-120">Choose to enter data manually:</span></span>

![Agregar el Asistente para la relación de confianza para usuario autenticado: Seleccionar origen de datos](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="c0740-122">Escriba un nombre para mostrar para el usuario de confianza.</span><span class="sxs-lookup"><span data-stu-id="c0740-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="c0740-123">El nombre no es importante para la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0740-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="c0740-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) no es compatible con el cifrado de tokens, por lo que no configure un certificado de cifrado de tokens:</span><span class="sxs-lookup"><span data-stu-id="c0740-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Agregar el Asistente para la relación de confianza para usuario autenticado: Configurar certificado](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="c0740-126">Habilitar la compatibilidad de protocolo pasivo de WS-Federation, mediante la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0740-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="c0740-127">Compruebe que el puerto sea correcto para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c0740-127">Verify the port is correct for the app:</span></span>

![Agregar el Asistente para la relación de confianza para usuario autenticado: Configurar URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="c0740-129">Debe ser una dirección URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c0740-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="c0740-130">IIS Express se puede proporcionar un certificado autofirmado al hospedar la aplicación durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c0740-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="c0740-131">Kestrel requiere configuración manual de certificados.</span><span class="sxs-lookup"><span data-stu-id="c0740-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="c0740-132">Consulte la [documentación Kestrel](xref:fundamentals/servers/kestrel) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="c0740-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="c0740-133">Haga clic en **siguiente** con el resto del asistente y **cerrar** al final.</span><span class="sxs-lookup"><span data-stu-id="c0740-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="c0740-134">ASP.NET Core Identity requiere un **Id. de nombre** de notificación.</span><span class="sxs-lookup"><span data-stu-id="c0740-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="c0740-135">Agregue uno de los **editar reglas de notificación** cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="c0740-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Editar reglas de notificación](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="c0740-137">En el **transformar notificaciones Asistente para agregar regla**, deje el valor predeterminado **enviar atributos LDAP como notificaciones** plantilla seleccionada y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c0740-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="c0740-138">Agregue una regla de asignación el **SAM-Account-Name** atributo LDAP para la **Id. de nombre** notificación saliente:</span><span class="sxs-lookup"><span data-stu-id="c0740-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Agregar el Asistente para reglas de notificación de transformación: Configurar regla de notificación](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="c0740-140">Haga clic en **finalizar** > **Aceptar** en el **editar reglas de notificación** ventana.</span><span class="sxs-lookup"><span data-stu-id="c0740-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="c0740-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0740-141">Azure Active Directory</span></span>

* <span data-ttu-id="c0740-142">Vaya a la hoja de registros de aplicaciones del inquilino AAD.</span><span class="sxs-lookup"><span data-stu-id="c0740-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="c0740-143">Haga clic en **nuevo registro de aplicaciones**:</span><span class="sxs-lookup"><span data-stu-id="c0740-143">Click **New application registration**:</span></span>

![Azure Active Directory: Registros de aplicaciones](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="c0740-145">Escriba un nombre para el registro de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0740-145">Enter a name for the app registration.</span></span> <span data-ttu-id="c0740-146">Esto no es importante para la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0740-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="c0740-147">Escriba la dirección URL de la aplicación escucha en el que el **dirección URL de inicio de sesión**:</span><span class="sxs-lookup"><span data-stu-id="c0740-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Crear registro de aplicación](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="c0740-149">Haga clic en **extremos** y tenga en cuenta la **documento de metadatos de federación** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="c0740-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="c0740-150">Se trata el middleware de WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="c0740-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: puntos de conexión](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="c0740-152">Navegue hasta el nuevo registro de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0740-152">Navigate to the new app registration.</span></span> <span data-ttu-id="c0740-153">Haga clic en **configuración** > **propiedades** y tome nota de la **App ID URI**.</span><span class="sxs-lookup"><span data-stu-id="c0740-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="c0740-154">Se trata el middleware de WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="c0740-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Propiedades de registro de aplicación](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="c0740-156">Adición de WS-Federation como proveedor de inicio de sesión externo para ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="c0740-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="c0740-157">Agregar una dependencia en [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c0740-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="c0740-158">Agregue el WS-Federation a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c0740-158">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

    ```csharp
    services.AddIdentity<IdentityUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="c0740-159">Inicie sesión con WS-Federation</span><span class="sxs-lookup"><span data-stu-id="c0740-159">Log in with WS-Federation</span></span>

<span data-ttu-id="c0740-160">Vaya a la aplicación y haga clic en el **iniciarla** vínculo en el encabezado de la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="c0740-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="c0740-161">Hay una opción para iniciar sesión con WsFederation: ![Inicie sesión en la página](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="c0740-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="c0740-162">Con ADFS como proveedor, el botón se redirige a una página de inicio de sesión de AD FS: ![Página de inicio de sesión de ADFS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="c0740-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="c0740-163">Con Azure Active Directory como proveedor, el botón se redirige a una página de inicio de sesión de AAD: ![Página de inicio de sesión de AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="c0740-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="c0740-164">Un inicio de sesión correcto para un nuevo usuario redirige a la página de registro de usuario de la aplicación: ![Página de registro](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="c0740-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="c0740-165">Use WS-Federation sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="c0740-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="c0740-166">El middleware de WS-Federation se puede usar sin la identidad.</span><span class="sxs-lookup"><span data-stu-id="c0740-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="c0740-167">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c0740-167">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
