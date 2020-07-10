---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Novedades de ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: En este documento se describen las nuevas características y mejoras introducidas en ASP.NET MVC 2. Este documento también está disponible para su descarga.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 1a0a29241d8afecd295b11013b27621b21c9ed52
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "86162724"
---
# <a name="whats-new-in-aspnet-mvc-2"></a>Novedades de ASP.NET MVC 2

> En este documento se describen las nuevas características y mejoras introducidas en ASP.NET MVC 2. Este documento también está disponible para su [descarga](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)

[Aparición](#_TOC1)   
[Actualización de un proyecto de ASP.NET MVC 1,0 a ASP.NET MVC 2](#_TOC2)   
[Nuevas características](#_TOC3)   
[Aplicaciones auxiliares con plantilla](#_TOC3_1)   
[Lugares](#_TOC3_2)   
[Compatibilidad con controladores asincrónicos](#_TOC3_3)   
[Compatibilidad con DefaultValueAttribute en parámetros de método de acción](#_TOC3_4)   
[Compatibilidad para enlazar datos binarios con enlazadores de modelos](#_TOC3_5)   
[Clases ModelMetadata y ModelMetadataProvider](#_TOC3_6)   
[Compatibilidad con atributos de DataAnnotations](#_TOC3_7)   
[Proveedores de validadores de modelo](#_TOC3_8)   
[Validación del lado cliente](#_TOC3_9)   
[Nuevos fragmentos de código para Visual Studio 2010](#_TOC3_10)   
[Nuevo filtro de acción de RequireHttpsAttribute](#_TOC3_11)   
[Reemplazar el verbo del método HTTP](#_TOC3_12)   
[Nueva clase HiddenInputAttribute para aplicaciones auxiliares con plantilla](#_TOC3_13)   
[El método auxiliar HTML. ValidationSummary puede mostrar los errores de nivel de modelo](#_TOC3_14)   
[Las plantillas T4 de Visual Studio generan código que es específico de la versión de destino de las mejoras de la API de .NET Framework](#_TOC3_15)[API Improvements](#_TOC4)  
[Últimos cambios](#_TOC5)  
[Ausencia](#_TOC6)  

## <a name="introduction"></a><a id="_TOC1"></a>Aparición

ASP.NET MVC 2 se basa en ASP.NET MVC 1,0 y presenta un gran conjunto de mejoras y características que se centran en aumentar la productividad. Esta versión es compatible con ASP.NET MVC 1,0, de modo que todo el conocimiento, los conocimientos, el código y las extensiones para ASP.NET MVC 1,0 continúan aplicándose.

Para obtener más información sobre ASP.NET MVC, visite los siguientes recursos:

- [Documentación de ASP.NET MVC en MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [El sitio web de ASP.NET MVC](https://asp.net/mvc/)
- [Foros de ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a name="upgrading-an-aspnet-mvc-10-project-to-aspnet-mvc-2"></a><a id="_TOC2"></a>Actualización de un proyecto de ASP.NET MVC 1,0 a ASP.NET MVC 2

ASP.NET MVC 2 se puede instalar en paralelo con ASP.NET MVC 1,0 en el mismo servidor, lo que proporciona a los desarrolladores de aplicaciones flexibilidad a la hora de elegir cuándo actualizar una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2. Para obtener información sobre cómo actualizar, vea el documento [upgrading a ASP.net mvc 1,0 Application to ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a name="new-features"></a><a id="_TOC3"></a>Nuevas características

En esta sección se describen las características que se han introducido en la versión MVC 2.

### <a name="templated-helpers"></a><a id="_TOC3_1"></a>Aplicaciones auxiliares con plantilla

Las aplicaciones auxiliares con plantilla permiten asociar automáticamente elementos HTML para editarlos y mostrarlos con tipos de datos. Por ejemplo, cuando los datos del tipo System. DateTime se muestran en una vista, se puede representar automáticamente un elemento de la interfaz de usuario del selector de fecha. Esto es similar a cómo funcionan las plantillas de campo en ASP.NET datos dinámicos. Para obtener más información, vea [usar aplicaciones auxiliares con plantilla para Mostrar datos](https://go.microsoft.com/fwlink/?LinkId=159062) en el sitio web de MSDN.

### <a name="areas"></a><a id="_TOC3_2"></a>Lugares

Las áreas permiten organizar un proyecto grande en varias secciones más pequeñas con el fin de administrar la complejidad de una aplicación Web de gran tamaño. Cada sección ("área") normalmente representa una sección independiente de un sitio web grande y se utiliza para agrupar conjuntos de controladores y vistas relacionados. Para obtener más información, vea [Tutorial: organizar una aplicación de ASP.NET MVC por áreas](https://go.microsoft.com/fwlink/?LinkId=158978) en el sitio web de MSDN.

Para crear una nueva área, en Explorador de soluciones, haga clic con el botón secundario en el proyecto, haga clic en agregar y, a continuación, haga clic en área. Se mostrará un cuadro de diálogo que le pedirá el nombre del área. Después de escribir el nombre del área, Visual Studio agrega una nueva área al proyecto.

En la ilustración siguiente se muestra un diseño de ejemplo para un proyecto con dos áreas, administración y blogs.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Al crear un área, Visual Studio agrega una clase que se deriva de AreaRegistration a cada área. Esta clase es necesaria para registrar el área y sus rutas, tal y como se muestra en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

La plantilla de proyecto predeterminada para ASP.NET MVC 2 incluye una llamada al método RegisterAllAreas en el código del archivo global. asax. Este método registra cada área del proyecto buscando todos los tipos que derivan de la clase AreaRegistration, creando instancias de una instancia del tipo y, a continuación, llamando al método RegisterArea en la instancia. En el ejemplo siguiente se muestra cómo hacerlo.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Si no especifica el espacio de nombres en el método RegisterArea llamando al contexto. Namespaces. Add, el espacio de nombres de la clase de registro se utiliza de forma predeterminada.

### <a name="support-for-asynchronous-controllers"></a><a id="_TOC3_3"></a>Compatibilidad con controladores asincrónicos

ASP.NET MVC 2 permite ahora a los controladores procesar solicitudes de forma asincrónica. Esto puede mejorar el rendimiento al permitir que los servidores que llaman con frecuencia a operaciones de bloqueo (como las solicitudes de red) llamen a los homólogos sin bloqueo en su lugar. Para obtener más información, vea el tema [uso de un controlador asincrónico en ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) en MSDN.

### <a name="support-for-defaultvalueattribute-in-action-method-parameters"></a><a id="_TOC3_4"></a>Compatibilidad con DefaultValueAttribute en parámetros de método de acción

La clase System. ComponentModel. DefaultValueAttribute permite proporcionar un valor predeterminado para el parámetro argument a un método de acción. Por ejemplo, supongamos que se ha definido la siguiente ruta predeterminada:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Supongamos también que se ha definido el siguiente método de controlador y acción:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Cualquiera de las siguientes direcciones URL de solicitud invocará el método de acción de vista que se define en el ejemplo anterior.

- /Article/View/123
- /Article/View/123? Page = 1 (de hecho, lo mismo que la solicitud anterior)
- /Article/View/123? página = 2

Sin el atributo DefaultValueAttribute, la primera dirección URL de la lista anterior no funcionaría, porque el argumento de página es un tipo de valor que no acepta valores NULL cuyo valor no se ha proporcionado.

Si el código se escribe en Visual Basic 2010 o Visual C# 2010, puede usar parámetros opcionales en lugar del atributo DefaultValueAttribute, tal como se muestra en el ejemplo siguiente:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a name="support-for-binding-binary-data-with-model-binders"></a><a id="_TOC3_5"></a>Compatibilidad para enlazar datos binarios con enlazadores de modelos

Hay dos nuevas sobrecargas de la aplicación auxiliar HTML. Hidden que codifican los valores binarios como cadenas codificadas en base 64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Un uso típico consiste en insertar una marca de tiempo para un objeto en la vista. Por ejemplo, la aplicación podría incluir el siguiente objeto de producto:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Un formulario de edición puede representar la propiedad TimeStamp en el formulario tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Este marcado representa un elemento de entrada oculto con el valor de marca de tiempo como una cadena codificada en base 64 que es similar al ejemplo siguiente:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Este formulario se puede publicar en un método de acción que tiene un argumento de tipo product, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

En el método de acción, la propiedad TimeStamp se rellena correctamente porque la cadena expuesta codificada en base-64 se convierte en una matriz de bytes.

### <a name="modelmetadata-and-modelmetadataprovider-classes"></a><a id="_TOC3_6"></a>Clases ModelMetadata y ModelMetadataProvider

La clase ModelMetadataProvider proporciona una abstracción para obtener los metadatos para el modelo dentro de una vista. MVC 2 incluye un proveedor predeterminado que pone a disposición los metadatos expuestos por los atributos del espacio de nombres System. ComponentModel. DataAnnotations. Es posible crear proveedores de metadatos que proporcionen metadatos de otros almacenes de datos, como bases de datos o archivos XML.

La clase ViewDataDictionary expone un objeto ModelMetadata que contiene los metadatos extraídos del modelo por la clase ModelMetadataProvider. Esto permite a las aplicaciones auxiliares con plantilla utilizar estos metadatos y ajustar su salida en consecuencia.

Para obtener más información, consulte la documentación de las clases [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) y [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) .

### <a name="support-for-dataannotations-attributes"></a><a id="_TOC3_7"></a>Compatibilidad con atributos de DataAnnotations

ASP.NET MVC 2 admite el uso de los atributos de validación RangeAttribute, RequiredAttribute, StringLengthAttribute y RegexAttribute (definidos en el espacio de nombres System. ComponentModel. DataAnnotations) al enlazar a un modelo con el fin de proporcionar validación de entrada.

Para obtener más información, vea [Cómo: validar datos del modelo mediante los atributos de DataAnnotations](https://go.microsoft.com/fwlink/?LinkId=159063) en el sitio web de MSDN. Un proyecto de ejemplo que muestra el uso de estos atributos está disponible para su descarga en [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753) .

### <a name="model-validator-providers"></a><a id="_TOC3_8"></a>Proveedores de validadores de modelo

La clase del proveedor de validación de modelos representa una abstracción que proporciona la lógica de validación para el modelo. ASP.NET MVC incluye un proveedor predeterminado basado en atributos de validación que se incluyen en el espacio de nombres System. ComponentModel. DataAnnotations. También puede crear sus propios proveedores de validación que definen reglas de validación personalizadas y asignaciones personalizadas de reglas de validación para el modelo. Para obtener más información, vea la documentación de la clase [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) .

### <a name="client-side-validation"></a><a id="_TOC3_9"></a>Validación del lado cliente

La clase de proveedor de validadores de modelo expone los metadatos de validación en el explorador en forma de datos serializados con JSON que puede consumir una biblioteca de validación del lado cliente. ASP.NET MVC 2 incluye una biblioteca de validación de cliente y un adaptador que admite los atributos de validación de los espacios de nombres de DataAnnotations indicados anteriormente. La clase de proveedor también le permite usar otras bibliotecas de validación de cliente escribiendo un adaptador que procese los datos JSON y llame a la biblioteca alternativa.

### <a name="new-code-snippets-for-visual-studio-2010"></a><a id="_TOC3_10"></a>Nuevos fragmentos de código para Visual Studio 2010

Un conjunto de fragmentos de código HTML para ASP.NET MVC 2 se instala con Visual Studio 2010. Para ver una lista de estos fragmentos de código, en el menú herramientas, seleccione Administrador de fragmentos de código. En idioma, seleccione HTML y, en ubicación, seleccione ASP.NET MVC 2. Para obtener más información sobre cómo usar fragmentos de código, vea la documentación de Visual Studio.

### <a name="new-requirehttpsattribute-action-filter"></a><a id="_TOC3_11"></a>Nuevo filtro de acción de RequireHttpsAttribute

ASP.NET MVC 2 incluye una nueva clase RequireHttpsAttribute que se puede aplicar a los controladores y métodos de acción. De forma predeterminada, el filtro redirige una solicitud que no es SSL (HTTP) al equivalente habilitado para SSL (HTTPS).

### <a name="overriding-the-http-method-verb"></a><a id="_TOC3_12"></a>Reemplazar el verbo del método HTTP

Al compilar un sitio web mediante el estilo de arquitectura REST, los verbos HTTP se usan para determinar la acción que se realizará para un recurso. REST requiere que las aplicaciones admitan toda la gama de verbos HTTP comunes, como GET, PUT, POST y DELETE.

ASP.NET MVC 2 incluye nuevos atributos que se pueden aplicar a los métodos de acción y a la sintaxis compacta de características. Estos atributos permiten a ASP.NET MVC seleccionar un método de acción basado en el verbo HTTP. En el ejemplo siguiente, una solicitud POST llamará al primer método de acción y una solicitud PUT llamará al segundo método de acción.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

En versiones anteriores de ASP.NET MVC, estos métodos de acción requerían una sintaxis más detallada, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Dado que los exploradores solo admiten los verbos HTTP GET y POST, no es posible publicar en una acción que requiera un verbo diferente. Por lo tanto, no es posible admitir de forma nativa todas las solicitudes RESTful.

Sin embargo, para admitir las solicitudes RESTful durante las operaciones POST, ASP.NET MVC 2 presenta un nuevo método auxiliar HTML HttpMethodOverride. Este método representa un elemento de entrada oculto que hace que el formulario eMule de forma eficaz cualquier método HTTP. Por ejemplo, mediante el método auxiliar HTML HttpMethodOverride, puede hacer que un envío de formulario aparezca como una solicitud PUT o DELETE. El comportamiento de HttpMethodOverride afecta a los siguientes atributos:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

El elemento INPUT oculto tiene su nombre X-HTTP-Method-override y su valor establecido en el verbo HTTP que se va a emular. El valor de invalidación también se puede especificar en un encabezado HTTP o en un valor de cadena de consulta como un par de nombre y valor.

La invalidación solo se puede usar cuando la solicitud real es una solicitud POST. El valor de invalidación se omitirá para las solicitudes que usen cualquier otro verbo HTTP.

### <a name="new-hiddeninputattribute-class-for-templated-helpers"></a><a id="_TOC3_13"></a>Nueva clase HiddenInputAttribute para aplicaciones auxiliares con plantilla

Puede aplicar el nuevo atributo HiddenInputAttribute a una propiedad de modelo para indicar si se debe representar un elemento de entrada oculto al mostrar el modelo en una plantilla de editor. (El atributo establece un valor de UIHint implícito de HiddenInput). La propiedad DisplayValue del atributo le permite especificar si el valor se muestra en el editor y en los modos de presentación. Cuando DisplayValue se establece en false, no se muestra nada, ni siquiera el marcado HTML que normalmente rodea a un campo. El valor predeterminado de DisplayValue es true.

Puede usar el atributo HiddenInputAttribute en los siguientes escenarios:

- Cuando una vista permite a los usuarios editar el identificador de un objeto y es necesario mostrar el valor, así como proporcionar un elemento de entrada oculto que contiene el identificador anterior para que se pueda devolver al controlador.
- Cuando una vista permite a los usuarios editar una propiedad binaria que nunca se debe mostrar, como una propiedad timestamp. En ese caso, el valor y el marcado HTML circundante (como la etiqueta y el valor) no se muestran.

En el ejemplo siguiente se muestra cómo usar la clase HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Cuando el atributo se establece en true (o no se especifica ningún parámetro), ocurre lo siguiente:

- En Mostrar plantillas, se representa una etiqueta y el valor se muestra al usuario.
- En las plantillas de editor, se representa una etiqueta y el valor se representa en un elemento de entrada oculto.

Cuando el atributo se establece en false, ocurre lo siguiente:

- En las plantillas para mostrar, no se representa nada para ese campo.
- En las plantillas de editor, no se representa ninguna etiqueta y el valor se representa en un elemento de entrada oculto.

### <a name="htmlvalidationsummary-helper-method-can-display-model-level-errors"></a><a id="_TOC3_14"></a>El método auxiliar HTML. ValidationSummary puede mostrar los errores de nivel de modelo

En lugar de mostrar siempre todos los errores de validación, el método auxiliar HTML. ValidationSummary tiene una nueva opción para mostrar solo los errores de nivel de modelo. Esto permite que los errores de nivel de modelo se muestren en el Resumen de la validación y los errores específicos del campo que se van a mostrar junto a cada campo.

### <a name="t4-templates-in-visual-studio-generate-code-that-is-specific-to-the-target-version-of-the-net-framework"></a><a id="_TOC3_15"></a>Las plantillas T4 de Visual Studio generan código que es específico de la versión de destino de la .NET Framework

Hay disponible una nueva propiedad para los archivos T4 desde el host T4 de ASP.NET MVC que especifica la versión de la .NET Framework que usa la aplicación. Esto permite a las plantillas T4 generar código y marcado que es específico de una versión de la .NET Framework. En Visual Studio 2008, el valor siempre es .NET 3,5. En Visual Studio 2010, el valor es .NET 3,5 o .NET 4.

## <a name="api-improvements"></a><a id="_TOC4"></a>Mejoras de API

En esta sección se describen los cambios en los miembros y tipos de ASP.NET MVC existentes.

- Se ha agregado un método CreateActionInvoker virtual protegido en la clase de controlador. La propiedad ActionInvoker del controlador invoca este método y permite la creación diferida de instancias del Invocador si ya no se ha establecido ningún invocador.
- Se ha agregado un método HandleUnauthorizedRequest virtual protegido en la clase AuthorizeAttribute. Esto permite que los filtros que se derivan de AuthorizeAttribute controlen el comportamiento cuando se produce un error de autorización.
- Se ha agregado un método Add (clave de cadena, valor de objeto) en la clase ValueProviderDictionary. Esto le permite usar la sintaxis del inicializador de diccionario para ValueProviderDictionary, como en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Se ha agregado un \_ método get Object en la clase sys. Mvc. AjaxContext. Se trata de un método de JavaScript similar al método get \_ Data, pero si el tipo de contenido de la respuesta es Application/JSON, Get \_ Object devuelve el objeto JSON.
- Se ha agregado una propiedad descriptor en la clase AuthorizationContext.
- Se ha agregado un token UrlParameter. Optional que se puede usar para solucionar problemas al enlazar con un modelo que contiene una propiedad ID cuando la propiedad está ausente en un envío de formulario. Para obtener más información, consulte los [parámetros de dirección URL opcionales de ASP.NET MVC 2](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) en el blog de Phil Haack.

## <a name="breaking-changes"></a><a id="_TOC5"></a>Cambios importantes

Los cambios siguientes pueden provocar errores en las aplicaciones ASP.NET MVC 1,0 existentes.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Cambio en el comportamiento de validación de propiedades para las clases que implementan IDataErrorInfo

En el caso de los objetos de modelo que usan IDataErrorInfo para realizar la validación, se validan todas las propiedades, con independencia de si se ha establecido un nuevo valor. En ASP.NET MVC 1,0, solo se validaron las propiedades con nuevos valores establecidos. En ASP.NET MVC 2, solo se llama a la propiedad error de IDataErrorInfo si todos los validadores de propiedad se han realizado correctamente.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>El script de asignación de scripts de IIS ya no está disponible en el instalador

El script de asignación de scripts de IIS es un script de línea de comandos que se usa para configurar asignaciones de scripts para IIS 6 y para IIS 7 en modo clásico. El script de asignación de script no es necesario si usa el Servidor de desarrollo de Visual Studio o si usa IIS 7 en modo integrado. Los scripts están disponibles como una descarga independiente no compatible en [ASP.net webstack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>El método auxiliar HTML. Substitute en MVC Futures ya no está disponible

Debido a los cambios en el comportamiento de representación de los motores de vista de MVC, el método auxiliar HTML. Substitute no funciona y se ha quitado.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>La interfaz IValueProvider reemplaza todos los usos de IDictionary

Cada argumento de propiedad o método que aceptó IDictionary en MVC 1,0 ahora acepta IValueProvider. Este cambio solo afecta a las aplicaciones que incluyen proveedores de valores personalizados o enlazadores de modelos personalizados. Entre los ejemplos de propiedades y métodos que se ven afectados por este cambio se incluyen los siguientes:

- La propiedad ValueProvider de las clases ControllerBase y ModelBindingContext.
- Los métodos TryUpdateModel de la clase Controller.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Se han agregado nuevas clases CSS en el archivo site. CSS

El archivo site. CSS de las plantillas de proyecto de ASP.NET MVC se ha actualizado para incluir los nuevos estilos utilizados por la funcionalidad de validación y por las aplicaciones auxiliares con plantilla.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Las aplicaciones auxiliares ahora devuelven un objeto MvcHtmlString

Con el fin de aprovechar la nueva sintaxis de expresión de codificación HTML en ASP.NET 4, el tipo de valor devuelto para las aplicaciones auxiliares de HTML ahora es MvcHtmlString en lugar de una cadena. Si usa ASP.NET MVC 2 y las nuevas aplicaciones auxiliares en ASP.NET 3,5, no podrá beneficiarse de la sintaxis de codificación HTML; la nueva sintaxis solo está disponible cuando se ejecuta ASP.NET MVC 2 en ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult ahora responde solo a las solicitudes HTTP POST

Para mitigar los ataques de secuestro de JSON que tienen la posibilidad de la divulgación de información, de forma predeterminada, la clase JsonResult ahora responde solo a las solicitudes HTTP POST. Las llamadas GET de Ajax a métodos de acción que devuelven un objeto JsonResult se deben cambiar para usar POST en su lugar. Si es necesario, puede invalidar este comportamiento estableciendo la nueva propiedad JsonRequestBehavior de JsonResult. Para obtener más información sobre la posible vulnerabilidad, vea la entrada de blog sobre el [secuestro de JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) en el blog de Phil Haack.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Los establecedores de la propiedad Model y ModelType en ModelBindingContext están obsoletos

Se ha agregado una nueva propiedad ModelMetadata configurable a la clase ModelBindingContext. La nueva propiedad encapsula las propiedades Model y ModelType. Aunque las propiedades Model y ModelType están obsoletas, por compatibilidad con versiones anteriores, los captadores de propiedad siguen funcionando; delegan a la propiedad ModelMetadata para recuperar el valor.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Los cambios en la clase DefaultControllerFactory interrumpen los generadores de controladores personalizados que derivan de él

La clase DefaultControllerFactory se corrigió quitando la propiedad RequestContext. En lugar de esta propiedad, la instancia de contexto de solicitud se pasa a los métodos GetControllerInstance y GetControllerType protegidos. Este cambio afecta a los generadores de controladores personalizados que se derivan de DefaultControllerFactory.

Los generadores de controladores personalizados se suelen usar para proporcionar la inserción de dependencias para aplicaciones ASP.NET MVC. Para actualizar los generadores de controladores personalizados para admitir ASP.NET MVC 2, cambie la firma o las firmas del método para que coincidan con las nuevas firmas y use el parámetro de contexto de la solicitud en lugar de la propiedad.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Area" es ahora una clave de ruta-valor reservada

La cadena "Area" en los valores de ruta ahora tiene un significado especial en ASP.NET MVC, de la misma manera que "Controller" y "Action". Una implicación es que si se proporcionan las aplicaciones auxiliares HTML con un diccionario de valores de ruta que contiene "Area", las aplicaciones auxiliares dejarán de anexar "Area" en la cadena de consulta.

Si usa la característica de áreas, asegúrese de no usar {Area} como parte de la dirección URL de la ruta.

## <a name="disclaimer"></a><a id="_TOC6"></a>Ausencia

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información contenida en este documento representa la visión actual de Microsoft Corporation sobre los asuntos tratados a fecha de su publicación. Dado que Microsoft debe responder a condiciones de mercado variables, no debe interpretarse como un compromiso por parte de Microsoft, y Microsoft no puede garantizar la precisión de la información presentada tras la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de todas las leyes de derechos de autor aplicables. Sin limitar los derechos otorgados por las leyes de derechos de autor, ninguna parte de este documento puede ser reproducida o introducida en un sistema de recuperación, ni transmitida de ninguna forma ni por ningún medio, ya sea electrónico, mecánico, fotocopias, grabación u otros, con ningún propósito, sin la previa autorización por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor y otros derechos de propiedad intelectual sobre los contenidos de este documento. El suministro de este documento no le otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que ello se prevea en un contrato por escrito de licencia de Microsoft.

A menos que se indique lo contrario, las compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y eventos que se describen aquí son ficticios y no se pretende ni se debe inferir ninguna asociación con compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares o eventos reales.

© 2010 Microsoft Corporation. Todos los derechos reservados.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
