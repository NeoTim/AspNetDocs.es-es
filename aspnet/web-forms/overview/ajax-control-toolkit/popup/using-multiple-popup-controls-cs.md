---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Usar varios controles emergentes (C#) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. También es posible usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cf7f929b696240e6805a74fb576ad56738491fa
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035422"
---
<a name="using-multiple-popup-controls-c"></a>Usar varios controles emergentes (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. También es posible usar más de un control emergente en una sola página.


## <a name="overview"></a>Información general

El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. También es posible usar más de un control emergente en una sola página.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente. En el escenario actual, el panel contiene un `Calendar` control. Para evitar la actualización de la página causados por las devoluciones de datos del calendario, el panel se coloca dentro de un `UpdatePanel` control:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

La página también contiene dos cuadros de texto. Para cada cuadro de texto, el calendario emergente debe aparecer una vez que se activa el cuadro de texto.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Ahora ampliar cada uno de los dos cuadros de texto con un `PopupControlExtender`. El `TargetControlID` atributo proporciona el identificador del control asociado el extensor puede aceptar. El `PopupControlID` atributo contiene el Id. del panel emergente. En este caso, ambos extensores muestran el mismo panel, pero también son posibles, paneles diferentes.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Ahora al hacer clic dentro de un campo de texto, un calendario aparece debajo del campo, que le permite seleccionar una fecha. (Obtener la fecha seleccionada en los cuadros de texto se tratarán en otro tutorial.)


[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
