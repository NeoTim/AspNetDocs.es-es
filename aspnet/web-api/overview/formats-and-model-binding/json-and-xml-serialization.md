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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serialización JSON y XML en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describen los formateadores JSON y XML de ASP.NET Web API.

En ASP.NET Web API, un *formateador de tipo multimedia* es un objeto que puede:

- Leer objetos CLR desde un cuerpo de mensaje HTTP
- Escribir objetos CLR en un cuerpo de mensaje HTTP

Web API proporciona formateadores de tipo multimedia para JSON y XML. El marco de trabajo inserta estos formateadores en la canalización de forma predeterminada. Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.

## <a name="contents"></a>Contenido

- [Formatter de tipo multimedia JSON](#json_media_type_formatter)

    - [Propiedades de solo lectura](#json_readonly)
    - [Fechas](#json_dates)
    - [Sangrías](#json_indenting)
    - [Camel Casing](#json_camelcasing)
    - [Objetos anónimos y débilmente tipados](#json_anon)
- [Forador de tipo de medios XML](#xml_media_type_formatter)

    - [Propiedades de solo lectura](#xml_readonly)
    - [Fechas](#xml_dates)
    - [Sangrías](#xml_indenting)
    - [Configuración de serializadores XML por tipo](#xml_pertype)
- [Eliminación del formateador JSON o XML](#removing_the_json_or_xml_formatter)
- [Manejo de referencias circulares de objetos](#handling_circular_object_references)
- [Prueba de la serialización de objetos](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formatter de tipo multimedia JSON

La clase **JsonMediaTypeFormatter** proporciona el formato JSON. De forma predeterminada, **JsonMediaTypeFormatter** utiliza la biblioteca [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) para realizar la serialización. Json.NET es un proyecto de código abierto de terceros.

Si lo prefiere, puede configurar la clase **JsonMediaTypeFormatter** para que use **DataContractJsonSerializer** en lugar de Json.NET. Para ello, establezca la propiedad **UseDataContractJsonSerializer** en **true:**

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialización de JSON

En esta sección se describen algunos comportamientos específicos del formateador JSON, utilizando el serializador [de Json.NET](https://github.com/JamesNK/Newtonsoft.Json) predeterminado. Esto no pretende ser una documentación completa de la biblioteca de Json.NET; Para obtener más información, consulte la [Documentación de Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>¿Qué se serializa?

De forma predeterminada, todas las propiedades públicas y los campos se incluyen en el JSON serializado. Para omitir una propiedad o un campo, decórelo con el atributo **JsonIgnore.**

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si prefiere &quot;un enfoque&quot; opt-in, decore la clase con el atributo **DataContract.** Si este atributo está presente, se omiten los miembros a menos que tengan el **DataMember**. También puede usar **DataMember** para serializar miembros privados.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

Las propiedades de solo lectura se serializan de forma predeterminada.

<a id="json_dates"></a>
### <a name="dates"></a>Fechas

De forma predeterminada, Json.NET escribe fechas en formato [ISO 8601.](http://www.w3.org/TR/NOTE-datetime) Las fechas en UTC (hora universal coordinada) se escriben con un sufijo "Z". Las fechas en la hora local incluyen un desplazamiento de zona horaria. Por ejemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

De forma predeterminada, Json.NET conserva la zona horaria. Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si prefiere utilizar el formato`"\/Date(ticks)\/"`de fecha JSON de [Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) ( ) en lugar de ISO 8601, establezca la propiedad **DateFormatHandling** en la configuración del serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Sangrías

Para escribir JSON con sangría, establezca el valor **Formato** en **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camel Casing

Para escribir nombres de propiedad JSON con mayúsculas y minúsculas, sin cambiar el modelo de datos, establezca **CamelCasePropertyNamesContractResolver** en el serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anónimos y débilmente tipados

Un método de acción puede devolver un objeto anónimo y serializarlo en JSON. Por ejemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

El cuerpo del mensaje de respuesta contendrá el siguiente JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si la API web recibe objetos JSON estructurados de forma flexible de los clientes, puede deserializar el cuerpo de la solicitud a un tipo **Newtonsoft.Json.Linq.JObject.**

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Sin embargo, normalmente es mejor usar objetos de datos fuertemente tipados. A continuación, no es necesario analizar los datos usted mismo y obtiene las ventajas de la validación del modelo.

El serializador XML no admite tipos anónimos ni instancias **JObject.** Si utiliza estas características para los datos JSON, debe quitar el formateador XML de la canalización, como se describe más adelante en este artículo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Forador de tipo de medios XML

La clase **XmlMediaTypeFormatter** proporciona formato XML. De forma predeterminada, **XmlMediaTypeFormatter** utiliza la **clase DataContractSerializer** para realizar la serialización.

Si lo prefiere, puede configurar **XmlMediaTypeFormatter** para que utilice **XmlSerializer** en lugar de **DataContractSerializer**. Para ello, establezca la propiedad **UseXmlSerializer en** **true:**

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

La clase **XmlSerializer** admite un conjunto de tipos más estrecho que **DataContractSerializer**, pero proporciona más control sobre el XML resultante. Considere la posibilidad de usar **XmlSerializer** si necesita coincidir con un esquema XML existente.

### <a name="xml-serialization"></a>Serialización XML

En esta sección se describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.

De forma predeterminada, DataContractSerializer se comporta de la siguiente manera:

- Se serializan todas las propiedades y campos públicos de lectura y escritura. Para omitir una propiedad o un campo, decórelo con el atributo **IgnoreDataMember.**
- Los miembros privados y protegidos no se serializan.
- Las propiedades de solo lectura no se serializan. (Sin embargo, se serializa el contenido de una propiedad de colección de solo lectura.)
- Los nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.
- Se utiliza un espacio de nombres XML predeterminado.

Si necesita más control sobre la serialización, puede decorar la clase con el **atributo DataContract.** Cuando este atributo está presente, la clase se serializa de la siguiente manera:

- &quot;&quot; Enfoque de suscripción: las propiedades y los campos no se serializan de forma predeterminada. Para serializar una propiedad o un campo, decórelo con el atributo **DataMember.**
- Para serializar un miembro privado o protegido, decórelo con el atributo **DataMember.**
- Las propiedades de solo lectura no se serializan.
- Para cambiar cómo aparece el nombre de clase en el XML, establezca el parámetro *Name* en el atributo **DataContract.**
- Para cambiar cómo aparece un nombre de miembro en el XML, establezca el parámetro *Name* en el atributo **DataMember.**
- Para cambiar el espacio de nombres XML, establezca el parámetro *Namespace* en la clase **DataContract.**

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

Las propiedades de solo lectura no se serializan. Si una propiedad de solo lectura tiene un campo privado de respaldo, puede marcar el campo privado con el atributo **DataMember.** Este enfoque requiere el atributo **DataContract** de la clase.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Fechas

Las fechas están escritas en formato ISO 8601. Por ejemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Sangrías

Para escribir XML con sangría, establezca la propiedad **Indent** en **true:**

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Configuración de serializadores XML por tipo

Puede establecer serializadores XML diferentes para diferentes tipos clr. Por ejemplo, puede tener un objeto de datos determinado que requiere XmlSerializer para la compatibilidad con **versiones anteriores.** Puede usar **XmlSerializer** para este objeto y seguir utilizando **DataContractSerializer** para otros tipos.

Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Puede especificar un **XmlSerializer** o cualquier objeto que derive de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Eliminación del formateador JSON o XML

Puede quitar el formateador JSON o el formateador XML de la lista de formateadores, si no desea utilizarlos. Las principales razones para hacer esto son:

- Para restringir las respuestas de la API web a un tipo de medio determinado. Por ejemplo, puede decidir admitir solo respuestas JSON y quitar el formateador XML.
- Para reemplazar el formateador predeterminado con un formateador personalizado. Por ejemplo, podría reemplazar el formateador JSON con su propia implementación personalizada de un formateador JSON.

El código siguiente muestra cómo quitar los formateadores predeterminados. Llame a esto desde el método **\_Application Start,** definido en Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Manejo de referencias circulares de objetos

De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores. Si dos propiedades hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces. Este es un problema particular si el gráfico de objetos contiene ciclos, porque el serializador producirá una excepción cuando detecte un bucle en el gráfico.

Tenga en cuenta los siguientes modelos de objetos y controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Invocar esta acción hará que el formateador produzca una excepción, que se traduce en una respuesta de código de estado 500 (Error interno del servidor) al cliente.

Para conservar las referencias a objetos en JSON, agregue el código siguiente al método **Application\_Start** en el archivo Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Ahora la acción del controlador devolverá JSON que tiene este aspecto:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Observe que el serializador agrega una &quot;propiedad $id&quot; a ambos objetos. Además, detecta que la propiedad Employee.Department crea un bucle, por&quot;lo&quot;&quot;que&quot;reemplaza el valor por una referencia de objeto: á $ref: 1 .

> [!NOTE]
> Las referencias a objetos no son estándar en JSON. Antes de usar esta característica, considere si sus clientes podrán analizar los resultados. Podría ser mejor simplemente eliminar ciclos del gráfico. Por ejemplo, el vínculo de Empleado de vuelta al Departamento no es realmente necesario en este ejemplo.

Para conservar las referencias a objetos en XML, tiene dos opciones. La opción más sencilla `[DataContract(IsReference=true)]` es agregar a la clase de modelo. El parámetro *IsReference* habilita las referencias a objetos. Recuerde que **DataContract** hace que la serialización opt-in, por lo que también tendrá que agregar **DataMember** atributos a las propiedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Ahora el formateador producirá XML similar a lo siguiente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si desea evitar atributos en la clase de modelo, hay otra opción: crear una nueva instancia de **DataContractSerializer** específica del tipo y establecer *preserveObjectReferences* en **true** en el constructor. A continuación, establezca esta instancia como serializador por tipo en el formateador de tipo de medio XML. El código siguiente muestra cómo hacerlo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Prueba de la serialización de objetos

Al diseñar la API web, es útil probar cómo se serializarán los objetos de datos. Puede hacerlo sin crear un controlador o invocar una acción de controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
