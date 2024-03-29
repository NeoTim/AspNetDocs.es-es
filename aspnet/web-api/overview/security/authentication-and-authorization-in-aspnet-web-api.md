---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticación y autorización en ASP.NET Web API Microsoft Docs
author: MikeWasson
description: Proporciona una visión general de la autenticación y la autorización en ASP.NET API web.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675866"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticación y autorización en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Ha creado una API web, pero ahora desea controlar el acceso a ella. En esta serie de artículos, veremos algunas opciones para proteger una API web de usuarios no autorizados. Esta serie cubrirá tanto la autenticación como la autorización.

- *La autenticación* es conocer la identidad del usuario. Por ejemplo, Alice inicia sesión con su nombre de usuario y contraseña, y el servidor utiliza la contraseña para autenticar a Alice.
- *La autorización* es decidir si un usuario puede realizar una acción. Por ejemplo, Alice tiene permiso para obtener un recurso, pero no para crear un recurso.

El primer artículo de la serie ofrece una visión general de la autenticación y autorización en ASP.NET Web API. Otros temas describen escenarios de autenticación comunes para Web API.

> [!NOTE]
> Gracias a las personas que revisaron esta serie y proporcionaron valiosos comentarios: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Authentication

La API web supone que la autenticación se produce en el host. Para el hospedaje web, el host es IIS, que utiliza módulos HTTP para la autenticación. Puede configurar el proyecto para que use cualquiera de los módulos de autenticación integrados en IIS o ASP.NET, o escribir su propio módulo HTTP para realizar la autenticación personalizada.

Cuando el host autentica al usuario, crea una *entidad de seguridad,* que es un objeto [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) que representa el contexto de seguridad en el que se ejecuta el código. El host asocia la entidad de seguridad al subproceso actual estableciendo **Thread.CurrentPrincipal**. La entidad de seguridad contiene un objeto **Identity** asociado que contiene información sobre el usuario. Si el usuario está autenticado, la propiedad **Identity.IsAuthenticated** devuelve **true**. Para las solicitudes anónimas, **IsAuthenticated** devuelve **false**. Para obtener más información acerca de las entidades de seguridad, vea [Seguridad basada en](https://msdn.microsoft.com/library/shz8h065.aspx)roles .

### <a name="http-message-handlers-for-authentication"></a>Controladores de mensajes HTTP para autenticación

En lugar de usar el host para la autenticación, puede colocar la lógica de autenticación en un controlador de [mensajes HTTP.](../advanced/http-message-handlers.md) En ese caso, el controlador de mensajes examina la solicitud HTTP y establece la entidad de seguridad.

¿Cuándo debe usar controladores de mensajes para la autenticación? Estas son algunas compensaciones:

- Un módulo HTTP ve todas las solicitudes que pasan por la canalización de ASP.NET. Un controlador de mensajes solo ve las solicitudes que se enrutan a la API web.
- Puede establecer controladores de mensajes por ruta, lo que le permite aplicar un esquema de autenticación a una ruta específica.
- Los módulos HTTP son específicos de IIS. Los controladores de mensajes son independientes del host, por lo que se pueden usar con el hospedaje web y el autohospedaje.
- Los módulos HTTP participan en el registro, la auditoría, etc. de IIS.
- Los módulos HTTP se ejecutaron anteriormente en la canalización. Si controla la autenticación en un controlador de mensajes, la entidad de seguridad no se establece hasta que se ejecuta el controlador. Además, la entidad de seguridad vuelve a la entidad de seguridad anterior cuando la respuesta sale del controlador de mensajes.

Por lo general, si no necesita admitir el autohospedaje, un módulo HTTP es una mejor opción. Si necesita admitir el autohospedaje, considere la posibilidad de un controlador de mensajes.

### <a name="setting-the-principal"></a>Configuración del director

Si la aplicación realiza cualquier lógica de autenticación personalizada, debe establecer la entidad de seguridad en dos lugares:

- **Thread.CurrentPrincipal**. Esta propiedad es la forma estándar de establecer la entidad de seguridad del subproceso en .NET.
- **HttpContext.Current.User**. Esta propiedad es específica de ASP.NET.

El código siguiente muestra cómo establecer la entidad de seguridad:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Para el alojamiento web, debe establecer la entidad de seguridad en ambos lugares; de lo contrario, el contexto de seguridad puede volverse incoherente. Para el autohospedaje, sin embargo, **HttpContext.Current** es null. Para asegurarse de que el código es independiente del host, por lo tanto, compruebe si es null antes de asignar lo que se muestra a **HttpContext.Current**, como se muestra.

## <a name="authorization"></a>Authorization

La autorización se produce más adelante en la canalización, más cerca del controlador. Esto le permite tomar decisiones más detalladas al conceder acceso a los recursos.

- *Los filtros de autorización* se ejecutan antes de la acción del controlador. Si la solicitud no está autorizada, el filtro devuelve una respuesta de error y no se invoca la acción.
- Dentro de una acción de controlador, puede obtener la entidad de seguridad actual de la **ApiController.User** propiedad. Por ejemplo, puede filtrar una lista de recursos en función del nombre de usuario, devolviendo solo los recursos que pertenecen a ese usuario.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Uso del atributo [Authorize]

Web API proporciona un filtro de autorización integrado, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Este filtro comprueba si el usuario está autenticado. Si no es así, devuelve el código de estado HTTP 401 (No autorizado), sin invocar la acción.

Puede aplicar el filtro globalmente, en el nivel de controlador o en el nivel de acciones individuales.

**Globalmente:** para restringir el acceso a cada controlador de API web, agregue el filtro **AuthorizeAttribute** a la lista de filtros globales:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controlador**: Para restringir el acceso a un controlador específico, agregue el filtro como atributo al controlador:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Acción**: Para restringir el acceso a acciones específicas, agregue el atributo al método de acción:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Como alternativa, puede restringir el controlador y, a continuación, `[AllowAnonymous]` permitir el acceso anónimo a acciones específicas, mediante el atributo. En el ejemplo `Post` siguiente, el método `Get` está restringido, pero el método permite el acceso anónimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

En los ejemplos anteriores, el filtro permite a cualquier usuario autenticado tener acceso a los métodos restringidos; sólo los usuarios anónimos se mantienen fuera. También puede limitar el acceso a usuarios específicos o a usuarios en roles específicos:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> El filtro **AuthorizeAttribute** para controladores de API web se encuentra en el espacio de nombres **System.Web.Http.** Hay un filtro similar para controladores MVC en el espacio de nombres **System.Web.Mvc,** que no es compatible con controladores de API web.

### <a name="custom-authorization-filters"></a>Filtros de autorización personalizados

Para escribir un filtro de autorización personalizado, derive de uno de estos tipos:

- **AuthorizeAttribute**. Amplíe esta clase para realizar la lógica de autorización en función del usuario actual y de los roles del usuario.
- **AuthorizationFilterAttribute**. Amplíe esta clase para realizar una lógica de autorización sincrónica que no se basa necesariamente en el usuario o rol actual.
- **IAuthorizationFilter**. Implemente esta interfaz para realizar la lógica de autorización asincrónica; por ejemplo, si la lógica de autorización realiza llamadas de red o E/S asincrónicas. (Si la lógica de autorización está enlazada a la CPU, es más sencillo derivar de **AuthorizationFilterAttribute**, ya que no es necesario escribir un método asincrónico.)

En el diagrama siguiente se muestra la jerarquía de clases para la **AuthorizeAttribute** clase.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorización dentro de una acción del controlador

En algunos casos, puede permitir que una solicitud continúe, pero cambiar el comportamiento en función de la entidad de seguridad. Por ejemplo, la información que devuelva puede cambiar en función del rol del usuario. Dentro de un método de controlador, puede obtener la entidad de seguridad actual de la **ApiController.User** propiedad.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
