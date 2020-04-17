---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herencia de tipos complejos en OData v4 con ASP.NET Web API . Microsoft Docs
author: rick-anderson
description: Según la especificación de OData v4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo complejo es un tipo estructurado sin una clave.) API web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: f433cf625c7d6ff4922d8c4a9954682fc0f1cc33
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543605"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Herencia de tipos complejos en OData v4 con ASP.NET Web API

por [Microsoft](https://github.com/microsoft)

> Según la [especificación](http://www.odata.org/documentation/odata-version-4-0/)oData v4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo *complejo* es un tipo estructurado sin una clave.) Web API OData 5.3 admite la herencia de tipos complejos.
> 
> En este tema se muestra cómo crear un modelo de datos de entidad (EDM) con tipos de herencia complejos. Para obtener el código fuente completo, vea Ejemplo de herencia de [tipos complejos](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)de OData .
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software utilizadas en el tutorial
> 
> 
> - Web API OData 5.3
> - OData v4

## <a name="model-hierarchy"></a>Jerarquía del modelo

Para ilustrar la herencia de tipos complejos, usaremos la siguiente jerarquía de clases.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`es un tipo complejo abstracto. `Rectangle`, `Triangle`, `Circle` y son tipos `Shape`complejos derivados de , y `RoundRectangle` deriva de `Rectangle`. `Window`es un tipo de `Shape` entidad y contiene una instancia.

Aquí están las clases CLR que definen estos tipos.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Construir el modelo EDM

Para crear el EDM, puede usar **ODataConventionModelBuilder**, que deduce las relaciones de herencia de los tipos CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

También puede compilar el EDM explícitamente mediante **ODataModelBuilder**. Esto requiere más código, pero le da más control sobre el EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Estos dos ejemplos crean el mismo esquema de EDM.

## <a name="metadata-document"></a>Documento de metadatos

Aquí está el documento de metadatos de OData, que muestra la herencia de tipos complejos.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

En el documento de metadatos, puede ver que:

- El `Shape` tipo complejo es abstracto.
- El `Rectangle` `Triangle`tipo `Circle` , , y `Shape`complex tiene el tipo base .
- El `RoundRectangle` tipo tiene `Rectangle`el tipo base .

## <a name="casting-complex-types"></a>Tipos complejos de fundición

Ahora se admite la conversión en tipos complejos. Por ejemplo, la siguiente `Shape` consulta `Rectangle`convierte a en un archivo .

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Aquí está la carga útil de la respuesta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
