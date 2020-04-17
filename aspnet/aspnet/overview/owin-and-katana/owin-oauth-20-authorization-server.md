---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Servidor de autorización OWIN OAuth 2.0 Microsoft Docs
author: hongyes
description: Este tutorial le guiará sobre cómo implementar un servidor de autorización de OAuth 2.0 mediante el middleware OWIN OAuth. Este es un tutorial avanzado que sólo outlin...
ms.author: riande
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: d758fa2639d10e1b7be8d87c5d1f7adb7e292ac7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540429"
---
<a name="owin-oauth-20-authorization-server"></a>Servidor de autorización de OAuth 2.0 de OWIN
====================
por [Hongye Sun](https://github.com/hongyes) y [Praburaj Thiagarajan](https://github.com/Praburaj)

> Este tutorial le guiará sobre cómo implementar un servidor de autorización de OAuth 2.0 mediante el middleware OWIN OAuth. Este es un tutorial avanzado que solo describe los pasos para crear un servidor de autorización OWIN OAuth 2.0. Este no es un tutorial paso a paso. [Descargue el código de ejemplo](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
>
> > [!NOTE]
> > Este esquema no debe estar pensado para usarse para crear una aplicación de producción segura. Este tutorial está diseñado para proporcionar solo un esquema sobre cómo implementar un servidor de autorización de OAuth 2.0 mediante el middleware OWIN OAuth.
>
>
> ## <a name="software-versions"></a>Versiones de software
>
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) | [Visual Studio 2013 Express para escritorio](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express). Visual Studio 2012 con la actualización más reciente debería funcionar, pero el tutorial no se ha probado con él, y algunas selecciones de menú y cuadros de diálogo son diferentes. |
> | .NET 4.5 |  |
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en [Katana Project en GitHub](https://github.com/aspnet/AspNetKatana/). Para preguntas y comentarios sobre el tutorial en sí, consulte la sección de comentarios en la parte inferior de la página.


El marco de trabajo de [OAuth 2.0](http://tools.ietf.org/html/rfc6749) permite que una aplicación de terceros obtenga acceso limitado a un servicio HTTP. En lugar de usar las credenciales del propietario del recurso para tener acceso a un recurso protegido, el cliente obtiene un token de acceso (que es una cadena que indica un ámbito, una duración y otros atributos de acceso específicos). Los tokens de acceso se emiten a clientes de terceros mediante un servidor de autorización con la aprobación del propietario del recurso.

Este tutorial cubrirá:

- Cómo crear un servidor de autorización para admitir cuatro tipos de concesión de autorización y actualizar tokens:
    - Concesión del código de autorización
    - Subvención implícita
    - Concesión de credenciales de contraseña de propietario de recursos
    - Concesión de credenciales de cliente
- Creación de un servidor de recursos protegido por un token de acceso.
- Creación de clientes de OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerrequisitos

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) o [visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)gratuito, como se indica en **Versiones** de software en la parte superior de la página.
- Familiaridad con OWIN. Consulte [Introducción al proyecto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) y [Novedades de OWIN y Katana](index.md).
- Familiaridad con la terminología de [OAuth,](http://tools.ietf.org/html/rfc6749) incluidos [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [Flujo de protocolo](http://tools.ietf.org/html/rfc6749#section-1.2)y Concesión de [autorización](http://tools.ietf.org/html/rfc6749#section-1.3). [La introducción de OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) proporciona una buena introducción.

## <a name="create-an-authorization-server"></a>Crear un servidor de autorización

En este tutorial, esbozaremos aproximadamente cómo usar [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) y ASP.NET MVC para crear un servidor de autorización. Esperamos proporcionar pronto una descarga para el ejemplo completado, ya que este tutorial no incluye cada paso. En primer lugar, cree una aplicación web vacía denominada *AuthorizationServer* e instale los siguientes paquetes:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (O cualquier otro paquete de inicio de sesión social como Microsoft.Owin.Security.Facebook)

Agregue una [clase de inicio de OWIN](owin-startup-class-detection.md) en la carpeta raíz del proyecto denominada *Inicio*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Cree una carpeta de inicio de *aplicación.\_* Seleccione la carpeta Inicio de la *aplicación\_* y use Mayús+Alt+A para agregar la versión descargada del archivo *AuthorizationServer-App\_Start-Startup.Auth.cs.*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

El código anterior habilita las cookies de inicio de sesión de aplicaciones/externas y la autenticación de Google, que son utilizadas por el propio servidor de autorización para administrar cuentas.

El `UseOAuthAuthorizationServer` método de extensión consiste en configurar el servidor de autorización. Las opciones de configuración son:

- `AuthorizeEndpointPath`: la ruta de acceso de solicitud donde las aplicaciones cliente redirigirán el agente de usuario con el fin de obtener el consentimiento de los usuarios para emitir un token o código. Debe comenzar con una barra diagonal`/Authorize`inicial, por ejemplo, "..
- `TokenEndpointPath`: las aplicaciones cliente de ruta de acceso de solicitud se comunican directamente para obtener el token de acceso. Debe comenzar con una barra diagonal inicial, como "/Token". Si se emite un secreto de [cliente\_](http://tools.ietf.org/html/rfc6749#appendix-A.2)al cliente, se debe proporcionar a este punto de conexión.
- `ApplicationCanDisplayErrors`: se `true` establece en si la aplicación web desea generar `/Authorize` una página de error personalizada para los errores de validación de cliente en el punto de conexión. Esto sólo es necesario para los casos en los que el explorador `client_id` no `redirect_uri` se redirige de nuevo a la aplicación cliente, por ejemplo, cuando el o son incorrectos. El `/Authorize` punto final debe esperar ver el "oauth. Error", "oauth. ErrorDescription" y "oauth. ErrorUri" se agregan al entorno OWIN.

    > [!NOTE]
    > Si no es true, el servidor de autorización devolverá una página de error predeterminada con los detalles del error.
- `AllowInsecureHttp`: True para permitir que las solicitudes de autorización `redirect_uri` y token lleguen a direcciones URI HTTP y para permitir que los parámetros de solicitud de autorización entrantes tengan direcciones URI HTTP.

    > [!WARNING]
    > Seguridad - Esto es sólo para el desarrollo.
- `Provider`: el objeto proporcionado por la aplicación para procesar los eventos generados por el middleware del servidor de autorización. La aplicación puede implementar la interfaz completamente, `OAuthAuthorizationServerProvider` o puede crear una instancia y asignar delegados necesarios para los flujos de OAuth que admite este servidor.
- `AuthorizationCodeProvider`: produce un código de autorización de un solo uso para volver a la aplicación cliente. Para que el servidor de OAuth sea `AuthorizationCodeProvider` seguro, la aplicación `OnCreate/OnCreateAsync` **DEBE** proporcionar una instancia para `OnReceive/OnReceiveAsync`donde el token generado por el evento se considera válido para una sola llamada a .
- `RefreshTokenProvider`: produce un token de actualización que se puede utilizar para producir un nuevo token de acceso cuando sea necesario. Si no se proporciona, el servidor `/Token` de autorización no devolverá tokens de actualización del punto de conexión.

## <a name="account-management"></a>Administración de cuentas

A OAuth no le importa dónde ni cómo administre la información de su cuenta de usuario. Es [ASP.NET identidad](../../../identity/index.md) la que es responsable de ello. En este tutorial, simplificaremos el código de administración de la cuenta y solo nos aseguraremos de que el usuario pueda iniciar sesión mediante el middleware de cookies OWIN. Aquí está el código `AccountController`de ejemplo simplificado para:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`se utiliza para validar al cliente con su URL de redireccionamiento registrada. `ValidateClientAuthentication`comprueba el encabezado de esquema básico y el cuerpo del formulario para obtener las credenciales del cliente.

La página de inicio de sesión se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image1.png)

Revise ahora la sección Concesión de código de [autorización](http://tools.ietf.org/html/rfc6749#section-4.1) oauth 2 del IETF.

**El proveedor** (en la tabla siguiente) es [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Proveedor, que es `OAuthAuthorizationServerProvider`de tipo , que contiene todos los eventos del servidor OAuth.

| Pasos de flujo de la sección Concesión de código de autorización | La descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) El cliente inicia el flujo dirigiendo el agente de usuario del propietario del recurso al punto de conexión de autorización. El cliente incluye su identificador de cliente, el ámbito solicitado, el estado local y un URI de redirección al que el servidor de autorización enviará el agente de usuario una vez que se conceda (o deniegue) el acceso. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) El servidor de autorización autentica al propietario del recurso (a través del agente de usuario) y establece si el propietario del recurso concede o deniega la solicitud de acceso del cliente. | **Si el usuario concede acceso&gt; &lt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) Suponiendo que el propietario del recurso concede acceso, el servidor de autorización redirige el agente de usuario al cliente mediante el URI de redirección proporcionado anteriormente (en la solicitud o durante el registro del cliente). ... |  |
|  |  |
| (D) El cliente solicita un token de acceso desde el punto de conexión de token del servidor de autorización mediante la inclusión del código de autorización recibido en el paso anterior. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. El cliente incluye el URI de redirección utilizado para obtener el código de autorización para la verificación. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

A continuación `AuthorizationCodeProvider.CreateAsync` se `ReceiveAsync` muestra una implementación de ejemplo para y para controlar la creación y validación de código de autorización.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

El código anterior utiliza un diccionario simultáneo en memoria para almacenar el código y el vale de identidad y restaurar la identidad después de recibir el código. En una aplicación real, sería reemplazado por un almacén de datos persistente. El punto de conexión de autorización es para que el propietario del recurso conceda acceso al cliente. Por lo general, necesita una interfaz de usuario para permitir al usuario hacer clic en un botón y confirmar la concesión. El middleware OWIN OAuth permite que el código de aplicación controle el punto de conexión de autorización. En nuestra aplicación de ejemplo, `OAuthController` usamos un controlador MVC llamado para controlarlo. Aquí está la implementación de ejemplo:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

La `Authorize` acción comprobará primero si el usuario ha iniciado sesión en el servidor de autorización. Si no es así, el middleware de autenticación desafía al autor de la llamada a autenticarse mediante la cookie "Aplicación" y redirige a la página de inicio de sesión. (Véase el código resaltado arriba.) Si el usuario ha iniciado sesión, representará la vista Autorizar, como se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image2.png)

Si se selecciona el `Authorize` botón **Conceder,** la acción creará una nueva identidad "Portador" e iniciará sesión con ella. Desencadenará el servidor de autorización para generar un token de portador y enviarlo de vuelta al cliente con carga JSON.

### <a name="implicit-grant"></a>Subvención implícita

Refiera a la sección de la [concesión implícita de](http://tools.ietf.org/html/rfc6749#section-4.2) OAuth 2 del IETF ahora.

 El flujo [de concesión implícita](http://tools.ietf.org/html/rfc6749#section-4.2) que se muestra en la figura 4 es el flujo y la asignación que sigue el middleware OWIN OAuth.

| Pasos de flujo de la sección Subvención implícita | La descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) El cliente inicia el flujo dirigiendo el agente de usuario del propietario del recurso al punto de conexión de autorización. El cliente incluye su identificador de cliente, el ámbito solicitado, el estado local y un URI de redirección al que el servidor de autorización enviará el agente de usuario una vez que se conceda (o deniegue) el acceso. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) El servidor de autorización autentica al propietario del recurso (a través del agente de usuario) y establece si el propietario del recurso concede o deniega la solicitud de acceso del cliente. | **Si el usuario concede acceso&gt; &lt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) Suponiendo que el propietario del recurso concede acceso, el servidor de autorización redirige el agente de usuario al cliente mediante el URI de redirección proporcionado anteriormente (en la solicitud o durante el registro del cliente). ... |  |
|  |  |
| (D) El cliente solicita un token de acceso desde el punto de conexión de token del servidor de autorización mediante la inclusión del código de autorización recibido en el paso anterior. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. El cliente incluye el URI de redirección utilizado para obtener el código de autorización para la verificación. |  |

Puesto que ya hemos`OAuthController.Authorize` implementado el punto final de autorización (acción) para la concesión de código de autorización, habilita automáticamente el flujo implícito también. Nota: `Provider.ValidateClientRedirectUri` se utiliza para validar el ID de cliente con su URL de redirección, que protege el flujo de concesión implícito de enviar el token de acceso a los clientes malintencionados (ataque del[hombre en el medio).](https://www.owasp.org/index.php/Man-in-the-middle_attack)

### <a name="resource-owner-password-credentials-grant"></a>Concesión de credenciales de contraseña de propietario de recursos

Refiera a la sección de la concesión de [credenciales](http://tools.ietf.org/html/rfc6749#section-4.3) del propietario del recurso OAuth 2 del IETF ahora.

 El flujo [de concesión](http://tools.ietf.org/html/rfc6749#section-4.3) de credenciales de contraseña del propietario de recursos que se muestra en la figura 5 es el flujo y la asignación que sigue el middleware OWIN OAuth.

| Pasos de flujo de la sección Concesión de credenciales de contraseña de propietario de recursos | La descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) El propietario del recurso proporciona al cliente su nombre de usuario y contraseña. |  |
|  |  |
| (B) El cliente solicita un token de acceso desde el punto de conexión de token del servidor de autorización mediante la inclusión de las credenciales recibidas del propietario del recurso. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) El servidor de autorización autentica al cliente y valida las credenciales del propietario del recurso y, si es válida, emite un token de acceso. |  |

Aquí está la `Provider.GrantResourceOwnerCredentials`implementación de ejemplo para:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> El código anterior está destinado a explicar esta sección del tutorial y no debe usarse en aplicaciones seguras o de producción. No comprueba las credenciales de los propietarios de recursos. Se supone que cada credencial es válida y crea una nueva identidad para ella. La nueva identidad se usará para generar el token de acceso y el token de actualización. Sustituya el código por su propio código de administración de cuentas seguro.


### <a name="client-credentials-grant"></a>Concesión de credenciales de cliente

Refiera a la sección de la concesión de [credenciales](http://tools.ietf.org/html/rfc6749#section-4.4) del cliente OAuth 2 del IETF ahora.

 El flujo de concesión de [credenciales](http://tools.ietf.org/html/rfc6749#section-4.4) de cliente que se muestra en la figura 6 es el flujo y la asignación que sigue el middleware OWIN OAuth.

| Pasos de flujo de la sección Concesión de credenciales de cliente | La descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) El cliente se autentica con el servidor de autorización y solicita un token de acceso desde el punto de conexión del token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) El servidor de autorización autentica al cliente y, si es válido, emite un token de acceso. |  |

Aquí está la `Provider.GrantClientCredentials`implementación de ejemplo para:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> El código anterior está destinado a explicar esta sección del tutorial y no debe usarse en aplicaciones seguras o de producción. Reemplace el código con su propio código de administración de cliente seguro.


### <a name="refresh-token"></a>Actualizar token

Refiera a la sección del [token](http://tools.ietf.org/html/rfc6749#section-1.5) de actualización de OAuth 2 del IETF ahora.

 El flujo [de token](http://tools.ietf.org/html/rfc6749#section-1.5) de actualización que se muestra en la figura 2 es el flujo y la asignación que sigue el middleware OWIN OAuth.

| Pasos de flujo de la sección Concesión de credenciales de cliente | La descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (G) El cliente solicita un nuevo token de acceso autenticando con el servidor de autorización y presentando el token de actualización. Los requisitos de autenticación de cliente se basan en el tipo de cliente y en las directivas del servidor de autorización. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) El servidor de autorización autentica al cliente y valida el token de actualización y, si es válido, emite un nuevo token de acceso (y, opcionalmente, un nuevo token de actualización). |  |

Aquí está la `Provider.GrantRefreshToken`implementación de ejemplo para:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Cree un servidor de recursos protegido por el token de acceso

Cree un proyecto de aplicación web vacío e instale los siguientes paquetes en el proyecto:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Cree una clase de inicio y configure la autenticación y la API web. Consulte *AuthorizationServer-ResourceServer-Startup.cs* en la descarga de ejemplo.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

En la descarga de ejemplo, consulte *\_AuthorizationServer-ResourceServer-App Start-Startup.Auth.cs.*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

En la descarga de ejemplo, consulte *\_AuthorizationServer-ResourceServer-App Start-Startup.WebApi.cs.*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`método permite CORS para todos los dominios.
- `UseOAuthBearerAuthentication`método habilita el middleware de autenticación de token de portador de OAuth que recibirá y validará el token de portador desde el encabezado de autorización en la solicitud.
- `Config.SuppressDefaultHostAuthenticaiton`suprime la entidad de seguridad autenticada de host predeterminada de la aplicación, por lo tanto, todas las solicitudes serán anónimas después de esta llamada.
- `HostAuthenticationFilter`habilita la autenticación solo para el tipo de autenticación especificado. En este caso, es el tipo de autenticación portador.

Para demostrar la identidad autenticada, creamos un ApiController para generar notificaciones de usuario actual.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Si el servidor de autorización y el servidor de recursos no están en el mismo equipo, el middleware de OAuth usará las diferentes claves de máquina para cifrar y descifrar el token de acceso del portador. Para compartir la misma clave privada entre ambos `machinekey` proyectos, agregamos la misma configuración en ambos archivos *web.config.*

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Crear clientes de OAuth 2.0

 Usamos el paquete NuGet [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) para simplificar el código de cliente.

### <a name="authorization-code-grant-client"></a>Cliente de Concesión de Código de Autorización

 Este cliente es una aplicación MVC. Desencadenará un flujo de concesión de código de autorización para obtener el token de acceso del back-end. Tiene una sola página como se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image3.png)

- El botón **Autorizar** redirigirá el explorador al servidor de autorización para notificar al propietario del recurso para conceder acceso a este cliente.
- El botón **Actualizar** obtendrá un nuevo token de acceso y un token de actualización mediante el token de actualización actual.
- El botón **Access Protected Resource API** llamará al servidor de recursos para obtener los datos de notificaciones del usuario actual y mostrarlos en la página.

Aquí está el código `HomeController` de ejemplo del cliente.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`requiere SSL de forma predeterminada. Puesto que nuestra demostración está utilizando HTTP, debe agregar la siguiente configuración en el archivo de configuración:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Seguridad: nunca deshabilite SSL en una aplicación de producción. Sus credenciales de inicio de sesión ahora se envían en texto no cifrado a través de la conexión. El código anterior es solo para la depuración y exploración de ejemplo local.


### <a name="implicit-grant-client"></a>Cliente de subvención implícita

Este cliente utiliza JavaScript para:

1. Abra una nueva ventana y reoriente al punto final autorizado del servidor de autorización.
2. Obtenga el token de acceso de los fragmentos de URL cuando redirija hacia atrás.

La siguiente imagen muestra este proceso:

![](owin-oauth-20-authorization-server/_static/image4.png)

El cliente debe tener dos páginas: una para la página de inicio y la otra para la devolución de llamada. Aquí está el código JavaScript de ejemplo que se encuentra en el archivo *Index.cshtml:*

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Aquí está el código de control de devolución de llamada en el archivo *SignIn.cshtml:*

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Una práctica recomendada es mover JavaScript a un archivo externo y no incrustarlo con el marcado Razor. Para mantener esta muestra simple, se han combinado.


### <a name="resource-owner-password-credentials-grant-client"></a>Credenciales de contraseña de propietario de recursos Conceder cliente

Usamos una aplicación de consola para demostrar este cliente. Este es el código:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Cliente de concesión de credenciales de cliente

Al igual que la concesión de credenciales de contraseña de propietario de recursos, aquí está el código de la aplicación de consola:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
