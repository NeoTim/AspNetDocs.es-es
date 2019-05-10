---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Tipos abiertos en OData v4 con ASP.NET Web API | Microsoft Docs
author: microsoft
description: En OData v4, un tipo abierto es un tipo estructurado que contiene las propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo. Abrir...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65108461"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="c9c27-104">Tipos abiertos en OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c9c27-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="c9c27-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c9c27-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c9c27-106">En OData v4, un *abrir tipo* es un tipo estructurado que contiene las propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo.</span><span class="sxs-lookup"><span data-stu-id="c9c27-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="c9c27-107">Tipos abiertos permiten agregar flexibilidad a los modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="c9c27-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="c9c27-108">Este tutorial muestra cómo utilizar tipos abiertos en ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="c9c27-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="c9c27-109">En este tutorial se da por supuesto que ya sabe cómo crear un extremo de OData en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="c9c27-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="c9c27-110">Si no, empiece por leer [crear un punto de conexión de OData v4](create-an-odata-v4-endpoint.md) primero.</span><span class="sxs-lookup"><span data-stu-id="c9c27-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c9c27-111">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="c9c27-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c9c27-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="c9c27-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="c9c27-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="c9c27-113">OData v4</span></span>

<span data-ttu-id="c9c27-114">En primer lugar, la terminología OData:</span><span class="sxs-lookup"><span data-stu-id="c9c27-114">First, some OData terminology:</span></span>

- <span data-ttu-id="c9c27-115">Tipo de entidad: Un tipo estructurado con una clave.</span><span class="sxs-lookup"><span data-stu-id="c9c27-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="c9c27-116">Tipo complejo: Un tipo estructurado sin una clave.</span><span class="sxs-lookup"><span data-stu-id="c9c27-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="c9c27-117">Tipo Open: Un tipo con propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="c9c27-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="c9c27-118">Tipos de entidad y tipos complejos pueden estar abiertos.</span><span class="sxs-lookup"><span data-stu-id="c9c27-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="c9c27-119">El valor de una propiedad dinámica puede ser un tipo primitivo, un tipo complejo o un tipo de enumeración; o una colección de cualquiera de esos tipos.</span><span class="sxs-lookup"><span data-stu-id="c9c27-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="c9c27-120">Para obtener más información sobre los tipos abiertos, vea el [especificación de OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="c9c27-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="c9c27-121">Instalar las bibliotecas de OData de Web</span><span class="sxs-lookup"><span data-stu-id="c9c27-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="c9c27-122">Use el Administrador de paquetes de NuGet para instalar las bibliotecas de OData de Web API más recientes.</span><span class="sxs-lookup"><span data-stu-id="c9c27-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="c9c27-123">En la ventana de consola de administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="c9c27-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="c9c27-124">Definir los tipos de CLR</span><span class="sxs-lookup"><span data-stu-id="c9c27-124">Define the CLR Types</span></span>

<span data-ttu-id="c9c27-125">Empiece por definir los modelos EDM como tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="c9c27-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="c9c27-126">Cuando se crea el Entity Data Model (EDM),</span><span class="sxs-lookup"><span data-stu-id="c9c27-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="c9c27-127">`Category` es un tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="c9c27-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="c9c27-128">`Address` es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="c9c27-128">`Address` is a complex type.</span></span> <span data-ttu-id="c9c27-129">(No tiene una clave, por lo que no es un tipo de entidad.)</span><span class="sxs-lookup"><span data-stu-id="c9c27-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="c9c27-130">`Customer` es un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="c9c27-130">`Customer` is an entity type.</span></span> <span data-ttu-id="c9c27-131">(Tiene una clave).</span><span class="sxs-lookup"><span data-stu-id="c9c27-131">(It has a key.)</span></span>
- <span data-ttu-id="c9c27-132">`Press` es un tipo complejo abierto.</span><span class="sxs-lookup"><span data-stu-id="c9c27-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="c9c27-133">`Book` es un tipo de entidad abierto.</span><span class="sxs-lookup"><span data-stu-id="c9c27-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="c9c27-134">Para crear un tipo abierto, el tipo CLR debe tener una propiedad de tipo `IDictionary<string, object>`, que contiene las propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="c9c27-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="c9c27-135">Generar el modelo EDM</span><span class="sxs-lookup"><span data-stu-id="c9c27-135">Build the EDM Model</span></span>

<span data-ttu-id="c9c27-136">Si usas **ODataConventionModelBuilder** para crear el EDM, `Press` y `Book` se agregan automáticamente como tipos abiertos, en función de la presencia de un `IDictionary<string, object>` propiedad.</span><span class="sxs-lookup"><span data-stu-id="c9c27-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="c9c27-137">También puede generar el EDM explícitamente, mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="c9c27-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="c9c27-138">Agregar un controlador de OData</span><span class="sxs-lookup"><span data-stu-id="c9c27-138">Add an OData Controller</span></span>

<span data-ttu-id="c9c27-139">A continuación, agregue un controlador de OData.</span><span class="sxs-lookup"><span data-stu-id="c9c27-139">Next, add an OData controller.</span></span> <span data-ttu-id="c9c27-140">Para este tutorial, vamos a usar un controlador simplificado que admite solo GET y POST las solicitudes y usa una lista en memoria para almacenar las entidades.</span><span class="sxs-lookup"><span data-stu-id="c9c27-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="c9c27-141">Tenga en cuenta que la primera `Book` instancia no tiene ninguna propiedad dinámica.</span><span class="sxs-lookup"><span data-stu-id="c9c27-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="c9c27-142">El segundo `Book` instancia tiene las siguientes propiedades dinámicas:</span><span class="sxs-lookup"><span data-stu-id="c9c27-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="c9c27-143">"Publicado": Tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="c9c27-143">"Published": Primitive type</span></span>
- <span data-ttu-id="c9c27-144">"Autores": Colección de tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="c9c27-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="c9c27-145">"OtherCategories": Colección de tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="c9c27-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="c9c27-146">Además, el `Press` propiedad eso `Book` instancia tiene las siguientes propiedades dinámicas:</span><span class="sxs-lookup"><span data-stu-id="c9c27-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="c9c27-147">"Blog": Tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="c9c27-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="c9c27-148">"Address": Tipo complejo</span><span class="sxs-lookup"><span data-stu-id="c9c27-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="c9c27-149">Consultar los metadatos</span><span class="sxs-lookup"><span data-stu-id="c9c27-149">Query the Metadata</span></span>

<span data-ttu-id="c9c27-150">Para obtener el documento de metadatos de OData, envíe una solicitud GET a `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="c9c27-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="c9c27-151">El cuerpo de respuesta debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9c27-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="c9c27-152">En el documento de metadatos, puede ver:</span><span class="sxs-lookup"><span data-stu-id="c9c27-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="c9c27-153">Para el `Book` y `Press` tipos, el valor de la `OpenType` atributo es true.</span><span class="sxs-lookup"><span data-stu-id="c9c27-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="c9c27-154">El `Customer` y `Address` tipos no tienen este atributo.</span><span class="sxs-lookup"><span data-stu-id="c9c27-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="c9c27-155">El `Book` tipo de entidad tiene tres propiedades declaradas: ISBN, título y presione.</span><span class="sxs-lookup"><span data-stu-id="c9c27-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="c9c27-156">Los metadatos de OData no incluyen el `Book.Properties` propiedad de la clase CLR.</span><span class="sxs-lookup"><span data-stu-id="c9c27-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="c9c27-157">De forma similar, la `Press` tipo complejo tiene solo dos propiedades declaradas: Nombre y categoría.</span><span class="sxs-lookup"><span data-stu-id="c9c27-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="c9c27-158">Los metadatos no incluyen el `Press.DynamicProperties` propiedad de la clase CLR.</span><span class="sxs-lookup"><span data-stu-id="c9c27-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="c9c27-159">Consultar una entidad</span><span class="sxs-lookup"><span data-stu-id="c9c27-159">Query an Entity</span></span>

<span data-ttu-id="c9c27-160">Para obtener el libro con ISBN igual a "978-0-7356-7942-9", envíe una solicitud GET a `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="c9c27-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="c9c27-161">El cuerpo de respuesta debe ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="c9c27-161">The response body should look similar to the following.</span></span> <span data-ttu-id="c9c27-162">(Con sangría para que sea más legible.)</span><span class="sxs-lookup"><span data-stu-id="c9c27-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="c9c27-163">Tenga en cuenta que las propiedades dinámicas están incluidas alineadas con las propiedades declaradas.</span><span class="sxs-lookup"><span data-stu-id="c9c27-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="c9c27-164">REGISTRE una entidad</span><span class="sxs-lookup"><span data-stu-id="c9c27-164">POST an Entity</span></span>

<span data-ttu-id="c9c27-165">Para agregar una entidad de libro, envíe una solicitud POST a `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="c9c27-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="c9c27-166">El cliente puede establecer propiedades dinámicas en la carga de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c9c27-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="c9c27-167">Esta es una solicitud de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="c9c27-167">Here is an example request.</span></span> <span data-ttu-id="c9c27-168">Tenga en cuenta las propiedades "Price" y "Publicado".</span><span class="sxs-lookup"><span data-stu-id="c9c27-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="c9c27-169">Si establece un punto de interrupción en el método de controlador, puede ver que la API Web agregado estas propiedades para el `Properties` diccionario.</span><span class="sxs-lookup"><span data-stu-id="c9c27-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="c9c27-170">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c9c27-170">Additional Resources</span></span>

[<span data-ttu-id="c9c27-171">Ejemplo de tipo abierto de OData</span><span class="sxs-lookup"><span data-stu-id="c9c27-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
