---
title: Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo solucionar errores comunes al hospedar aplicaciones ASP.NET Core en Azure App Service e IIS.
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: d1cdac4d27ee1bc3ebb4329c1bbd3bdacb34a58c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050272"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En este tema, se ofrece información sobre cómo solucionar errores comunes al hospedar aplicaciones ASP.NET Core en Azure App Service e IIS.

Recopile la siguiente información:

* Comportamiento del explorador (código de estado y mensaje de error)
* Entradas de registro de eventos de la aplicación
  * Azure App Service : vea <xref:host-and-deploy/azure-apps/troubleshoot>.
  * IIS
    1. Seleccione **Inicio** en el menú **Windows**, escriba *Visor de eventos* y presione **Entrar**.
    1. Una vez abierto el **Visor de eventos**, amplíe **Registros de Windows** > **Aplicación** en la barra lateral.
* Entradas de registro de stdout y depuración de módulo ASP.NET Core
  * Azure App Service : vea <xref:host-and-deploy/azure-apps/troubleshoot>.
  * IIS: siga las instrucciones de las secciones [Creación y redireccionamiento de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) y [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) del tema Módulo ASP.NET Core.

Compare la información sobre errores con los siguientes errores comunes. Si se encuentra una coincidencia, siga los consejos de solución de problemas.

La lista de errores en este tema no es exhaustiva. Si se produce algún error que no aparezca aquí, abra un problema nuevo mediante el botón **Comentarios sobre el contenido** situado en la parte inferior de este tema con instrucciones detalladas sobre cómo reproducir el error.

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>El instalador no puede obtener VC++ Redistributable

* **Excepción del instalador:** 0x80072efd **--O--** 0x80072f76: error no especificado

* **Excepción del registro del instalador&#8224;:** error 0x80072efd **--O--** 0x80072f76: No se ha podido ejecutar el paquete EXE

  &#8224;El registro se encuentra en *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.

Solución del problema:

Si el sistema no tiene acceso a Internet al [instalar la agrupación de hospedaje .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*. Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Si se produce un error en el instalador, es posible que el servidor no reciba el entorno de tiempo de ejecución de .NET Core necesario para hospedar una [implementación dependiente del marco (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd). Si va a hospedar una FDD, confirme que el tiempo de ejecución está instalado en **Programas y características** o en **Aplicaciones y características**. Si se requiere un tiempo de ejecución específico, descargue el tiempo de ejecución de [Archivos de descarga de .NET](https://dotnet.microsoft.com/download/archives) e instálelo en el sistema. Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits

**Registro de aplicación:** el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** no se ha podido cargar. Los datos son el error.

Solución del problema:

Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo. Este problema se produce si ha instalado el módulo ASP.NET Core antes de una actualización del sistema operativo y, a continuación, ejecuta cualquier grupo de aplicaciones en modo de 32 bits después de una actualización del sistema operativo. Después de actualizar el sistema operativo, repare el módulo ASP.NET Core. Consulte [Instalación de la agrupación de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Seleccione **Reparar** cuando se ejecute el instalador.

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a>Se ha implementado una aplicación x86, pero el grupo de aplicaciones no está habilitado para aplicaciones de 32 bits

* **Explorador:** Error HTTP 500.30: error de inicio en el proceso ANCM

* **Registro de aplicación:** En la aplicación '/LM/W3SVC/5/ROOT' con la raíz física '{PATH}' se ha producido una excepción administrada inesperada, código de excepción = '0xe0434352'. Compruebe los registros stderr para obtener más información. La aplicación '/LM/W3SVC/5/ROOT' con la raíz física '{PATH}' no ha podido cargar el CLR ni la aplicación administrada. El subproceso de trabajo CLR se ha cerrado prematuramente

* **Registro de stdout del módulo ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.

::: moniker range=">= aspnetcore-2.2"

* **Registro de depuración del módulo de ASP.NET Core:** Se ha devuelto un valor HRESULT con errores: 0x8007023e

::: moniker-end

El SDK intercepta este escenario al publicar una aplicación independiente. El SDK genera un error si el RID no coincide con el destino de plataforma (por ejemplo, RID de `win10-x64` con `<PlatformTarget>x86</PlatformTarget>` en el archivo del proyecto).

Solución del problema:

En el caso de una implementación dependiente del marco x86 (`<PlatformTarget>x86</PlatformTarget>`), habilite el grupo de aplicaciones de IIS para aplicaciones de 32 bits. En el Administrador de IIS, abra la **Configuración avanzada** del grupo de aplicaciones y establezca **Habilitar aplicaciones de 32 bits** en **True**.

## <a name="platform-conflicts-with-rid"></a>Conflictos de plataforma con RID

* **Explorador:** error HTTP 502.5: error en el proceso

* **Registro de aplicación:** la aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"C:\{{PATH}{ASSEMBLY}.{exe|dll}"', código de error = '0x80004005 : ff'.

* **Registro de stdout del módulo ASP.NET Core:** Excepción no controlada: System.BadImageFormatException: No se ha podido cargar el archivo ni el ensamblado '{ASSEMBLY}.dll'. Se ha intentado cargar un programa con un formato incorrecto.

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [Solución de problemas (IIS)](xref:host-and-deploy/iis/troubleshoot) o [Solución de problemas (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Si esta excepción se produce en una implementación de Azure Apps al actualizar una aplicación e implementar ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior. Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Punto de conexión de URI incorrecto o sitio web detenido

* **Explorador:** ERR_CONNECTION_REFUSED **--O--** No se podido establecer la conexión

* **Registro de aplicación:** No hay ninguna entrada

* **Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker range=">= aspnetcore-2.2"

* **Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker-end

Solución del problema:

* Confirme que se usa el punto de conexión de URI correcto para la aplicación. Compruebe los enlaces.

* Confirme que el sitio web de IIS no está en estado *Detenido*.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Características de servidor CoreWebEngine o W3SVC deshabilitadas

**Excepción de sistema operativo:** Para usar el módulo ASP.NET Core, se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC.

Solución del problema:

Confirme que están habilitados el rol y las características correctos. Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Ruta de acceso física de sitio web incorrecta o aplicación que falta

* **Explorador:** 403 Prohibido: acceso denegado **--O BIEN--** 403.14 Prohibido: el servidor web está configurado para no mostrar una lista de los contenidos de este directorio.

* **Registro de aplicación:** No hay ninguna entrada

* **Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker range=">= aspnetcore-2.2"

* **Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker-end

Solución del problema:

Consulte la opción **Configuración básica** del sitio web de IIS y la carpeta de la aplicación física. Confirme que la aplicación está en la carpeta en la **ruta de acceso física** del sitio web de IIS.

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a>Rol incorrecto, módulo de ASP.NET Core no instalado o permisos incorrectos

* **Explorador:** 500.19 Error interno del servidor: no se puede acceder a la página solicitada porque los datos de configuración relacionados de la página no son válidos. **--O--** No se puede mostrar esta página

* **Registro de aplicación:** No hay ninguna entrada

* **Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker range=">= aspnetcore-2.2"

* **Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker-end

Solución del problema:

* Confirme que está habilitado el rol adecuado. Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Abra **Programas y características** o **Aplicaciones y características** y confirme que **Hospedaje de Windows Server** está instalado. Si **Hospedaje de Windows Server** no está presente en la lista de programas instalados, descargue e instale el conjunto de hospedaje de .NET Core.

  [Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Para obtener más información, consulte [Instalar el conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Asegúrese de que **Grupo de aplicaciones** > **Modelo de proceso** > **Identidad** esté establecido en **ApplicationPoolIdentity** o que la identidad personalizada tenga los permisos correctos para acceder a la carpeta de implementación de la aplicación.

* Si ha desinstalado el paquete de hospedaje de ASP.NET Core y ha instalado una versión anterior de dicho paquete, el archivo *applicationHost.config* no incluirá ninguna sección para el módulo ASP.NET Core. Abra *applicationHost.config* en *%windir%/System32/inetsrv/config* y busque el grupo de secciones `<configuration><configSections><sectionGroup name="system.webServer">`. Si falta la sección del módulo ASP.NET Core en el grupo de secciones, añada el elemento de sección:

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```
  
  Si quiere, también puede instalar la versión más reciente del paquete de hospedaje de ASP.NET Core. La versión más reciente es compatible con las versiones anteriores de las aplicaciones ASP.NET Core admitidas.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Elemento processPath incorrecto, falta la variable PATH, agrupación de hospedaje no instalada, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe

::: moniker range=">= aspnetcore-2.2"

* **Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"{...}" ', código de error = '0 x 80070002 : 0. La aplicación '{PATH}' no se ha podido iniciar. No se ha encontrado el archivo ejecutable en '{PATH}'. No se ha podido iniciar la aplicación '/LM/W3SVC/2/ROOT', código de error '0x8007023e'.

* **Registro de stdout del módulo ASP.NET Core:** No se ha creado el archivo de registro.

* **Registro de depuración del módulo de ASP.NET Core:** Registro de eventos: "La aplicación '{PATH}'" no se ha podido iniciar. No se ha encontrado el archivo ejecutable en '{PATH}'. Se ha devuelto un valor HRESULT con errores: 0x8007023e

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Explorador:** error HTTP 502.5: error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"{...}" ', código de error = '0 x 80070002 : 0.

* **Registro de stdout del módulo ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.

::: moniker-end

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [Solución de problemas (IIS)](xref:host-and-deploy/iis/troubleshoot) o [Solución de problemas (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Compruebe el atributo *processPath* del elemento `<aspNetCore>` de *web.config* para confirmar que es `dotnet` para una implementación dependiente del marco (FDD) o `.\{ASSEMBLY}.exe` para una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).

* En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH. Confirme que *C:\Archivos de programa\dotnet\\* existe en el valor PATH del sistema.

* En el caso de una FDD, es posible que la identidad del usuario del grupo de aplicaciones no pueda acceder a *dotnet.exe*. Confirme que la identidad del usuario del grupo de aplicaciones tiene acceso al directorio *C:\Archivos de programa\dotnet*. Confirme que no haya ninguna regla de denegación configurada para la identidad del usuario del grupo de aplicaciones en los directorios *C:\Archivos de programa\dotnet* y de la aplicación.

* Puede que se haya implementado una FDD y que se instalara .NET Core sin reiniciar IIS. Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.

* Puede que se haya implementado una FDD sin instalar el entorno de tiempo de ejecución de .NET Core en el sistema de hospedaje. Si no se ha instalado el entorno de tiempo de ejecución de .NET Core, ejecute el **instalador de la agrupación de hospedaje de .NET Core** en el sistema.

  [Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Para obtener más información, consulte [Instalar el conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

  Si se requiere un tiempo de ejecución específico, descargue el tiempo de ejecución de [Archivos de descarga de .NET](https://dotnet.microsoft.com/download/archives) e instálelo en el sistema. Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

* Puede que se haya implementado una FDD y que el paquete *Microsoft Visual C++ 2015 Redistributable (x64)* no esté instalado en el sistema. Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorrectos del elemento \<aspNetCore>

::: moniker range=">= aspnetcore-2.2"

* **Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM

* **Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas. Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo. No se ha podido encontrar el controlador de la solicitud en proceso. Resultado obtenido al invocar a hostfxr: ¿pretendía ejecutar comandos SDK de dotnet? Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 No se ha podido iniciar la aplicación '/LM/W3SVC/3/ROOT', código de error '0x8000ffff'.

* **Registro de stdout del módulo ASP.NET Core:** ¿pretendía ejecutar comandos SDK de dotnet? Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409

* **Registro de depuración del módulo de ASP.NET Core:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas. Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo. Se ha devuelto un valor HRESULT con errores: 0x8000ffff No se ha podido encontrar el controlador de la solicitud en proceso. Resultado obtenido al invocar a hostfxr: ¿pretendía ejecutar comandos SDK de dotnet? Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Se ha devuelto un valor HRESULT con errores: 0x8000ffff

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Explorador:** error HTTP 502.5: error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"dotnet" .\{ASSEMBLY}.dll', código de error = '0x80004005 : 80008081.

* **Registro de stdout del módulo de ASP.NET Core:** La aplicación que se va a ejecutar no existe: 'PATH\{ASSEMBLY}.dll'

::: moniker-end

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [Solución de problemas (IIS)](xref:host-and-deploy/iis/troubleshoot) o [Solución de problemas (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Examine el atributo *arguments* del elemento `<aspNetCore>` en *web.config* para confirmar que (a) es `.\{ASSEMBLY}.dll` para una implementación dependiente del marco (FDD); o (b) no está presente, es una cadena vacía (`arguments=""`) o es una lista de argumentos de la aplicación (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) para una implementación independiente (SCD).

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a>Falta el marco compartido de .NET Core

* **Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM

* **Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas. Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo. No se ha podido encontrar el controlador de la solicitud en proceso. Resultado obtenido al invocar a hostfxr: No se ha podido encontrar ninguna versión de Framework compatible. El marco especificado 'Microsoft.AspNetCore.App', versión '{VERSION}', no se ha podido encontrar.

No se ha podido iniciar la aplicación '/LM/W3SVC/5/ROOT', código de error '0x8000ffff'.

* **Registro de stdout del módulo de ASP.NET Core:** No se ha podido encontrar ninguna versión de Framework compatible. El marco especificado 'Microsoft.AspNetCore.App', versión '{VERSION}', no se ha podido encontrar.

* **Registro de depuración del módulo de ASP.NET Core:** Se ha devuelto un valor HRESULT con errores: 0x8000ffff

::: moniker-end

Solución del problema:

En el caso de una implementación dependiente del marco (FDD), confirme que tiene instalado el entorno de tiempo de ejecución correcto en el sistema.

## <a name="stopped-application-pool"></a>Grupo de aplicaciones detenido

* **Explorador:** 503 Servicio no disponible

* **Registro de aplicación:** No hay ninguna entrada

* **Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker range=">= aspnetcore-2.2"

* **Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker-end

Solución del problema:

Confirme que el grupo de aplicaciones no está en estado *Detenido*.

## <a name="sub-application-includes-a-handlers-section"></a>La aplicación secundaria incluye una sección de \<controladores>

* **Explorador:** error HTTP 500.19: error interno del servidor

* **Registro de aplicación:** No hay ninguna entrada

* **Registro de stdout del módulo de ASP.NET Core:** El archivo de registro de la aplicación raíz se ha creado y muestra un funcionamiento normal. No se ha creado el archivo de registro de la aplicación secundaria.

::: moniker range=">= aspnetcore-2.2"

* **Registro de depuración del módulo de ASP.NET Core:** El archivo de registro de la aplicación raíz se ha creado y muestra un funcionamiento normal. No se ha creado el archivo de registro de la aplicación secundaria.

::: moniker-end

Solución del problema:

::: moniker range=">= aspnetcore-2.2"

Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>` o que la aplicación secundaria no hereda los controladores de la aplicación primaria.

La sección `<system.webServer>` de la aplicación primaria de *web.config* está colocada dentro de un elemento `<location>`. La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación primaria no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location). Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.

::: moniker-end

## <a name="stdout-log-path-incorrect"></a>Ruta de acceso incorrecta al registro de stdout

* **Explorador:** la aplicación responde normalmente.

::: moniker range=">= aspnetcore-2.2"

* **Registro de aplicación:** No se ha podido iniciar la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Mensaje de excepción: HRESULT 0 x 80070005 se ha devuelto en {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. No se ha podido detener la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Mensaje de excepción: HRESULT 0 x 80070002 se ha devuelto en {PATH}. No se ha podido iniciar la redirección de stdout en {PATH}\aspnetcorev2_inprocess.dll.

* **Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

* **Registro de depuración del módulo de ASP.NET Core:** No se ha podido iniciar la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Mensaje de excepción: HRESULT 0 x 80070005 se ha devuelto en {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. No se ha podido detener la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Mensaje de excepción: HRESULT 0 x 80070002 se ha devuelto en {PATH}. No se ha podido iniciar la redirección de stdout en {PATH}\aspnetcorev2_inprocess.dll.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Registro de aplicación:** Advertencia: No se ha podido crear el archivo de registro de stdout \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, código de error = -2147024893.

* **Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.

::: moniker-end

Solución del problema:

* La ruta de acceso `stdoutLogFile` especificada en el elemento `<aspNetCore>` de *web.config* no existe. Para obtener más información, consulte [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creación y redireccionamiento de registros: módulo de ASP.NET Core).

* El usuario del grupo de aplicaciones no tiene acceso de escritura a la ruta de acceso del registro de stdout.

## <a name="application-configuration-general-issue"></a>Problema general de configuración de aplicación

::: moniker range=">= aspnetcore-2.2"

* **Explorador:** error HTTP 500.0: error de carga del controlador en proceso ANCM **--O--** Error HTTP 500.30: error de inicio en el proceso ANCM

* **Registro de aplicación:** Variable

* **Registro de stdout del módulo de ASP.NET Core:** El archivo de registro se ha creado, pero está vacío o se ha creado con entradas normales, hasta el punto en que se producen errores en la aplicación.

* **Registro de depuración del módulo de ASP.NET Core:** Variable

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Explorador:** error HTTP 502.5: error en el proceso

* **Registro de aplicación:** la aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{PATH}\'' ha creado el proceso con la línea de comandos '"C:\{PATH}\{ASSEMBLY}.{exe|dll}"', pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado "{PORT}", código de error = '{ERROR CODE}'

* **Registro de stdout del módulo de ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.

::: moniker-end

Solución del problema:

El proceso no se ha iniciado, probablemente debido a un problema de configuración o programación de la aplicación.

Para obtener más información, vea los temas siguientes:

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
