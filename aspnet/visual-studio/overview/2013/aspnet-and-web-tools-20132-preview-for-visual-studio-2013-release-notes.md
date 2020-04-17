---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET y Web Tools 2013.2 para Visual Studio 2013 Notas de la versión de Visual Studio 2013 Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 41b0f3ad43846bc5799ecd398188b0f6ffcc0d66
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543631"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Notas de la versión de ASP.NET and Web Tools 2013.2 para Visual Studio 2013

por [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas de la instalación

ASP.NET y Web Tools para Visual Studio 2013.2 se incluyen en el instalador principal y se pueden descargar como parte de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentación

Los tutoriales y otra información sobre ASP.NET y Herramientas web para Visual Studio 2013.2 están disponibles en el [sitio web de ASP.NET.](https://www.asp.net/)

## <a name="software-requirements"></a>Requisitos de software

ASP.NET y Web Tools para Visual Studio 2013.2 requiere Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuevas características de ASP.NET y Herramientas web para Visual Studio 2013.2

En las secciones siguientes se describen las características que se han introducido en la versión.

- [Plantillas de proyecto de una ASP.NET](#oneaspnet)
- [Soporte SSL al iniciar aplicaciones web en IIS Express](#ssl)
- [Mejoras de Visual Studio Web Editor](#vswebeditor)
- [Vínculo con exploradores](#browserlink)
- [Compatibilidad con Azure App Service Web Apps en Visual Studio](#waws)
- [Crear recursos remotos de Azure al crear un nuevo proyecto web](#AzureResources)
- [Mejoras en la publicación web](#webpublish)
- [andamios ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Formularios Web Forms ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Páginas Web 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET identidad 2.0.0](#identity)
- [Componentes de Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Plantillas de proyecto de una ASP.NET

- Actualizaciones de ASP.NET plantillas de proyecto para admitir la confirmación de la cuenta y el restablecimiento de contraseña.
- Actualice ASP.NET plantilla de API web para admitir la autenticación mediante cuentas organizativas locales.
- La plantilla SPA ASP.NET ahora contiene la autenticación que se basa en MVC y las vistas del lado del servidor. La plantilla tiene un controlador WebAPI al que solo pueden acceder los usuarios autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Soporte SSL al iniciar aplicaciones web en IIS Express

Para eliminar la advertencia de seguridad al examinar y depurar HTTPS en localhost, hemos agregado un cuadro de diálogo para permitir que Internet Explorer y Chrome confíen en el certificado SSL expreso de IIS autofirmado.

Por ejemplo, una propiedad de proyecto web se puede establecer para usar SSL. Haga clic en F4 para abrir el cuadro de diálogo de propiedades. Cambie **SSL habilitado** a true. Copie la URL de SSL.

![Propiedad habilitada para SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Establezca la pestaña web de la página de propiedades del `https://localhost:44300/` proyecto web para usar la dirección URL basada en HTTPS (la dirección URL SSL será a menos que haya creado previamente sitios web SSL.)

![Establecer URL del proyecto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Presione CTRL+F5 para ejecutar la aplicación. Siga las instrucciones para confiar en el certificado autofirmado que iIS Express ha generado.

![Advertencia SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lea el cuadro de diálogo Advertencia de **seguridad** y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa localhost.

![Advertencia de seguridad](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

El sitio se mostrará en IE o Chrome sin la advertencia de certificado en el navegador.

![Página HTTPS sin advertencias](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox utiliza su propio almacén de certificados, por lo que mostrará una advertencia.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Mejoras de Visual Studio Web Editor

- **Nuevo elemento de proyecto JSON y editor:** hemos agregado un elemento de proyecto JSON y un editor a Visual Studio. Las características actuales del editor JSON incluyen coloración, validación de sintaxis, finalización de llaves, esquematización, configuración de opciones de herramientas y mucho más.

    ![JSON Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense ahora admite [JSON Schema](http://json-schema.org/) v3 y v4. Hay un cuadro combinado de esquema para elegir esquemas existentes, editar la ruta de acceso del esquema local o simplemente arrastrar soltar un archivo JSON de proyecto en él para obtener la ruta de acceso relativa.

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor de esquemas JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuevo editor sass (SCSS):** Agregamos LESS en VS2013 RTM, y ahora tenemos un elemento de proyecto sass y un editor. Las características del editor de Sass son comparables al editor LESS, e incluyen coloración, variable y Mixins IntelliSense, comentario / uncomentario, información rápida, formato, validación de sintaxis, esquematización, definición de goto, selector de color, configuración de opciones de herramientas, etc.

    ![Añadir nuevo artículo: Hoja de estilos SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de hojas de estilo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- Nuevo selector de **URL en documentos HTML, Razor, CSS, LESS y Sass:** VS 2013 se incluye sin selector de URL fuera de las páginas de formularios Web Forms. El nuevo selector de URL para editores HTML, Razor, CSS, LESS y Sass es un selector de escritura fluido y sin diálogos que entiende '..' y filtra las listas de archivos adecuadamente para etiquetas y enlaces img.

    ![Selector de URL para la etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selector de URL para vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selector de URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Actualizaciones al editor LESS añadiendo más funciones**
- Actualización de **Intellisense knockout:** Hemos añadido una sintaxis KnockOut no estándar para VS intelliSense, sintaxis "ko-vs-editor viewModel:". Se puede utilizar para enlazar a varios modelos de vista en una página mediante comentarios en el formulario:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    También agregamos compatibilidad con ViewModel IntelliSense anidado, por lo que puede profundizar en objetos profundamente anidados en ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    El IntelliSense que se muestra es el IntelliSense completo del objeto JavaScript.

    ![Intellisense muestra el objeto JavaScript completo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- Nuevo selector de **URL en documentos HTML, Razor, CSS, LESS y Sass:** VS 2013 se suministra sin selector de URL fuera de las páginas de formularios Web Forms. El nuevo selector de URL para editores HTML, Razor, CSS, LESS y Sass es un selector de escritura fluido y sin diálogos que entiende '..' y filtra las listas de archivos adecuadamente para etiquetas y enlaces img.

    ![Selector de URL para la etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selector de URL para vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selector de URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Vínculo con exploradores

- El vínculo del explorador ahora admite conexiones HTTPS y lo enumerará en el panel con otras conexiones, siempre y cuando el explorador confíe en el certificado.
- Asignación de código fuente HTML estática
- Soporte SPA para datos de mapeo
- Actualización automática de datos de asignación

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Compatibilidad con Azure App Service Web Apps en Visual Studio

- **Admite el inicio de sesión de Azure.**
- **Depuración remota y Vista remota para aplicaciones web:** ahora admitimos [la depuración remota para aplicaciones web en Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) y la vista remota de archivos de contenido de aplicaciones web en el explorador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Crear recursos remotos de Azure al crear un nuevo proyecto web

Agregamos una casilla de verificación ["Crear recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) de Azure en el nuevo cuadro de diálogo de la aplicación web. Al elegirlo, podrá integrar la experiencia de crear una nueva aplicación web, configurar el sitio de publicación de Azure para realizar pruebas y crear un perfil de publicación en unos sencillos pasos.

![Nuevo proyecto con recursos de Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicación en Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Mejoras en la publicación web

- Mejorar la experiencia del usuario para la publicación.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>andamios ASP.NET

- **Soporte de Enum:** Si el modelo usa Enums, mvc Scaffolder generará la lista desplegable para Enum. Esto usa las aplicaciones auxiliares de Enum en MVC.
- **Compatibilidad con Bootstrap:** se han actualizado las plantillas EditorFor en MVC Scaffolding para que usen las clases Bootstrap.
- **Compatibilidad con paquetes:** MVC y Web API Scaffolders agregarán paquetes 5.1 para MVC y Web API

Las siguientes capturas de pantalla muestran modelos de andamios.

- Código del modelo:

     ![Código modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compile el código del modelo, haga clic con el botón derecho y seleccione **Agregar**, **Nuevo elemento con scaffolding**.

     ![Añadir nuevo elemento con andamios](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Elija **MVC5 Controller con vistas, mediante Entity Framework:**

     ![Agregar nuevo controlador MVC5 con vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Agregar controlador** mediante el modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Compruebe el código generado, por ejemplo Views/WeekdayModels/Edit.cshtml contains `@Html.EnumDropDownListFor`: ![View containing EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Ejecute la página para ver el cuadro combinado enum generado, observe que si un valor puede ser null, se puede elegir una cadena vacía para el cuadro combinado. Por ejemplo, la página **Crear** muestra lo siguiente:

    ![Caja combinada que permite una cadena vacía](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM se lanzará en abril de 2014. Aquí están los puntos destacados de las notas de la versión, pero por favor revise las [notas de la versión completa](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obtener más información sobre estos cambios.

- Aplicaciones de **Windows Phone 8.1**de destino: NuGet 2.8.1 ahora admite la segmentación de aplicaciones de Windows Phone 8.1 mediante los monikers de marco de destino 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' y 'WPA81'.

- **Resolución de revisiones para dependencias:** al resolver las dependencias de paquetes, NuGet ha implementado históricamente una estrategia de selección de la versión de paquete principal y secundaria más baja que satisface las dependencias del paquete. A diferencia de la versión principal y secundaria, sin embargo, la versión del parche siempre se resolvió a la versión más alta. Aunque el comportamiento era bien intencionado, creaba una falta de determinismo para instalar paquetes con dependencias.
- **DependencyVersion Switch**: Aunque NuGet 2.8 cambia el comportamiento *predeterminado* para resolver dependencias, también agrega un control más preciso sobre el proceso de resolución de dependencias a través del modificador -DependencyVersion en la consola del administrador de paquetes. El modificador permite resolver las dependencias a la versión más baja posible (comportamiento predeterminado), la versión más alta posible o la versión menor o de revisión más alta. Este Switch trabaja solamente para el install-package en el comando powershell.
- **Atributo DependencyVersion:** además del modificador -DependencyVersion detallado anteriormente, NuGet también ha permitido la capacidad de establecer un nuevo atributo en el archivo nuget.config que define cuál es el valor predeterminado, si el modificador -DependencyVersion no se especifica en una invocación de install-package. Este valor también será respetado por el cuadro de diálogo Administrador de paquetes NuGet para cualquier operación de paquete de instalación. Para establecer este valor, agregue el atributo siguiente al archivo nuget.config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- Vista previa de **las operaciones NuGet con -WhatIf:** algunos paquetes NuGet pueden tener gráficos de dependencia profunda y, como tal, puede ser útil durante una operación de instalación, desinstalación o actualización para ver primero lo que sucederá. NuGet 2.8 agrega el estándar de PowerShell -what what if switch to the install-package, uninstall-package, and update-package commands to enable visualizing the entire closure of packages to which the command will be applied.
- **Paquete**de degradación: No es raro instalar una versión preliminar de un paquete con el fin de investigar nuevas características y luego decidir revertir a la última versión estable. Antes de NuGet 2.8, se trataba de un proceso de varios pasos para desinstalar el paquete de versión preliminar y sus dependencias y, a continuación, instalar la versión anterior. Con NuGet 2.8, sin embargo, el paquete de actualización ahora revertirá todo el cierre del paquete (por ejemplo, el árbol de dependencias del paquete) a la versión anterior.
- **Dependencias**de desarrollo: muchos tipos diferentes de capacidades se pueden entregar como paquetes NuGet, incluidas las herramientas que se usan para optimizar el proceso de desarrollo. Estos componentes, aunque pueden ser fundamentales en el desarrollo de un nuevo paquete, no deben considerarse una dependencia del nuevo paquete cuando se publique más adelante. NuGet 2.8 permite que un paquete se identifique en el archivo .nuspec como developmentDependency. Cuando se instala, estos metadatos también se agregarán al archivo packages.config del proyecto en el que se instaló el paquete. Cuando ese archivo packages.config se analiza más adelante para las dependencias de NuGet durante el paquete nuget.exe, excluirá las dependencias marcadas como dependencias de desarrollo.
- **Archivos packages.config individuales para diferentes plataformas:** al desarrollar aplicaciones para varias plataformas de destino, es común tener diferentes archivos de proyecto para cada uno de los entornos de compilación respectivos. También es común consumir diferentes paquetes NuGet en diferentes archivos de proyecto, ya que los paquetes tienen diferentes niveles de compatibilidad para diferentes plataformas. NuGet 2.8 proporciona compatibilidad mejorada para este escenario mediante la creación de diferentes archivos packages.config para diferentes archivos de proyecto específicos de la plataforma.
- **Retroceso a caché local:** aunque los paquetes NuGet normalmente se consumen desde una galería remota, como la [galería NuGet](http://www.nuget.org) mediante una conexión de red, hay muchos escenarios en los que el cliente no está conectado. Sin una conexión de red, el cliente NuGet no pudo instalar correctamente paquetes, incluso cuando esos paquetes ya estaban en el equipo del cliente en la caché NuGet local. NuGet 2.8 agrega reserva de caché automática a la consola del administrador de paquetes.

    La característica de reserva de caché no requiere ningún argumento de comando específico. Además, la reserva de caché actualmente solo funciona en la consola del administrador de paquetes: el comportamiento no funciona actualmente en el cuadro de diálogo del administrador de paquetes.
- **Corrección**de errores: Una de las principales correcciones de errores realizadas fue la mejora del rendimiento en el comando update-package -reinstall.

    Además de estas características y la corrección de rendimiento antes mencionada, esta versión de NuGet también incluye muchas otras correcciones de errores. Hubo 181 problemas totales abordados en la versión. Para obtener una lista completa de los elementos de trabajo corregidos en NuGet 2.8, vea [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) para esta versión.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularios Web Forms ASP.NET

- Las plantillas de formularios Web Forms ahora muestran cómo realizar la confirmación de la cuenta y el restablecimiento de contraseña para ASP.NET identidad.
- El control Origen de datos de entidad y el proveedor de datos dinámicos para Entity Framework 6. Para obtener más información, consulte el siguiente blog de MSDN: Proveedor de [datos dinámicos y Control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Mejoras en el enrutamiento de atributos](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Soporte de Bootstrap para plantillas de editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Soporte de enum en vistas](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Compatibilidad discreta con atributos MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Apoyar el contexto "esto" en El Ajax discreto](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Varias [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Manejo global de errores](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Mejoras en el enrutamiento de atributos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Mejoras en la página de ayuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Compatibilidad con IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formatterador de tipo multimedia BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Mejor soporte para filtros asincrónicos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Análisis de consultas para la biblioteca de formato de cliente](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Varias [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Páginas Web 3.1.2

- Varias [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework se ha actualizado a la versión 6.1 tanto para tiempo de ejecución como para herramientas. Entity Framework (EF) 6.1 es una actualización menor de Entity Framework 6 e incluye una serie de correcciones de errores y nuevas características. Para obtener información detallada sobre EF6.1, incluidos los vínculos a la documentación de las nuevas características, vea Historial de [versiones](https://msdn.microsoft.com/data/jj574253)de Entity Framework . Las nuevas características de esta versión incluyen:

- **La consolidación de** herramientas proporciona una forma coherente de crear un nuevo modelo de EF. Esta característica amplía el asistente ADO.NET Entity Data Model para admitir la creación de modelos de Code First, incluida la ingeniería inversa de una base de datos existente. Estas características estaban disponibles anteriormente en calidad Beta en EF Power Tools.
- **El control de errores de confirmación** de transacción proporciona el nuevo [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) que hace uso de la capacidad recién introducida para interceptar operaciones de transacción. **El CommitFailureHandler** permite la recuperación automática de errores de conexión mientras se confirma una transacción.
- **IndexAttribute** permite especificar índices colocando un atributo en una propiedad (o propiedades) en el modelo de Code First. Code First creará un índice correspondiente en la base de datos.
- La API de **asignación pública** proporciona acceso a la información que EF tiene sobre cómo se asignan las propiedades y los tipos a las columnas y tablas de la base de datos. En versiones anteriores, esta API era interna.
- **Capacidad para configurar interceptores a través del archivo App/Web.config**(permitiendo agregar interceptores sin volver a compilar la aplicación).
- **DatabaseLogger** es un nuevo interceptor que facilita el registro de todas las operaciones de base de datos en un archivo. En combinación con la característica anterior, esto le permite cambiar fácilmente el registro de las operaciones de base de datos para una aplicación implementada, sin necesidad de volver a compilar.
- Se ha mejorado la detección de cambios del modelo de **migraciones** para que las migraciones con scaffolding sean más precisas; rendimiento del proceso de detección de cambios también se ha mejorado en gran medida.
- **Mejoras de rendimiento,** incluidas operaciones de base de datos reducidas durante la inicialización, optimizaciones para la comparación de igualdad nula en consultas LINQ, generación de vistas más rápida (creación de modelos) en más escenarios y materialización más eficaz de entidades de seguimiento con varias asociaciones.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET identidad 2.0.0

- **Autenticación de dos factores:** ASP.NET identidad ahora admite la autenticación de dos factores. La autenticación de dos factores proporciona una capa adicional de seguridad a sus cuentas de usuario en caso de que su contraseña se vea comprometida. También hay protección para los ataques de fuerza bruta contra los dos códigos de factor.
- **Bloqueo de cuenta:** Proporciona una manera de bloquear al usuario si el usuario introduce su contraseña o códigos de dos factores incorrectamente. El número de intentos no válidos y el intervalo de tiempo para los usuarios están bloqueados se pueden configurar. Un desarrollador puede desactivar opcionalmente el bloqueo de cuentas para determinadas cuentas de usuario en caso de que sea necesario.
- **Confirmación de la cuenta:** El sistema ASP.NET Identity ahora admite la confirmación de la cuenta. Este es un escenario bastante común en la mayoría de los sitios web hoy en día donde, cuando se registra para una nueva cuenta en el sitio web, se le requiere confirmar su correo electrónico antes de que pueda hacer cualquier cosa en el sitio web. Confirmación de correo electrónico es útil porque impide que se creen cuentas falsas. Esto es extremadamente útil si está utilizando el correo electrónico como un método de comunicación con los usuarios de su sitio web, como sitios del foro, banca, comercio electrónico o sitios web sociales.
- **Restablecimiento de contraseña:** Restablecimiento de contraseña es una característica donde el usuario puede restablecer sus contraseñas si ha olvidado su contraseña.
- **Sello de seguridad (cierre la sesión en todas partes):** Admite una forma de regenerar el token de seguridad para el usuario en los casos en que el usuario cambia su contraseña o cualquier otra información relacionada con la seguridad, como la eliminación de un inicio de sesión asociado (como Facebook, Google, cuenta Microsoft, etc.). Esto es necesario para asegurarse de que los tokens generados con la contraseña antigua se invalidan. En el proyecto de ejemplo, si cambia la contraseña del usuario, se genera un nuevo token para el usuario y se invalidan los tokens anteriores. Esta característica proporciona una capa adicional de seguridad a su aplicación ya que cuando cambie su contraseña, se cerrará la sesión desde cualquier lugar (todos los demás navegadores) donde ha iniciado sesión en esta aplicación.
- **Haga que el tipo de clave principal sea extensible para usuarios y roles:** en ASP.NET identidad 1.0, el tipo de clave principal para la tabla Usuarios y roles era cadenas. Esto significa que cuando el sistema de identidad de ASP.NET se conservaba en SQL Server mediante Entity Framework, estábamos usando nvarchar. Hubo muchas discusiones en torno a esta implementación predeterminada en Stack Overflow y en base a los comentarios entrantes. Hemos proporcionado un enlace de extensibilidad donde puede especificar cuál debe ser la clave principal de la tabla Usuarios y roles. Este enlace de extensibilidad es especialmente útil si va a migrar la aplicación y la aplicación estaba almacenando UserIds son GUID o ints.
- **Compatibilidad con IQueryable en usuarios y roles:** se ha agregado compatibilidad con IQueryable en UsersStore y RolesStore, puede obtener fácilmente la lista de usuarios y roles.
- **Soporte de la operación De eliminar a través del UserManager**
- **Indización en UserName:** en ASP.NET implementación de Identity Entity Framework, hemos agregado un índice único en el nombre de usuario mediante el nuevo IndexAttribute en EF 6.1.0. Esto se asegura de que los nombres de usuario son siempre únicos y no había ninguna condición de carrera en la que usted podría terminar con los nombres de usuario duplicados.
- **Validador de contraseña mejorado:** El validador de contraseñas que se envió en ASP.NET Identity 1.0 era un validador de contraseña bastante básico que solo validaba la longitud mínima. Hay un nuevo validador de contraseña sin necesidad de controlar la complejidad de la contraseña. Tenga en cuenta que incluso si activa todos los ajustes de esta contraseña, le recomendamos que habilite la autenticación de dos factores para las cuentas de usuario.
- **IdentityFactory Middleware/ CreatePerOwinContext**:

    - **Administrador de usuarios:** puede utilizar la implementación de Factory para obtener una instancia de UserManager desde el contexto OWIN. Este patrón es similar a lo que usamos para obtener AuthenticationManager del contexto OWIN para el inicio de sesión y el cierre de sesión. Esta es una forma recomendada de obtener una instancia de UserManager por solicitud para la aplicación.
    - **DbContextFactory**: ASP.NET Identity usa Entity Framework para conservar el sistema de identidades en SQL Server. Para ello, el sistema de identidades tiene una referencia a ApplicationDbContext. El Middleware DbContextFactory devuelve una instancia de ApplicationDbContext por solicitud que puede usar en la aplicación.
- **ASP.NET paquete NuGet**de ejemplos de identidad: el paquete NuGet de ejemplos puede facilitar la instalación y ejecución de ejemplos para ASP.NET identidad y seguir los procedimientos recomendados. Este es un ejemplo ASP.NET aplicación MVC. Modifique el código para que se adapte a la aplicación antes de implementarlo en producción. El ejemplo debe instalarse en una aplicación ASP.NET vacía. Para obtener más información sobre el paquete, vaya a la siguiente entrada de blog: [Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

Había un montón de errores que se corrigieron en esta versión. Consulte las notas de [la versión 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) para obtener información más detallada.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Había un montón de errores que se corrigieron en esta versión. Consulte las notas de [la versión 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obtener información más detallada.
