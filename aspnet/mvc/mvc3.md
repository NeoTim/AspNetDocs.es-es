---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (incluye actualización de herramientas de abril de 2011) ASP.NET MVC 3 es un marco para crear aplicaciones web escalables y basadas en estándares utilizando un patrón de diseño bien establecido...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 6fe734dc0d0106bb346df154e1e03f97b307567a
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675140"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(incluye actualización de herramientas de abril de 2011)*
> 
> ASP.NET MVC 3 es un marco para crear aplicaciones web escalables basadas en estándares mediante patrones de diseño bien establecidos y la potencia de ASP.NET y .NET Framework.
> 
> Se instala en paralelo con ASP.NET MVC 2, así que empezar a usarlo hoy!
> 
> Descargue el [instalador aquí](https://microsoft.com/download/details.aspx?id=4211)

## <a name="top-features"></a>Principales características

- Sistema de andamios integrado extensible a través de NuGet
- Plantillas de proyecto habilitadas para HTML 5
- Vistas expresivas, incluido el nuevo motor Razor View
- Potentes ganchos con inyección de dependencia y filtros de acción global
- Compatibilidad con JavaScript enriquecido con JavaScript discreto, validación de jQuery y enlace JSON
- *Lea la lista completa de funciones [a continuación](#overview)*

## <a name="top-links"></a>Enlaces principales

Novedades de ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 lanzado](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express y Orchard lanzados - La versión web](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx) de Microsoft enero en contexto
- Scott Guthrie: Anunciando la [versión de ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de la versión para ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalación y ayuda

- Instalar ASP.NET MVC 3 mediante el Instalador de [plataforma web (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instale ASP.NET MVC 3 mediante el ejecutable del [instalador](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instale [ASP.NET MVC 3 para Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lea el [tutorial Introducción a ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtenga ayuda y analice ASP.NET MVC 3 en los [foros](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Descripción general de ASP.NET MVC 3

ASP.NET MVC 3 se basa en ASP.NET MVC 1 y 2, agregando excelentes características que simplifican el código y permiten una extensibilidad más profunda. En este tema se proporciona información general sobre muchas de las nuevas características que se incluyen en esta versión, organizadas en las secciones siguientes:

- [Andamiaje extensible con integración MvcScaffold](#BM_MvcScaffolding)
- [Plantillas de proyecto habilitadas para HTML 5](#BM_HTML5)
- [El motor Razor View](#BM_TheRazorViewEngine)
- [Soporte para motores de vista múltiple](#BM_Support_for_Multiple_View_Engines)
- [Mejoras en el controlador](#BM_Controller_Improvements)
- [JavaScript y Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Mejoras en la validación del modelo](#BM_Model_Validation_Improvements)
- [Mejoras en la inyección de dependencia](#BM_Dependency_Injection_Improvements)
- [Otras nuevas características](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Andamiaje extensible con integración MvcScaffold

El nuevo sistema de andamios facilita la recogida y el inicio de uso productivo si es totalmente nuevo en el marco de trabajo y automatiza las tareas de desarrollo comunes si tiene experiencia y ya sabe lo que está haciendo.

Esto es compatible con el nuevo paquete *de scaffolding* NuGet denominado **MvcScaffolding**. El término "Scaffolding" es utilizado por muchas tecnologías de software para significar "generar rápidamente un esquema básico de su software que luego puede editar y personalizar". El paquete de andamios que estamos creando para ASP.NET MVC es muy beneficioso en varios escenarios:

- **Si estás aprendiendo ASP.NET MVC por primera vez,** porque te da una forma rápida de obtener código útil y de trabajo, que luego puedes editar y adaptar según tus necesidades. ¡Te salva del trauma de mirar una página en blanco y no tener idea de por dónde empezar!
- **Si conoce bien ASP.NET MVC y ahora está explorando alguna nueva tecnología** de complementos, como un asignador relacional de objetos, un motor de vista, una biblioteca de pruebas, etc., porque el creador de esa tecnología también puede haber creado un paquete de andamiado para él.
- **Si su trabajo implica crear repetidamente clases o archivos similares de algún tipo,** ya que puede crear scaffolders personalizados que emitan accesorios de prueba, scripts de implementación o cualquier otra cosa que necesite. Todos los miembros de tu equipo también pueden usar tus scaffolders personalizados.

Otras características de MvcScaffolding incluyen:

- Soporte para proyectos de C y VB
- Soporte para los motores de vista Razor y ASPX
- Admite andamios en áreas MVC ASP.NET y mediante diseños de vista personalizados/maestros
- Puede personalizar fácilmente la salida editando plantillas T4
- Puede agregar scaffolders completamente nuevos mediante la lógica de PowerShell personalizada y plantillas T4 personalizadas. Estos (y los parámetros personalizados que les ha dado) aparecen automáticamente en la lista de finalización de pestañas de la consola.
- Puede obtener paquetes NuGet que contengan scaffolders adicionales para diferentes tecnologías (por ejemplo, hay uno de prueba de concepto para LINQ to SQLLINQ to SQL ahora) y mezclarlos y combinarlos y combinarlos y combinarlos

La actualización de herramientas de ASP.NET MVC 3 incluye una excelente compatibilidad de Visual Studio para este sistema de scaffolding, como:

- Agregar cuadro de diálogo de controlador ahora admite scaffolding automáticos completos de crear, leer, actualizar y eliminar acciones de controlador y las vistas correspondientes. De forma predeterminada, esto aplica scaffolding al código de acceso a datos mediante EF Code First.
- Agregar cuadro de diálogo de controlador admite *scaffolding extensibles* a través de paquetes NuGet como *MvcScaffolding*. Esto permite conectar scaffolding personalizados en el cuadro de diálogo que le permitiría crear andamios para otras tecnologías de acceso a datos como NHibernate o incluso JET con ODBCDirect si usted está tan inclinado!

Para obtener más información acerca de Scaffolding en ASP.NET MVC 3, vea los siguientes recursos:

- Serie de steve Sanderson, incluyendo: 

    1. [Introducción: Scaffolding su ASP.NET MVC 3 proyecto con el paquete MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso estándar: Casos de uso y opciones típicos](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relaciones de uno a varios](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Acciones de andamios y pruebas unitarias](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Anulación de las plantillas T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Este post: Creación de scaffolders personalizados](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- La publicación de Scott Hanselman de su sesión de PDC 2010 [Building a Blog with Microsoft "Unnamed Package of Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Plantillas de proyecto HTML 5

El cuadro de diálogo Nuevo proyecto incluye una casilla de verificación Habilitar versiones HTML 5 de plantillas de proyecto. Estas plantillas aprovechan Modernizr 1.7 para proporcionar compatibilidad con HTML 5 y CSS 3 en navegadores de nivel inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>El motor Razor View

ASP.NET MVC 3 viene con un nuevo motor de vista llamado Razor que ofrece las siguientes ventajas:

- La sintaxis de Razor es limpia y concisa, lo que requiere un número mínimo de pulsaciones de teclas.
- Razor es fácil de aprender, en parte porque se basa en lenguajes existentes, como C- y Visual Basic.
- Visual Studio incluye IntelliSense y coloración de código para la sintaxis de Razor.
- Las vistas de Razor se pueden probar unitariamente sin necesidad de ejecutar la aplicación o iniciar un servidor web.

Algunas características nuevas de Razor incluyen lo siguiente:

- `@model`sintaxis para especificar el tipo que se pasa a la vista.
- `@* *@`sintaxis de comentarios.
- La capacidad de especificar valores `layoutpage`predeterminados (como ) una vez para todo un sitio.
- El `Html.Raw` método para mostrar texto sin codificarlo en HTML.
- Compatibilidad con el uso compartido de código entre varias vistas (archivos*\_viewstart.cshtml* o * \_viewstart.vbhtml).*

Razor también incluye nuevas aplicaciones auxiliares HTML, como las siguientes:

- `Chart`. Representa un gráfico, que ofrece las mismas características que el control de gráfico en ASP.NET 4.
- `WebGrid`. Representa una cuadrícula de datos, completa con la funcionalidad de paginación y ordenación.
- `Crypto`. Utiliza algoritmos hash para crear contraseñas con sal y hash correctamente saladas.
- `WebImage`. Representa una imagen.
- `WebMail`. Envía un mensaje de correo electrónico.

Para obtener más información acerca de Razor, consulte los siguientes recursos:

- [La entrada del blog de Scott Guthrie presenta A Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [La entrada de blog de @model Scott Guthrie presenta la palabra clave](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [La entrada del blog de Scott Guthrie presenta diseños de Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referencia rápida de la API de Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Soporte para motores de vista múltiple

El cuadro de diálogo **Agregar vista** de ASP.NET MVC 3 le permite elegir el motor de vista con el que desea trabajar y el cuadro de diálogo Nuevo **proyecto** le permite especificar el motor de vista predeterminado para un proyecto. Puede elegir el motor de vista de formularios Web Forms (ASPX), Razor o un motor de vista de código abierto como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Mejoras en el controlador

### <a name="global-action-filters"></a>Filtros de Acción Global

A veces desea realizar la lógica antes de que se ejecute un método de acción o después de que se ejecute un método de acción. Para admitir esto, ASP.NET MVC 2 proporcionaron filtros de acción. Los filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar un comportamiento previo y posterior a la acción a métodos de acción de controlador específicos. Sin embargo, en algunos casos es posible que desee especificar el comportamiento previo o posterior a la acción que se aplica a todos los métodos de acción. MVC 3 le permite especificar filtros `GlobalFilters` globales agregándolos a la colección. Para obtener más información acerca de los filtros de acción global, consulte los siguientes recursos:

- [Blog de Scott Guthrie en LA vista previa MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrar en ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nueva propiedad "ViewBag"

Los controladores `ViewData` MVC 2 admiten una propiedad que le permite pasar datos a una plantilla de vista mediante una API de diccionario enlazada en tiempo de completo. En MVC 3, también puede usar una `ViewBag` sintaxis algo más simple con la propiedad para lograr el mismo propósito. Por ejemplo, en `ViewData["Message"]="text"`lugar de `ViewBag.Message="text"`escribir , puede escribir . No es necesario definir ninguna clase fuertemente `ViewBag` tipada para usar la propiedad. Dado que es una propiedad dinámica, en su lugar, puede obtener o establecer propiedades y las resolverá dinámicamente en tiempo de ejecución. Internamente, `ViewBag` las propiedades se almacenan `ViewData` como pares nombre/valor en el diccionario. (Nota: en la mayoría de las `ViewBag` versiones preliminares de MVC 3, la propiedad se denomina la `ViewModel` propiedad.)

### <a name="new-actionresult-types"></a>Nuevos tipos "ActionResult"

Los `ActionResult` siguientes tipos y métodos auxiliares correspondientes son nuevos o mejorados en MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Devuelve un código de estado HTTP 404 al cliente.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Devuelve una redirección temporal (código de estado HTTP 302) o una redirección permanente (código de estado HTTP 301), dependiendo de un parámetro booleano. Junto con este cambio, la clase [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) ahora tiene tres `RedirectPermanent` `RedirectToRoutePermanent`métodos `RedirectToActionPermanent`para realizar redirecciones permanentes: , , y . Estos métodos devuelven `Permanent` una instancia `true`de `RedirectResult` with la propiedad establecida en .
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Devuelve un código de estado HTTP especificado por el usuario.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Mejoras en JavaScript y Ajax

De forma predeterminada, Ajax y las aplicaciones auxiliares de validación de MVC 3 usan un enfoque de JavaScript discreto. JavaScript discreto evita inyectar JavaScript en línea en HTML. Esto hace que su HTML sea más pequeño y menos desordenado, y facilita el intercambio o la personalización de bibliotecas JavaScript. Las aplicaciones auxiliares de `jQueryValidate` validación de MVC 3 también usan el complemento de forma predeterminada. Si desea el comportamiento de MVC 2, puede deshabilitar JavaScript discreto mediante una configuración de archivo *web.config.* Para obtener más información acerca de las mejoras de JavaScript y Ajax, consulte los siguientes recursos:

- [Introducción básica a JavaScript discreto en el sitio de Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Publicación de JavaScript discreta de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Publicación de validación de JavaScript discreta de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creación de una aplicación MVC 3 con Razor y JavaScript discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial en el sitio ASP.NET)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validación del lado cliente habilitada de forma predeterminada

En versiones anteriores de MVC, debe `Html.EnableClientValidation` llamar explícitamente al método desde una vista para habilitar la validación del lado cliente. En MVC 3 esto ya no es necesario porque la validación del lado cliente está habilitada de forma predeterminada. (Puede deshabilitar esto mediante una configuración en el archivo *web.config.)*

Para que la validación del lado cliente funcione, todavía debe hacer referencia a las bibliotecas de validación jQuery y jQuery adecuadas en su sitio. Puede alojar esas bibliotecas en su propio servidor o hacer referencia a ellas desde una red de entrega de contenido (CDN) como las CDN de Microsoft o Google.

### <a name="remote-validator"></a>Validador remoto

ASP.NET MVC 3 admite la nueva clase [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) que le permite aprovechar la compatibilidad del complemento de validación de jQuery. Esto permite que la biblioteca de validación del lado cliente llame automáticamente a un método personalizado que defina en el servidor para realizar la lógica de validación que solo se puede realizar en el lado del servidor.

En el ejemplo `Remote` siguiente, el atributo especifica que `UserNameAvailable` la `UsersController` validación de cliente `UserName` llamará a una acción nombrada en la clase para validar el campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

En el ejemplo siguiente se muestra el controlador correspondiente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obtener más información `Remote` acerca de cómo usar el atributo, vea [Cómo: implementar la validación remota en ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) en la biblioteca MSDN.

### <a name="json-binding-support"></a>Compatibilidad con enlaces JSON

ASP.NET MVC 3 incluye compatibilidad de enlace JSON integrada que permite que los métodos de acción reciban datos codificados en JSON y los enlacen modelos a parámetros de método de acción. Esta capacidad es útil en escenarios que implican plantillas de cliente y enlace de datos. (Las plantillas de cliente le permiten dar formato y mostrar un único elemento de datos o un conjunto de elementos de datos mediante plantillas que se ejecutan en el cliente.) MVC 3 le permite conectar fácilmente plantillas de cliente con métodos de acción en el servidor que envían y reciben datos JSON. Para obtener más información acerca de la compatibilidad con enlaces JSON, consulte la sección Mejoras de **JavaScript y AJAX** de la entrada de blog MVC 3 Preview de [Scott Guthrie.](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Mejoras en la validación del modelo

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadatos "DataAnnotations"

ASP.NET MVC `DataAnnotations` 3 admite `DisplayAttribute`atributos de metadatos como .

### <a name="validationattribute-class"></a>Clase "ValidationAttribute"

La `ValidationAttribute` clase se ha mejorado en .NET `IsValid` Framework 4 para admitir una nueva sobrecarga que proporciona más información sobre el contexto de validación actual, como qué objeto se está validando. Esto permite escenarios más enriquecidos donde puede validar el valor actual en función de otra propiedad del modelo. Por ejemplo, `CompareAttribute` el nuevo atributo permite comparar los valores de dos propiedades de un modelo. En el ejemplo `ComparePassword` siguiente, la `Password` propiedad debe coincidir con el campo para que sea válida.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validación

La interfaz [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) permite realizar la validación de nivel de modelo y le permite proporcionar mensajes de error de validación que son específicos del estado del modelo general o entre dos propiedades dentro del modelo. MVC 3 ahora recupera `IValidatableObject` errores de la interfaz al enlazar modelos y marca o resalta automáticamente los campos afectados dentro de una vista mediante las aplicaciones auxiliares de formulario HTML integradas.

La interfaz [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) permite a ASP.NET MVC detectar en tiempo de ejecución si un validador tiene compatibilidad con la validación de cliente. Esta interfaz ha sido diseñada para que pueda integrarse con una variedad de marcos de validación.

Para obtener más información acerca de las interfaces de validación, vea la sección Mejoras de **validación** de modelos de la entrada de blog MVC 3 Preview de [Scott Guthrie.](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx) (Sin embargo, tenga en cuenta que la referencia a "IValidateObject" en el blog debe ser "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Mejoras en la inyección de dependencia

ASP.NET MVC 3 proporciona un mejor soporte para aplicar la inyección de dependencia (DI) y para la integración con contenedores de inyección de dependencia o inversión de control (IOC). Se ha añadido soporte para DI en las siguientes áreas:

- Controladores (registro e inyección de fábricas de controladores, controladores de inyección).
- Vistas (registro e inyección de motores de vista, inyección de dependencias en páginas de vista).
- Filtros de acción (localización e inyección de filtros).
- Aglutinantes de modelos (registro e inyección).
- Proveedores de validación de modelos (registro e inyección).
- Proveedores de metadatos de modelo (registro e inyección).
- Proveedores de valor (registro e inyección).

MVC 3 admite la biblioteca [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) y `IServiceLocator` cualquier contenedor DI que admita la interfaz de esa biblioteca. También es compatible `IDependencyResolver` con una nueva interfaz que facilita la integración de marcos di.

Para obtener más información acerca de DI en MVC 3, vea los siguientes recursos:

- [La serie de entradas de blog de Brad Wilson sobre ubicación de servicio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Otras nuevas características

### <a name="nuget-integration"></a>Integración de NuGet

ASP.NET MVC 3 instala y habilita NuGet automáticamente como parte de su instalación. NuGet es un administrador de paquetes de código abierto gratuito que facilita la búsqueda, instalación y uso de bibliotecas y herramientas de .NET en sus proyectos. Funciona con todos los tipos de proyecto de Visual Studio (incluidos ASP.NET formularios Web Forms y ASP.NET MVC).

NuGet permite a los desarrolladores que mantienen proyectos de código abierto (por ejemplo, proyectos como Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks y Elmah) empaquetar sus bibliotecas y registrarlas en una galería en línea. A continuación, es fácil para los desarrolladores de .NET que desean usar una de estas bibliotecas para encontrar el paquete e instalarlo en los proyectos en los que están trabajando.

Con la actualización de herramientas de ASP.NET 3, las plantillas de proyecto incluyen bibliotecas DeJavaP preinstaladas en paquetes NuGet, por lo que se pueden actualizar a través de NuGet. Entity Framework Code First también está preinstalado como un paquete NuGet.

Para más información sobre NuGet, consulte la [documentación correspondiente](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Almacenamiento en caché de resultados de página parcial

ASP.NET MVC ha admitido el almacenamiento en caché de salida de respuestas de página completa desde la versión 1. MVC 3 también admite el almacenamiento en caché de resultados de página parcial, lo que le permite almacenar fácilmente en caché regiones o fragmentos de una respuesta. Para obtener más información acerca del almacenamiento en caché, vea la sección Almacenamiento en **caché** de resultados de página parcial de la entrada de blog de [Scott Guthrie en el candidato](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) de la versión MVC 3 y la sección Almacenamiento en caché de resultados de acción **secundaria** de las notas de la [versión MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Control granular sobre la validación de solicitudes

ASP.NET MVC tiene validación de solicitudes integrada que ayuda automáticamente a proteger contra ataques de inyección XSS y HTML. Sin embargo, a veces desea deshabilitar explícitamente la validación de solicitudes, como si desea permitir que los usuarios publiquen contenido HTML (por ejemplo, en entradas de blog o contenido CMS). Ahora puede agregar un atributo [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) a modelos o modelos de vista para deshabilitar la validación de solicitudes por propiedad durante el enlace de modelos. Para obtener más información acerca de la validación de solicitudes, consulte los siguientes recursos:

- La sección **JavaScript y validación discreta** en [la entrada de blog de Scott Guthrie en el candidato de la versión MVC 3.](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Cuadro de diálogo Extensible "Nuevo proyecto"

En ASP.NET MVC 3 puede agregar plantillas de proyecto, ver motores y marcos de proyecto de prueba unitaria al cuadro de diálogo **Nuevo proyecto.**

### <a name="template-scaffolding-improvements"></a>Mejoras en el andamiaje de plantillas

ASP.NET MVC 3 plantillas de scaffolding hacen un mejor trabajo de identificar propiedades de clave principal en los modelos y controlarlas adecuadamente que en versiones anteriores de MVC. (Por ejemplo, las plantillas de scaffolding ahora se aseguran de que la clave principal no esté scaffolding como un campo de formulario editable.)

De forma predeterminada, los scaffolding Create `Html.EditorFor` y Edit `Html.TextBoxFor` ahora usan la aplicación auxiliar en lugar de la aplicación auxiliar. Esto mejora la compatibilidad con metadatos en el modelo en forma de atributos de anotación de datos cuando el cuadro de diálogo **Agregar vista** genera una vista.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuevas sobrecargas para "Html.LabelFor" y "Html.LabelForModel"

Se han agregado nuevas `LabelFor` sobrecargas de método para los métodos auxiliares y. `LabelForModel` Las nuevas sobrecargas le permiten especificar o invalidar el texto de la etiqueta.

### <a name="sessionless-controller-support"></a>Soporte para controladores sin sesión

En ASP.NET MVC 3 puede indicar si desea que una clase de controlador use el estado de sesión y, si es así, si el estado de sesión debe ser de lectura/escritura o de solo lectura. Para obtener más información acerca de la compatibilidad con controladores sin sesión, vea [MVC 3 Notas de la versión](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nueva clase "AdditionalMetadataAttribute"

Puede usar el atributo [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) para rellenar el `ModelMetadata.AdditionalValues` diccionario de una propiedad de modelo. Por ejemplo, si un modelo de vista tiene una propiedad que solo se debe mostrar a un administrador, puede anotar esa propiedad como se muestra en el ejemplo siguiente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Estos metadatos están disponibles para cualquier plantilla de presentación o editor cuando se representa un modelo de vista de producto. Depende de usted interpretar la información de metadatos.

### <a name="accountcontroller-improvements"></a>Mejoras de AccountController

La plantilla de proyecto AccountController en Internet se ha mejorado considerablemente.

### <a name="new-intranet-project-template"></a>Nueva plantilla de proyecto de intranet

Se incluye una nueva plantilla de proyecto de intranet que habilita la autenticación de Windows y quita el AccountController.
