---
title: Almacenamiento en caché en ASP.NET Core distribuido
author: guardrex
description: Obtenga información sobre cómo usar una caché distribuida de ASP.NET Core para mejorar el rendimiento de la aplicación y la escalabilidad, especialmente en un entorno de granja de servidores en la nube o servidor.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: performance/caching/distributed
ms.openlocfilehash: 7337ee3b823064c942832d8a44e4d4289bc4fd0e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052412"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="60e23-103">Almacenamiento en caché en ASP.NET Core distribuido</span><span class="sxs-lookup"><span data-stu-id="60e23-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="60e23-104">Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60e23-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="60e23-105">Una caché distribuida es una caché compartida por varios servidores de aplicación, que normalmente se mantiene como un servicio externo a los servidores de aplicaciones que acceden a ellos.</span><span class="sxs-lookup"><span data-stu-id="60e23-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="60e23-106">Una memoria caché distribuida puede mejorar el rendimiento y escalabilidad de una aplicación ASP.NET Core, especialmente cuando la aplicación está hospedada en un servicio en la nube o una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="60e23-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="60e23-107">Una memoria caché distribuida tiene varias ventajas sobre otros escenarios de almacenamiento en caché se almacenan en caché datos en los servidores de aplicaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="60e23-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="60e23-108">Cuando se distribuyen los datos almacenados en caché, los datos:</span><span class="sxs-lookup"><span data-stu-id="60e23-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="60e23-109">Es *coherente* (coherentes) a través de solicitudes en varios servidores.</span><span class="sxs-lookup"><span data-stu-id="60e23-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="60e23-110">Sobrevive a los reinicios del servidor y las implementaciones de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="60e23-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="60e23-111">No utilice memoria local.</span><span class="sxs-lookup"><span data-stu-id="60e23-111">Doesn't use local memory.</span></span>

<span data-ttu-id="60e23-112">Configuración de caché distribuida es específico de la implementación.</span><span class="sxs-lookup"><span data-stu-id="60e23-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="60e23-113">En este artículo se describe cómo configurar SQL Server y las memorias caché distribuidas de Redis.</span><span class="sxs-lookup"><span data-stu-id="60e23-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="60e23-114">Las implementaciones de otros fabricantes también están disponibles, como [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache en GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="60e23-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="60e23-115">Independientemente de qué implementación está seleccionada, la aplicación interactúa con la caché mediante el <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaz.</span><span class="sxs-lookup"><span data-stu-id="60e23-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="60e23-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60e23-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60e23-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="60e23-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="60e23-118">Para usar un servidor SQL Server distributed cache, referencia del [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-118">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="60e23-119">Para usar un Redis caché, referencia distribuida el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) y agregue una referencia de paquete para el [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-119">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="60e23-120">El paquete de Redis no se incluye en el `Microsoft.AspNetCore.App` del paquete, por lo que debe hacer referencia el paquete de Redis por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="60e23-120">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="60e23-121">Para usar un servidor SQL Server distributed cache, referencia del [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="60e23-122">Para usar un Redis caché, referencia distribuida el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) y agregue una referencia de paquete para el [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="60e23-123">El paquete de Redis no se incluye en el `Microsoft.AspNetCore.App` del paquete, por lo que debe hacer referencia el paquete de Redis por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="60e23-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="60e23-124">Para usar un servidor SQL Server distributed cache, referencia del [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-124">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="60e23-125">Para usar un Redis caché, referencia distribuida el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-125">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="60e23-126">El paquete de Redis se incluye en `Microsoft.AspNetCore.All` del paquete, por lo que no es necesario hacer referencia al paquete de Redis por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="60e23-126">The Redis package is included in `Microsoft.AspNetCore.All` package, so you don't need to reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="60e23-127">Para utilizar un servidor SQL de caché distribuida, agregue una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-127">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="60e23-128">Para usar un Redis caché distribuida, agregue una referencia de paquete para el [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paquete.</span><span class="sxs-lookup"><span data-stu-id="60e23-128">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="60e23-129">Interfaz IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="60e23-129">IDistributedCache interface</span></span>

<span data-ttu-id="60e23-130">El <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaz proporciona los siguientes métodos para manipular los elementos en la implementación de caché distribuida:</span><span class="sxs-lookup"><span data-stu-id="60e23-130">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="60e23-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Acepta una clave de cadena y recupera un elemento almacenado en caché como un `byte[]` matriz si se encuentra en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="60e23-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="60e23-132"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Agrega un elemento (como `byte[]` matriz) a la memoria caché utilizando una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="60e23-132"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="60e23-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Actualiza un elemento en la memoria caché en función de su clave, restablecer el tiempo de espera de expiración deslizante (si existe).</span><span class="sxs-lookup"><span data-stu-id="60e23-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="60e23-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Quita un elemento de caché en función de su clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="60e23-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="60e23-135">Establezca los servicios de almacenamiento en caché distribuidos</span><span class="sxs-lookup"><span data-stu-id="60e23-135">Establish distributed caching services</span></span>

<span data-ttu-id="60e23-136">Registrar una implementación de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60e23-136">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="60e23-137">Las implementaciones de proporcionados por .NET Framework que se describe en este tema se incluyen:</span><span class="sxs-lookup"><span data-stu-id="60e23-137">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="60e23-138">Caché distribuida de memoria</span><span class="sxs-lookup"><span data-stu-id="60e23-138">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="60e23-139">Caché distribuida de SQL Server</span><span class="sxs-lookup"><span data-stu-id="60e23-139">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="60e23-140">Caché distribuida de Redis</span><span class="sxs-lookup"><span data-stu-id="60e23-140">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="60e23-141">Caché distribuida de memoria</span><span class="sxs-lookup"><span data-stu-id="60e23-141">Distributed Memory Cache</span></span>

<span data-ttu-id="60e23-142">La caché de memoria distribuida (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) es una implementación proporcionados por el marco de trabajo de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> que almacena los elementos en la memoria.</span><span class="sxs-lookup"><span data-stu-id="60e23-142">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="60e23-143">La memoria caché distribuida no es una caché distribuida real.</span><span class="sxs-lookup"><span data-stu-id="60e23-143">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="60e23-144">Los elementos en caché se almacenan por la instancia de aplicación en el servidor donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="60e23-144">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="60e23-145">La caché de memoria distribuido es una implementación útil:</span><span class="sxs-lookup"><span data-stu-id="60e23-145">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="60e23-146">En escenarios de prueba y desarrollo.</span><span class="sxs-lookup"><span data-stu-id="60e23-146">In development and testing scenarios.</span></span>
* <span data-ttu-id="60e23-147">Cuando se usa un único servidor de producción y consumo de memoria no supone un problema.</span><span class="sxs-lookup"><span data-stu-id="60e23-147">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="60e23-148">Implementar los resúmenes de caché distribuida de memoria, almacenamiento de datos en caché.</span><span class="sxs-lookup"><span data-stu-id="60e23-148">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="60e23-149">Permite implementar que un verdadero distribuir soluciones de almacenamiento en caché en el futuro cuando varios nodos o tolerancia a errores que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="60e23-149">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="60e23-150">La aplicación de ejemplo hace uso de la caché de memoria distribuida cuando la aplicación se ejecuta en el entorno de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="60e23-150">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="60e23-151">Caché distribuida de SQL Server</span><span class="sxs-lookup"><span data-stu-id="60e23-151">Distributed SQL Server Cache</span></span>

<span data-ttu-id="60e23-152">La implementación de caché distribuida de SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permite que la memoria caché distribuida usar una base de datos de SQL Server como almacén de respaldo.</span><span class="sxs-lookup"><span data-stu-id="60e23-152">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="60e23-153">Para crear una tabla de elemento almacenado en caché de SQL Server en una instancia de SQL Server, puede usar el `sql-cache` herramienta.</span><span class="sxs-lookup"><span data-stu-id="60e23-153">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="60e23-154">La herramienta crea una tabla con el nombre y el esquema que especifique.</span><span class="sxs-lookup"><span data-stu-id="60e23-154">The tool creates a table with the name and schema that you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="60e23-155">Agregar `SqlConfig.Tools` a la `<ItemGroup>` elemento del archivo de proyecto y ejecute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="60e23-155">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="60e23-156">Crear una tabla en SQL Server ejecutando el `sql-cache create` comando.</span><span class="sxs-lookup"><span data-stu-id="60e23-156">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="60e23-157">Proporcionar la instancia de SQL Server (`Data Source`), base de datos (`Initial Catalog`), esquema (por ejemplo, `dbo`) y el nombre de tabla (por ejemplo, `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="60e23-157">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="60e23-158">Se registra un mensaje para indicar que la herramienta se realizó correctamente:</span><span class="sxs-lookup"><span data-stu-id="60e23-158">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="60e23-159">La tabla creada por el `sql-cache` herramienta tiene el siguiente esquema:</span><span class="sxs-lookup"><span data-stu-id="60e23-159">The table created by the `sql-cache` tool has the following schema:</span></span>

![Tabla de la caché de SQL Server](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="60e23-161">Una aplicación debe manipular los valores de caché mediante una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, no un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="60e23-161">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="60e23-162">La aplicación de ejemplo implementa <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> en un entorno de desarrollo que no son:</span><span class="sxs-lookup"><span data-stu-id="60e23-162">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> <span data-ttu-id="60e23-163">Un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (y, opcionalmente, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> y <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) normalmente se almacenan fuera de control de código fuente (por ejemplo, almacenando el [Secret Manager](xref:security/app-secrets) o en *appsettings.json* / *appsettings. {Entorno} .json* archivos).</span><span class="sxs-lookup"><span data-stu-id="60e23-163">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{Environment}.json* files).</span></span> <span data-ttu-id="60e23-164">La cadena de conexión puede contener las credenciales que se deben mantener fuera de los sistemas de control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="60e23-164">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="60e23-165">Caché en Redis distribuida</span><span class="sxs-lookup"><span data-stu-id="60e23-165">Distributed Redis Cache</span></span>

<span data-ttu-id="60e23-166">[Redis](https://redis.io/) es un almacén de datos en memoria de código abierto, que a menudo se usa como una memoria caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="60e23-166">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="60e23-167">Puede usar Redis localmente, y puede configurar un [Azure Redis Cache](https://azure.microsoft.com/services/cache/) para una aplicación ASP.NET Core hospedadas en Azure.</span><span class="sxs-lookup"><span data-stu-id="60e23-167">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span> <span data-ttu-id="60e23-168">Una aplicación configura la implementación de caché mediante un <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instancia (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="60e23-168">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

<span data-ttu-id="60e23-169">Para instalar Redis en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="60e23-169">To install Redis on your local machine:</span></span>

* <span data-ttu-id="60e23-170">Instalar el [paquete Redis Chocolatey](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="60e23-170">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="60e23-171">Ejecute `redis-server` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="60e23-171">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="60e23-172">Usar la memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="60e23-172">Use the distributed cache</span></span>

<span data-ttu-id="60e23-173">Para usar el <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaz, solicitar una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> desde cualquier constructor en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="60e23-173">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="60e23-174">Proporciona la instancia [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="60e23-174">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="60e23-175">Cuando se inicia la aplicación, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> se inyecta en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="60e23-175">When the app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="60e23-176">La hora actual se almacena en caché mediante <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (para obtener más información, consulte [Host Web: Interfaz IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="60e23-176">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="60e23-177">La aplicación de ejemplo inyecta <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> en el `IndexModel` para su uso en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="60e23-177">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="60e23-178">Cada vez que se carga la página de índice, se comprueba la memoria caché para el tiempo en caché en `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="60e23-178">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="60e23-179">Si no ha expirado el tiempo en caché, se muestra el tiempo.</span><span class="sxs-lookup"><span data-stu-id="60e23-179">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="60e23-180">Si 20 segundos transcurridos desde la última vez que se tuvo acceso el tiempo en caché (la última vez que se cargó esta página), aparece la página *tiempo en caché expirado*.</span><span class="sxs-lookup"><span data-stu-id="60e23-180">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="60e23-181">Actualizar inmediatamente el tiempo en caché a la hora actual, seleccione el **Restablecer tiempo en caché** botón.</span><span class="sxs-lookup"><span data-stu-id="60e23-181">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="60e23-182">Los desencadenadores de botón el `OnPostResetCachedTime` método del controlador.</span><span class="sxs-lookup"><span data-stu-id="60e23-182">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="60e23-183">No hay ninguna necesidad de usar una duración Singleton o en el ámbito para <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instancias (al menos para las implementaciones integradas).</span><span class="sxs-lookup"><span data-stu-id="60e23-183">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="60e23-184">También puede crear un <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instancia donde es posible que necesite uno en lugar de usar la inserción de dependencias, pero crear una instancia en el código puede hacer que su código sea más difícil de probar e infringe el [dependencias explicitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="60e23-184">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="60e23-185">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="60e23-185">Recommendations</span></span>

<span data-ttu-id="60e23-186">La hora de decidir qué implementación de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> es mejor para su aplicación, considere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="60e23-186">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="60e23-187">Infraestructura existente</span><span class="sxs-lookup"><span data-stu-id="60e23-187">Existing infrastructure</span></span>
* <span data-ttu-id="60e23-188">Requisitos de rendimiento</span><span class="sxs-lookup"><span data-stu-id="60e23-188">Performance requirements</span></span>
* <span data-ttu-id="60e23-189">Costo</span><span class="sxs-lookup"><span data-stu-id="60e23-189">Cost</span></span>
* <span data-ttu-id="60e23-190">Experiencia del equipo</span><span class="sxs-lookup"><span data-stu-id="60e23-190">Team experience</span></span>

<span data-ttu-id="60e23-191">Soluciones de almacenamiento en caché normalmente se basan en el almacenamiento en memoria para proporcionar una recuperación rápida de los datos almacenados en caché, pero la memoria es un recurso limitado y expanda el costo.</span><span class="sxs-lookup"><span data-stu-id="60e23-191">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="60e23-192">Único almacén de datos utilizados normalmente en una memoria caché.</span><span class="sxs-lookup"><span data-stu-id="60e23-192">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="60e23-193">Por lo general, una instancia de Redis cache proporciona un mayor rendimiento y latencia más baja que una memoria caché de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="60e23-193">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="60e23-194">Sin embargo, las pruebas comparativas se suelen ser necesarias para determinar las características de rendimiento de las estrategias de almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="60e23-194">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="60e23-195">Cuando se usa SQL Server como un almacén de respaldo de caché distribuida, use la misma base de datos para la memoria caché y almacenamiento de datos normal de la aplicación y recuperación puede afectar negativamente al rendimiento de ambos.</span><span class="sxs-lookup"><span data-stu-id="60e23-195">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="60e23-196">Se recomienda usar una instancia de SQL Server dedicada para la memoria caché distribuida auxiliar.</span><span class="sxs-lookup"><span data-stu-id="60e23-196">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60e23-197">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="60e23-197">Additional resources</span></span>

* [<span data-ttu-id="60e23-198">Redis Cache en Azure</span><span class="sxs-lookup"><span data-stu-id="60e23-198">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="60e23-199">SQL Database en Azure</span><span class="sxs-lookup"><span data-stu-id="60e23-199">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <span data-ttu-id="60e23-200">[ASP.NET Core proveedor IDistributedCache de NCache en granjas de servidores Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache en GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="60e23-200">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
