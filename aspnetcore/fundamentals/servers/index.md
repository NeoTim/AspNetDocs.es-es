---
title: Implementaciones de servidores web en ASP.NET Core
author: guardrex
description: Detecte los servidores web Kestrel y HTTP.sys de ASP.NET Core. Obtenga más información sobre cómo elegir un servidor y cuándo se debe usar un servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/servers/index
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementaciones de servidores web en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)

Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso. La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) compuestos en un <xref:Microsoft.AspNetCore.Http.HttpContext>.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core se suministra con los siguientes componentes:

* El servidor [Kestrel](xref:fundamentals/servers/kestrel) es la implementación de servidor HTTP multiplataforma predeterminada.
* El servidor HTTP de IIS es un [servidor en proceso](#in-process-hosting-model) de IIS.
* El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo de Windows basado en el [controlador del kernel HTTP.sys y la API de servidor HTTP](/windows/desktop/Http/http-api-start-page).

Cuando se usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación se ejecuta:

* En el mismo proceso que el proceso de trabajo de IIS (el [modelo de hospedaje en proceso](#in-process-hosting-model)) con el [servidor HTTP de IIS](#iis-http-server). La configuración recomendada es *En proceso*.
* En un proceso distinto al del proceso de trabajo de IIS (el [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model)) con el [servidor Kestrel](#kestrel).

El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) es un módulo nativo de IIS que controla las solicitudes de IIS nativas entre IIS y el servidor de IIS en proceso o Kestrel. Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module>.

## <a name="hosting-models"></a>Modelos de hospedaje

### <a name="in-process-hosting-model"></a>Modelo de hospedaje en proceso

Con el hospedaje en proceso, una aplicación ASP.NET Core se ejecuta en el mismo proceso que su proceso de trabajo de IIS. El hospedaje en proceso proporciona un rendimiento mejorado con respecto al hospedaje fuera de proceso porque las solicitudes no se realizan mediante proxy en el adaptador de bucle invertido, una interfaz de red que devuelve el tráfico saliente a la misma máquina. IIS controla la administración de procesos con el [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

El módulo ASP.NET Core:

* Realiza la inicialización de aplicaciones.
  * Carga [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Llama a `Program.Main`.
* Controla la duración de la solicitud nativa de IIS.

No se admite el modelo de hospedaje en proceso para aplicaciones de ASP.NET Core que tienen como destino .NET Framework.

En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada en proceso:

![Módulo ASP.NET Core](_static/ancm-inprocess.png)

Una solicitud llega de Internet al controlador HTTP.sys en modo kernel. El controlador enruta la solicitud nativa a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo recibe la solicitud nativa y la pasa a IIS HTTP Server (`IISHttpServer`). El servidor HTTP de IIS es una implementación de servidor en proceso para IIS que convierte una solicitud nativa en administrada.

Una vez que IIS HTTP Server procesa la solicitud, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. La respuesta de la aplicación se pasa a IIS a través del servidor HTTP de IIS. IIS envía la respuesta al cliente que inició la solicitud.

El hospedaje en proceso es opcional para las aplicaciones existentes, pero, para las plantillas [dotnet new](/dotnet/core/tools/dotnet-new), este modelo es el predeterminado para todos los escenarios de IIS e IIS Express.

### <a name="out-of-process-hosting-model"></a>Modelo de hospedaje fuera de proceso

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos. El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea. Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:

![Módulo ASP.NET Core](_static/ancm-outofprocess.png)

Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel. El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.

El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{PORT}`. Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo. El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.

Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel. La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.

Para obtener instrucciones de configuración para IIS y el módulo ASP.NET Core, consulte los temas siguientes:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core se suministra con los siguientes componentes:

* El servidor [Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado.
* El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo de Windows basado en el [controlador del kernel HTTP.sys y la API de servidor HTTP](/windows/desktop/Http/http-api-start-page).

Al usar [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación se ejecuta en un proceso distinto al del proceso de trabajo de IIS (*fuera de proceso*) con el [servidor Kestrel](#kestrel).

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos. El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea. Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:

![Módulo ASP.NET Core](_static/ancm-outofprocess.png)

Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel. El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.

El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`. Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo. El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.

Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel. La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.

Para obtener instrucciones de configuración para IIS y el módulo ASP.NET Core, consulte los temas siguientes:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.

---

::: moniker-end

## <a name="kestrel"></a>Kestrel

Kestrel es el servidor web predeterminado que se incluye en las plantillas de proyecto de ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

Kestrel se puede usar:

* Por sí solo como un servidor perimetral que procesa solicitudes directamente desde una red, incluida Internet.

  ![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

* Con un *servidor proxy inverso*, tal como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/). Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel.

  ![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Se admite cualquiera de las configuraciones de hospedaje, &mdash; con o sin un servidor proxy inverso&mdash; para ASP.NET Core 2.1 o aplicaciones posteriores.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si la aplicación únicamente acepta solicitudes de una red interna, Kestrel puede usarse por sí solo.

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

Si la aplicación se expone a Internet, Kestrel debe usar un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache ](https://httpd.apache.org/). Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel.

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales de acceso público expuestos directamente a Internet es la seguridad. Las versiones 1.x de Kestrel no incluyen características de seguridad importantes para defenderse contra ataques procedentes de Internet. Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño de solicitud y los límites de conexiones simultáneas correspondientes.

::: moniker-end

Si quiere obtener instrucciones e información sobre la configuración de Kestrel y su uso en una configuración de proxy inverso, vea <xref:fundamentals/servers/kestrel>.

### <a name="nginx-with-kestrel"></a>Nginx con Kestrel

Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, consulte <xref:host-and-deploy/linux-nginx>.

### <a name="apache-with-kestrel"></a>Apache con Kestrel

Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, consulte <xref:host-and-deploy/linux-apache>.

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a>Servidor HTTP de IIS

El servidor HTTP de IIS es un [servidor en proceso](#in-process-hosting-model) para IIS necesario para las implementaciones en proceso. El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) controla las solicitudes de IIS nativas entre IIS y el servidor HTTP de IIS. Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

Si las aplicaciones ASP.NET Core se ejecutan en Windows, HTTP.sys es una alternativa a Kestrel. Suele recomendarse Kestrel para un rendimiento óptimo. HTTP.sys se puede usar en escenarios en los que la aplicación se expone a Internet y las funcionalidades necesarias son compatibles con HTTP.sys pero no con Kestrel. Para obtener más información, consulta <xref:fundamentals/servers/httpsys>.

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys también se puede usar para las aplicaciones que solo se exponen a una red interna.

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

Para obtener instrucciones de configuración de HTTP.sys, vea <xref:fundamentals/servers/httpsys>.

## <a name="aspnet-core-server-infrastructure"></a>Infraestructura de servidores de ASP.NET Core

El elemento <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> disponible en el método `Startup.Configure` expone la propiedad <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> del tipo <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>. Kestrel y HTTP.sys solo exponen una característica cada uno (<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>), pero otras implementaciones de servidor pueden exponer funcionalidades adicionales.

Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.

## <a name="custom-servers"></a>Servidores personalizados

Si los servidores integrados no cumplen los requisitos de la aplicación, se puede crear una implementación de servidor personalizado. En la [guía de Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) se muestra cómo escribir una implementación de <xref:Microsoft.AspNetCore.Hosting.Server.IServer> basada en [Nowin](https://github.com/Bobris/Nowin). Solo las interfaces de la característica que usa la aplicación requieren implementación, aunque como mínimo se debe admitir <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> y <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature>.

## <a name="server-startup"></a>Inicio del servidor

El servidor se inicia cuando el entorno de desarrollo integrado (IDE) o editor inicia la aplicación:

* [Visual Studio](https://www.visualstudio.com/vs/): los perfiles de inicio se pueden usar para iniciar la aplicación y el servidor con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module) o la consola.
* [Visual Studio Code](https://code.visualstudio.com/): la aplicación y el servidor se inician mediante [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), con lo que se activa el depurador CoreCLR.
* [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/): la aplicación y el servidor se inician mediante [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Al iniciar la aplicación desde un símbolo del sistema en la carpeta del proyecto, [dotnet run](/dotnet/core/tools/dotnet-run) inicia la aplicación y el servidor (solo Kestrel y HTTP.sys). La configuración se especifica mediante la opción `-c|--configuration`, que está establecida en `Debug` (valor predeterminado) o `Release`. Si no hay perfiles de inicio en un archivo *launchSettings.json*, use la opción `--launch-profile <NAME>` para establecer el perfil de inicio (por ejemplo, `Development` o `Production`). Para más información, vea [dotnet run](/dotnet/core/tools/dotnet-run) y [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="http2-support"></a>Compatibilidad con HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación siguientes:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * Sistema operativo
    * Windows Server 2016/Windows 10 o posterior&dagger;
    * Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)
    * HTTP/2 se admitirá en una versión futura en macOS.
  * Plataforma de destino: .NET Core 2.2 o posterior
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 o posterior
  * Marco de destino: No aplicable a implementaciones de HTTP.sys.
* [IIS (en proceso)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Plataforma de destino: .NET Core 2.2 o posterior
* [IIS (fuera de proceso)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.
  * Marco de destino: No aplicable a implementaciones fuera de proceso de IIS.

&dagger;Kestrel tiene compatibilidad limitada para HTTP/2 en Windows Server 2012 R2 y Windows 8.1. La compatibilidad es limitada porque la lista de conjuntos de cifrado TLS admitidos y disponibles en estos sistemas operativos está limitada. Se puede requerir un certificado generado mediante Elliptic Curve Digital Signature Algorithm (ECDSA) para proteger las conexiones TLS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 o posterior
  * Marco de destino: No aplicable a implementaciones de HTTP.sys.
* [IIS (fuera de proceso)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.
  * Marco de destino: No aplicable a implementaciones fuera de proceso de IIS.

::: moniker-end

Una conexión HTTP/2 debe usar [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) y TLS 1.2 o posterior. Para más información, vea los temas que pertenecen a los escenarios de implementación de servidor.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
