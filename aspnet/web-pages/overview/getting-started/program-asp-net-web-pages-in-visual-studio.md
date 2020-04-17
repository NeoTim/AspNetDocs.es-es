---
uid: web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programación ASP.NET páginas Web (Razor) con Visual Studio ( Visual Studio) Microsoft Docs
author: Rick-Anderson
description: En este apéndice se explica cómo puede usar Visual Studio 2010 o Visual Web Developer 2010 Express para programar ASP.NET páginas Web con la sintaxis de Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: e586a116fc48a797bdd40befad5851a548282ce6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542903"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programación ASP.NET páginas Web (Razor) con Visual Studio

 por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo puede usar Visual Studio o Visual Web Developer Express para programar sitios web de ASP.NET páginas Web (Razor).
>
> Temas que se abordarán
>
> - Lo que necesita instalar (si es que algo) para trabajar con ASP.NET páginas Web en su versión de Visual Studio.
> - Cómo agregar compatibilidad con ASP.NET páginas web a Visual Web Developer 2010 Express.
> - Cómo usar características en Visual Studio para trabajar con páginas ASP.NET Razor, incluido IntelliSense y el depurador.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software utilizadas en el tutorial
>
>
> - ASP.NET Páginas Web (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Este tutorial también funciona con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 y WebMatrix 2.

Puede programar páginas Web ASP.NET con sintaxis Razor mediante WebMatrix o muchos otros editores de código. También puede usar Microsoft Visual Studio, que es un entorno de desarrollo integrado (IDE) con todas las funciones que proporciona un potente conjunto de herramientas para crear muchos tipos de aplicaciones (no solo sitios web). Para trabajar con páginas de ASP.NET Razor, puede usar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Dos características particularmente útiles que Visual Studio proporciona para la programación con ASP.NET páginas web de Razor son:

- *IntelliSense*. La característica IntelliSense integrada en Visual Studio es más completa que IntelliSense en WebMatrix.
- *Depurador*. El depurador le permite solucionar problemas del código deteniendo un programa mientras se está ejecutando, examinando variables y recorriendo el código línea por línea.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Uso de Visual Studio con diferentes versiones de ASP.NET páginas Web

Para desarrollar ASP.NET aplicaciones web en Visual Studio 2017, instale la carga de trabajo de **desarrollo web y ASP.NET.**

Visual Studio 2012 y Visual Studio 2013 incluyen compatibilidad con ASP.NET páginas Web. (Los paquetes necesarios para admitir ASP.NET páginas Web se instalan al instalar Visual Studio.)

Visual Studio 2010 no incluye compatibilidad de forma predeterminada para ASP.NET páginas Web. Para usar ASP.NET páginas Web con Visual Studio 2010, debe instalar el paquete ASP.NET MVC. Para obtener ASP.NET páginas Web 2, instale ASP.NET MVC 4.

En la tabla siguiente se resume la compatibilidad con ASP.NET páginas Web en diferentes versiones de Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Instalar ASP.NET MVC 4 | (Incluido) | (Incluido) |
| **ASP.NET Web Pages 3** |  | Actualización a ASP.NET páginas Web 3 a través de NuGet | (Incluido) |

Para trabajar con Visual Studio 2010, vea [Instalación de compatibilidad con ASP.NET páginas Web en Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Iniciar Visual Studio desde WebMatrix

Si ha iniciado un proyecto en WebMatrix y desea cambiar a Visual Studio, WebMatrix proporciona un botón para abrir fácilmente el proyecto en Visual Studio. Debe tener Visual Studio instalado en el equipo para que este botón se habilite. La siguiente imagen muestra el botón en WebMatrix.

![iniciar Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Al hacer clic en el botón, el proyecto se abre en Visual Studio. Puede alternar entre WebMatrix y Visual Studio sin ningún problema. Se le notificará si algún archivo ha cambiado en el otro entorno y necesita volver a cargarse para obtener los últimos cambios.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Creación de ASP.NET sitio de Razor en Visual Studio

Para crear un sitio web de ASP.NET Razor en Visual Studio:

1. Abra Visual Studio.
2. En el menú **Archivo** , haga clic en **Nuevo sitio Web**.

    ![crear un nuevo sitio web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. En el cuadro de diálogo **Nuevo sitio Web,** seleccione el idioma que desea usar (Visual C o Visual Basic).
4. Seleccione la plantilla **ASP.NET sitio web (Razor).**

    ![sitio de afeitar](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Haga clic en **Aceptar**.

El nuevo proyecto existe y se rellena con algunas páginas web predeterminadas para ayudarle a empezar.

### <a name="using-intellisense"></a>Using IntelliSense

Ahora que ha creado un sitio, puede ver cómo funciona IntelliSense en Visual Studio.

1. En el sitio web que acaba de crear, abra el *Default.cshtml* página.
2. Después `<h3>` de las etiquetas `@ServerInfo.` de la página, escriba (incluido el punto). Observe cómo IntelliSense muestra los `ServerInfo` métodos disponibles para la aplicación auxiliar en una lista desplegable.

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Seleccione `GetHtml` el método de la lista y, a continuación, pulse Intro. IntelliSense rellena automáticamente el método. (Al igual que con cualquier método `()` en C-, debe agregar caracteres después del método.) El código completado `GetHtml` para el método es similar al siguiente ejemplo:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Presione Ctrl+F5 para ejecutar la página. Esto es lo que la página se ve como cuando se muestra en un navegador:

    ![página predeterminada en el navegador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Cierre el explorador.

### <a name="using-the-debugger"></a>Uso del depurador

1. En la parte superior de la *Página Default.cshtml,* después de la línea que comienza con `Page.Title`, agregue la siguiente línea de código:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. En el margen gris del editor a la izquierda del código, haga clic al lado de esta nueva línea para agregar un punto de *interrupción.* Un punto de interrupción es un marcador que indica al depurador que deje de ejecutar el programa en ese momento para que pueda ver lo que está sucediendo.

    ![establecer punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Quite la llamada `ServerInfo.GetHtml` al método y agregue `@myTime` una llamada a la variable en su lugar. Esta llamada muestra el valor de hora actual devuelto por la nueva línea de código.
4. Presione F5 para ejecutar la página en el depurador. La página se detiene en el punto de interrupción establecido. La siguiente imagen muestra el aspecto de la página en el editor con el punto de interrupción (en amarillo).

    ![punto de interrupción de depuración](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. En la barra de herramientas Depurar, haga clic en el botón **Paso** a paso (o presione F11) para ejecutar la siguiente línea de código. Cada vez que haga clic en este botón, avance la ejecución a la siguiente línea de código.

    ![Entrar en el botón](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examine el valor `myTime` de la variable manteniendo el puntero del mouse sobre ella o inspeccionando los valores que se muestran en las **ventanas Locales** y Pila de **llamadas.** Visual Studio muestra el valor de la variable.

    ![mostrar el valor de la hora](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Cuando haya terminado de examinar la variable y recorrer el código, presione F5 para continuar ejecutando la página sin detenerse en cada línea. Cuando haya terminado de recorrer todo el código, el explorador mostrará la página.

Para obtener más información sobre el depurador y cómo depurar código en Visual Studio, vea [Tutorial: depurar páginas web en Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Uso de Razor en proyectos MVC ASP.NET con Visual Studio

La sintaxis de Razor también se utiliza ampliamente en ASP.NET proyectos MVC. MVC es una forma eficaz y basada en patrones para crear sitios web dinámicos. Si el sitio de páginas Web de ASP.NET se vuelve difícil de mantener, es posible que desee considerar la posibilidad de convertirlo en una aplicación MVC ASP.NET. Para obtener un ejemplo de creación de una aplicación MVC, vea [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalación de compatibilidad con páginas Web ASP.NET en Visual Studio 2010

En esta sección se muestra cómo instalar Visual Web Developer Express 2010 y las herramientas de páginas web de ASP.NET para Visual Studio.

1. Si aún no tiene el Instalador de plataforma web, descárguelo desde la siguiente DIRECCIÓN URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Ejecute el Instalador de plataforma web.
3. Haga clic en la pestaña **Productos.**

    ![Ficha Productos WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Busque **ASP.NET MVC 4** (para ASP.NET páginas Web 2) y, a continuación, haga clic en **Agregar**. Estos productos incluyen herramientas de Visual Studio para crear sitios web de ASP.NET Razor.

    ![Opciones de instalación de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Haga clic en **Instalar** para completar la instalación.
