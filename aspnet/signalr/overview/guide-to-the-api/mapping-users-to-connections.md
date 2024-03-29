---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Asignación de usuarios de SignalR a conexiones ? Microsoft Docs
author: bradygaster
description: En este tema se muestra cómo conservar información sobre los usuarios y sus conexiones. Patrick Fletcher ayudó a escribir este tema. Versiones de software utilizadas en este tema...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675782"
---
# <a name="mapping-signalr-users-to-connections"></a>Asignar usuarios de SignalR a las conexiones

 por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se muestra cómo conservar información sobre los usuarios y sus conexiones.
>
> Patrick Fletcher ayudó a escribir este tema.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).

## <a name="introduction"></a>Introducción

Cada cliente que se conecta a un concentrador pasa un identificador de conexión único. Puede recuperar este valor `Context.ConnectionId` en la propiedad del contexto del concentrador. Si la aplicación necesita asignar un usuario al identificador de conexión y conservar esa asignación, puede usar una de las siguientes opciones:

- [El proveedor de ID de usuario (SignalR 2)](#IUserIdProvider)
- [Almacenamiento en memoria](#inmemory), como un diccionario
- [Grupo SignalR para cada usuario](#groups)
- [Almacenamiento permanente y externo,](#database)como una tabla de base de datos o almacenamiento de tablas de Azure

Cada una de estas implementaciones se muestra en este tema. Utilice los `OnConnected` `OnDisconnected`métodos `OnReconnected` , `Hub` , y de la clase para realizar un seguimiento del estado de la conexión de usuario.

El mejor enfoque para su aplicación depende de:

- El número de servidores web que hospedan la aplicación.
- Si necesita obtener una lista de los usuarios conectados actualmente.
- Si necesita conservar la información de grupo y usuario cuando se reinicia la aplicación o el servidor.
- Si la latencia de llamar a un servidor externo es un problema.

En la tabla siguiente se muestra qué enfoque funciona para estas consideraciones.

|  | Más de un servidor | Obtener una lista de los usuarios conectados actualmente | Persistir información después de reiniciar | Rendimiento óptimo |
| --- | --- | --- | --- | --- |
| Proveedor UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| En memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupos de un solo usuario | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, externo | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Proveedor de IUserID

Esta característica permite a los usuarios especificar lo que el userId se basa en un IRequest a través de una nueva interfaz IUserIdProvider.

**El IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

De forma predeterminada, habrá una implementación `IPrincipal.Identity.Name` que usa el usuario como nombre de usuario. Para cambiar esto, registre `IUserIdProvider` la implementación de con el host global cuando se inicie la aplicación:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Desde un concentrador, podrá enviar mensajes a estos usuarios a través de la siguiente API:

**Enviar un mensaje a un usuario específico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Almacenamiento en memoria

En los ejemplos siguientes se muestra cómo conservar la conexión y la información del usuario en un diccionario que se almacena en memoria. El diccionario `HashSet` utiliza a para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación SignalR. Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.

Si la aplicación se cierra, se pierde toda la información, pero se volverá a rellenar a medida que los usuarios restablezcan sus conexiones. El almacenamiento en memoria no funciona si el entorno incluye más de un servidor web porque cada servidor tendría una colección independiente de conexiones.

El primer ejemplo muestra una clase que administra la asignación de usuarios a conexiones. La clave para el HashSet será el nombre del usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

En el ejemplo siguiente se muestra cómo utilizar la clase de asignación de conexión desde un concentrador. La instancia de la clase se `_connections`almacena en un nombre de variable .

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupos de un solo usuario

Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando desee llegar solo a ese usuario. El nombre de cada grupo es el nombre del usuario. Si un usuario tiene más de una conexión, cada identificador de conexión se agrega al grupo del usuario.

No debe quitar manualmente al usuario del grupo cuando el usuario se desconecta. Esta acción se realiza automáticamente mediante el marco de SignalR.

En el ejemplo siguiente se muestra cómo implementar grupos de un solo usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Almacenamiento permanente y externo

En este tema se muestra cómo usar una base de datos o Azure Table Storage para almacenar información de conexión. Este enfoque funciona cuando tiene varios servidores web porque cada servidor web puede interactuar con el mismo repositorio de datos. Si los servidores web dejan de funcionar `OnDisconnected` o la aplicación se reinicia, no se llama al método. Por lo tanto, es posible que el repositorio de datos tenga registros para los identificadores de conexión que ya no son válidos. Para limpiar estos registros huérfanos, es posible que desee invalidar cualquier conexión que se haya creado fuera de un período de tiempo que sea relevante para su aplicación. Los ejemplos de esta sección incluyen un valor para el seguimiento cuando se creó la conexión, pero no muestran cómo limpiar registros antiguos porque es posible que desee hacerlo como proceso en segundo plano.

### <a name="database"></a>Base de datos

En los ejemplos siguientes se muestra cómo conservar la conexión y la información de usuario en una base de datos. Puede utilizar cualquier tecnología de acceso a datos; sin embargo, en el ejemplo siguiente se muestra cómo definir modelos mediante Entity Framework. Estos modelos de entidad corresponden a tablas y campos de base de datos. La estructura de datos puede variar considerablemente en función de los requisitos de la aplicación.

En el primer ejemplo se muestra cómo definir una entidad de usuario que se puede asociar a muchas entidades de conexión.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

A continuación, desde el concentrador, puede realizar un seguimiento del estado de cada conexión con el código que se muestra a continuación.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure

El siguiente ejemplo de almacenamiento de tablas de Azure es similar al ejemplo de base de datos. No incluye toda la información que necesitaría para empezar a usar Azure Table Storage Service. Para obtener información, consulte [Cómo usar Table Storage desde .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

En el ejemplo siguiente se muestra una entidad de tabla para almacenar información de conexión. Particiona los datos por nombre de usuario e identifica cada entidad por el identificador de conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

En el concentrador, puede realizar un seguimiento del estado de la conexión de cada usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
