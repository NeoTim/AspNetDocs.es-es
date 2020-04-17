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
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="663ec-104">Herencia de tipos complejos en OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="663ec-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="663ec-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="663ec-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="663ec-106">Según la [especificación](http://www.odata.org/documentation/odata-version-4-0/)oData v4, un tipo complejo puede heredar de otro tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="663ec-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="663ec-107">(Un tipo *complejo* es un tipo estructurado sin una clave.) Web API OData 5.3 admite la herencia de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="663ec-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="663ec-108">En este tema se muestra cómo crear un modelo de datos de entidad (EDM) con tipos de herencia complejos.</span><span class="sxs-lookup"><span data-stu-id="663ec-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="663ec-109">Para obtener el código fuente completo, vea Ejemplo de herencia de [tipos complejos](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)de OData .</span><span class="sxs-lookup"><span data-stu-id="663ec-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="663ec-110">Versiones de software utilizadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="663ec-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="663ec-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="663ec-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="663ec-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="663ec-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="663ec-113">Jerarquía del modelo</span><span class="sxs-lookup"><span data-stu-id="663ec-113">Model Hierarchy</span></span>

<span data-ttu-id="663ec-114">Para ilustrar la herencia de tipos complejos, usaremos la siguiente jerarquía de clases.</span><span class="sxs-lookup"><span data-stu-id="663ec-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="663ec-115">`Shape`es un tipo complejo abstracto.</span><span class="sxs-lookup"><span data-stu-id="663ec-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="663ec-116">`Rectangle`, `Triangle`, `Circle` y son tipos `Shape`complejos derivados de , y `RoundRectangle` deriva de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="663ec-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="663ec-117">`Window`es un tipo de `Shape` entidad y contiene una instancia.</span><span class="sxs-lookup"><span data-stu-id="663ec-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="663ec-118">Aquí están las clases CLR que definen estos tipos.</span><span class="sxs-lookup"><span data-stu-id="663ec-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="663ec-119">Construir el modelo EDM</span><span class="sxs-lookup"><span data-stu-id="663ec-119">Build the EDM Model</span></span>

<span data-ttu-id="663ec-120">Para crear el EDM, puede usar **ODataConventionModelBuilder**, que deduce las relaciones de herencia de los tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="663ec-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="663ec-121">También puede compilar el EDM explícitamente mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="663ec-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="663ec-122">Esto requiere más código, pero le da más control sobre el EDM.</span><span class="sxs-lookup"><span data-stu-id="663ec-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="663ec-123">Estos dos ejemplos crean el mismo esquema de EDM.</span><span class="sxs-lookup"><span data-stu-id="663ec-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="663ec-124">Documento de metadatos</span><span class="sxs-lookup"><span data-stu-id="663ec-124">Metadata Document</span></span>

<span data-ttu-id="663ec-125">Aquí está el documento de metadatos de OData, que muestra la herencia de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="663ec-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="663ec-126">En el documento de metadatos, puede ver que:</span><span class="sxs-lookup"><span data-stu-id="663ec-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="663ec-127">El `Shape` tipo complejo es abstracto.</span><span class="sxs-lookup"><span data-stu-id="663ec-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="663ec-128">El `Rectangle` `Triangle`tipo `Circle` , , y `Shape`complex tiene el tipo base .</span><span class="sxs-lookup"><span data-stu-id="663ec-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="663ec-129">El `RoundRectangle` tipo tiene `Rectangle`el tipo base .</span><span class="sxs-lookup"><span data-stu-id="663ec-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="663ec-130">Tipos complejos de fundición</span><span class="sxs-lookup"><span data-stu-id="663ec-130">Casting Complex Types</span></span>

<span data-ttu-id="663ec-131">Ahora se admite la conversión en tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="663ec-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="663ec-132">Por ejemplo, la siguiente `Shape` consulta `Rectangle`convierte a en un archivo .</span><span class="sxs-lookup"><span data-stu-id="663ec-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="663ec-133">Aquí está la carga útil de la respuesta:</span><span class="sxs-lookup"><span data-stu-id="663ec-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
