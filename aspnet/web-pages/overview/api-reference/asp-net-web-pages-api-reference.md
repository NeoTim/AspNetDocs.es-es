---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Referencia rápida de la API de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Esta página contiene una lista con ejemplos breves de los objetos, propiedades y métodos más utilizados para programar ASP.NET Web Pages con sintaxis Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463585"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="a5972-103">Referencia rápida de la API de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="a5972-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>

<span data-ttu-id="a5972-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a5972-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a5972-105">Esta página contiene una lista con ejemplos breves de los objetos, propiedades y métodos más utilizados para programar ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="a5972-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="a5972-106">Las descripciones marcadas con "(V2)" se introdujeron en ASP.NET Web Pages versión 2.</span><span class="sxs-lookup"><span data-stu-id="a5972-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="a5972-107">Para obtener documentación de referencia de API, consulte la [documentación de referencia de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=208659) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="a5972-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="a5972-108">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="a5972-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="a5972-109">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a5972-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a5972-110">Este tutorial también funciona con ASP.NET Web Pages 2 y ASP.NET Web Pages 1,0 (excepto para las características marcadas como V2).</span><span class="sxs-lookup"><span data-stu-id="a5972-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>

<span data-ttu-id="a5972-111">Esta página contiene información de referencia para lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a5972-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="a5972-112">Clases</span><span class="sxs-lookup"><span data-stu-id="a5972-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="a5972-113">Data</span><span class="sxs-lookup"><span data-stu-id="a5972-113">Data</span></span>](#Data)
- [<span data-ttu-id="a5972-114">Aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="a5972-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="a5972-115">Validación</span><span class="sxs-lookup"><span data-stu-id="a5972-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="a5972-116">Clases</span><span class="sxs-lookup"><span data-stu-id="a5972-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="a5972-117">Contiene datos que se pueden compartir con cualquier página de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5972-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="a5972-118">Puede usar la propiedad `App` dinámica para tener acceso a los mismos datos, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a5972-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="a5972-119">Convierte un valor de cadena en un valor booleano (true/false).</span><span class="sxs-lookup"><span data-stu-id="a5972-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="a5972-120">Devuelve false o el valor especificado si la cadena no representa true/false.</span><span class="sxs-lookup"><span data-stu-id="a5972-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="a5972-121">Convierte un valor de cadena en una fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="a5972-121">Converts a string value to date/time.</span></span> <span data-ttu-id="a5972-122">Devuelve `DateTime.MinValue` o el valor especificado si la cadena no representa una fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="a5972-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="a5972-123">Convierte un valor de cadena en un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="a5972-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="a5972-124">Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="a5972-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="a5972-125">Convierte un valor de cadena en un valor de tipo float.</span><span class="sxs-lookup"><span data-stu-id="a5972-125">Converts a string value to a float.</span></span> <span data-ttu-id="a5972-126">Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="a5972-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="a5972-127">Convierte un valor de cadena en un entero.</span><span class="sxs-lookup"><span data-stu-id="a5972-127">Converts a string value to an integer.</span></span> <span data-ttu-id="a5972-128">Devuelve 0 o el valor especificado si la cadena no representa un entero.</span><span class="sxs-lookup"><span data-stu-id="a5972-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="a5972-129">Crea una dirección URL compatible con el explorador a partir de una ruta de acceso de archivo local, con partes de ruta de acceso adicionales opcionales.</span><span class="sxs-lookup"><span data-stu-id="a5972-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="a5972-130">Representa el *valor* como marcado HTML en lugar de representarlo como salida codificada en HTML.</span><span class="sxs-lookup"><span data-stu-id="a5972-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="a5972-131">Devuelve true si el valor se puede convertir de una cadena al tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="a5972-132">Devuelve true si el objeto o la variable no tiene ningún valor.</span><span class="sxs-lookup"><span data-stu-id="a5972-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="a5972-133">Devuelve true si la solicitud es una publicación.</span><span class="sxs-lookup"><span data-stu-id="a5972-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="a5972-134">(Las solicitudes iniciales suelen ser GET).</span><span class="sxs-lookup"><span data-stu-id="a5972-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="a5972-135">Especifica la ruta de acceso de una página de diseño que se va a aplicar a esta página.</span><span class="sxs-lookup"><span data-stu-id="a5972-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="a5972-136">Contiene los datos compartidos entre la página, las páginas de diseño y las páginas parciales de la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="a5972-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="a5972-137">Puede usar la propiedad `Page` dinámica para tener acceso a los mismos datos, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a5972-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="a5972-138">(Páginas de diseño) Representa el contenido de una página de contenido que no está en ninguna sección con nombre.</span><span class="sxs-lookup"><span data-stu-id="a5972-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="a5972-139">Representa una página de contenido mediante la ruta de acceso especificada y datos adicionales opcionales.</span><span class="sxs-lookup"><span data-stu-id="a5972-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="a5972-140">Puede obtener los valores de los parámetros adicionales de `PageData` por posición (ejemplo 1) o clave (ejemplo 2).</span><span class="sxs-lookup"><span data-stu-id="a5972-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="a5972-141">(Páginas de diseño) Representa una sección de contenido que tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="a5972-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="a5972-142">Establezca *requerido* en false para que una sección sea opcional.</span><span class="sxs-lookup"><span data-stu-id="a5972-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="a5972-143">Obtiene o establece el valor de una cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5972-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="a5972-144">Obtiene los archivos que se cargaron en la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="a5972-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="a5972-145">Obtiene los datos que se publicaron en un formulario (como cadenas).</span><span class="sxs-lookup"><span data-stu-id="a5972-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="a5972-146">`Request[key]` comprueba las colecciones `Request.Form` y `Request.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="a5972-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="a5972-147">Obtiene los datos que se especificaron en la cadena de consulta de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a5972-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="a5972-148">`Request[key]` comprueba las colecciones `Request.Form` y `Request.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="a5972-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="a5972-149">Deshabilita de forma selectiva la validación de solicitudes para un elemento de formulario, un valor de cadena de consulta, una cookie o un valor de encabezado.</span><span class="sxs-lookup"><span data-stu-id="a5972-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="a5972-150">La validación de solicitudes está habilitada de forma predeterminada e impide que los usuarios publiquen marcado u otro contenido potencialmente peligroso.</span><span class="sxs-lookup"><span data-stu-id="a5972-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="a5972-151">Agrega un encabezado de servidor HTTP a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a5972-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="a5972-152">Almacena en caché el resultado de la página durante un tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="a5972-153">Opcionalmente, establezca la *variable* para restablecer el tiempo de espera en cada acceso de página y *VaryByParams* para almacenar en caché diferentes versiones de la página para cada cadena de consulta diferente en la solicitud de la página.</span><span class="sxs-lookup"><span data-stu-id="a5972-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="a5972-154">Redirige la solicitud del explorador a una nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="a5972-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="a5972-155">Establece el código de Estado HTTP que se envía al explorador.</span><span class="sxs-lookup"><span data-stu-id="a5972-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="a5972-156">Escribe el contenido de los *datos* en la respuesta con un tipo MIME opcional.</span><span class="sxs-lookup"><span data-stu-id="a5972-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="a5972-157">Escribe el contenido de un archivo en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a5972-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="a5972-158">(Páginas de diseño) Define una sección de contenido que tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="a5972-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="a5972-159">Descodifica una cadena codificada en HTML.</span><span class="sxs-lookup"><span data-stu-id="a5972-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="a5972-160">Codifica una cadena para la representación en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="a5972-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="a5972-161">Devuelve la ruta de acceso física del servidor para la ruta de acceso virtual especificada.</span><span class="sxs-lookup"><span data-stu-id="a5972-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="a5972-162">Descodifica texto desde una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a5972-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="a5972-163">Codifica el texto que se va a colocar en una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a5972-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="a5972-164">Obtiene o establece un valor que existe hasta que el usuario cierra el explorador.</span><span class="sxs-lookup"><span data-stu-id="a5972-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="a5972-165">Muestra una representación de cadena del valor del objeto.</span><span class="sxs-lookup"><span data-stu-id="a5972-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="a5972-166">Obtiene datos adicionales de la dirección URL (por ejemplo, */MYPAGE/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="a5972-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="a5972-167">Cambia la contraseña del usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="a5972-168">Confirma una cuenta mediante el token de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="a5972-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="a5972-169">Crea una nueva cuenta de usuario con el nombre de usuario y la contraseña especificados.</span><span class="sxs-lookup"><span data-stu-id="a5972-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="a5972-170">Para requerir un token de confirmación, pase true para *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="a5972-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="a5972-171">Obtiene el identificador entero para el usuario que ha iniciado sesión actualmente.</span><span class="sxs-lookup"><span data-stu-id="a5972-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="a5972-172">Obtiene el nombre del usuario que ha iniciado sesión actualmente.</span><span class="sxs-lookup"><span data-stu-id="a5972-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="a5972-173">Genera un token de restablecimiento de contraseña que se puede enviar por correo electrónico a un usuario para que el usuario pueda restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="a5972-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="a5972-174">Devuelve el ID. de usuario del nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="a5972-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="a5972-175">Devuelve true si el usuario actual ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="a5972-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="a5972-176">Devuelve true si el usuario se ha confirmado (por ejemplo, a través de un correo electrónico de confirmación).</span><span class="sxs-lookup"><span data-stu-id="a5972-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="a5972-177">Devuelve true si el nombre del usuario actual coincide con el nombre de usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="a5972-178">Inicia la sesión del usuario mediante el establecimiento de un token de autenticación en la cookie.</span><span class="sxs-lookup"><span data-stu-id="a5972-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="a5972-179">Cierra la sesión del usuario quitando la cookie del token de autenticación.</span><span class="sxs-lookup"><span data-stu-id="a5972-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="a5972-180">Si el usuario no está autenticado, establece el estado de HTTP en 401 (No autorizado).</span><span class="sxs-lookup"><span data-stu-id="a5972-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="a5972-181">Si el usuario actual no es miembro de uno de los roles especificados, establece el estado de HTTP en 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="a5972-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="a5972-182">Si el usuario actual no es el usuario especificado por el *nombre*de usuario, establece el estado de HTTP en 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="a5972-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="a5972-183">Si el token de restablecimiento de contraseña es válido, cambia la contraseña del usuario a la nueva contraseña.</span><span class="sxs-lookup"><span data-stu-id="a5972-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="a5972-184">Datos</span><span class="sxs-lookup"><span data-stu-id="a5972-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="a5972-185">Ejecuta *SQLstatement* (con parámetros opcionales) como INSERT, delete o Update y devuelve un recuento de los registros afectados.</span><span class="sxs-lookup"><span data-stu-id="a5972-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="a5972-186">Devuelve la columna de identidad a partir de la fila insertada más recientemente.</span><span class="sxs-lookup"><span data-stu-id="a5972-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="a5972-187">Abre el archivo de base de datos especificado o la base de datos especificada mediante una cadena de conexión con nombre del archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="a5972-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="a5972-188">Abre una base de datos con la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="a5972-188">Opens a database using the connection string.</span></span> <span data-ttu-id="a5972-189">(Esto contrasta con `Database.Open`, que usa un nombre de cadena de conexión).</span><span class="sxs-lookup"><span data-stu-id="a5972-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="a5972-190">Consulta la base de datos mediante *SQLstatement* (opcionalmente, pasando parámetros) y devuelve los resultados como una colección.</span><span class="sxs-lookup"><span data-stu-id="a5972-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="a5972-191">Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un solo registro.</span><span class="sxs-lookup"><span data-stu-id="a5972-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="a5972-192">Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un valor único.</span><span class="sxs-lookup"><span data-stu-id="a5972-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="a5972-193">Asistentes</span><span class="sxs-lookup"><span data-stu-id="a5972-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="a5972-194">Representa el código JavaScript de Google Analytics para el ID. especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="a5972-195">Representa el código JavaScript de StatCounter Analytics para el proyecto especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="a5972-196">Representa el código JavaScript de Yahoo Analytics de la cuenta especificada.</span><span class="sxs-lookup"><span data-stu-id="a5972-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="a5972-197">Pasa una búsqueda a Bing.</span><span class="sxs-lookup"><span data-stu-id="a5972-197">Passes a search to Bing.</span></span> <span data-ttu-id="a5972-198">Para especificar el sitio en el que se va a buscar y un título para el cuadro de búsqueda, puede establecer las propiedades `Bing.SiteUrl` y `Bing.SiteTitle`.</span><span class="sxs-lookup"><span data-stu-id="a5972-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="a5972-199">Normalmente, estas propiedades se establecen en la página *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="a5972-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="a5972-200">Inicializa un gráfico.</span><span class="sxs-lookup"><span data-stu-id="a5972-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="a5972-201">Agrega una leyenda a un gráfico.</span><span class="sxs-lookup"><span data-stu-id="a5972-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="a5972-202">Agrega una serie de valores al gráfico.</span><span class="sxs-lookup"><span data-stu-id="a5972-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="a5972-203">Devuelve un valor hash para los datos especificados.</span><span class="sxs-lookup"><span data-stu-id="a5972-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="a5972-204">El algoritmo predeterminado es `sha256`.</span><span class="sxs-lookup"><span data-stu-id="a5972-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="a5972-205">Permite que los usuarios de Facebook realicen una conexión a las páginas.</span><span class="sxs-lookup"><span data-stu-id="a5972-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="a5972-206">Representa la interfaz de usuario para cargar archivos.</span><span class="sxs-lookup"><span data-stu-id="a5972-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="a5972-207">Representa la etiqueta de jugador de Xbox especificada.</span><span class="sxs-lookup"><span data-stu-id="a5972-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="a5972-208">Representa la imagen de Gravatar para la dirección de correo electrónico especificada.</span><span class="sxs-lookup"><span data-stu-id="a5972-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="a5972-209">Convierte un objeto de datos en una cadena en el formato notación de objetos JavaScript (JSON).</span><span class="sxs-lookup"><span data-stu-id="a5972-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="a5972-210">Convierte una cadena de entrada codificada en JSON en un objeto de datos que se puede iterar o insertar en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="a5972-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="a5972-211">Representa los vínculos de redes sociales mediante el título especificado y la dirección URL opcional.</span><span class="sxs-lookup"><span data-stu-id="a5972-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="a5972-212">Asocia un mensaje de error a un campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="a5972-212">Associates an error message with a form field.</span></span> <span data-ttu-id="a5972-213">Utilice la aplicación auxiliar `ModelState` para tener acceso a este miembro.</span><span class="sxs-lookup"><span data-stu-id="a5972-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="a5972-214">Asocia un mensaje de error a un formulario.</span><span class="sxs-lookup"><span data-stu-id="a5972-214">Associates an error message with a form.</span></span> <span data-ttu-id="a5972-215">Utilice la aplicación auxiliar `ModelState` para tener acceso a este miembro.</span><span class="sxs-lookup"><span data-stu-id="a5972-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="a5972-216">Devuelve true si no hay ningún error de validación.</span><span class="sxs-lookup"><span data-stu-id="a5972-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="a5972-217">Utilice la aplicación auxiliar `ModelState` para tener acceso a este miembro.</span><span class="sxs-lookup"><span data-stu-id="a5972-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="a5972-218">Representa las propiedades y los valores de un objeto y los objetos secundarios.</span><span class="sxs-lookup"><span data-stu-id="a5972-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="a5972-219">Representa la prueba de comprobación reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="a5972-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="a5972-220">Establece claves públicas y privadas para el servicio reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="a5972-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="a5972-221">Normalmente, estas propiedades se establecen en la página *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="a5972-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="a5972-222">Devuelve el resultado de la prueba de reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="a5972-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="a5972-223">Representa información de estado sobre ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="a5972-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="a5972-224">Representa una secuencia de Twitter para el usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="a5972-225">Representa una secuencia de Twitter para el texto de búsqueda especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="a5972-226">Representa un reproductor de vídeo Flash para el archivo especificado con un ancho y un alto opcionales.</span><span class="sxs-lookup"><span data-stu-id="a5972-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="a5972-227">Representa un reproductor de Windows Media para el archivo especificado con el ancho y el alto opcionales.</span><span class="sxs-lookup"><span data-stu-id="a5972-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="a5972-228">Representa un reproductor de Silverlight para el archivo *. xap* especificado con el ancho y el alto requeridos.</span><span class="sxs-lookup"><span data-stu-id="a5972-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="a5972-229">Devuelve el objeto especificado por la *clave*o null si no se encuentra el objeto.</span><span class="sxs-lookup"><span data-stu-id="a5972-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="a5972-230">Quita el objeto especificado por la *clave* de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="a5972-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="a5972-231">Coloca el *valor* en la memoria caché con el nombre especificado por *key*.</span><span class="sxs-lookup"><span data-stu-id="a5972-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="a5972-232">Crea un nuevo objeto de `WebGrid` con los datos de una consulta.</span><span class="sxs-lookup"><span data-stu-id="a5972-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="a5972-233">Representa el marcado para mostrar los datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="a5972-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="a5972-234">Representa un buscapersonas para el objeto `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="a5972-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="a5972-235">Carga una imagen desde la ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="a5972-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="a5972-236">Agrega la imagen especificada como una marca de agua.</span><span class="sxs-lookup"><span data-stu-id="a5972-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="a5972-237">Agrega el texto especificado a la imagen.</span><span class="sxs-lookup"><span data-stu-id="a5972-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="a5972-238">Voltea la imagen horizontal o verticalmente.</span><span class="sxs-lookup"><span data-stu-id="a5972-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="a5972-239">Carga una imagen cuando se envía una imagen a una página durante una carga de archivos.</span><span class="sxs-lookup"><span data-stu-id="a5972-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="a5972-240">Cambia el tamaño de una imagen.</span><span class="sxs-lookup"><span data-stu-id="a5972-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="a5972-241">Gira la imagen a la izquierda o a la derecha.</span><span class="sxs-lookup"><span data-stu-id="a5972-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="a5972-242">Guarda la imagen en la ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="a5972-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="a5972-243">Establece la contraseña para el servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="a5972-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="a5972-244">Normalmente, esta propiedad se establece en la página *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="a5972-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="a5972-245">Envía un mensaje de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="a5972-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="a5972-246">Establece el nombre del servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="a5972-246">Sets the SMTP server name.</span></span> <span data-ttu-id="a5972-247">Normalmente, esta propiedad se establece en la página *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="a5972-247">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="a5972-248">Establece el nombre de usuario del servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="a5972-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="a5972-249">Normalmente, debe establecer esta propiedad en la página *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="a5972-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="a5972-250">Validación</span><span class="sxs-lookup"><span data-stu-id="a5972-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="a5972-251">2 Representa un mensaje de error de validación para el campo especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="a5972-252">2 Muestra una lista de todos los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="a5972-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="a5972-253">2 Registra un elemento de entrada de usuario para el tipo de validación especificado.</span><span class="sxs-lookup"><span data-stu-id="a5972-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="a5972-254">2 Representa dinámicamente los atributos de clase CSS para la validación del lado cliente para que pueda dar formato a los mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="a5972-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="a5972-255">(Requiere que se haga referencia a las bibliotecas de scripts de cliente adecuadas y que se definan las clases CSS).</span><span class="sxs-lookup"><span data-stu-id="a5972-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="a5972-256">2 Habilita la validación del lado cliente para el campo de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="a5972-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="a5972-257">(Requiere que haga referencia a las bibliotecas de scripts de cliente adecuadas).</span><span class="sxs-lookup"><span data-stu-id="a5972-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="a5972-258">2 Devuelve true si todos los elementos de entrada de usuario que se registran para la validación contienen valores válidos.</span><span class="sxs-lookup"><span data-stu-id="a5972-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="a5972-259">2 Especifica que los usuarios deben proporcionar un valor para el elemento de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="a5972-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="a5972-260">2 Especifica que los usuarios deben proporcionar valores para cada uno de los elementos de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="a5972-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="a5972-261">Este método no permite especificar un mensaje de error personalizado.</span><span class="sxs-lookup"><span data-stu-id="a5972-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="a5972-262">2 Especifica una prueba de validación cuando se usa el método `Validation.Add`.</span><span class="sxs-lookup"><span data-stu-id="a5972-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
