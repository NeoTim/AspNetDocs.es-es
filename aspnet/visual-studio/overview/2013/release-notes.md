---
uid: visual-studio/overview/2013/release-notes
title: Notas de la versión de ASP.NET y Web Tools para Visual Studio 2013 Microsoft Docs
author: rick-anderson
description: Este documento describe la versión de ASP.NET y Web Tools para Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 4494b4acdd30384e1def89b7fff9bad30933e38e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543501"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Notas de la versión de ASP.NET and Web Tools para Visual Studio 2013

por [Microsoft](https://github.com/microsoft)

> Este documento describe la versión de ASP.NET y Web Tools para Visual Studio 2013.

## <a name="contents"></a>Contenido

- [Notas de instalación](#TOC1)
- [Documentación](#TOC2)
- [Requisitos de software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características en ASP.NET y Herramientas web para Visual Studio 2013

- [Una ASP.NET](#TOC6)
- [Nueva experiencia en proyectos web](#newproj)
- [andamios ASP.NET](#scaffold)
- [Vínculo con exploradores](#browser-link)
- [Mejoras de Visual Studio Web Editor](#web-editor)
- [Compatibilidad con aplicaciones web de Azure App Service en Visual Studio](#waws)
- [Mejoras en la publicación web](#publish)
- [NuGet 2.7](#nuget)
- [Formularios Web Forms ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Componentes de Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET App Suspend](#TOC15)
- [Problemas conocidos y cambios importantes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notas de la instalación

ASP.NET y Web Tools para Visual Studio 2013 se incluyen en el instalador principal y se pueden descargar [aquí.](https://www.asp.net/downloads)

<a id="TOC2"></a>
## <a name="documentation"></a>Documentación

Los tutoriales y otra información sobre ASP.NET y Herramientas web para Visual Studio 2013 están disponibles en el [sitio web de ASP.NET.](https://www.asp.net/)

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET y Web Tools requiere Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características en ASP.NET y Herramientas web para Visual Studio 2013

En las secciones siguientes se describen las características que se han introducido en la versión.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Una ASP.NET

Con el lanzamiento de Visual Studio 2013, hemos dado un paso hacia el uso de tecnologías de ASP.NET, para que pueda mezclar y combinar fácilmente las que desee. Por ejemplo, puede iniciar un proyecto mediante MVC y agregar fácilmente páginas de formularios Web Forms al proyecto más adelante, o scaffolding Web API en un proyecto de formularios Web Forms. Una ASP.NET se trata de hacer que sea más fácil para usted como desarrollador para hacer las cosas que ama en ASP.NET. Independientemente de la tecnología que elija, puede tener confianza en que está basándose en el marco subyacente de confianza de One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nueva experiencia en proyectos web

Hemos mejorado la experiencia de crear nuevos proyectos web en Visual Studio 2013. En el cuadro de diálogo **Nuevo ASP.NET proyecto web** puede seleccionar el tipo de proyecto que desee, configurar cualquier combinación de tecnologías (formularios Web Forms, MVC, Web API), configurar opciones de autenticación y agregar un proyecto de prueba unitaria.

![Proyecto de nueva ASP.NET](release-notes/_static/image1.png)

El nuevo cuadro de diálogo le permite cambiar las opciones de autenticación predeterminadas para muchas de las plantillas. Por ejemplo, al crear un proyecto de formularios Web Forms ASP.NET, puede seleccionar cualquiera de las siguientes opciones:

- Sin autenticación
- Cuentas de usuario individuales (ASP.NET la membresía o el proveedor de redes sociales inician sesión)
- Cuentas organizativas (Active Directory en una aplicación de Internet)
- Autenticación de Windows (Active Directory en una aplicación de intranet)

![Opciones de autenticación](release-notes/_static/image2.png)

Para obtener más información sobre el nuevo proceso para crear proyectos web, vea [Crear ASP.NET proyectos web en Visual Studio 2013](creating-web-projects-in-visual-studio.md). Para más información sobre las nuevas opciones de autenticación, vea [ASP.NET identidad](#TOC8) más adelante en este documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>andamios ASP.NET

ASP.NET Scaffolding es un marco de generación de código para ASP.NET aplicaciones web. Facilita la adición de código reutilizable al proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, el scaffolding se limitaba a ASP.NET proyectos MVC. Con Visual Studio 2013, ahora puede usar scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms. Visual Studio 2013 no admite actualmente la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms agregando dependencias MVC al proyecto. La compatibilidad con la generación de páginas para formularios Web Forms se agregará en una actualización futura.

Cuando se usa scaffolding, nos aseguramos de que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si comienza con un proyecto de formularios Web Forms ASP.NET y, a continuación, usa scaffolding para agregar un controlador de API web, los paquetes NuGet y las referencias necesarios se agregan automáticamente al proyecto.

Para agregar scaffolding MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento con scaffolding** y seleccione **dependencias MVC 5** en la ventana de diálogo. Hay dos opciones para scaffolding MVC; Mínimo y Completo. Si selecciona Mínimo, solo se agregan al proyecto los paquetes NuGet y las referencias de ASP.NET MVC. Si selecciona la opción Completo, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto MVC.

La compatibilidad con controladores asincrónicos de scaffolding utiliza las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y tutoriales, vea [ASP.NET Scaffolding Overview](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Vínculo del explorador: canal de SignalR entre el explorador y Visual Studio

La nueva característica [vínculo de explorador](using-browser-link.md) le permite conectar varios exploradores a Visual Studio y actualizarlos todos haciendo clic en un botón de la barra de herramientas. Puede conectar varios exploradores al sitio de desarrollo, incluidos los emuladores móviles, y hacer clic en Actualizar para actualizar todos los exploradores al mismo tiempo. Browser Link también expone una API para permitir a los desarrolladores escribir extensiones de vínculo de explorador.

![](release-notes/_static/image3.png)

Al permitir a los desarrolladores aprovechar la API de vínculo de explorador, es posible crear escenarios muy avanzados que traspasan los límites entre Visual Studio y cualquier explorador que esté conectado. Web Essentials aprovecha la API para crear una experiencia integrada entre Visual Studio y las herramientas de desarrollo del explorador, control remoto de emuladores móviles y mucho más.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Mejoras de Visual Studio Web Editor

Visual Studio 2013 incluye un nuevo editor HTML para archivos Razor y archivos HTML en aplicaciones web. El nuevo editor HTML proporciona un único esquema unificado basado en HTML5. Tiene finalización automática de llaves, jQuery UI y AngularJS atributo IntelliSense, atributo IntelliSense Agrupación, ID y nombre de clase Intellisense, y otras mejoras que incluyen un mejor rendimiento, formato y SmartTags.

En la siguiente captura de pantalla se muestra el uso del atributo Bootstrap IntelliSense en el editor HTML.

![Intellisense en el editor HTML](release-notes/_static/image4.png)

Visual Studio 2013 también incluye editores CoffeeScript y LESS integrados. El editor LESS viene con todas las características interesantes del editor CSS y tiene Intellisense específico @import para variables y mixins en todos los documentos LESS en la cadena.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Compatibilidad con aplicaciones web de Azure App Service en Visual Studio

En Visual Studio 2013 con el SDK de Azure para .NET 2.2, puede usar el **Explorador** de servidores para interactuar directamente con las aplicaciones web remotas. Puede iniciar sesión en su cuenta de Azure, crear nuevas aplicaciones web, configurar aplicaciones, ver registros en tiempo real y mucho más. Próximamente después de que se publique SDK 2.2, podrá ejecutar en modo de depuración de forma remota en Azure. La mayoría de las nuevas características de Azure App Service Web Apps también funcionan en Visual Studio 2012 al instalar la versión actual del SDK de Azure para .NET.

Para obtener más información, vea los siguientes recursos:

- [Cree una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Mejoras en la publicación web

Visual Studio 2013 incluye características de publicación web nuevas y mejoradas. Estas son algunos de ellos:

- Automatice fácilmente el cifrado de [archivos Web.config.](https://go.microsoft.com/fwlink/?LinkId=325529) (Este enlace y los dos siguientes apuntan a la documentación en MSDN que podría no estar disponible hasta tarde en el día el 10/17.)
- Automatice fácilmente [la sinconexión](https://go.microsoft.com/fwlink/?LinkId=325530)de una aplicación durante la implementación.
- Configure Web Deploy para usar la suma de comprobación de archivos [en lugar de la fecha](https://go.microsoft.com/fwlink/?LinkId=325531) de último cambio para determinar qué archivos se deben copiar en el servidor.
- Publique rápidamente archivos seleccionados individuales (incluido Web.config) cuando utilice los métodos de publicación FTP o del sistema de archivos, así como con Web Deploy.

Para obtener más información acerca de ASP.NET implementación web, consulte [el sitio de ASP.NET.](https://go.microsoft.com/fwlink/?LinkId=322027)

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en las notas de la versión de [NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet también elimina la necesidad de proporcionar el consentimiento explícito para que la característica de restauración de paquetes de NuGet descargue paquetes. El consentimiento (y la casilla de verificación asociada en el cuadro de diálogo de preferencias de NuGet) ahora se concede mediante la instalación de NuGet. Ahora la restauración del paquete simplemente funciona de forma predeterminada.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms ASP.NET

### <a name="one-aspnet"></a>Una ASP.NET

Las plantillas de proyecto de formularios Web Forms se integran perfectamente con la nueva experiencia One ASP.NET. Puede agregar compatibilidad con MVC y Web API al proyecto de formularios Web Forms y puede configurar la autenticación mediante el Asistente para creación de proyectos One ASP.NET. Para obtener más información, vea [Crear ASP.NET proyectos web en Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto de formularios Web Forms admiten el nuevo marco de ASP.NET Identity. Además, las plantillas ahora admiten la creación de un proyecto de intranet de formularios Web Forms. Para obtener más información, vea [Métodos](creating-web-projects-in-visual-studio.md#auth) de autenticación en **la creación de ASP.NET proyectos web en Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Las plantillas de formularios Web Forms usan [Bootstrap](http://twitter.github.io/bootstrap/) para proporcionar un aspecto elegante y responsivo que puede personalizar fácilmente. Para obtener más información, vea Bootstrap en las plantillas de proyecto web de [Visual Studio 2013.](creating-web-projects-in-visual-studio.md#bootstrap)

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Una ASP.NET

Las plantillas de proyecto de Web MVC se integran perfectamente con la nueva experiencia One ASP.NET. Puede personalizar el proyecto MVC y configurar la autenticación mediante el Asistente para la creación de proyectos uno ASP.NET. Puede encontrar un tutorial introductorio para ASP.NET MVC 5 en [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obtener información sobre cómo actualizar proyectos MVC 4 a MVC 5, vea Cómo actualizar un ASP.NET MVC 4 y proyecto de [API web para ASP.NET MVC 5 y Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto MVC se han actualizado para usar ASP.NET identidad para la autenticación y la administración de identidades. Un tutorial con la autenticación de Facebook y Google y la nueva API de pertenencia se puede encontrar en Crear una aplicación [ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) y Crear una aplicación MVC ASP.NET con auth y SQL DB e implementar en Azure App [Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

La plantilla de proyecto MVC se ha actualizado para usar [Bootstrap](http://getbootstrap.com/) para proporcionar un aspecto elegante y responsivo que puede personalizar fácilmente. Para obtener más información, vea Bootstrap en las plantillas de proyecto web de [Visual Studio 2013.](creating-web-projects-in-visual-studio.md#bootstrap)

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro en ASP.NET MVC que se ejecutan antes de los filtros de autorización en la canalización de ASP.NET MVC y permiten especificar la lógica de autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales de la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden agregar desafíos de autenticación en respuesta a solicitudes no autorizadas.

### <a name="filter-overrides"></a>Anulaciones de filtro

Ahora puede invalidar qué filtros se aplican a un método de acción o controlador determinado especificando un filtro de invalidación. Los filtros de invalidación especifican un conjunto de tipos de filtro que no se deben ejecutar para un ámbito determinado (acción o controlador). Esto le permite configurar filtros que se aplican globalmente, pero luego excluir ciertos filtros globales de la aplicación a acciones o controladores específicos.

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET MVC ahora soporta el enrutamiento de atributos, gracias [http://attributerouting.net](http://attributerouting.net)a una contribución de Tim McCall, el autor de . Con el enrutamiento de atributos puede especificar las rutas anotando sus acciones y controladores.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET Web API ahora admite el enrutamiento de atributos, [http://attributerouting.net](http://attributerouting.net)gracias a una contribución de Tim McCall, el autor de . Con el enrutamiento de atributos puede especificar las rutas de la API web anotando sus acciones y controladores como este:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

El enrutamiento de atributos proporciona más control sobre los URI de la API web. Por ejemplo, puede definir fácilmente una jerarquía de recursos mediante un único controlador de API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

El enrutamiento de atributos también proporciona una sintaxis conveniente para especificar parámetros opcionales, valores predeterminados y restricciones de ruta:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Para obtener más información sobre el enrutamiento de atributos, consulte [Enrutamiento de atributos en Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Las plantillas de proyecto de API web y aplicación de página única ahora admiten la autorización mediante OAuth 2.0. OAuth 2.0 es un marco para autorizar el acceso de cliente a los recursos protegidos. Funciona para una variedad de clientes, incluyendo navegadores y dispositivos móviles.

La compatibilidad con OAuth 2.0 se basa en el nuevo middleware de seguridad proporcionado por microsoft OWIN Components para la autenticación del portador y la implementación del rol de servidor de autorización. Como alternativa, los clientes se pueden autorizar mediante un servidor de autorización organizativa, como Azure Active Directory o ADFS en Windows Server 2012 R2.

### <a name="odata-improvements"></a>Mejoras en OData

**Soporte para $select, $expand, $batch y $value**

ASP.NET Web API OData ahora tiene soporte completo para $select, $expand y $value. También puede utilizar $batch para el procesamiento por lotes de solicitudes y el procesamiento de conjuntos de cambios.

Las opciones $select y $expand permiten cambiar la forma de los datos que se devuelven desde un extremo de OData. Para obtener más información, consulte [Introducción a $select y $expand compatibilidad con OData de API web.](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)

**Extensibilidad mejorada**

Los formateadores de OData ahora son extensibles. Puede agregar metadatos de entrada de Atom, admitir entradas de flujo y vínculos multimedia con nombre, agregar anotaciones de instancia y personalizar cómo se generan los vínculos.

**Soporte sin tipo**

Ahora puede crear servicios DeData sin necesidad de definir tipos CLR para los tipos de entidad. En su lugar, los controladores de OData pueden tomar o devolver instancias de **IEdmObject**, que son los formateadores de OData serializar/deserializar.

**Reutilizar un modelo existente**

Si ya tiene un modelo de datos de entidad (EDM) existente, ahora puede reutilizarlo directamente, en lugar de tener que crear uno nuevo. Por ejemplo, si usa Entity Framework, puede usar el EDM que EF compila para usted.

### <a name="request-batching"></a>Solicitar procesamiento por lotes

El procesamiento por lotes de solicitudes combina varias operaciones en una sola solicitud HTTP POST, para reducir el tráfico de red y proporcionar una interfaz de usuario más fluida y menos conversacional. ASP.NET Web API ahora admite varias estrategias para el procesamiento por lotes de solicitudes:

- Use el extremo $batch de un servicio OData.
- Empaquete varias solicitudes en una sola solicitud multiparte MIME.
- Utilice un formato de procesamiento por lotes personalizado.

Para habilitar el procesamiento por lotes de solicitudes, simplemente agregue una ruta con un controlador de procesamiento por lotes a la configuración de la API web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

También puede controlar si las solicitudes se ejecutan secuencialmente o en cualquier orden.

### <a name="portable-aspnet-web-api-client"></a>Cliente de API web ASP.NET portátil

Ahora puede usar el cliente de API web ASP.NET para crear bibliotecas de clases portátiles que funcionen en las aplicaciones de Windows Store y Windows Phone 8. También puede crear formateadores portátiles que se pueden compartir entre el cliente y el servidor.

### <a name="improved-testability"></a>Máxima capacidad de detección

Web API 2 facilita mucho la prueba unitaria de los controladores de API. Simplemente cree una instancia del controlador de API con el mensaje de solicitud y la configuración y, a continuación, llame al método de acción que desea probar. También es fácil simular la clase **UrlHelper,** para los métodos de acción que realizan la generación de vínculos.

### <a name="ihttpactionresult"></a>IHttpActionResult

Ahora puede implementar IHttpActionResult para encapsular el resultado de los métodos de acción de la API web. El tiempo de ejecución de ASP.NET Web API ejecuta un IHttpActionResult devuelto desde un método de acción de API web para generar el mensaje de respuesta resultante. Se puede devolver un IHttpActionResult desde cualquier acción de API web para simplificar las pruebas unitarias de la implementación de la API web. Para mayor comodidad, se proporcionan varias implementaciones de IHttpActionResult de fábrica, incluidos los resultados para devolver códigos de estado específicos, contenido con formato o respuestas negociadas por contenido.

### <a name="httprequestcontext"></a>HttpRequestContext

El nuevo **HttpRequestContext** realiza un seguimiento de cualquier estado que está vinculado a la solicitud, pero no está disponible inmediatamente desde la solicitud. Por ejemplo, puede usar **HttpRequestContext** para obtener datos de ruta, la entidad de seguridad asociada a la solicitud, el certificado de cliente, **El UrlHelper** y la raíz de ruta de acceso virtual. Puede crear fácilmente un **HttpRequestContext** para fines de pruebas unitarias.

Dado que la entidad de seguridad de la solicitud se fluye con la solicitud en lugar de depender de **Thread.CurrentPrincipal**, la entidad de seguridad ahora está disponible durante toda la duración de la solicitud mientras está en la canalización de la API web.

### <a name="cors"></a>CORS

Gracias a otra gran contribución de Brock Allen, ASP.NET ahora es totalmente compatible con Cross Origin Request Sharing (CORS).

La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio. [CORS](http://www.w3.org/TR/cors/) es un estándar W3C que permite a un servidor relajar la directiva del mismo origen. Con CORS, un servidor puede permitir explícitamente algunas solicitudes de origen cruzado y rechazar otras.

Web API 2 ahora es compatible con CORS, incluido el control automático de solicitudes de comprobación previa. Para obtener más información, consulte [Habilitación de solicitudes entre orígenes en API web de ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro en ASP.NET API web que se ejecutan antes de los filtros de autorización en la canalización de API web de ASP.NET y le permiten especificar la lógica de autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales de la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden agregar desafíos de autenticación en respuesta a solicitudes no autorizadas.

### <a name="filter-overrides"></a>Anulaciones de filtro

Ahora puede invalidar qué filtros se aplican a un determinado método de acción o controlador, especificando un filtro de invalidación. Los filtros de invalidación especifican un conjunto de tipos de filtro que no se deben ejecutar para un ámbito determinado (acción o controlador). Esto le permite agregar filtros globales, pero luego excluir algunos de acciones o controladores específicos.

### <a name="owin-integration"></a>Integración con OWIN

ASP.NET Web API ahora es totalmente compatible con OWIN y se puede ejecutar en cualquier host compatible con OWIN. También se incluye un **HostAuthenticationFilter** que proporciona integración con el sistema de autenticación OWIN.

Con la integración de OWIN, puede autohospedar la API web en su propio proceso junto con otro middleware de OWIN, como SignalR. Para obtener más información, consulte [Usar OWIN para autohospedar ASP.NET API web](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

En las secciones siguientes se describen las características de SignalR 2.0.

- [Construido sobre OWIN](#builtonowin)
- [MapHubs y MapConnection ahora son MapSignalR](#MapSignalR)
- [Soporte entre dominios](#crossdomain)
- [Soporte para iOS y Android a través de MonoTouch y MonoDroid](#mobile)
- [Cliente .NET portátil](#portable)
- [Nuevo paquete de auto-anfitrión](#selfhost)
- [Soporte de servidor compatible con versiones anteriores](#backwardcompat)
- [Se ha eliminado la compatibilidad del servidor con .NET 4.0](#remove40)
- [Envío de un mensaje a una lista de clientes y grupos](#messagelist)
- [Enviar un mensaje a un usuario específico](#sendtouser)
- [Mejor soporte para el manejo de errores](#errorhandling)
- [Pruebas unitarias más fáciles de los bujes](#unittesting)
- [Manejo de errores de JavaScript](#javascripterror)

Para obtener un ejemplo de cómo actualizar un proyecto 1.x existente a SignalR 2.0, vea [Actualizar un proyecto de SignalR 1.x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Construido sobre OWIN

SignalR 2.0 se basa completamente en [OWIN (la interfaz Web abierta para .NET).](http://owin.org/) Este cambio hace que el proceso de instalación de SignalR sea mucho más coherente entre las aplicaciones de SignalR hospedadas en web y autohospedadas, pero también ha requerido una serie de cambios en la API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs y MapConnection ahora son MapSignalR

Para la compatibilidad con los estándares OWIN, estos métodos se han renombrado a `MapSignalR`. `MapSignalR`llamado sin parámetros asignará `MapHubs` todos los concentradores (como lo hace en la versión 1.x); para asignar objetos **PersistentConnection** individuales, especifique el tipo de conexión como parámetro de tipo y la extensión de dirección URL para la conexión como primer argumento.

Se `MapSignalR` llama al método en una clase de inicio de Owin. Visual Studio 2013 contiene una nueva plantilla para una clase de inicio de Owin; para utilizar esta plantilla, haga lo siguiente:

1. Haga clic con el botón derecho en el proyecto
2. Seleccione **Agregar**, **Nuevo elemento...**
3. Seleccione **Owin Startup (Clase de inicio)**. Asigne el nombre Startup.cs la nueva **clase**.

En una **aplicación web,** la clase `MapSignalR` de inicio de Owin que contiene el método se agrega al proceso de inicio de Owin mediante una entrada en el nodo de configuración de la aplicación del archivo Web.Config, como se muestra a continuación.

En una **aplicación autohospedada**, la clase Startup se `WebApp.Start` pasa como parámetro de tipo del método.

**Asignación de concentradores y conexiones en SignalR 1.x (desde el archivo de aplicación global en una aplicación web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Asignación de concentradores y conexiones en SignalR 2.0 (desde un archivo de clase de inicio de Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

En una **aplicación autohospedada**, la clase Startup se `WebApp.Start` pasa como parámetro de tipo para el método, como se muestra a continuación.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Soporte entre dominios

En SignalR 1.x, las solicitudes entre dominios se controlaban mediante una única marca EnableCrossDomain. Esta marca controlaba las solicitudes JSONP y CORS. Para una mayor flexibilidad, toda la compatibilidad con CORS se ha quitado del componente de servidor de SignalR (los clientes JavaScript siguen utilizando CORS normalmente si se detecta que el explorador lo admite) y el nuevo middleware OWIN se ha puesto a disposición para admitir estos escenarios.

En SignalR 2.0, si se requiere JSONP en el cliente (para admitir solicitudes entre dominios `EnableJSONP` en `HubConfiguration` exploradores `true`antiguos), deberá habilitarse explícitamente estableciendo en el objeto en , como se muestra a continuación. JSONP está deshabilitado de forma predeterminada, ya que es menos seguro que CORS.

Para agregar el nuevo middleware CORS en SignalR 2.0, agregue la `Microsoft.Owin.Cors` biblioteca al proyecto y llame `UseCors` antes del middleware de SignalR, como se muestra en la sección siguiente.

**Agregar Microsoft.Owin.Cors al proyecto:** para instalar esta biblioteca, ejecute el siguiente comando en la consola del Administrador de paquetes:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Este comando agregará la versión 2.0.0 del paquete al proyecto.

**Llamando a UseCors**

Los siguientes fragmentos de código muestran cómo implementar conexiones entre dominios en SignalR 1.x y 2.0.

**Implementación de solicitudes entre dominios en SignalR 1.x (desde el archivo de aplicación global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementación de solicitudes entre dominios en SignalR 2.0 (desde un archivo de código de C))**

El código siguiente muestra cómo habilitar CORS o JSONP en un proyecto de SignalR 2.0. Este ejemplo `Map` de `RunSignalR` código `MapSignalR`usa y en lugar de , para que el middleware CORS se ejecute solo para `MapSignalR`las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta especificada en .) `Map` También se puede usar para cualquier otro middleware que necesite ejecutarse para un prefijo de dirección URL específico, en lugar de para toda la aplicación.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>Soporte para iOS y Android a través de MonoTouch y MonoDroid

Se ha agregado compatibilidad para clientes iOS y Android mediante componentes MonoTouch y MonoDroid de la [biblioteca Xamarin.](https://xamarin.com/) Para obtener más información sobre cómo usarlos, vea Uso de [componentes](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)de Xamarin . Estos componentes estarán disponibles en [Xamarin Store](https://store.xamarin.com/) cuando la versión de SignalR RTW está disponible.

<a id="portable"></a>Cliente .NET portátil

Para facilitar mejor el desarrollo multiplataforma, los clientes de Silverlight, WinRT y Windows Phone se han reemplazado por un único cliente .NET portátil que admite las siguientes plataformas:

- NET 4.5
- Silverlight 5
- WinRT (.NET para aplicaciones de la Tienda Windows)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuevo paquete de auto-anfitrión

Ahora hay un paquete NuGet para que sea más fácil empezar a usar SignalR Self-Host (aplicaciones de SignalR que se hospedan en un proceso u otra aplicación, en lugar de hospedarse en un servidor web). Para actualizar un proyecto de autohospedaje compilado con SignalR 1.x, quite el paquete Microsoft.AspNet.SignalR.Owin y agregue el paquete Microsoft.AspNet.SignalR.SelfHost. Para obtener más información sobre cómo empezar a usar el paquete de autohospedaje, vea [Tutorial: Autohospedaje de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Soporte de servidor compatible con versiones anteriores

En versiones anteriores de SignalR, las versiones del paquete SignalR utilizado en el cliente y el servidor debían ser idénticos. Para admitir aplicaciones de cliente grueso que serían difíciles de actualizar, SignalR 2.0 ahora admite el uso de una versión de servidor más reciente con un cliente anterior. **Nota: SignalR 2.0 no soporta servidores construidos con versiones anteriores con clientes más nuevos.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Se ha eliminado la compatibilidad del servidor con .NET 4.0

SignalR 2.0 ha eliminado la compatibilidad con la interoperabilidad del servidor con .NET 4.0. .NET 4.5 debe utilizarse con servidores SignalR 2.0. Todavía hay un cliente de .NET 4.0 para SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Envío de un mensaje a una lista de clientes y grupos

En SignalR 2.0, es posible enviar un mensaje mediante una lista de identificadores de cliente y grupo. Los siguientes fragmentos de código muestran cómo hacerlo.

**Envío de un mensaje a una lista de clientes y grupos mediante PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Envío de un mensaje a una lista de clientes y grupos mediante Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Enviar un mensaje a un usuario específico

Esta característica permite a los usuarios especificar lo que el userId se basa en un IRequest a través de una nueva interfaz IUserIdProvider:

**La interfaz IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

De forma predeterminada, habrá una implementación que usa el IPrincipal.Identity.Name del usuario como nombre de usuario.

En los hubs, podrá enviar mensajes a estos usuarios a través de una nueva API:

**Uso de la API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Mejor soporte de manejo de errores

Los usuarios ahora pueden iniciar **HubException** desde cualquier invocación de concentrador. El constructor de la **HubException** puede tomar un mensaje de cadena y un objeto datos de error adicionales. SignalR serializará automáticamente la excepción y la enviará al cliente donde se usará para rechazar o producir un error en la invocación del método de concentrador.

La configuración de excepciones de **concentrador detallada de la demostración** no tiene ninguna relación con **HubException** que se devuelve al cliente o no; siempre se envía.

**Código del lado del servidor que muestra el envío de una HubException al cliente**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Código de cliente JavaScript que demuestra responder a una HubException enviada desde el servidor**

[!code-html[Main](release-notes/samples/sample16.html)]

**Código de cliente .NET que demuestra responder a una excepción HubException enviada desde el servidor**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Pruebas unitarias más fáciles de los bujes

SignalR 2.0 incluye `IHubCallerConnectionContext` una interfaz llamada en hubs que facilita la creación de invocaciones simuladas del lado cliente. Los siguientes fragmentos de código muestran el uso de esta interfaz con los divertidos de prueba populares [xUnit.net](https://github.com/xunit/xunit) y [moq.](https://code.google.com/p/moq/)

**Señalder de prueba unitaria con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Señalder de prueba unitaria con moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Manejo de errores de JavaScript

En SignalR 2.0, todas las devoluciones de llamada de control de errores de JavaScript devuelven objetos de error de JavaScript en lugar de cadenas sin procesar. Esto permite que SignalR fluya información más completa a los controladores de errores. Puede obtener la excepción `source` interna de la propiedad del error.

**Código de cliente JavaScript que controla la excepción Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nuevo sistema de membresía ASP.NET

ASP.NET Identidad es el nuevo sistema de pertenencia para las aplicaciones ASP.NET. ASP.NET Identity facilita la integración de datos de perfil específicos del usuario con datos de aplicaciones. ASP.NET Identity también le permite elegir el modelo de persistencia para los perfiles de usuario de la aplicación. Puede almacenar los datos en una base de datos de SQL Server o en otro almacén de datos, incluidos almacenes de datos NoSQL como las tablas de Azure Storage. Para obtener más información, vea Cuentas de [usuario individuales](creating-web-projects-in-visual-studio.md#indauth) en **Creación de proyectos web de ASP.NET en Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticación basada en notificaciones

ASP.NET ahora admite la autenticación basada en notificaciones, donde la identidad del usuario se representa como un conjunto de notificaciones de un emisor de confianza. Los usuarios se pueden autenticar con un nombre de usuario y una contraseña mantenidos en una base de datos de aplicaciones o mediante proveedores de identidades sociales (por ejemplo: cuentas de Microsoft, Facebook, Google, Twitter) o mediante cuentas organizativas a través de Azure Active Directory o Servicios de federación de Active Directory (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integración con Azure Active Directory y Windows Server Active Directory

Ahora puede crear ASP.NET proyectos que usen Azure Active Directory o Windows Server Active Directory (AD) para la autenticación. Para obtener más información, vea [Cuentas organizativas](creating-web-projects-in-visual-studio.md#orgauth) en **Creación de ASP.NET proyectos web en Visual Studio 2013**.

### <a name="owin-integration"></a>Integración con OWIN

ASP.NET autenticación se basa ahora en el middleware OWIN que se puede usar en cualquier host basado en OWIN. Para obtener más información acerca de OWIN, consulte la siguiente sección Componentes de [Microsoft OWIN.](#TOC7)

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

[Open Web Interface for .NET](http://owin.org/) (OWIN) define una abstracción entre servidores web de .NET y aplicaciones web. OWIN desacopla la aplicación web del servidor, lo que hace que las aplicaciones web sean independientes del host. Por ejemplo, puede hospedar una aplicación web basada en OWIN en IIS o hospedarla de forma automática en un proceso personalizado.

Los cambios introducidos en los componentes de Microsoft OWIN (también conocido como el proyecto Katana) incluyen nuevos componentes de servidor y host, nuevas bibliotecas auxiliares y middleware, y nuevo middleware de autenticación.

Para obtener más información sobre OWIN y Katana, consulte [Novedades de OWIN y Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: Las aplicaciones [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) no se pueden ejecutar en modo clásico de IIS; deben ejecutarse en modo integrado.**

**Nota: Las aplicaciones [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) deben ejecutarse con plena confianza.**

### <a name="new-servers-and-hosts"></a>Nuevos servidores y hosts

Con esta versión, se agregaron nuevos componentes para habilitar escenarios de autohospedaje. Estos componentes incluyen los siguientes paquetes NuGet:

- **Microsoft.Owin.Host.HttpListener**. Proporciona un servidor OWIN que usa **HttpListener** para escuchar las solicitudes HTTP y dirigirlas a la canalización de OWIN.
- **Microsoft.Owin.Hosting** Proporciona una biblioteca para los desarrolladores que desean hospedar una canalización de OWIN en un proceso personalizado, como una aplicación de consola o un servicio de Windows.
- **OwinHost**. Proporciona un ejecutable independiente que `Microsoft.Owin.Hosting` se ajusta y le permite hospedar una canalización de OWIN sin tener que escribir una aplicación host personalizada.

Además, `Microsoft.Owin.Host.SystemWeb` el paquete ahora permite que el middleware proporcione sugerencias al servidor **SystemWeb,** lo que indica que se debe llamar al middleware durante una fase de canalización de ASP.NET específica. Esta característica es especialmente útil para el middleware de autenticación, que debe ejecutarse al principio de la canalización de ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Bibliotecas auxiliares y Middleware

Aunque puede escribir componentes OWIN utilizando solo las definiciones de `Microsoft.Owin` función y tipo de la especificación OWIN, el nuevo paquete proporciona un conjunto más fácil de usar de abstracciones. Este paquete combina varios paquetes anteriores `Owin.Extensions`(por ejemplo, , `Owin.Types`) en un único modelo de objetos bien estructurado que puede ser fácilmente utilizado por otros componentes OWIN. De hecho, la mayoría de los componentes de Microsoft OWIN ahora utilizan este paquete.

> [!NOTE]
> Las aplicaciones [OWIN](http://www.owin.org) no se pueden ejecutar en modo clásico de IIS; deben ejecutarse en modo integrado.

> [!NOTE]
> Las aplicaciones [OWIN](http://www.owin.org) deben ejecutarse con plena confianza.

Esta versión también incluye el paquete Microsoft.Owin.Diagnostics, que incluye middleware para validar una aplicación OWIN en ejecución, además de middleware de página de error para ayudar a investigar errores.

### <a name="authentication-components"></a>Componentes de autenticación

Los siguientes componentes de autenticación están disponibles.

- **Microsoft.Owin.Security.ActiveDirectory**. Habilita la autenticación mediante servicios de directorio locales o basados en la nube.
- **Microsoft.Owin.Security.Cookies** Habilita la autenticación mediante cookies. Este paquete se `Microsoft.Owin.Security.Forms`nombró anteriormente .
- **Microsoft.Owin.Security.Facebook** Habilita la autenticación mediante el servicio basado en OAuth de Facebook.
- **Microsoft.Owin.Security.Google** Habilita la autenticación mediante el servicio basado en OpenID de Google.
- **Microsoft.Owin.Security.Jwt** Habilita la autenticación mediante tokens JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** Habilita la autenticación mediante cuentas de Microsoft.
- **Microsoft.Owin.Security.OAuth**. Proporciona un servidor de autorización de OAuth, así como middleware para autenticar tokens portadores.
- **Microsoft.Owin.Security.Twitter** Habilita la autenticación mediante el servicio basado en OAuth de Twitter.

Esta versión `Microsoft.Owin.Cors` también incluye el paquete, que contiene middleware para procesar solicitudes HTTP entre orígenes.

> [!NOTE]
> La compatibilidad con la firma de JWT se ha quitado en la versión final de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Para obtener una lista de nuevas características y otros cambios en Entity Framework 6, vea Historial de [versiones](https://msdn.com/data/jj574253)de Entity Framework .

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 incluye las siguientes características nuevas:

- Soporte para edición de pestañas. Anteriormente, el comando Formato de **documento,** sangría automática y formato automático en Visual Studio no funcionaban correctamente al usar la opción **Mantener pestañas.** Este cambio corrige el formato de Visual Studio para el código Derazor para el formato de pestaña.
- Compatibilidad con reglas de reescritura de URL al generar vínculos.
- Eliminación del atributo transparente de seguridad.
  > [!NOTE]
  > Se trata de un cambio importante y hace que Razor 3 sea incompatible con MVC4 y anteriores, mientras que Razor 2 es incompatible con MVC5 o ensamblados compilados en MVC5.

Los problemas de Razor 3 corregidos en Visual Studio 2013 de las versiones preliminares se pueden encontrar [aquí](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET App Suspend

ASP.NET App Suspend es una característica que cambia el juego en .NET Framework 4.5.1 que cambia radicalmente la experiencia del usuario y el modelo económico para hospedar un gran número de sitios de ASP.NET en un solo equipo. Para obtener más información, vea [ASP.NET App Suspend : hospedaje web de .NET compartido con capacidad de respuesta](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

En esta sección se describen los problemas conocidos y los cambios importantes en el ASP.NET y Web Tools para Visual Studio 2013.

### <a name="nuget"></a>NuGet

- La nueva restauración de paquetes no funciona en Mono cuando se usa el [archivo SLN:](https://nuget.codeplex.com/workitem/3596) se corregirá en una próxima descarga de nuget.exe y en la actualización del [paquete NuGet.CommandLine.](http://www.nuget.org/packages/NuGet.CommandLine/)
- La nueva restauración de [paquetes no funciona con proyectos Wix:](https://nuget.codeplex.com/workitem/3598) se corregirá en una próxima descarga de nuget.exe y en la actualización del [paquete NuGet.CommandLine.](http://www.nuget.org/packages/NuGet.CommandLine/)
- La restauración automática de paquetes no funciona para proyectos en una carpeta de [solución:](https://nuget.codeplex.com/workitem/3625) se corregirá en NuGet 2.8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`no regresa `IQueryable<T>` siempre, ya que `$select` `$expand`agregamos soporte para y .

    Nuestras muestras `ODataQueryOptions<T>` anteriores para siempre `ApplyTo` `IQueryable<T>`se fundió el valor devuelto de a . Esto funcionó anteriormente porque las opciones`$filter`de `$orderby` `$skip`consulta `$top`que admitimos anteriormente ( , , , ) no cambian la forma de la consulta. Ahora que `$select` apoyamos y `$expand` `ApplyTo` el valor `IQueryable<T>` devuelto de no será siempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si está utilizando el código de ejemplo de anterior, seguirá `$select` `$expand`funcionando si el cliente no envía y . Sin embargo, si `$select` `$expand` desea admitir y tiene que cambiar ese código a esto.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url o RequestContext.Url es null durante una solicitud por lotes**

    En un escenario de procesamiento por lotes, **UrlHelper** es null cuando se tiene acceso desde **Request.Url** o **RequestContext.Url**.

    Actualmente se realiza un seguimiento de este problema aquí: [BatchRequestContext.Url es null para](http://aspnetwebstack.codeplex.com/workitem/1301)la solicitud de procesamiento por lotes.

    La solución alternativa para este problema es crear una nueva instancia de **UrlHelper**, como en el ejemplo siguiente:

    **Creación de una nueva instancia de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Al usar MVC5 y OrgAuth, si tiene vistas que realizan la validación de AntiForgerToken, puede encontrarse con el siguiente error al publicar datos en la vista:

    **Error**:

    *Error del servidor en la aplicación '/'*

    <em>Una reclamación<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>de tipo<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' ' o ' ' no estaba presente en el ClaimsIdentity proporcionado. Para habilitar la compatibilidad con tokens antifalsificación con la autenticación basada en notificaciones, compruebe que el proveedor de notificaciones configurado proporciona ambas notificaciones en las instancias ClaimsIdentity que genera. Si el proveedor de notificaciones configurado en su lugar usa un tipo de notificación diferente como un identificador único, se puede configurar estableciendo la propiedad estática AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Solución**:

    Agregue la siguiente línea en Global.asax para corregirlo:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Esto se corregirá para la próxima versión.
2. Después de actualizar una aplicación MVC4 a MVC5, compile la solución e iníciela. Debería ver el siguiente error:

    [A] System.Web.WebPages.Razor.Configuration.HostSection no se puede convertir a [B]System.Web.WebPages.Razor.Configuration.HostSection. El\_tipo A se origina en 'System.Web.WebPages.Razor, Versión 2.0.0.0, Culture,neutral, PublicKeyToken, 31bf3856ad364e35' en el contexto 'Predeterminado' en la ubicación 'C:'windows'Microsoft.Net', 'ensamblaje', GAC MSIL,System.Web.WebPages.Razor,v4.0\_2.0.0.0\_\_31bf3856ad364e35, System.Web.WebPages.Dll.dll.dll.. El tipo B se origina a partir de 'System.Web.WebPages.Razor, Versión 3.0.0.0, Culture-neutral, PublicKeyToken-31bf3856ad364e35' en el contexto 'Predeterminado' en la ubicación 'C:'Windows'Microsoft.NET'Framework'v4.0.30319'Archivos de ASP.NET temporales'root'6d05bbd0-e8b5908e-assembly-dl3-c9cbca63-f8910382\_6273ce01-System.Web.WebPages.Razor.dll'.

    Para corregir el error anterior, abra *todos los* archivos Web.config (incluidos los de la carpeta Vistas) del proyecto y haga lo siguiente:

   1. Actualice todas las apariciones de la versión "4.0.0.0" de "System.Web.Mvc" a "5.0.0.0".
   2. Actualice todas las apariciones de la versión "2.0.0.0" &quot;de "System.Web.Helpers", System.Web.WebPages&quot; y &quot;System.Web.WebPages.Razor&quot; a "3.0.0.0"

      Por ejemplo, después de realizar los cambios anteriores, los enlaces de ensamblado deben tener este aspecto:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Para obtener información sobre cómo actualizar proyectos MVC 4 a MVC 5, vea Cómo actualizar un ASP.NET MVC 4 y proyecto de [API web para ASP.NET MVC 5 y Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Cuando se utiliza la validación del lado cliente con jQuery Unobtrusive Validation, el mensaje de validación a veces es incorrecto para un elemento de entrada HTML con type 'number'. El error de validación de un valor obligatorio ("Se requiere el campo Edad") se muestra cuando se introduce un número no válido en lugar del mensaje correcto de que se requiere un número válido.

    Este problema se encuentra normalmente con código con scaffolding para un modelo con una propiedad de entero en las vistas Crear y Editar.

    Para evitar este problema, cambie la aplicación auxiliar del editor de:

    `@Html.EditorFor(person => person.Age)`

    Por:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ya no admite la confianza parcial. Los proyectos que se vinculan a los archivos binarios MVC o WebAPI deben quitar el atributo [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) y el atributo [AllowPartiallyTrustedCallers.](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) La eliminación de estos atributos eliminará los errores del compilador, como los siguientes.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Tenga en cuenta que, como efecto secundario de esto, no puede usar ensamblados 4.0 y 5.0 en la misma aplicación. Debe actualizar todos ellos a 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Plantilla SPA con autorización de Facebook puede causar inestabilidad en IE mientras que el sitio web está alojado en la zona de intranet

La plantilla SPA proporciona el inicio de sesión externo con Facebook. Cuando el proyecto creado con la plantilla se ejecuta localmente, iniciar sesión puede provocar que IE se bloquee.

Solución:

1. Alojar el sitio web en la zona de Internet; O

2. Pruebe el escenario en un explorador que no sea IE.

### <a name="web-forms-scaffolding"></a>Web Forms Scaffolding

El scaffolding de formularios Web Forms se ha quitado de VS2013 y estará disponible en una actualización futura de Visual Studio. Sin embargo, todavía puede usar scaffolding dentro de un proyecto de formularios Web Forms agregando dependencias MVC y generando scaffolding para MVC. El proyecto contendrá una combinación de formularios Web Forms y MVC.

Para agregar MVC al proyecto de formularios Web Forms, agregue un nuevo elemento con scaffolding y seleccione **MVC 5 Dependencias**. Seleccione Mínimo o Completo en función de si necesita todos los archivos de contenido, como scripts. A continuación, agregue un elemento con scaffolding para MVC, que creará vistas y un controlador en el proyecto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC y Web API Scaffolding - HTTP 404, Error no encontrado

Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quede en un estado incoherente. Algunos de los cambios realizados serán scaffolding se revertirán, pero otros cambios, como los paquetes NuGet instalados, no se revertirán. Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 al navegar a elementos con scaffolding.

Solución alternativa:

- Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccione MVC 5 Dependencias (mínimas o completas). Este proceso agregará todos los cambios necesarios al proyecto.
- Para corregir este error para Web API:

  1. Agregue la clase WebApiConfig al proyecto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configure WebApiConfig.Register en\_el método Application Start en Global.asax de la siguiente manera:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
