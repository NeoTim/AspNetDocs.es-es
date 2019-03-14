---
title: Hospedaje e implementación de Razor Components
author: guardrex
description: 'Descubra cómo hospedar e implementar aplicaciones de Blazor y Razor Components con ASP.NET Core, redes de entrega de contenido (CDN), servidores de archivos y GitHub Pages.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
---
# <a name="host-and-deploy-razor-components"></a>Hospedaje e implementación de Razor Components

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a>Publicar la aplicación

Las aplicaciones se publican para la implementación en la configuración de lanzamiento con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish). Un IDE puede controlar la ejecución del comando `dotnet publish` automáticamente mediante sus características de publicación integradas, por lo que puede que no sea necesario ejecutar manualmente el comando desde un símbolo del sistema, en función de las herramientas de desarrollo que se usen.

```console
dotnet publish -c Release
```

`dotnet publish` desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación. Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación. La implementación se crea en la carpeta */bin/Release/&lt;plataforma-de-destino&gt;/publish*.

Los recursos de la carpeta *publish* se implementan en el servidor web. La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.

## <a name="host-configuration-values"></a>Valores de configuración de host

Las aplicaciones de Razor Components que usan el [modelo de hospedaje del lado servidor](xref:razor-components/hosting-models#server-side-hosting-model) pueden aceptar [valores de configuración de host web](xref:fundamentals/host/web-host#host-configuration-values).

Las aplicaciones de Blazor que usan el [modelo de hospedaje del lado cliente](xref:razor-components/hosting-models#client-side-hosting-model) pueden aceptar los siguientes valores de configuración de host como argumentos de línea de comandos en tiempo de ejecución en el entorno de desarrollo.

### <a name="content-root"></a>Raíz del contenido

El argumento `--contentroot` establece la ruta de acceso absoluta al directorio que incluye los archivos de contenido de la aplicación.

* Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema. En el directorio de la aplicación, ejecute lo siguiente:

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**. Este valor se selecciona al ejecutar la aplicación con el depurador de Visual Studio y al ejecutarla desde un símbolo del sistema con `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**. Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a>Ruta de acceso base

El argumento `--pathbase` establece la ruta de acceso base de la aplicación para una aplicación que se ejecuta localmente con una ruta de acceso virtual que no es raíz (el valor `href` de la etiqueta `<base>` se establece en una ruta de acceso que no sea `/` para ensayo y producción). Para obtener más información, vea la sección [Ruta de acceso base de la aplicación](#app-base-path).

> [!IMPORTANT]
> A diferencia de la ruta de acceso proporcionada al valor `href` de la etiqueta `<base>`, no incluya una barra diagonal final (`/`) al pasar el valor del argumento `--pathbase`. Si se proporciona la ruta de acceso base de la aplicación en la etiqueta `<base>` como `<base href="/CoolApp/" />` (se incluye una barra diagonal final), se pasa el valor del argumento de línea de comandos como `--pathbase=/CoolApp` (sin barra diagonal final).

* Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema. En el directorio de la aplicación, ejecute lo siguiente:

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**. Este valor se selecciona al ejecutar la aplicación con el depurador de Visual Studio y al ejecutarla desde un símbolo del sistema con `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**. Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a>Direcciones URL

El argumento `--urls` indica las direcciones IP o las direcciones de host con los puertos y protocolos en los que escuchar las solicitudes.

* Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema. En el directorio de la aplicación, ejecute lo siguiente:

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**. Este valor se selecciona al ejecutar la aplicación con el depurador de Visual Studio y al ejecutarla desde un símbolo del sistema con `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**. Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a>Implementación de una aplicación Blazor del lado cliente

Con el [modelo de hospedaje del lado cliente](xref:razor-components/hosting-models#client-side-hosting-model):

* La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.
* La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Se puede seguir cualquiera de las estrategias siguientes:
  * Una aplicación ASP.NET Core proporciona la aplicación Blazor. Este proceso se trata en la sección [Implementación hospedada en Blazor del lado cliente con ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core).
  * La aplicación Blazor se coloca en un servicio o servidor web de hospedaje estático, donde no se usa .NET para proporcionar la aplicación Blazor. Este proceso se trata en la sección [Implementación independiente de Blazor del lado cliente](#client-side-blazor-standalone-deployment).
  
### <a name="configure-the-linker"></a>Configurar el enlazador

Blazor realiza la vinculación de lenguaje intermedio (IL) en cada compilación para quitar el IL innecesario de los ensamblados de salida. Puede controlar la vinculación del ensamblado durante la compilación. Para obtener más información, consulta <xref:host-and-deploy/razor-components/configure-linker>.

### <a name="rewrite-urls-for-correct-routing"></a>Reescritura de las URL para conseguir un enrutamiento correcto

Enrutar las solicitudes para los componentes de la página en una aplicación del lado cliente no es tan sencillo como enrutar las solicitudes a una aplicación hospedada del lado servidor. Imagine que tiene una aplicación del lado cliente con dos páginas:

* **_Main.cshtml_**: se carga en la raíz de la aplicación y contiene un vínculo a la página de información (`href="About"`).
* **_About.cshtml_**: página de información.

Cuando se solicita el documento predeterminado de la aplicación mediante la barra de direcciones del explorador (por ejemplo, `https://www.contoso.com/`):

1. El explorador realiza una solicitud.
1. Se devuelve la página predeterminada, que suele ser *index.html*.
1. *index.html* arranca la aplicación.
1. Se carga el enrutador de Blazor y se muestra la página principal de Razor (*Main.cshtml*).

En la página principal, al seleccionar el vínculo a la página de información, se carga la página de información. Seleccionar el vínculo a la página de información funciona en el cliente porque el enrutador de Blazor impide que el explorador realice una solicitud en Internet a `www.contoso.com` sobre `About` y presenta la propia página de información. Todas las solicitudes de páginas internas *dentro de la aplicación del lado cliente* funcionan del mismo modo: Las solicitudes no desencadenan solicitudes basadas en el explorador a recursos hospedados en el servidor en Internet. El enrutador controla las solicitudes de forma interna.

Si se realiza una solicitud mediante la barra de direcciones del explorador para `www.contoso.com/About`, se produce un error. Este recurso no existe en el host de Internet de la aplicación, por lo que se devuelve una respuesta *404 No encontrado*.

Dado que los exploradores realizan solicitudes a hosts basados en Internet para las páginas del lado cliente, los servidores web y los servicios de hospedaje deben reescribir todas las solicitudes de recursos que no estén físicamente en el servidor a la página *index.html*. Cuando se devuelve *index.html*, el enrutador de la aplicación del lado cliente se hace cargo y responde con el recurso correcto.

### <a name="app-base-path"></a>Ruta de acceso base de la aplicación

La ruta de acceso base de la aplicación es la ruta de acceso raíz de la aplicación virtual en el servidor. Por ejemplo, a una aplicación que reside en el servidor de Contoso, en una carpeta virtual de `/CoolApp/`, se accede desde `https://www.contoso.com/CoolApp`; su ruta de acceso base virtual es `/CoolApp/`. Al establecer la ruta de acceso base de la aplicación en `CoolApp/`, la aplicación sabe dónde reside virtualmente en el servidor. La aplicación puede usar la ruta de acceso base de la aplicación para construir direcciones URL relativas a la raíz de la aplicación desde un componente que no se encuentre en el directorio raíz. Esto permite a los componentes que existen en diferentes niveles de la estructura de directorios compilar vínculos a otros recursos en ubicaciones de toda la aplicación. La ruta de acceso base de la aplicación también se usa para interceptar clics en hipervínculos en los que el destino `href` del vínculo está dentro del espacio de URI de la ruta de acceso base de la aplicación y es el enrutador de Blazor quien controla la navegación interna.

En muchos escenarios de hospedaje, la ruta de acceso virtual del servidor a la aplicación es la raíz de la aplicación. En estos casos, la ruta de acceso base de la aplicación es una barra diagonal (`<base href="/" />`), que es la configuración predeterminada para una aplicación. En otros escenarios de hospedaje, como las subaplicaciones o los directorios virtuales de IIS y GitHub Pages, la ruta de acceso base de la aplicación debe establecerse en la ruta de acceso virtual del servidor a la aplicación. Para establecer la ruta de acceso base de la aplicación, agregue o actualice la etiqueta `<base>` en *index.html*, que se encuentra dentro de los elementos de etiqueta `<head>`. Establezca el valor del atributo `href` en `<virtual-path>/` (la barra diagonal final es necesaria), donde `<virtual-path>/` es la ruta de acceso raíz de la aplicación virtual completa en el servidor para la aplicación. En el ejemplo anterior, se establece la ruta de acceso virtual en `CoolApp/`: `<base href="CoolApp/" />`.

En el caso de una aplicación con una ruta de acceso virtual que no es raíz configurada (por ejemplo, `<base href="CoolApp/" />`), la aplicación no puede encontrar sus recursos *cuando se ejecuta de forma local*. Para solucionar este problema durante la fase de desarrollo y pruebas local, puede proporcionar un argumento de *ruta de acceso base* que coincida con el valor `href` de la etiqueta `<base>` en tiempo de ejecución.

Para pasar el argumento de ruta de acceso base con la ruta de acceso raíz (`/`) al ejecutar la aplicación de forma local, ejecute el siguiente comando desde el directorio de la aplicación:

```console
dotnet run --pathbase=/CoolApp
```

La aplicación responde de forma local en `http://localhost:port/CoolApp`.

Para obtener más información, consulte la sección sobre el [valor de configuración de host de la ruta de acceso base](#path-base).

> [!IMPORTANT]
> Si una aplicación usa el [modelo de hospedaje del lado cliente](xref:razor-components/hosting-models#client-side-hosting-model) (basado en la plantilla de proyecto de **Blazor**) y se hospeda como una subaplicación de IIS en una aplicación ASP.NET Core, es importante deshabilitar el controlador del módulo de ASP.NET Core heredado o asegurarse de que la subaplicación no hereda la sección `<handlers>` de la aplicación raíz (principal) en el archivo *web.config*.
>
> Para quitar el controlador del archivo *web.config* publicado de la aplicación, agregue una sección `<handlers>` al archivo:
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> Como alternativa, deshabilite la herencia de la sección `<system.webServer>` de la aplicación raíz (principal) mediante un elemento `<location>` con `inheritInChildApplications` establecido en `false`:
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> Además de configurarse la ruta de acceso base de la aplicación, se quita el controlador o se deshabilita la herencia, como se describe en esta sección. Establezca la ruta de acceso base de la aplicación en el archivo *index.html* de la aplicación en el alias de IIS que ha usado al configurar la subaplicación en IIS.

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a>Implementación hospedada en Blazor del lado cliente con ASP.NET Core

Una *implementación hospedada* proporciona la aplicación Blazor del lado cliente a los exploradores desde una [aplicación ASP.NET Core](xref:index) que se ejecuta en un servidor.

La aplicación Blazor se incluye con la aplicación ASP.NET Core en la salida publicada para que ambas se implementen juntas. Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. En el caso de una implementación hospedada, Visual Studio incluye la plantilla de proyecto de **Blazor (hospedada en ASP.NET Core)** (la plantilla `blazorhosted` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.

Para obtener información sobre cómo implementar en Azure App Service, consulte los temas siguientes:

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.

### <a name="client-side-blazor-standalone-deployment"></a>Implementación independiente de Blazor del lado cliente

Una *implementación independiente* proporciona la aplicación Blazor del lado cliente como un conjunto de archivos estáticos que los clientes solicitan directamente. No se usa ningún servidor web para proporcionar la aplicación Blazor.

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a>Hospedaje independiente de Blazor del lado cliente con IIS

IIS es un servidor de archivos estáticos compatible con las aplicaciones de Blazor. Para configurar IIS para hospedar Blazor, vea [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Compilación de un sitio web estático en IIS).

Los recursos publicados se crean en la carpeta *\\bin\\Release\\&lt;plataforma-de-destino&gt;\\publish*. Hospede el contenido de la carpeta *publish* en el servidor web o el servicio de hospedaje.

**web.config**

Cuando se publica un proyecto de Blazor, se crea un archivo *web.config* con la siguiente configuración de IIS:

* Se establecen los tipos MIME de las siguientes extensiones de archivo:
  * *\*.dll*: `application/octet-stream`
  * *\*.json*: `application/json`
  * *\*.wasm*: `application/wasm`
  * *\*.woff*: `application/font-woff`
  * *\*.woff2*: `application/font-woff`
* Se habilita la compresión HTTP de los siguientes tipos MIME:
  * `application/octet-stream`
  * `application/wasm`
* Se establecen reglas del módulo URL Rewrite:
  * Proporcionar el subdirectorio donde residen los recursos estáticos de la aplicación (*&lt;nombre_ensamblado&gt;\\dist\\&lt;ruta_acceso_solicitada&gt;*).
  * Crear el enrutamiento de reserva de SPA para que las solicitudes de recursos que no sean archivos se redirijan al documento predeterminado de la aplicación en su carpeta de recursos estáticos (*&lt;nombre_ensamblado&gt;\\dist\\index.html*).

**Instalación del módulo URL Rewrite**

El [módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) es necesario para reescribir las URL. El módulo no se instala de forma predeterminada y no está disponible para instalar como una característica de servicio de rol del servidor web (IIS). El módulo se debe descargar desde el sitio web de IIS. Use el instalador de plataforma web para instalar el módulo:

1. De forma local, vaya a la [página de descargas del módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). En el caso de la versión en inglés, seleccione **WebPI** para descargar el instalador de WebPI. En el caso de otros idiomas, seleccione la arquitectura adecuada del servidor (x86/x64) para descargar el instalador.
1. Copie el instalador en el servidor. Ejecute el instalador. Haga clic en el botón **Instalar** y acepte los términos de licencia. No es necesario reiniciar el servidor al finalizar la instalación.

**Configuración del sitio web**

Configure la **ruta de acceso física** del sitio web a la carpeta de la aplicación. La carpeta contiene:

* El archivo *web.config* que usa IIS para configurar el sitio web, incluidas las reglas de redireccionamiento y los tipos de contenido de archivos necesarios.
* La carpeta de recursos estáticos de la aplicación.

**Solución de problemas**

Si se recibe un error *500 Error interno del servidor* y el administrador de IIS produce errores al intentar acceder a la configuración del sitio web, confirme que el módulo URL Rewrite esté instalado. Si no lo está, IIS no puede analizar el archivo *web.config*. Esto impide que el Administrador de IIS cargue la configuración del sitio web y que el sitio web proporcione los archivos estáticos de Blazor.

Para obtener más información sobre cómo solucionar problemas de las implementaciones en IIS, vea <xref:host-and-deploy/iis/troubleshoot>.

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a>Hospedaje independiente de Blazor del lado cliente con Nginx

El siguiente archivo *nginx.conf* se ha simplificado para mostrar cómo configurar Nginx para enviar el archivo *Index.html* siempre que no pueda encontrar un archivo correspondiente en el disco.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

Para obtener más información sobre la configuración del servidor web de producción de Nginx, consulte [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creación de archivos de configuración de NGINX y NGINX Plus).

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a>Hospedaje independiente de Blazor del lado cliente con Nginx en Docker

Para hospedar Blazor en Docker mediante Nginx, configure el Dockerfile para usar la imagen de Nginx basada en Alpine. Actualice el Dockerfile para copiar el archivo *nginx.config* en el contenedor.

Agregue una línea al Dockerfile, como se muestra en el ejemplo siguiente:

```
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a>Hospedaje independiente de Blazor del lado cliente con GitHub Pages

Para controlar las reescrituras de URL, agregue un archivo *404.html* con un script que controle el redireccionamiento de la solicitud a la página *index.html*. Para consultar una implementación de ejemplo que ha proporcionado la comunidad, vea [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)) (Aplicaciones de página única para GitHub Pages [rafrex/spa-github-pages en GitHub]). Se puede ver un ejemplo del enfoque de la comunidad en [blazor-demo/blazor-demo.github.io en GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sitio activo](https://blazor-demo.github.io/)).

Al usar un sitio de proyecto en lugar de un sitio de la organización, agregue o actualice la etiqueta `<base>` en *index.html*. Establezca el valor del atributo `href` en `<repository-name>/`, donde `<repository-name>/` es el nombre del repositorio de GitHub.

## <a name="deploy-a-server-side-razor-components-app"></a>Implementación de una aplicación Razor Components del lado servidor

Con el [modelo de hospedaje del lado servidor](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de SignalR.

La aplicación se incluye con la aplicación ASP.NET Core en la salida publicada para que ambas se implementen juntas. Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. En el caso de una implementación del lado servidor, Visual Studio incluye la plantilla de proyecto de **Blazor (del lado servidor en ASP.NET Core)** (la plantilla `blazorserver` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Cuando se publica la aplicación ASP.NET Core, la aplicación Razor Components se incluye en la salida publicada para que ambas se puedan implementar juntas. Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.

Para obtener información sobre cómo implementar en Azure App Service, consulte los temas siguientes:

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.
