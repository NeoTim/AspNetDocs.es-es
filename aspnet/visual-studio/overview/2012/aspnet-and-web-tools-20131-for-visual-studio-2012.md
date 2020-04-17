---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de la versión para ASP.NET y Web Tools 2013.1 para Visual Studio 2012 Microsoft Docs
author: rick-anderson
description: Este documento describe la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d4aced4e77a150d52358c2d2513ff79e6594a9de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543579"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Notas de la versión de ASP.NET and Web Tools 2013.1 para Visual Studio 2012

por [Microsoft](https://github.com/microsoft)

> Este documento describe la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012.

## <a name="contents"></a>Contenido

- [Notas de instalación](#install)
- [Requisitos de software](#requirements)
- Nuevas características en ASP.NET y Web Tools 2013.1 para Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Templates](#templates) (Plantillas [C++])

        - [ASP.NET MVC 5 plantilla](#mvc5template)
        - [ASP.NET plantilla de Web API 2](#apitemplate)
        - [Plantillas de elementos](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [andamios ASP.NET](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Problemas conocidos y cambios importantes

    - [andamios ASP.NET](#issuescaffolding)

        - [MVC y Web API Scaffolding - HTTP 404, Error no encontrado](#404issue)
        - [Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Ver el archivo cshtml con Browse With o F5 provoca un error de servidor](#browseissue)
        - [Url Rewrite y Tilde(')](#rewriteissue)
    - [Templates](#templateissue) (Plantillas [C++])

<a id="install"></a>
## <a name="installation-notes"></a>Notas de la instalación

[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET y Web Tools 2013.1 para Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nuevas características en ASP.NET y Web Tools 2013.1 para Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Al aplicar scaffolding MVC 5 controladores y vistas, el marcado de las vistas usa [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Plantillas

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 plantilla

Agregamos una nueva plantilla MVC 5. Hace referencia a los últimos paquetes NuGet de MVC 5 y puede usar scaffolding para agregar controladores y vistas.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET plantilla de Web API 2

Hemos añadido una nueva plantilla de Web API 2. Hace referencia a los paquetes NuGet de Web API 2 más recientes y puede usar scaffolding para agregar controladores y vistas.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Plantillas de elementos

Agregamos nuevas plantillas de elementos para vistas MVC 5, páginas web (Razor 3) y controladores de Web API 2. Instalan los paquetes NuGet relacionados en el proyecto mientras agregan nuevos elementos.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Cuando se aplica a un scaffolding a un controlador MVC o Web API mediante Entity Framework, usamos Framework 6. Para obtener más información acerca de Entity Framework, vea el Historial de [versiones](https://msdn.com/data/jj574253)de Entity Framework .

También puede descargar e instalar Entity Framework 6 Tools para Visual Studio 2012. Consulte [Get Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>andamios ASP.NET

ASP.NET Scaffolding es un marco de generación de código para ASP.NET aplicaciones web. Facilita la adición de código reutilizable al proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, el scaffolding se limitaba a ASP.NET proyectos MVC. Con esta actualización, ahora puede usar scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms. Esta actualización no admite la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms agregando dependencias MVC al proyecto. La compatibilidad con la generación de páginas para formularios Web Forms se agregará en una actualización futura.

Cuando se usa scaffolding, nos aseguramos de que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si comienza con un proyecto de formularios Web Forms ASP.NET y, a continuación, usa scaffolding para agregar un controlador de API web, los paquetes NuGet y las referencias necesarios se agregan automáticamente al proyecto.

Para agregar scaffolding MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento con scaffolding** y seleccione **dependencias MVC 5** en la ventana de diálogo. Hay dos opciones para scaffolding MVC; Mínimo y Completo. Si selecciona Mínimo, solo se agregan al proyecto los paquetes NuGet y las referencias de ASP.NET MVC. Si selecciona la opción Completo, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto MVC.

La compatibilidad con controladores asincrónicos de scaffolding utiliza las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y tutoriales, vea [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md). Estos tutoriales muestran scaffolding con Visual Studio 2013, pero también son aplicables a ASP.NET y Web Tools 2013.1 para Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Con esta actualización, Visual Studio 2012 ahora admite herramientas y edición de Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en las notas de la versión de [NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet elimina la necesidad de que los usuarios permitan explícitamente nuGet restaurar los paquetes que faltan. Al instalar NuGet 2.7, los usuarios consienten implícitamente la restauración automática de los paquetes que faltan. Los usuarios pueden excluirse explícitamente de la restauración de paquetes a través de la configuración de NuGet en Visual Studio. Este cambio simplifica el funcionamiento de la restauración de paquetes.

## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>andamios ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC y Web API Scaffolding - HTTP 404, Error no encontrado

Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quede en un estado incoherente. Algunos de los cambios realizados serán scaffolding se revertirán, pero otros cambios, como los paquetes NuGet instalados, no se revertirán. Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 al navegar a elementos con scaffolding.

Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccione MVC 5 Dependencias (mínimas o completas). Este proceso agregará todos los cambios necesarios al proyecto.

Para corregir este error para Web API:

1. Agregue la siguiente clase WebApiConfig al proyecto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configure WebApiConfig.Register en\_el método Application Start en Global.asax de la siguiente manera:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding

Si Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding con Entity Framework (como Web API 2 Controller con acciones, mediante Entity Framework), es posible que Visual Studio Express no haya podido cargar la imagen nativa de un ensamblado dependiente de System.Web.Extensions.

Para corregir este problema, configure Visual Studio Express para que funcione con la imagen MSIL de System.Web.Extensions:

1. Abra el símbolo del sistema en el modo Administrador.
2. Ir a %Archivos de programa%-Microsoft Visual Studio 11.0-Common7-IDE o %ProgramFiles(x86)%-Microsoft Visual Studio 11.0-Common7-IDE (para Windows de 64 bits).
3. Abra VWDExpress.exe.config en un editor de texto.
4. Agregue la siguiente &lt;línea&gt;/&lt;&gt; en el elemento de tiempo de ejecución de configuración:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Reinicie Visual Studio Express 2012 para Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>Ver el archivo cshtml con Browse With o F5 provoca un error de servidor

Al crear un proyecto MVC 5 en Visual Studio 2012 (o abrir en Visual Studio 2012 un proyecto MVC 5 que se creó en Visual Studio 2013) e intentar ver un archivo cshtml mediante Examinar con o F5, recibirá un error que indica : Error de **servidor en la aplicación '/'**. El servidor intenta navegar a`http://localhost:XXXX/Views/../XXXX.cshtml`

Para resolver este problema, cambie la configuración **de Acción de inicio** del proyecto a Página **específica**. No es necesario proporcionar un valor para la página.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Después de realizar este cambio, al seleccionar F5 se navega a la raíz de la aplicación (`http://localhost:XXXX`). Este comportamiento no es el mismo que el comportamiento de los proyectos MVC 5 en Visual Studio 2013, donde la configuración **Página actual** inicia la página abierta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Url Rewrite y Tilde(')

Después de actualizar a ASP.NET Razor 3 o ASP.NET MVC 5, es posible que la notación de tilde (o) ya no funcione correctamente si está utilizando reescrituras de URL. La reescritura de url afecta a la notación &lt;tilde()) &lt;en&gt;elementos HTML como A/&gt;, &lt;SCRIPT/&gt;, LINK/ y, como resultado, la tilde ya no se asigna al directorio raíz.

Por ejemplo, si vuelve **a**escribir las solicitudes de &lt; **asp.net/content** asp.net , el atributo&gt; href en A href-"/content/"/ se resuelve en **/content/content/** en lugar de **/**. Para suprimir este cambio, puede establecer el contexto **WasUrlRewritten\_** de IIS en false en cada página Web o en Application **\_BeginRequest** en Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Plantillas

Al crear proyectos de ASP.NET MVC con Visual Studio 2012 en Windows 8.1 o Windows Server 2012 R2, Visual Studio muestra un mensaje de error que indica "Error al configurar Web [url] para ASP.NET 4.5."

![error de configuración](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Verá este error porque Visual Studio 2012 no habilita la característica ASP.NET 4.5 cuando se instala en esas versiones de Windows. Para habilitar ASP.NET 4.5, siga los pasos descritos en [Activar o desactivar](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)las características de Windows .

![activar o desactivar las características de Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Como alternativa, puede habilitar ASP.NET 4.5 a través de la línea de comandos.

1. Abra el símbolo del sistema en el modo Administrador.
2. Ejecute el siguiente comando para habilitar ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
