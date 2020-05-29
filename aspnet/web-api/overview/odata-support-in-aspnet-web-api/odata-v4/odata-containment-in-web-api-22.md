---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contención en OData V4 con Web API 2,2 | Microsoft Docs
author: rick-anderson
description: 'Tradicionalmente, solo se podía tener acceso a una entidad si estaba encapsulada dentro de un conjunto de entidades. Sin embargo, OData V4 proporciona dos opciones adicionales: singleton y con...'
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3be81eac9de4686a0d187396e951b121ea65bac4
ms.sourcegitcommit: a4c3c7e04e5f53cf8cd334f036d324976b78d154
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84173009"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a>Contención en OData V4 con Web API 2,2

por Jinfu tan

> Tradicionalmente, solo se podía tener acceso a una entidad si estaba encapsulada dentro de un conjunto de entidades. Sin embargo, OData V4 proporciona dos opciones adicionales, singleton y contención, en las que se admite WebAPI 2,2.

En este tema se muestra cómo definir una contención en un punto de conexión de OData en WebApi 2,2. Para obtener más información acerca de la contención, vea [contención está llegando a OData V4](https://devblogs.microsoft.com/odata/tutorial-sample-containment-is-coming-with-odata-v4/). Para crear un punto de conexión de OData V4 en Web API, consulte [crear un punto de conexión de oData V4 mediante ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).

En primer lugar, vamos a crear un modelo de dominio de contención en el servicio de OData con este modelo de datos:

![Modelo de datos](odata-containment-in-web-api-22/_static/image1.png)

Una cuenta contiene muchos PaymentInstruments (PI), pero no se define un conjunto de entidades para PI. En su lugar, solo se puede tener acceso a la PI a través de una cuenta.

Puede descargar la solución que se usa en este tema desde [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definir el modelo de datos

1. Defina los tipos de CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    El `Contained` atributo se usa para las propiedades de navegación de contención.
2. Generar el modelo EDM basado en los tipos de CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder`Administrará la compilación del modelo EDM si el `Contained` atributo se agrega a la propiedad de navegación correspondiente. Si la propiedad es un tipo de colección, `GetCount(string NameContains)` también se creará una función.

    Los metadatos generados tendrán un aspecto similar al siguiente:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    El `ContainsTarget` atributo indica que la propiedad de navegación es una contención.

## <a name="define-the-containing-entity-set-controller"></a>Definir el controlador de conjunto de entidades contenedoras

Las entidades contenidas no tienen su propio controlador; la acción se define en el controlador de conjunto de entidades contenedoras. En este ejemplo, hay un AccountsController, pero no PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Si la ruta de acceso de OData es de 4 o más segmentos, solo funciona el enrutamiento de atributo, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` en el controlador anterior. De lo contrario, el atributo y el enrutamiento convencional funcionan: por ejemplo, `GetPayInPIs(int key)` coincide con `GET ~/Accounts(1)/PayinPIs` .

*Gracias a Leo hu en el contenido original de este artículo.*
