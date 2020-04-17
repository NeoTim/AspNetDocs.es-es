---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Creación de aplicaciones auxiliares HTML personalizadas (VB) Microsoft Docs
author: rick-anderson
description: El objetivo de este tutorial es demostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar dentro de las vistas MVC. Aprovechando HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 52f16bf666edc9f1c95c01faf004e303fcb6a0b2
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542513"
---
# <a name="creating-custom-html-helpers-vb"></a>Crear asistentes de HTML personalizados (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> El objetivo de este tutorial es demostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar dentro de las vistas MVC. Aprovechando las aplicaciones auxiliares HTML, puede reducir la cantidad de tediosa escritura de etiquetas HTML que debe realizar para crear una página HTML estándar.

El objetivo de este tutorial es demostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar dentro de las vistas MVC. Aprovechando las aplicaciones auxiliares HTML, puede reducir la cantidad de tediosa escritura de etiquetas HTML que debe realizar para crear una página HTML estándar.

En la primera parte de este tutorial, describo algunas de las aplicaciones auxiliares HTML existentes incluidas con el marco de ASP.NET MVC. A continuación, describo dos métodos de creación de aplicaciones auxiliares HTML personalizadas: explico cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método compartido y mediante la creación de un método de extensión.

## <a name="understanding-html-helpers"></a>Comprender los ayudantes HTML

Una aplicación auxiliar HTML es solo un método que devuelve una cadena. La cadena puede representar cualquier tipo de contenido que desee. Por ejemplo, puede utilizar aplicaciones auxiliares HTML `<input>` para `<img>` representar etiquetas HTML estándar como HTML y etiquetas. También puede usar aplicaciones auxiliares HTML para representar contenido más complejo, como una franja de pestañas o una tabla HTML de datos de base de datos.

El marco de ASP.NET MVC incluye el siguiente conjunto de aplicaciones auxiliares HTML estándar (esto no es una lista completa):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Por ejemplo, considere el formulario en el listado 1. Este formulario se representa con la ayuda de dos de las aplicaciones auxiliares HTML estándar (consulte la figura 1). Este formulario `Html.BeginForm()` utiliza `Html.TextBox()` los métodos y Helper.

[![Página renderizada con aplicaciones auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Figura 01**: Página representada con aplicaciones auxiliares HTML[(Haga clic para ver la imagen de tamaño completo)](creating-custom-html-helpers-vb/_static/image3.png)

**Listado 1 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

El `Html.BeginForm()` método auxiliar se utiliza para `<form>` crear las etiquetas HTML de apertura y cierre. Observe que `Html.BeginForm()` se llama al método dentro de una instrucción using. La instrucción using `<form>` garantiza que la etiqueta se cierra al final del bloque using.

Si lo prefiere, en lugar de crear un bloque using, puede llamar `<form>` al método auxiliar Html.EndForm() para cerrar la etiqueta. Utilice cualquier enfoque para crear `<form>` una etiqueta de apertura y cierre que le parezca más intuitiva.

Los `Html.TextBox()` métodos auxiliares se utilizan `<input>` en el listado 1 para representar etiquetas HTML. Si selecciona ver origen en su navegador, verá el origen HTML en el listado 2. Observe que el origen contiene etiquetas HTML estándar.

> [!IMPORTANT]
> observe que `Html.TextBox()`la aplicación auxiliar `<%= %>` -HTML `<% %>` se representa con etiquetas en lugar de etiquetas. Si no incluye el signo igual, no se representa nada en el explorador.

El marco de ASP.NET MVC contiene un pequeño conjunto de aplicaciones auxiliares. Lo más probable es que necesite ampliar el marco MVC con aplicaciones auxiliares HTML personalizadas. En el resto de este tutorial, aprenderá dos métodos para crear aplicaciones auxiliares HTML personalizadas.

**Listado 2 –`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Creación de aplicaciones auxiliares HTML con métodos compartidos

La forma más fácil de crear una nueva aplicación auxiliar HTML es crear un método compartido que devuelva una cadena. Imagine, por ejemplo, que decide crear una nueva aplicación `<label>` auxiliar HTML que represente una etiqueta HTML. Puede utilizar la clase del listado `<label>`2 para representar un archivo .

**Listado 2 –`Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

No hay nada especial en la clase en el listado 2. El `Label()` método simplemente devuelve una cadena.

La vista de índice modificada `LabelHelper` en `<label>` el listado 3 utiliza la para representar etiquetas HTML. Observe que la `<%@ imports %>` vista incluye una directiva que importa el espacio de nombres Application1.Helpers.

**Listado 2 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Creación de aplicaciones auxiliares HTML con métodos de extensión

Si desea crear aplicaciones auxiliares HTML que funcionan igual que las aplicaciones auxiliares HTML estándar incluidas en el marco de ASP.NET MVC, debe crear métodos de extensión. Los métodos de extensión permiten agregar nuevos métodos a una clase existente. Al crear un método auxiliar HTML, `HtmlHelper` agregue nuevos métodos a la clase representada por la propiedad Html de una vista.

El módulo de Visual Basic en el `Label()` listado `HtmlHelper` 3 agrega un método de extensión denominado a la clase. Hay un par de cosas que usted debe notar acerca de este módulo. En primer lugar, observe que `<Extension()>` el módulo está decorado con el atributo. Para utilizar este atributo, debe `System.Runtime.CompilerServices` importar el espacio de nombres

En segundo lugar, observe `Label()` que el `HtmlHelper` primer parámetro del método representa la clase. El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.

**Listado 3 –`Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Visual Studio Intellisense como todos los demás métodos de una clase (consulte la figura 2). La única diferencia es que los métodos de extensión aparecen con un símbolo especial junto a ellos (un icono de una flecha hacia abajo).

[![Uso del método de extensión Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Figura 02**: Uso del método de extensión Html.Label() ( Haga clic para ver la[imagen de tamaño completo](creating-custom-html-helpers-vb/_static/image6.png))

La vista index modificada en el listado 4 utiliza el método &lt;&gt; de extensión Html.Label() para representar todas sus etiquetas de etiqueta.

**Listado 4 –`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Resumen

En este tutorial, aprendió dos métodos para crear aplicaciones auxiliares HTML personalizadas. En primer lugar, aprendió `Label()` a crear una aplicación auxiliar HTML personalizada mediante la creación de un método compartido que devuelve una cadena. A continuación, aprendió a `Label()` crear un método auxiliar HTML `HtmlHelper` personalizado mediante la creación de un método de extensión en la clase.

En este tutorial, me centré en la creación de un método de aplicación auxiliar HTML extremadamente simple. Tenga en cuenta que una aplicación auxiliar HTML puede ser tan complicada como desee. Puede crear aplicaciones auxiliares HTML que representen contenido enriquecido, como vistas de árbol, menús o tablas de datos de base de datos.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-views-overview-vb.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
