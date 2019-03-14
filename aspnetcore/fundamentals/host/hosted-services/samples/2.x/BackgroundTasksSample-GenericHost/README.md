---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056492"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a>Ejemplo de tareas en segundo plano de ASP.NET Core (host genérico)

En este ejemplo se muestra el uso de [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). En este ejemplo se muestran las características descritas en el tema [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) (Tareas en segundo plano con servicios hospedados en ASP.NET Core).

Al ejecutar el ejemplo en [Visual Studio Code](https://code.visualstudio.com/), establezca el valor de **console** de la configuración de la consola en *.vscode/launch.json* como `externalTerminal` o `integratedTerminal`. El uso de la `internalConsole` no es compatible con la entrada de pulsación de tecla de la consola que utiliza la aplicación para poner en cola elementos de trabajo en segundo plano.
