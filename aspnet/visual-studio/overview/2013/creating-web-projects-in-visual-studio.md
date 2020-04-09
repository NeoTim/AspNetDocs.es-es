---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Creación de ASP.NET proyectos web en Visual Studio 2013 Microsoft Docs
author: tdykstra
description: En este tema se explican las opciones para crear ASP.NET proyectos web en Visual Studio 2013 con Update 3 Aquí se encuentran algunas de las nuevas características para el desarrollo web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675686"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Crear proyectos web de ASP.NET en Visual Studio 2013

por [Tom Dykstra](https://github.com/tdykstra)

> En este tema se explican las opciones para crear proyectos web de ASP.NET en Visual Studio 2013 con Update 3
> 
> Estas son algunas de las nuevas características para el desarrollo web en comparación con versiones anteriores de Visual Studio:
> 
> - Una interfaz de usuario sencilla para crear proyectos que ofrecen [compatibilidad con varios marcos de ASP.NET](#add) (formularios web, MVC y WEB API).
> - [ASP.NET Identity](#indauth), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de ASP.NET y funciona con software de hospedaje web que no sea IIS.
> - El uso de [Bootstrap](#bootstrap) para proporcionar capacidades de diseño y temas responsivos.
> - Nuevas características para formularios Web Forms que solían ofrecerse solo para MVC, como la [creación automática](#testproj) de proyectos de prueba y una plantilla de sitio [Intranet.](#winauth)
> 
> Para obtener información sobre cómo crear proyectos web para Azure Cloud Services o Azure Mobile Services, consulte [Introducción](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) a Azure Cloud Services y ASP.NET y Creación de una aplicación de tabla de clasificación con el [back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)de .NET de Azure Mobile Services .

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerrequisitos

Este artículo se aplica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalada.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Proyectos de aplicación web frente a proyectos de sitios web

ASP.NET le da la opción de elegir entre dos tipos de proyectos web: proyectos de *aplicación web* y proyectos de *sitio web.* Se recomiendan proyectos de aplicación web para nuevo desarrollo y este artículo solo se aplica a los proyectos de aplicación web. Para obtener más información, vea Proyectos de aplicación web frente a proyectos de [sitio web en Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) en el sitio MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Descripción general de la creación de proyectos de aplicaciones web

Los pasos siguientes muestran cómo crear un proyecto web:

1. Haga clic en **Nuevo proyecto** en la página **Inicio** o en el menú **Archivo.**
2. En el cuadro de diálogo **Nuevo proyecto,** haga clic en **Web** en el panel izquierdo y **ASP.NET aplicación web** en el panel central.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image1.png)

    Puede elegir **Nube** en el panel izquierdo para crear un [Azure Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), Azure Mobile [Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)o [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). En este tema no se tratan esas plantillas.
3. En el panel derecho, haga clic en la casilla **Agregar Application Insights al proyecto** si desea la supervisión del estado y el uso de la aplicación. Para más información, consulte [Supervisar el rendimiento de aplicaciones web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especifique **Nombre del**proyecto , **Ubicación**y otras opciones y, a continuación, haga clic en **Aceptar**.

    Aparece el cuadro de diálogo **Nuevo proyecto ASP.NET**.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Haga clic en una plantilla.

    ![Seleccione una plantilla:](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si desea agregar compatibilidad con marcos adicionales no incluidos en la plantilla, haga clic en la casilla de verificación adecuada. (En el ejemplo que se muestra, podría agregar MVC o Web API a un proyecto de formularios Web Forms.)

    ![Añadir marcos](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si desea agregar un proyecto de prueba unitaria, haga clic en **Agregar pruebas unitarias**.

    ![Adición de pruebas unitarias](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si desea un método de autenticación diferente al que proporciona la plantilla de forma predeterminada, haga clic en **Cambiar autenticación**.

    ![Configurar botón de autenticación](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurar el cuadro de diálogo de autenticación](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Cree una aplicación web o una máquina virtual en Azure

Visual Studio incluye características que facilitan el trabajo con los servicios de Azure para hospedar aplicaciones web. Por ejemplo, puede hacer todo lo siguiente directamente desde el IDE de Visual Studio:

- Cree y administre aplicaciones web o máquinas virtuales que hagan que la aplicación esté disponible a través de Internet.
- Vea los registros creados por la aplicación mientras se ejecuta en la nube.
- Ejecutar en modo de depuración de forma remota mientras la aplicación se ejecuta en la nube.
- Vea y administre otros servicios de Azure, como bases de datos SQL.

Puede [crear una cuenta](https://www.windowsazure.com/pricing/free-trial/) de Azure que incluya servicios básicos, como aplicaciones web de forma gratuita, y si es suscriptor de MSDN, puede [activar las ventajas](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que le proporcionan créditos mensuales para servicios de Azure adicionales. 

De forma **predeterminada,** el cuadro de diálogo Nuevo proyecto de ASP.NET permite crear una aplicación web o una máquina virtual para un nuevo proyecto web. Si no desea crear una nueva aplicación web o máquina virtual, desactive la casilla **Host in the cloud (Host en la nube).**

![Crear recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

El título de la casilla de verificación puede ser **Host en la nube** o Crear recursos **remotos**y, en cualquier caso, el efecto es el mismo. Si deja activada la casilla de verificación, Visual Studio crea una aplicación web en Azure App Service de forma predeterminada. Puede usar el cuadro desplegable para cambiarlo a una **máquina virtual** si lo prefiere. Si aún no ha iniciado sesión en Azure, se le pedirán las credenciales de Azure. Después de iniciar sesión, un cuadro de diálogo le permite configurar los recursos que Visual Studio creará para el proyecto. En la siguiente ilustración se muestra el cuadro de diálogo de una aplicación web; aparecen diferentes opciones si decide crear una máquina virtual.

![Configurar la configuración de la aplicación Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Para obtener más información acerca de cómo usar este proceso para crear recursos de Azure, vea [Introducción](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) a Azure y ASP.NET y [Creación de una máquina virtual para un sitio web con Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

El resto de este artículo proporciona más información sobre las plantillas disponibles y sus opciones. El artículo también presenta Bootstrap, el diseño y el marco de trabajo de temas utilizados en las plantillas.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Plantillas de proyecto web de Visual Studio 2013

Visual Studio usa plantillas para crear proyectos web. Una plantilla de proyecto puede crear archivos y carpetas en el nuevo proyecto, instalar paquetes NuGet y proporcionar código de ejemplo para una aplicación de trabajo rudimentaria. Las plantillas implementan los estándares web más recientes y están diseñadas para demostrar las prácticas recomendadas para usar tecnologías ASP.NET, así como para darle un salto en la creación de su propia aplicación.

Visual Studio 2013 proporciona las siguientes opciones para plantillas de proyecto web para proyectos destinados a .NET 4.5 o versiones posteriores de .NET Framework:

- [Plantilla vacía](#empty)
- [Plantilla de formularios Web Forms](#wf)
- [Plantilla de MVC](#mvc)
- [Plantilla de API web](#webapi)
- [Plantilla de aplicación de una sola página](#spa)
- [Plantilla de Azure Mobile Service](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Plantillas de Visual Studio 2012](#vs2012)

También puede instalar una extensión de Visual Studio que proporciona una plantilla de [Facebook.](#facebook)

Para obtener información acerca de cómo crear proyectos destinados a .NET 4, vea Plantillas de [Visual Studio 2012](#vs2012) más adelante en este tema.

Para obtener información sobre cómo crear aplicaciones ASP.NET para clientes móviles, consulte [Soporte móvil en ASP.NET](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Plantilla vacía

La plantilla Vacío proporciona las carpetas y archivos mínimos para una aplicación web ASP.NET, como un archivo de proyecto (*.csproj* o .* vbproj*) y un archivo *Web.config.* Puede agregar compatibilidad con formularios Web Forms, MVC o Web API mediante las casillas de verificación de la etiqueta **Agregar carpetas y referencias principales para:** .

Para la plantilla vacía no hay opciones de autenticación disponibles. La funcionalidad de autenticación se implementa en aplicaciones de ejemplo y la plantilla Vacía no crea una aplicación de ejemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Plantilla de formularios Web Forms

El marco de trabajo de formularios Web Forms proporciona las siguientes características que le permiten crear rápidamente sitios web que son ricos en características de acceso a datos y interfaz de usuario:

- Un diseñador WYSIWYG en Visual Studio.
- Controles de servidor que representan HTML y que puede personalizar estableciendo propiedades y estilos.
- Una amplia variedad de controles para el acceso a datos y la visualización de datos.
- Un modelo de eventos que expone eventos que puede programar como programaría una aplicación cliente como WPFWPF.
- Conservación automática del estado (datos) entre solicitudes HTTP.

En general, la creación de una aplicación de formularios Web Forms requiere menos esfuerzo de programación que la creación de la misma aplicación mediante el marco de ASP.NET MVC. Sin embargo, los formularios Web Forms no son solo para el desarrollo rápido de aplicaciones. Hay muchas aplicaciones comerciales complejas y marcos construidos sobre formularios Web Forms.

Dado que una página de formularios Web Forms y los controles de la página generan automáticamente gran parte del marcado que se envía al explorador, no tiene el tipo de control detallado sobre el HTML que ofrece ASP.NET MVC. El modelo declarativo para configurar páginas y controles minimiza la cantidad de código que tiene que escribir, pero oculta parte del comportamiento de HTML y HTTP. Por ejemplo, no siempre es posible especificar exactamente qué marcado podría generar un control.

El marco de trabajo de formularios Web Forms no se presta tan fácilmente como ASP.NET MVC a las prácticas de desarrollo basadas en patrones, como el [desarrollo basado en pruebas,](http://en.wikipedia.org/wiki/Test-driven_development)la [separación de preocupaciones,](http://en.wikipedia.org/wiki/Separation_of_concerns)la [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control)y la [inserción](http://en.wikipedia.org/wiki/Dependency_injection)de dependencias. Si desea escribir código factorizado de esa manera, puede; no es tan automático como lo es en el marco de ASP.NET MVC. El proyecto [MVP de ASP.NET formularios Web Forms](http://webformsmvp.com/) muestra un enfoque que facilita la separación de preocupaciones y la capacidad de prueba, al tiempo que mantiene el rápido desarrollo que se creó para ofrecer formularios Web Forms. Microsoft SharePoint se basa en Web Forms MVP.

La plantilla de formularios Web Forms crea una aplicación de formularios Web Forms de ejemplo que usa [Bootstrap](#bootstrap) para proporcionar características de diseño y temas adaptables. En la siguiente ilustración se muestra la página de inicio.

![Página de inicio de la aplicación de plantilla de formularios Web Forms](creating-web-projects-in-visual-studio/_static/image10.png)

Para obtener más información acerca de los formularios Web Forms, vea [ASP.NET formularios Web Forms](https://asp.net/web-forms). Para obtener información acerca de lo que la plantilla de formularios Web Forms hace por usted, vea Crear una aplicación básica de [formularios Web Forms mediante Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Plantilla MVC

ASP.NET MVC fue diseñado para facilitar las prácticas de desarrollo basadas en patrones, como el [desarrollo basado en pruebas,](http://en.wikipedia.org/wiki/Test-driven_development)la [separación de preocupaciones,](http://en.wikipedia.org/wiki/Separation_of_concerns)la [inversión del control](http://en.wikipedia.org/wiki/Inversion_of_control)y la [inserción](http://en.wikipedia.org/wiki/Dependency_injection)de dependencias. El marco de trabajo fomenta la separación de la capa de lógica empresarial de una aplicación web de su capa de presentación. Al dividir la aplicación en modelos (M), vistas (V) y controladores (C), ASP.NET MVC puede facilitar la administración de la complejidad en aplicaciones más grandes.

Con ASP.NET MVC, trabajará más directamente con HTML y HTTP que en formularios Web Forms. Por ejemplo, los formularios Web Forms pueden conservar automáticamente el estado entre las solicitudes HTTP, pero tiene que codificar explícitamente en MVC. La ventaja del modelo MVC es que le permite tomar el control completo sobre exactamente lo que la aplicación está haciendo y cómo se comporta en el entorno web. La desventaja es que tienes que escribir más código.

MVC se diseñó para ser extensible, proporcionando a los desarrolladores de energía la capacidad de personalizar el marco de trabajo para sus necesidades de aplicación. Además, el código fuente de ASP.NET MVC está disponible bajo una licencia OSI.

La plantilla MVC crea una aplicación MVC 5 de ejemplo que usa [Bootstrap](#bootstrap) para proporcionar características de diseño y temas adaptables. En la siguiente ilustración se muestra la página de inicio.

![Aplicación de ejemplo MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obtener más información acerca de MVC, vea [ASP.NET MVC](https://asp.net/mvc). Para obtener información acerca de cómo seleccionar la plantilla MVC 4, vea Plantillas de [Visual Studio 2012](#vs2012) más adelante en este artículo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Plantilla de API web

La plantilla de API web crea un servicio web de ejemplo basado en la API web, incluidas las páginas de ayuda de la API basadas en MVC.

ASP.NET Web API es un marco que facilita la creación de servicios HTTP disponibles para una amplia variedad de clientes, entre los que se incluyen exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para crear servicios RESTful en .NET Framework.

La plantilla de API web crea un servicio web de ejemplo. Las siguientes ilustraciones muestran páginas de ayuda de ejemplo.

![Página de ayuda de la API web](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de ayuda de la API web para GET API](creating-web-projects-in-visual-studio/_static/image13.png)

Para obtener más información acerca de Web API, consulte [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Plantilla de aplicación de página única

La plantilla Aplicación de página única (SPA) crea una aplicación de ejemplo que utiliza JavaScript, HTML 5 y [KnockoutJS](http://knockoutjs.com/) en el cliente y ASP.NET Web API en el servidor.

La única opción de autenticación para la plantilla SPA son las cuentas de [usuario individuales.](#indauth)

La ilustración siguiente muestra el estado inicial de la aplicación de ejemplo que compila la plantilla SPA.

![Aplicación de muestra SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obtener información sobre cómo crear una aplicación mediante la plantilla SPA, consulte [Web API - External Authentication Services](../../../web-api/overview/security/external-authentication-services.md).

Para obtener más información acerca de ASP.NET aplicaciones de una sola página y sobre plantillas SPA adicionales que utilizan marcos de JavaScript distintos de KnockoutJS, consulte los siguientes recursos:

- [ASP.NET Aplicación de una sola página](../../../single-page-application/index.md).
- [Comprensión de las características de seguridad en la plantilla SPA para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplicaciones de una sola página: cree aplicaciones web modernas y adaptables con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Plantilla de Facebook

Puede instalar una extensión de Visual Studio que proporcione una plantilla de [Facebook.](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409) Esta plantilla crea una aplicación de ejemplo que está diseñada para ejecutarse dentro del sitio web de Facebook. Se basa en ASP.NET MVC y usa Web API para la funcionalidad de actualización en tiempo real.

No hay opciones de autenticación disponibles para la plantilla de Facebook porque las aplicaciones de Facebook se ejecutan en el sitio de Facebook y dependen de la autenticación de Facebook.

Para obtener más información sobre ASP.NET aplicaciones de Facebook, consulte [Actualización de la API](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx)de Facebook MVC .

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Plantillas de Visual Studio 2012

El cuadro de diálogo de creación de proyectos web de Visual Studio 2013 no proporciona acceso a algunas plantillas que estaban disponibles en Visual Studio 2012. Si desea usar una de estas plantillas, puede hacer clic en el visual Studio 2012 nodo en el panel izquierdo de la Visual Studio nuevo proyecto cuadro de diálogo.

![Plantillas de Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

El nodo **Visual Studio 2012** le permite elegir las siguientes plantillas web a las que no tiene acceso en la lista predeterminada de plantillas para Visual Studio 2013:

- Aplicación web MVC 4 de ASP.NET
- Aplicación web de Entidades de datos dinámicos de ASP.NET
- ASP.NET AJAX Server Control
- ASP.NET AJAX Server Control Extender
- Control de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap en las plantillas de proyecto web de Visual Studio 2013

Las plantillas de proyecto de Visual Studio 2013 usan [Bootstrap](http://getbootstrap.com/), un diseño y marco de trabajo de temas creado por Twitter. Bootstrap utiliza CSS3 para proporcionar un diseño responsivo, lo que significa que los diseños pueden adaptarse dinámicamente a diferentes tamaños de ventana del navegador. Por ejemplo, en una ventana de explorador amplia, la página principal creada por la plantilla de formularios Web Forms tiene el siguiente aspecto:

![Página de inicio de la aplicación de plantilla de formularios Web Forms](creating-web-projects-in-visual-studio/_static/image16.png)

Haga que la ventana sea más estrecha y las columnas dispuestas horizontalmente se muevan a disposición vertical:

![Disposición de columna vertical de arranque](creating-web-projects-in-visual-studio/_static/image17.png)

Acotar la ventana un poco más, y el menú superior horizontal se convierte en un icono que se puede hacer clic para expandiren en un menú orientado verticalmente:

![Icono del menú Bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menú vertical Bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

También puede utilizar la función de temas de Bootstrap para realizar fácilmente un cambio en el aspecto de la aplicación. Por ejemplo, puede realizar los siguientes pasos para cambiar el tema.

1. En el explorador, [http://Bootswatch.com](http://Bootswatch.com)vaya a , elija un tema y, a continuación, haga clic en **Descargar**. (Esto descarga *bootstrap.min.css* de forma predeterminada; si desea examinar el código CSS, obtenga *bootstrap.css* en lugar de la versión minimizada.)
2. Copie el contenido del archivo CSS descargado.
3. En Visual Studio, cree un nuevo archivo de **hoja** de estilos denominado *bootstrap-theme.css* en la carpeta *Content* y pegue el código CSS descargado en él.
4. Abra *\_App Start/Bundle.config* y cambie *bootstrap.css* a *bootstrap-theme.css*.

Ejecute el proyecto de nuevo y la aplicación tiene un nuevo aspecto. La siguiente ilustración muestra el efecto del tema Amelia:

![Tema Bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Muchos temas de Bootstrap están disponibles, tanto las versiones gratuitas como las premium. Bootstrap también ofrece una amplia variedad de componentes de interfaz de usuario, como [menús desplegables,](http://twitter.github.io/bootstrap/components.html#dropdowns)grupos de [botones](http://twitter.github.io/bootstrap/components.html#buttonGroups)e [iconos.](http://twitter.github.io/bootstrap/base-css.html#images) Para obtener más información acerca de Bootstrap, consulte [el sitio de Bootstrap](http://twitter.github.io/bootstrap/).

Si usa el diseñador de formularios Web Forms en Visual Studio, tenga en cuenta que el diseñador no admite CSS3, por lo que no muestra con precisión todos los efectos de los temas de Bootstrap o los cambios de diseño adaptables. Sin embargo, las páginas de formularios Web Forms se mostrarán correctamente cuando se vean con un explorador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Adición de compatibilidad con marcos adicionales

Al seleccionar una plantilla, la casilla de verificación de los marcos utilizados por la plantilla se selecciona automáticamente. Por ejemplo, si selecciona la plantilla **de formularios Web Forms,** la casilla de verificación **Formularios Web** Forms está activada y no puede borrarla.

![Seleccione una plantilla:](creating-web-projects-in-visual-studio/_static/image21.png)

![Añadir marcos](creating-web-projects-in-visual-studio/_static/image22.png)

Puede activar la casilla de verificación de un marco de trabajo que no se incluye en la plantilla para agregar compatibilidad con ese marco cuando se crea el proyecto. Por ejemplo, para habilitar el uso de páginas *.aspx* de formularios Web Forms cuando haya seleccionado la plantilla MVC, active la casilla de verificación **Formularios Web Forms.** O bien, para habilitar MVC cuando use la plantilla de formularios Web Forms, haga clic en la casilla **MVC.** Agregar un marco de trabajo permite el tiempo de diseño, así como la compatibilidad en tiempo de ejecución. Por ejemplo, si agrega compatibilidad con MVC a un proyecto de formularios Web Forms, podrá aplicar scaffolding a los controladores y vistas.

Si combina formularios Web Forms y MVC en un proyecto y habilita [direcciones URL descriptivas](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) en formularios Web Forms, puede haber problemas de enrutamiento inesperados donde una dirección URL tiene varios destinos posibles. Las rutas que se definen primero tendrán prioridad. Por ejemplo, si `Home` tiene un controlador y una `http://contoso.com/home` página *Home.aspx,* la dirección `EnableFriendlyUrls` URL irá `MapRoute` a *Home.aspx* si llama al método antes de llamar `MapRoute` al `EnableFriendlyUrls`método en *RouteConfig.cs*, o la misma dirección URL irá a la vista predeterminada del `Home` controlador si llama antes.

Agregar un marco de trabajo no agrega ninguna funcionalidad de aplicación de ejemplo. Por ejemplo, si agrega compatibilidad con formularios Web Forms cuando ha seleccionado la plantilla MVC, no se crea ningún archivo de página principal *Default.aspx.* Solo se agregan las carpetas, archivos y referencias necesarios para admitir el marco de trabajo. Por este motivo, agregar marcos de trabajo no cambia las opciones de autenticación, que se implementan mediante código en aplicaciones de ejemplo creadas por las plantillas. Por ejemplo, si selecciona la plantilla vacía y agrega compatibilidad con formularios Web Forms o MVC, el botón **Configurar autenticación** seguirá deshabilitado.

En las secciones siguientes se describe brevemente el efecto de cada casilla de verificación.

### <a name="add-web-forms-support"></a>Agregar compatibilidad con formularios Web Forms

Crea carpetas vacías de datos de *aplicación\_* y *modelos* y un archivo *Global.asax.* Estas ya están creadas por todas las plantillas que no sean la plantilla vacía, por lo que al seleccionar la casilla de verificación Formularios Web Forms no hay ninguna diferencia para otras plantillas.

La plantilla de formularios Web Forms habilita las direcciones URL descriptivas de forma predeterminada, pero al agregar compatibilidad con formularios Web Forms a otras plantillas seleccionando la casilla de verificación Direcciones URL descriptivas de formularios Web Forms no se habilitan automáticamente.

### <a name="add-mvc-support"></a>Agregar compatibilidad con MVC

Instala paquetes NuGet MVC, Razor y WebPages, crea carpetas vacías de *Datos\_* de aplicación , *Controladores*, *Modelos*y *Vistas,* crea la carpeta Inicio de la *aplicación\_* con *RouteConfig.cs* archivo y crea el archivo *Global.asax.*

### <a name="add-web-api-support"></a>Agregar compatibilidad con Web API

Instala paquetes NuGet WebApi y Newtonsoft.Json, crea carpetas vacías de *\_App Data*, *Controllers*y *Models,* crea una carpeta *App\_Start* con *WebApiConfig.cs* archivo y crea el archivo *Global.asax.*

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticación

Visual Studio 2013 ofrece varias opciones de autenticación para las plantillas de formularios Web Forms, MVC y Web API:

- [Sin autenticación](#noauth)
- [Cuentas](#indauth) de usuario individuales (ASP.NET Identidad, anteriormente conocida como membresía ASP.NET)
- [Cuentas organizativas](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [Autenticación de Windows](#winauth) (Intranet)

![Configurar el cuadro de diálogo de autenticación](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sin autenticación

Si selecciona **Sin autenticación**, la aplicación de ejemplo no contendrá ninguna página web para iniciar sesión, ninguna interfaz de usuario que indique quién ha iniciado sesión, ninguna clase de entidad para una base de datos de pertenencia y ninguna cadena de conexión para una base de datos de pertenencia.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Cuentas de usuario individuales

Si selecciona Cuentas de **usuario individuales**, la aplicación de ejemplo se configurará para usar ASP.NET identidad (anteriormente conocida como pertenencia a ASP.NET) para la autenticación de usuario. ASP.NET Identity permite a un usuario registrar una cuenta, creando un nombre de usuario y una contraseña en el sitio o iniciando sesión con proveedores sociales como Facebook, Google, Microsoft Account o Twitter. El almacén de datos predeterminado para los perfiles de usuario en ASP.NET Identity es una base de datos localde SQL Server, que puede implementar en SQL ServerSQL Server o Azure SQL Database para el sitio de producción.

En Visual Studio 2013 estas características son las mismas que en Visual Studio 2012, pero se ha reescrito el código subyacente para el sistema de pertenencia ASP.NET. Entre las ventajas de la nueva base de código se incluyen las siguientes:

- El nuevo sistema de pertenencia se basa en [OWIN](http://owin.org/) en lugar del módulo autenticación de formularios ASP.NET. Esto significa que puede usar el mismo mecanismo de autenticación, ya sea que use formularios Web Forms o MVC en IIS, o que esté hospedando auto-auto Web API o SignalR.
- Entity Framework Code First administra la nueva base de datos de pertenencia y todas las tablas están representadas por clases de entidad que puede modificar. Esto significa que puede personalizar fácilmente el esquema de base de datos y la interfaz de usuario web relacionada con el perfil para que se ajuste a sus propias necesidades, y puede implementar fácilmente las actualizaciones mediante migraciones de Code First.

El nuevo sistema de pertenencia se implementa automáticamente en las nuevas plantillas y se puede implementar manualmente en cualquier proyecto destinado a .NET 4.5 o posterior.

ASP.NET Identity es una buena opción si está creando un sitio web de Internet que es principalmente para clientes externos. Si su organización usa Active Directory u Office 365 y desea crear un proyecto que habilite el inicio de sesión único para empleados y socios comerciales, la opción **Cuentas organizativas** podría ser una mejor opción.

Para obtener más información acerca de la opción Cuentas de usuario individuales, consulte los siguientes recursos:

- [www.asp.net/identity](../../../identity/index.md). Documentación sobre ASP.NET Identidad en el sitio web ASP.NET.
- [Cree una aplicación ASP.NET MVC 5 con Facebook y Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). También muestra cómo personalizar los datos de perfil de usuario.
- [API web - Servicios de autenticación externa](../../../web-api/overview/security/external-authentication-services.md)
- [Agregar inicios de sesión externos a la aplicación ASP.NET en Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Cuentas organizativas

Si selecciona **Cuentas organizativas**, la aplicación de ejemplo se configurará para usar Windows Identity Foundation (WIF) para la autenticación basada en cuentas de usuario en Azure Active Directory (Azure AD, que incluye Office 365) o Windows Server Active Directory. Para obtener más información, consulte Opciones de autenticación de [cuenta](#orgauthoptions) safines más adelante en este tema.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticación de Windows

Si selecciona Autenticación de **Windows**, la aplicación de ejemplo se configurará para usar el módulo IIS de autenticación de Windows para la autenticación. La aplicación mostrará el dominio y el identificador de usuario de la cuenta de active directory o de equipo local que ha iniciado sesión en Windows, pero no incluirá el registro de usuarios ni la interfaz de usuario de inicio de sesión. Esta opción está destinada a sitios web de Intranet.

Como alternativa, puede crear un sitio de Intranet que use la autenticación de AD eligiendo la [opción Local en Cuentas organizativas](#orgauthonprem). La opción Local usa Windows Identity Foundation (WIF) en lugar del módulo Autenticación de Windows. Se requieren algunos pasos adicionales para configurar la opción local, pero WIF habilita las características que no están disponibles con el módulo de autenticación de Windows. Por ejemplo, con WIF puede configurar el acceso a la aplicación en Active Directory y consultar los datos del directorio.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opciones de autenticación de cuentas organizativas

El cuadro de diálogo **Configurar autenticación** ofrece varias opciones para la autenticación de cuenta de Azure Active Directory (Azure AD, que incluye Office 365) o Windows Server Active Directory (AD):

- [Nube: organización única](#orgauthsingle) (Azure AD o AD mediante la integración de directorios con Azure AD)
- [Nube: organización múltiple](#orgauthmulti) (Azure AD o AD mediante la integración de directorios con Azure AD)
- [Local](#orgauthonprem) (AD)

Si desea probar una de las opciones de Azure AD pero aún no tiene una cuenta, [haga clic aquí para registrarse en una cuenta](https://go.microsoft.com/fwlink/?LinkId=309942)de Azure AD.

> [!NOTE]
> Si elige una de las opciones de Azure AD, el proyecto requiere una base de datos y debe iniciar sesión en una cuenta de administrador global para el inquilino de Azure AD. Escriba el nombre y la contraseña de admin@contoso.onmicrosoft.comuna cuenta de organización (por ejemplo, ) que tenga permisos administrativos para el inquilino de Azure AD.
> 
> **No escriba las credenciales de una cuenta contoso@hotmail.comMicrosoft (por ejemplo, ) en el cuadro de diálogo de inicio de sesión.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Nube - Autenticación de organización única

![Autenticación de organización única](creating-web-projects-in-visual-studio/_static/image24.png)

Elija esta opción si desea habilitar la autenticación para las cuentas de usuario definidas en un [inquilino](https://technet.microsoft.com/library/jj573650.aspx)de Azure AD. Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la empresa Contoso que están en el inquilino de contoso.onmicrosoft.com. No podrá configurar Azure AD para permitir que los usuarios de otros inquilinos accedan a la aplicación.

#### <a name="domain"></a>Domain

Escriba el dominio de Azure AD en el que `contoso.onmicrosoft.com`desea configurar la aplicación, por ejemplo: . Si tiene un [dominio personalizado,](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/) `contoso.onmicrosoft.com`como , por ejemplo, `contoso.com` puede escribirlo aquí.

#### <a name="access-level"></a>Nivel de acceso

Si la aplicación necesita consultar o actualizar la información del directorio mediante Graph API, elija **Single Sign-On, Read Directory Data** o Single **Sign-On, Read and Write Directory Data**. De lo contrario, elija **Inicio de sesión único**. Para obtener más información, consulte Niveles de [acceso a](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) aplicaciones y Uso de Graph API para consultar [Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI de Id. de aplicación

De forma predeterminada, la plantilla crea un URI de identificador de aplicación anexando el nombre del proyecto al dominio de Azure AD. Por ejemplo, si el `Example` nombre del `contoso.onmicrosoft.com`proyecto es y `https://contoso.onmicrosoft.com/Example`el dominio es , el URI del identificador de aplicación se convierte en . Si desea especificar manualmente el URI de identificador de aplicación, expanda la sección **Más opciones** e introduzca el URI de identificador de aplicación en el cuadro de texto. El URI de identificador `https://`de aplicación debe comenzar por .

De forma predeterminada, si una aplicación que ya está aprovisionada en Azure AD tiene el mismo URI de identificador de aplicación que el que Visual Studio usa para el proyecto, el proyecto se conectará a la aplicación existente en lugar de aprovisionar una nueva. Si desea que se aprovisione una nueva aplicación en ese caso, desactive la casilla Sobrescribir la entrada de **aplicación si ya existe una con el mismo identificador.**

Si la casilla **Sobrescribir** está desactivada y Visual Studio encuentra una aplicación existente con el mismo URI de identificador de aplicación, crea un nuevo URI anexando un número al URI que iba a usar. Por ejemplo, supongamos `Example`que el nombre del proyecto es , deja el cuadro de texto en blanco, desactiva la casilla **Sobrescribir** y el inquilino de Azure AD ya tiene una aplicación con el URI `https://contoso.onmicrosoft.com/Example`. En ese caso, se aprovisionará una nueva aplicación `https://contoso.onmicrosoft.com/Example_20130619330903`con un URI de identificador de aplicación como .

#### <a name="provisioning-the-application-in-azure-ad"></a>Aprovisionamiento de la aplicación en Azure AD

Para aprovisionar la aplicación en Azure AD o conectar el proyecto a una aplicación existente, Visual Studio necesita las credenciales de un administrador global para el dominio. Al hacer clic en **Aceptar** en el cuadro de diálogo **Configurar autenticación,** se le pedirá el nombre de usuario y la contraseña de un administrador global para el dominio que especificó. Más adelante, al hacer clic en **Crear proyecto** en el cuadro de diálogo Nuevo **ASP.NET proyecto,** Visual Studio aprovisiona la aplicación en Azure AD. Tenga en cuenta que como parte de este proceso Visual Studio incrusta valores secretos de cliente en el archivo Web.config que expiran un año después de la creación.

Para obtener información acerca de cómo crear aplicaciones que utilizan la autenticación en la nube **- organización única,** consulte los siguientes recursos:

- [Autenticación de Azure](../2012/windows-azure-authentication.md)
- [Incorporación del inicio de sesión a la aplicación web utilizando Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desarrollo de aplicaciones ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteja ASP.NET Web API con Azure AD y Microsoft OWIN Components](https://msdn.microsoft.com/magazine/dn463788.aspx)

Los tutoriales aún no se han actualizado para Visual Studio 2013; algunos de lo que los tutoriales le dirigen a hacer manualmente se automatiza en Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Nube - Autenticación multiorganización

![Autenticación de varias organizaciones](creating-web-projects-in-visual-studio/_static/image25.png)

Elija esta opción si desea habilitar la autenticación para las cuentas de usuario definidas en varios [inquilinos](https://technet.microsoft.com/library/jj573650.aspx)de Azure AD. Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la empresa Contoso que están en el inquilino de contoso.onmicrosoft.com y los empleados de la empresa Fabrikam que están en el inquilino fabrikam.onmicrosoft.com.

La configuración que especifique y el paso de aprovisionamiento de aplicaciones son similares a la autenticación de [una sola organización.](#orgauthsingle)

Para obtener información acerca de cómo crear aplicaciones que utilizan la autenticación **cloud - Multi Organization,** consulte los siguientes recursos:

- [Fácil integración de aplicaciones web &amp; con Azure Active Directory, ASP.NET Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) en el blog del equipo de Active Directory.
- [Desarrollo de aplicaciones web multiinquilino con el](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial de Azure AD. El tutorial aún no se ha actualizado para Visual Studio 2013; parte de lo que el tutorial le indica que haga manualmente se automatiza en Visual Studio 2013.
- [Usted tiene que registrarse con sus propias organizaciones ASP.NET aplicación antes](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)de que pueda iniciar sesión . Blog de Vittorio Bertocci que explica cómo resolver un problema común que las personas encuentran al crear un proyecto que utiliza la autenticación multiorganización.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticación organizativa local

![Autenticación organizativa local](creating-web-projects-in-visual-studio/_static/image26.png)

Elija esta opción si desea habilitar la autenticación para las cuentas de usuario definidas en Windows Server Active Directory (AD) y no desea usar Azure AD. Puede usar esta opción para crear un sitio de Intranet o un sitio de Internet. Para un sitio de Internet, use Servicios de federación de Active Directory (ADFS) para proporcionar acceso a AD. Para obtener más información, vea Usar la opción de [autenticación organizativa local (ADFS) con ASP.NET en Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para un sitio de Intranet, como alternativa puede elegir Autenticación de [Windows](#winauth) en lugar de esta opción. Para la opción Autenticación de Windows no es que proporcione una dirección URL del documento de metadatos. Sin embargo, la autenticación de Windows no le ofrece la capacidad de controlar el acceso a la aplicación en Active Directory ni de consultar datos de directorio.

#### <a name="on-premises-authority"></a>Autoridad local

Escriba una dirección URL que apunte al documento de metadatos. El documento de metadatos contiene las coordenadas de la autoridad. La aplicación utilizará esas coordenadas para impulsar el flujo de inicio de sesión web.

#### <a name="application-id-uri"></a>URI de Id. de aplicación

Proporcione un URI único que AD puede usar para identificar esta aplicación o deje en blanco para permitir que Visual Studio cree uno.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Este documento ha proporcionado ayuda básica para crear un nuevo proyecto web ASP.NET en Visual Studio 2013. Para obtener más información sobre el uso [https://www.asp.net/visual-studio/](../../index.md)de Visual Studio para el desarrollo web, vea .
