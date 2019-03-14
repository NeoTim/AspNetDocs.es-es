---
title: Hospedar ASP.NET Core en Linux con Nginx
author: rick-anderson
description: Sepa cómo configurar Nginx como un proxy inverso en Ubuntu 16.04 para reenviar el tráfico HTTP a una aplicación web de ASP.NET Core que se ejecuta en Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: a04927ca0377b965f3b4574e55fb9ed450959a7f
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57041782"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="4d78c-103">Hospedar ASP.NET Core en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="4d78c-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="4d78c-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="4d78c-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="4d78c-105">En esta guía se explica cómo configurar un entorno de ASP.NET Core listo para producción en un servidor de Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="4d78c-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="4d78c-106">Probablemente estas instrucciones sean válidas también con versiones más recientes de Ubuntu, pero no se han probado con versiones más recientes.</span><span class="sxs-lookup"><span data-stu-id="4d78c-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="4d78c-107">Para información sobre otras distribuciones de Linux compatibles con ASP.NET Core, consulte [Requisitos previos para .NET Core en Linux](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="4d78c-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="4d78c-108">En Ubuntu 14.04, *supervisord* is es la solución recomendada para supervisar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4d78c-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="4d78c-109">*systemd* no está disponible en Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="4d78c-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="4d78c-110">Para obtener instrucciones de Ubuntu 14.04, vea la [versión anterior de este tema](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="4d78c-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="4d78c-111">En esta guía:</span><span class="sxs-lookup"><span data-stu-id="4d78c-111">This guide:</span></span>

* <span data-ttu-id="4d78c-112">Se coloca una aplicación ASP.NET Core existente detrás de un servidor proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="4d78c-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="4d78c-113">Se configura el servidor proxy inverso para reenviar las solicitudes al servidor web de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4d78c-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="4d78c-114">Se garantiza que la aplicación web se ejecute al inicio como un demonio.</span><span class="sxs-lookup"><span data-stu-id="4d78c-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="4d78c-115">Se configura una herramienta de administración de procesos para ayudar a reiniciar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4d78c-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d78c-116">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4d78c-116">Prerequisites</span></span>

1. <span data-ttu-id="4d78c-117">Acceso a un servidor de Ubuntu 16.04 con una cuenta de usuario estándar con privilegios sudo.</span><span class="sxs-lookup"><span data-stu-id="4d78c-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="4d78c-118">Tener .NET Core Runtime instalado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4d78c-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="4d78c-119">Vaya a la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="4d78c-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="4d78c-120">Seleccione el tiempo de ejecución más reciente que no sea versión preliminar en la lista **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="4d78c-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="4d78c-121">Selecciónelo y siga las instrucciones de Ubuntu correspondientes a la versión de Ubuntu del servidor.</span><span class="sxs-lookup"><span data-stu-id="4d78c-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="4d78c-122">Disponer de una aplicación de ASP.NET Core existente.</span><span class="sxs-lookup"><span data-stu-id="4d78c-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="4d78c-123">Publicar y copiar en la aplicación</span><span class="sxs-lookup"><span data-stu-id="4d78c-123">Publish and copy over the app</span></span>

<span data-ttu-id="4d78c-124">Configure la aplicación para una [implementación dependiente de Framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="4d78c-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="4d78c-125">Ejecute [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el entorno de desarrollo para empaquetar una aplicación en un directorio (por ejemplo, *bin/Release/&lt;target_framework_moniker&gt;/publish*) que se pueda ejecutar en el servidor:</span><span class="sxs-lookup"><span data-stu-id="4d78c-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="4d78c-126">La aplicación también se puede publicar como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) si prefiere no mantener .NET Core Runtime en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4d78c-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="4d78c-127">Copie la aplicación de ASP.NET Core en el servidor usando una herramienta que se integre en el flujo de trabajo de la organización (como SCP o SFTP).</span><span class="sxs-lookup"><span data-stu-id="4d78c-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="4d78c-128">Es habitual encontrar las aplicaciones web en el directorio *var* (por ejemplo, *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="4d78c-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="4d78c-129">En un escenario de implementación de producción, un flujo de trabajo de integración continua lleva a cabo la tarea de publicar la aplicación y copiar los recursos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4d78c-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="4d78c-130">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4d78c-130">Test the app:</span></span>

1. <span data-ttu-id="4d78c-131">Desde la línea de comandos, ejecute la aplicación: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="4d78c-132">En un explorador, vaya a `http://<serveraddress>:<port>` para comprobar que la aplicación funciona en Linux de forma local.</span><span class="sxs-lookup"><span data-stu-id="4d78c-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="4d78c-133">Configurar un servidor proxy inverso</span><span class="sxs-lookup"><span data-stu-id="4d78c-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="4d78c-134">Un proxy inverso es una configuración común para trabajar con aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="4d78c-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="4d78c-135">Un proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d78c-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="4d78c-136">Usar un servidor proxy inverso</span><span class="sxs-lookup"><span data-stu-id="4d78c-136">Use a reverse proxy server</span></span>

<span data-ttu-id="4d78c-137">Kestrel resulta muy adecuado para suministrar contenido dinámico de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d78c-137">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="4d78c-138">Sin embargo, las funcionalidades de servicio web no son tan completas como las de los servidores, como IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="4d78c-138">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="4d78c-139">Un servidor proxy inverso puede reducir las cargas de trabajo, por ejemplo, suministrar contenido estático, almacenar solicitudes en caché, comprimir solicitudes y finalizar HTTPS desde el servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="4d78c-139">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="4d78c-140">Un servidor proxy inverso puede residir en un equipo dedicado o se puede implementar junto con un servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="4d78c-140">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="4d78c-141">Para los fines de esta guía, se usa una única instancia de Nginx.</span><span class="sxs-lookup"><span data-stu-id="4d78c-141">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="4d78c-142">Se ejecuta en el mismo servidor, junto con el servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="4d78c-142">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="4d78c-143">En función de requisitos, se puede elegir una configuración diferente.</span><span class="sxs-lookup"><span data-stu-id="4d78c-143">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="4d78c-144">Como el proxy inverso reenvía las solicitudes, use el [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) del paquete [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="4d78c-144">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="4d78c-145">El middleware actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que los URI de redireccionamiento y otras directivas de seguridad funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="4d78c-145">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="4d78c-146">Cualquier componente que dependa del esquema (como la autenticación, la generación de vínculos, los redireccionamientos o la geolocalización) debe colocarse después de invocar al Middleware de encabezados reenviados.</span><span class="sxs-lookup"><span data-stu-id="4d78c-146">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="4d78c-147">Como norma general, el Middleware de encabezados reenviados se debe ejecutar antes de cualquier otro middleware, salvo el middleware de diagnóstico y control de errores.</span><span class="sxs-lookup"><span data-stu-id="4d78c-147">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="4d78c-148">Hacerlo en ese orden garantiza que el middleware que se basa en la información de encabezados reenviados pueda usar los valores de encabezado para procesarlos.</span><span class="sxs-lookup"><span data-stu-id="4d78c-148">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d78c-149">Invoque el método <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> en `Startup.Configure` antes de llamar a <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> o un middleware de esquema de autenticación similar.</span><span class="sxs-lookup"><span data-stu-id="4d78c-149">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="4d78c-150">Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="4d78c-150">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d78c-151">Invoque el método <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> en `Startup.Configure` antes de llamar a <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> y <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> o un middleware de esquema de autenticación similar.</span><span class="sxs-lookup"><span data-stu-id="4d78c-151">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> and <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="4d78c-152">Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="4d78c-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="4d78c-153">Si no se especifica ningún valor <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> para el middleware, los encabezados predeterminados para reenviar son `None`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-153">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="4d78c-154">Solo los servidores proxy que se ejecutan en localhost (127.0.0.1, [::1]) son de confianza de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="4d78c-154">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="4d78c-155">Si otras redes o servidores proxy de confianza de la organización tramitan solicitudes entre Internet y el servidor web, agréguelos a la lista de <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> o <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> con <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="4d78c-155">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="4d78c-156">En el ejemplo siguiente se agrega un servidor proxy de confianza en la dirección IP 10.0.0.100 al middleware de encabezados reenviados `KnownProxies` en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4d78c-156">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="4d78c-157">Para obtener más información, consulta <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4d78c-157">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="4d78c-158">Instalar Nginx</span><span class="sxs-lookup"><span data-stu-id="4d78c-158">Install Nginx</span></span>

<span data-ttu-id="4d78c-159">Use `apt-get` para instalar Nginx.</span><span class="sxs-lookup"><span data-stu-id="4d78c-159">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="4d78c-160">El instalador crea un script de inicio *systemd* que ejecuta Nginx como demonio al iniciarse el sistema.</span><span class="sxs-lookup"><span data-stu-id="4d78c-160">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="4d78c-161">Siga las instrucciones de instalación para Ubuntu en [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx: paquetes oficiales de Debian y Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="4d78c-161">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="4d78c-162">Si se necesitan módulos de Nginx opcionales, puede que haya que compilar Nginx desde el origen.</span><span class="sxs-lookup"><span data-stu-id="4d78c-162">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="4d78c-163">Puesto que Nginx se ha instalado por primera vez, ejecute lo siguiente para iniciarlo de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="4d78c-163">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="4d78c-164">Compruebe que un explorador muestra la página de aterrizaje predeterminada de Nginx.</span><span class="sxs-lookup"><span data-stu-id="4d78c-164">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="4d78c-165">La página de aterrizaje está accesible en `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-165">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="4d78c-166">Configurar Nginx</span><span class="sxs-lookup"><span data-stu-id="4d78c-166">Configure Nginx</span></span>

<span data-ttu-id="4d78c-167">Para configurar Nginx como un proxy inverso para reenviar solicitudes a la aplicación ASP.NET Core, modifique */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="4d78c-167">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="4d78c-168">Ábralo en un editor de texto y reemplace el contenido por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="4d78c-168">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="4d78c-169">Cuando no hay ninguna coincidencia de `server_name`, Nginx usa el servidor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="4d78c-169">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="4d78c-170">Si no se define ningún servidor predeterminado, el primer servidor del archivo de configuración es el servidor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="4d78c-170">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="4d78c-171">Como procedimiento recomendado, agregue un servidor predeterminado específico que devuelva un código de estado 444 en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="4d78c-171">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="4d78c-172">Un ejemplo de configuración del servidor predeterminado es:</span><span class="sxs-lookup"><span data-stu-id="4d78c-172">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="4d78c-173">Con el archivo de configuración anterior y el servidor predeterminado, Nginx acepta tráfico público en el puerto 80 con el encabezado de host `example.com` o `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-173">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="4d78c-174">Las solicitudes que no coincidan con estos hosts no se reenviarán al Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4d78c-174">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="4d78c-175">Nginx reenvía las solicitudes coincidentes con Kestrel a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-175">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="4d78c-176">Para más información, consulte [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) (Cómo Nginx procesa una solicitud).</span><span class="sxs-lookup"><span data-stu-id="4d78c-176">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="4d78c-177">Para cambiar la IP o el puerto de Kestrel, vea [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (Kestrel: configuración de los puntos de conexión).</span><span class="sxs-lookup"><span data-stu-id="4d78c-177">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="4d78c-178">Si no se especifica una [directiva de server_name](https://nginx.org/docs/http/server_names.html) adecuada, su aplicación se expone a vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="4d78c-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="4d78c-179">Los enlaces de carácter comodín de subdominio (por ejemplo, `*.example.com`) no presentan este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable).</span><span class="sxs-lookup"><span data-stu-id="4d78c-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="4d78c-180">Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4d78c-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="4d78c-181">Una vez establecida la configuración de Nginx, ejecute `sudo nginx -t` para comprobar la sintaxis de los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="4d78c-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="4d78c-182">Si la prueba del archivo de configuración es correcta, fuerce a Nginx a recopilar los cambios mediante la ejecución de `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="4d78c-183">Para ejecutar la aplicación directamente en el servidor:</span><span class="sxs-lookup"><span data-stu-id="4d78c-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="4d78c-184">Vaya al directorio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4d78c-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="4d78c-185">Ejecute la aplicación: `dotnet <app_assembly.dll>`, donde `app_assembly.dll` es el nombre de archivo de ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4d78c-185">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="4d78c-186">Si la aplicación se ejecuta en el servidor, pero no responde a través de Internet, compruebe el firewall del servidor y confirme que el puerto 80 está abierto.</span><span class="sxs-lookup"><span data-stu-id="4d78c-186">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="4d78c-187">Si está usando una máquina virtual Ubuntu de Azure, agregue una regla de grupo de seguridad de red que posibilite el tráfico entrante del puerto 80.</span><span class="sxs-lookup"><span data-stu-id="4d78c-187">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="4d78c-188">No es necesario para habilitar una regla de tráfico saliente en el puerto 80, ya que dicho tráfico se concede automáticamente al habilitar la regla de entrada.</span><span class="sxs-lookup"><span data-stu-id="4d78c-188">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="4d78c-189">Cuando termine de probar la aplicación, ciérrela con `Ctrl+C` en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="4d78c-189">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="4d78c-190">Supervisión de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4d78c-190">Monitor the app</span></span>

<span data-ttu-id="4d78c-191">Nginx ahora está configurado para reenviar las solicitudes realizadas a `http://<serveraddress>:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-191">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="4d78c-192">Sin embargo, Nginx no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4d78c-192">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="4d78c-193">Se puede usar *systemd* para crear un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="4d78c-193">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="4d78c-194">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="4d78c-194">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="4d78c-195">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="4d78c-195">Create the service file</span></span>

<span data-ttu-id="4d78c-196">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="4d78c-196">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="4d78c-197">Este es un archivo de servicio de ejemplo de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4d78c-197">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="4d78c-198">Si el usuario *www-data* no se usó en la configuración, primero se debe crear el usuario aquí definido y otorgársele la propiedad adecuada de los archivos.</span><span class="sxs-lookup"><span data-stu-id="4d78c-198">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="4d78c-199">Use `TimeoutStopSec` para configurar el período de tiempo que hay que esperar para que la aplicación se apague después de que reciba la señal de interrupción inicial.</span><span class="sxs-lookup"><span data-stu-id="4d78c-199">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="4d78c-200">Si la aplicación no se apaga en este período, se emite SIGKILL para terminar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4d78c-200">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="4d78c-201">Proporcione el valor como segundos sin unidad (por ejemplo, `150`), un intervalo de tiempo (por ejemplo, `2min 30s`) o `infinity` para deshabilitar el tiempo de expiración.</span><span class="sxs-lookup"><span data-stu-id="4d78c-201">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="4d78c-202">El valor predeterminado de `TimeoutStopSec` es el valor de `DefaultTimeoutStopSec` del archivo de configuración del administrador (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf* o *user.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="4d78c-202">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="4d78c-203">El tiempo de expiración predeterminado para la mayoría de las distribuciones es de 90 segundos.</span><span class="sxs-lookup"><span data-stu-id="4d78c-203">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="4d78c-204">Linux tiene un sistema de archivos que distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4d78c-204">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="4d78c-205">Al establecer ASPNETCORE_ENVIRONMENT en "Production", se busca el archivo de configuración *appsettings.Production.json*, en lugar de *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="4d78c-205">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="4d78c-206">Algunos valores (por ejemplo, cadenas de conexión de SQL) deben ser de escape para que los proveedores de configuración lean las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="4d78c-206">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="4d78c-207">Use el siguiente comando para generar un valor de escape correctamente para su uso en el archivo de configuración:</span><span class="sxs-lookup"><span data-stu-id="4d78c-207">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="4d78c-208">Guarde el archivo y habilite el servicio.</span><span class="sxs-lookup"><span data-stu-id="4d78c-208">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="4d78c-209">Inicie el servicio y compruebe que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="4d78c-209">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="4d78c-210">Con el proxy inverso configurado y Kestrel administrado a través de systemd, la aplicación web está completamente configurada y se puede acceder a ella desde un explorador en la máquina local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-210">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="4d78c-211">También es accesible desde una máquina remota, salvo que haya algún firewall que la pueda estar bloqueando.</span><span class="sxs-lookup"><span data-stu-id="4d78c-211">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="4d78c-212">Al inspeccionar los encabezados de respuesta, el encabezado `Server` muestra que Kestrel suministra la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d78c-212">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="4d78c-213">Visualización de registros</span><span class="sxs-lookup"><span data-stu-id="4d78c-213">View logs</span></span>

<span data-ttu-id="4d78c-214">Dado que la aplicación web que usa Kestrel se administra mediante `systemd`, todos los procesos y eventos se registran en un diario centralizado.</span><span class="sxs-lookup"><span data-stu-id="4d78c-214">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="4d78c-215">En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por `systemd`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-215">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="4d78c-216">Para ver los elementos específicos de `kestrel-helloapp.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4d78c-216">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="4d78c-217">Para obtener más opciones de filtrado, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.</span><span class="sxs-lookup"><span data-stu-id="4d78c-217">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="4d78c-218">Protección de datos</span><span class="sxs-lookup"><span data-stu-id="4d78c-218">Data protection</span></span>

<span data-ttu-id="4d78c-219">Hay varios [softwares intermedios](xref:fundamentals/middleware/index) de ASP.NET Core que utilizan la [pila de protección de datos de ASP.NET Core](xref:security/data-protection/introduction), incluidos los de autenticación, como el de cookies, y las protecciones de falsificación de solicitud entre sitios (CSRF).</span><span class="sxs-lookup"><span data-stu-id="4d78c-219">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="4d78c-220">Aunque el código de usuario no llame a las API de protección de datos, esta se debe configurar para crear un [almacén de claves](xref:security/data-protection/implementation/key-management) criptográficas persistente.</span><span class="sxs-lookup"><span data-stu-id="4d78c-220">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="4d78c-221">Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4d78c-221">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="4d78c-222">Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4d78c-222">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="4d78c-223">Todos los tokens de autenticación basados en cookies se invalidan.</span><span class="sxs-lookup"><span data-stu-id="4d78c-223">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="4d78c-224">Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud.</span><span class="sxs-lookup"><span data-stu-id="4d78c-224">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="4d78c-225">Ya no se pueden descifrar los datos protegidos con el conjunto de claves.</span><span class="sxs-lookup"><span data-stu-id="4d78c-225">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="4d78c-226">Esto puede incluir [tokens CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) y [cookies de TempData de ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="4d78c-226">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="4d78c-227">Para configurar la protección de datos de modo que sea persistente y permita cifrar el anillo de claves, consulte:</span><span class="sxs-lookup"><span data-stu-id="4d78c-227">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="4d78c-228">Campos del encabezado de solicitud más largos</span><span class="sxs-lookup"><span data-stu-id="4d78c-228">Long request header fields</span></span>

<span data-ttu-id="4d78c-229">Si la aplicación requiere campos de encabezado de solicitud más largos que los permitidos por la configuración predeterminada del servidor proxy (normalmente 4K u 8K, según la plataforma), será necesario ajustar las siguientes directivas.</span><span class="sxs-lookup"><span data-stu-id="4d78c-229">If the app requires request header fields longer than permitted by the proxy server's default settings (typically 4K or 8K depending on the platform), the following directives require adjustment.</span></span> <span data-ttu-id="4d78c-230">Los valores aplicables dependerán del escenario.</span><span class="sxs-lookup"><span data-stu-id="4d78c-230">The values to apply are scenario-dependent.</span></span> <span data-ttu-id="4d78c-231">Para obtener más información, consulte la documentación del servidor.</span><span class="sxs-lookup"><span data-stu-id="4d78c-231">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="4d78c-232">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="4d78c-232">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="4d78c-233">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="4d78c-233">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="4d78c-234">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="4d78c-234">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="4d78c-235">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="4d78c-235">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="4d78c-236">No aumente los valores predeterminados de los búferes de proxy a menos que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="4d78c-236">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="4d78c-237">El aumento de estos valores incrementa el riesgo de saturación del búfer (desbordamiento) y ataques por denegación de servicio (DoS) realizados por usuarios malintencionados.</span><span class="sxs-lookup"><span data-stu-id="4d78c-237">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="4d78c-238">Protección de la nube</span><span class="sxs-lookup"><span data-stu-id="4d78c-238">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="4d78c-239">Habilitar AppArmor</span><span class="sxs-lookup"><span data-stu-id="4d78c-239">Enable AppArmor</span></span>

<span data-ttu-id="4d78c-240">Linux Security Modules (LSM) es una plataforma que forma parte del kernel de Linux desde Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="4d78c-240">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="4d78c-241">LSM admite diferentes implementaciones de los módulos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="4d78c-241">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="4d78c-242">[AppArmor](https://wiki.ubuntu.com/AppArmor) es un LSM que implementa un sistema de control de acceso obligatorio que permite restringir el programa a un conjunto limitado de recursos.</span><span class="sxs-lookup"><span data-stu-id="4d78c-242">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="4d78c-243">Asegúrese de que AppArmor está habilitado y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="4d78c-243">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="4d78c-244">Configuración del firewall</span><span class="sxs-lookup"><span data-stu-id="4d78c-244">Configure the firewall</span></span>

<span data-ttu-id="4d78c-245">Cierre todos los puertos externos que no estén en uso.</span><span class="sxs-lookup"><span data-stu-id="4d78c-245">Close off all external ports that are not in use.</span></span> <span data-ttu-id="4d78c-246">Uncomplicated Firewall (ufw) proporciona un front-end para `iptables` al proporcionar una interfaz de línea de comandos para configurar el firewall.</span><span class="sxs-lookup"><span data-stu-id="4d78c-246">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="4d78c-247">Si tiene algún firewall activado y no está configurado correctamente, este impedirá el acceso a todo el sistema.</span><span class="sxs-lookup"><span data-stu-id="4d78c-247">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="4d78c-248">Si usa SSH para establecer la conexión y no especifica el puerto SSH correcto, quedará bloqueado y no podrá acceder al sistema.</span><span class="sxs-lookup"><span data-stu-id="4d78c-248">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="4d78c-249">El puerto predeterminado es el 22.</span><span class="sxs-lookup"><span data-stu-id="4d78c-249">The default port is 22.</span></span> <span data-ttu-id="4d78c-250">Para obtener más información, consulte la [introducción a UFW](https://help.ubuntu.com/community/UFW) y el [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="4d78c-250">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="4d78c-251">Instale `ufw` y configúrelo para permitir el tráfico en los puertos que convenga.</span><span class="sxs-lookup"><span data-stu-id="4d78c-251">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="4d78c-252">Protección de Nginx</span><span class="sxs-lookup"><span data-stu-id="4d78c-252">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="4d78c-253">Cambiar el nombre de la respuesta de Nginx</span><span class="sxs-lookup"><span data-stu-id="4d78c-253">Change the Nginx response name</span></span>

<span data-ttu-id="4d78c-254">Edite *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="4d78c-254">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="4d78c-255">Configurar opciones</span><span class="sxs-lookup"><span data-stu-id="4d78c-255">Configure options</span></span>

<span data-ttu-id="4d78c-256">Configure el servidor con más módulos que sean necesarios.</span><span class="sxs-lookup"><span data-stu-id="4d78c-256">Configure the server with additional required modules.</span></span> <span data-ttu-id="4d78c-257">Sopese la posibilidad de usar un firewall de aplicación web como [ModSecurity](https://www.modsecurity.org/) para proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4d78c-257">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="4d78c-258">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="4d78c-258">HTTPS configuration</span></span>

* <span data-ttu-id="4d78c-259">Configure el servidor para que escuche el tráfico HTTPS en el puerto `443`. Para ello, especifique un certificado válido emitido por una entidad de certificados (CA) de confianza.</span><span class="sxs-lookup"><span data-stu-id="4d78c-259">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="4d78c-260">Refuerce la seguridad con algunos de los procedimientos descritos en el siguiente archivo */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="4d78c-260">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="4d78c-261">Entre los ejemplos se incluye la elección de un cifrado más seguro y el redireccionamiento de todo el tráfico a través de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4d78c-261">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="4d78c-262">Agregar un encabezado `HTTP Strict-Transport-Security` (HSTS) garantiza que todas las solicitudes siguientes realizadas por el cliente son a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4d78c-262">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="4d78c-263">Si se va a deshabilitar HTTPS en el futuro, no agregue el encabezado HSTS, o bien elija un valor `max-age` adecuado.</span><span class="sxs-lookup"><span data-stu-id="4d78c-263">Don't add the HSTS header or chose an appropriate `max-age` if HTTPS will be disabled in the future.</span></span>

<span data-ttu-id="4d78c-264">Agregue el archivo de configuración */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="4d78c-264">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="4d78c-265">Edite el archivo de configuración */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="4d78c-265">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="4d78c-266">El ejemplo contiene las dos secciones `http` y `server` en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="4d78c-266">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="4d78c-267">Proteger Nginx frente al secuestro de clic</span><span class="sxs-lookup"><span data-stu-id="4d78c-267">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="4d78c-268">El [secuestro de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocido como *redireccionamiento de interfaz de usuario*, es un ataque malintencionado donde al visitante de un sitio web se le engaña para que haga clic en un vínculo o botón de una página distinta de la que actualmente está visitando.</span><span class="sxs-lookup"><span data-stu-id="4d78c-268">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="4d78c-269">Use `X-FRAME-OPTIONS` para proteger el sitio.</span><span class="sxs-lookup"><span data-stu-id="4d78c-269">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="4d78c-270">Para mitigar los ataques de secuestro de clic:</span><span class="sxs-lookup"><span data-stu-id="4d78c-270">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="4d78c-271">Edite el archivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="4d78c-271">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="4d78c-272">Agregue la línea `add_header X-Frame-Options "SAMEORIGIN";`.</span><span class="sxs-lookup"><span data-stu-id="4d78c-272">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="4d78c-273">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="4d78c-273">Save the file.</span></span>
1. <span data-ttu-id="4d78c-274">Reinicie Nginx.</span><span class="sxs-lookup"><span data-stu-id="4d78c-274">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="4d78c-275">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="4d78c-275">MIME-type sniffing</span></span>

<span data-ttu-id="4d78c-276">Este encabezado evita que la mayoría de los exploradores examinen el MIME de una respuesta fuera del tipo de contenido declarado, ya que el encabezado indica al explorador que no reemplace el tipo de contenido de respuesta.</span><span class="sxs-lookup"><span data-stu-id="4d78c-276">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="4d78c-277">Con la opción `nosniff`, si el servidor indica que el contenido es "text/html", el explorador lo representa como "text/html".</span><span class="sxs-lookup"><span data-stu-id="4d78c-277">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="4d78c-278">Edite el archivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="4d78c-278">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="4d78c-279">Agregue la línea `add_header X-Content-Type-Options "nosniff";`, guarde el archivo y después reinicie Nginx.</span><span class="sxs-lookup"><span data-stu-id="4d78c-279">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d78c-280">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4d78c-280">Additional resources</span></span>

* [<span data-ttu-id="4d78c-281">Requisitos previos para .NET Core en Linux</span><span class="sxs-lookup"><span data-stu-id="4d78c-281">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <span data-ttu-id="4d78c-282">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx: versiones de código binario: paquetes oficiales de Debian y Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="4d78c-282">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* <span data-ttu-id="4d78c-283">[Nginx: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (Nginx: uso del encabezado Forwarded)</span><span class="sxs-lookup"><span data-stu-id="4d78c-283">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)</span></span>
