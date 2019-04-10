---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Enlace de datos el Control deslizante (C#) | Microsoft Docs
author: wenz
description: El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible enlazar la sición actual...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f966869c106416886dfa4e9e3c2cf67ee5fe337
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59385818"
---
# <a name="databinding-the-slider-control-c"></a>Enlace de datos del control deslizante (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.


## <a name="overview"></a>Información general

El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

A continuación, agregue dos `TextBox` controles a la página. Uno se transformará en un control deslizante gráfico y el otro va a contener la posición del control deslizante.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

El siguiente paso es ya el paso final. El `SliderExtender` control de ASP.NET AJAX Control Toolkit hace que un control deslizante del primer cuadro de texto y el segundo cuadro de texto se actualiza automáticamente cuando el control deslizante colocar los cambios. En orden para que funcione, el `SliderExtender`del `TargetControlID` atributo debe establecerse en el identificador del primer cuadro de texto; el `BoundControlID` atributo debe establecerse en el identificador del segundo cuadro de texto.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: escribir un nuevo valor en el cuadro de texto actualiza la posición del control deslizante. Si realiza el segundo cuadro de texto de solo lectura, puede agregar una protección no segura para el campo de texto para que resulte más difícil para el usuario actualizar manualmente el valor allí.


[![Scuadro de texto y lider están sincronizadas](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Cuadro de texto y el control deslizante estén sincronizados ([haga clic aquí para ver imagen en tamaño completo](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-the-slider-control-with-auto-postback-cs.md)
> [Siguiente](using-the-slider-control-with-auto-postback-vb.md)
