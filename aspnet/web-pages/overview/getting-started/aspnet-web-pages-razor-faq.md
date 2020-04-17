---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET las páginas web (Razor) Microsoft Docs
author: Rick-Anderson
description: En este artículo se enumeran algunas preguntas frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix. Versiones de software utilizadas en el tutorial ASP.NET Páginas Web (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: a312d1327bc88e721bf7fc7459e420e3f582c88d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543709"
---
# <a name="aspnet-web-pages-razor-faq"></a>Preguntas frecuentes de ASP.NET Web Pages (Razor)

 por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET páginas web. Use [Visual Studio](xref:web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o Visual Studio [Code](https://code.visualstudio.com/).
>
> En este artículo se enumeran algunas preguntas frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software utilizadas en el tutorial
> 
> 
> - ASP.NET Páginas Web (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2, WebMatrix 2 y Visual Studio 2012.

- [¿Cuál es la diferencia entre ASP.NET Web Pages, ASP.NET Web Forms y ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [¿Necesito WebMatrix para trabajar con páginas web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [¿Puedo usar ASP.NET controles de formularios Web Forms en una página de páginas Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [¿Puedo implementar un sitio de ASP.NET páginas Web sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [¿Tengo que usar la aplicación auxiliar WebSecurity para admitir inicios de sesión?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [¿ASP.NET páginas web es compatible con HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [¿Puedo usar JavaScript y jQuery con páginas web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionales](#AdditionalResources)

Para preguntas sobre errores y otros problemas, consulte la Guía de solución de problemas de [páginas web de ASP.NET (Razor).](https://go.microsoft.com/fwlink/?LinkId=253001)

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>¿Cuál es la diferencia entre ASP.NET Web Pages, ASP.NET Web Forms y ASP.NET MVC?

Las tres son tecnologías ASP.NET para crear aplicaciones web dinámicas:

- ASP.NET Web Pages se centra en agregar código dinámico (del lado del servidor) y acceso a la base de datos a páginas HTML, y cuenta con una sintaxis sencilla y ligera.
- ASP.NET formularios Web Forms se basa en un modelo de objetos de página y controles tradicionales de tipo de ventana (botones, listas, etc.). Los formularios Web Forms usan un modelo basado en eventos que es familiar para aquellos que han trabajado con el desarrollo basado en cliente (formularios Windows).
- ASP.NET MVC implementa el patrón model-view-controller para ASP.NET. El énfasis está en la "separación de preocupaciones" (procesamiento, datos y capas de interfaz de usuario).

Los tres marcos están totalmente respaldados y continúan siendo desarrollados por el equipo de ASP.NET. En general, la elección del marco de trabajo que se va a usar depende de los antecedentes y la experiencia con ASP.NET.

ASP.NET páginas web en particular fue diseñado para facilitar a las personas que ya saben HTML agregar procesamiento de servidor a sus páginas. Es una buena opción para estudiantes, aficionados, personas en general que son nuevos en la programación. También puede ser una buena opción para los desarrolladores que tienen experiencia con non-ASP.NET tecnologías web.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>¿Necesito WebMatrix para trabajar con páginas web?

No. WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET páginas web. Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o Visual Studio [Code](https://code.visualstudio.com/).

Si no desea usar Visual Studio o Visual Studio Code, puede instalar los productos de componentes individualmente mediante [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Necesita los siguientes productos:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (que instala el marco de trabajo de ASP.NET Web Pages también)
- IIS Express (el servidor web)
- Microsoft SQL Server Compact 4.0 (la base de datos)

Puede utilizar un editor de texto para editar *páginas .cshtml* (o *.vbhtml).*

Administrar bases de datos de SQL Server Compact (archivos *.sdf)* sin una herramienta es un poco más difícil. Visual Studio contiene herramientas para administrar bases de *datos .sdf.* También puede ejecutar comandos SQL en el código para realizar muchas tareas de administración de SQL ServerSQL Server .

Para probar *páginas .cshtml* sin usar un entorno de desarrollo integrado (IDE), puede implementarlas en un servidor activo. (Consulte [¿Puedo implementar un sitio de ASP.NET páginas Web sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Ejecutar IIS Express sin usar un IDE

Si instala IIS Express en el equipo como un servidor web, puede usarlo para probar las páginas. Puede ejecutar IIS Express desde la línea de comandos y asociarlo a un número de puerto específico. A continuación, especifique ese puerto cuando solicite archivos *.cshtml* en el explorador.

En Windows, abra un símbolo del sistema con privilegios de administrador y cambie a *C:-Archivos de programa-IIS Express.* (Para sistemas de 64 bits, utilice la carpeta *C:-Archivos de programa (x86)-IIS Express.)* A continuación, escriba el siguiente comando, utilizando la ruta de acceso real a su sitio:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Puede usar cualquier número de puerto que no esté reservado por algún otro proceso. (Los números de puerto superiores a 1024 suelen ser gratuitos.) Para `path` el valor, utilice la ruta de acceso de la carpeta del sitio web donde se encuentran los archivos *.cshtml.*

Después de ejecutar este comando para configurar IIS Express para servir las páginas, puede abrir un explorador y buscar un archivo *.cshtml.* Utilice una dirección URL como la siguiente:

`http://localhost:35896/default.cshtml`

Para obtener ayuda con las opciones `iisexpress.exe /?` de línea de comandos de IIS Express, escriba en la línea de comandos.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>¿Puedo usar ASP.NET controles de formularios Web Forms en una página de páginas Web?

No. Los controles de formularios Web Forms como el control [CheckBox,](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) los controles de [validación](https://msdn.microsoft.com/library/bwd43d0x)y el control [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) solo funcionan en páginas de formularios Web Forms (archivos *.aspx).* Estos controles requieren el marco de página de formularios Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>¿Puedo implementar un sitio de ASP.NET páginas Web sin usar WebMatrix?

Sí. Puede copiar manualmente los archivos del sitio web en un servidor (normalmente mediante FTP). Si realiza una copia manual, también tiene que copiar los archivos que admiten SQL Server Compact (la base de datos). Para obtener más información, consulte la entrada de blog [Implementación de aplicaciones de páginas web sin una herramienta.](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317)

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>¿Tengo que usar la aplicación auxiliar WebSecurity para admitir inicios de sesión?

No. El `SimpleMembership` proveedor que forma parte de ASP.NET páginas Web es una opción. Los proveedores de seguridad que forman parte de ASP.NET (con los que podría estar acostumbrado a trabajar en formularios Web Forms) también están disponibles. Por ejemplo, puede usar la autenticación de formularios en ASP.NET páginas web tal como lo haría en formularios Web Forms. Para obtener un ejemplo de cómo usar la autenticación de formularios, vea el artículo de soporte técnico de Microsoft Cómo implementar la [autenticación basada en formularios en la aplicación de ASP.NET mediante C .NET](https://support.microsoft.com/kb/301240). Para descargar un ejemplo sencillo, consulte [ASP.NET versión de "Contraseña de inicio &amp; de sesión](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obtener información acerca de cómo usar la autenticación de Windows, consulte la entrada de blog Uso de la autenticación de [Windows en ASP.NET páginas Web](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>¿ASP.NET páginas web es compatible con HTML5?

Sí. Las páginas que se crean con ASP.NET páginas Web *(páginas .cshtml* o *.vbhtml)* son esencialmente páginas HTML que también contienen código que se ejecuta en el servidor, antes de que se represente la página. Siempre que el explorador del usuario admita HTML5, puede usar elementos HTML5 en una página *.cshtml* o *.vbhtml.*

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>¿Puedo usar JavaScript y jQuery con páginas web?

Totalmente. Las páginas que se crean con ASP.NET páginas Web *(páginas .cshtml* o *.vbhtml)* son solo páginas HTML con código de servidor en ellas. Por lo tanto, cualquier cosa que pueda hacer en una página HTML normal mediante JavaScript o jQuery también puede hacer en una página *.cshtml* o *.vbhtml.*

La plantilla Sitio de **inicio** en WebMatrix contiene varias bibliotecas jQuery. Si crea un sitio con esa plantilla, la carpeta *Scripts* contiene una biblioteca principal de jQuery (*jquery-1.6.2.js)* y bibliotecas para la validación de jQuery (*jquery.validate.js*, etc.).

Aquí hay algunas entradas de blog que ilustran formas de usar jQuery con ASP.NET web:

- [Adición de jQuery Goodness a ASP.NET Web Pages usando WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) de Rachel Appel
- [5 min: WebMatrix + jQuery UI + json + jQuery plantillas](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [WebMatrix And jQuery Forms](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) de Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Foro web WebMatrix y ASP.NET web en](https://forums.asp.net/1224.aspx/1?WebMatrix) el sitio web de ASP.NET
