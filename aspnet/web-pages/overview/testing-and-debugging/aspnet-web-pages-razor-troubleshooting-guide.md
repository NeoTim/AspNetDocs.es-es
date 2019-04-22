---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guía de solución de problemas (Razor) de ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe problemas que podría tener cuando se trabaja con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas. Versiones de software ASP.NET Web Pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: adbaa5cbda4a60a8b222ba49bb148b28b2e214cc
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59389211"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guía de solución de problemas de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe problemas que podría tener cuando se trabaja con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas.
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y las páginas Web de ASP.NET 1.0.


Este tema contiene las siguientes secciones:

- [Problemas con la ejecución de páginas](#Issues_Running_.cshtml_Pages)
- [Problemas con el código de Razor](#IssuesWithRazorCode)
- [Problemas con la seguridad y pertenencia](#membership)
- [Problemas con el envío de correo electrónico](#email)
- [Recursos adicionales](#AdditionalResources)

Para preguntas generales, vea [ASP.NET Web Pages (Razor) preguntas más frecuentes sobre](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemas con la ejecución de páginas

Puede evitar que una variedad de problemas *.cshtml* y *.vbhtml* páginas se ejecuten correctamente. En esta sección se enumera los mensajes de error comunes y causas probables.

### <a name="http-error-403---forbidden-access-is-denied"></a>Error HTTP 403 - Prohibido: Se denegó el acceso

*No tiene permiso para ver este directorio o esta página con las credenciales proporcionadas.*

Este error puede producirse si el servidor no se está ejecutando la versión correcta de .NET Framework. Asegúrese de que el equipo que ejecuta el servidor (local o remotamente) tiene al menos .NET Framework 4 instalado. Además, asegúrese de que la propia aplicación se configura para ejecutar la versión correcta.

Si ve este problema localmente mientras trabaja en WebMatrix, haga clic en el **sitio** área de trabajo y en la vista de árbol, después, haga clic en **configuración**. En el **seleccione la versión de .NET Framework** , seleccione **(integrado) de .NET 4**. Si esta versión ya está configurada, intente ejecutar WebMatrix como administrador.

Asegúrese de que la raíz de su sitio Web tiene al menos una *.cshtml* archivos en ella.

Si ve este error cuando el servidor web en un servidor remoto, póngase en contacto con el administrador del servidor. Asegúrese de que el servidor tenga .NET Framework 4 o posterior instalado. Además, asegúrese de que la aplicación se está ejecutando en un grupo de aplicaciones está configurado para usar esa versión de.NET Framework.

Si tiene control sobre el servidor, asegúrese de que se está ejecutando la versión correcta de .NET Framework. También puede intentar reparar la instalación ejecutando el `aspnet_regiis -iru` comando. (Por ejemplo, si instala IIS después de instalar .NET Framework, IIS no se configurarse correctamente para ejecutar las páginas ASP.NET.) Para obtener más información, consulte [herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Error HTTP 403.14 - prohibido

*El servidor Web está configurado para no mostrar el contenido de este directorio.*

Este error puede producirse si se solicita un recurso protegido (como el *Web.config* archivo) o que se encuentra en una carpeta que está protegida (como *aplicación\_datos* o *aplicación\_Código*).

### <a name="http-error-40417---not-found"></a>Error HTTP 404.17 - no encontrado

*El contenido solicitado aparece como una secuencia de comandos y no se enviarán por el controlador de archivos estáticos.*

Este error puede producirse si el servidor no está configurado correctamente para usar .NET Framework 4 o posterior y, por tanto, no reconoce el código en `@{ }` bloques. Consulte la descripción anterior de *Error HTTP 403 - Prohibido: Se denegó el acceso*.

### <a name="http-error-4047---not-found"></a>No se encontró un Error HTTP 404.7:

*El módulo de filtrado de solicitudes está configurado para denegar la extensión de archivo*

Este error puede producirse si *.cshtml* o *.vbhtml* extensiones se han bloqueado explícitamente en el servidor. Un síntoma de este problema es que funcionan las direcciones URL cuando no incluye la extensión, pero las direcciones URL que incluyen *.cshtml* o *.vbhtml* no funcionan. Una posible solución consiste en volver a habilitar las extensiones en la carpeta del sitio *Web.config* archivo. El ejemplo siguiente muestra cómo habilitar el *.cshtml* extensión.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>No se encontró un Error HTTP 404.8:

*El módulo de filtrado de solicitudes está configurado para denegar una ruta de acceso en la dirección URL que contiene una sección hiddenSegment.*

Este error puede producirse si se solicita un recurso protegido (como el *Web.config* archivo) o que se encuentra en una carpeta que está protegida (como *aplicación\_datos* o *aplicación\_Código*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Este tipo de página no disponible (Error de servidor en la aplicación '/')

Consulte la descripción anterior de HTTP 404.17 de Error.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemas con el código de Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>El nombre '*clase*' no existe en el contexto actual

A menudo, una razón ve este error es que `class` las referencias de una aplicación auxiliar, pero la aplicación auxiliar no está instalado. Por ejemplo, si intenta usar una aplicación auxiliar, pero si no ha instalado el paquete de NuGet, verá este error. Usar la galería en WebMatrix para buscar e instalar la aplicación auxiliar.

Si se instala la aplicación auxiliar, pero la página aún no lo reconoce, intente agregar un `using` instrucción en el código. En el `using` instrucción, referencia de espacio de nombres que incluye la aplicación auxiliar. Por ejemplo, las aplicaciones auxiliares básicas que se encuentran en el paquete de aplicaciones auxiliares de ASP.NET Web están en el `System.Web.Helpers` espacio de nombres. En la parte superior de la página donde desea usar la aplicación auxiliar, agregue esta línea:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemas con la seguridad y pertenencia

Si utiliza el sistema de seguridad integrada (pertenencia) en ASP.NET Web Pages (Razor), pueden surgir los siguientes problemas.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Para llamar a este método, la propiedad "Membership.Provider" debe ser una instancia de "ExtendedMembershipProvider"

Este error puede indicar que no hay `AspNetSqlMembershipProvider` clase está configurada. (Un síntoma es que el sitio funciona bien localmente, pero este error produce cuando se publica en el servidor de un proveedor de hospedaje). Una corrección para este problema consiste en habilitar explícitamente la pertenencia sencilla agregando lo siguiente en el sitio *Web.config* archivo:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemas con el envío de correo electrónico

Problemas con el envío de correo electrónico pueden resultar complicado para depurar. Un problema inicial puede ser que no se puede conectar al servidor SMTP. Si la conexión es correcta, ASP.NET entrega el mensaje al servidor SMTP. Sin embargo, puede haber problemas con el propio mensaje que impide que el servidor SMTP de enviarlo.

Si la aplicación no envía correo electrónico correctamente, intente lo siguiente:

- El nombre del servidor SMTP es a menudo algo como `smtp.provider.com` o `smtp.provider.net`. Sin embargo, si publica el sitio en un proveedor de hospedaje, el nombre del servidor SMTP en ese momento podría ser `localhost`. Esta situación se produce porque una vez que ha publicado y el sitio se ejecuta en el servidor del proveedor, el servidor SMTP puede ser local desde la perspectiva de la aplicación. Este cambio en los nombres de servidor podría significar que tiene que cambiar el nombre del servidor SMTP como parte del proceso de publicación.
- Normalmente, el número de puerto es 25. Sin embargo, algunos proveedores requieren que se use el puerto 587 o algún otro puerto. Con el propietario del servidor SMTP, compruebe qué número de puerto que esperan que se va a usar.
- Asegúrese de que use las credenciales correctas. Si ha publicado su sitio en un proveedor de hospedaje, use las credenciales que el proveedor ha indicado específicamente son para el correo electrónico. Estas credenciales pueden diferir de las credenciales que use para publicar.
- A veces no necesita las credenciales en absoluto. Si va a enviar correo electrónico mediante el uso de su ISP personal, el proveedor de correo electrónico es posible que ya conoce sus credenciales. Después de publicar, es posible que deba usar credenciales diferentes a cuando se prueban en el equipo local.
- Si su proveedor de correo electrónico usa cifrado, establezca `WebMail.EnableSsl` a `true`.

Si hay un error al enviar correo electrónico, puede aparecer un mensaje de error ASP.NET estándar, que tiene este aspecto:

![Mensaje de error ASP.NET cuando hay un problema con el correo electrónico](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

También puede depurar problemas con el envío de correo electrónico mediante el uso de un `try-catch` bloque, como en el ejemplo siguiente. Cuando se usa un `try-catch` bloque, ASP.NET no muestra sus mensajes de error estándar. En su lugar, puede capturar el error en la `catch` parte del bloque.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Sustituya los valores adecuados para `your-SMTP-server-name`, y así sucesivamente. Algunos de los mensajes de error que puede aparecer de esta manera incluyen lo siguiente:

- *Error al enviar correo.*

    -o bien-

    *Un intento de conexión porque la parte conectada no respondió adecuadamente tras un período de tiempo o conexión establecida porque el host conectado no respondió*

    Este error normalmente significa que la aplicación no se pudo conectar al servidor SMTP. Compruebe el nombre del servidor y número de puerto.
- *Buzón no disponible. La respuesta del servidor fue: 5.1.0 &lt; someuser@invaliddomain &gt; remitente rechazado: dominio del remitente no válido*

    Este mensaje puede indicar que el `From` dirección no es correcta o falta.
- *La cadena especificada no es de la forma necesaria para una dirección de correo electrónico.*

    Este error puede indicar que el valor de la `To` o `From` propiedades no se reconocen como direcciones de correo electrónico. (ASP.NET no puede comprobar que la dirección de correo electrónico es válida, solo del dentro el formato correcto, como *name@domain.com*.)

> [!NOTE]
> Quite el código que muestra el error (`@errorMessage`) antes de publicar la página en un sitio activo. No es una buena idea permitir que los usuarios ver los mensajes de error que obtienen de un servidor.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Preguntas frecuentes de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix y ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) foro en el sitio Web ASP.NET
