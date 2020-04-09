---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Enrutamiento de atributos en ASP.NET Web API 2 Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: bb68fe8e6769619029a3fa039d6f0d6f3303afbe
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675384"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="dfbee-102">Enrutamiento de atributos en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="dfbee-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="dfbee-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dfbee-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dfbee-104">*Enrutamiento* es cómo Web API hace coincidir un URI con una acción.</span><span class="sxs-lookup"><span data-stu-id="dfbee-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="dfbee-105">Web API 2 admite un nuevo tipo de enrutamiento, denominado enrutamiento de *atributos.*</span><span class="sxs-lookup"><span data-stu-id="dfbee-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="dfbee-106">Como su nombre indica, el enrutamiento de atributos utiliza atributos para definir rutas.</span><span class="sxs-lookup"><span data-stu-id="dfbee-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="dfbee-107">El enrutamiento de atributos proporciona más control sobre los URI de la API web.</span><span class="sxs-lookup"><span data-stu-id="dfbee-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="dfbee-108">Por ejemplo, puede crear fácilmente URI que describan jerarquías de recursos.</span><span class="sxs-lookup"><span data-stu-id="dfbee-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="dfbee-109">El estilo anterior de enrutamiento, denominado enrutamiento basado en convenciones, sigue siendo totalmente compatible.</span><span class="sxs-lookup"><span data-stu-id="dfbee-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="dfbee-110">De hecho, puede combinar ambas técnicas en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="dfbee-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="dfbee-111">En este tema se muestra cómo habilitar el enrutamiento de atributos y se describen las distintas opciones para el enrutamiento de atributos.</span><span class="sxs-lookup"><span data-stu-id="dfbee-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="dfbee-112">Para ver un tutorial de extremo a extremo que usa el enrutamiento de atributos, consulte Crear una API REST con enrutamiento de [atributos en Web API 2.](create-a-rest-api-with-attribute-routing.md)</span><span class="sxs-lookup"><span data-stu-id="dfbee-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfbee-113">Prerrequisitos</span><span class="sxs-lookup"><span data-stu-id="dfbee-113">Prerequisites</span></span>

<span data-ttu-id="dfbee-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Edición Community, Professional o Enterprise</span><span class="sxs-lookup"><span data-stu-id="dfbee-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="dfbee-115">Como alternativa, use nuGet Package Manager para instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="dfbee-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="dfbee-116">En el menú **Herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione Consola del Administrador de **paquetes**.</span><span class="sxs-lookup"><span data-stu-id="dfbee-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="dfbee-117">Escriba el siguiente comando en la ventana Consola del Administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="dfbee-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="dfbee-118">¿Por qué enrutamiento de atributos?</span><span class="sxs-lookup"><span data-stu-id="dfbee-118">Why Attribute Routing?</span></span>

<span data-ttu-id="dfbee-119">La primera versión de Web API utiliza el enrutamiento *basado en convenciones.*</span><span class="sxs-lookup"><span data-stu-id="dfbee-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="dfbee-120">En ese tipo de enrutamiento, se definen una o varias plantillas de ruta, que son básicamente cadenas parametrizadas.</span><span class="sxs-lookup"><span data-stu-id="dfbee-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="dfbee-121">Cuando el marco de trabajo recibe una solicitud, coincide con el URI con la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="dfbee-122">Para obtener más información sobre el enrutamiento basado en convenciones, consulte [Enrutamiento en ASP.NET API web](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="dfbee-122">For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="dfbee-123">Una ventaja del enrutamiento basado en convenciones es que las plantillas se definen en un solo lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="dfbee-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="dfbee-124">Desafortunadamente, el enrutamiento basado en convenciones dificulta la compatibilidad con determinados patrones de URI que son comunes en las API RESTful.</span><span class="sxs-lookup"><span data-stu-id="dfbee-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="dfbee-125">Por ejemplo, los recursos a menudo contienen recursos secundarios: los clientes tienen pedidos, las películas tienen actores, los libros tienen autores, etc.</span><span class="sxs-lookup"><span data-stu-id="dfbee-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="dfbee-126">Es natural crear URI que reflejen estas relaciones:</span><span class="sxs-lookup"><span data-stu-id="dfbee-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="dfbee-127">Este tipo de URI es difícil de crear mediante el enrutamiento basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="dfbee-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="dfbee-128">Aunque se puede hacer, los resultados no se escalan bien si tiene muchos controladores o tipos de recursos.</span><span class="sxs-lookup"><span data-stu-id="dfbee-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="dfbee-129">Con el enrutamiento de atributos, es trivial definir una ruta para este URI.</span><span class="sxs-lookup"><span data-stu-id="dfbee-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="dfbee-130">Simplemente agregue un atributo a la acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="dfbee-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="dfbee-131">Estos son algunos otros patrones que el enrutamiento de atributos facilita.</span><span class="sxs-lookup"><span data-stu-id="dfbee-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="dfbee-132">**Control de versiones de la API**</span><span class="sxs-lookup"><span data-stu-id="dfbee-132">**API versioning**</span></span>

<span data-ttu-id="dfbee-133">En este ejemplo, "/api/v1/products" se enrutaría a un controlador diferente de "/api/v2/products".</span><span class="sxs-lookup"><span data-stu-id="dfbee-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="dfbee-134">**Segmentos URI sobrecargados**</span><span class="sxs-lookup"><span data-stu-id="dfbee-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="dfbee-135">En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.</span><span class="sxs-lookup"><span data-stu-id="dfbee-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="dfbee-136">**Múltiples tipos de parámetros**</span><span class="sxs-lookup"><span data-stu-id="dfbee-136">**Multiple parameter types**</span></span>

<span data-ttu-id="dfbee-137">En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.</span><span class="sxs-lookup"><span data-stu-id="dfbee-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="dfbee-138">Habilitación del enrutamiento de atributos</span><span class="sxs-lookup"><span data-stu-id="dfbee-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="dfbee-139">Para habilitar el enrutamiento de atributos, llame a **MapHttpAttributeRoutes** durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="dfbee-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="dfbee-140">Este método de extensión se define en la clase **System.Web.Http.HttpConfigurationExtensions.**</span><span class="sxs-lookup"><span data-stu-id="dfbee-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="dfbee-141">El enrutamiento de atributos se puede combinar con el enrutamiento [basado en convenciones.](routing-in-aspnet-web-api.md)</span><span class="sxs-lookup"><span data-stu-id="dfbee-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="dfbee-142">Para definir rutas basadas en convenciones, llame al método **MapHttpRoute.**</span><span class="sxs-lookup"><span data-stu-id="dfbee-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="dfbee-143">Para obtener más información acerca de la configuración de la API web, consulte [Configuración de ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="dfbee-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="dfbee-144">Nota: Migración desde Web API 1</span><span class="sxs-lookup"><span data-stu-id="dfbee-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="dfbee-145">Antes de Web API 2, las plantillas de proyecto de API web generaban código como este:</span><span class="sxs-lookup"><span data-stu-id="dfbee-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="dfbee-146">Si el enrutamiento de atributos está habilitado, este código producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="dfbee-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="dfbee-147">Si actualiza un proyecto de API web existente para usar el enrutamiento de atributos, asegúrese de actualizar este código de configuración a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfbee-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="dfbee-148">Para obtener más información, consulte [Configuración de la API web con ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="dfbee-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="dfbee-149">Adición de atributos de ruta</span><span class="sxs-lookup"><span data-stu-id="dfbee-149">Adding Route Attributes</span></span>

<span data-ttu-id="dfbee-150">A continuación se muestra un ejemplo de una ruta definida mediante un atributo:</span><span class="sxs-lookup"><span data-stu-id="dfbee-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="dfbee-151">La &quot;cadena customers/-customerId-/orders&quot; es la plantilla DE URI para la ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="dfbee-152">Web API intenta hacer coincidir el URI de solicitud con la plantilla.</span><span class="sxs-lookup"><span data-stu-id="dfbee-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="dfbee-153">En este ejemplo, "clientes" y "pedidos" son segmentos literales, y "'customerId'" es un parámetro de variable.</span><span class="sxs-lookup"><span data-stu-id="dfbee-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="dfbee-154">Los siguientes URI coincidirían con esta plantilla:</span><span class="sxs-lookup"><span data-stu-id="dfbee-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="dfbee-155">Puede restringir la coincidencia mediante [restricciones](#constraints), que se describen más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="dfbee-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="dfbee-156">Tenga en &quot;cuenta que&quot; el parámetro .customerId en la plantilla de ruta coincide con el nombre del parámetro *customerId* en el método.</span><span class="sxs-lookup"><span data-stu-id="dfbee-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="dfbee-157">Cuando Web API invoca la acción del controlador, intenta enlazar los parámetros de ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="dfbee-158">Por ejemplo, si `http://example.com/customers/1/orders`el URI es , Web API intenta enlazar el valor "1" al parámetro *customerId* de la acción.</span><span class="sxs-lookup"><span data-stu-id="dfbee-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="dfbee-159">Una plantilla URI puede tener varios parámetros:</span><span class="sxs-lookup"><span data-stu-id="dfbee-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="dfbee-160">Cualquier método de controlador que no tenga un atributo de ruta utiliza enrutamiento basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="dfbee-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="dfbee-161">De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="dfbee-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="dfbee-162">HTTP Methods</span><span class="sxs-lookup"><span data-stu-id="dfbee-162">HTTP Methods</span></span>

<span data-ttu-id="dfbee-163">Web API también selecciona acciones basadas en el método HTTP de la solicitud (GET, POST, etc.).</span><span class="sxs-lookup"><span data-stu-id="dfbee-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="dfbee-164">De forma predeterminada, Web API busca una coincidencia que no distinve entre mayúsculas y minúsculas con el inicio del nombre del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="dfbee-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="dfbee-165">Por ejemplo, un `PutCustomers` método de controlador denominado coincide con una solicitud HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="dfbee-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="dfbee-166">Puede invalidar esta convención decorando el método con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="dfbee-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="dfbee-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="dfbee-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-168">**[HttpGet]**</span></span>
- <span data-ttu-id="dfbee-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-169">**[HttpHead]**</span></span>
- <span data-ttu-id="dfbee-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="dfbee-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="dfbee-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-172">**[HttpPost]**</span></span>
- <span data-ttu-id="dfbee-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="dfbee-173">**[HttpPut]**</span></span>

<span data-ttu-id="dfbee-174">En el ejemplo siguiente se asigna el método CreateBook a solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="dfbee-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="dfbee-175">Para todos los demás métodos HTTP, incluidos los métodos no estándar, utilice el atributo **AcceptVerbs,** que toma una lista de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="dfbee-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="dfbee-176">Prefijos de ruta</span><span class="sxs-lookup"><span data-stu-id="dfbee-176">Route Prefixes</span></span>

<span data-ttu-id="dfbee-177">A menudo, las rutas en un regulador comienzan con el mismo prefijo.</span><span class="sxs-lookup"><span data-stu-id="dfbee-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="dfbee-178">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dfbee-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="dfbee-179">Puede establecer un prefijo común para un controlador completo mediante el atributo **[RoutePrefix]:**</span><span class="sxs-lookup"><span data-stu-id="dfbee-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="dfbee-180">Utilice una tilde (-) en el atributo de método para invalidar el prefijo de ruta:</span><span class="sxs-lookup"><span data-stu-id="dfbee-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="dfbee-181">El prefijo de ruta puede incluir parámetros:</span><span class="sxs-lookup"><span data-stu-id="dfbee-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="dfbee-182">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="dfbee-182">Route Constraints</span></span>

<span data-ttu-id="dfbee-183">Las restricciones de ruta le permiten restringir la forma en que coinciden los parámetros de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="dfbee-184">La sintaxis &quot;general es&quot;.parámetro:restricción.</span><span class="sxs-lookup"><span data-stu-id="dfbee-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="dfbee-185">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dfbee-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="dfbee-186">Aquí, la primera ruta solo &quot;se&quot; seleccionará si el segmento id del URI es un entero.</span><span class="sxs-lookup"><span data-stu-id="dfbee-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="dfbee-187">De lo contrario, se elegirá la segunda ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="dfbee-188">En la tabla siguiente se enumeran las restricciones que se admiten.</span><span class="sxs-lookup"><span data-stu-id="dfbee-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="dfbee-189">Restricción</span><span class="sxs-lookup"><span data-stu-id="dfbee-189">Constraint</span></span> | <span data-ttu-id="dfbee-190">Descripción</span><span class="sxs-lookup"><span data-stu-id="dfbee-190">Description</span></span> | <span data-ttu-id="dfbee-191">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="dfbee-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dfbee-192">alpha</span><span class="sxs-lookup"><span data-stu-id="dfbee-192">alpha</span></span> | <span data-ttu-id="dfbee-193">Coincide con caracteres alfabéticos latinos en mayúsculas o minúsculas (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="dfbee-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="dfbee-194">•x:alpha</span><span class="sxs-lookup"><span data-stu-id="dfbee-194">{x:alpha}</span></span> |
| <span data-ttu-id="dfbee-195">bool</span><span class="sxs-lookup"><span data-stu-id="dfbee-195">bool</span></span> | <span data-ttu-id="dfbee-196">Coincide con un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="dfbee-196">Matches a Boolean value.</span></span> | <span data-ttu-id="dfbee-197">•x:bool.</span><span class="sxs-lookup"><span data-stu-id="dfbee-197">{x:bool}</span></span> |
| <span data-ttu-id="dfbee-198">datetime</span><span class="sxs-lookup"><span data-stu-id="dfbee-198">datetime</span></span> | <span data-ttu-id="dfbee-199">Coincide con un **dateTime** valor.</span><span class="sxs-lookup"><span data-stu-id="dfbee-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="dfbee-200">•x:datetime</span><span class="sxs-lookup"><span data-stu-id="dfbee-200">{x:datetime}</span></span> |
| <span data-ttu-id="dfbee-201">Decimal</span><span class="sxs-lookup"><span data-stu-id="dfbee-201">decimal</span></span> | <span data-ttu-id="dfbee-202">Coincide con un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="dfbee-202">Matches a decimal value.</span></span> | <span data-ttu-id="dfbee-203">.x:decimal ?</span><span class="sxs-lookup"><span data-stu-id="dfbee-203">{x:decimal}</span></span> |
| <span data-ttu-id="dfbee-204">double</span><span class="sxs-lookup"><span data-stu-id="dfbee-204">double</span></span> | <span data-ttu-id="dfbee-205">Coincide con un valor de punto flotante de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="dfbee-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="dfbee-206">•x:doble</span><span class="sxs-lookup"><span data-stu-id="dfbee-206">{x:double}</span></span> |
| <span data-ttu-id="dfbee-207">FLOAT</span><span class="sxs-lookup"><span data-stu-id="dfbee-207">float</span></span> | <span data-ttu-id="dfbee-208">Coincide con un valor de punto flotante de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="dfbee-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="dfbee-209">•x:float</span><span class="sxs-lookup"><span data-stu-id="dfbee-209">{x:float}</span></span> |
| <span data-ttu-id="dfbee-210">guid</span><span class="sxs-lookup"><span data-stu-id="dfbee-210">guid</span></span> | <span data-ttu-id="dfbee-211">Coincide con un valor GUID.</span><span class="sxs-lookup"><span data-stu-id="dfbee-211">Matches a GUID value.</span></span> | <span data-ttu-id="dfbee-212">•x:guid</span><span class="sxs-lookup"><span data-stu-id="dfbee-212">{x:guid}</span></span> |
| <span data-ttu-id="dfbee-213">int</span><span class="sxs-lookup"><span data-stu-id="dfbee-213">int</span></span> | <span data-ttu-id="dfbee-214">Coincide con un valor entero de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="dfbee-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="dfbee-215">•x:int.</span><span class="sxs-lookup"><span data-stu-id="dfbee-215">{x:int}</span></span> |
| <span data-ttu-id="dfbee-216">length</span><span class="sxs-lookup"><span data-stu-id="dfbee-216">length</span></span> | <span data-ttu-id="dfbee-217">Coincide con una cadena con la longitud especificada o dentro de un intervalo de longitudes especificado.</span><span class="sxs-lookup"><span data-stu-id="dfbee-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="dfbee-218">•x:longitud(6) •x:longitud(1,20)</span><span class="sxs-lookup"><span data-stu-id="dfbee-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="dfbee-219">long</span><span class="sxs-lookup"><span data-stu-id="dfbee-219">long</span></span> | <span data-ttu-id="dfbee-220">Coincide con un valor entero de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="dfbee-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="dfbee-221">•x:largo</span><span class="sxs-lookup"><span data-stu-id="dfbee-221">{x:long}</span></span> |
| <span data-ttu-id="dfbee-222">max</span><span class="sxs-lookup"><span data-stu-id="dfbee-222">max</span></span> | <span data-ttu-id="dfbee-223">Coincide con un entero con un valor máximo.</span><span class="sxs-lookup"><span data-stu-id="dfbee-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="dfbee-224">•x:max(10)</span><span class="sxs-lookup"><span data-stu-id="dfbee-224">{x:max(10)}</span></span> |
| <span data-ttu-id="dfbee-225">Maxlength</span><span class="sxs-lookup"><span data-stu-id="dfbee-225">maxlength</span></span> | <span data-ttu-id="dfbee-226">Coincide con una cadena con una longitud máxima.</span><span class="sxs-lookup"><span data-stu-id="dfbee-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="dfbee-227">•x:maxlength(10)</span><span class="sxs-lookup"><span data-stu-id="dfbee-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="dfbee-228">Min</span><span class="sxs-lookup"><span data-stu-id="dfbee-228">min</span></span> | <span data-ttu-id="dfbee-229">Coincide con un entero con un valor mínimo.</span><span class="sxs-lookup"><span data-stu-id="dfbee-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="dfbee-230">•x:min(10)</span><span class="sxs-lookup"><span data-stu-id="dfbee-230">{x:min(10)}</span></span> |
| <span data-ttu-id="dfbee-231">minlength</span><span class="sxs-lookup"><span data-stu-id="dfbee-231">minlength</span></span> | <span data-ttu-id="dfbee-232">Coincide con una cadena con una longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="dfbee-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="dfbee-233">•x:minlength(10)</span><span class="sxs-lookup"><span data-stu-id="dfbee-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="dfbee-234">range</span><span class="sxs-lookup"><span data-stu-id="dfbee-234">range</span></span> | <span data-ttu-id="dfbee-235">Coincide con un entero dentro de un intervalo de valores.</span><span class="sxs-lookup"><span data-stu-id="dfbee-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="dfbee-236">•x:rango(10,50)</span><span class="sxs-lookup"><span data-stu-id="dfbee-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="dfbee-237">regex</span><span class="sxs-lookup"><span data-stu-id="dfbee-237">regex</span></span> | <span data-ttu-id="dfbee-238">Coincide con una expresión regular.</span><span class="sxs-lookup"><span data-stu-id="dfbee-238">Matches a regular expression.</span></span> | <span data-ttu-id="dfbee-239">•x:regex('d{3}''d{3}''d ''''''''''''''''''''''''''''''''''''''''''{4}</span><span class="sxs-lookup"><span data-stu-id="dfbee-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="dfbee-240">Observe que algunas de las &quot;&quot;restricciones, como min , toman argumentos entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="dfbee-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="dfbee-241">Puede aplicar varias restricciones a un parámetro, separadas por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="dfbee-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="dfbee-242">Restricciones de ruta personalizadas</span><span class="sxs-lookup"><span data-stu-id="dfbee-242">Custom Route Constraints</span></span>

<span data-ttu-id="dfbee-243">Puede crear restricciones de ruta personalizadas implementando la interfaz **IHttpRouteConstraint.**</span><span class="sxs-lookup"><span data-stu-id="dfbee-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="dfbee-244">Por ejemplo, la restricción siguiente restringe un parámetro a un valor entero distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="dfbee-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="dfbee-245">El código siguiente muestra cómo registrar la restricción:</span><span class="sxs-lookup"><span data-stu-id="dfbee-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="dfbee-246">Ahora puede aplicar la restricción en sus rutas:</span><span class="sxs-lookup"><span data-stu-id="dfbee-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="dfbee-247">También puede reemplazar toda la **clase DefaultInlineConstraintResolver** implementando la interfaz **IInlineConstraintResolver.**</span><span class="sxs-lookup"><span data-stu-id="dfbee-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="dfbee-248">Si lo hace, reemplazará todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** las agregue específicamente.</span><span class="sxs-lookup"><span data-stu-id="dfbee-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="dfbee-249">Parámetros URI opcionales y valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="dfbee-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="dfbee-250">Puede hacer que un parámetro URI sea opcional agregando un signo de interrogación al parámetro route.</span><span class="sxs-lookup"><span data-stu-id="dfbee-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="dfbee-251">Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="dfbee-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="dfbee-252">En este `/api/books/locale/1033` ejemplo, devuelva `/api/books/locale` el mismo recurso.</span><span class="sxs-lookup"><span data-stu-id="dfbee-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="dfbee-253">Como alternativa, puede especificar un valor predeterminado dentro de la plantilla de ruta, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="dfbee-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="dfbee-254">Esto es casi lo mismo que el ejemplo anterior, pero hay una ligera diferencia de comportamiento cuando se aplica el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dfbee-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="dfbee-255">En el primer ejemplo,'lcid:int?''' y se asigna directamente al parámetro de método, por lo que el parámetro tendrá este valor exacto.</span><span class="sxs-lookup"><span data-stu-id="dfbee-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="dfbee-256">En el segundo ejemplo,'lcid:int'1033'''' y '''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''</span><span class="sxs-lookup"><span data-stu-id="dfbee-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="dfbee-257">El model-binder predeterminado convertirá "1033" al valor numérico 1033.</span><span class="sxs-lookup"><span data-stu-id="dfbee-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="dfbee-258">Sin embargo, puede conectar un enlazador de modelos personalizado, que podría hacer algo diferente.</span><span class="sxs-lookup"><span data-stu-id="dfbee-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="dfbee-259">(En la mayoría de los casos, a menos que tenga enlazadores de modelos personalizados en la canalización, los dos formularios serán equivalentes.)</span><span class="sxs-lookup"><span data-stu-id="dfbee-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="dfbee-260">Nombres de rutas</span><span class="sxs-lookup"><span data-stu-id="dfbee-260">Route Names</span></span>

<span data-ttu-id="dfbee-261">En Web API, cada ruta tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="dfbee-261">In Web API, every route has a name.</span></span> <span data-ttu-id="dfbee-262">Los nombres de ruta son útiles para generar vínculos, de modo que pueda incluir un vínculo en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dfbee-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="dfbee-263">Para especificar el nombre de la ruta, establezca la propiedad **Name** en el atributo.</span><span class="sxs-lookup"><span data-stu-id="dfbee-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="dfbee-264">En el ejemplo siguiente se muestra cómo establecer el nombre de ruta y también cómo utilizar el nombre de ruta al generar un vínculo.</span><span class="sxs-lookup"><span data-stu-id="dfbee-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="dfbee-265">Orden de ruta</span><span class="sxs-lookup"><span data-stu-id="dfbee-265">Route Order</span></span>

<span data-ttu-id="dfbee-266">Cuando el marco de trabajo intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden determinado.</span><span class="sxs-lookup"><span data-stu-id="dfbee-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="dfbee-267">Para especificar el orden, establezca la propiedad **Order** en el atributo route.</span><span class="sxs-lookup"><span data-stu-id="dfbee-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="dfbee-268">Los valores más bajos se evalúan primero.</span><span class="sxs-lookup"><span data-stu-id="dfbee-268">Lower values are evaluated first.</span></span> <span data-ttu-id="dfbee-269">El valor de pedido predeterminado es cero.</span><span class="sxs-lookup"><span data-stu-id="dfbee-269">The default order value is zero.</span></span>

<span data-ttu-id="dfbee-270">Así es como se determina el pedido total:</span><span class="sxs-lookup"><span data-stu-id="dfbee-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="dfbee-271">Compare la propiedad **Order** del atributo route.</span><span class="sxs-lookup"><span data-stu-id="dfbee-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="dfbee-272">Observe cada segmento URI en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="dfbee-273">Para cada segmento, ordene de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="dfbee-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="dfbee-274">Segmentos literales.</span><span class="sxs-lookup"><span data-stu-id="dfbee-274">Literal segments.</span></span>
    2. <span data-ttu-id="dfbee-275">Parámetros de ruta con restricciones.</span><span class="sxs-lookup"><span data-stu-id="dfbee-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="dfbee-276">Parámetros de ruta sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="dfbee-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="dfbee-277">Segmentos de parámetros comodín con restricciones.</span><span class="sxs-lookup"><span data-stu-id="dfbee-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="dfbee-278">Segmentos de parámetros comodín sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="dfbee-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="dfbee-279">En el caso de un empate, las rutas se ordenan mediante una comparación de cadenas ordinales sin distinción entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="dfbee-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="dfbee-280">A continuación se muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dfbee-280">Here is an example.</span></span> <span data-ttu-id="dfbee-281">Supongamos que define el controlador siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfbee-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="dfbee-282">Estas rutas se ordenan de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="dfbee-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="dfbee-283">pedidos/detalles</span><span class="sxs-lookup"><span data-stu-id="dfbee-283">orders/details</span></span>
2. <span data-ttu-id="dfbee-284">pedidos/'id'</span><span class="sxs-lookup"><span data-stu-id="dfbee-284">orders/{id}</span></span>
3. <span data-ttu-id="dfbee-285">pedidos/-nombreDeCliente-Nombre-Cliente-Nombre_de_cliente-</span><span class="sxs-lookup"><span data-stu-id="dfbee-285">orders/{customerName}</span></span>
4. <span data-ttu-id="dfbee-286">pedidos/fecha?\*</span><span class="sxs-lookup"><span data-stu-id="dfbee-286">orders/{\*date}</span></span>
5. <span data-ttu-id="dfbee-287">órdenes/pendientes</span><span class="sxs-lookup"><span data-stu-id="dfbee-287">orders/pending</span></span>

<span data-ttu-id="dfbee-288">Tenga en cuenta que "detalles" es un segmento literal y aparece antes de "id", pero "pendiente" aparece en último lugar porque el **Order** propiedad es 1.</span><span class="sxs-lookup"><span data-stu-id="dfbee-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="dfbee-289">(En este ejemplo se supone que no hay clientes denominados "detalles" o "pendientes".</span><span class="sxs-lookup"><span data-stu-id="dfbee-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="dfbee-290">En general, trate de evitar rutas ambiguas.</span><span class="sxs-lookup"><span data-stu-id="dfbee-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="dfbee-291">En este ejemplo, una `GetByCustomer` mejor plantilla de ruta para es "customers/'customerName'" )</span><span class="sxs-lookup"><span data-stu-id="dfbee-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
