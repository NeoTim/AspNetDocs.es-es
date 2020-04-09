---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: Notas de la versión de ASP.NET y Web Tools 2012.2 Microsoft Docs
author: rick-anderson
description: Notas de la versión de ASP.NET y Web Tools 2012.2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: abd6d8ce0646852a194369589cb730fc98ecb3ad
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675908"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>Notas de la versión de ASP.NET and Web Tools 2012.2

> Este documento describe la versión de ASP.NET y de las herramientas Web 2012.2. Es una actualización de Visual Studio Web Tooling y ASP.NET.

- [Notas de instalación](#_Installation)
- [Documentación](#_Documentation)
- [Soporte técnico](#_Support)
- [Requisitos de software](#_Software_Requirements)
- [Nuevas características en ASP.NET y Web Tools 2012.2](#_New_Features_in)

    - [Tooling](#_Tooling)
    - [publicación Web](#_Web_Publishing)
    - [ASP.NET MVC Templates](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly URLs](#_ASP.NET_Friendly_URLs)
- [Problemas conocidos y cambios importantes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notas de la instalación

ASP.NET y Web Tools 2012.2 para Visual Studio 2012 se pueden instalar mediante el instalador de [la plataforma web.](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2) Se trata de una actualización de Visual Studio 2012 o Visual Studio Express 2012 para Web, que es necesaria. Si no tiene Visual Studio instalado, se instalará Visual Studio Express 2012 para Web.

También puede instalar ASP.NET y Web Tools 2012.2 manualmente. Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web instalado. A continuación, siga estas instrucciones: 

1. Descargue [ASP.NET instalador y Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) desde el Centro de descarga.
2. Cuando se le solicite, haga clic en Ejecutar. También puede guardar el archivo para ejecutarlo más adelante.
3. Compruebe la versión de Visual Studio que va a actualizar. Puede hacerlo iniciando Visual Studio que desea actualizar. A continuación, haga clic en el elemento de menú Ayuda.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Si ve el &quot;elemento de menú Acerca de&quot; Microsoft Visual Studio 2012 para Web, descargue [Web Developer Tools 2012.2 - Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkID=282228). De lo contrario, descargue [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Cuando se le solicite, haga clic en Ejecutar. También puede guardar el archivo para ejecutarlo más adelante.

> [!NOTE]
> ASP.NET y Web Tools 2012.2 no incluye SQL Server Data Tools. SQL ServerSQL Server y Windows Azure SQL Databases proporciona un conjunto más completo de herramientas de base de datos, incluido el desarrollo respaldado por proyectos sin conexión, la comparación de esquemas y las capacidades mejoradas de implementación de bases de datos. Para obtener más información o para [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)instalar SQL Server Data Tools, visite .

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentación

Los tutoriales y otra información sobre ASP.NET y Web Tools 2012.2 están disponibles en ASP.NET sitio web ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Soporte técnico

ASP.NET y Web Tools 2012.2 se lanza y es compatible oficialmente. Puede utilizar su canal de soporte normal. También puede publicar preguntas en los[https://forums.asp.net/](https://forums.asp.net/)foros de ASP.NET ( ), donde los miembros de la comunidad ASP.NET con frecuencia pueden proporcionar apoyo informal.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

El ASP.NET y Web Tools 2012.2 requiere Visual Studio 2012 o Visual Studio Express 2012 para Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuevas características en ASP.NET y Web Tools 2012.2

En esta sección se describen las características que se han introducido en la versión ASP.NET y Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Tooling

- Inspector de página 

    - Admite la asignación de selección de JavaScript que permite al Inspector de página asignar elementos que se agregaron dinámicamente a la página al código JavaScript correspondiente.
    - La capacidad de ver las actualizaciones CSS en tiempo real.
    - Para obtener más información, lea [CSS Auto-Sync y JavaScript Selection Mapping en El Inspector de páginas](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Admite resaltado de sintaxis para CoffeeScript, Mustache, Handlebars y JsRender.
    - El editor HTML proporciona Intellisense para enlaces Knockout.
    - Menos edición y compatibilidad con el compilador para habilitar la creación de CSS dinámico mediante LESS.
    - Pegue JSON como una clase .NET. Con este comando Special Paste para pegar JSON en un archivo de código de C o VB.NET, y Visual Studio generará automáticamente las clases .NET deduzcadas desde el JSON.
- La compatibilidad con emuladores móviles agrega enlaces de extensibilidad para que los emuladores de terceros se puedan instalar como VSIX. Los emuladores instalados se mostrarán en la lista desplegable F5, para que los desarrolladores puedan obtener una vista previa de sus sitios web en una variedad de dispositivos móviles. Obtenga más información sobre esta característica en la entrada de blog de Scott Hanselman sobre la nueva integración de [BrowserStack con Visual Studio.](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx)

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>publicación Web

- Los proyectos de sitio web ahora tienen la misma experiencia de publicación que los proyectos de aplicaciones web, incluida la publicación en sitios web de Windows Azure.
- &#8211; de publicación selectiva para uno o varios archivos puede realizar las siguientes acciones (después de publicar en un punto de conexión de Web Deploy): 

    - Publicar archivos seleccionados.
    - Consulte la diferencia entre un archivo local y un archivo remoto.
    - Actualice el archivo local con el archivo remoto o actualice el archivo remoto con el archivo local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC Templates

- La nueva plantilla de aplicación de Facebook facilita la escritura de aplicaciones Canvas de Facebook. En unos pocos pasos sencillos, puede crear una aplicación de Facebook que obtiene datos de un usuario conectado y se integra con los amigos de este. La plantilla incluye una nueva biblioteca para encargarse de todos los elementos necesarios a la hora de crear una aplicación de Facebook, incluida la autenticación, los permisos, el acceso a los datos de Facebook y mucho más. Para obtener más información sobre [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)el uso de la plantilla de aplicación de Facebook, consulte .
- Una nueva plantilla de MVC de aplicación de página única permite a los desarrolladores crear aplicaciones web de cliente interactivas mediante HTML 5, CSS 3 y las populares bibliotecas Knockout y jQuery JavaScript, encima de ASP.NET Web API. La plantilla incluye una aplicación de lista "todo" que muestra prácticas comunes para crear una aplicación HTML5 de JavaScript que utiliza una API de servidor RESTful. Puedes leer [https://www.asp.net/single-page-application](../../../single-page-application/index.md)más en .
- Ahora puede crear un VSIX que agregue nuevas plantillas al cuadro de diálogo ASP.NET MVC Nuevo proyecto. Aprenda cómo hacerlo aquí:[https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes paquete &#8211; mvc plantillas de proyecto se han actualizado para incluir el nuevo paquete NuGet 'FixedDisplayModes', que contiene una solución alternativa para un error en MVC 4. Para obtener más información sobre la corrección contenida en[https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)el paquete, consulte esta entrada de blog ( ) del equipo MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API se ha mejorado con varias características nuevas:

- ASP.NET Web API OData
- seguimiento de API web de ASP.NET
- página de ayuda de la API web de ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData le ofrece la flexibilidad que necesita para crear puntos de conexión de OData con lógica empresarial enriquecida sobre cualquier origen de datos. Con ASP.NET Web API OData controla la cantidad de semántica de OData que desea exponer. ASP.NET Web API OData se incluye con las plantillas de proyecto[http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)ASP.NET MVC 4 y también está disponible en NuGet ( ).

ASP.NET Web API OData admite actualmente las siguientes características:

- Habilite la semántica de consultas de OData aplicando el atributo [Queryable].
- Valide fácilmente las consultas de OData y restrinja el conjunto de opciones, operadores y funciones de consulta compatibles.
- Los parámetros se enlazan a ODataQueryOptions directamente para obtener una representación de árbol de sintaxis abstracta de la consulta que, a continuación, se puede validar y aplicar a un IQueryable o IEnumerable.
- Habilite la paginación controlada por servicios y la generación de vínculos de página siguiente especificando límites de resultados en el atributo [Queryable].
- Solicite un recuento insertado del número total de recursos coincidentes mediante $inlinecount.
- Controlar la propagación nula.
- Cualquiera/Todos los operadores en $filter.
- Deducir un modelo de datos de entidad por convención o personalizar explícitamente un modelo de una manera similar a Entity Framework Code-First.
- Exponer conjuntos de entidades derivando de EntitySetController.
- Convenciones simples y personalizables para exponer propiedades de navegación, manipular vínculos e implementar acciones de OData.
- Enrutamiento simplificado mediante el método de extensión MapODataRoute.
- Compatibilidad con el control de versiones mediante la exposición de varios modelos de EDM.
- Exponga el documento de servicio y $metadata para que pueda generar clientes (.NET, Windows Phone, Windows Store, etc.) para la API web.
- Compatibilidad con los formatos detallados Atom, JSON y JSON de OData.
- Crear, actualizar, actualizar parcialmente (PATCH) y eliminar entidades.
- Consultar y manipular relaciones entre entidades.
- Cree vínculos de relación que se conecten a sus rutas.
- Tipos complejos.
- Herencia de tipo de entidad.
- Propiedades de colección.
- Enumeraciones.
- Acciones de OData.
- Construido sobre la misma base que WCF Data[http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)Services, a saber, ODataLib ( ).

Para obtener más información sobre [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141)ASP.NET Web API OData, consulte .

#### <a name="aspnet-web-api-tracing"></a>seguimiento de API web de ASP.NET

ASP.NET Web API Tracing integra los datos de seguimiento de las API web con .NET Tracing. Ahora está habilitado de forma predeterminada en la plantilla de proyecto de API web. Los datos de seguimiento de las API web se envían a la ventana Salida y están disponibles a través de IntelliTrace. ASP.NET seguimiento de API web le permite realizar un seguimiento de la información sobre la API web cuando se hospeda en Windows Azure mediante la integración con Diagnósticos de [Windows Azure.](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx) También puede instalar y habilitar ASP.NET Seguimiento de API web en cualquier[http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)aplicación mediante el paquete NuGet de seguimiento de API web ASP.NET ( ).

Para obtener más información sobre cómo configurar [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874)y usar ASP.NET Web API Tracing, consulte .

#### <a name="aspnet-web-api-help-page"></a>página de ayuda de la API web de ASP.NET

La página de ayuda de ASP.NET Web API ahora se incluye de forma predeterminada en la plantilla de proyecto de API web. La página de ayuda de la API web de ASP.NET genera automáticamente documentación para las API web, incluidos los puntos de conexión HTTP, los métodos HTTP admitidos, los parámetros y las cargas de mensajes de solicitud y respuesta de ejemplo. La documentación se extrae automáticamente de los comentarios del código. También puede agregar la página de ayuda de ASP.NET Web API a[http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)cualquier aplicación mediante el paquete NuGet de página de ayuda de la API web ASP.NET ( ).

Para obtener más información sobre cómo configurar y [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140)personalizar la página de ayuda de ASP.NET Web API, consulte .

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR facilita la adición de capacidades web en tiempo real a la aplicación ASP.NET, mediante WebSockets si está disponible y retrocediendo automáticamente a otras técnicas cuando no lo está.

Para obtener más información sobre [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271)el uso de ASP.NET SignalR, consulte .

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

ASP.NET FriendlyURLs hace que sea muy fácil para los desarrolladores de formularios web para generar direcciones URL de aspecto más limpio (sin la extensión .aspx). Requiere poca o ninguna configuración y se puede utilizar con aplicaciones ASP.NET v4.0 existentes. La característica FriendlyURLs también facilita a los desarrolladores agregar soporte móvil a sus aplicaciones, al admitir el cambio entre vistas de escritorio y móviles.

Para obtener más información sobre cómo instalar [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)y usar ASP.NET URL descriptivas, consulte .

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

En esta sección se describen los problemas conocidos y los cambios importantes que se encuentran en la versión ASP.NET y Web Tools 2012.2.

### <a name="installation-issues"></a>Problemas de instalación

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Instalaciones fuera de servicio de Visual Studio 2012

La instalación de una SKU adicional de Visual Studio 2012 después de instalar ASP.NET y Web Tools 2012.2 requerirá una operación de reparación. Considere la siguiente secuencia:

1. Instalar Visual Studio 2012 Express para Web
2. Instale ASP.NET y Web Tools 2012.2
3. Instale Visual Studio 2012 Professional, Premium o Ultimate

El paso 2 solo resultaría en la instalación de actualizaciones para Express for Web. Para asegurarse de que la SKU adicional instalada durante el paso 3 contiene la actualización, deberá reparar el ASP.NET y Web Tools 2012.2 para instalar las actualizaciones de la última SKU instalada. Esto también se aplica si se invierten las SSU de los pasos 1 y 3.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalación de Microsoft ASP.NET y Web Tools 2012.2 cuando Visual Studio está abierto

Si VS está abierto durante la instalación de Microsoft ASP.NET y Web Tools 2012.2, Visual Studio podría terminar en un estado incorrecto. Se recomienda que los usuarios cierren todas las instancias de Visual Studio antes de continuar con la instalación.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Cancelación de ASP.NET y web Tools 2012.2 en medio de la instalación

La cancelación de ASP.NET y la instalación de Web Tools 2012.2 en medio de la instalación dejará Visual Studio en mal estado. Para solucionar este problema, siga estos pasos: 

- Vaya a Agregar o quitar programas.
- Desinstale Microsoft ASP.NET y Web Tools 2012.2, si está presente.
- Vuelva a instalar Microsoft ASP.NET y Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Después de desinstalar ASP.NET y Web Tools 2012.2 faltan las plantillas de ASP.NET MVC 4 y Razor v2 Web Site

La desinstalación de ASP.NET y Web Tools 2012.2 también desinstalará todas las plantillas de sitio web de ASP.NET MVC 4 y Razor v2 de Visual Studio 2012.

La solución consiste en reparar la instalación de Visual Studio 2012 para reinstalar ASP.NET MVC 4 y Razor v2 Plantillas de sitio web.

### <a name="tooling-issues"></a>Problemas de herramientas

#### <a name="nuget-error-reported-during-project-creation"></a>Error de NuGet notificado durante la creación del proyecto

Después de instalar ASP.NET y Web Tools 2012.2 puede ver el siguiente error al crear un proyecto MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

El ASP.NET y Web Tools 2012.2 incluye NuGet 2.1 y actualizará la extensión en Visual Studio 2012. En algunos casos, el instalador de VSIX no podrá actualizar correctamente VSIX. Los siguientes pasos le permitirán solucionar este problema:

1. Inicie Visual Studio 2012 como administrador
2. Vaya a&gt;Herramientas- Extensiones y actualizaciones y desinstale NuGet.
3. Cierre Visual Studio.
4. Vaya a la carpeta de instalación de ASP.NET y Web Tools 2012.2:

    1. Para Visual Studio 2012: Archivos de **programa, Microsoft ASP.NET, ASP.NET Web Stack, Visual Studio 2012**
    2. Para Visual Studio 2012 Express para Web: Archivos de **programa, Microsoft ASP.NET, ASP.NET Web Stack, Visual Studio Express 2012 para Web**
5. Haga doble clic en NuGet.Tools.vsix para reinstalar NuGet

### <a name="web-api-issues"></a>Problemas con la API web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Problemas de análisis en $filter y DateTime literales

El analizador de URI de OData no puede analizar correctamente los literales de fecha y hora parciales. Por ejemplo, $filter-start eq datetime'2012-12-31T12:00' no se puede analizar correctamente. Una solución alternativa es utilizar el literal completo, $filter-start eq datetime'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData no admite nombres de propiedad que no distinguen mayúsculas de minúsculas.

OData no admite nombres de propiedad que no distinguen mayúsculas de minúsculas en consultas de OData y ruta de acceso de odata. Ver elementos de trabajo:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Si los usuarios tienen diferentes mayúsculas y minúsculas en el lado del cliente javascript y el lado del servidor, probablemente encontrarán este problema. Este problema es por diseño en el protocolo odata. Sin embargo, muchos usuarios informan de este problema. Para evitarlo, los usuarios tienen que corregir sus casos en la URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Las convenciones de enrutamiento de OData predeterminadas no admiten POST/PUT en la propiedad de navegación.

Las convenciones de enrutamiento de OData predeterminadas no admiten POST/PUT en la propiedad de navegación. Consulte workitem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Nos falta esta convención de uso común en las convenciones predeterminadas.

Para evitarlo, los usuarios deben ampliar la nueva convención de enrutamiento para admitirlo.

### <a name="facebook-template-issues"></a>Problemas con la plantilla de Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>La plantilla de aplicación de Facebook solo funciona con .NET 4.5

Debe seleccionar .NET 4.5 en la lista desplegable de marco de trabajo en el cuadro de diálogo Nuevo proyecto para ver la plantilla Aplicación de Facebook en ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controlador de actualizaciones en tiempo real

La plantilla Aplicación de Facebook permite al usuario crear fácilmente un controlador de API web para gestionar las actualizaciones en tiempo real de Facebook. Si la máquina de desarrollo está detrás de NAT, es posible que el controlador no funcione sin más configuración de red. Consulte aquí para obtener más información:[http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Los valores de cadena de consulta entran en conflicto con los parámetros de OAuth de Facebook

Los siguientes campos entran en conflicto con la URL de devolución de llamada del cuadro de diálogo de Facebook OAuth. No agregue sus propios valores de cadena de consulta\_con\_los siguientes nombres: código, error, descripción del error, motivo del error.

#### <a name="using-page-inspector-with-facebook-template"></a>Uso de Page Inspector con plantilla de Facebook

No puede usar la característica Inspector de página en Visual Studio 2012 al depurar la aplicación de Facebook. El Inspector de página no admite actualmente iframes.

### <a name="single-page-application-template-issues"></a>Problemas de plantilla de aplicación de una sola página

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con la actualización de JQuery 1.9/Knockout 2.2.1, al ejecutar el proyecto SPA MVC predeterminado, el nuevo evento de enfoque de entrada de elemento de todo no se controla correctamente.

Con la actualización de JQuery 1.9/Knockout 2.2.1, al ejecutar el proyecto SPA MVC predeterminado, la nueva edición de elementos de todo escriba ya no el foco de nuevo en el nuevo cuadro de edición de elementos de todo después de escribir el nuevo elemento todo en la lista de todo.

Para realizar [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)una referencia alternativa y realizar una corrección similar al siguiente código de ejemplo:

Archivo todo.model.js  
 función todolist(data), agregue lo siguiente:  
 **self.isSelected á ko.observable(false);**

función todoList.prototype.addTodo, agregue el siguiente texto en negro:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

File index.cshtml, agregue el siguiente texto en negro:  
 &lt;formulario data-bind:&quot;addTodo&quot;&gt;  
 &lt;clase de&quot;entrada&quot; addTodo type&quot;- text&quot; data-bind ?&quot;valor: newTodoTitle, marcador de posición: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: ? blur: addTodo ?&quot; /&gt;  
 &lt;/form&gt;
