---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Reutilizar la interfaz de usuario mediante páginas maestras y parciales ? Microsoft Docs
author: rick-anderson
description: En el paso 7 se examinan las formas en que podemos aplicar el "Principio SECO" dentro de nuestras plantillas de vista para eliminar la duplicación de código, utilizando plantillas de vista parcial y páginas maestras.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: f381c4424a9fa0718cd234beeb01ce41bc4ca61e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542604"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Reutilizar la interfaz de usuario con páginas maestras y parciales

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 7 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> En el paso 7 se examinan las formas en que podemos aplicar el "Principio SECO" dentro de nuestras plantillas de vista para eliminar la duplicación de código, utilizando plantillas de vista parcial y páginas maestras.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner Paso 7: Parciales y Páginas Maestras

Una de las filosofías de diseño ASP.NET MVC abraza es el principio "No repetirse" (comúnmente conocido como "DRY"). Un diseño DRY ayuda a eliminar la duplicación de código y lógica, lo que en última instancia hace que las aplicaciones sean más rápidas de crear y más fáciles de mantener.

Ya hemos visto el principio DRY aplicado en varios de nuestros escenarios NerdDinner. Algunos ejemplos: nuestra lógica de validación se implementa dentro de nuestra capa de modelo, lo que permite que se aplique en ambos escenarios de edición y creación en nuestro controlador; estamos reutilizando la plantilla de vista "NotFound" en los métodos de acción Editar, Detalles y Eliminar; estamos usando un patrón de nomenclatura de convención con nuestras plantillas de vista, lo que elimina la necesidad de especificar explícitamente el nombre cuando llamamos al método auxiliar View(); y estamos reutilizando la clase DinnerFormViewModel para escenarios de acción Editar y Crear.

Ahora veamos maneras en que podemos aplicar el "Principio DRY" dentro de nuestras plantillas de vista para eliminar la duplicación de código allí también.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Volver a visitar nuestras plantillas de edición y creación de vistas

Actualmente estamos usando dos plantillas de vista diferentes, "Edit.aspx" y "Create.aspx", para mostrar nuestra interfaz de usuario del formulario de cena. Una rápida comparación visual de ellos destaca lo similares que son. A continuación se muestra el aspecto del formulario de creación:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Y así es como se ve nuestro formulario "Editar":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

¿No hay mucha diferencia? Aparte del título y el texto del encabezado, el diseño del formulario y los controles de entrada son idénticos.

Si abrimos las plantillas de vista "Edit.aspx" y "Create.aspx", encontraremos que contienen el mismo diseño de formulario y código de control de entrada. Esta duplicación significa que terminamos teniendo que hacer cambios dos veces cada vez que introducimos o cambiamos una nueva propiedad Dinner, lo cual no es bueno.

### <a name="using-partial-view-templates"></a>Uso de plantillas de vista parcial

ASP.NET MVC admite la capacidad de definir plantillas de "vista parcial" que se pueden usar para encapsular la lógica de representación de vista para una subparte de una página. "Parciales" proporciona una manera útil de definir la lógica de representación de vista una vez y, a continuación, volver a usarla en varios lugares de una aplicación.

Para ayudar a "DRY-up" nuestra Edit.aspx y Create.aspx duplicación de plantilla de vista, podemos crear una plantilla de vista parcial denominada "DinnerForm.ascx" que encapsula el diseño del formulario y los elementos de entrada comunes a ambos. Haremos esto haciendo clic con el botón derecho en nuestro directorio&gt;/Views/Dinners y eligiendo el comando de menú "Agregar- Ver":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Esto mostrará el cuadro de diálogo "Agregar vista". Nombraremos la nueva vista que queremos crear "DinnerForm", seleccionaremos la casilla de verificación "Crear una vista parcial" dentro del cuadro de diálogo e indicaremos que le pasaremos una clase DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio creará una nueva plantilla de vista "DinnerForm.ascx" para nosotros dentro del directorio "Vistas-Cenas".

A continuación, podemos copiar/pegar el diseño de formulario duplicado / código de control de entrada de nuestras plantillas de vista Edit.aspx/ Create.aspx en nuestra nueva plantilla de vista parcial "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

A continuación, podemos actualizar nuestras plantillas de vista Editar y crear para llamar a la plantilla parcial DinnerForm y eliminar la duplicación de formularios. Podemos hacer esto llamando a Html.RenderPartial("DinnerForm") dentro de nuestras plantillas de vista:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Puede calificar explícitamente la ruta de acceso de la plantilla parcial que desee al llamar a Html.RenderPartial (por ejemplo: .Views/Dinners/DinnerForm.ascx"). En nuestro código anterior, sin embargo, estamos aprovechando el patrón de nomenclatura basado en convenciones dentro de ASP.NET MVC y simplemente especificando "DinnerForm" como el nombre del parcial que se va a representar. Cuando hacemos esto ASP.NET MVC buscará primero en el directorio de vistas basadas en convenciones (para DinnersController esto sería /Views/Dinners). Si no encuentra la plantilla parcial allí, la buscará en el directorio /Views/Shared.

Cuando se llama a Html.RenderPartial() con solo el nombre de la vista parcial, ASP.NET MVC pasará a la vista parcial los mismos objetos de diccionario Model y ViewData utilizados por la plantilla de vista que realiza la llamada. Como alternativa, hay versiones sobrecargadas de Html.RenderPartial() que le permiten pasar un objeto Model alternativo o un diccionario ViewData para que la vista parcial la utilice. Esto es útil para escenarios en los que solo desea pasar un subconjunto del modelo/modelo de vista completo.

| **Tema secundario: &lt;¿Por qué % %&gt; en lugar de &lt;% ?&gt;** |
| --- |
| Una de las cosas sutiles que podría haber notado &lt;con&gt; el código &lt;anterior es&gt; que estamos usando un bloque % % en lugar de un bloque % % al llamar a Html.RenderPartial(). &lt;Los&gt; bloques % % en ASP.NET indican que un desarrollador &lt;desea representar un&gt; valor especificado (por ejemplo: % - %" % representaría "Hola"). &lt;%&gt; % bloques en su lugar indican que el desarrollador desea ejecutar código y que &lt;cualquier salida representada dentro&gt;de ellos debe hacerse explícitamente (por ejemplo: % Response.Write("Hello") % . La razón por &lt;la&gt; que estamos usando un bloque % % con nuestro código Html.RenderPartial anterior es porque el método Html.RenderPartial() no devuelve una cadena y, en su lugar, genera el contenido directamente en la secuencia de salida de la plantilla de vista de llamada. Lo hace por razones de eficiencia de rendimiento y, al hacerlo, evita la necesidad de crear un objeto de cadena temporal (potencialmente muy grande). Esto reduce el uso de memoria y mejora el rendimiento general de la aplicación. Un error común al usar Html.RenderPartial() es olvidarse de agregar un punto y &lt;coma al final de la llamada cuando está dentro de un&gt; bloque %. Por ejemplo, este código provocará &lt;un error del compilador:&gt; % Html.RenderPartial("DinnerForm") % En su lugar necesita escribir: &lt;% Html.RenderPartial("DinnerForm"); %&gt; Esto &lt;se&gt; debe a que % % bloques son instrucciones de código independientes y cuando se utilizan instrucciones de código de C- debe terminarse con un punto y coma. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Uso de plantillas de vista parcial para aclarar el código

Hemos creado la plantilla de vista parcial "DinnerForm" para evitar duplicar la lógica de representación de vistas en varios lugares. Esta es la razón más común para crear plantillas de vista parcial.

A veces todavía tiene sentido crear vistas parciales incluso cuando solo se les llama en un solo lugar. Las plantillas de vista muy complicadas a menudo pueden ser mucho más fáciles de leer cuando su lógica de representación de vista se extrae y se particiona en una o más plantillas parciales bien nombradas.

Por ejemplo, considere el siguiente fragmento de código del archivo Site.master en nuestro proyecto (que examinaremos en breve). El código es relativamente sencillo de leer, en parte porque la lógica para mostrar un vínculo de inicio de sesión/cierre de sesión en la parte superior derecha de la pantalla se encapsula dentro del parcial "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Siempre que se encuentre confundido tratando de entender el marcado html/code dentro de una plantilla de vista, considere si no sería más claro si parte de él se extrajo y refactorizó en vistas parciales bien nombradas.

### <a name="master-pages"></a>Páginas maestras

Además de admitir vistas parciales, ASP.NET MVC también admite la capacidad de crear plantillas de "página maestra" que se pueden usar para definir el diseño común y html de nivel superior de un sitio. A continuación, se pueden agregar controles de marcador de posición de contenido a la página maestra para identificar regiones reemplazables que las vistas pueden reemplazar o "rellenar". Esto proporciona una manera muy eficaz (y DRY) para aplicar un diseño común a través de una aplicación.

De forma predeterminada, los nuevos ASP.NET MVC tienen una plantilla de página maestra agregada automáticamente a ellos. Esta página maestra se denomina "Site.master" y se encuentra dentro de la carpeta "Views":

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

El archivo Site.master predeterminado tiene el siguiente aspecto. Define el html exterior del sitio, junto con un menú para la navegación en la parte superior. Contiene dos controles de marcador de posición de contenido reemplazables: uno para el título y otro para el lugar donde se debe reemplazar el contenido principal de una página:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todas las plantillas de vista que hemos creado para nuestra aplicación NerdDinner ("Lista", "Detalles", "Editar", "Crear", "No Encontrado", etc.) se han basado en esta plantilla Site.master. Esto se indica a través del atributo "MasterPageFile" &lt;que se&gt; agregó de forma predeterminada a la directiva superior % - Page % cuando creamos nuestras vistas mediante el cuadro de diálogo "Agregar vista":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Esto significa que podemos cambiar el contenido de Site.master y hacer que los cambios se apliquen y usen automáticamente cuando representamos cualquiera de nuestras plantillas de vista.

Vamos a actualizar nuestra sección de encabezado Site.master para que el encabezado de nuestra aplicación sea "NerdDinner" en lugar de "Mi aplicación MVC". También vamos a actualizar nuestro menú de navegación para que la primera pestaña sea "Buscar una cena" (controlada por el método de acción Index() de HomeController), y agreguemos una nueva pestaña llamada "Host a Dinner" (controlada por el método de acción Create() de DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Cuando guardemos el archivo Site.master y actualicemos nuestro navegador, veremos que nuestros cambios de encabezado aparecen en todas las vistas de nuestra aplicación. Por ejemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Y con la URL */Dinners/Edit/[id]:*

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>siguiente paso

Los parciales y las páginas maestras proporcionan opciones muy flexibles que le permiten organizar vistas de forma limpia. Verá que le ayudan a evitar duplicar el contenido/código de la vista y a hacer que las plantillas de vista sean más fáciles de leer y mantener.

Ahora revisemos el escenario de catalogación que creamos anteriormente y habilitemos la compatibilidad con la paginación escalable.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Siguiente](implement-efficient-data-paging.md)
