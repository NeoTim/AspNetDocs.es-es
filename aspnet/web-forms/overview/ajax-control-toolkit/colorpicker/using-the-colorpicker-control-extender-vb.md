---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Uso del extensor de control ColorPicker (VB) Microsoft Docs
author: rick-anderson
description: ColorPicker es un extensor de ASP.NET AJAX que proporciona funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control emergente. Se puede acoplar a cualquier ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: c373102665ac17020ca8d5f14d3488a874bb68de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542851"
---
# <a name="using-the-colorpicker-control-extender-vb"></a>Uso del extensor de control ColorPicker (VB)

por [Microsoft](https://github.com/microsoft)

> ColorPicker es un extensor de ASP.NET AJAX que proporciona funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control emergente. Se puede adjuntar a cualquier ASP.NET control TextBox. eso.

El objetivo de este tutorial es explicar cómo puede utilizar el extensor de control AJAX Control Toolkit ColorPicker. El extensor de control ColorPicker muestra un cuadro de diálogo emergente que le permite seleccionar un color. El ColorPicker es útil siempre que desee proporcionar una interfaz de usuario intuitiva para que un usuario elija un color.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Ampliación de un control TextBox con el extensor de control ColorPicker

Imagine, por ejemplo, que desea crear un sitio web que permita a los visitantes crear tarjetas de visita personalizadas. Los visitantes pueden introducir el texto de una tarjeta de visita y elegir el color. La página ASP.NET del listado 1 contiene dos controles TextBox denominados txtCardText y txtCardColor. Al enviar el formulario, se muestran los valores seleccionados (consulte la figura 1).

[![Formulario simple para crear una tarjeta de visita](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: Formulario simple para crear una tarjeta de visita ( Haga clic para ver la[imagen de tamaño completo](using-the-colorpicker-control-extender-vb/_static/image2.png))

**Listado 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

El formulario en el listado 1 funciona, pero no proporciona una gran experiencia de usuario. El usuario tiene que escribir un color en el cuadro de texto. Si el usuario quiere un color especializado - por ejemplo, sólo el tono correcto de verde guisante - entonces el usuario debe averiguar el código de color HTML sin ninguna ayuda.

Puede utilizar el extensor de control ColorPicker para crear una mejor experiencia de usuario. El ColorPicker muestra un cuadro de diálogo de color al mover el foco a un TextBox control (consulte la figura 2).

[![El extensor de control ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: El extensor de control ColorPicker[(Haga clic para ver la imagen de tamaño completo)](using-the-colorpicker-control-extender-vb/_static/image4.png)

Debe completar dos pasos para utilizar el extensor de control ColorPicker con el formulario en el listado 1:

1. Agregar un control ScriptManager a la página
2. Agregue el extensor de control ColorPicker a la página

Para poder usar ColorPicker, debe agregar un ScriptManager a la página. Un buen lugar para agregar el ScriptManager está &lt;&gt; justo debajo de la etiqueta de formulario del lado del servidor de apertura. Puede arrastrar el ScriptManager a la página desde el cuadro de herramientas (el ScriptManager se encuentra en la pestaña Extensiones de AJAX). Como alternativa, puede escribir la siguiente etiqueta en la vista de origen debajo de la etiqueta de formulario del lado del servidor de apertura:

&lt;asp:ScriptManager ID-"ScriptManager1" runat-"servidor" /&gt;

La forma más fácil de agregar el extensor de control ColorPicker a la página es en la vista Diseño. Si pasa el ratón sobre el cuadro de texto txtCardColor, aparece una opción de tarea inteligente que le permite agregar un extensor (consulte la figura 3). Si elige esta opción, aparece el Asistente para extensor (consulte la figura 4).

[![Adición de un extensor](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: Adición de un extensor[(Haga clic para ver la imagen de tamaño completo)](using-the-colorpicker-control-extender-vb/_static/image6.png)

[![Selección de un extensor de control con el Asistente de extensor](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: Selección de un extensor de control con el Asistente para extensor (Haga clic para ver la[imagen de tamaño completo)](using-the-colorpicker-control-extender-vb/_static/image8.png)

Puede elegir el extensor ColorPicker para ampliar el cuadro de texto txtCardColor con el extensor ColorPicker. Haga clic en Aceptar para cerrar el cuadro de diálogo.

Después de realizar estos cambios, el origen de la página se parece al listado 2.

**Listado 2 - CreateCard.aspx (con ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Observe que la página ahora contiene un Control ColorPickerExtender que aparece directamente debajo del control textBox txtCardColor . El control ColorPickerExtender extiende el control txtCardColor para que muestre un cuadro de diálogo de selector de color.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Uso de un botón para iniciar el cuadro de diálogo Selector de color

El extensor ColorPicker admite las siguientes propiedades:

- PopupButtonId: el identificador de un botón de la página que hace que aparezca el cuadro de diálogo del selector de color.
- PopupPosition: la posición, relativa al control de destino, del cuadro de diálogo del selector de color. Los valores posibles son Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right e Left (el valor predeterminado es BottomLeft).
- SampleControlId: el identificador de un control que muestra el color seleccionado.
- SelectedColor - El color inicial seleccionado por el ColorPicker.

Puede utilizar estas propiedades para personalizar cómo se muestra el cuadro de diálogo del selector de color y cómo se muestra el color seleccionado. La página del listado 3 ilustra cómo puede utilizar varias de estas propiedades.

**Listado 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La página del listado 3 incluye un botón Seleccionar color (consulte la figura 5). Al hacer clic en este botón, el cuadro de diálogo del selector de color aparece encima del cuadro de texto. Si selecciona un color en el cuadro de diálogo, el color seleccionado aparece como el color de fondo del control LblSample Label.

El ColorPicker PopupButtonID propiedad se utiliza para asociar el seleccionar color botón con el ColorPicker extensor. Cuando se proporciona un valor para el PopupButtonID propiedad, el cuadro de diálogo selector de color ya no aparece cuando el control de destino tiene el foco. Debe hacer clic en el botón para mostrar el cuadro de diálogo.

El SampleControlID propiedad se utiliza para asociar un control que muestra el color seleccionado con el ColorPicker. ColorPicker cambia el color de fondo de este control al color seleccionado actualmente.

[![Visualización del cuadro de diálogo del selector de color con un botón](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: Visualización del cuadro de diálogo del selector de color con un botón[(Haga clic para ver la imagen de tamaño completo)](using-the-colorpicker-control-extender-vb/_static/image10.png)

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a usar el extensor de control ColorPicker para mostrar un cuadro de diálogo de selector de color emergente. En primer lugar, examinamos cómo puede mostrar el cuadro de diálogo cuando el foco se mueve a un TextBox control. A continuación, aprendió a crear un botón que muestra el cuadro de diálogo del selector de color cuando se hace clic en el botón.

> [!div class="step-by-step"]
> [Anterior](using-the-colorpicker-control-extender-cs.md)
