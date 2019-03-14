---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Densidad de la conexión de SignalR las pruebas con Crank | Microsoft Docs
author: bradygaster
description: Pruebas de densidad de la conexión de SignalR con Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 40c9764f0c47b83df8300553b4b290429937345c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57058032"
---
<a name="signalr-connection-density-testing-with-crank"></a>Pruebas de densidad de la conexión de SignalR con Crank
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe cómo usar la herramienta manivela para probar una aplicación con varios clientes simulados.


Una vez que la aplicación se ejecuta en su entorno de hospedaje (Azure un rol, IIS, web o autohospedan mediante Owin), puede probar la respuesta de la aplicación a un alto nivel de densidad de la conexión con la herramienta manivela. El entorno de hospedaje puede ser un servidor de Internet Information Services (IIS), un host Owin o un rol web de Azure. (Nota: Contadores de rendimiento no están disponibles en Azure App Service Web Apps, por lo que no podrá obtener datos de rendimiento de una prueba de densidad de la conexión.)

Densidad de la conexión se refiere al número de conexiones TCP simultáneas que pueden establecerse en un servidor. Cada conexión TCP implica su propia sobrecarga y abre un gran número de conexiones inactivas para crear un cuello de botella de memoria.

[SignalR codebase](https://github.com/signalr/signalr) incluye una herramienta de prueba de carga denominada **manivela**. La versión más reciente de manivela puede encontrarse en [la bifurcación Dev](https://github.com/SignalR/signalr/tree/dev) en GitHub. Puede descargar un archivo Zip archive de la rama de desarrollo de SignalR codebase [aquí](https://github.com/SignalR/SignalR/archive/dev.zip).

Manivela puede utilizarse para saturar totalmente la memoria del servidor con el fin de calcular el número total de conexiones inactivas posibles en el hardware del servidor. Como alternativa, también puede usar manivela para prueba de carga del servidor en una determinada cantidad de presión de memoria, por refuerza las conexiones hasta que se alcanza un umbral de memoria específica o de un número concreto.

Al realizar pruebas, es importante usar clientes remotos para evitar cualquier competición por los recursos (es decir, las conexiones TCP y memoria). Supervisar los clientes para asegurarse de que no supera los cuellos de botella que pueden impedir que el servidor de alcanzar su capacidad máxima (memoria o CPU). Es posible que deba aumentar el número de clientes con el fin de cargar totalmente el servidor.

### <a name="running-a-connection-density-test"></a>Ejecutar una prueba de densidad de la conexión

Esta sección describen los pasos necesarios para ejecutar una prueba de densidad de la conexión en una aplicación de SignalR.

1. Descargue y compile el [codebase de la rama de desarrollo de SignalR](https://github.com/SignalR/SignalR/archive/dev.zip). En un símbolo del sistema, vaya a &lt;directorio del proyecto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Implementar la aplicación en su entorno de hospedaje deseado. Tome nota del punto de conexión que usa su aplicación; Por ejemplo, en la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), es el punto de conexión `http://<yourhost>:8080/signalr`.
3. Instalar [los contadores de rendimiento de SignalR](signalr-performance.md#perfcounters) en el servidor. Si la aplicación se ejecuta en Azure, consulte [utilizando los contadores de rendimiento de SignalR en un rol Web de Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Una vez haya descargado y compilado el código base e instala los contadores de rendimiento en el host, la herramienta de línea de comandos de manivela puede encontrarse en el `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` carpeta.

Las opciones disponibles para la herramienta manivela incluyen:

- **/?**: Muestra la pantalla de ayuda. También se muestran las opciones disponibles si el **Url** se omite el parámetro.
- **/Url**: La dirección URL para las conexiones SignalR. Este parámetro es necesario. Para una aplicación de SignalR mediante la asignación predeterminada, se finalizará la ruta de acceso en "/ signalr".
- **/Transport**: El nombre del transporte utilizado. El valor predeterminado es `auto`, lo que seleccionará el mejor protocolo disponible. Las opciones incluyen `WebSockets`, `ServerSentEvents`, y `LongPolling` (`ForeverFrame` no es una opción para manivela, desde el cliente de .NET en lugar de Internet Explorer se usa). Para obtener más información sobre cómo SignalR selecciona los transportes, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: El número de clientes que se agregan en cada lote. El valor predeterminado es 50.
- **/ConnectInterval**: El intervalo en milisegundos entre la adición de conexiones. El valor predeterminado es 500.
- **/ Conexiones**: El número de conexiones que se usa para la aplicación de prueba de carga. El valor predeterminado es 100.000.
- **/ConnectTimeout**: El tiempo de espera en segundos antes de anular la prueba. El valor predeterminado es 300.
- **MinServerMBytes**: El número de megabytes de servidor mínima para llegar a. El valor predeterminado es 500.
- **SendBytes**: El tamaño de la carga enviada al servidor en bytes. El valor predeterminado es 0.
- **SendInterval**: El retraso en milisegundos entre los mensajes al servidor. El valor predeterminado es 500.
- **SendTimeout**: El tiempo de espera en milisegundos para los mensajes en el servidor. El valor predeterminado es 300.
- **ControllerUrl**: La dirección Url donde va a hospedar un concentrador de controladores de un cliente. El valor predeterminado es null (ningún centro de controlador). El concentrador se inicia cuando se inicia la sesión de manivela; ninguna otra contacto entre el concentrador y manivela se realiza.
- **NumClients**: El número de clientes simulados para conectarse a la aplicación. El valor predeterminado es uno.
- **Archivo de registro**: El nombre de archivo para el archivo de registro para la ejecución de pruebas. De manera predeterminada, es `crank.csv`.
- **SampleInterval**: El tiempo en milisegundos entre muestras del contador de rendimiento. El valor predeterminado es 1000.
- **SignalRInstance**: El nombre de instancia para los contadores de rendimiento en el servidor. El valor predeterminado es usar el estado de conexión de cliente.

### <a name="example"></a>Ejemplo

El siguiente comando probará un sitio denominado `pfsignalr` en Azure que hospeda una aplicación en el puerto 8080 con un concentrador denominado "ControllerHub", usando 100 conexiones.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
