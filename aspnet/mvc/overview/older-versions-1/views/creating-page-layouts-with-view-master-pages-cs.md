---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Creación de diseños de página con ver páginas maestras (C- ) Microsoft Docs
author: rick-anderson
description: En este tutorial, aprenderá a crear un diseño de página común para varias páginas de la aplicación aprovechando las páginas maestras de vista. Puede utilizar un...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: d313e017d7061ae03e8dea79e611d0cf3838297d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542526"
---
# <a name="creating-page-layouts-with-view-master-pages-c"></a>Crear diseños de página con páginas maestras de vista (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> En este tutorial, aprenderá a crear un diseño de página común para varias páginas de la aplicación aprovechando las páginas maestras de vista. Puede usar una página maestra de vista, por ejemplo, para definir un diseño de página de dos columnas y usar el diseño de dos columnas para todas las páginas de la aplicación web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Creación de diseños de página con ver páginas maestras

En este tutorial, aprenderá a crear un diseño de página común para varias páginas de la aplicación aprovechando las páginas maestras de vista. Puede usar una página maestra de vista, por ejemplo, para definir un diseño de página de dos columnas y usar el diseño de dos columnas para todas las páginas de la aplicación web.

También puede aprovechar las páginas maestras de vista para compartir contenido común en varias páginas de la aplicación. Por ejemplo, puede colocar el logotipo de su sitio web, enlaces de navegación y anuncios de banner en una página maestra de vista. De este modo, cada página de la aplicación mostraría este contenido automáticamente.

En este tutorial, aprenderá a crear una nueva página maestra de vista y a crear una nueva página de contenido de vista basada en la página maestra.

### <a name="creating-a-view-master-page"></a>Creación de una página maestra de vista

Comencemos creando una página maestra de vista que defina un diseño de dos columnas. Para agregar una nueva página maestra de vista a un proyecto MVC, haga clic con el botón secundario en la carpeta Vistas, Seleccione la opción de menú **Agregar, Nuevo elemento**y seleccione la plantilla **Página maestra** de vista MVC (consulte la figura 1).

[![Adición de una página maestra de vista](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figura 01**: Adición de una página maestra de vista[(Haga clic para ver la imagen de tamaño completo)](creating-page-layouts-with-view-master-pages-cs/_static/image3.png)

Puede crear más de una página maestra de vista en una aplicación. Cada página maestra de vista puede definir un diseño de página diferente. Por ejemplo, es posible que desee que determinadas páginas tengan un diseño de dos columnas y otras páginas para tener un diseño de tres columnas.

Una página maestra de vista se parece mucho a una vista ESTÁNDAR ASP.NET MVC. Sin embargo, a diferencia de una vista `<asp:ContentPlaceHolder>` normal, una página maestra de vista contiene una o más etiquetas. Las `<contentplaceholder>` etiquetas se utilizan para marcar las áreas de la página maestra que se pueden invalidar en una página de contenido individual.

Por ejemplo, la página maestra de vista del listado 1 define un diseño de dos columnas. Contiene dos `<contentplaceholder>` etiquetas. Uno `<ContentPlaceHolder>` para cada columna.

**Listado 1 –`Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

El cuerpo de la página maestra `<div>` de vista en el listado 1 contiene dos etiquetas que corresponden a las dos columnas. La clase de columna Hoja de `<div>` estilos en cascada se aplica a ambas etiquetas. Esta clase se define en la hoja de estilos declarada en la parte superior de la página maestra. Puede obtener una vista previa de cómo se renderizará la página maestra de vista cambiando a la vista Diseño. Haga clic en la pestaña Diseño en la parte inferior izquierda del editor de código fuente (consulte la figura 2).

[![Vista previa de una página maestra en el diseñador](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figura 02**: Vista previa de una página maestra en el diseñador[(Haga clic para ver la imagen de tamaño completo)](creating-page-layouts-with-view-master-pages-cs/_static/image6.png)

### <a name="creating-a-view-content-page"></a>Creación de una página de contenido de vista

Después de crear una página maestra de vista, puede crear una o varias páginas de contenido de vista basadas en la página maestra de vista. Por ejemplo, puede crear una página de contenido de vista de índice para el controlador de inicio haciendo clic con el botón secundario en la carpeta Vistas, Inicio, seleccionando **Agregar, Nuevo elemento**, seleccionando la plantilla Página de contenido de **vista MVC,** escribiendo el nombre Index.aspx y haciendo clic en el botón **Agregar** (consulte la figura 3).

[![Adición de una página de contenido de vista](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figura 03**: Adición de una página de contenido de vista[(Haga clic para ver la imagen de tamaño completo)](creating-page-layouts-with-view-master-pages-cs/_static/image9.png)

Después de hacer clic en el botón Agregar, aparece un nuevo cuadro de diálogo que le permite seleccionar una página maestra de vista para asociarla a la página de contenido de vista (consulte la figura 4). Puede navegar a la página maestra de la vista Site.master que creamos en la sección anterior.

[![Selección de una página maestra](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figura 04**: Selección de una página maestra[(Haga clic para ver la imagen de tamaño completo)](creating-page-layouts-with-view-master-pages-cs/_static/image12.png)

Después de crear una nueva página de contenido de vista basada en la página maestra Site.master, obtendrá el archivo en el listado 2.

**Listado 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Observe que esta `<asp:Content>` vista contiene una etiqueta `<asp:ContentPlaceHolder>` que corresponde a cada una de las etiquetas de la página maestra de la vista. Cada `<asp:Content>` etiqueta incluye un atributo ContentPlaceHolderID `<asp:ContentPlaceHolder>` que apunta a la particular que reemplaza.

Observe, además, que la página de vista de contenido en el listado 2 no contiene ninguna de las etiquetas HTML de apertura y cierre normales. Por ejemplo, no contiene las `<html>` `<head>` etiquetas de apertura y cierre. Todas las etiquetas de apertura y cierre normales se encuentran en la página maestra de la vista.

Cualquier contenido que desee mostrar en una página `<asp:Content>` de contenido de vista debe colocarse dentro de una etiqueta. Si coloca cualquier contenido HTML u otro contenido fuera de estas etiquetas, obtendrá un error cuando intente ver la página.

No es necesario reemplazar `<asp:ContentPlaceHolder>` todas las etiquetas de una página maestra en una página de vista de contenido. Solo necesita invalidar `<asp:ContentPlaceHolder>` una etiqueta cuando desee reemplazar la etiqueta con contenido determinado.

Por ejemplo, la vista de índice `<asp:Content>` modificada en el listado 3 contiene solo dos etiquetas. Cada una `<asp:Content>` de las etiquetas incluye texto.

**Listado 3 –`Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Cuando se solicita la vista en el listado 3, representa la página en la figura 5. Observe que la vista representa una página con dos columnas. Observe, además, que el contenido de la página de contenido de vista se combina con el contenido de la página maestra de vista

[![La página de contenido de la vista de índice](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figura 05**: La página de contenido de la vista de índice ([Haga clic para ver la imagen de tamaño completo](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modificación del contenido de la página maestra de view

Un problema que se produce casi inmediatamente al trabajar con páginas maestras de vista es el problema de modificar el contenido de la página maestra de vista cuando se solicitan diferentes páginas de contenido de vista. Por ejemplo, desea que cada página de la aplicación web tenga un título único. Sin embargo, el título se declara en la página maestra de vista y no en la página de contenido de la vista. Entonces, ¿cómo personaliza el título de la página para cada página de contenido de vista?

Hay dos maneras de modificar el título que muestra una página de contenido de vista. En primer lugar, puede asignar un título `<%@ page %>` de página al atributo title de la directiva declarada en la parte superior de una página de contenido de vista. Por ejemplo, si desea asignar el título de página "Super Great Website" a la vista de índice, puede incluir la siguiente directiva en la parte superior de la vista de índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Cuando la vista de índice se representa en el explorador, el título deseado aparece en la barra de título del navegador:

[![Barra de título del navegador](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)

Hay un requisito importante que una página de vista maestra debe satisfacer para que el atributo title funcione. La página maestra de `<head runat="server">` vista debe `<head>` contener una etiqueta en lugar de una etiqueta normal para su encabezado. Si `<head>` la etiqueta no incluye el atributo runat-"server", el título no aparecerá. La página maestra de `<head runat="server">` vista predeterminada incluye la etiqueta necesaria.

Un enfoque alternativo para modificar el contenido de la página maestra desde una `<asp:ContentPlaceHolder>` página de contenido de vista individual es ajustar la región que desea modificar en una etiqueta. Por ejemplo, imagine que desea cambiar no solo el título, sino también las metaetiquetas, representadas por una página de vista maestra. La página de vista maestra `<asp:ContentPlaceHolder>` del `<head>` listado 4 contiene una etiqueta dentro de su etiqueta.

**Listado 4 –`Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Tenga en `<asp:ContentPlaceHolder>` cuenta que la etiqueta del listado 4 incluye contenido predeterminado: un título predeterminado y metaetiquetas predeterminadas. Si no invalida `<asp:ContentPlaceHolder>` esta etiqueta en una página de contenido de vista individual, se mostrará el contenido predeterminado.

La página de vista de `<asp:ContentPlaceHolder>` contenido en el listado 5 reemplaza la etiqueta para mostrar un título personalizado y metaetiquetas personalizadas.

**Listado 5 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Resumen

Este tutorial le proporcionó una introducción básica para ver páginas maestras y páginas de contenido. Ha aprendido a crear nuevas páginas maestras de vista y a crear páginas de contenido de vista basadas en ellas. También examinamos cómo puede modificar el contenido de una página maestra de vista desde una página de contenido de vista determinada.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Siguiente](passing-data-to-view-master-pages-cs.md)
