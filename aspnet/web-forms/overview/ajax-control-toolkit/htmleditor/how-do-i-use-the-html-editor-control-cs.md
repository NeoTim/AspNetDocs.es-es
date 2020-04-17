---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: ¿Cómo utilizo el Control del Editor HTML? (C-) Microsoft Docs
author: rick-anderson
description: HTMLEditor es un ASP.NET AJAX Control que le permite crear y editar fácilmente contenido HTML a través de botones en una barra de herramientas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: eb3a87f914c24ada52ebb5a315a7e242e96158f7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543124"
---
# <a name="how-do-i-use-the-html-editor-control-c"></a>¿Cómo utilizo el Control del Editor HTML? (C#)

por [Microsoft](https://github.com/microsoft)

> HTMLEditor es un ASP.NET AJAX Control que le permite crear y editar fácilmente contenido HTML a través de botones en una barra de herramientas.

El objetivo de este tutorial es proporcionarle una visión general del control Editor HTML incluido con el Kit de herramientas de control de AJAX. El Editor HTML incluye opciones para cambiar el tamaño de fuente, seleccionar una fuente, cambiar el color de fondo, modificar el color de primer plano, agregar vínculos, agregar imágenes, cambiar la alineación del texto y realizar operaciones de cortar, copiar y pegar (consulte la figura 1).

[![El Editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Figura 01**: El editor HTML[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-html-editor-control-cs/_static/image2.png)

El editor HTML le permite introducir contenido mediante un modo de diseño o puede introducir HTML directamente. También se le proporciona la opción de obtener una vista previa del contenido HTML (consulte la figura 2).

[![Botones diseño, HTML y vista previa](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Figura 02**: Botones de diseño, HTML y vista previa[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-html-editor-control-cs/_static/image4.png)

En este tutorial, aprenderá a mostrar el Editor HTML, a personalizar los botones de la barra de herramientas que aparecen en el Editor HTML y a evitar ataques de secuencias de comandos entre sitios.

## <a name="displaying-the-html-editor"></a>Visualización del Editor HTML

Para poder usar el Editor HTML en una página ASP.NET, primero debe agregar un control ScriptManager a la página. El control ScriptManager se encuentra debajo de la pestaña Extensiones de AJAX en el cuadro de herramientas Visual Studio/Visual Web Developer Express.

Debe colocar el control ScriptManager en la parte superior de la página antes que cualquier otro control de la página. Por ejemplo, puede colocarlo inmediatamente debajo &lt;&gt; de la etiqueta de formulario del lado del servidor de apertura.

El control Editor HTML se encuentra en el cuadro de herramientas con el resto de los controles del Kit de herramientas de control de AJAX. Se denomina el editor control (consulte la figura 3).

[![El control Editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Figura 03**: El control Editor HTML[(Haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-html-editor-control-cs/_static/image6.png)

Después de arrastrar el Editor HTML a una página, puede establecer sus propiedades en la hoja de propiedades. Por ejemplo, normalmente desea establecer las propiedades Anchura y Altura. El listado 1 contiene el origen de una página de ASP.NET que contiene un editor HTML.

**Listado 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

La página del listado 1 contiene un control Editor HTML, un control Button y un control Literal. Al hacer clic en el botón, el contenido del Editor HTML aparece en el control Literal (consulte la figura 4).

[![Envío de un formulario con un editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Figura 04**: Envío de un formulario con un editor HTML[(haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-html-editor-control-cs/_static/image8.png)

La propiedad Contenido del editor HTML se utiliza para recuperar el contenido HTML introducido en el Editor HTML. Tenga en cuenta que este contenido HTML puede contener JavaScript. En la siguiente sección, analizamos cómo puede evitar los ataques de inyección de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalización de la barra de herramientas del editor HTML

Puede personalizar exactamente qué botones aparecen en el editor. Por ejemplo, es posible que desee quitar la pestaña HTML para evitar que los usuarios cambien el Editor HTML al modo HTML. O bien, es posible que desee quitar la lista desplegable de tamaño de fuente para evitar que los usuarios creen texto demasiado grande en una publicación de mensaje del foro (consulte la figura 5).

[![Un editor HTML personalizado](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Figura 05**: Un editor HTML personalizado[(haga clic para ver la imagen de tamaño completo)](how-do-i-use-the-html-editor-control-cs/_static/image10.png)

Puede personalizar los botones de la barra de herramientas derivando un nuevo editor HTML de la clase Editor base. Por ejemplo, el editor personalizado del listado 2 solo contiene botones de barra de herramientas para negrita y cursiva. Se han eliminado todos los demás botones de la barra de herramientas. Además, la pestaña HTML se ha eliminado de la parte inferior del editor (pero las pestañas Diseño y Vista previa siguen ahí).

**Listado 2\_- Código de la aplicación CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Debe agregar la clase en el\_listado 2 a la carpeta de código de aplicación para que la clase se compile automáticamente. Si la\_carpeta Código de aplicación no existe en su sitio web, simplemente puede agregar la carpeta.

Después de crear un editor personalizado, puede agregarlo a una página de ASP.NET de la misma manera que agrega el editor HTML normal (consulte el listado 3).

**Listado 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitar ataques de secuencias de comandos entre sitios (XSS)

Siempre que acepte la entrada de un usuario y vuelva a mostrares esa entrada en su sitio web, es posible que abra su sitio web en ataques de secuencias de comandos entre sitios (XSS). En teoría, un hacker malintencionado podría enviar código JavaScript que se ejecuta cuando se vuelve a mostrar la entrada. El JavaScript podría ser utilizado para robar contraseñas de usuario u otra información confidencial.

Normalmente, puede derrotar los ataques XSS codificando cualquier entrada que recupere de un usuario antes de mostrarla en una página web. Sin embargo, la codificación HTML de &lt;la&gt; salida del Editor HTML no solo codificaría las etiquetas de script, sino que también codificaría todas las etiquetas HTML. En otras palabras, perdería todo el formato, como el tipo de fuente, el tamaño de fuente y el color de fondo.

Si está recopilando información confidencial de los usuarios, como contraseñas, números de tarjetas de crédito y números de seguridad social, no debe mostrar contenido no codificado que recupere de un usuario con el Editor HTML. Debe utilizar el Editor HTML solo en situaciones en las que no vuelva a mostrar el contenido HTML o el contenido HTML se envía a su sitio web por una parte de confianza.

Imagine, por ejemplo, que está creando una aplicación de blog. En esta situación, tiene sentido utilizar el Editor HTML al componer entradas de blog. Usted es el único que envía una entrada de blog y, presumiblemente, puede confiar en sí mismo para no enviar JavaScript malicioso. Sin embargo, no tiene sentido utilizar el Editor HTML al permitir que los usuarios anónimos publiquen comentarios. Debe tener especial cuidado en situaciones en las que los usuarios envían información confidencial, como contraseñas. Potencialmente, un usuario malintencionado podría publicar un comentario que contenga el JavaScript correcto para robar una contraseña.

## <a name="summary"></a>Resumen

En este tutorial, se le proporcionó una breve descripción general del control Editor HTML incluido en el Kit de herramientas de control de AJAX. Ha aprendido a usar el Editor HTML para aceptar contenido enriquecido de un usuario y enviar el contenido al servidor. También hemos explicado cómo puede personalizar los botones de la barra de herramientas que muestra el Editor HTML. Por último, aprendió a evitar ataques de secuencias de comandos entre sitios al usar el Editor HTML para aceptar entradas potencialmente malintencionadas.

> [!div class="step-by-step"]
> [Siguiente](how-do-i-use-the-html-editor-control-vb.md)
