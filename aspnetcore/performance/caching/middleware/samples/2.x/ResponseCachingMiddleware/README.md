---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062722"
---
# <a name="aspnet-core-response-caching-sample"></a>Ejemplo de almacenamiento en caché de respuesta de ASP.NET Core

Este ejemplo ilustra el uso de ASP.NET Core [Middleware de almacenamiento en caché de respuestas](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

La aplicación responde con su página de índice, incluido un `Cache-Control` encabezado para configurar el comportamiento de almacenamiento en caché. La aplicación también establece la `Vary` encabezado para configurar la memoria caché para dar servicio a la respuesta solo si el `Accept-Encoding` encabezado de las solicitudes posteriores coincide con el de la solicitud original.

Al ejecutar el ejemplo, la página de índice se sirve desde la memoria caché cuando se almacenan y se almacena en caché durante 10 segundos.
