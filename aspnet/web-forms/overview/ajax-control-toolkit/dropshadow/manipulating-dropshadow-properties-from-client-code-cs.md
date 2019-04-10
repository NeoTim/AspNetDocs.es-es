---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipular propiedades DropShadow desde el código de cliente (C#) | Microsoft Docs
author: wenz
description: Personalizar la interfaz de edición de DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 3bf4b8fe85780135c821fbb7fcceefd326dce656
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381347"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipular propiedades DropShadow desde el código de cliente (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. También se pueden cambiar las propiedades de este extensor con el código de JavaScript de cliente.


## <a name="overview"></a>Información general

El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. También se pueden cambiar las propiedades de este extensor con el código de JavaScript de cliente.

## <a name="steps"></a>Pasos

El código comienza con un panel que contiene algunas líneas de texto:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

La clase CSS asociada le ofrece el panel de un color de fondo bueno:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

El `DropShadowExtender` se agrega para ampliar el panel con un efecto de sombra paralela, establecido en 50% de opacidad:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

A continuación, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Otro panel contiene dos vínculos de JavaScript para establecer la opacidad de la sombra paralela: el vínculo menos reduce la opacidad de la sombra, aumenta el vínculo del signo más.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

La función de JavaScript `changeOpacity()` , a continuación, debe buscar el `DropShadowExtender` control en la página. ASP.NET AJAX se define la `$find()` método para exactamente de esa tarea. A continuación, la `get_Opacity()` método recupera la opacidad actual, el `set_Opacity()` método establece esta propiedad. El código de JavaScript, a continuación, coloca el valor de opacidad actual en el `<label>` elemento:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Tse cambia la opacidad en el lado cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Se cambia la opacidad del lado cliente ([haga clic aquí para ver imagen en tamaño completo](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Siguiente](adjusting-the-z-index-of-a-dropshadow-vb.md)
