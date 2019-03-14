---
title: Hospedar ASP.NET Core en Linux con Apache
description: Aprenda a configurar Apache como servidor proxy inverso en CentOS para redirigir el tráfico HTTP a una aplicación web ASP.NET Core que se ejecuta en Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 0dec9c657134bba3224a1fbb69aaefaaba753404
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57026492"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="e1c21-103">Hospedar ASP.NET Core en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="e1c21-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="e1c21-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="e1c21-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="e1c21-105">Mediante esta guía, aprenda a configurar [Apache](https://httpd.apache.org/) como servidor proxy inverso en [CentOS 7](https://www.centos.org/) para redirigir el tráfico HTTP a una aplicación web ASP.NET Core que se ejecuta en el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e1c21-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="e1c21-106">La [extensión mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) y los módulos relacionados crean el proxy inverso del servidor.</span><span class="sxs-lookup"><span data-stu-id="e1c21-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1c21-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e1c21-107">Prerequisites</span></span>

* <span data-ttu-id="e1c21-108">Servidor que ejecute CentOS 7, con una cuenta de usuario estándar con privilegios sudo.</span><span class="sxs-lookup"><span data-stu-id="e1c21-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="e1c21-109">Tener .NET Core Runtime instalado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e1c21-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="e1c21-110">Vaya a la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="e1c21-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="e1c21-111">Seleccione el tiempo de ejecución más reciente que no sea versión preliminar en la lista **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="e1c21-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="e1c21-112">Seleccione y siga las instrucciones relativas a CentOS/Oracle.</span><span class="sxs-lookup"><span data-stu-id="e1c21-112">Select and follow the instructions for CentOS/Oracle.</span></span>
* <span data-ttu-id="e1c21-113">Disponer de una aplicación de ASP.NET Core existente.</span><span class="sxs-lookup"><span data-stu-id="e1c21-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="e1c21-114">Publicar y copiar en la aplicación</span><span class="sxs-lookup"><span data-stu-id="e1c21-114">Publish and copy over the app</span></span>

<span data-ttu-id="e1c21-115">Configure la aplicación para una [implementación dependiente de Framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="e1c21-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="e1c21-116">Ejecute [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el entorno de desarrollo para empaquetar una aplicación en un directorio (por ejemplo, *bin/Release/&lt;target_framework_moniker&gt;/publish*) que se pueda ejecutar en el servidor:</span><span class="sxs-lookup"><span data-stu-id="e1c21-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="e1c21-117">La aplicación también se puede publicar como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) si prefiere no mantener .NET Core Runtime en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e1c21-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="e1c21-118">Copie la aplicación de ASP.NET Core en el servidor usando una herramienta que se integre en el flujo de trabajo de la organización (como SCP o SFTP).</span><span class="sxs-lookup"><span data-stu-id="e1c21-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="e1c21-119">Es habitual encontrar las aplicaciones web en el directorio *var* (por ejemplo, *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="e1c21-119">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="e1c21-120">En un escenario de implementación de producción, un flujo de trabajo de integración continua lleva a cabo la tarea de publicar la aplicación y copiar los recursos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e1c21-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="e1c21-121">Configurar un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="e1c21-121">Configure a proxy server</span></span>

<span data-ttu-id="e1c21-122">Un proxy inverso es una configuración común para trabajar con aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="e1c21-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="e1c21-123">El proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e1c21-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="e1c21-124">Un servidor proxy es el que reenvía las solicitudes de cliente a otro servidor en lugar de realizarlas él mismo.</span><span class="sxs-lookup"><span data-stu-id="e1c21-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="e1c21-125">Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="e1c21-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="e1c21-126">En esta guía, Apache se configura como proxy inverso que se ejecuta en el mismo servidor en el que Kestrel atiende la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1c21-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="e1c21-127">Como el proxy inverso reenvía las solicitudes, use el [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) del paquete [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="e1c21-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="e1c21-128">El middleware actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que los URI de redireccionamiento y otras directivas de seguridad funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="e1c21-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="e1c21-129">Cualquier componente que dependa del esquema (como la autenticación, la generación de vínculos, los redireccionamientos o la geolocalización) debe colocarse después de invocar al Middleware de encabezados reenviados.</span><span class="sxs-lookup"><span data-stu-id="e1c21-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="e1c21-130">Como norma general, el Middleware de encabezados reenviados se debe ejecutar antes de cualquier otro middleware, salvo el middleware de diagnóstico y control de errores.</span><span class="sxs-lookup"><span data-stu-id="e1c21-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="e1c21-131">Hacerlo en ese orden garantiza que el middleware que se basa en la información de encabezados reenviados pueda usar los valores de encabezado para procesarlos.</span><span class="sxs-lookup"><span data-stu-id="e1c21-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e1c21-132">Invoque el método <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> en `Startup.Configure` antes de llamar a <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> o un middleware de esquema de autenticación similar.</span><span class="sxs-lookup"><span data-stu-id="e1c21-132">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="e1c21-133">Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="e1c21-133">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e1c21-134">Invoque el método <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> en `Startup.Configure` antes de llamar a <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> y <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> o un middleware de esquema de autenticación similar.</span><span class="sxs-lookup"><span data-stu-id="e1c21-134">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> and <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="e1c21-135">Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="e1c21-135">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="e1c21-136">Si no se especifica ningún valor <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> para el middleware, los encabezados predeterminados para reenviar son `None`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-136">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="e1c21-137">Solo los servidores proxy que se ejecutan en localhost (127.0.0.1, [::1]) son de confianza de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e1c21-137">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="e1c21-138">Si otras redes o servidores proxy de confianza de la organización tramitan solicitudes entre Internet y el servidor web, agréguelos a la lista de <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> o <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> con <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="e1c21-138">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="e1c21-139">En el ejemplo siguiente se agrega un servidor proxy de confianza en la dirección IP 10.0.0.100 al middleware de encabezados reenviados `KnownProxies` en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e1c21-139">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="e1c21-140">Para obtener más información, consulta <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="e1c21-140">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="e1c21-141">Instalar Apache</span><span class="sxs-lookup"><span data-stu-id="e1c21-141">Install Apache</span></span>

<span data-ttu-id="e1c21-142">Actualice los paquetes de CentOS a sus versiones estables más recientes:</span><span class="sxs-lookup"><span data-stu-id="e1c21-142">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="e1c21-143">Instale el servidor web de Apache en CentOS con un único comando `yum`:</span><span class="sxs-lookup"><span data-stu-id="e1c21-143">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="e1c21-144">Salida de ejemplo después de ejecutar el comando:</span><span class="sxs-lookup"><span data-stu-id="e1c21-144">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="e1c21-145">En este ejemplo, la salida refleja httpd.86_64, puesto que la versión de CentOS 7 es de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="e1c21-145">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="e1c21-146">Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="e1c21-146">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="e1c21-147">Configurar Apache</span><span class="sxs-lookup"><span data-stu-id="e1c21-147">Configure Apache</span></span>

<span data-ttu-id="e1c21-148">Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-148">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="e1c21-149">Todos los archivos que tengan la extensión *.conf* se procesan en orden alfabético, además de los archivos de configuración del módulo de `/etc/httpd/conf.modules.d/`, que contiene todos los archivos de configuración necesarios para cargar los módulos.</span><span class="sxs-lookup"><span data-stu-id="e1c21-149">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="e1c21-150">Cree un archivo de configuración denominado *helloapp.conf* para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e1c21-150">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="e1c21-151">El bloque `VirtualHost` puede aparecer varias veces en uno o varios archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="e1c21-151">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="e1c21-152">En el archivo de configuración anterior, Apache acepta tráfico público en el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="e1c21-152">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="e1c21-153">El dominio `www.example.com` se atiende y el alias `*.example.com` se resuelve en el mismo sitio web.</span><span class="sxs-lookup"><span data-stu-id="e1c21-153">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="e1c21-154">Para más información, consulte [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Compatibilidad con el host virtual basado en nombres).</span><span class="sxs-lookup"><span data-stu-id="e1c21-154">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="e1c21-155">Las solicitudes se redirigen mediante proxy en la raíz al puerto 5000 del servidor en 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="e1c21-155">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="e1c21-156">Para la comunicación bidireccional, se requieren `ProxyPass` y `ProxyPassReverse`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-156">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="e1c21-157">Para cambiar la IP o el puerto de Kestrel, vea [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (Kestrel: configuración de los puntos de conexión).</span><span class="sxs-lookup"><span data-stu-id="e1c21-157">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="e1c21-158">Si no se especifica una [directiva de ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) correcta en **VirtualHost**, el bloque expone la aplicación a las vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="e1c21-158">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="e1c21-159">Los enlaces de carácter comodín de subdominio (por ejemplo, `*.example.com`) no presentan este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable).</span><span class="sxs-lookup"><span data-stu-id="e1c21-159">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="e1c21-160">Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="e1c21-160">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="e1c21-161">El registro se puede configurar por `VirtualHost` con las directivas `ErrorLog` y `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-161">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="e1c21-162">`ErrorLog` es la ubicación donde el servidor registra los errores, y `CustomLog` establece el nombre de archivo y el formato del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="e1c21-162">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="e1c21-163">En este caso, aquí es donde se registra la información de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e1c21-163">In this case, this is where request information is logged.</span></span> <span data-ttu-id="e1c21-164">Hay una línea para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="e1c21-164">There's one line for each request.</span></span>

<span data-ttu-id="e1c21-165">Guarde el archivo y pruebe la configuración.</span><span class="sxs-lookup"><span data-stu-id="e1c21-165">Save the file and test the configuration.</span></span> <span data-ttu-id="e1c21-166">Si se pasa todo, la respuesta debe ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-166">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="e1c21-167">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="e1c21-167">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="e1c21-168">Supervisión de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e1c21-168">Monitor the app</span></span>

<span data-ttu-id="e1c21-169">Apache está configurado ahora para reenviar las solicitudes efectuadas a `http://localhost:80` en la aplicación ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-169">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="e1c21-170">En cambio, Apache no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e1c21-170">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="e1c21-171">Use *systemd* y cree un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="e1c21-171">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="e1c21-172">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="e1c21-172">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="e1c21-173">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="e1c21-173">Create the service file</span></span>

<span data-ttu-id="e1c21-174">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="e1c21-174">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="e1c21-175">Un archivo de servicio de ejemplo para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e1c21-175">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="e1c21-176">Si el usuario *apache* no se usa en la configuración, primero se debe crear el usuario y se le debe conceder la propiedad adecuada de los archivos.</span><span class="sxs-lookup"><span data-stu-id="e1c21-176">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="e1c21-177">Use `TimeoutStopSec` para configurar el período de tiempo que hay que esperar para que la aplicación se apague después de que reciba la señal de interrupción inicial.</span><span class="sxs-lookup"><span data-stu-id="e1c21-177">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="e1c21-178">Si la aplicación no se apaga en este período, se emite SIGKILL para terminar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e1c21-178">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="e1c21-179">Proporcione el valor como segundos sin unidad (por ejemplo, `150`), un intervalo de tiempo (por ejemplo, `2min 30s`) o `infinity` para deshabilitar el tiempo de expiración.</span><span class="sxs-lookup"><span data-stu-id="e1c21-179">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="e1c21-180">El valor predeterminado de `TimeoutStopSec` es el valor de `DefaultTimeoutStopSec` del archivo de configuración del administrador (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf* o *user.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="e1c21-180">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="e1c21-181">El tiempo de expiración predeterminado para la mayoría de las distribuciones es de 90 segundos.</span><span class="sxs-lookup"><span data-stu-id="e1c21-181">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="e1c21-182">Algunos valores (por ejemplo, cadenas de conexión de SQL) deben ser de escape para que los proveedores de configuración lean las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="e1c21-182">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="e1c21-183">Use el siguiente comando para generar un valor de escape correctamente para su uso en el archivo de configuración:</span><span class="sxs-lookup"><span data-stu-id="e1c21-183">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="e1c21-184">Guarde el archivo y habilite el servicio.</span><span class="sxs-lookup"><span data-stu-id="e1c21-184">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="e1c21-185">Inicie el servicio y compruebe que se está ejecutando:</span><span class="sxs-lookup"><span data-stu-id="e1c21-185">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="e1c21-186">Con el proxy inverso configurado y Kestrel administrado mediante *systemd*, la aplicación web está completamente configurada y se puede acceder a ella desde un explorador en la máquina local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-186">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="e1c21-187">Inspeccionar los encabezados de respuesta; el encabezado **Server** indica que Kestrel atiende la aplicación ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e1c21-187">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="e1c21-188">Visualización de registros</span><span class="sxs-lookup"><span data-stu-id="e1c21-188">View logs</span></span>

<span data-ttu-id="e1c21-189">Dado que la aplicación web que usa Kestrel se administra mediante *systemd*, los procesos y eventos se registran en un diario centralizado.</span><span class="sxs-lookup"><span data-stu-id="e1c21-189">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="e1c21-190">Sin embargo, este diario incluye todas las entradas de todos los servicios y procesos administrados por *systemd*.</span><span class="sxs-lookup"><span data-stu-id="e1c21-190">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="e1c21-191">Para ver los elementos específicos de `kestrel-helloapp.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e1c21-191">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="e1c21-192">Para el filtrado de tiempo, especifique las opciones de tiempo con el comando.</span><span class="sxs-lookup"><span data-stu-id="e1c21-192">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="e1c21-193">Por ejemplo, use `--since today` para filtrar por el día actual o `--until 1 hour ago` para ver las entradas de la hora anterior.</span><span class="sxs-lookup"><span data-stu-id="e1c21-193">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="e1c21-194">Para más información, consulte la [página man de journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="e1c21-194">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="e1c21-195">Protección de datos</span><span class="sxs-lookup"><span data-stu-id="e1c21-195">Data protection</span></span>

<span data-ttu-id="e1c21-196">Hay varios [softwares intermedios](xref:fundamentals/middleware/index) de ASP.NET Core que utilizan la [pila de protección de datos de ASP.NET Core](xref:security/data-protection/introduction), incluidos los de autenticación, como el de cookies, y las protecciones de falsificación de solicitud entre sitios (CSRF).</span><span class="sxs-lookup"><span data-stu-id="e1c21-196">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="e1c21-197">Aunque el código de usuario no llame a las API de protección de datos, esta se debe configurar para crear un [almacén de claves](xref:security/data-protection/implementation/key-management) criptográficas persistente.</span><span class="sxs-lookup"><span data-stu-id="e1c21-197">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="e1c21-198">Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e1c21-198">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="e1c21-199">Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e1c21-199">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="e1c21-200">Todos los tokens de autenticación basados en cookies se invalidan.</span><span class="sxs-lookup"><span data-stu-id="e1c21-200">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="e1c21-201">Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud.</span><span class="sxs-lookup"><span data-stu-id="e1c21-201">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="e1c21-202">Ya no se pueden descifrar los datos protegidos con el conjunto de claves.</span><span class="sxs-lookup"><span data-stu-id="e1c21-202">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="e1c21-203">Esto puede incluir [tokens CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) y [cookies de TempData de ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="e1c21-203">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="e1c21-204">Para configurar la protección de datos de modo que sea persistente y permita cifrar el anillo de claves, consulte:</span><span class="sxs-lookup"><span data-stu-id="e1c21-204">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="e1c21-205">Protección de la nube</span><span class="sxs-lookup"><span data-stu-id="e1c21-205">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="e1c21-206">Configurar el firewall</span><span class="sxs-lookup"><span data-stu-id="e1c21-206">Configure firewall</span></span>

<span data-ttu-id="e1c21-207">*Firewalld* es un demonio dinámico para administrar el firewall con compatibilidad con zonas de red.</span><span class="sxs-lookup"><span data-stu-id="e1c21-207">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="e1c21-208">Los puertos y el filtrado de paquetes se pueden seguir administrando mediante iptables.</span><span class="sxs-lookup"><span data-stu-id="e1c21-208">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="e1c21-209">*Firewalld* debe instalarse de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e1c21-209">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="e1c21-210">`yum` puede usarse para instalar el paquete o comprobar que está instalado.</span><span class="sxs-lookup"><span data-stu-id="e1c21-210">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="e1c21-211">Use `firewalld` para abrir solo los puertos necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e1c21-211">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="e1c21-212">En este caso se usan los puertos 80 y 443.</span><span class="sxs-lookup"><span data-stu-id="e1c21-212">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="e1c21-213">Los siguientes comandos establecen de forma permanente que se abran los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="e1c21-213">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="e1c21-214">Vuelva a cargar la configuración del firewall.</span><span class="sxs-lookup"><span data-stu-id="e1c21-214">Reload the firewall settings.</span></span> <span data-ttu-id="e1c21-215">Compruebe los servicios y puertos disponibles en la zona predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e1c21-215">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="e1c21-216">Hay opciones disponibles si se inspecciona `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-216">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="https-configuration"></a><span data-ttu-id="e1c21-217">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="e1c21-217">HTTPS configuration</span></span>

<span data-ttu-id="e1c21-218">Para configurar Apache para HTTPS, se usa el módulo *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="e1c21-218">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="e1c21-219">Cuando se instaló el módulo *httpd*, también lo hizo el módulo *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="e1c21-219">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="e1c21-220">Si aún no se ha instalado, use `yum` para agregarlo a la configuración.</span><span class="sxs-lookup"><span data-stu-id="e1c21-220">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="e1c21-221">Para usar HTTPS, instale el módulo `mod_rewrite` para habilitar la reescritura de direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="e1c21-221">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="e1c21-222">Modifique el archivo *helloapp.conf* para permitir la reescritura de direcciones URL y proteger la comunicación en el puerto 443:</span><span class="sxs-lookup"><span data-stu-id="e1c21-222">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="e1c21-223">En este ejemplo se usa un certificado generado localmente.</span><span class="sxs-lookup"><span data-stu-id="e1c21-223">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="e1c21-224">**SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio.</span><span class="sxs-lookup"><span data-stu-id="e1c21-224">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="e1c21-225">**SSLCertificateKeyFile** debe ser el archivo de claves generado al crear el CSR.</span><span class="sxs-lookup"><span data-stu-id="e1c21-225">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="e1c21-226">**SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) proporcionado por la entidad de certificación.</span><span class="sxs-lookup"><span data-stu-id="e1c21-226">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="e1c21-227">Guarde el archivo y pruebe la configuración.</span><span class="sxs-lookup"><span data-stu-id="e1c21-227">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="e1c21-228">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="e1c21-228">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="e1c21-229">Sugerencias adicionales de Apache</span><span class="sxs-lookup"><span data-stu-id="e1c21-229">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="e1c21-230">Encabezados adicionales</span><span class="sxs-lookup"><span data-stu-id="e1c21-230">Additional headers</span></span>

<span data-ttu-id="e1c21-231">Para protegerse frente a ataques malintencionados, hay unos encabezados que se deben modificar o agregar.</span><span class="sxs-lookup"><span data-stu-id="e1c21-231">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="e1c21-232">Asegúrese de que el módulo `mod_headers` está instalado.</span><span class="sxs-lookup"><span data-stu-id="e1c21-232">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="e1c21-233">Protección de Apache de los ataques de secuestro de clic</span><span class="sxs-lookup"><span data-stu-id="e1c21-233">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="e1c21-234">El [secuestro de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocido como *redireccionamiento de interfaz de usuario*, es un ataque malintencionado donde al visitante de un sitio web se le engaña para que haga clic en un vínculo o botón de una página distinta de la que actualmente está visitando.</span><span class="sxs-lookup"><span data-stu-id="e1c21-234">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="e1c21-235">Use `X-FRAME-OPTIONS` para proteger el sitio.</span><span class="sxs-lookup"><span data-stu-id="e1c21-235">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="e1c21-236">Para mitigar los ataques de secuestro de clic:</span><span class="sxs-lookup"><span data-stu-id="e1c21-236">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="e1c21-237">Edite el archivo *httpd.conf*.</span><span class="sxs-lookup"><span data-stu-id="e1c21-237">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="e1c21-238">Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-238">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="e1c21-239">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="e1c21-239">Save the file.</span></span>
1. <span data-ttu-id="e1c21-240">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="e1c21-240">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="e1c21-241">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="e1c21-241">MIME-type sniffing</span></span>

<span data-ttu-id="e1c21-242">El encabezado `X-Content-Type-Options` impide que Internet Explorer *examine MIME* (de forma que el elemento `Content-Type` de un archivo se determina a partir del contenido del archivo).</span><span class="sxs-lookup"><span data-stu-id="e1c21-242">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="e1c21-243">Si el servidor establece el encabezado `Content-Type` en `text/html` con la opción `nosniff` establecida, Internet Explorer representa el contenido como `text/html` sin tener en cuenta el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="e1c21-243">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="e1c21-244">Edite el archivo *httpd.conf*.</span><span class="sxs-lookup"><span data-stu-id="e1c21-244">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="e1c21-245">Agregue la línea `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="e1c21-245">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="e1c21-246">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="e1c21-246">Save the file.</span></span> <span data-ttu-id="e1c21-247">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="e1c21-247">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="e1c21-248">Equilibrio de carga</span><span class="sxs-lookup"><span data-stu-id="e1c21-248">Load Balancing</span></span>

<span data-ttu-id="e1c21-249">En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia.</span><span class="sxs-lookup"><span data-stu-id="e1c21-249">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="e1c21-250">Para no tener un único punto de error, el uso de *mod_proxy_balancer* y la modificación de **VirtualHost** permitirían administrar varias instancias de las aplicaciones web detrás del servidor proxy de Apache.</span><span class="sxs-lookup"><span data-stu-id="e1c21-250">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="e1c21-251">En el archivo de configuración que se muestra a continuación, se configura una instancia adicional de `helloapp` para que se ejecute en el puerto 5001.</span><span class="sxs-lookup"><span data-stu-id="e1c21-251">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="e1c21-252">La sección *Proxy* se establece con una configuración de equilibrador con dos miembros para equilibrar la carga *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="e1c21-252">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="e1c21-253">Límites de velocidad</span><span class="sxs-lookup"><span data-stu-id="e1c21-253">Rate Limits</span></span>

<span data-ttu-id="e1c21-254">Use *mod_ratelimit*, que se incluye en el módulo *httpd*; el ancho de banda de los clientes puede ser limitado:</span><span class="sxs-lookup"><span data-stu-id="e1c21-254">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="e1c21-255">En el archivo de ejemplo se limita el ancho de banda a 600 KB/s en la ubicación raíz:</span><span class="sxs-lookup"><span data-stu-id="e1c21-255">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="e1c21-256">Campos del encabezado de solicitud más largos</span><span class="sxs-lookup"><span data-stu-id="e1c21-256">Long request header fields</span></span>

<span data-ttu-id="e1c21-257">Si la aplicación requiere campos de encabezado de solicitud más largos que los permitidos por la configuración predeterminada del servidor proxy (normalmente 8190 bytes), ajuste el valor de la directiva [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize).</span><span class="sxs-lookup"><span data-stu-id="e1c21-257">If the app requires request header fields longer than permitted by the proxy server's default setting (typically 8,190 bytes), adjust the value of the [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive.</span></span> <span data-ttu-id="e1c21-258">El valor aplicable dependerá del escenario.</span><span class="sxs-lookup"><span data-stu-id="e1c21-258">The value to apply is scenario-dependent.</span></span> <span data-ttu-id="e1c21-259">Para obtener más información, consulte la documentación del servidor.</span><span class="sxs-lookup"><span data-stu-id="e1c21-259">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="e1c21-260">No aumente el valor predeterminado de `LimitRequestFieldSize` a menos que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="e1c21-260">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="e1c21-261">El aumento de este valor incrementa el riesgo de saturación del búfer (desbordamiento) y ataques por denegación de servicio (DoS) realizados por usuarios malintencionados.</span><span class="sxs-lookup"><span data-stu-id="e1c21-261">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1c21-262">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e1c21-262">Additional resources</span></span>

* [<span data-ttu-id="e1c21-263">Requisitos previos para .NET Core en Linux</span><span class="sxs-lookup"><span data-stu-id="e1c21-263">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
