---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento en ASP.NET Web API ( Web API) Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675758"
---
# <a name="routing-in-aspnet-web-api"></a>Enrutar en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET API web enruta las solicitudes HTTP a los controladores.

> [!NOTE]
> Si está familiarizado con ASP.NET MVC, el enrutamiento de API web es muy similar al enrutamiento MVC. La principal diferencia es que Web API utiliza el verbo HTTP, no la ruta de acceso URI, para seleccionar la acción. También puede usar el enrutamiento de estilo MVC en Web API. Este artículo no asume ningún conocimiento de ASP.NET MVC.

## <a name="routing-tables"></a>Tablas de enrutamiento

En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP. Los métodos públicos del controlador se denominan *métodos* de acción o simplemente *acciones.* Cuando el marco de la API web recibe una solicitud, enruta la solicitud a una acción.

Para determinar qué acción invocar, el marco de trabajo utiliza una tabla de *enrutamiento.* La plantilla de proyecto de Visual Studio para Web API crea una ruta predeterminada:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Esta ruta se define en el archivo *WebApiConfig.cs,* que se coloca en el directorio *\_App Start:*

![](routing-in-aspnet-web-api/_static/image1.png)

Para obtener más `WebApiConfig` información acerca de la clase, consulte [Configuración de ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Si autohospeda la API web, debe establecer la `HttpSelfHostConfiguration` tabla de enrutamiento directamente en el objeto. Para obtener más información, consulte [Auto-Host a Web API](../older-versions/self-host-a-web-api.md).

Cada entrada de la tabla de ruteo contiene una plantilla de *ruta.* La plantilla de ruta &quot;predeterminada para la API web&quot;es api/-controller-/-id- . En esta &quot;plantilla, api&quot; es un segmento de ruta de acceso literal, y los parámetros de marcador de posición son variables de marcador de posición.

Cuando el marco de la API web recibe una solicitud HTTP, intenta hacer coincidir el URI con una de las plantillas de ruta de la tabla de enrutamiento. Si ninguna ruta coincide, el cliente recibe un error 404. Por ejemplo, los siguientes URI coinciden con la ruta predeterminada:

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

Sin embargo, el URI siguiente no &quot;coincide, porque carece del segmento api:&quot;

- /contactos/1

> [!NOTE]
> La razón para usar "api" en la ruta es evitar colisiones con ASP.NET el enrutamiento MVC. De este modo, &quot;puede&quot; hacer que /contacts &quot;vaya a&quot; un controlador MVC y /api/contacts vaya a un controlador de API web. Por supuesto, si no le gusta esta convención, puede cambiar la tabla de rutas predeterminada.

Una vez que se encuentra una ruta coincidente, Web API selecciona el controlador y la acción:

- Para encontrar el controlador, &quot;la&quot; API web agrega Controller al valor de la variable *.*
- Para buscar la acción, Web API examina el verbo HTTP y, a continuación, busca una acción cuyo nombre comience con ese nombre de verbo HTTP. Por ejemplo, con una solicitud GET, Web API &quot;&quot;busca una &quot;acción&quot; &quot;con el&quot;prefijo Get , como GetContact o GetAllContacts . Esta convención solo se aplica a los verbos GET, POST, PUT, DELETE, HEAD, OPTIONS y PATCH. Puede habilitar otros verbos HTTP mediante atributos en el controlador. Veremos un ejemplo de eso más tarde.
- Otras variables de marcador de posición en la plantilla de ruta, como, por *ejemplo, el identificador,* se asignan a los parámetros de acción.

Veamos un ejemplo. Supongamos que define el controlador siguiente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Aquí están algunas posibles peticiones HTTP, junto con la acción que consigue invocada para cada uno:

| Verbo HTTP | Ruta de URI | Acción | Parámetro |
| --- | --- | --- | --- |
| GET | api/productos | GetAllProducts | *(ninguno)* |
| GET | api/productos/4 | GetProductById | 4 |
| Delete | api/productos/4 | DeleteProduct | 4 |
| POST | api/productos | *(sin coincidencia)* |  |

Tenga en cuenta que *el* segmento de la URI, si está presente, se asigna al parámetro *id* de la acción. En este ejemplo, el controlador define dos métodos GET, uno con un parámetro *id* y otro sin parámetros.

Además, tenga en cuenta que la solicitud &quot;POST fallará, porque el controlador no define un Post... &quot; método.

## <a name="routing-variations"></a>Variaciones de enrutamiento

En la sección anterior se describe el mecanismo de enrutamiento básico para ASP.NET Web API. En esta sección se describen algunas variaciones.

### <a name="http-verbs"></a>Verbos HTTP

En lugar de utilizar la convención de nomenclatura para los verbos HTTP, puede especificar explícitamente el verbo HTTP para una acción decorando el método de acción con uno de los siguientes atributos:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

En el ejemplo `FindProduct` siguiente, el método se asigna a las solicitudes GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Para permitir varios verbos HTTP para una acción o para permitir verbos HTTP distintos de GET, `[AcceptVerbs]` PUT, POST, DELETE, HEAD, OPTIONS y PATCH, utilice el atributo, que toma una lista de verbos HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Enrutamiento por nombre de acción

Con la plantilla de enrutamiento predeterminada, Web API utiliza el verbo HTTP para seleccionar la acción. Sin embargo, también puede crear una ruta donde el nombre de la acción se incluya en el URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

En esta plantilla de ruta, el parámetro *.action ,* nombra el método de acción en el controlador. Con este estilo de enrutamiento, utilice atributos para especificar los verbos HTTP permitidos. Por ejemplo, supongamos que el controlador tiene el siguiente método:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

En este caso, una solicitud GET para "api/products/details/1" se asignaría al `Details` método. Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.

Puede invalidar el nombre de `[ActionName]` la acción mediante el atributo. En el ejemplo siguiente, hay dos &quot;acciones que se asignan a api/products/thumbnail/*id*. Uno es compatible con GET y el otro es post:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>No acciones

Para evitar que un método se invoque como una acción, utilice el `[NonAction]` atributo. Esto indica al marco de trabajo que el método no es una acción, incluso si de lo contrario coincidiría con las reglas de enrutamiento.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Lecturas adicionales

En este tema se proporciona una vista de alto nivel del enrutamiento. Para obtener más detalles, vea Selección de [enrutamiento y acción](routing-and-action-selection.md), que describe exactamente cómo el marco de trabajo hace coincidir un URI con una ruta, selecciona un controlador y, a continuación, selecciona la acción que se va a invocar.
