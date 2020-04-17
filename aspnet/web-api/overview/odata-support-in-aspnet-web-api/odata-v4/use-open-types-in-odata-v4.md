---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Abrir tipos en OData v4 con ASP.NET Web API . Microsoft Docs
author: rick-anderson
description: En OData v4, un tipo abierto es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo. Abrir...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d63c96df6614896b3b67eef94a8907e528572567
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543735"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Abrir tipos en OData v4 con ASP.NET Web API

por [Microsoft](https://github.com/microsoft)

> En OData v4, un *tipo abierto* es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo. Los tipos abiertos le permiten agregar flexibilidad a sus modelos de datos. En este tutorial se muestra cómo usar tipos abiertos en ASP.NET Web API OData.
> 
> En este tutorial se supone que ya sabe cómo crear un punto de conexión de OData en ASP.NET API web. Si no es así, comience leyendo Crear un punto de conexión de [OData v4](create-an-odata-v4-endpoint.md) primero.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software utilizadas en el tutorial
> 
> 
> - Web API OData 5.3
> - OData v4

En primer lugar, algunaterminología de OData:

- Tipo de entidad: un tipo estructurado con una clave.
- Tipo complejo: un tipo estructurado sin clave.
- Tipo abierto: un tipo con propiedades dinámicas. Tanto los tipos de entidad como los tipos complejos pueden estar abiertos.

El valor de una propiedad dinámica puede ser un tipo primitivo, un tipo complejo o un tipo de enumeración; o una colección de cualquiera de esos tipos. Para obtener más información acerca de los tipos abiertos, consulte la [especificación de OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instale las bibliotecas Web OData

Use nuGet Package Manager para instalar las bibliotecas oData de la API web más recientes. En la ventana Consola del Administrador de paquetes:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definir los tipos CLR

Comience definiendo los modelos de EDM como tipos CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Cuando se crea Entity Data Model (EDM),

- `Category`es un tipo de enumeración.
- `Address`es un tipo complejo. (No tiene una clave, por lo que no es un tipo de entidad.)
- `Customer`es un tipo de entidad. (Tiene una llave.)
- `Press`es un tipo complejo abierto.
- `Book`es un tipo de entidad abierta.

Para crear un tipo abierto, el tipo `IDictionary<string, object>`CLR debe tener una propiedad de tipo , que contiene las propiedades dinámicas.

## <a name="build-the-edm-model"></a>Construir el modelo EDM

Si usa **ODataConventionModelBuilder** para crear `Press` el `Book` EDM y se agregan automáticamente como `IDictionary<string, object>` tipos abiertos, en función de la presencia de una propiedad.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

También puede compilar el EDM explícitamente mediante **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Agregar un controlador OData

A continuación, agregue un controlador OData. Para este tutorial, usaremos un controlador simplificado que solo admite solicitudes GET y POST y usa una lista en memoria para almacenar entidades.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Observe que `Book` la primera instancia no tiene propiedades dinámicas. La `Book` segunda instancia tiene las siguientes propiedades dinámicas:

- "Publicado": Tipo primitivo
- "Authors": Colección de tipos primitivos
- "OtherCategories": colección de tipos de enumeración.

Además, `Press` la `Book` propiedad de esa instancia tiene las siguientes propiedades dinámicas:

- "Blog": Tipo primitivo
- "Address": Tipo complejo

## <a name="query-the-metadata"></a>Consultar los metadatos

Para obtener el documento de metadatos de `~/$metadata`OData, envíe una solicitud GET a . El cuerpo de respuesta debe ser similar a esto:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

En el documento de metadatos, puede ver que:

- Para `Book` los `Press` tipos y, `OpenType` el valor del atributo es true. Los `Customer` `Address` tipos y no tienen este atributo.
- El `Book` tipo de entidad tiene tres propiedades declaradas: ISBN, Title y Press. Los metadatos de OData `Book.Properties` no incluyen la propiedad de la clase CLR.
- De forma `Press` similar, el tipo complejo solo tiene dos propiedades declaradas: Name y Category. Los metadatos no `Press.DynamicProperties` incluyen la propiedad de la clase CLR.

## <a name="query-an-entity"></a>Consultar una entidad

Para obtener el libro con ISBN igual a "978-0-7356-7942-9", envíe una solicitud GET a `~/Books('978-0-7356-7942-9')`. El cuerpo de la respuesta debe ser similar al siguiente. (Indentado para que sea más legible.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Observe que las propiedades dinámicas se incluyen en línea con las propiedades declaradas.

## <a name="post-an-entity"></a>POST una Entidad

Para agregar una entidad Book, `~/Books`envíe una solicitud POST a . El cliente puede establecer propiedades dinámicas en la carga de la solicitud.

Aquí hay una solicitud de ejemplo. Tenga en cuenta las propiedades "Precio" y "Publicado".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Si establece un punto de interrupción en el método de controlador, puede ver que la API web agregó estas propiedades al `Properties` diccionario.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de tipo abierto de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
