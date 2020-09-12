---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Descripción de los servicios Web de ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Los servicios web son una parte integral de .NET Framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque Web...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: eac3d53fd871d0cb5a2870488ce752c057cc5b1a
ms.sourcegitcommit: 45754124123403520b9fa2e706a4d1292494159b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2020
ms.locfileid: "86162842"
---
# <a name="understanding-aspnet-ajax-web-services"></a>Descripción de los servicios web de ASP.NET AJAX

por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Los servicios web son una parte integral de .NET Framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque los servicios web se usan normalmente para permitir que distintos sistemas operativos, modelos de objetos y lenguajes de programación envíen y reciban datos, también se pueden usar para insertar datos de forma dinámica en una página de ASP.NET AJAX o para enviar datos de una página a un sistema back-end. Todo esto se puede hacer sin tener que recurrir a las operaciones de postback.

## <a name="calling-web-services-with-aspnet-ajax"></a>Llamada a servicios web con ASP.NET AJAX

Dan Wahlin

Los servicios web son una parte integral de .NET Framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque los servicios web se usan normalmente para permitir que distintos sistemas operativos, modelos de objetos y lenguajes de programación envíen y reciban datos, también se pueden usar para insertar datos de forma dinámica en una página de ASP.NET AJAX o para enviar datos de una página a un sistema back-end. Todo esto se puede hacer sin tener que recurrir a las operaciones de postback.

Aunque el control UpdatePanel de AJAX de ASP.NET proporciona una manera sencilla de que AJAX habilite cualquier página de ASP.NET, puede haber ocasiones en las que necesite tener acceso de forma dinámica a los datos del servidor sin usar un UpdatePanel. En este artículo, verá cómo hacerlo creando y consumiendo servicios web en páginas de ASP.NET AJAX.

Este artículo se centrará en la funcionalidad disponible en las extensiones AJAX de Core ASP.NET, así como un control de servicio web habilitado en el kit de herramientas de ASP.NET AJAX denominado AutoCompleteExtender. Entre los temas tratados se incluyen la definición de servicios web habilitados para AJAX, la creación de proxies de cliente y la llamada a servicios web con JavaScript. También verá cómo se pueden realizar llamadas de servicio web directamente a los métodos de página de ASP.NET.

## <a name="web-services-configuration"></a>Configuración de servicios Web

Cuando se crea un nuevo proyecto de sitio web con Visual Studio 2008, el archivo de web.config tiene una serie de nuevas adiciones que pueden ser desconocidas para los usuarios de versiones anteriores de Visual Studio. Algunas de estas modificaciones asignan el prefijo "ASP" a los controles de AJAX de ASP.NET para que se puedan usar en páginas mientras que otras definen HttpHandlers y HttpModules necesarios. En la lista 1 se muestran las modificaciones realizadas en el `<httpHandlers>` elemento en web.config que afectan a las llamadas del servicio Web. El HttpHandler predeterminado usado para procesar llamadas. asmx se quita y se reemplaza por una clase ScriptHandlerFactory ubicada en el ensamblado System.Web.Extensions.dll. System.Web.Extensions.dll contiene toda la funcionalidad básica que usa ASP.NET AJAX.

**Lista 1. Configuración del controlador del servicio Web de ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Este reemplazo de HttpHandler se realiza para permitir que se realicen llamadas notación de objetos JavaScript (JSON) desde páginas de ASP.NET AJAX a servicios Web de .NET mediante un proxy de servicio Web de JavaScript. ASP.NET AJAX envía mensajes JSON a los servicios web en lugar de a las llamadas estándar del Protocolo simple de acceso a objetos (SOAP) asociadas normalmente a los servicios Web. Esto genera mensajes de solicitud y respuesta más pequeños en general. También permite un procesamiento de datos de lado cliente más eficaz, ya que la biblioteca de JavaScript de ASP.NET AJAX está optimizada para funcionar con objetos JSON. La lista 2 y la lista 3 muestran ejemplos de mensajes de solicitud y respuesta de servicio Web serializados en formato JSON. El mensaje de solicitud mostrado en la lista 2 pasa un parámetro Country con un valor de "Bélgica" mientras que el mensaje de respuesta de la lista 3 pasa una matriz de objetos Customer y sus propiedades asociadas.

**Lista 2. Mensaje de solicitud de servicio Web serializado a JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *>[!NOTE]el nombre de la operación se define como parte de la dirección URL del servicio web; además, los mensajes de solicitud no se envían siempre a través de JSON. Los servicios Web pueden usar el atributo ScriptMethod con el parámetro UseHttpGet establecido en true, lo que hace que los parámetros se pasen a través de los parámetros de cadena de consulta.*

**Lista 3. Mensaje de respuesta de servicio Web serializado a JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

En la sección siguiente, verá cómo crear servicios Web capaces de administrar mensajes de solicitud JSON y responder con tipos simples y complejos.

## <a name="creating-ajax-enabled-web-services"></a>Creación de servicios web habilitados para AJAX

El marco de ASP.NET AJAX proporciona varias maneras de llamar a los servicios Web. Puede usar el control AutoCompleteExtender (disponible en el kit de herramientas de ASP.NET AJAX) o JavaScript. Sin embargo, antes de llamar a un servicio, tiene que habilitar AJAX para que se pueda llamar mediante código de script de cliente.

Si no está familiarizado con los servicios Web de ASP.NET, le resultará sencillo crear y habilitar los servicios de AJAX. .NET Framework admite la creación de servicios Web de ASP.NET desde su lanzamiento inicial en 2002 y las extensiones de AJAX de ASP.NET proporcionan funcionalidad AJAX adicional que se basa en el conjunto predeterminado de características de .NET Framework. Visual Studio .NET 2008 beta 2 tiene compatibilidad integrada para crear archivos de servicio Web. asmx y deriva automáticamente el código asociado junto con las clases de la clase System. Web. Services. WebService. A medida que agrega métodos a la clase, debe aplicar el atributo WebMethod para que lo llamen los consumidores del servicio Web.

En la lista 4 se muestra un ejemplo de cómo aplicar el atributo WebMethod a un método denominado GetCustomersByCountry ().

**Lista 4. Usar el atributo WebMethod en un servicio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

El método GetCustomersByCountry () acepta un parámetro Country y devuelve una matriz de objetos Customer. El valor de país que se pasa al método se reenvía a una clase de capa de negocio que, a su vez, llama a una clase de capa de datos para recuperar los datos de la base de datos, rellenar las propiedades de objeto de cliente con datos y devolver la matriz.

## <a name="using-the-scriptservice-attribute"></a>Usar el atributo ScriptService

Aunque agregar el atributo WebMethod permite que los clientes que envían mensajes SOAP estándar al servicio Web llamen al método GetCustomersByCountry (), no permite que se realicen llamadas JSON desde aplicaciones de AJAX de ASP.NET. Para permitir que se realicen llamadas JSON, tiene que aplicar el atributo de la extensión de ASP.NET AJAX `ScriptService` a la clase de servicio Web. Esto permite a un servicio Web enviar mensajes de respuesta con formato JSON y permite que el script del lado cliente llame a un servicio mediante el envío de mensajes JSON.

En la lista 5 se muestra un ejemplo de cómo aplicar el atributo ScriptService a una clase de servicio web denominada CustomersService.

**Lista 5. Usar el atributo ScriptService para habilitar AJAX para un servicio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

El atributo ScriptService actúa como un marcador que indica que se puede llamar desde el código de script AJAX. En realidad, no controla ninguna de las tareas de serialización o deserialización de JSON que se producen en segundo plano. La ScriptHandlerFactory (configurada en web.config) y otras clases relacionadas realizan la mayor parte del procesamiento de JSON.

## <a name="using-the-scriptmethod-attribute"></a>Usar el atributo ScriptMethod

El atributo ScriptService es el único atributo de AJAX de ASP.NET que debe definirse en un servicio Web .NET para que lo usen las páginas de ASP.NET AJAX. Sin embargo, otro atributo denominado ScriptMethod también se puede aplicar directamente a los métodos Web en un servicio. ScriptMethod define tres propiedades, entre las que se incluyen `UseHttpGet` , `ResponseFormat` y `XmlSerializeString` . Cambiar los valores de estas propiedades puede ser útil en los casos en los que el tipo de solicitud aceptado por un método Web debe cambiarse a GET, cuando un método Web necesite devolver datos XML sin formato en forma de `XmlDocument` un `XmlElement` objeto o, o cuando los datos devueltos de un servicio se deben serializar siempre como XML en lugar de como JSON.

La propiedad UseHttpGet se puede usar cuando un método Web debe aceptar solicitudes GET en lugar de solicitudes POST. Las solicitudes se envían mediante una dirección URL con parámetros de entrada de método Web convertidos en parámetros QueryString. El valor predeterminado de la propiedad UseHttpGet es false y solo se debe establecer en `true` cuando se sabe que las operaciones son seguras y cuando no se pasan datos confidenciales a un servicio Web. En el listado 6 se muestra un ejemplo del uso del atributo ScriptMethod con la propiedad UseHttpGet.

**Listado 6. Usar el atributo ScriptMethod con la propiedad UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

A continuación se muestra un ejemplo de los encabezados que se envían cuando se llama al método Web HttpGetEcho que se muestra en la lista 6:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Además de permitir que los métodos Web acepten solicitudes HTTP GET, el atributo ScriptMethod también se puede usar cuando es necesario devolver respuestas XML desde un servicio en lugar de JSON. Por ejemplo, un servicio web puede recuperar una fuente RSS de un sitio remoto y devolverlo como un objeto XmlDocument o XmlElement. El procesamiento de los datos XML se puede producir en el cliente.

En el listado 7 se muestra un ejemplo del uso de la propiedad ResponseFormat para especificar que los datos XML deben devolverse desde un método Web.

**Listado 7. Usar el atributo ScriptMethod con la propiedad ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

La propiedad ResponseFormat también se puede usar junto con la propiedad XmlSerializeString. La propiedad XmlSerializeString tiene un valor predeterminado de false, lo que significa que todos los tipos de valor devueltos excepto las cadenas devueltas por un método Web se serializan como XML cuando la `ResponseFormat` propiedad está establecida en `ResponseFormat.Xml` . Cuando `XmlSerializeString` se establece en `true` , todos los tipos devueltos de un método Web se serializan como XML, incluidos los tipos de cadena. Si la propiedad ResponseFormat tiene un valor de `ResponseFormat.Json` la propiedad XmlSerializeString, se omite.

La lista 8 muestra un ejemplo del uso de la propiedad XmlSerializeString para forzar que las cadenas se serialicen como XML.

**Listado 8. Usar el atributo ScriptMethod con la propiedad XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

El valor devuelto desde la llamada al método Web GetXmlString que se muestra en la lista 8 se muestra a continuación:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Aunque el formato JSON predeterminado minimiza el tamaño total de los mensajes de solicitud y respuesta, y es más fácil de usar por los clientes de ASP.NET AJAX de una manera entre exploradores, las propiedades ResponseFormat y XmlSerializeString se pueden usar cuando las aplicaciones cliente como Internet Explorer 5 o una versión posterior esperan que se devuelvan datos XML desde un método Web.

## <a name="working-with-complex-types"></a>Trabajar con tipos complejos

En la lista 5 se mostró un ejemplo de cómo devolver un tipo complejo denominado Customer de un servicio Web. La clase Customer define varios tipos simples diferentes internamente como propiedades como FirstName y LastName. Los tipos complejos usados como parámetro de entrada o tipo de valor devuelto en un método Web habilitado para AJAX se serializan automáticamente en JSON antes de enviarse al lado cliente. Sin embargo, los tipos complejos anidados (los definidos internamente dentro de otro tipo) no se ponen a disposición del cliente como objetos independientes de forma predeterminada.

En los casos en los que un tipo complejo anidado utilizado por un servicio web también debe usarse en una página cliente, el atributo GenerateScriptType de ASP.NET AJAX se puede Agregar al servicio Web. Por ejemplo, la clase CustomerDetails que se muestra en la lista 9 contiene las propiedades address y Gender que ***representan tipos complejos anidados.***

**Listado 9. La clase CustomerDetails que se muestra aquí contiene dos tipos complejos anidados.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Los objetos dirección y género definidos dentro de la clase CustomerDetails que se muestra en la lista 9 no estarán disponibles automáticamente para su uso en el lado cliente a través de JavaScript, ya que son tipos anidados (la dirección es una clase y el sexo es una enumeración). En situaciones en las que un tipo anidado que se usa en un servicio Web debe estar disponible en el lado cliente, se puede usar el atributo GenerateScriptType mencionado anteriormente (vea el listado 10). Este atributo se puede agregar varias veces en casos en los que se devuelven diferentes tipos complejos anidados de un servicio. Se puede aplicar directamente a la clase de servicio web o sobre métodos Web específicos.

**Listado 10. Usar el atributo GenerateScriptService para definir tipos anidados que deben estar disponibles para el cliente.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Al aplicar el `GenerateScriptType` atributo al servicio Web, los tipos de dirección y sexo estarán disponibles automáticamente para su uso por parte del código JavaScript de ASP.net Ajax del lado cliente. En el listado 11 se muestra un ejemplo del código JavaScript que se genera automáticamente y se envía al cliente mediante la adición del atributo GenerateScriptType en un servicio Web. Verá cómo usar los tipos complejos anidados más adelante en el artículo.

**Enumeración 11. Los tipos complejos anidados se ponen a disposición de una página de ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Ahora que ha visto cómo crear servicios web y hacerlos accesibles para páginas de ASP.NET AJAX, echemos un vistazo a cómo crear y usar servidores proxy de JavaScript para que los datos se puedan recuperar o enviar a servicios Web.

## <a name="creating-javascript-proxies"></a>Crear servidores proxy de JavaScript

La llamada a un servicio web estándar (.NET u otra plataforma) implica normalmente la creación de un objeto proxy que le protege de las complejidades del envío de mensajes de solicitud y respuesta SOAP. Con las llamadas al servicio Web AJAX de ASP.NET, se pueden crear y usar servidores proxy de JavaScript para llamar fácilmente a los servicios sin preocuparse por la serialización y deserialización de los mensajes JSON. Los proxies de JavaScript se pueden generar automáticamente mediante el control ScriptManager de ASP.NET AJAX.

La creación de un proxy de JavaScript que puede llamar a los servicios web se realiza mediante la propiedad de los servicios de ScriptManager. Esta propiedad permite definir uno o más servicios a los que una página ASP.NET AJAX puede llamar de forma asincrónica para enviar o recibir datos sin necesidad de operaciones de postback. Un servicio se define mediante el control de AJAX de ASP.NET `ServiceReference` y se asigna la dirección URL del servicio Web a la `Path` propiedad del control. La lista 12 muestra un ejemplo de cómo hacer referencia a un servicio denominado CustomersService. asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Listado 12. Definición de un servicio Web usado en una página de ASP.NET AJAX.**

Al agregar una referencia a CustomersService. asmx a través del control ScriptManager, se genera dinámicamente un proxy de JavaScript y la página hace referencia a él. El proxy se incrusta mediante la &lt; etiqueta de script &gt; y se carga dinámicamente llamando al archivo CustomersService. asmx y anexando/JS al final del mismo. En el ejemplo siguiente se muestra cómo el proxy de JavaScript se incrusta en la página cuando la depuración está deshabilitada en web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *>[!NOTE]Si desea ver el código de proxy de JavaScript real que se genera, puede escribir la dirección URL del servicio Web de .net deseado en el cuadro de dirección de Internet Explorer y anexar/JS al final de la misma.*

Si la depuración está habilitada en web.config se incrustará en la página una versión de depuración del proxy de JavaScript, tal como se muestra a continuación:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

El proxy de JavaScript creado por ScriptManager también se puede insertar directamente en la página en lugar de hacer referencia a él mediante el &lt; &gt; atributo src de la etiqueta de script. Esto puede hacerse estableciendo la propiedad InlineScript del control ServiceReference en true (el valor predeterminado es false). Esto puede ser útil cuando un proxy no se comparte entre varias páginas y cuando desea reducir el número de llamadas de red realizadas al servidor. Cuando InlineScript está establecido en true, el explorador no almacenará en caché el script de proxy, por lo que se recomienda el valor predeterminado False en los casos en los que varias páginas de una aplicación ASP.NET AJAX usan el proxy. A continuación se muestra un ejemplo del uso de la propiedad InlineScript:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Uso de servidores proxy de JavaScript

Una vez que se hace referencia a un servicio Web mediante una página de ASP.NET AJAX mediante el control ScriptManager, se puede realizar una llamada al servicio Web y los datos devueltos se pueden controlar mediante funciones de devolución de llamada. Se llama a un servicio web haciendo referencia a su espacio de nombres (si existe), nombre de clase y nombre del método Web. Los parámetros que se pasan al servicio Web se pueden definir junto con una función de devolución de llamada que controla los datos devueltos.

Un ejemplo del uso de un proxy de JavaScript para llamar a un método Web denominado GetCustomersByCountry () se muestra en la lista 13. Se llama a la función GetCustomersByCountry () cuando un usuario final hace clic en un botón de la página.

**Enumeración 13. Llamar a un servicio Web con un proxy de JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Esta llamada hace referencia al espacio de nombres InterfaceTraining, la clase CustomersService y el método Web GetCustomersByCountry definidos en el servicio. Pasa un valor de país Obtenido de un cuadro de texto, así como una función de devolución de llamada denominada OnWSRequestComplete que debe invocarse cuando se devuelve la llamada de servicio Web asincrónica. OnWSRequestComplete controla la matriz de objetos Customer que devuelve el servicio y los convierte en una tabla que se muestra en la página. La salida generada a partir de la llamada se muestra en la figura 1.

[![Enlazar datos obtenidos mediante la realización de una llamada AJAX asincrónica a un servicio Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: enlace de datos obtenidos mediante la realización de una llamada AJAX asincrónica a un servicio Web.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-web-services/_static/image3.png))

Los proxies de JavaScript también pueden realizar llamadas unidireccionales a servicios web en los casos en los que se debe llamar a un método Web pero el proxy no debe esperar una respuesta. Por ejemplo, puede que desee llamar a un servicio web para iniciar un proceso, como un flujo de trabajo, pero no esperar un valor devuelto por el servicio. En los casos en los que es necesario realizar una llamada unidireccional a un servicio, la función de devolución de llamada que se muestra en la lista 13 simplemente se puede omitir. Dado que no se define ninguna función de devolución de llamada, el objeto proxy no esperará a que el servicio Web devuelva datos.

## <a name="handling-errors"></a>Control de errores

Las devoluciones de llamada asincrónicas a los servicios Web pueden encontrar diferentes tipos de errores, como la red que está inactiva, el servicio Web no está disponible o se devuelve una excepción. Afortunadamente, los objetos proxy de JavaScript generados por ScriptManager permiten definir varias devoluciones de llamada para controlar errores y errores, además de la devolución de llamada de operación correcta mostrada anteriormente. Se puede definir una función de devolución de llamada de error inmediatamente después de la función de devolución de llamada estándar en la llamada al método Web, tal como se muestra en la lista 14.

**Listado 14. Definir una función de devolución de llamada de error y mostrar errores.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Cualquier error que se produzca cuando se llame al servicio Web desencadenará la llamada a la función de devolución de llamada OnWSRequestFailed (), que acepta un objeto que representa el error como parámetro. El objeto error expone varias funciones diferentes para determinar la causa del error, así como si se ha agotado el tiempo de espera de la llamada. La lista 14 muestra un ejemplo del uso de las distintas funciones de error y la figura 2 muestra un ejemplo de la salida generada por las funciones.

[![Salida generada al llamar a funciones de error de ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: salida generada al llamar a las funciones de error de ASP.NET AJAX.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-web-services/_static/image6.png))

## <a name="handling-xml-data-returned-from-a-web-service"></a>Control de los datos XML devueltos por un servicio Web

Antes vio cómo un método Web podía devolver datos XML sin formato mediante el atributo ScriptMethod junto con su propiedad ResponseFormat. Cuando ResponseFormat se establece en ResponseFormat.Xml, los datos devueltos del servicio Web se serializan como XML en lugar de JSON. Esto puede ser útil cuando es necesario pasar datos XML directamente al cliente para su procesamiento mediante JavaScript o XSLT. En la hora actual, Internet Explorer 5 o posterior proporciona el mejor modelo de objetos del lado cliente para analizar y filtrar datos XML debido a su compatibilidad integrada con MSXML.

Recuperar datos XML de un servicio Web no es diferente de recuperar otros tipos de datos. Empiece invocando el proxy de JavaScript para llamar a la función adecuada y definir una función de devolución de llamada. Una vez que se devuelve la llamada, puede procesar los datos en la función de devolución de llamada.

En el listado 15 se muestra un ejemplo de llamada a un método Web denominado GetRssFeed () que devuelve un objeto XmlElement. GetRssFeed () acepta un único parámetro que representa la dirección URL de la fuente RSS que se va a recuperar.

**Enumerar 15. Trabajar con datos XML devueltos de un servicio Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

En este ejemplo se pasa una dirección URL a una fuente RSS y se procesan los datos XML devueltos en la función OnWSRequestComplete (). OnWSRequestComplete () primero comprueba si el explorador es Internet Explorer para saber si el analizador MSXML está disponible o no. Si es así, se usa una instrucción XPath para buscar todas &lt; las &gt; etiquetas de elemento en la fuente RSS. Después, cada elemento se recorre en iteración y el &lt; título &gt; y las etiquetas de vínculo asociados &lt; &gt; se encuentran y procesan para mostrar los datos de cada elemento. En la figura 3 se muestra un ejemplo de la salida generada a partir de la realización de una llamada de AJAX de ASP.NET a través de un proxy de JavaScript al método Web GetRssFeed ().

## <a name="handling-complex-types"></a>Controlar tipos complejos

Los tipos complejos aceptados o devueltos por un servicio Web se exponen automáticamente a través de un proxy de JavaScript. Sin embargo, los tipos complejos anidados no son accesibles directamente en el lado cliente, a menos que el atributo GenerateScriptType se aplique al servicio como se explicó anteriormente. ¿Por qué sería conveniente usar un tipo complejo anidado en el lado cliente?

Para responder a esta pregunta, supongamos que una página de ASP.NET AJAX muestra datos de cliente y permite a los usuarios finales actualizar la dirección de un cliente. Si el servicio web especifica que el tipo de dirección (un tipo complejo definido dentro de una clase CustomerDetails) se puede enviar al cliente, el proceso de actualización se puede dividir en funciones independientes para mejorar el uso del código.

[![Salida que crea a partir de una llamada a un servicio Web que devuelve datos de RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: salida de la creación de una llamada a un servicio Web que devuelve datos de RSS.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-web-services/_static/image9.png))

La lista 16 muestra un ejemplo de código del lado cliente que invoca un objeto de dirección definido en un espacio de nombres del modelo, lo rellena con datos actualizados y lo asigna a la propiedad de dirección de un objeto CustomerDetails. A continuación, el objeto CustomerDetails se pasa al servicio web para su procesamiento.

**Listado 16. Usar tipos complejos anidados**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Crear y usar métodos de página

Los servicios web proporcionan una manera excelente de exponer servicios que se pueden volver a utilizar a una variedad de clientes, incluidas páginas de ASP.NET AJAX. Sin embargo, puede haber casos en los que una página necesite recuperar datos que nunca se utilizarán o compartirán en otras páginas. En este caso, la creación de un archivo. asmx para permitir que la página tenga acceso a los datos puede parecer un exceso de información, ya que el servicio solo se usa en una sola página.

ASP.NET AJAX proporciona otro mecanismo para realizar llamadas de tipo servicio Web sin crear archivos. asmx independientes. Esto se hace mediante una técnica denominada "métodos de página". Los métodos de página son métodos estáticos (compartidos en VB.NET) insertados directamente en una página o un archivo de código lateral que tienen aplicado el atributo WebMethod. Al aplicar el atributo WebMethod se puede llamar mediante un objeto especial de JavaScript denominado PageMethods que se crea dinámicamente en tiempo de ejecución. El objeto PageMethods actúa como un proxy que le protege del proceso de serialización/deserialización de JSON. Tenga en cuenta que para poder usar el objeto PageMethods, debe establecer la propiedad EnablePageMethods de ScriptManager en true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

En la lista 17 se muestra un ejemplo de cómo definir dos métodos de página en una clase de código lateral ASP.NET. Estos métodos recuperan datos de una clase de capa de negocio que se encuentra en la \_ carpeta de código de la aplicación del sitio Web.

**Enumeración 17. Definir métodos de página.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Cuando ScriptManager detecta la presencia de métodos Web en la página, genera una referencia dinámica al objeto PageMethods mencionado anteriormente. La llamada a un método Web se logra haciendo referencia a la clase PageMethods seguida del nombre del método y de los datos de parámetro necesarios que se deben pasar. En el Listado 18 se muestran ejemplos de llamada a los dos métodos de página mostrados anteriormente.

**Enumerar 18. Llamar a métodos de página con el objeto PageMethods de JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

El uso del objeto PageMethods es muy similar al uso de un objeto proxy de JavaScript. En primer lugar, debe especificar todos los datos de parámetros que deben pasarse al método de página y, a continuación, definir la función de devolución de llamada a la que se debe llamar cuando se devuelva la llamada asincrónica. También se puede especificar una devolución de llamada de error (consulte la lista 14 para ver un ejemplo de errores de control).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>El kit de herramientas de AutoCompleteExtender y ASP.NET AJAX

El kit de herramientas de ASP.NET AJAX (disponible en [http://ajax.asp.net](http://ajax.asp.net) ) ofrece varios controles que se pueden usar para tener acceso a servicios Web. En concreto, el kit de herramientas contiene un control útil denominado `AutoCompleteExtender` que se puede usar para llamar a servicios web y Mostrar datos en páginas sin escribir ningún código JavaScript.

El control AutoCompleteExtender se puede usar para ampliar la funcionalidad existente de un cuadro de texto y ayudar a los usuarios a encontrar más fácilmente los datos que están buscando. A medida que escriben en un cuadro de texto, el control se puede usar para consultar un servicio Web y muestra los resultados por debajo del cuadro de texto de forma dinámica. En la figura 4 se muestra un ejemplo de cómo usar el control AutoCompleteExtender para mostrar los identificadores de cliente de una aplicación de soporte técnico. A medida que el usuario escribe caracteres diferentes en el cuadro de texto, se mostrarán elementos diferentes en función de su entrada. Los usuarios pueden seleccionar el ID. de cliente deseado.

El uso de AutoCompleteExtender dentro de una página de ASP.NET AJAX requiere que el ensamblado de AjaxControlToolkit.dll se agregue a la carpeta bin del sitio Web. Una vez agregado el ensamblado del kit de herramientas, querrá hacer referencia a él en web.config para que los controles que contiene estén disponibles para todas las páginas de una aplicación. Para ello, agregue la siguiente etiqueta dentro de la &lt; etiqueta controles de web.config &gt; :

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

En los casos en los que solo necesita usar el control en una página específica, puede hacer referencia a él agregando la Directiva de referencia a la parte superior de una página como se muestra a continuación, en lugar de actualizar web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]

[![Usar el control AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: uso del control AutoCompleteExtender.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-web-services/_static/image12.png))

Una vez que el sitio web se ha configurado para usar el kit de herramientas de ASP.NET AJAX, se puede Agregar un control AutoCompleteExtender en la página de forma muy similar a como se haría con un control de servidor ASP.NET normal. En la lista 19 se muestra un ejemplo del uso del control para llamar a un servicio Web.

**Enumerar 19. Usar el control AutoCompleteExtender de ASP.NET AJAX Toolkit.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender tiene varias propiedades diferentes, entre las que se incluyen el identificador estándar y las propiedades runat que se encuentran en los controles de servidor. Además, permite definir el número de caracteres que un usuario final escribe antes de que se consulten los datos del servicio Web. La propiedad MinimumPrefixLength que se muestra en la lista 19 hace que se llame al servicio cada vez que se escribe un carácter en el cuadro de texto. Querrá tener cuidado al establecer este valor, ya que cada vez que el usuario escribe un carácter, se llamará al servicio web para buscar valores que coincidan con los caracteres del cuadro de texto. El servicio Web al que se llamará así como el método Web de destino se definen mediante las propiedades ServicePath y ServiceMethod, respectivamente. Por último, la propiedad TargetControlID identifica el cuadro de texto al que se va a enlazar el control AutoCompleteExtender.

El servicio Web al que se llama debe tener el atributo ScriptService aplicado tal y como se ha descrito anteriormente y el método Web de destino debe aceptar dos parámetros denominados prefixText y Count. El parámetro prefixText representa los caracteres que escribe el usuario final y el parámetro Count representa el número de elementos que se van a devolver (el valor predeterminado es 10). En la lista 20 se muestra un ejemplo del método Web GetCustomerIDs llamado por el control AutoCompleteExtender que se mostró anteriormente en la lista 19. El método Web llama a un método de nivel de negocio que a su vez llama a un método de nivel de datos que controla el filtrado de los datos y la devolución de los resultados de búsqueda de coincidencias. El código para el método de nivel de datos se muestra en la lista 21.

**Enumerar 20. Filtrado de datos enviados desde el control AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Listado 21. Filtrar resultados en función de los datos proporcionados por el usuario final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusión

ASP.NET AJAX proporciona una excelente compatibilidad para llamar a servicios web sin tener que escribir una gran cantidad de código JavaScript personalizado para administrar los mensajes de solicitud y respuesta. En este artículo se ha visto cómo habilitar AJAX para habilitar los servicios Web .NET para que puedan procesar mensajes JSON y cómo definir servidores proxy de JavaScript mediante el control ScriptManager. También ha visto cómo se pueden usar los proxies de JavaScript para llamar a los servicios Web, administrar tipos simples y complejos y tratar los errores. Por último, ha visto cómo se pueden usar los métodos de página para simplificar el proceso de creación y realización de llamadas a servicios web y cómo el control AutoCompleteExtender puede proporcionar ayuda a los usuarios finales a medida que escriben. Aunque el UpdatePanel disponible en ASP.NET AJAX será el control elegido para muchos programadores de AJAX debido a su simplicidad, saber cómo llamar a los servicios web a través de servidores proxy de JavaScript puede ser útil en muchas aplicaciones.

## <a name="bio"></a>Biografía

Dan Wahlin (profesional más valioso de Microsoft para ASP.NET y servicios Web XML) es un instructor de desarrollo de .NET y consultor de arquitectura en la interfaz de aprendizaje técnico ( [http://www.interfacett.com](http://www.interfacett.com) ). Dan fundado el sitio web de XML para desarrolladores de ASP.NET ([www.XMLforASP.net](http://www.XMLforASP.NET)), se encuentra en la oficina del orador de INETA y habla en varias conferencias. Dan coautoría profesional Windows DNA (Wrox), ASP.NET: Tips, tutoriales y código (SAMS), ASP.NET 1,1 Insider Solutions, Professional ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 MVP y XML creado para desarrolladores de ASP.NET (SAMS). Cuando no está escribiendo código, artículos o libros, Juan disfruta de escribir y grabar música y jugar con golf y baloncesto con su esposa y niños.

Scott Cate ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el Presidente de myKB.com ([www.myKB.com](http://www.myKB.com)), donde se especializa en escribir aplicaciones basadas en ASP.net centradas en las soluciones de software de la base de conocimiento. Puede ponerse en contacto con Scott por correo electrónico en [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o en su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-localization.md)
> [Siguiente](understanding-asp-net-ajax-debugging-capabilities.md)
