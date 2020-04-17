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
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="42f36-104">Abrir tipos en OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="42f36-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="42f36-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="42f36-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="42f36-106">En OData v4, un *tipo abierto* es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo.</span><span class="sxs-lookup"><span data-stu-id="42f36-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="42f36-107">Los tipos abiertos le permiten agregar flexibilidad a sus modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="42f36-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="42f36-108">En este tutorial se muestra cómo usar tipos abiertos en ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="42f36-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="42f36-109">En este tutorial se supone que ya sabe cómo crear un punto de conexión de OData en ASP.NET API web.</span><span class="sxs-lookup"><span data-stu-id="42f36-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="42f36-110">Si no es así, comience leyendo Crear un punto de conexión de [OData v4](create-an-odata-v4-endpoint.md) primero.</span><span class="sxs-lookup"><span data-stu-id="42f36-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="42f36-111">Versiones de software utilizadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="42f36-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="42f36-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="42f36-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="42f36-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="42f36-113">OData v4</span></span>

<span data-ttu-id="42f36-114">En primer lugar, algunaterminología de OData:</span><span class="sxs-lookup"><span data-stu-id="42f36-114">First, some OData terminology:</span></span>

- <span data-ttu-id="42f36-115">Tipo de entidad: un tipo estructurado con una clave.</span><span class="sxs-lookup"><span data-stu-id="42f36-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="42f36-116">Tipo complejo: un tipo estructurado sin clave.</span><span class="sxs-lookup"><span data-stu-id="42f36-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="42f36-117">Tipo abierto: un tipo con propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="42f36-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="42f36-118">Tanto los tipos de entidad como los tipos complejos pueden estar abiertos.</span><span class="sxs-lookup"><span data-stu-id="42f36-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="42f36-119">El valor de una propiedad dinámica puede ser un tipo primitivo, un tipo complejo o un tipo de enumeración; o una colección de cualquiera de esos tipos.</span><span class="sxs-lookup"><span data-stu-id="42f36-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="42f36-120">Para obtener más información acerca de los tipos abiertos, consulte la [especificación de OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="42f36-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="42f36-121">Instale las bibliotecas Web OData</span><span class="sxs-lookup"><span data-stu-id="42f36-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="42f36-122">Use nuGet Package Manager para instalar las bibliotecas oData de la API web más recientes.</span><span class="sxs-lookup"><span data-stu-id="42f36-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="42f36-123">En la ventana Consola del Administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="42f36-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="42f36-124">Definir los tipos CLR</span><span class="sxs-lookup"><span data-stu-id="42f36-124">Define the CLR Types</span></span>

<span data-ttu-id="42f36-125">Comience definiendo los modelos de EDM como tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="42f36-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="42f36-126">Cuando se crea Entity Data Model (EDM),</span><span class="sxs-lookup"><span data-stu-id="42f36-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="42f36-127">`Category`es un tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="42f36-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="42f36-128">`Address`es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="42f36-128">`Address` is a complex type.</span></span> <span data-ttu-id="42f36-129">(No tiene una clave, por lo que no es un tipo de entidad.)</span><span class="sxs-lookup"><span data-stu-id="42f36-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="42f36-130">`Customer`es un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="42f36-130">`Customer` is an entity type.</span></span> <span data-ttu-id="42f36-131">(Tiene una llave.)</span><span class="sxs-lookup"><span data-stu-id="42f36-131">(It has a key.)</span></span>
- <span data-ttu-id="42f36-132">`Press`es un tipo complejo abierto.</span><span class="sxs-lookup"><span data-stu-id="42f36-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="42f36-133">`Book`es un tipo de entidad abierta.</span><span class="sxs-lookup"><span data-stu-id="42f36-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="42f36-134">Para crear un tipo abierto, el tipo `IDictionary<string, object>`CLR debe tener una propiedad de tipo , que contiene las propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="42f36-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="42f36-135">Construir el modelo EDM</span><span class="sxs-lookup"><span data-stu-id="42f36-135">Build the EDM Model</span></span>

<span data-ttu-id="42f36-136">Si usa **ODataConventionModelBuilder** para crear `Press` el `Book` EDM y se agregan automáticamente como `IDictionary<string, object>` tipos abiertos, en función de la presencia de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="42f36-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="42f36-137">También puede compilar el EDM explícitamente mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="42f36-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="42f36-138">Agregar un controlador OData</span><span class="sxs-lookup"><span data-stu-id="42f36-138">Add an OData Controller</span></span>

<span data-ttu-id="42f36-139">A continuación, agregue un controlador OData.</span><span class="sxs-lookup"><span data-stu-id="42f36-139">Next, add an OData controller.</span></span> <span data-ttu-id="42f36-140">Para este tutorial, usaremos un controlador simplificado que solo admite solicitudes GET y POST y usa una lista en memoria para almacenar entidades.</span><span class="sxs-lookup"><span data-stu-id="42f36-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="42f36-141">Observe que `Book` la primera instancia no tiene propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="42f36-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="42f36-142">La `Book` segunda instancia tiene las siguientes propiedades dinámicas:</span><span class="sxs-lookup"><span data-stu-id="42f36-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="42f36-143">"Publicado": Tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="42f36-143">"Published": Primitive type</span></span>
- <span data-ttu-id="42f36-144">"Authors": Colección de tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="42f36-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="42f36-145">"OtherCategories": colección de tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="42f36-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="42f36-146">Además, `Press` la `Book` propiedad de esa instancia tiene las siguientes propiedades dinámicas:</span><span class="sxs-lookup"><span data-stu-id="42f36-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="42f36-147">"Blog": Tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="42f36-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="42f36-148">"Address": Tipo complejo</span><span class="sxs-lookup"><span data-stu-id="42f36-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="42f36-149">Consultar los metadatos</span><span class="sxs-lookup"><span data-stu-id="42f36-149">Query the Metadata</span></span>

<span data-ttu-id="42f36-150">Para obtener el documento de metadatos de `~/$metadata`OData, envíe una solicitud GET a .</span><span class="sxs-lookup"><span data-stu-id="42f36-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="42f36-151">El cuerpo de respuesta debe ser similar a esto:</span><span class="sxs-lookup"><span data-stu-id="42f36-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="42f36-152">En el documento de metadatos, puede ver que:</span><span class="sxs-lookup"><span data-stu-id="42f36-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="42f36-153">Para `Book` los `Press` tipos y, `OpenType` el valor del atributo es true.</span><span class="sxs-lookup"><span data-stu-id="42f36-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="42f36-154">Los `Customer` `Address` tipos y no tienen este atributo.</span><span class="sxs-lookup"><span data-stu-id="42f36-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="42f36-155">El `Book` tipo de entidad tiene tres propiedades declaradas: ISBN, Title y Press.</span><span class="sxs-lookup"><span data-stu-id="42f36-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="42f36-156">Los metadatos de OData `Book.Properties` no incluyen la propiedad de la clase CLR.</span><span class="sxs-lookup"><span data-stu-id="42f36-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="42f36-157">De forma `Press` similar, el tipo complejo solo tiene dos propiedades declaradas: Name y Category.</span><span class="sxs-lookup"><span data-stu-id="42f36-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="42f36-158">Los metadatos no `Press.DynamicProperties` incluyen la propiedad de la clase CLR.</span><span class="sxs-lookup"><span data-stu-id="42f36-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="42f36-159">Consultar una entidad</span><span class="sxs-lookup"><span data-stu-id="42f36-159">Query an Entity</span></span>

<span data-ttu-id="42f36-160">Para obtener el libro con ISBN igual a "978-0-7356-7942-9", envíe una solicitud GET a `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="42f36-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="42f36-161">El cuerpo de la respuesta debe ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="42f36-161">The response body should look similar to the following.</span></span> <span data-ttu-id="42f36-162">(Indentado para que sea más legible.)</span><span class="sxs-lookup"><span data-stu-id="42f36-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="42f36-163">Observe que las propiedades dinámicas se incluyen en línea con las propiedades declaradas.</span><span class="sxs-lookup"><span data-stu-id="42f36-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="42f36-164">POST una Entidad</span><span class="sxs-lookup"><span data-stu-id="42f36-164">POST an Entity</span></span>

<span data-ttu-id="42f36-165">Para agregar una entidad Book, `~/Books`envíe una solicitud POST a .</span><span class="sxs-lookup"><span data-stu-id="42f36-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="42f36-166">El cliente puede establecer propiedades dinámicas en la carga de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="42f36-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="42f36-167">Aquí hay una solicitud de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="42f36-167">Here is an example request.</span></span> <span data-ttu-id="42f36-168">Tenga en cuenta las propiedades "Precio" y "Publicado".</span><span class="sxs-lookup"><span data-stu-id="42f36-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="42f36-169">Si establece un punto de interrupción en el método de controlador, puede ver que la API web agregó estas propiedades al `Properties` diccionario.</span><span class="sxs-lookup"><span data-stu-id="42f36-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="42f36-170">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="42f36-170">Additional Resources</span></span>

[<span data-ttu-id="42f36-171">Ejemplo de tipo abierto de OData</span><span class="sxs-lookup"><span data-stu-id="42f36-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
