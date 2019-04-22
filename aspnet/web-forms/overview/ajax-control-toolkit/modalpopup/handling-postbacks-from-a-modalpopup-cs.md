---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Controlar los Postbacks desde un ModalPopup (C#) | Microsoft Docs
author: wenz
description: El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener especial cuidado cuando un punto de venta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 54dd3bae21e661e0b17cab6a71f0df33b6712bcd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59388561"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a>Controlar postbacks desde un ModalPopup (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.


## <a name="overview"></a>Información general

El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente modal. Allí, el usuario puede escribir un nombre y una dirección de correo electrónico. Se utiliza un botón para cerrar el cuadro emergente y guardar la información. Tenga en cuenta que el `OnClick` atributo está establecido para que se produce un postback cuando se hace clic en este botón:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

La página en Sí consta de dos etiquetas para exactamente la misma información: dirección de correo electrónico y nombre. Se utiliza un botón para desencadenar el elemento emergente modal:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Para hacer que aparezca la ventana emergente, agregue el `ModalPopupExtender` control. Establecer el `PopupControlID` atributo ID del panel y `TargetControlID` para el identificador del botón:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Ahora cada vez que el `Save` dentro de la ventana emergente modal se hace clic en botón, el servidor `SaveData()` se ejecuta el método. Allí, puede guardar los datos introducidos en un almacén de datos. Por simplicidad, los nuevos datos solo se genera en la etiqueta:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Además, los controles de cuadro de texto dentro de la ventana emergente modal deben rellenarse con el nombre actual y el correo electrónico. Sin embargo esto solo es necesario cuando se produce ninguna devolución de datos. Si se produce un postback, la característica de viewstate ASP.NET rellenará automáticamente con los valores apropiados en los cuadros de texto.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![El elemento emergente modal provoca una devolución de datos](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

El elemento emergente modal produce un postback ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-modalpopup-with-a-repeater-control-cs.md)
> [Siguiente](positioning-a-modalpopup-cs.md)
