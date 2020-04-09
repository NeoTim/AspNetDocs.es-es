---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento en ASP.NET Web API ( Web API) Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675758"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="27351-102">Enrutar en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="27351-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="27351-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="27351-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="27351-104">En este artículo se describe cómo ASP.NET API web enruta las solicitudes HTTP a los controladores.</span><span class="sxs-lookup"><span data-stu-id="27351-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="27351-105">Si está familiarizado con ASP.NET MVC, el enrutamiento de API web es muy similar al enrutamiento MVC.</span><span class="sxs-lookup"><span data-stu-id="27351-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="27351-106">La principal diferencia es que Web API utiliza el verbo HTTP, no la ruta de acceso URI, para seleccionar la acción.</span><span class="sxs-lookup"><span data-stu-id="27351-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="27351-107">También puede usar el enrutamiento de estilo MVC en Web API.</span><span class="sxs-lookup"><span data-stu-id="27351-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="27351-108">Este artículo no asume ningún conocimiento de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27351-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="27351-109">Tablas de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="27351-109">Routing Tables</span></span>

<span data-ttu-id="27351-110">En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="27351-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="27351-111">Los métodos públicos del controlador se denominan *métodos* de acción o simplemente *acciones.*</span><span class="sxs-lookup"><span data-stu-id="27351-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="27351-112">Cuando el marco de la API web recibe una solicitud, enruta la solicitud a una acción.</span><span class="sxs-lookup"><span data-stu-id="27351-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="27351-113">Para determinar qué acción invocar, el marco de trabajo utiliza una tabla de *enrutamiento.*</span><span class="sxs-lookup"><span data-stu-id="27351-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="27351-114">La plantilla de proyecto de Visual Studio para Web API crea una ruta predeterminada:</span><span class="sxs-lookup"><span data-stu-id="27351-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="27351-115">Esta ruta se define en el archivo *WebApiConfig.cs,* que se coloca en el directorio *\_App Start:*</span><span class="sxs-lookup"><span data-stu-id="27351-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="27351-116">Para obtener más `WebApiConfig` información acerca de la clase, consulte [Configuración de ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="27351-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="27351-117">Si autohospeda la API web, debe establecer la `HttpSelfHostConfiguration` tabla de enrutamiento directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="27351-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="27351-118">Para obtener más información, consulte [Auto-Host a Web API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="27351-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="27351-119">Cada entrada de la tabla de ruteo contiene una plantilla de *ruta.*</span><span class="sxs-lookup"><span data-stu-id="27351-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="27351-120">La plantilla de ruta &quot;predeterminada para la API web&quot;es api/-controller-/-id- .</span><span class="sxs-lookup"><span data-stu-id="27351-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="27351-121">En esta &quot;plantilla, api&quot; es un segmento de ruta de acceso literal, y los parámetros de marcador de posición son variables de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="27351-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="27351-122">Cuando el marco de la API web recibe una solicitud HTTP, intenta hacer coincidir el URI con una de las plantillas de ruta de la tabla de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="27351-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="27351-123">Si ninguna ruta coincide, el cliente recibe un error 404.</span><span class="sxs-lookup"><span data-stu-id="27351-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="27351-124">Por ejemplo, los siguientes URI coinciden con la ruta predeterminada:</span><span class="sxs-lookup"><span data-stu-id="27351-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="27351-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="27351-125">/api/contacts</span></span>
- <span data-ttu-id="27351-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="27351-126">/api/contacts/1</span></span>
- <span data-ttu-id="27351-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="27351-127">/api/products/gizmo1</span></span>

<span data-ttu-id="27351-128">Sin embargo, el URI siguiente no &quot;coincide, porque carece del segmento api:&quot;</span><span class="sxs-lookup"><span data-stu-id="27351-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="27351-129">/contactos/1</span><span class="sxs-lookup"><span data-stu-id="27351-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="27351-130">La razón para usar "api" en la ruta es evitar colisiones con ASP.NET el enrutamiento MVC.</span><span class="sxs-lookup"><span data-stu-id="27351-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="27351-131">De este modo, &quot;puede&quot; hacer que /contacts &quot;vaya a&quot; un controlador MVC y /api/contacts vaya a un controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="27351-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="27351-132">Por supuesto, si no le gusta esta convención, puede cambiar la tabla de rutas predeterminada.</span><span class="sxs-lookup"><span data-stu-id="27351-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="27351-133">Una vez que se encuentra una ruta coincidente, Web API selecciona el controlador y la acción:</span><span class="sxs-lookup"><span data-stu-id="27351-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="27351-134">Para encontrar el controlador, &quot;la&quot; API web agrega Controller al valor de la variable *.*</span><span class="sxs-lookup"><span data-stu-id="27351-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="27351-135">Para buscar la acción, Web API examina el verbo HTTP y, a continuación, busca una acción cuyo nombre comience con ese nombre de verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="27351-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="27351-136">Por ejemplo, con una solicitud GET, Web API &quot;&quot;busca una &quot;acción&quot; &quot;con el&quot;prefijo Get , como GetContact o GetAllContacts .</span><span class="sxs-lookup"><span data-stu-id="27351-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="27351-137">Esta convención solo se aplica a los verbos GET, POST, PUT, DELETE, HEAD, OPTIONS y PATCH.</span><span class="sxs-lookup"><span data-stu-id="27351-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="27351-138">Puede habilitar otros verbos HTTP mediante atributos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="27351-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="27351-139">Veremos un ejemplo de eso más tarde.</span><span class="sxs-lookup"><span data-stu-id="27351-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="27351-140">Otras variables de marcador de posición en la plantilla de ruta, como, por *ejemplo, el identificador,* se asignan a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="27351-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="27351-141">Veamos un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="27351-141">Let's look at an example.</span></span> <span data-ttu-id="27351-142">Supongamos que define el controlador siguiente:</span><span class="sxs-lookup"><span data-stu-id="27351-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="27351-143">Aquí están algunas posibles peticiones HTTP, junto con la acción que consigue invocada para cada uno:</span><span class="sxs-lookup"><span data-stu-id="27351-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="27351-144">Verbo HTTP</span><span class="sxs-lookup"><span data-stu-id="27351-144">HTTP Verb</span></span> | <span data-ttu-id="27351-145">Ruta de URI</span><span class="sxs-lookup"><span data-stu-id="27351-145">URI Path</span></span> | <span data-ttu-id="27351-146">Acción</span><span class="sxs-lookup"><span data-stu-id="27351-146">Action</span></span> | <span data-ttu-id="27351-147">Parámetro</span><span class="sxs-lookup"><span data-stu-id="27351-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="27351-148">GET</span><span class="sxs-lookup"><span data-stu-id="27351-148">GET</span></span> | <span data-ttu-id="27351-149">api/productos</span><span class="sxs-lookup"><span data-stu-id="27351-149">api/products</span></span> | <span data-ttu-id="27351-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="27351-150">GetAllProducts</span></span> | <span data-ttu-id="27351-151">*(ninguno)*</span><span class="sxs-lookup"><span data-stu-id="27351-151">*(none)*</span></span> |
| <span data-ttu-id="27351-152">GET</span><span class="sxs-lookup"><span data-stu-id="27351-152">GET</span></span> | <span data-ttu-id="27351-153">api/productos/4</span><span class="sxs-lookup"><span data-stu-id="27351-153">api/products/4</span></span> | <span data-ttu-id="27351-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="27351-154">GetProductById</span></span> | <span data-ttu-id="27351-155">4</span><span class="sxs-lookup"><span data-stu-id="27351-155">4</span></span> |
| <span data-ttu-id="27351-156">Delete</span><span class="sxs-lookup"><span data-stu-id="27351-156">DELETE</span></span> | <span data-ttu-id="27351-157">api/productos/4</span><span class="sxs-lookup"><span data-stu-id="27351-157">api/products/4</span></span> | <span data-ttu-id="27351-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="27351-158">DeleteProduct</span></span> | <span data-ttu-id="27351-159">4</span><span class="sxs-lookup"><span data-stu-id="27351-159">4</span></span> |
| <span data-ttu-id="27351-160">POST</span><span class="sxs-lookup"><span data-stu-id="27351-160">POST</span></span> | <span data-ttu-id="27351-161">api/productos</span><span class="sxs-lookup"><span data-stu-id="27351-161">api/products</span></span> | <span data-ttu-id="27351-162">*(sin coincidencia)*</span><span class="sxs-lookup"><span data-stu-id="27351-162">*(no match)*</span></span> |  |

<span data-ttu-id="27351-163">Tenga en cuenta que *el* segmento de la URI, si está presente, se asigna al parámetro *id* de la acción.</span><span class="sxs-lookup"><span data-stu-id="27351-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="27351-164">En este ejemplo, el controlador define dos métodos GET, uno con un parámetro *id* y otro sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="27351-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="27351-165">Además, tenga en cuenta que la solicitud &quot;POST fallará, porque el controlador no define un Post... &quot; método.</span><span class="sxs-lookup"><span data-stu-id="27351-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="27351-166">Variaciones de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="27351-166">Routing Variations</span></span>

<span data-ttu-id="27351-167">En la sección anterior se describe el mecanismo de enrutamiento básico para ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="27351-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="27351-168">En esta sección se describen algunas variaciones.</span><span class="sxs-lookup"><span data-stu-id="27351-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="27351-169">Verbos HTTP</span><span class="sxs-lookup"><span data-stu-id="27351-169">HTTP verbs</span></span>

<span data-ttu-id="27351-170">En lugar de utilizar la convención de nomenclatura para los verbos HTTP, puede especificar explícitamente el verbo HTTP para una acción decorando el método de acción con uno de los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="27351-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="27351-171">En el ejemplo `FindProduct` siguiente, el método se asigna a las solicitudes GET:</span><span class="sxs-lookup"><span data-stu-id="27351-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="27351-172">Para permitir varios verbos HTTP para una acción o para permitir verbos HTTP distintos de GET, `[AcceptVerbs]` PUT, POST, DELETE, HEAD, OPTIONS y PATCH, utilice el atributo, que toma una lista de verbos HTTP.</span><span class="sxs-lookup"><span data-stu-id="27351-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="27351-173">Enrutamiento por nombre de acción</span><span class="sxs-lookup"><span data-stu-id="27351-173">Routing by Action Name</span></span>

<span data-ttu-id="27351-174">Con la plantilla de enrutamiento predeterminada, Web API utiliza el verbo HTTP para seleccionar la acción.</span><span class="sxs-lookup"><span data-stu-id="27351-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="27351-175">Sin embargo, también puede crear una ruta donde el nombre de la acción se incluya en el URI:</span><span class="sxs-lookup"><span data-stu-id="27351-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="27351-176">En esta plantilla de ruta, el parámetro *.action ,* nombra el método de acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="27351-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="27351-177">Con este estilo de enrutamiento, utilice atributos para especificar los verbos HTTP permitidos.</span><span class="sxs-lookup"><span data-stu-id="27351-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="27351-178">Por ejemplo, supongamos que el controlador tiene el siguiente método:</span><span class="sxs-lookup"><span data-stu-id="27351-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="27351-179">En este caso, una solicitud GET para "api/products/details/1" se asignaría al `Details` método.</span><span class="sxs-lookup"><span data-stu-id="27351-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="27351-180">Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.</span><span class="sxs-lookup"><span data-stu-id="27351-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="27351-181">Puede invalidar el nombre de `[ActionName]` la acción mediante el atributo.</span><span class="sxs-lookup"><span data-stu-id="27351-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="27351-182">En el ejemplo siguiente, hay dos &quot;acciones que se asignan a api/products/thumbnail/*id*. Uno es compatible con GET y el otro es post:</span><span class="sxs-lookup"><span data-stu-id="27351-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="27351-183">No acciones</span><span class="sxs-lookup"><span data-stu-id="27351-183">Non-Actions</span></span>

<span data-ttu-id="27351-184">Para evitar que un método se invoque como una acción, utilice el `[NonAction]` atributo.</span><span class="sxs-lookup"><span data-stu-id="27351-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="27351-185">Esto indica al marco de trabajo que el método no es una acción, incluso si de lo contrario coincidiría con las reglas de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="27351-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="27351-186">Lecturas adicionales</span><span class="sxs-lookup"><span data-stu-id="27351-186">Further Reading</span></span>

<span data-ttu-id="27351-187">En este tema se proporciona una vista de alto nivel del enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="27351-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="27351-188">Para obtener más detalles, vea Selección de [enrutamiento y acción](routing-and-action-selection.md), que describe exactamente cómo el marco de trabajo hace coincidir un URI con una ruta, selecciona un controlador y, a continuación, selecciona la acción que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="27351-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
