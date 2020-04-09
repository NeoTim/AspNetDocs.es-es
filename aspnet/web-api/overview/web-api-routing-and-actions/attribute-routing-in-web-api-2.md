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
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Enrutamiento de atributos en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

*Enrutamiento* es cómo Web API hace coincidir un URI con una acción. Web API 2 admite un nuevo tipo de enrutamiento, denominado enrutamiento de *atributos.* Como su nombre indica, el enrutamiento de atributos utiliza atributos para definir rutas. El enrutamiento de atributos proporciona más control sobre los URI de la API web. Por ejemplo, puede crear fácilmente URI que describan jerarquías de recursos.

El estilo anterior de enrutamiento, denominado enrutamiento basado en convenciones, sigue siendo totalmente compatible. De hecho, puede combinar ambas técnicas en el mismo proyecto.

En este tema se muestra cómo habilitar el enrutamiento de atributos y se describen las distintas opciones para el enrutamiento de atributos. Para ver un tutorial de extremo a extremo que usa el enrutamiento de atributos, consulte Crear una API REST con enrutamiento de [atributos en Web API 2.](create-a-rest-api-with-attribute-routing.md)

## <a name="prerequisites"></a>Prerrequisitos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Edición Community, Professional o Enterprise

Como alternativa, use nuGet Package Manager para instalar los paquetes necesarios. En el menú **Herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione Consola del Administrador de **paquetes**. Escriba el siguiente comando en la ventana Consola del Administrador de paquetes:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>¿Por qué enrutamiento de atributos?

La primera versión de Web API utiliza el enrutamiento *basado en convenciones.* En ese tipo de enrutamiento, se definen una o varias plantillas de ruta, que son básicamente cadenas parametrizadas. Cuando el marco de trabajo recibe una solicitud, coincide con el URI con la plantilla de ruta. Para obtener más información sobre el enrutamiento basado en convenciones, consulte [Enrutamiento en ASP.NET API web](routing-in-aspnet-web-api.md).

Una ventaja del enrutamiento basado en convenciones es que las plantillas se definen en un solo lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores. Desafortunadamente, el enrutamiento basado en convenciones dificulta la compatibilidad con determinados patrones de URI que son comunes en las API RESTful. Por ejemplo, los recursos a menudo contienen recursos secundarios: los clientes tienen pedidos, las películas tienen actores, los libros tienen autores, etc. Es natural crear URI que reflejen estas relaciones:

`/customers/1/orders`

Este tipo de URI es difícil de crear mediante el enrutamiento basado en convenciones. Aunque se puede hacer, los resultados no se escalan bien si tiene muchos controladores o tipos de recursos.

Con el enrutamiento de atributos, es trivial definir una ruta para este URI. Simplemente agregue un atributo a la acción del controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Estos son algunos otros patrones que el enrutamiento de atributos facilita.

**Control de versiones de la API**

En este ejemplo, "/api/v1/products" se enrutaría a un controlador diferente de "/api/v2/products".

`/api/v1/products`
`/api/v2/products`

**Segmentos URI sobrecargados**

En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.

`/orders/1`
`/orders/pending`

**Múltiples tipos de parámetros**

En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Habilitación del enrutamiento de atributos

Para habilitar el enrutamiento de atributos, llame a **MapHttpAttributeRoutes** durante la configuración. Este método de extensión se define en la clase **System.Web.Http.HttpConfigurationExtensions.**

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

El enrutamiento de atributos se puede combinar con el enrutamiento [basado en convenciones.](routing-in-aspnet-web-api.md) Para definir rutas basadas en convenciones, llame al método **MapHttpRoute.**

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Para obtener más información acerca de la configuración de la API web, consulte [Configuración de ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: Migración desde Web API 1

Antes de Web API 2, las plantillas de proyecto de API web generaban código como este:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Si el enrutamiento de atributos está habilitado, este código producirá una excepción. Si actualiza un proyecto de API web existente para usar el enrutamiento de atributos, asegúrese de actualizar este código de configuración a lo siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Para obtener más información, consulte [Configuración de la API web con ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Adición de atributos de ruta

A continuación se muestra un ejemplo de una ruta definida mediante un atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La &quot;cadena customers/-customerId-/orders&quot; es la plantilla DE URI para la ruta. Web API intenta hacer coincidir el URI de solicitud con la plantilla. En este ejemplo, "clientes" y "pedidos" son segmentos literales, y "'customerId'" es un parámetro de variable. Los siguientes URI coincidirían con esta plantilla:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Puede restringir la coincidencia mediante [restricciones](#constraints), que se describen más adelante en este tema.

Tenga en &quot;cuenta que&quot; el parámetro .customerId en la plantilla de ruta coincide con el nombre del parámetro *customerId* en el método. Cuando Web API invoca la acción del controlador, intenta enlazar los parámetros de ruta. Por ejemplo, si `http://example.com/customers/1/orders`el URI es , Web API intenta enlazar el valor "1" al parámetro *customerId* de la acción.

Una plantilla URI puede tener varios parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Cualquier método de controlador que no tenga un atributo de ruta utiliza enrutamiento basado en convenciones. De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.

## <a name="http-methods"></a>HTTP Methods

Web API también selecciona acciones basadas en el método HTTP de la solicitud (GET, POST, etc.). De forma predeterminada, Web API busca una coincidencia que no distinve entre mayúsculas y minúsculas con el inicio del nombre del método de controlador. Por ejemplo, un `PutCustomers` método de controlador denominado coincide con una solicitud HTTP PUT.

Puede invalidar esta convención decorando el método con los siguientes atributos:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

En el ejemplo siguiente se asigna el método CreateBook a solicitudes HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Para todos los demás métodos HTTP, incluidos los métodos no estándar, utilice el atributo **AcceptVerbs,** que toma una lista de métodos HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefijos de ruta

A menudo, las rutas en un regulador comienzan con el mismo prefijo. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Puede establecer un prefijo común para un controlador completo mediante el atributo **[RoutePrefix]:**

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Utilice una tilde (-) en el atributo de método para invalidar el prefijo de ruta:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

El prefijo de ruta puede incluir parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Restricciones de ruta

Las restricciones de ruta le permiten restringir la forma en que coinciden los parámetros de la plantilla de ruta. La sintaxis &quot;general es&quot;.parámetro:restricción. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Aquí, la primera ruta solo &quot;se&quot; seleccionará si el segmento id del URI es un entero. De lo contrario, se elegirá la segunda ruta.

En la tabla siguiente se enumeran las restricciones que se admiten.

| Restricción | Descripción | Ejemplo |
| --- | --- | --- |
| alpha | Coincide con caracteres alfabéticos latinos en mayúsculas o minúsculas (a-z, A-Z) | •x:alpha |
| bool | Coincide con un valor booleano. | •x:bool. |
| datetime | Coincide con un **dateTime** valor. | •x:datetime |
| Decimal | Coincide con un valor decimal. | .x:decimal ? |
| double | Coincide con un valor de punto flotante de 64 bits. | •x:doble |
| FLOAT | Coincide con un valor de punto flotante de 32 bits. | •x:float |
| guid | Coincide con un valor GUID. | •x:guid |
| int | Coincide con un valor entero de 32 bits. | •x:int. |
| length | Coincide con una cadena con la longitud especificada o dentro de un intervalo de longitudes especificado. | •x:longitud(6) •x:longitud(1,20) |
| long | Coincide con un valor entero de 64 bits. | •x:largo |
| max | Coincide con un entero con un valor máximo. | •x:max(10) |
| Maxlength | Coincide con una cadena con una longitud máxima. | •x:maxlength(10) |
| Min | Coincide con un entero con un valor mínimo. | •x:min(10) |
| minlength | Coincide con una cadena con una longitud mínima. | •x:minlength(10) |
| range | Coincide con un entero dentro de un intervalo de valores. | •x:rango(10,50) |
| regex | Coincide con una expresión regular. | •x:regex('d{3}''d{3}''d ''''''''''''''''''''''''''''''''''''''''''{4} |

Observe que algunas de las &quot;&quot;restricciones, como min , toman argumentos entre paréntesis. Puede aplicar varias restricciones a un parámetro, separadas por dos puntos.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Restricciones de ruta personalizadas

Puede crear restricciones de ruta personalizadas implementando la interfaz **IHttpRouteConstraint.** Por ejemplo, la restricción siguiente restringe un parámetro a un valor entero distinto de cero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

El código siguiente muestra cómo registrar la restricción:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Ahora puede aplicar la restricción en sus rutas:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

También puede reemplazar toda la **clase DefaultInlineConstraintResolver** implementando la interfaz **IInlineConstraintResolver.** Si lo hace, reemplazará todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** las agregue específicamente.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parámetros URI opcionales y valores predeterminados

Puede hacer que un parámetro URI sea opcional agregando un signo de interrogación al parámetro route. Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

En este `/api/books/locale/1033` ejemplo, devuelva `/api/books/locale` el mismo recurso.

Como alternativa, puede especificar un valor predeterminado dentro de la plantilla de ruta, como se indica a continuación:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Esto es casi lo mismo que el ejemplo anterior, pero hay una ligera diferencia de comportamiento cuando se aplica el valor predeterminado.

- En el primer ejemplo,'lcid:int?''' y se asigna directamente al parámetro de método, por lo que el parámetro tendrá este valor exacto.
- En el segundo ejemplo,'lcid:int'1033'''' y ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' El model-binder predeterminado convertirá "1033" al valor numérico 1033. Sin embargo, puede conectar un enlazador de modelos personalizado, que podría hacer algo diferente.

(En la mayoría de los casos, a menos que tenga enlazadores de modelos personalizados en la canalización, los dos formularios serán equivalentes.)

<a id="route-names"></a>
## <a name="route-names"></a>Nombres de rutas

En Web API, cada ruta tiene un nombre. Los nombres de ruta son útiles para generar vínculos, de modo que pueda incluir un vínculo en una respuesta HTTP.

Para especificar el nombre de la ruta, establezca la propiedad **Name** en el atributo. En el ejemplo siguiente se muestra cómo establecer el nombre de ruta y también cómo utilizar el nombre de ruta al generar un vínculo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Orden de ruta

Cuando el marco de trabajo intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden determinado. Para especificar el orden, establezca la propiedad **Order** en el atributo route. Los valores más bajos se evalúan primero. El valor de pedido predeterminado es cero.

Así es como se determina el pedido total:

1. Compare la propiedad **Order** del atributo route.
2. Observe cada segmento URI en la plantilla de ruta. Para cada segmento, ordene de la siguiente manera:

    1. Segmentos literales.
    2. Parámetros de ruta con restricciones.
    3. Parámetros de ruta sin restricciones.
    4. Segmentos de parámetros comodín con restricciones.
    5. Segmentos de parámetros comodín sin restricciones.
3. En el caso de un empate, las rutas se ordenan mediante una comparación de cadenas ordinales sin distinción entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.

A continuación se muestra un ejemplo: Supongamos que define el controlador siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Estas rutas se ordenan de la siguiente manera.

1. pedidos/detalles
2. pedidos/'id'
3. pedidos/-nombreDeCliente-Nombre-Cliente-Nombre_de_cliente-
4. pedidos/fecha?\*
5. órdenes/pendientes

Tenga en cuenta que "detalles" es un segmento literal y aparece antes de "id", pero "pendiente" aparece en último lugar porque el **Order** propiedad es 1. (En este ejemplo se supone que no hay clientes denominados "detalles" o "pendientes". En general, trate de evitar rutas ambiguas. En este ejemplo, una `GetByCustomer` mejor plantilla de ruta para es "customers/'customerName'" )
