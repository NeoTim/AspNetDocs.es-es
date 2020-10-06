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
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "86188645"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Compatibilidad de las opciones de consulta de OData en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

En esta información general sobre los ejemplos de código se muestran las opciones de consulta de OData compatibles en ASP.NET Web API 2 para ASP.NET 4. x. 

OData define los parámetros que se pueden usar para modificar una consulta de OData. El cliente envía estos parámetros en la cadena de consulta del URI de solicitud. Por ejemplo, para ordenar los resultados, un cliente utiliza el parámetro $orderby:

`http://localhost/Products?$orderby=Name`

La especificación de OData llama a estas *Opciones de consulta*de parámetros. Puede habilitar las opciones de consulta de OData para cualquier controlador de API Web del proyecto &#8212; el controlador no necesita ser un punto de conexión de OData. Esto le ofrece una manera cómoda de agregar características como el filtrado y la ordenación de cualquier aplicación de API Web.

Antes de habilitar las opciones de consulta, lea el tema [Guía de seguridad de oData](odata-security-guidance.md).

- [Habilitar opciones de consulta de OData](#enable)
- [Consultas de ejemplo](#examples)
- [Paginación controlada por servidor](#server-paging)
- [Limitar las opciones de consulta](#limiting_query_options)
- [Invocar directamente las opciones de consulta](#ODataQueryOptions)
- [Validación de consultas](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Habilitar opciones de consulta de OData

Web API admite las siguientes opciones de consulta de OData:

| Opción | Descripción |
| --- | --- |
| $expand | Expande las entidades relacionadas en línea. |
| $filter | Filtra los resultados, en función de una condición booleana. |
| $inlinecount | Indica al servidor que incluya el recuento total de entidades coincidentes en la respuesta. (Útil para la paginación del lado servidor). |
| $orderby | Ordena los resultados. |
| $select | Selecciona las propiedades que se van a incluir en la respuesta. |
| $skip | Omite los primeros n resultados. |
| $top | Devuelve solo los primeros n resultados. |

Para usar las opciones de consulta de OData, debe habilitarlas explícitamente. Puede habilitarlas globalmente para toda la aplicación o habilitarlas para determinados controladores o acciones específicas.

Para habilitar las opciones de consulta de OData globalmente, llame a **EnableQuerySupport** en la clase **HttpConfiguration** en el inicio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

El método **EnableQuerySupport** habilita las opciones de consulta globalmente para cualquier acción de controlador que devuelve un tipo **IQueryable** . Si no desea que las opciones de consulta estén habilitadas para toda la aplicación, puede habilitarlas para acciones específicas del controlador agregando el atributo **[Queryable]** al método de acción.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Consultas de ejemplo

En esta sección se muestran los tipos de consultas que son posibles mediante las opciones de consulta de OData. Para obtener detalles específicos sobre las opciones de consulta, consulte la documentación de OData en [www.OData.org](http://www.odata.org/).

Para obtener información sobre $expand y $select, vea [usar $SELECT, $Expand y $Value en ASP.net web API OData](using-select-expand-and-value.md).

**Paginación controlada por el cliente**

En el caso de los conjuntos de entidades de gran tamaño, es posible que el cliente desee limitar el número de resultados. Por ejemplo, un cliente podría mostrar 10 entradas a la vez, con vínculos "siguiente" para obtener la página siguiente de resultados. Para ello, el cliente usa las opciones $top y $skip.

`http://localhost/Products?$top=10&$skip=20`

La opción $top proporciona el número máximo de entradas que se van a devolver y la opción $skip proporciona el número de entradas que se van a omitir. En el ejemplo anterior se capturan las entradas 21 a 30.

**Filtros**

La opción $filter permite que un cliente filtre los resultados aplicando una expresión booleana. Las expresiones de filtro son bastante eficaces; incluyen operadores lógicos y aritméticos, funciones de cadena y funciones de fecha.

| Devuelve todos los productos con una categoría igual a "Toys". | `http://localhost/Products?$filter=Category` EC ' juguetes ' |
| --- | --- |
| Devolver todos los productos con un precio inferior a 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Operadores lógicos: devuelve todos los productos en los que el precio >= 5 y el precio <= 15. | `http://localhost/Products?$filter=Price` GE 5 y precio le 15 |
| Funciones de cadena: devuelve todos los productos con "ZZ" en el nombre. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funciones de fecha: devuelve todos los productos con ReleaseDate después de 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Ordenar**

Para ordenar los resultados, use el $orderby filtro.

| Ordenar por precio. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Ordenar por precio en orden descendente (de mayor a menor). | `http://localhost/Products?$orderby=Price desc` |
| Ordene por categoría y, a continuación, ordene por precio en orden descendente dentro de las categorías. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paginación controlada por servidor

Si la base de datos contiene millones de registros, no desea enviarlos todos en una carga. Para evitar esto, el servidor puede limitar el número de entradas que envía en una sola respuesta. Para habilitar la paginación del servidor, establezca la propiedad **pageSize** en el atributo **consultable** . El valor es el número máximo de entradas que se van a devolver.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Si el controlador devuelve el formato OData, el cuerpo de la respuesta contendrá un vínculo a la siguiente página de datos:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

El cliente puede usar este vínculo para capturar la página siguiente. Para obtener información sobre el número total de entradas del conjunto de resultados, el cliente puede establecer la opción de consulta $inlinecount con el valor "Allpages".

`http://localhost/Products?$inlinecount=allpages`

El valor "Allpages" indica al servidor que incluya el recuento total de la respuesta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Los vínculos de página siguiente y el recuento en línea requieren el formato OData. La razón es que OData define campos especiales en el cuerpo de la respuesta para que contengan el vínculo y el recuento.

En el caso de los formatos que no son de OData, sigue siendo posible admitir los vínculos de página siguiente y el recuento alineado, ajustando los resultados de la consulta en un objeto **PageResult &lt; T &gt; ** . Sin embargo, requiere un poco más de código. Este es un ejemplo:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

A continuación se muestra una respuesta JSON de ejemplo:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitar las opciones de consulta

Las opciones de consulta proporcionan al cliente un gran control sobre la consulta que se ejecuta en el servidor. En algunos casos, puede que desee limitar las opciones disponibles por motivos de seguridad o de rendimiento. El atributo **[Queryable]** tiene algunas propiedades integradas para este. Estos son algunos ejemplos.

Permitir solo $skip y $top, para admitir la paginación y nada más:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Permitir el orden solo por determinadas propiedades, para evitar la ordenación en las propiedades que no están indizadas en la base de datos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Permitir la función lógica "EQ", pero no otras funciones lógicas:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

No permita ningún operador aritmético:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Puede restringir las opciones globalmente mediante la construcción de una instancia de **QueryableAttribute** y pasándola a la función **EnableQuerySupport** :

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Invocar directamente las opciones de consulta

En lugar de usar el atributo **[Queryable]** , puede invocar las opciones de consulta directamente en el controlador. Para ello, agregue un parámetro **ODataQueryOptions** al método de controlador. En este caso, no se necesita el atributo **[Queryable]** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API rellena el **ODataQueryOptions** de la cadena de consulta de URI. Para aplicar la consulta, pase un **IQueryable** al método **Applyto** . El método devuelve otro **IQueryable**.

En el caso de escenarios avanzados, si no tiene un proveedor de consultas **IQueryable** , puede examinar el **ODataQueryOptions** y trasladar las opciones de consulta a otro formato. (Por ejemplo, consulte la entrada de blog de RaghuRam Nadiminti [traducir consultas de oData a HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que también incluye un [ejemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt)).

<a id="query-validation"></a>
## <a name="query-validation"></a>Validación de consultas

El atributo **[Queryable]** valida la consulta antes de ejecutarla. El paso de validación se realiza en el método **QueryableAttribute. ValidateQuery** . También puede personalizar el proceso de validación.

Consulte también la [Guía de seguridad de oData](odata-security-guidance.md).

En primer lugar, invalide una de las clases de validador que se define en el espacio de nombres **Web. http. OData. Query. validations** . Por ejemplo, la siguiente clase de validador deshabilita la opción ' DESC ' para la opción $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Subclase del atributo **[Queryable]** para invalidar el método **ValidateQuery** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Después, establezca el atributo personalizado globalmente o por controlador:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Si usa **ODataQueryOptions** directamente, establezca el validador en las opciones:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
