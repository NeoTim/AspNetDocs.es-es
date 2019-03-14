---
title: Evitar Cross-Site falsificación de solicitud entre (XSRF/CSRF) attacks en ASP.NET Core
author: steve-smith
description: Descubra cómo impedir ataques contra las aplicaciones web en un sitio Web malintencionado puede influir en la interacción entre un explorador cliente y la aplicación.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6e140717834b901e12ef7863fd07b983b0c55107
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57026392"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="ee196-103">Evitar Cross-Site falsificación de solicitud entre (XSRF/CSRF) attacks en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee196-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="ee196-104">Por [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ee196-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ee196-105">Falsificación de solicitud entre sitios (también conocida como XSRF o CSRF, pronunciado *vea surf*) es un ataque contra aplicaciones hospedadas en web mediante el cual una aplicación web malintencionado puede influir en la interacción entre un explorador cliente y una aplicación web que confía en que Explorador.</span><span class="sxs-lookup"><span data-stu-id="ee196-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="ee196-106">Estos ataques son posibles porque los exploradores web envían algunos tipos de tokens de autenticación automáticamente con cada solicitud a un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="ee196-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="ee196-107">Esta forma de vulnerabilidad de seguridad es también se denomina un *ataque de un solo clic* o *sesión volando* porque el ataque aprovecha las ventajas de sesión del autenticado previamente en el usuario.</span><span class="sxs-lookup"><span data-stu-id="ee196-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="ee196-108">Un ejemplo de un ataque CSRF:</span><span class="sxs-lookup"><span data-stu-id="ee196-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="ee196-109">Un usuario inicia sesión en `www.good-banking-site.com` utilizando la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="ee196-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="ee196-110">El servidor autentica al usuario y emite una respuesta que incluye una cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ee196-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="ee196-111">El sitio es vulnerable a ataques porque confía en cualquier solicitud que recibe una cookie de autenticación válida.</span><span class="sxs-lookup"><span data-stu-id="ee196-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="ee196-112">El usuario visita un sitio malintencionado, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="ee196-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="ee196-113">El sitio malintencionado, `www.bad-crook-site.com`, contiene un formulario HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee196-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="ee196-114">Tenga en cuenta que el formulario `action` publicaciones en el sitio vulnerable, no en el sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="ee196-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="ee196-115">Esta es la parte "cross-site" de CSRF.</span><span class="sxs-lookup"><span data-stu-id="ee196-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="ee196-116">El usuario selecciona el botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="ee196-116">The user selects the submit button.</span></span> <span data-ttu-id="ee196-117">El explorador realiza la solicitud y automáticamente incluye la cookie de autenticación para el dominio solicitado, `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="ee196-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="ee196-118">La solicitud se ejecuta en el `www.good-banking-site.com` servidor con contexto de autenticación del usuario y pueden realizar cualquier acción que se puede realizar un usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="ee196-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="ee196-119">Además de la situación donde el usuario selecciona el botón para enviar el formulario, el sitio malintencionado podría:</span><span class="sxs-lookup"><span data-stu-id="ee196-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="ee196-120">Ejecutar un script que envía el formulario de forma automática.</span><span class="sxs-lookup"><span data-stu-id="ee196-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="ee196-121">Enviar el envío del formulario como una solicitud AJAX.</span><span class="sxs-lookup"><span data-stu-id="ee196-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="ee196-122">Ocultar el formulario mediante CSS.</span><span class="sxs-lookup"><span data-stu-id="ee196-122">Hide the form using CSS.</span></span>

<span data-ttu-id="ee196-123">Estos escenarios alternativos no requieren ninguna otra acción o la entrada del usuario que no sea inicialmente visitando el sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="ee196-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="ee196-124">Uso de HTTPS no impide que un ataque CSRF.</span><span class="sxs-lookup"><span data-stu-id="ee196-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="ee196-125">El sitio Web malintencionado puede enviar un `https://www.good-banking-site.com/` solicitar tan fácilmente como puede enviar una solicitud poco segura.</span><span class="sxs-lookup"><span data-stu-id="ee196-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="ee196-126">Algunos ataques van dirigidos a puntos de conexión que respondan a solicitudes GET, en cuyo caso se puede usar una etiqueta de imagen para realizar la acción.</span><span class="sxs-lookup"><span data-stu-id="ee196-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="ee196-127">Esta forma de ataque es habitual en los sitios de foro que permiten imágenes pero bloquean JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee196-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="ee196-128">Las aplicaciones que cambian el estado en las solicitudes GET, donde se modifican las variables o los recursos, son vulnerables a ataques malintencionados.</span><span class="sxs-lookup"><span data-stu-id="ee196-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="ee196-129">**Las solicitudes GET que cambian el estado no son seguros. Una práctica recomendada consiste en no cambiar nunca el estado en una solicitud GET.**</span><span class="sxs-lookup"><span data-stu-id="ee196-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="ee196-130">Los ataques CSRF son posibles con aplicaciones web que utilizan cookies para la autenticación porque:</span><span class="sxs-lookup"><span data-stu-id="ee196-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="ee196-131">Los exploradores almacenan las cookies emitidas por una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ee196-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="ee196-132">Almacenado cookies incluyen cookies de sesión para los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="ee196-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="ee196-133">Los exploradores envían que todas las cookies asociadas con un dominio a la aplicación web de todas las solicitudes, independientemente de cómo se generó la solicitud a la aplicación dentro del explorador.</span><span class="sxs-lookup"><span data-stu-id="ee196-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="ee196-134">Sin embargo, los ataques CSRF no están limitados a aprovechar las cookies.</span><span class="sxs-lookup"><span data-stu-id="ee196-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="ee196-135">Por ejemplo, la autenticación básica e implícita también son vulnerables.</span><span class="sxs-lookup"><span data-stu-id="ee196-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="ee196-136">Después de que un usuario inicia sesión con la autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que la sesión&dagger; finaliza.</span><span class="sxs-lookup"><span data-stu-id="ee196-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="ee196-137">&dagger;En este contexto, *sesión* hace referencia a la sesión de cliente durante el cual el usuario está autenticado.</span><span class="sxs-lookup"><span data-stu-id="ee196-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="ee196-138">Es no relacionados con las sesiones de servidor o [Middleware de sesión de ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="ee196-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="ee196-139">Los usuarios pueden protegerse frente a vulnerabilidades CSRF tomando precauciones:</span><span class="sxs-lookup"><span data-stu-id="ee196-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="ee196-140">Inicie sesión fuera de las aplicaciones web cuando termine de utilizarlos.</span><span class="sxs-lookup"><span data-stu-id="ee196-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="ee196-141">Borrar las cookies del explorador periódicamente.</span><span class="sxs-lookup"><span data-stu-id="ee196-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="ee196-142">Sin embargo, las vulnerabilidades CSRF son fundamentalmente un problema con la aplicación web, no el usuario final.</span><span class="sxs-lookup"><span data-stu-id="ee196-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="ee196-143">Aspectos básicos de autenticación</span><span class="sxs-lookup"><span data-stu-id="ee196-143">Authentication fundamentals</span></span>

<span data-ttu-id="ee196-144">Autenticación basada en cookies es una forma popular de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ee196-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="ee196-145">Los sistemas de autenticación basada en token están creciendo en popularidad, especialmente para aplicaciones de página única (SPA).</span><span class="sxs-lookup"><span data-stu-id="ee196-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="ee196-146">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="ee196-146">Cookie-based authentication</span></span>

<span data-ttu-id="ee196-147">Cuando un usuario se autentica con su nombre de usuario y contraseña, se le emite un token, que contiene un vale de autenticación que se puede usar para la autenticación y autorización.</span><span class="sxs-lookup"><span data-stu-id="ee196-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="ee196-148">El token se almacena como hace que una cookie que acompaña a cada solicitud del cliente.</span><span class="sxs-lookup"><span data-stu-id="ee196-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="ee196-149">Generar y validar esta cookie se realizan mediante el Middleware de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="ee196-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="ee196-150">El [middleware](xref:fundamentals/middleware/index) serializa un principal de usuario en una cookie cifrada.</span><span class="sxs-lookup"><span data-stu-id="ee196-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="ee196-151">Valida la cookie en solicitudes posteriores, el middleware, vuelve a crear la entidad de seguridad y asigna la entidad de seguridad para el [usuario](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propiedad de [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="ee196-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="ee196-152">Autenticación basada en token</span><span class="sxs-lookup"><span data-stu-id="ee196-152">Token-based authentication</span></span>

<span data-ttu-id="ee196-153">Cuando un usuario se autentica, se le emite un token (no un token antifalsificación).</span><span class="sxs-lookup"><span data-stu-id="ee196-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="ee196-154">El token contiene información de usuario en forma de [notificaciones](/dotnet/framework/security/claims-based-identity-model) o un token de referencia que señala la aplicación al estado de usuario que se mantienen en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee196-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="ee196-155">Cuando un usuario intenta acceder a un recurso que requiere autenticación, el token se envía a la aplicación con un encabezado de autorización adicionales en forma de token de portador.</span><span class="sxs-lookup"><span data-stu-id="ee196-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="ee196-156">Esto hace que la aplicación sin estado.</span><span class="sxs-lookup"><span data-stu-id="ee196-156">This makes the app stateless.</span></span> <span data-ttu-id="ee196-157">En cada solicitud posterior, el token se pasa en la solicitud para la validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="ee196-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="ee196-158">Este token no es *cifrados*; tiene *codificado*.</span><span class="sxs-lookup"><span data-stu-id="ee196-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="ee196-159">En el servidor, se descodifica el token para acceder a su información.</span><span class="sxs-lookup"><span data-stu-id="ee196-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="ee196-160">Para enviar el token en solicitudes posteriores, almacenar el token en el almacenamiento local del explorador.</span><span class="sxs-lookup"><span data-stu-id="ee196-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="ee196-161">No se preocupe sobre una vulnerabilidad CSRF, si el token se almacena en el almacenamiento local del explorador.</span><span class="sxs-lookup"><span data-stu-id="ee196-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="ee196-162">CSRF constituye un problema cuando el token se almacena en una cookie.</span><span class="sxs-lookup"><span data-stu-id="ee196-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="ee196-163">Varias aplicaciones hospedadas en un dominio</span><span class="sxs-lookup"><span data-stu-id="ee196-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="ee196-164">Entornos de hospedaje compartidos son vulnerables al secuestro de sesión, inicio de sesión CSRF y otros ataques.</span><span class="sxs-lookup"><span data-stu-id="ee196-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="ee196-165">Aunque `example1.contoso.net` y `example2.contoso.net` son diferentes de los hosts, hay una relación de confianza implícita entre los hosts bajo la `*.contoso.net` dominio.</span><span class="sxs-lookup"><span data-stu-id="ee196-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="ee196-166">Esta relación de confianza implícita permite a los hosts no sea de confianza influir en la otra las cookies (las directivas de mismo origen que rigen las solicitudes AJAX no se aplican necesariamente a las cookies HTTP).</span><span class="sxs-lookup"><span data-stu-id="ee196-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="ee196-167">Al no compartir dominios, se pueden evitar ataques que aprovechan las cookies de confianza entre las aplicaciones hospedadas en el mismo dominio.</span><span class="sxs-lookup"><span data-stu-id="ee196-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="ee196-168">Cuando cada aplicación se hospeda en su propio dominio, no hay ninguna relación de confianza implícita cookie aprovechar.</span><span class="sxs-lookup"><span data-stu-id="ee196-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="ee196-169">Configuración de ASP.NET Core antifalsificación</span><span class="sxs-lookup"><span data-stu-id="ee196-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="ee196-170">ASP.NET Core se implementa utilizando antifalsificación [protección de datos de ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="ee196-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="ee196-171">La pila de protección de datos debe configurarse para que funcione en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="ee196-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="ee196-172">Consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="ee196-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="ee196-173">En ASP.NET Core 2.0 o posterior, el [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserta tokens antifalsificación en elementos de formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="ee196-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="ee196-174">El siguiente marcado en un archivo Razor genera automáticamente los tokens antifalsificación:</span><span class="sxs-lookup"><span data-stu-id="ee196-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="ee196-175">De forma similar, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) genera tokens antifalsificación de forma predeterminada, si el método del formulario no es GET.</span><span class="sxs-lookup"><span data-stu-id="ee196-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="ee196-176">La generación automática de tokens antifalsificación para los elementos de formulario HTML se produce cuando el `<form>` etiqueta contiene el `method="post"` atributo y cualquiera de las siguientes son verdaderas:</span><span class="sxs-lookup"><span data-stu-id="ee196-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="ee196-177">El atributo action está vacío (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="ee196-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="ee196-178">No se proporciona el atributo de acción (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="ee196-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="ee196-179">Se puede deshabilitar la generación automática de tokens antifalsificación para los elementos de formulario HTML:</span><span class="sxs-lookup"><span data-stu-id="ee196-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="ee196-180">Deshabilite explícitamente tokens antifalsificación con la `asp-antiforgery` atributo:</span><span class="sxs-lookup"><span data-stu-id="ee196-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="ee196-181">El elemento de formulario es elegido horizontal de aplicaciones auxiliares de etiquetas mediante el uso de la aplicación auxiliar de etiquetas [! participar símbolo](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="ee196-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="ee196-182">Quitar el `FormTagHelper` desde la vista.</span><span class="sxs-lookup"><span data-stu-id="ee196-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="ee196-183">El `FormTagHelper` puede quitarse de una vista mediante la adición de la directiva siguiente a la vista de Razor:</span><span class="sxs-lookup"><span data-stu-id="ee196-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="ee196-184">[Las páginas de Razor](xref:razor-pages/index) están protegidos automáticamente frente XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="ee196-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="ee196-185">Para obtener más información, consulte [XSRF/CSRF y páginas de Razor](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="ee196-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="ee196-186">El enfoque más común para defenderse contra ataques de CSRF es usar el *patrón del Token Sincronizador* (STP).</span><span class="sxs-lookup"><span data-stu-id="ee196-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="ee196-187">STP se utiliza cuando el usuario solicita una página con datos del formulario:</span><span class="sxs-lookup"><span data-stu-id="ee196-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="ee196-188">El servidor envía un token de identidad del usuario actual al cliente.</span><span class="sxs-lookup"><span data-stu-id="ee196-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="ee196-189">El cliente devuelve el token en el servidor para la comprobación.</span><span class="sxs-lookup"><span data-stu-id="ee196-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="ee196-190">Si el servidor recibe un token que no coincide con la identidad del usuario autenticado, se rechaza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ee196-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="ee196-191">El token es único e imprevisible.</span><span class="sxs-lookup"><span data-stu-id="ee196-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="ee196-192">El token puede usarse también para garantizar la secuencia correcta de una serie de solicitudes (por ejemplo, lo que garantiza la secuencia de solicitud de: página 1 &ndash; página 2 &ndash; página 3).</span><span class="sxs-lookup"><span data-stu-id="ee196-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="ee196-193">Todos los formularios en las plantillas de ASP.NET Core MVC y páginas de Razor de generan tokens antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="ee196-194">El siguiente par de ejemplos de vista genera tokens antifalsificación:</span><span class="sxs-lookup"><span data-stu-id="ee196-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="ee196-195">Agregar explícitamente un token de antifalsificación en un `<form>` elemento sin usar aplicaciones auxiliares de etiquetas con la aplicación auxiliar HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="ee196-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="ee196-196">En cada uno de los casos anteriores, ASP.NET Core agrega un campo de formulario oculto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee196-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="ee196-197">ASP.NET Core incluye tres [filtros](xref:mvc/controllers/filters) para trabajar con tokens antifalsificación:</span><span class="sxs-lookup"><span data-stu-id="ee196-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="ee196-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="ee196-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="ee196-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="ee196-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="ee196-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="ee196-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="ee196-201">Opciones de antifalsificación</span><span class="sxs-lookup"><span data-stu-id="ee196-201">Antiforgery options</span></span>

<span data-ttu-id="ee196-202">Personalizar [opciones antifalsificación](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ee196-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="ee196-203">&dagger;Establecer el antifalsificación `Cookie` propiedades mediante las propiedades de la [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) clase.</span><span class="sxs-lookup"><span data-stu-id="ee196-203">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="ee196-204">Opción</span><span class="sxs-lookup"><span data-stu-id="ee196-204">Option</span></span> | <span data-ttu-id="ee196-205">Descripción</span><span class="sxs-lookup"><span data-stu-id="ee196-205">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="ee196-206">Cookie</span><span class="sxs-lookup"><span data-stu-id="ee196-206">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="ee196-207">Determina la configuración utilizada para crear la cookie antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-207">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="ee196-208">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="ee196-208">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="ee196-209">El nombre del campo de formulario oculto utilizado por el sistema antifalsificación para representar los tokens antifalsificación en las vistas.</span><span class="sxs-lookup"><span data-stu-id="ee196-209">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="ee196-210">HeaderName</span><span class="sxs-lookup"><span data-stu-id="ee196-210">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="ee196-211">El nombre del encabezado usado por el sistema antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-211">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="ee196-212">Si `null`, el sistema considera que sólo los datos.</span><span class="sxs-lookup"><span data-stu-id="ee196-212">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="ee196-213">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="ee196-213">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="ee196-214">Especifica si se debe suprimir la generación de la `X-Frame-Options` encabezado.</span><span class="sxs-lookup"><span data-stu-id="ee196-214">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="ee196-215">De forma predeterminada, el encabezado se genera con un valor de "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="ee196-215">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="ee196-216">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="ee196-216">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="ee196-217">Opción</span><span class="sxs-lookup"><span data-stu-id="ee196-217">Option</span></span> | <span data-ttu-id="ee196-218">Descripción</span><span class="sxs-lookup"><span data-stu-id="ee196-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="ee196-219">Cookie</span><span class="sxs-lookup"><span data-stu-id="ee196-219">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="ee196-220">Determina la configuración utilizada para crear la cookie antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-220">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="ee196-221">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="ee196-221">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="ee196-222">El dominio de la cookie.</span><span class="sxs-lookup"><span data-stu-id="ee196-222">The domain of the cookie.</span></span> <span data-ttu-id="ee196-223">Tiene como valor predeterminado `null`.</span><span class="sxs-lookup"><span data-stu-id="ee196-223">Defaults to `null`.</span></span> <span data-ttu-id="ee196-224">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="ee196-224">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="ee196-225">La alternativa recomendada es Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="ee196-225">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="ee196-226">CookieName</span><span class="sxs-lookup"><span data-stu-id="ee196-226">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="ee196-227">El nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="ee196-227">The name of the cookie.</span></span> <span data-ttu-id="ee196-228">Si no se establece, el sistema genera un nombre único a partir del [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="ee196-228">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="ee196-229">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="ee196-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="ee196-230">La alternativa recomendada es Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="ee196-230">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="ee196-231">CookiePath</span><span class="sxs-lookup"><span data-stu-id="ee196-231">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="ee196-232">La ruta de acceso establecido en la cookie.</span><span class="sxs-lookup"><span data-stu-id="ee196-232">The path set on the cookie.</span></span> <span data-ttu-id="ee196-233">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="ee196-233">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="ee196-234">La alternativa recomendada es Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="ee196-234">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="ee196-235">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="ee196-235">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="ee196-236">El nombre del campo de formulario oculto utilizado por el sistema antifalsificación para representar los tokens antifalsificación en las vistas.</span><span class="sxs-lookup"><span data-stu-id="ee196-236">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="ee196-237">HeaderName</span><span class="sxs-lookup"><span data-stu-id="ee196-237">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="ee196-238">El nombre del encabezado usado por el sistema antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-238">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="ee196-239">Si `null`, el sistema considera que sólo los datos.</span><span class="sxs-lookup"><span data-stu-id="ee196-239">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="ee196-240">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="ee196-240">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="ee196-241">Especifica si el sistema antifalsificación requiere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ee196-241">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="ee196-242">Si `true`, producirá un error de las solicitudes que no sean HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ee196-242">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="ee196-243">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="ee196-243">Defaults to `false`.</span></span> <span data-ttu-id="ee196-244">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="ee196-244">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="ee196-245">La alternativa recomendada es establecer Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="ee196-245">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="ee196-246">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="ee196-246">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="ee196-247">Especifica si se debe suprimir la generación de la `X-Frame-Options` encabezado.</span><span class="sxs-lookup"><span data-stu-id="ee196-247">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="ee196-248">De forma predeterminada, el encabezado se genera con un valor de "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="ee196-248">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="ee196-249">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="ee196-249">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="ee196-250">Para obtener más información, consulte [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="ee196-250">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="ee196-251">Configurar características antifalsificación con IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="ee196-251">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="ee196-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) proporciona la API para configurar características antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="ee196-253">`IAntiforgery` se puede solicitar en el `Configure` método de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="ee196-253">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="ee196-254">En el ejemplo siguiente se usa el software intermedio de la página principal de la aplicación para generar un token de antifalsificación y enviarlo en la respuesta como una cookie (mediante la convención de nomenclatura de forma predeterminada Angular que se describe más adelante en este tema):</span><span class="sxs-lookup"><span data-stu-id="ee196-254">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="ee196-255">Requerir la validación antifalsificación</span><span class="sxs-lookup"><span data-stu-id="ee196-255">Require antiforgery validation</span></span>

<span data-ttu-id="ee196-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) es un filtro de acción que se puede aplicar a una acción individual, un controlador, o globalmente.</span><span class="sxs-lookup"><span data-stu-id="ee196-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="ee196-257">Las solicitudes realizadas a las acciones que se haya aplicado este filtro se bloquean a menos que la solicitud incluye un token antifalsificación válido.</span><span class="sxs-lookup"><span data-stu-id="ee196-257">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="ee196-258">El `ValidateAntiForgeryToken` atributo requiere un token para las solicitudes a los métodos de acción decora, incluidas las solicitudes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="ee196-258">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="ee196-259">Si el `ValidateAntiForgeryToken` atributo se aplica en todos los controladores de la aplicación, aunque puede reemplazarse con el `IgnoreAntiforgeryToken` atributo.</span><span class="sxs-lookup"><span data-stu-id="ee196-259">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="ee196-260">ASP.NET Core no admite agregar tokens antifalsificación automáticamente a las solicitudes GET.</span><span class="sxs-lookup"><span data-stu-id="ee196-260">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="ee196-261">Validar automáticamente los tokens antifalsificación para solo los métodos HTTP no seguros</span><span class="sxs-lookup"><span data-stu-id="ee196-261">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="ee196-262">Aplicaciones de ASP.NET Core no generan tokens antifalsificación para los métodos HTTP seguros (GET, HEAD, opciones y seguimiento).</span><span class="sxs-lookup"><span data-stu-id="ee196-262">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="ee196-263">En lugar de aplicar ampliamente el `ValidateAntiForgeryToken` atributo y, a continuación, invalidar con `IgnoreAntiforgeryToken` atributos, el [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) se puede usar el atributo.</span><span class="sxs-lookup"><span data-stu-id="ee196-263">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="ee196-264">Este atributo funciona de forma idéntica a la `ValidateAntiForgeryToken` atributo, salvo que no requiere tokens para las solicitudes realizadas mediante los siguientes métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="ee196-264">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="ee196-265">GET</span><span class="sxs-lookup"><span data-stu-id="ee196-265">GET</span></span>
* <span data-ttu-id="ee196-266">HEAD</span><span class="sxs-lookup"><span data-stu-id="ee196-266">HEAD</span></span>
* <span data-ttu-id="ee196-267">OPCIONES</span><span class="sxs-lookup"><span data-stu-id="ee196-267">OPTIONS</span></span>
* <span data-ttu-id="ee196-268">TRACE</span><span class="sxs-lookup"><span data-stu-id="ee196-268">TRACE</span></span>

<span data-ttu-id="ee196-269">Se recomienda usar `AutoValidateAntiforgeryToken` ampliamente para escenarios que no son API.</span><span class="sxs-lookup"><span data-stu-id="ee196-269">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="ee196-270">Esto garantiza que las acciones POST están protegidas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ee196-270">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="ee196-271">La alternativa consiste en pasar por alto los tokens antifalsificación de forma predeterminada, a menos que `ValidateAntiForgeryToken` se aplica a los métodos de acción individuales.</span><span class="sxs-lookup"><span data-stu-id="ee196-271">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="ee196-272">Lo más probable es que en este escenario para un método de acción POST debe dejar no esté por equivocación, salir de la aplicación sea vulnerable a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="ee196-272">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="ee196-273">Todas las publicaciones deben enviar el token antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="ee196-273">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="ee196-274">Las API no tienen un mecanismo automático para enviar la parte sin cookie del token.</span><span class="sxs-lookup"><span data-stu-id="ee196-274">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="ee196-275">Probablemente la implementación depende de la implementación del código de cliente.</span><span class="sxs-lookup"><span data-stu-id="ee196-275">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="ee196-276">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="ee196-276">Some examples are shown below:</span></span>

<span data-ttu-id="ee196-277">Ejemplo de nivel de clase:</span><span class="sxs-lookup"><span data-stu-id="ee196-277">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="ee196-278">Ejemplo global:</span><span class="sxs-lookup"><span data-stu-id="ee196-278">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="ee196-279">Reemplazo global o atributos de antifalsificación del controlador</span><span class="sxs-lookup"><span data-stu-id="ee196-279">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="ee196-280">El [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro se usa para eliminar la necesidad de un token antifalsificación para una determinada acción (o controlador).</span><span class="sxs-lookup"><span data-stu-id="ee196-280">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="ee196-281">Cuando se aplica, invalida este filtro `ValidateAntiForgeryToken` y `AutoValidateAntiforgeryToken` los filtros especificados en un nivel superior (globalmente o en un controlador).</span><span class="sxs-lookup"><span data-stu-id="ee196-281">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="ee196-282">Los tokens de actualización después de la autenticación</span><span class="sxs-lookup"><span data-stu-id="ee196-282">Refresh tokens after authentication</span></span>

<span data-ttu-id="ee196-283">Deben actualizar los tokens después de que el usuario se autentica al redirigir al usuario a una vista o página de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ee196-283">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="ee196-284">Las SPA, AJAX y JavaScript</span><span class="sxs-lookup"><span data-stu-id="ee196-284">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="ee196-285">En las aplicaciones tradicionales basadas en HTML, los tokens antifalsificación se pasan al servidor mediante los campos de formulario oculto.</span><span class="sxs-lookup"><span data-stu-id="ee196-285">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="ee196-286">En las aplicaciones modernas basadas en JavaScript y SPA, cuántas solicitudes se realizan mediante programación.</span><span class="sxs-lookup"><span data-stu-id="ee196-286">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="ee196-287">Estas solicitudes AJAX pueden usar otras técnicas (por ejemplo, los encabezados de solicitud o las cookies) para enviar el token.</span><span class="sxs-lookup"><span data-stu-id="ee196-287">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="ee196-288">Si se usan cookies para almacenar los tokens de autenticación y para autenticar las solicitudes de API en el servidor, CSRF es un posible problema.</span><span class="sxs-lookup"><span data-stu-id="ee196-288">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="ee196-289">Si se usa almacenamiento local para almacenar el token, se podrían mitigar vulnerabilidades CSRF porque los valores del almacenamiento local no se envían automáticamente al servidor con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="ee196-289">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="ee196-290">Por lo tanto, uso de almacenamiento local para almacenar el token antifalsificación en el cliente y enviar el token como un encabezado de solicitud es un enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="ee196-290">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="ee196-291">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ee196-291">JavaScript</span></span>

<span data-ttu-id="ee196-292">Con JavaScript con vistas, el token se puede crear mediante un servicio en la vista.</span><span class="sxs-lookup"><span data-stu-id="ee196-292">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="ee196-293">Insertar el [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) servicio en la vista y la llamada [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="ee196-293">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="ee196-294">Este enfoque elimina la necesidad de tratar directamente con la configuración de cookies desde el servidor o leerlos desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="ee196-294">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="ee196-295">El ejemplo anterior utiliza JavaScript para leer el valor del campo oculto para el encabezado de POST de AJAX.</span><span class="sxs-lookup"><span data-stu-id="ee196-295">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="ee196-296">JavaScript también puede tokens de acceso en las cookies y usar el contenido de la cookie para crear un encabezado con el valor del token.</span><span class="sxs-lookup"><span data-stu-id="ee196-296">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="ee196-297">Suponiendo que el script solicita para enviar el token en un encabezado denominado `X-CSRF-TOKEN`, configure el servicio antifalsificación para buscar el `X-CSRF-TOKEN` encabezado:</span><span class="sxs-lookup"><span data-stu-id="ee196-297">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="ee196-298">El ejemplo siguiente utiliza JavaScript para realizar una solicitud AJAX con el encabezado adecuado:</span><span class="sxs-lookup"><span data-stu-id="ee196-298">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="ee196-299">AngularJS</span><span class="sxs-lookup"><span data-stu-id="ee196-299">AngularJS</span></span>

<span data-ttu-id="ee196-300">AngularJS utiliza una convención para dirección CSRF.</span><span class="sxs-lookup"><span data-stu-id="ee196-300">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="ee196-301">Si el servidor envía una cookie con el nombre `XSRF-TOKEN`, AngularJS `$http` servicio agrega el valor de la cookie a un encabezado cuando envía una solicitud al servidor.</span><span class="sxs-lookup"><span data-stu-id="ee196-301">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="ee196-302">Este proceso es automático.</span><span class="sxs-lookup"><span data-stu-id="ee196-302">This process is automatic.</span></span> <span data-ttu-id="ee196-303">El encabezado no debe establecerse explícitamente en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ee196-303">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="ee196-304">El nombre del encabezado es `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="ee196-304">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="ee196-305">El servidor debería detectar este encabezado y validar su contenido.</span><span class="sxs-lookup"><span data-stu-id="ee196-305">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="ee196-306">Para la API de ASP.NET Core trabajar con esta convención en el inicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ee196-306">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="ee196-307">Configurar la aplicación para proporcionar un token en una cookie denominada `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="ee196-307">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="ee196-308">Configurar el servicio que se busca un encabezado denominado antifalsificación `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="ee196-308">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="ee196-309">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee196-309">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="ee196-310">Extender antifalsificación</span><span class="sxs-lookup"><span data-stu-id="ee196-310">Extend antiforgery</span></span>

<span data-ttu-id="ee196-311">El [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite a los desarrolladores extender el comportamiento del sistema anti-CSRF por los datos adicionales de ida y vuelta de cada token.</span><span class="sxs-lookup"><span data-stu-id="ee196-311">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="ee196-312">El [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) método se llama cada vez que se genera un token de campo y el valor devuelto está incrustado en el token generado.</span><span class="sxs-lookup"><span data-stu-id="ee196-312">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="ee196-313">Podría devolver una marca de tiempo, un valor nonce o cualquier otro valor y, a continuación, llamar a un implementador [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) para validar estos datos cuando se valida el token.</span><span class="sxs-lookup"><span data-stu-id="ee196-313">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="ee196-314">Nombre de usuario del cliente ya se han incrustado en los tokens generados, por lo que es necesario incluir esta información.</span><span class="sxs-lookup"><span data-stu-id="ee196-314">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="ee196-315">Si un token incluye datos suplementarios pero no `IAntiForgeryAdditionalDataProvider` está configurado, no se validan los datos suplementarios.</span><span class="sxs-lookup"><span data-stu-id="ee196-315">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee196-316">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ee196-316">Additional resources</span></span>

* <span data-ttu-id="ee196-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) en [Abrir Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="ee196-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
