---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Creación de un valor numérico arriba/abajo el Control con un back-end de Web Service (VB) | Microsoft Docs
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla de verificación, una numérica arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar c como más...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bf69b3e6932df528e8ccd2348ffa6f13132f99f8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029492"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Crear un control NumericUpDown con un back-end de servicio web (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar tan más cómodo. De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.


## <a name="overview"></a>Información general

En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar tan más cómodo. De forma predeterminada, el `NumericUpDown` control siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.

## <a name="steps"></a>Pasos

ASP.NET AJAX Control Toolkit contiene el `NumericUpDown` extensor que agrega automáticamente dos botones a un cuadro de texto: Uno para aumentar su valor, uno para reducirlo. Sin embargo, el control también admite una llamada al servicio web (o una llamada al método de página). Cada vez que el arriba o hacia abajo del botón se hace clic en, el código de JavaScript se conecta al servidor web de código y ejecuta un método no existe. La firma del método es el siguiente:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

El `current` argumento es el valor actual en el cuadro de texto; el `tag` atributo es los datos de contexto adicional que pueden establecerse como una propiedad de la `NumericUpDown` extensor (pero no es necesario).

Para este ejemplo, el numérica arriba/abajo el control solo permitirá que los valores que son potencias de dos: 1, 2, 4, 8, 16, 32, 64 y así sucesivamente. Por lo tanto, el método que se ejecuta cuando el usuario desea aumentar el valor debe duplicar el valor antiguo; el otro método debe dividir el valor por dos. Así que aquí es el servicio web completa:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Por último, cree una nueva página ASP.NET. Como es habitual, necesita un `ScriptManager` (control), un `TextBox` control y un `NumericUpDownExtender` control. Por último, tendrá que proporcionar la información del servicio web:

- `ServiceDownMethod` nombre de la profundidad método web o método de página
- `ServiceDownPath` ruta de acceso al servicio web con el método de servicio fuera de servicio; Omitir si está utilizando un método de página
- `ServiceUpMethod` nombre de la copia de método web o método de página
- `ServiceUpPath` ruta de acceso al servicio web con el método de servicio arriba; Omitir si está utilizando un método de página

Este es el marcado completo para la página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Si ejecuta la página, observe cómo el valor en el cuadro de texto siempre duplica cuando haga clic en el botón superior y se reduce a la mitad al hacer clic en el botón inferior.


[![Aparecen únicamente los números que sean la potencia de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Aparecen únicamente los números que sean la potencia de 2 ([haga clic aquí para ver imagen en tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
