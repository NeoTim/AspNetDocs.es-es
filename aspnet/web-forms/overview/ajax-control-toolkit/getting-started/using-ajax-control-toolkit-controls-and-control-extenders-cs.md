---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Uso de los controles del kit de herramientas de control de AJAX y los extensores de control (C- Microsoft Docs
author: rick-anderson
description: Aprenda a agregar controles y extensores de AJAX Control Toolkit a sus páginas de ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 5729db63f831b74ca37c573791c53c39265c1f39
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543722"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Usar los controles de AJAX Control Toolkit y los controles extensores (C#)

por [Microsoft](https://github.com/microsoft)

> Aprenda a agregar controles y extensores de AJAX Control Toolkit a sus páginas de ASP.NET.

El kit de herramientas de control de AJAX contiene un conjunto de controles y extensores de control. En este breve tutorial, aprenderá a agregar controles y extensores de control a una página de ASP.NET.

> [!NOTE] 
> 
> Para obtener instrucciones sobre cómo instalar el Kit de herramientas de control de AJAX y agregar el Kit de herramientas de control de AJAX a la caja de herramientas de Visual Studio/Visual Web Developer, consulte el tutorial [Introducción al Kit](get-started-with-the-ajax-control-toolkit-cs.md)de herramientas de control de AJAX .

## <a name="using-ajax-control-toolkit-controls"></a>Uso de controles de AJAX Control Toolkit

Un control ajax Control Toolkit funciona igual que un control de ASP.NET normal. Puede arrastrar el control desde el cuadro de herramientas a una página ASP.NET. Puede agregar el control a la página en la vista Diseño o en la vista Origen.

Hay un requisito especial cuando se utilizan los controles del Kit de herramientas de control de AJAX. La página debe contener un control ScriptManager. El control ScriptManager es responsable de incluir todos los JavaScript necesarios requeridos por los controles de AJAX Control Toolkit.

Por ejemplo, la pestaña Ajax Control Toolkit incluye un control denominado el control Editor. Este control muestra un editor HTML enriquecido. Siga estos pasos para agregar el control Editor a una página:

1. Cree una nueva página de ASP.NET denominada ShowEditor.aspx
2. Seleccione el control ScriptManager en la pestaña Extensiones de AJAX en el cuadro de herramientas y arrastre el control a la página.
3. Seleccione el control Editor desde debajo de la pestaña AJAX Control Toolkit en el cuadro de herramientas y arrastre el control a la página (consulte la figura 1). El diseñador debe parecerse a la figura 2.
4. Ejecute el sitio web seleccionando la opción de menú **Depurar, Iniciar depuración** o pulsando la tecla F5.
5. Debería ver la página en la Figura 3.

[![Selección del control Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figura 01**: Selección del control Editor HTML[(Haga clic para ver la imagen de tamaño completo)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png)

[![Visual Studio Designer con ScriptManager y control Edit](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figura 02**: Diseñador de Visual Studio con ScriptManager y control Editar[(Haga clic para ver la imagen de tamaño completo)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png)

[![El DisplayEditor.aspx página](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figura 03**: El DisplayEditor.aspx[página(Haga clic para ver la imagen de tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Uso de extensores de control de AJAX

El Kit de herramientas de control de AJAX también contiene extensores de control. Como su nombre indica, un extensor de control extiende la funcionalidad de un control existente. Por ejemplo, el extensor de control ConfirmButton extiende el control Button estándar ASP.NET. El extensor cambia el Button control s comportamiento para que el botón muestra un cuadro de diálogo de confirmación al hacer clic en él.

Un extensor de control, al igual que un control AJAX Control Toolkit, requiere un control ScriptManager. Debe agregar un control ScriptManager a una página antes de empezar a usar extensores de control en la página.

Siga estos pasos para utilizar el extensor de control ConfirmButton:

1. Cree una nueva página de ASP.NET denominada ShowConfirmButton.aspx
2. Agregue un control ScriptManager a la página arrastrando el control a la página desde debajo de la pestaña Extensiones de AJAX.
3. Agregue un control Button estándar a la página arrastrando el botón desde debajo de la pestaña Estándar de la caja de herramientas hasta la superficie de Designer.
4. Haga clic la opción de la tarea **del extensor** del agregar (véase el cuadro 4).
5. En el cuadro de diálogo Elegir extensor, seleccione ConfirmButtonExtender (consulte la figura 5) y haga clic en el botón Aceptar.
6. Seleccione el Button control en el diseñador y\_expanda el extensores, Button1 ConfirmButtonExtender nodo en el propiedades ventana (consulte la figura 6). Asigne el valor *Really?* a la propiedad ConfirmText.
7. Ejecute la página seleccionando la opción de menú **Depurar, Iniciar depuración** o pulse la tecla F5.

[![La opción de tarea Agregar extensor](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figura 04**: La opción de tarea Agregar extensor[(Haga clic para ver la imagen de tamaño completo)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png)

[![Selección del extensor de control ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figura 05**: Selección del extensor de control ConfirmButton([Haga clic para ver la imagen de tamaño completo)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png)

[![Establecer una propiedad ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figura 06**: Establecer una propiedad ConfirmButton[(Haga clic para ver la imagen de tamaño completo)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png)

Cuando se abra la página, debería ver un botón. Al hacer clic en el botón, obtendrá el cuadro de diálogo de confirmación en la Figura 7.

[![Visualización del cuadro de diálogo de confirmación](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figura 07**: Visualización del cuadro de diálogo de confirmación[(Haga clic para ver la imagen de tamaño completo)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png)

Observe que normalmente no arrastra un extensor de control a una página. En su lugar, utilice la opción de tarea **Agregar extensor** para agregar un extensor a un control que ya haya agregado a una página. Observe, además, que se establecen las propiedades del extensor de control abriendo la hoja de propiedades para el control que se va a extender.

Un único control de ASP.NET puede ampliarse mediante varios extensores de control. La hoja de propiedades del control que se está ampliando enumerará todos los extensores de control asociados con el control.

> [!div class="step-by-step"]
> [Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Siguiente](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
