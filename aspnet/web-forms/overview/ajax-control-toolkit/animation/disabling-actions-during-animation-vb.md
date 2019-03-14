---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deshabilitar las acciones durante la animación (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También admite la acción...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 811e1d75f79885f3f4c561d9211fec625fcf1807
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032782"
---
<a name="disabling-actions-during-animation-vb"></a>Deshabilitar las acciones durante la animación (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También es compatible con las acciones, como clics del mouse. Sin embargo cuando un clic del mouse inicia una animación, es deseable deshabilitar los clics del mouse durante la animación.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También es compatible con las acciones, como clics del mouse. Sin embargo cuando un clic del mouse inicia una animación, es deseable deshabilitar los clics del mouse durante la animación.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

La animación se aplicarán a un botón HTML similar al siguiente:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Tenga en cuenta que, puesto que deseamos que el botón para crear un postback; se usa un HTML Control en lugar de un Control Web solo deberá iniciar la animación del lado cliente para nosotros.

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Dentro de la `<Animations>` nodo, `<OnClick>` es el elemento correcto para controlar el clic del mouse. Sin embargo, se puede hacer clic en el botón durante la animación, también. El `<EnableAction>` elemento puede ocuparse de eso. Establecer `Enabled="false"` deshabilita el botón como parte de la animación. Puesto que estamos usando varias animaciones individuales (deshabilitar el botón y las animaciones reales), el `<Parallel>` elemento tiene que pegar las animaciones juntos en una sola. Aquí está el marcado completo para `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

También sería posible volver a habilitar al botón después de la animación, mediante el siguiente elemento XML al final de la lista:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Sin embargo en un escenario determinado sería inútil desde el botón se atenúa y no está visible al final de la animación.


[![El botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic aquí para ver imagen en tamaño completo](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-vb.md)
> [Siguiente](triggering-an-animation-in-another-control-vb.md)
