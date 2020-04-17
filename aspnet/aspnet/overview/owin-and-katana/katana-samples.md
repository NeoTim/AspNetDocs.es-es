---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Muestras de Katana ? Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 15cc1084b16db2619f2295ee21dec4f49eb2e354
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540446"
---
# <a name="katana-samples"></a>Ejemplos de Katana

por [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Ejemplos de Katana

**ASP.NET código fuente** | [de](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes) muestra de rutas  
En algunas aplicaciones, querrá conectar componentes OWIN en la tabla de rutas Asp.Net lado a lado con componentes que no sean OWIN. En este ejemplo se muestra cómo utilizar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionados por Microsoft.Owin.Host.SystemWeb.

**Código** | [fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines) de muestra de tuberías de bifurcación  
Las canalizaciones de procesamiento de solicitudes OWIN no necesitan ser lineales, se pueden bifurcar para procesar solicitudes de diferentes maneras. En este ejemplo se muestra cómo construir una canalización de bifurcación basada en rutas de acceso de solicitud u otros datos de solicitud, como encabezados. Estos componentes están disponibles en el paquete nuget Microsoft.Owin.Mapping.

**Custom Server Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) de ejemplo de servidor personalizado   
Muestra cómo utilizar un servidor OWIN personalizado al hospedar oWIN de forma automática.

**Embedded Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) de muestra integrado  
Algunos servidores OWIN se pueden ejecutar&quot;dentro de&quot;su propio proceso (autohospedado). En este ejemplo se muestra cómo iniciar una aplicación OWIN mediante las herramientas proporcionadas por el paquete nuget Microsoft.Owin.Hosting.

**Código** | [fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) de muestra HelloWorld  
OWIN es una abstracción de API de servidor HTTP que permite la portabilidad de aplicaciones en varios servidores. En este ejemplo se muestra cómo escribir una aplicación Hello World mediante algunos **contenedores simples** alrededor de la abstracción OWIN sin procesar y ejecutarla en un servidor web como ASP.NET.

**Hello World Raw OWIN Código** | [fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) de muestra  
En este ejemplo se muestra cómo escribir una aplicación Hello World mediante la abstracción **OWIN sin procesar** y ejecutarla en un servidor web como Asp.Net.

**SignalR Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) de muestra de SignalR  
Muestra cómo autohospedar SignalR mediante OWIN / Katana. Para obtener más información sobre el autohospedaje de SignalR, vea [Tutorial: Autohospedaje de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Static Files Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) de ejemplo de archivos estáticos   
Muestra cómo admitir solicitudes HTTP para archivos estáticos mediante OWIN / Katana.

**Web API** | [Código fuente de](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) la API web   
En este ejemplo se muestra cómo hospedar OWIN en IIS y agregar api web a la canalización de OWIN.

**Web Socket Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) de muestra de Web Socket   
Muestra cómo admitir Web Sockets en OWIN mediante la clase [System.Net.WebSockets.WebSocket.](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)
