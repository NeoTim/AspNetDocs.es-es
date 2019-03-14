---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento de ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061352"
---
<a name="routing-in-aspnet-web-api"></a>Enrutamiento de ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET Web API enruta las solicitudes HTTP a los controladores.

> [!NOTE]
> Si está familiarizado con ASP.NET MVC, API Web de enrutamiento es muy similar para el enrutamiento de MVC. La principal diferencia es que API Web usa el verbo HTTP, no la ruta de acceso URI, para seleccionar la acción. También puede usar el enrutamiento de estilo MVC en Web API. En este artículo no supone ningún conocimiento de ASP.NET MVC.

## <a name="routing-tables"></a>Tablas de enrutamiento

En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP. Se llama a los métodos públicos del controlador de *métodos de acción* o simplemente *acciones*. Cuando el marco API Web recibe una solicitud, enruta la solicitud a una acción.

Para determinar qué acción va a invocar, el marco de trabajo usa un *tabla de enrutamiento*. La plantilla de proyecto de Visual Studio para Web API crea una ruta predeterminada:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Esta ruta se define en el *WebApiConfig.cs* archivo, que se coloca en el *aplicación\_iniciar* directorio:

![](routing-in-aspnet-web-api/_static/image1.png)

Para obtener más información sobre la `WebApiConfig` de clases, vea [configuración de ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Si la API Web de autohospedaje, debe establecer la tabla de enrutamiento directamente en el `HttpSelfHostConfiguration` objeto. Para obtener más información, consulte [autohospedaje una API Web](../older-versions/self-host-a-web-api.md).

Cada entrada en la tabla de enrutamiento contiene un *plantilla de ruta*. La plantilla de ruta predeterminada para la API Web es &quot;api / {controller} / {id}&quot;. En esta plantilla, &quot;api&quot; es un segmento de ruta de acceso literal y {controller} y {id} son variables de marcador de posición.

Cuando el marco API Web recibe una solicitud HTTP, intenta coincidir con el identificador URI con una de las plantillas de ruta en la tabla de enrutamiento. Si coincide con ninguna ruta, el cliente recibe un error 404. Por ejemplo, los siguientes URI coinciden con la ruta predeterminada:

- / api/contacts
- /api/contacts/1
- /api/products/gizmo1

Sin embargo, el siguiente URI no coincide, porque carece del &quot;api&quot; segmento:

- /contacts/1

> [!NOTE]
> La razón para usar "api" en la ruta es evitar colisiones con enrutamiento de ASP.NET MVC. De este modo, puede tener &quot;/pone en contacto con&quot; vaya a un controlador MVC y &quot;/api/contacts&quot; vaya a un controlador Web API. Por supuesto, si no le gusta esta convención, puede cambiar la tabla de rutas predeterminadas.

Una vez que se encuentra una ruta coincidente, Web API selecciona el controlador y la acción:

- Para buscar el controlador, Web API agrega &quot;controlador&quot; al valor de la *{controller}* variable.
- Para buscar la acción, API Web examina el verbo HTTP y, a continuación, busca una acción cuyo nombre comienza con ese nombre del verbo HTTP. Por ejemplo, con una solicitud GET, Web API busca una acción con el prefijo &quot;obtener&quot;, tales como &quot;GetContact&quot; o &quot;GetAllContacts&quot;. Esta convención se aplica solo a GET, POST, PUT, DELETE, HEAD, opciones y aplicar revisiones a verbos. Puede habilitar otros verbos HTTP mediante el uso de atributos en el controlador. Veremos un ejemplo de eso más adelante.
- Otras variables de marcador de posición en la plantilla de ruta, como *{id},* se asignan a parámetros de acción.

Veamos un ejemplo. Supongamos que define el controlador siguiente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Estas son algunas posibles solicitudes HTTP, junto con la acción que se invoca para cada uno:

| Verbo HTTP | Ruta de acceso URI | Acción | Parámetro |
| --- | --- | --- | --- |
| GET | API/products | GetAllProducts | *(ninguno)* |
| GET | productos/API/4 | GetProductById | 4 |
| SUPRIMIR | productos/API/4 | DeleteProduct | 4 |
| EXPONER | API/products | *(no coincidencia)* |  |

Tenga en cuenta que el *{id}* segmento del URI, si está presente, se asigna a la *id* parámetro de la acción. En este ejemplo, el controlador define dos métodos GET, uno con un *id* parámetro y otra sin parámetros.

Además, tenga en cuenta que la solicitud POST producirá un error, porque el controlador no define un &quot;Post... &quot; método.

## <a name="routing-variations"></a>Variaciones de enrutamientos

La sección anterior describe el mecanismo de enrutamiento básico para ASP.NET Web API. Esta sección describen algunas variaciones.

### <a name="http-verbs"></a>Verbos HTTP

En lugar de usar la convención de nomenclatura para los verbos HTTP, puede especificar explícitamente el verbo HTTP para una acción decorando el método de acción con uno de los siguientes atributos:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

En el ejemplo siguiente, la `FindProduct` método se asigna a las solicitudes GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Para permitir varios verbos HTTP para una acción, o para permitir que los verbos HTTP que no sean GET, PUT, POST, DELETE, HEAD, opciones y revisión, use el `[AcceptVerbs]` atributo, que toma una lista de verbos HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Enrutamiento por nombre de acción

Con la plantilla de enrutamiento de forma predeterminada, la API Web usa el verbo HTTP para seleccionar la acción. Sin embargo, también puede crear una ruta donde se incluye el nombre de acción en el URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

En esta plantilla de ruta, el *{action}* el método de acción en el controlador de los nombres de parámetros. Con este estilo de enrutamiento, usar atributos para especificar los verbos HTTP permitidos. Por ejemplo, suponga que el controlador tiene el siguiente método:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

En este caso, se asignaría una solicitud GET para "api/productos/detalles/1" para el `Details` método. Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.

Puede reemplazar el nombre de acción mediante el `[ActionName]` atributo. En el ejemplo siguiente, hay dos acciones que se asignan a &quot;productos/api/miniatura/*id*. Uno es compatible con GET y el otro admite POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>No acciones

Para evitar que un método invocando como una acción, use el `[NonAction]` atributo. Esto indica al marco que el método no es una acción, incluso si en caso contrario, coincidiría con las reglas de enrutamiento.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Información adicional

En este tema se proporciona una visión general del enrutamiento. Para obtener más información, consulte [enrutamiento y selección de acción](routing-and-action-selection.md), que describen exactamente cómo el marco de trabajo coincide con un URI a una ruta, selecciona un controlador y, a continuación, selecciona la acción que se invoca.
