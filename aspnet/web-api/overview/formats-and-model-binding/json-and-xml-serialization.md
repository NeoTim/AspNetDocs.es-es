---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: 'Serialización JSON y XML en ASP.NET Web API: ASP.NET 4.x'
author: MikeWasson
description: Describe los formateadores JSON y XML en ASP.NET Web API para ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e6e02fa1c48e9c5fb8499379508619ddb317ccc9
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675836"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="49bb1-103">Serialización JSON y XML en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="49bb1-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="49bb1-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="49bb1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="49bb1-105">En este artículo se describen los formateadores JSON y XML de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="49bb1-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="49bb1-106">En ASP.NET Web API, un *formateador de tipo multimedia* es un objeto que puede:</span><span class="sxs-lookup"><span data-stu-id="49bb1-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="49bb1-107">Leer objetos CLR desde un cuerpo de mensaje HTTP</span><span class="sxs-lookup"><span data-stu-id="49bb1-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="49bb1-108">Escribir objetos CLR en un cuerpo de mensaje HTTP</span><span class="sxs-lookup"><span data-stu-id="49bb1-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="49bb1-109">Web API proporciona formateadores de tipo multimedia para JSON y XML.</span><span class="sxs-lookup"><span data-stu-id="49bb1-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="49bb1-110">El marco de trabajo inserta estos formateadores en la canalización de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="49bb1-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="49bb1-111">Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="49bb1-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="49bb1-112">Contenido</span><span class="sxs-lookup"><span data-stu-id="49bb1-112">Contents</span></span>

- [<span data-ttu-id="49bb1-113">Formatter de tipo multimedia JSON</span><span class="sxs-lookup"><span data-stu-id="49bb1-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="49bb1-114">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="49bb1-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="49bb1-115">Fechas</span><span class="sxs-lookup"><span data-stu-id="49bb1-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="49bb1-116">Sangrías</span><span class="sxs-lookup"><span data-stu-id="49bb1-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="49bb1-117">Camel Casing</span><span class="sxs-lookup"><span data-stu-id="49bb1-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="49bb1-118">Objetos anónimos y débilmente tipados</span><span class="sxs-lookup"><span data-stu-id="49bb1-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="49bb1-119">Forador de tipo de medios XML</span><span class="sxs-lookup"><span data-stu-id="49bb1-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="49bb1-120">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="49bb1-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="49bb1-121">Fechas</span><span class="sxs-lookup"><span data-stu-id="49bb1-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="49bb1-122">Sangrías</span><span class="sxs-lookup"><span data-stu-id="49bb1-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="49bb1-123">Configuración de serializadores XML por tipo</span><span class="sxs-lookup"><span data-stu-id="49bb1-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="49bb1-124">Eliminación del formateador JSON o XML</span><span class="sxs-lookup"><span data-stu-id="49bb1-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="49bb1-125">Manejo de referencias circulares de objetos</span><span class="sxs-lookup"><span data-stu-id="49bb1-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="49bb1-126">Prueba de la serialización de objetos</span><span class="sxs-lookup"><span data-stu-id="49bb1-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="49bb1-127">Formatter de tipo multimedia JSON</span><span class="sxs-lookup"><span data-stu-id="49bb1-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="49bb1-128">La clase **JsonMediaTypeFormatter** proporciona el formato JSON.</span><span class="sxs-lookup"><span data-stu-id="49bb1-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="49bb1-129">De forma predeterminada, **JsonMediaTypeFormatter** utiliza la biblioteca [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="49bb1-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="49bb1-130">Json.NET es un proyecto de código abierto de terceros.</span><span class="sxs-lookup"><span data-stu-id="49bb1-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="49bb1-131">Si lo prefiere, puede configurar la clase **JsonMediaTypeFormatter** para que use **DataContractJsonSerializer** en lugar de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="49bb1-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="49bb1-132">Para ello, establezca la propiedad **UseDataContractJsonSerializer** en **true:**</span><span class="sxs-lookup"><span data-stu-id="49bb1-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="49bb1-133">Serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="49bb1-133">JSON Serialization</span></span>

<span data-ttu-id="49bb1-134">En esta sección se describen algunos comportamientos específicos del formateador JSON, utilizando el serializador [de Json.NET](https://github.com/JamesNK/Newtonsoft.Json) predeterminado.</span><span class="sxs-lookup"><span data-stu-id="49bb1-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="49bb1-135">Esto no pretende ser una documentación completa de la biblioteca de Json.NET; Para obtener más información, consulte la [Documentación de Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="49bb1-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="49bb1-136">¿Qué se serializa?</span><span class="sxs-lookup"><span data-stu-id="49bb1-136">What Gets Serialized?</span></span>

<span data-ttu-id="49bb1-137">De forma predeterminada, todas las propiedades públicas y los campos se incluyen en el JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="49bb1-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="49bb1-138">Para omitir una propiedad o un campo, decórelo con el atributo **JsonIgnore.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="49bb1-139">Si prefiere &quot;un enfoque&quot; opt-in, decore la clase con el atributo **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="49bb1-140">Si este atributo está presente, se omiten los miembros a menos que tengan el **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="49bb1-141">También puede usar **DataMember** para serializar miembros privados.</span><span class="sxs-lookup"><span data-stu-id="49bb1-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="49bb1-142">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="49bb1-142">Read-Only Properties</span></span>

<span data-ttu-id="49bb1-143">Las propiedades de solo lectura se serializan de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="49bb1-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="49bb1-144">Fechas</span><span class="sxs-lookup"><span data-stu-id="49bb1-144">Dates</span></span>

<span data-ttu-id="49bb1-145">De forma predeterminada, Json.NET escribe fechas en formato [ISO 8601.](http://www.w3.org/TR/NOTE-datetime)</span><span class="sxs-lookup"><span data-stu-id="49bb1-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="49bb1-146">Las fechas en UTC (hora universal coordinada) se escriben con un sufijo "Z".</span><span class="sxs-lookup"><span data-stu-id="49bb1-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="49bb1-147">Las fechas en la hora local incluyen un desplazamiento de zona horaria.</span><span class="sxs-lookup"><span data-stu-id="49bb1-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="49bb1-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49bb1-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="49bb1-149">De forma predeterminada, Json.NET conserva la zona horaria.</span><span class="sxs-lookup"><span data-stu-id="49bb1-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="49bb1-150">Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="49bb1-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="49bb1-151">Si prefiere utilizar el formato`"\/Date(ticks)\/"`de fecha JSON de [Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) ( ) en lugar de ISO 8601, establezca la propiedad **DateFormatHandling** en la configuración del serializador:</span><span class="sxs-lookup"><span data-stu-id="49bb1-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="49bb1-152">Sangrías</span><span class="sxs-lookup"><span data-stu-id="49bb1-152">Indenting</span></span>

<span data-ttu-id="49bb1-153">Para escribir JSON con sangría, establezca el valor **Formato** en **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="49bb1-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="49bb1-154">Camel Casing</span><span class="sxs-lookup"><span data-stu-id="49bb1-154">Camel Casing</span></span>

<span data-ttu-id="49bb1-155">Para escribir nombres de propiedad JSON con mayúsculas y minúsculas, sin cambiar el modelo de datos, establezca **CamelCasePropertyNamesContractResolver** en el serializador:</span><span class="sxs-lookup"><span data-stu-id="49bb1-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="49bb1-156">Objetos anónimos y débilmente tipados</span><span class="sxs-lookup"><span data-stu-id="49bb1-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="49bb1-157">Un método de acción puede devolver un objeto anónimo y serializarlo en JSON.</span><span class="sxs-lookup"><span data-stu-id="49bb1-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="49bb1-158">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49bb1-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="49bb1-159">El cuerpo del mensaje de respuesta contendrá el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="49bb1-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="49bb1-160">Si la API web recibe objetos JSON estructurados de forma flexible de los clientes, puede deserializar el cuerpo de la solicitud a un tipo **Newtonsoft.Json.Linq.JObject.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="49bb1-161">Sin embargo, normalmente es mejor usar objetos de datos fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="49bb1-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="49bb1-162">A continuación, no es necesario analizar los datos usted mismo y obtiene las ventajas de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="49bb1-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="49bb1-163">El serializador XML no admite tipos anónimos ni instancias **JObject.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="49bb1-164">Si utiliza estas características para los datos JSON, debe quitar el formateador XML de la canalización, como se describe más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="49bb1-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="49bb1-165">Forador de tipo de medios XML</span><span class="sxs-lookup"><span data-stu-id="49bb1-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="49bb1-166">La clase **XmlMediaTypeFormatter** proporciona formato XML.</span><span class="sxs-lookup"><span data-stu-id="49bb1-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="49bb1-167">De forma predeterminada, **XmlMediaTypeFormatter** utiliza la **clase DataContractSerializer** para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="49bb1-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="49bb1-168">Si lo prefiere, puede configurar **XmlMediaTypeFormatter** para que utilice **XmlSerializer** en lugar de **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="49bb1-169">Para ello, establezca la propiedad **UseXmlSerializer en** **true:**</span><span class="sxs-lookup"><span data-stu-id="49bb1-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="49bb1-170">La clase **XmlSerializer** admite un conjunto de tipos más estrecho que **DataContractSerializer**, pero proporciona más control sobre el XML resultante.</span><span class="sxs-lookup"><span data-stu-id="49bb1-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="49bb1-171">Considere la posibilidad de usar **XmlSerializer** si necesita coincidir con un esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="49bb1-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="49bb1-172">Serialización XML</span><span class="sxs-lookup"><span data-stu-id="49bb1-172">XML Serialization</span></span>

<span data-ttu-id="49bb1-173">En esta sección se describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="49bb1-174">De forma predeterminada, DataContractSerializer se comporta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="49bb1-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="49bb1-175">Se serializan todas las propiedades y campos públicos de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="49bb1-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="49bb1-176">Para omitir una propiedad o un campo, decórelo con el atributo **IgnoreDataMember.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="49bb1-177">Los miembros privados y protegidos no se serializan.</span><span class="sxs-lookup"><span data-stu-id="49bb1-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="49bb1-178">Las propiedades de solo lectura no se serializan.</span><span class="sxs-lookup"><span data-stu-id="49bb1-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="49bb1-179">(Sin embargo, se serializa el contenido de una propiedad de colección de solo lectura.)</span><span class="sxs-lookup"><span data-stu-id="49bb1-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="49bb1-180">Los nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="49bb1-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="49bb1-181">Se utiliza un espacio de nombres XML predeterminado.</span><span class="sxs-lookup"><span data-stu-id="49bb1-181">A default XML namespace is used.</span></span>

<span data-ttu-id="49bb1-182">Si necesita más control sobre la serialización, puede decorar la clase con el **atributo DataContract.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="49bb1-183">Cuando este atributo está presente, la clase se serializa de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="49bb1-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="49bb1-184">&quot;&quot; Enfoque de suscripción: las propiedades y los campos no se serializan de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="49bb1-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="49bb1-185">Para serializar una propiedad o un campo, decórelo con el atributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="49bb1-186">Para serializar un miembro privado o protegido, decórelo con el atributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="49bb1-187">Las propiedades de solo lectura no se serializan.</span><span class="sxs-lookup"><span data-stu-id="49bb1-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="49bb1-188">Para cambiar cómo aparece el nombre de clase en el XML, establezca el parámetro *Name* en el atributo **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="49bb1-189">Para cambiar cómo aparece un nombre de miembro en el XML, establezca el parámetro *Name* en el atributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="49bb1-190">Para cambiar el espacio de nombres XML, establezca el parámetro *Namespace* en la clase **DataContract.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="49bb1-191">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="49bb1-191">Read-Only Properties</span></span>

<span data-ttu-id="49bb1-192">Las propiedades de solo lectura no se serializan.</span><span class="sxs-lookup"><span data-stu-id="49bb1-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="49bb1-193">Si una propiedad de solo lectura tiene un campo privado de respaldo, puede marcar el campo privado con el atributo **DataMember.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="49bb1-194">Este enfoque requiere el atributo **DataContract** de la clase.</span><span class="sxs-lookup"><span data-stu-id="49bb1-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="49bb1-195">Fechas</span><span class="sxs-lookup"><span data-stu-id="49bb1-195">Dates</span></span>

<span data-ttu-id="49bb1-196">Las fechas están escritas en formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="49bb1-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="49bb1-197">Por ejemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="49bb1-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="49bb1-198">Sangrías</span><span class="sxs-lookup"><span data-stu-id="49bb1-198">Indenting</span></span>

<span data-ttu-id="49bb1-199">Para escribir XML con sangría, establezca la propiedad **Indent** en **true:**</span><span class="sxs-lookup"><span data-stu-id="49bb1-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="49bb1-200">Configuración de serializadores XML por tipo</span><span class="sxs-lookup"><span data-stu-id="49bb1-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="49bb1-201">Puede establecer serializadores XML diferentes para diferentes tipos clr.</span><span class="sxs-lookup"><span data-stu-id="49bb1-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="49bb1-202">Por ejemplo, puede tener un objeto de datos determinado que requiere XmlSerializer para la compatibilidad con **versiones anteriores.**</span><span class="sxs-lookup"><span data-stu-id="49bb1-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="49bb1-203">Puede usar **XmlSerializer** para este objeto y seguir utilizando **DataContractSerializer** para otros tipos.</span><span class="sxs-lookup"><span data-stu-id="49bb1-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="49bb1-204">Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="49bb1-205">Puede especificar un **XmlSerializer** o cualquier objeto que derive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="49bb1-206">Eliminación del formateador JSON o XML</span><span class="sxs-lookup"><span data-stu-id="49bb1-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="49bb1-207">Puede quitar el formateador JSON o el formateador XML de la lista de formateadores, si no desea utilizarlos.</span><span class="sxs-lookup"><span data-stu-id="49bb1-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="49bb1-208">Las principales razones para hacer esto son:</span><span class="sxs-lookup"><span data-stu-id="49bb1-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="49bb1-209">Para restringir las respuestas de la API web a un tipo de medio determinado.</span><span class="sxs-lookup"><span data-stu-id="49bb1-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="49bb1-210">Por ejemplo, puede decidir admitir solo respuestas JSON y quitar el formateador XML.</span><span class="sxs-lookup"><span data-stu-id="49bb1-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="49bb1-211">Para reemplazar el formateador predeterminado con un formateador personalizado.</span><span class="sxs-lookup"><span data-stu-id="49bb1-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="49bb1-212">Por ejemplo, podría reemplazar el formateador JSON con su propia implementación personalizada de un formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="49bb1-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="49bb1-213">El código siguiente muestra cómo quitar los formateadores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="49bb1-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="49bb1-214">Llame a esto desde el método **\_Application Start,** definido en Global.asax.</span><span class="sxs-lookup"><span data-stu-id="49bb1-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="49bb1-215">Manejo de referencias circulares de objetos</span><span class="sxs-lookup"><span data-stu-id="49bb1-215">Handling Circular Object References</span></span>

<span data-ttu-id="49bb1-216">De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="49bb1-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="49bb1-217">Si dos propiedades hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces.</span><span class="sxs-lookup"><span data-stu-id="49bb1-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="49bb1-218">Este es un problema particular si el gráfico de objetos contiene ciclos, porque el serializador producirá una excepción cuando detecte un bucle en el gráfico.</span><span class="sxs-lookup"><span data-stu-id="49bb1-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="49bb1-219">Tenga en cuenta los siguientes modelos de objetos y controlador.</span><span class="sxs-lookup"><span data-stu-id="49bb1-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="49bb1-220">Invocar esta acción hará que el formateador produzca una excepción, que se traduce en una respuesta de código de estado 500 (Error interno del servidor) al cliente.</span><span class="sxs-lookup"><span data-stu-id="49bb1-220">Invoking this action will cause the formatter to throw an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="49bb1-221">Para conservar las referencias a objetos en JSON, agregue el código siguiente al método **Application\_Start** en el archivo Global.asax:</span><span class="sxs-lookup"><span data-stu-id="49bb1-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="49bb1-222">Ahora la acción del controlador devolverá JSON que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="49bb1-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="49bb1-223">Observe que el serializador agrega una &quot;propiedad $id&quot; a ambos objetos.</span><span class="sxs-lookup"><span data-stu-id="49bb1-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="49bb1-224">Además, detecta que la propiedad Employee.Department crea un bucle, por&quot;lo&quot;&quot;que&quot;reemplaza el valor por una referencia de objeto: á $ref: 1 .</span><span class="sxs-lookup"><span data-stu-id="49bb1-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="49bb1-225">Las referencias a objetos no son estándar en JSON.</span><span class="sxs-lookup"><span data-stu-id="49bb1-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="49bb1-226">Antes de usar esta característica, considere si sus clientes podrán analizar los resultados.</span><span class="sxs-lookup"><span data-stu-id="49bb1-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="49bb1-227">Podría ser mejor simplemente eliminar ciclos del gráfico.</span><span class="sxs-lookup"><span data-stu-id="49bb1-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="49bb1-228">Por ejemplo, el vínculo de Empleado de vuelta al Departamento no es realmente necesario en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="49bb1-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="49bb1-229">Para conservar las referencias a objetos en XML, tiene dos opciones.</span><span class="sxs-lookup"><span data-stu-id="49bb1-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="49bb1-230">La opción más sencilla `[DataContract(IsReference=true)]` es agregar a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="49bb1-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="49bb1-231">El parámetro *IsReference* habilita las referencias a objetos.</span><span class="sxs-lookup"><span data-stu-id="49bb1-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="49bb1-232">Recuerde que **DataContract** hace que la serialización opt-in, por lo que también tendrá que agregar **DataMember** atributos a las propiedades:</span><span class="sxs-lookup"><span data-stu-id="49bb1-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="49bb1-233">Ahora el formateador producirá XML similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49bb1-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="49bb1-234">Si desea evitar atributos en la clase de modelo, hay otra opción: crear una nueva instancia de **DataContractSerializer** específica del tipo y establecer *preserveObjectReferences* en **true** en el constructor.</span><span class="sxs-lookup"><span data-stu-id="49bb1-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="49bb1-235">A continuación, establezca esta instancia como serializador por tipo en el formateador de tipo de medio XML.</span><span class="sxs-lookup"><span data-stu-id="49bb1-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="49bb1-236">El código siguiente muestra cómo hacerlo:</span><span class="sxs-lookup"><span data-stu-id="49bb1-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="49bb1-237">Prueba de la serialización de objetos</span><span class="sxs-lookup"><span data-stu-id="49bb1-237">Testing Object Serialization</span></span>

<span data-ttu-id="49bb1-238">Al diseñar la API web, es útil probar cómo se serializarán los objetos de datos.</span><span class="sxs-lookup"><span data-stu-id="49bb1-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="49bb1-239">Puede hacerlo sin crear un controlador o invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="49bb1-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
