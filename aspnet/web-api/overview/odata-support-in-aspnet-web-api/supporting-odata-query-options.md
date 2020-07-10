---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Compatibilidad de las opciones de consulta de OData en ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Información general con ejemplos de código muestra las opciones de consulta de OData compatibles en ASP.NET Web API 2 para ASP.NET 4. x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 96820fab7ac89885058962f44ded86cb0184ee97
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "86188645"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="00103-103">Compatibilidad de las opciones de consulta de OData en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="00103-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="00103-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="00103-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="00103-105">En esta información general sobre los ejemplos de código se muestran las opciones de consulta de OData compatibles en ASP.NET Web API 2 para ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="00103-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="00103-106">OData define los parámetros que se pueden usar para modificar una consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="00103-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="00103-107">El cliente envía estos parámetros en la cadena de consulta del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="00103-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="00103-108">Por ejemplo, para ordenar los resultados, un cliente utiliza el parámetro $orderby:</span><span class="sxs-lookup"><span data-stu-id="00103-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="00103-109">La especificación de OData llama a estas *Opciones de consulta*de parámetros.</span><span class="sxs-lookup"><span data-stu-id="00103-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="00103-110">Puede habilitar las opciones de consulta de OData para cualquier controlador de API Web del proyecto &#8212; el controlador no necesita ser un punto de conexión de OData.</span><span class="sxs-lookup"><span data-stu-id="00103-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="00103-111">Esto le ofrece una manera cómoda de agregar características como el filtrado y la ordenación de cualquier aplicación de API Web.</span><span class="sxs-lookup"><span data-stu-id="00103-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="00103-112">Antes de habilitar las opciones de consulta, lea el tema [Guía de seguridad de oData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="00103-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="00103-113">Habilitar opciones de consulta de OData</span><span class="sxs-lookup"><span data-stu-id="00103-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="00103-114">Consultas de ejemplo</span><span class="sxs-lookup"><span data-stu-id="00103-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="00103-115">Paginación controlada por servidor</span><span class="sxs-lookup"><span data-stu-id="00103-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="00103-116">Limitar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="00103-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="00103-117">Invocar directamente las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="00103-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="00103-118">Validación de consultas</span><span class="sxs-lookup"><span data-stu-id="00103-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="00103-119">Habilitar opciones de consulta de OData</span><span class="sxs-lookup"><span data-stu-id="00103-119">Enabling OData Query Options</span></span>

<span data-ttu-id="00103-120">Web API admite las siguientes opciones de consulta de OData:</span><span class="sxs-lookup"><span data-stu-id="00103-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="00103-121">Opción</span><span class="sxs-lookup"><span data-stu-id="00103-121">Option</span></span> | <span data-ttu-id="00103-122">Descripción</span><span class="sxs-lookup"><span data-stu-id="00103-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="00103-123">$expand</span><span class="sxs-lookup"><span data-stu-id="00103-123">$expand</span></span> | <span data-ttu-id="00103-124">Expande las entidades relacionadas en línea.</span><span class="sxs-lookup"><span data-stu-id="00103-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="00103-125">$filter</span><span class="sxs-lookup"><span data-stu-id="00103-125">$filter</span></span> | <span data-ttu-id="00103-126">Filtra los resultados, en función de una condición booleana.</span><span class="sxs-lookup"><span data-stu-id="00103-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="00103-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="00103-127">$inlinecount</span></span> | <span data-ttu-id="00103-128">Indica al servidor que incluya el recuento total de entidades coincidentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="00103-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="00103-129">(Útil para la paginación del lado servidor).</span><span class="sxs-lookup"><span data-stu-id="00103-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="00103-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="00103-130">$orderby</span></span> | <span data-ttu-id="00103-131">Ordena los resultados.</span><span class="sxs-lookup"><span data-stu-id="00103-131">Sorts the results.</span></span> |
| <span data-ttu-id="00103-132">$select</span><span class="sxs-lookup"><span data-stu-id="00103-132">$select</span></span> | <span data-ttu-id="00103-133">Selecciona las propiedades que se van a incluir en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="00103-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="00103-134">$skip</span><span class="sxs-lookup"><span data-stu-id="00103-134">$skip</span></span> | <span data-ttu-id="00103-135">Omite los primeros n resultados.</span><span class="sxs-lookup"><span data-stu-id="00103-135">Skips the first n results.</span></span> |
| <span data-ttu-id="00103-136">$top</span><span class="sxs-lookup"><span data-stu-id="00103-136">$top</span></span> | <span data-ttu-id="00103-137">Devuelve solo los primeros n resultados.</span><span class="sxs-lookup"><span data-stu-id="00103-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="00103-138">Para usar las opciones de consulta de OData, debe habilitarlas explícitamente.</span><span class="sxs-lookup"><span data-stu-id="00103-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="00103-139">Puede habilitarlas globalmente para toda la aplicación o habilitarlas para determinados controladores o acciones específicas.</span><span class="sxs-lookup"><span data-stu-id="00103-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="00103-140">Para habilitar las opciones de consulta de OData globalmente, llame a **EnableQuerySupport** en la clase **HttpConfiguration** en el inicio:</span><span class="sxs-lookup"><span data-stu-id="00103-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="00103-141">El método **EnableQuerySupport** habilita las opciones de consulta globalmente para cualquier acción de controlador que devuelve un tipo **IQueryable** .</span><span class="sxs-lookup"><span data-stu-id="00103-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="00103-142">Si no desea que las opciones de consulta estén habilitadas para toda la aplicación, puede habilitarlas para acciones específicas del controlador agregando el atributo **[Queryable]** al método de acción.</span><span class="sxs-lookup"><span data-stu-id="00103-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="00103-143">Consultas de ejemplo</span><span class="sxs-lookup"><span data-stu-id="00103-143">Example Queries</span></span>

<span data-ttu-id="00103-144">En esta sección se muestran los tipos de consultas que son posibles mediante las opciones de consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="00103-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="00103-145">Para obtener detalles específicos sobre las opciones de consulta, consulte la documentación de OData en [www.OData.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="00103-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="00103-146">Para obtener información sobre $expand y $select, vea [usar $SELECT, $Expand y $Value en ASP.net web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="00103-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="00103-147">**Paginación controlada por el cliente**</span><span class="sxs-lookup"><span data-stu-id="00103-147">**Client-Driven Paging**</span></span>

<span data-ttu-id="00103-148">En el caso de los conjuntos de entidades de gran tamaño, es posible que el cliente desee limitar el número de resultados.</span><span class="sxs-lookup"><span data-stu-id="00103-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="00103-149">Por ejemplo, un cliente podría mostrar 10 entradas a la vez, con vínculos "siguiente" para obtener la página siguiente de resultados.</span><span class="sxs-lookup"><span data-stu-id="00103-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="00103-150">Para ello, el cliente usa las opciones $top y $skip.</span><span class="sxs-lookup"><span data-stu-id="00103-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="00103-151">La opción $top proporciona el número máximo de entradas que se van a devolver y la opción $skip proporciona el número de entradas que se van a omitir.</span><span class="sxs-lookup"><span data-stu-id="00103-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="00103-152">En el ejemplo anterior se capturan las entradas 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="00103-152">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="00103-153">**Filtrado**</span><span class="sxs-lookup"><span data-stu-id="00103-153">**Filtering**</span></span>

<span data-ttu-id="00103-154">La opción $filter permite que un cliente filtre los resultados aplicando una expresión booleana.</span><span class="sxs-lookup"><span data-stu-id="00103-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="00103-155">Las expresiones de filtro son bastante eficaces; incluyen operadores lógicos y aritméticos, funciones de cadena y funciones de fecha.</span><span class="sxs-lookup"><span data-stu-id="00103-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="00103-156">Devuelve todos los productos con una categoría igual a "Toys".</span><span class="sxs-lookup"><span data-stu-id="00103-156">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="00103-157">`http://localhost/Products?$filter=Category`EC ' juguetes '</span><span class="sxs-lookup"><span data-stu-id="00103-157">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="00103-158">Devolver todos los productos con un precio inferior a 10.</span><span class="sxs-lookup"><span data-stu-id="00103-158">Return all products with price less than 10.</span></span> | <span data-ttu-id="00103-159">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="00103-159">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="00103-160">Operadores lógicos: devuelve todos los productos en los que el precio >= 5 y el precio <= 15.</span><span class="sxs-lookup"><span data-stu-id="00103-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="00103-161">`http://localhost/Products?$filter=Price`GE 5 y precio le 15</span><span class="sxs-lookup"><span data-stu-id="00103-161">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="00103-162">Funciones de cadena: devuelve todos los productos con "ZZ" en el nombre.</span><span class="sxs-lookup"><span data-stu-id="00103-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="00103-163">Funciones de fecha: devuelve todos los productos con ReleaseDate después de 2005.</span><span class="sxs-lookup"><span data-stu-id="00103-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="00103-164">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="00103-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="00103-165">**Ordenar**</span><span class="sxs-lookup"><span data-stu-id="00103-165">**Sorting**</span></span>

<span data-ttu-id="00103-166">Para ordenar los resultados, use el $orderby filtro.</span><span class="sxs-lookup"><span data-stu-id="00103-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="00103-167">Ordenar por precio.</span><span class="sxs-lookup"><span data-stu-id="00103-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="00103-168">Ordenar por precio en orden descendente (de mayor a menor).</span><span class="sxs-lookup"><span data-stu-id="00103-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="00103-169">Ordene por categoría y, a continuación, ordene por precio en orden descendente dentro de las categorías.</span><span class="sxs-lookup"><span data-stu-id="00103-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="00103-170">Paginación controlada por servidor</span><span class="sxs-lookup"><span data-stu-id="00103-170">Server-Driven Paging</span></span>

<span data-ttu-id="00103-171">Si la base de datos contiene millones de registros, no desea enviarlos todos en una carga.</span><span class="sxs-lookup"><span data-stu-id="00103-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="00103-172">Para evitar esto, el servidor puede limitar el número de entradas que envía en una sola respuesta.</span><span class="sxs-lookup"><span data-stu-id="00103-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="00103-173">Para habilitar la paginación del servidor, establezca la propiedad **pageSize** en el atributo **consultable** .</span><span class="sxs-lookup"><span data-stu-id="00103-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="00103-174">El valor es el número máximo de entradas que se van a devolver.</span><span class="sxs-lookup"><span data-stu-id="00103-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="00103-175">Si el controlador devuelve el formato OData, el cuerpo de la respuesta contendrá un vínculo a la siguiente página de datos:</span><span class="sxs-lookup"><span data-stu-id="00103-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="00103-176">El cliente puede usar este vínculo para capturar la página siguiente.</span><span class="sxs-lookup"><span data-stu-id="00103-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="00103-177">Para obtener información sobre el número total de entradas del conjunto de resultados, el cliente puede establecer la opción de consulta $inlinecount con el valor "Allpages".</span><span class="sxs-lookup"><span data-stu-id="00103-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="00103-178">El valor "Allpages" indica al servidor que incluya el recuento total de la respuesta:</span><span class="sxs-lookup"><span data-stu-id="00103-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="00103-179">Los vínculos de página siguiente y el recuento en línea requieren el formato OData.</span><span class="sxs-lookup"><span data-stu-id="00103-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="00103-180">La razón es que OData define campos especiales en el cuerpo de la respuesta para que contengan el vínculo y el recuento.</span><span class="sxs-lookup"><span data-stu-id="00103-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>

<span data-ttu-id="00103-181">En el caso de los formatos que no son de OData, sigue siendo posible admitir los vínculos de página siguiente y el recuento alineado, ajustando los resultados de la consulta en un objeto \*\*PageResult &lt; T &gt; \*\* .</span><span class="sxs-lookup"><span data-stu-id="00103-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="00103-182">Sin embargo, requiere un poco más de código.</span><span class="sxs-lookup"><span data-stu-id="00103-182">However, it requires a bit more code.</span></span> <span data-ttu-id="00103-183">Este es un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="00103-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="00103-184">A continuación se muestra una respuesta JSON de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="00103-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="00103-185">Limitar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="00103-185">Limiting the Query Options</span></span>

<span data-ttu-id="00103-186">Las opciones de consulta proporcionan al cliente un gran control sobre la consulta que se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="00103-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="00103-187">En algunos casos, puede que desee limitar las opciones disponibles por motivos de seguridad o de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="00103-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="00103-188">El atributo **[Queryable]** tiene algunas propiedades integradas para este.</span><span class="sxs-lookup"><span data-stu-id="00103-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="00103-189">Estos son algunos ejemplos.</span><span class="sxs-lookup"><span data-stu-id="00103-189">Here are some examples.</span></span>

<span data-ttu-id="00103-190">Permitir solo $skip y $top, para admitir la paginación y nada más:</span><span class="sxs-lookup"><span data-stu-id="00103-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="00103-191">Permitir el orden solo por determinadas propiedades, para evitar la ordenación en las propiedades que no están indizadas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="00103-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="00103-192">Permitir la función lógica "EQ", pero no otras funciones lógicas:</span><span class="sxs-lookup"><span data-stu-id="00103-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="00103-193">No permita ningún operador aritmético:</span><span class="sxs-lookup"><span data-stu-id="00103-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="00103-194">Puede restringir las opciones globalmente mediante la construcción de una instancia de **QueryableAttribute** y pasándola a la función **EnableQuerySupport** :</span><span class="sxs-lookup"><span data-stu-id="00103-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="00103-195">Invocar directamente las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="00103-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="00103-196">En lugar de usar el atributo **[Queryable]** , puede invocar las opciones de consulta directamente en el controlador.</span><span class="sxs-lookup"><span data-stu-id="00103-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="00103-197">Para ello, agregue un parámetro **ODataQueryOptions** al método de controlador.</span><span class="sxs-lookup"><span data-stu-id="00103-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="00103-198">En este caso, no se necesita el atributo **[Queryable]** .</span><span class="sxs-lookup"><span data-stu-id="00103-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="00103-199">Web API rellena el **ODataQueryOptions** de la cadena de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="00103-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="00103-200">Para aplicar la consulta, pase un **IQueryable** al método **Applyto** .</span><span class="sxs-lookup"><span data-stu-id="00103-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="00103-201">El método devuelve otro **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="00103-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="00103-202">En el caso de escenarios avanzados, si no tiene un proveedor de consultas **IQueryable** , puede examinar el **ODataQueryOptions** y trasladar las opciones de consulta a otro formato.</span><span class="sxs-lookup"><span data-stu-id="00103-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="00103-203">(Por ejemplo, consulte la entrada de blog de RaghuRam Nadiminti [traducir consultas de oData a HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que también incluye un [ejemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt)).</span><span class="sxs-lookup"><span data-stu-id="00103-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="00103-204">Validación de consultas</span><span class="sxs-lookup"><span data-stu-id="00103-204">Query Validation</span></span>

<span data-ttu-id="00103-205">El atributo **[Queryable]** valida la consulta antes de ejecutarla.</span><span class="sxs-lookup"><span data-stu-id="00103-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="00103-206">El paso de validación se realiza en el método **QueryableAttribute. ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="00103-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="00103-207">También puede personalizar el proceso de validación.</span><span class="sxs-lookup"><span data-stu-id="00103-207">You can also customize the validation process.</span></span>

<span data-ttu-id="00103-208">Consulte también la [Guía de seguridad de oData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="00103-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="00103-209">En primer lugar, invalide una de las clases de validador que se define en el espacio de nombres **Web. http. OData. Query. validations** .</span><span class="sxs-lookup"><span data-stu-id="00103-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="00103-210">Por ejemplo, la siguiente clase de validador deshabilita la opción ' DESC ' para la opción $orderby.</span><span class="sxs-lookup"><span data-stu-id="00103-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="00103-211">Subclase del atributo **[Queryable]** para invalidar el método **ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="00103-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="00103-212">Después, establezca el atributo personalizado globalmente o por controlador:</span><span class="sxs-lookup"><span data-stu-id="00103-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="00103-213">Si usa **ODataQueryOptions** directamente, establezca el validador en las opciones:</span><span class="sxs-lookup"><span data-stu-id="00103-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
