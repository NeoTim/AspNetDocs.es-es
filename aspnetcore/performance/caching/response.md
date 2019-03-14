---
title: Almacenamiento en caché de respuesta en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo usar el almacenamiento en caché de respuestas para reducir los requisitos de ancho de banda y aumentar el rendimiento de las aplicaciones de ASP.NET Core.
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064052"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="81658-103">Almacenamiento en caché de respuesta en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81658-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="81658-104">Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="81658-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="81658-105">Almacenamiento en caché de respuesta en las páginas de Razor está disponible en ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="81658-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="81658-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="81658-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="81658-107">Almacenamiento en caché de respuesta reduce el número de solicitudes hace que un cliente o un proxy a un servidor web.</span><span class="sxs-lookup"><span data-stu-id="81658-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="81658-108">Almacenamiento en caché de respuesta también reduce la cantidad de trabajo se realiza el servidor web para generar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="81658-109">Almacenamiento en caché de respuesta se controla mediante los encabezados que especifican cómo desea que client, proxy y middleware en caché las respuestas.</span><span class="sxs-lookup"><span data-stu-id="81658-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="81658-110">El [del atributo ResponseCache](#responsecache-attribute) participa en la configuración de almacenamiento en caché de los encabezados, los clientes que pueden respetar al almacenar en caché las respuestas de respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="81658-111">[Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) puede usarse para la memoria caché las respuestas en el servidor.</span><span class="sxs-lookup"><span data-stu-id="81658-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="81658-112">Puede usar el middleware `ResponseCache` propiedades para influir en el comportamiento de almacenamiento en caché del lado servidor de atributos.</span><span class="sxs-lookup"><span data-stu-id="81658-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="81658-113">Almacenamiento en caché de respuesta basado en HTTP</span><span class="sxs-lookup"><span data-stu-id="81658-113">HTTP-based response caching</span></span>

<span data-ttu-id="81658-114">El [especificación HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234) describe cómo deben comportarse las memorias caché de Internet.</span><span class="sxs-lookup"><span data-stu-id="81658-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="81658-115">El encabezado HTTP principal que se usa para almacenar en caché es [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), que se usa para especificar la memoria caché *directivas*.</span><span class="sxs-lookup"><span data-stu-id="81658-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="81658-116">Las directivas de controlan el comportamiento de almacenamiento en caché a medida que las solicitudes lleguen desde los clientes a servidores y respuestas lleguen desde los servidores a los clientes.</span><span class="sxs-lookup"><span data-stu-id="81658-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="81658-117">Mueven las solicitudes y respuestas a través de servidores proxy y servidores proxy también deben cumplir la especificación HTTP 1.1 Caching.</span><span class="sxs-lookup"><span data-stu-id="81658-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="81658-118">Common `Cache-Control` directivas se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="81658-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="81658-119">Directiva</span><span class="sxs-lookup"><span data-stu-id="81658-119">Directive</span></span>                                                       | <span data-ttu-id="81658-120">Acción</span><span class="sxs-lookup"><span data-stu-id="81658-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="81658-121">public</span><span class="sxs-lookup"><span data-stu-id="81658-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="81658-122">Una memoria caché puede almacenar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="81658-123">private</span><span class="sxs-lookup"><span data-stu-id="81658-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="81658-124">La respuesta no debe almacenarse en una memoria caché compartida.</span><span class="sxs-lookup"><span data-stu-id="81658-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="81658-125">Una caché privada puede almacenar y reutilizar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="81658-126">max-age</span><span class="sxs-lookup"><span data-stu-id="81658-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="81658-127">El cliente no aceptará una respuesta cuya antigüedad es superior al número especificado de segundos.</span><span class="sxs-lookup"><span data-stu-id="81658-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="81658-128">Ejemplos: `max-age=60` (60 segundos), `max-age=2592000` (1 mes)</span><span class="sxs-lookup"><span data-stu-id="81658-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="81658-129">no-cache</span><span class="sxs-lookup"><span data-stu-id="81658-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="81658-130">**En las solicitudes**: Una memoria caché no debe usar una respuesta almacenada para satisfacer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="81658-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="81658-131">Nota: El servidor de origen vuelve a genera la respuesta para el cliente y el software intermedio actualiza la respuesta almacenada en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="81658-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="81658-132">**En las respuestas**: La respuesta no debe usarse para una solicitud posterior sin validación en el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="81658-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="81658-133">no-store</span><span class="sxs-lookup"><span data-stu-id="81658-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="81658-134">**En las solicitudes**: Una memoria caché no debe almacenar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="81658-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="81658-135">**En las respuestas**: Una memoria caché no debe almacenar cualquier parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="81658-136">Otros encabezados de caché que desempeñan un papel en la caché se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="81658-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="81658-137">Header</span><span class="sxs-lookup"><span data-stu-id="81658-137">Header</span></span>                                                     | <span data-ttu-id="81658-138">Función</span><span class="sxs-lookup"><span data-stu-id="81658-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="81658-139">Edad</span><span class="sxs-lookup"><span data-stu-id="81658-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="81658-140">Una estimación de la cantidad de tiempo en segundos desde que se generó la respuesta o se valida correctamente en el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="81658-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="81658-141">Expira</span><span class="sxs-lookup"><span data-stu-id="81658-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="81658-142">La fecha y hora después de que la respuesta se considera obsoleto.</span><span class="sxs-lookup"><span data-stu-id="81658-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="81658-143">Pragma</span><span class="sxs-lookup"><span data-stu-id="81658-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="81658-144">Existe para hacia atrás almacena en caché la compatibilidad con HTTP/1.0 para configuración `no-cache` comportamiento.</span><span class="sxs-lookup"><span data-stu-id="81658-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="81658-145">Si el `Cache-Control` encabezado está presente, el `Pragma` se omite el encabezado.</span><span class="sxs-lookup"><span data-stu-id="81658-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="81658-146">Vary</span><span class="sxs-lookup"><span data-stu-id="81658-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="81658-147">Especifica que una respuesta almacenada en caché no se debe enviar a menos que todos los de la `Vary` coincide con los campos de encabezado de solicitud original de la respuesta almacenada en caché y la nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="81658-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="81658-148">Las directivas de Control de caché de solicitud de aspectos de almacenamiento en caché basado en HTTP</span><span class="sxs-lookup"><span data-stu-id="81658-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="81658-149">El [especificación HTTP 1.1 Caching para el encabezado Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiere una memoria caché que se respeten válido `Cache-Control` encabezado enviado por el cliente.</span><span class="sxs-lookup"><span data-stu-id="81658-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="81658-150">Un cliente puede realizar solicitudes con un `no-cache` fuerza el servidor para generar una nueva respuesta para cada solicitud y el valor de encabezado.</span><span class="sxs-lookup"><span data-stu-id="81658-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="81658-151">Teniendo siempre cliente `Cache-Control` encabezados de solicitud tiene sentido si piensa que el objetivo del almacenamiento en caché de HTTP.</span><span class="sxs-lookup"><span data-stu-id="81658-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="81658-152">En la especificación oficial, el almacenamiento en caché está pensado para reducir la sobrecarga de latencia y la red de satisfacer las solicitudes a través de una red de clientes, servidores proxy y servidores.</span><span class="sxs-lookup"><span data-stu-id="81658-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="81658-153">No es necesariamente una manera de controlar la carga en un servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="81658-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="81658-154">No hay ningún control del desarrollador sobre este comportamiento de almacenamiento en caché cuando se usa el [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) porque el middleware se ajusta al almacenamiento en caché de la especificación oficial.</span><span class="sxs-lookup"><span data-stu-id="81658-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="81658-155">[Planeado mejoras al middleware](https://github.com/aspnet/AspNetCore/issues/2612) son una oportunidad para configurar el middleware para omitir una solicitud `Cache-Control` encabezado de la hora de decidir atender una respuesta almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="81658-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="81658-156">Las mejoras previstas ofrecen una oportunidad para una mejor carga del servidor de control.</span><span class="sxs-lookup"><span data-stu-id="81658-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="81658-157">Otra tecnología de almacenamiento en caché en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81658-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="81658-158">Almacenamiento en caché en memoria</span><span class="sxs-lookup"><span data-stu-id="81658-158">In-memory caching</span></span>

<span data-ttu-id="81658-159">Almacenamiento en caché en memoria utiliza memoria del servidor para almacenar datos en caché.</span><span class="sxs-lookup"><span data-stu-id="81658-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="81658-160">Este tipo de almacenamiento en caché es adecuado para un solo servidor o varios servidores mediante *sesiones permanentes*.</span><span class="sxs-lookup"><span data-stu-id="81658-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="81658-161">Sesiones permanentes significa que las solicitudes de un cliente se enruten siempre al mismo servidor para su procesamiento.</span><span class="sxs-lookup"><span data-stu-id="81658-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="81658-162">Para obtener más información, consulte [almacenar en caché en memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="81658-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="81658-163">Caché distribuida</span><span class="sxs-lookup"><span data-stu-id="81658-163">Distributed Cache</span></span>

<span data-ttu-id="81658-164">Usar una caché distribuida para almacenar datos en memoria cuando la aplicación se hospeda en una granja de servidores en la nube o el servidor.</span><span class="sxs-lookup"><span data-stu-id="81658-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="81658-165">La memoria caché se comparte entre los servidores que procesan las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="81658-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="81658-166">Un cliente puede enviar una solicitud que se controla mediante cualquier servidor en el grupo si los datos almacenados en caché para el cliente están disponibles.</span><span class="sxs-lookup"><span data-stu-id="81658-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="81658-167">ASP.NET Core ofrece SQL Server y las memorias caché de Redis distribuida.</span><span class="sxs-lookup"><span data-stu-id="81658-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="81658-168">Para obtener más información, consulta <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="81658-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="81658-169">Aplicación auxiliar de etiquetas de caché</span><span class="sxs-lookup"><span data-stu-id="81658-169">Cache Tag Helper</span></span>

<span data-ttu-id="81658-170">Puede almacenar en caché el contenido de una vista de MVC o la página de Razor con la aplicación auxiliar de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="81658-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="81658-171">La aplicación auxiliar de etiquetas de caché usa almacenamiento en caché en memoria para almacenar datos.</span><span class="sxs-lookup"><span data-stu-id="81658-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="81658-172">Para obtener más información, consulte [aplicación auxiliar de etiquetas de caché en ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="81658-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="81658-173">Asistente de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="81658-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="81658-174">Puede almacenar en caché el contenido de una vista de MVC o la página de Razor en distribuidas en la nube o escenarios de granja de servidores web con la aplicación auxiliar de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="81658-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="81658-175">La aplicación auxiliar de etiquetas de caché de distribuido usa SQL Server o Redis para almacenar datos.</span><span class="sxs-lookup"><span data-stu-id="81658-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="81658-176">Para obtener más información, consulte [distribuidas aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="81658-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="81658-177">Atributo ResponseCache</span><span class="sxs-lookup"><span data-stu-id="81658-177">ResponseCache attribute</span></span>

<span data-ttu-id="81658-178">El [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) especifica los parámetros necesarios para establecer encabezados apropiados en el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="81658-179">Deshabilitar la memoria caché para el contenido que contiene información para los clientes autenticados.</span><span class="sxs-lookup"><span data-stu-id="81658-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="81658-180">Almacenamiento en caché solo debería habilitarse para el contenido que no cambia en función de la identidad de un usuario o si un usuario ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="81658-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="81658-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varía la respuesta almacenada en los valores de la lista de las claves de consulta determinada.</span><span class="sxs-lookup"><span data-stu-id="81658-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="81658-182">Cuando el valor único `*` es siempre el middleware varía las respuestas de todos los parámetros de cadena de consulta de solicitud.</span><span class="sxs-lookup"><span data-stu-id="81658-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="81658-183">`VaryByQueryKeys` requiere ASP.NET Core 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="81658-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="81658-184">[Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) debe estar habilitado para establecer el `VaryByQueryKeys` propiedad; en caso contrario, se produce una excepción en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="81658-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="81658-185">No hay un encabezado HTTP correspondiente para el `VaryByQueryKeys` propiedad.</span><span class="sxs-lookup"><span data-stu-id="81658-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="81658-186">La propiedad es una característica de HTTP controlada el Middleware de almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="81658-187">Para que el middleware servir una respuesta almacenada en caché, la cadena de consulta y el valor de cadena de consulta deben coincidir con una solicitud anterior.</span><span class="sxs-lookup"><span data-stu-id="81658-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="81658-188">Por ejemplo, considere la posibilidad de la secuencia de solicitudes y los resultados que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="81658-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="81658-189">Request</span><span class="sxs-lookup"><span data-stu-id="81658-189">Request</span></span>                          | <span data-ttu-id="81658-190">Resultado</span><span class="sxs-lookup"><span data-stu-id="81658-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="81658-191">Devuelto desde el servidor</span><span class="sxs-lookup"><span data-stu-id="81658-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="81658-192">Devuelve de middleware</span><span class="sxs-lookup"><span data-stu-id="81658-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="81658-193">Devuelto desde el servidor</span><span class="sxs-lookup"><span data-stu-id="81658-193">Returned from server</span></span>     |

<span data-ttu-id="81658-194">La primera solicitud es devuelto por el servidor y almacena en caché en el middleware.</span><span class="sxs-lookup"><span data-stu-id="81658-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="81658-195">Middleware devuelve la segunda solicitud porque la cadena de consulta coincide con la solicitud anterior.</span><span class="sxs-lookup"><span data-stu-id="81658-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="81658-196">La tercera solicitud no está en la memoria caché de middleware porque el valor de cadena de consulta no coincide con una solicitud anterior.</span><span class="sxs-lookup"><span data-stu-id="81658-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="81658-197">El `ResponseCacheAttribute` se usa para configurar y crear (a través de `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="81658-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="81658-198">El `ResponseCacheFilter` realiza el trabajo de actualización de los encabezados HTTP adecuados y las características de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="81658-199">El filtro:</span><span class="sxs-lookup"><span data-stu-id="81658-199">The filter:</span></span>

* <span data-ttu-id="81658-200">Quita los encabezados existentes para `Vary`, `Cache-Control`, y `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="81658-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="81658-201">Escribe los encabezados adecuados en función de las propiedades establecidas en el `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="81658-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="81658-202">Actualiza la respuesta a la característica HTTP de almacenamiento en caché si `VaryByQueryKeys` está establecido.</span><span class="sxs-lookup"><span data-stu-id="81658-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="81658-203">Variar</span><span class="sxs-lookup"><span data-stu-id="81658-203">Vary</span></span>

<span data-ttu-id="81658-204">Este encabezado solo cuando se escribe el `VaryByHeader` se establece la propiedad.</span><span class="sxs-lookup"><span data-stu-id="81658-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="81658-205">Se establece en el `Vary` el valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="81658-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="81658-206">El ejemplo siguiente usa el `VaryByHeader` propiedad:</span><span class="sxs-lookup"><span data-stu-id="81658-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="81658-207">Puede ver los encabezados de respuesta con las herramientas de red de su explorador.</span><span class="sxs-lookup"><span data-stu-id="81658-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="81658-208">La siguiente imagen muestra la F12 de borde de salida en el **red** pestaña cuando el `About2` se actualiza el método de acción:</span><span class="sxs-lookup"><span data-stu-id="81658-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Borde F12 salida en la ficha red cuando se llama al método de acción About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="81658-210">NoStore y Location.None</span><span class="sxs-lookup"><span data-stu-id="81658-210">NoStore and Location.None</span></span>

<span data-ttu-id="81658-211">`NoStore` invalida la mayoría de las demás propiedades.</span><span class="sxs-lookup"><span data-stu-id="81658-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="81658-212">Cuando esta propiedad se establece en `true`, `Cache-Control` encabezado se establece en `no-store`.</span><span class="sxs-lookup"><span data-stu-id="81658-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="81658-213">Si `Location` está establecido en `None`:</span><span class="sxs-lookup"><span data-stu-id="81658-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="81658-214">El valor de `Cache-Control` está establecido en `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="81658-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="81658-215">El valor de `Pragma` está establecido en `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="81658-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="81658-216">Si `NoStore` es `false` y `Location` es `None`, `Cache-Control` y `Pragma` se establecen en `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="81658-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="81658-217">Normalmente establece `NoStore` a `true` en las páginas de error.</span><span class="sxs-lookup"><span data-stu-id="81658-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="81658-218">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="81658-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="81658-219">Esto da como resultado los encabezados siguientes:</span><span class="sxs-lookup"><span data-stu-id="81658-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="81658-220">Ubicación y la duración</span><span class="sxs-lookup"><span data-stu-id="81658-220">Location and Duration</span></span>

<span data-ttu-id="81658-221">Para habilitar el almacenamiento en caché, `Duration` debe establecerse en un valor positivo y `Location` debe ser `Any` (valor predeterminado) o `Client`.</span><span class="sxs-lookup"><span data-stu-id="81658-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="81658-222">En este caso, el `Cache-Control` encabezado se establece en el valor de ubicación seguido por el `max-age` de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="81658-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="81658-223">`Location`de las opciones de `Any` y `Client` traducir `Cache-Control` valores de encabezado `public` y `private`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="81658-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="81658-224">Como se indicó anteriormente, establecer `Location` a `None` establece ambos `Cache-Control` y `Pragma` encabezados para `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="81658-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="81658-225">A continuación se muestra un ejemplo que muestra los encabezados generado estableciendo `Duration` y dejar el valor predeterminado `Location` valor:</span><span class="sxs-lookup"><span data-stu-id="81658-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="81658-226">Esto genera el siguiente encabezado:</span><span class="sxs-lookup"><span data-stu-id="81658-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="81658-227">Perfiles de memoria caché</span><span class="sxs-lookup"><span data-stu-id="81658-227">Cache profiles</span></span>

<span data-ttu-id="81658-228">En lugar de duplicar `ResponseCache` configuración en muchos atributos de acción de controlador, perfiles de memoria caché se puede configurar como opciones al configurar MVC en el `ConfigureServices` método `Startup`.</span><span class="sxs-lookup"><span data-stu-id="81658-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="81658-229">Valores que se encuentran en un perfil de caché que se hace referencia se utilizan como los valores predeterminados por los `ResponseCache` de atributo y se reemplazan por las propiedades especificadas en el atributo.</span><span class="sxs-lookup"><span data-stu-id="81658-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="81658-230">Configurar un perfil de caché:</span><span class="sxs-lookup"><span data-stu-id="81658-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="81658-231">Hacer referencia a un perfil de caché:</span><span class="sxs-lookup"><span data-stu-id="81658-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="81658-232">El `ResponseCache` atributo puede aplicarse tanto a las acciones (métodos) y controladores (clases).</span><span class="sxs-lookup"><span data-stu-id="81658-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="81658-233">Atributos de nivel de método invalidan la configuración especificada en los atributos de nivel de clase.</span><span class="sxs-lookup"><span data-stu-id="81658-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="81658-234">En el ejemplo anterior, un atributo de nivel de clase especifica una duración de 30 segundos, mientras que un atributo de nivel de método hace referencia a un perfil de caché con una duración establecida en 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="81658-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="81658-235">El encabezado resultante:</span><span class="sxs-lookup"><span data-stu-id="81658-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="81658-236">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="81658-236">Additional resources</span></span>

* [<span data-ttu-id="81658-237">Almacenar las respuestas en las memorias caché</span><span class="sxs-lookup"><span data-stu-id="81658-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="81658-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="81658-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
