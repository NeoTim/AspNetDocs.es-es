---
title: Uso de convenciones de API web
author: pranavkm
description: Obtenga más información sobre las convenciones de API web en ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 5ae96b213a19464045e1d0b1a76f8eb81089dc5b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029942"
---
# <a name="use-web-api-conventions"></a>Uso de convenciones de API web

Por [Pranav Krishnamoorthy](https://github.com/pranavkm) y [Scott Addie](https://github.com/scottaddie)

ASP.NET Core 2.2 (y versiones posteriores) incluye una forma de extraer la [documentación de API](xref:tutorials/web-api-help-pages-using-swagger) común y aplicarla a varias acciones o varios controladores, e incluso a todos los controladores dentro de un ensamblado. Las convenciones de API web son un sustituto para complementar acciones individuales con [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).

Una convención permite lo siguiente:

* Definir los tipos de valor devuelto más comunes y los códigos de estado devueltos a partir de un tipo específico de acción.
* Identificar las acciones que no siguen el estándar definido.

ASP.NET Core MVC 2.2 (y versiones posteriores) incluye un conjunto de convenciones predeterminadas en `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Las convenciones se basan en el controlador (*ValuesController.cs*) proporcionado en la plantilla de proyecto de la **API** de ASP.NET Core. Si sus acciones siguen los patrones de la plantilla, debería poder usar las convenciones predeterminadas correctamente. Si las convenciones predeterminadas no satisfacen sus necesidades, consulte [Creación de convenciones de API web](#create-web-api-conventions).

En tiempo de ejecución, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> entiende las convenciones. `ApiExplorer` es la abstracción de MVC para comunicarse con los generadores de documento de [OpenAPI](https://www.openapis.org/), conocido también como Swagger. Los atributos de la convención aplicada se asocian a una acción y se incluyen en la documentación de OpenAPI de la acción. Los [analizadores de API](xref:web-api/advanced/analyzers) también comprenden las convenciones. Si la acción no es convencional (por ejemplo, devuelve un código de estado no documentado en la convención aplicada), recibirá una advertencia en la que se le animará a documentar el código de estado.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Aplicación de convenciones de API web

Las convenciones no se crean, y es posible que cada acción esté asociada a una única convención. Las convenciones más específicas tienen prioridad sobre las menos específicas. La selección es no determinista cuando se aplican dos o más convenciones de la misma prioridad a una acción. Las siguientes opciones existen para aplicar una convención a una acción, de la más específica a la menos específica:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute`: se aplica a las acciones individuales y especifica el tipo de convención y el método de la convención que se aplica.

    En el ejemplo siguiente, el método de convención `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` del tipo de convención predeterminada se aplica a la acción `Update`:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    El método de convención `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` aplica los siguientes atributos a la acción:

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

Para obtener más información sobre `[ProducesDefaultResponseType]`, vea [Default Response](https://swagger.io/docs/specification/describing-responses/#default) (Respuesta predeterminada).

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un controlador: se aplica el tipo de convención especificado a todas las acciones del controlador. Un método de convención se representa con sugerencias que determinan las acciones a las que este se aplica. Para obtener más información sobre las sugerencias, consulte [Creación de convenciones de API web](#create-web-api-conventions).

    En el ejemplo siguiente, el conjunto predeterminado de convenciones se aplica a todas las acciones de *ContactsConventionController*:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un ensamblado: se aplica el tipo de convención especificado a todos los controladores del ensamblado actual. Como recomendación, aplique los atributos de nivel de ensamblado al archivo *Startup.cs*.

    En el ejemplo siguiente, el conjunto predeterminado de convenciones se aplica a todos los controladores del ensamblado:

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>Creación de convenciones de API web

Si las convenciones de API predeterminadas no satisfacen sus necesidades, cree sus propias convenciones. Una convención es:

* Un tipo estático con métodos.
* Capaz de definir [tipos de respuesta](#response-types) y [requisitos de nomenclatura](#naming-requirements) en acciones.

### <a name="response-types"></a>Tipos de respuesta

Estos métodos se anotan con atributos `[ProducesResponseType]` o `[ProducesDefaultResponseType]`. Por ejemplo:

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Si no se presentan atributos de metadatos más específicos, la aplicación de esta convención a un ensamblado requiere lo siguiente:

* El método de convención debe aplicarse a cualquier acción denominada `Find`.
* Un parámetro denominado `id` debe estar presente en la acción `Find`.

### <a name="naming-requirements"></a>Requisitos de nomenclatura

Los atributos `[ApiConventionNameMatch]` y `[ApiConventionTypeMatch]` pueden aplicarse al método de convención que determina las acciones a las que se aplican. Por ejemplo:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

En el ejemplo anterior:

* La opción `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` aplicada al método indica que la convención coincide con cualquier acción que vaya precedida de "Búsqueda". Entre los ejemplos de acciones coincidentes se incluyen `Find`, `FindPet` y `FindById`.
* El valor `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` aplicado al parámetro indica que la convención coincide con los métodos que tengan exactamente un parámetro que termine con el identificador del sufijo. Entre los ejemplos se incluyen parámetros tales como `id` o `petId`. `ApiConventionTypeMatch` puede aplicarse de forma similar a los tipos para restringir el tipo de parámetro. Un argumento `params[]` indica los parámetros restantes que no tienen que coincidir explícitamente.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
