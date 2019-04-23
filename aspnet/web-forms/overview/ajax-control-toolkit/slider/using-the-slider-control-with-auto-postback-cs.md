---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Usar el Control deslizante con Postback automático (C#) | Microsoft Docs
author: wenz
description: El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante Autocontab...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: a5b858a05470caa244902afbb404adbb2e4761b7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59402731"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="bb83d-104">Usar el Control deslizante con Postback automático (C#)</span><span class="sxs-lookup"><span data-stu-id="bb83d-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="bb83d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bb83d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bb83d-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bb83d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="bb83d-107">El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse.</span><span class="sxs-lookup"><span data-stu-id="bb83d-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="bb83d-108">Es posible hacer que el control deslizante autopostback una vez que los cambios en su valor.</span><span class="sxs-lookup"><span data-stu-id="bb83d-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="bb83d-109">Información general</span><span class="sxs-lookup"><span data-stu-id="bb83d-109">Overview</span></span>

<span data-ttu-id="bb83d-110">El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse.</span><span class="sxs-lookup"><span data-stu-id="bb83d-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="bb83d-111">Es posible hacer que el control deslizante autopostback una vez que los cambios en su valor.</span><span class="sxs-lookup"><span data-stu-id="bb83d-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="bb83d-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="bb83d-112">Steps</span></span>

<span data-ttu-id="bb83d-113">Para hacer que el control deslizante postback automáticamente tras un cambio, dos cuadros de texto necesitan el atributo `AutoPostBack="true"`: El cuadro de texto que se convertirá en el control deslizante propio y el cuadro de texto que contiene la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="bb83d-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="bb83d-114">Este es el marcado necesario para:</span><span class="sxs-lookup"><span data-stu-id="bb83d-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="bb83d-115">El `SliderExtender` control de ASP.NET AJAX Control Toolkit asigna la funcionalidad del control deslizante a los dos cuadros de texto:</span><span class="sxs-lookup"><span data-stu-id="bb83d-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="bb83d-116">Un elemento de etiqueta adicional se usará después para informar al usuario de una devolución de datos:</span><span class="sxs-lookup"><span data-stu-id="bb83d-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="bb83d-117">Por último, el `ScriptManager` control de AJAX de ASP.NET carga el código JavaScript necesario para el Kit de herramientas de Control para que funcione:</span><span class="sxs-lookup"><span data-stu-id="bb83d-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="bb83d-118">Ahora el control deslizante está publicando. en el lado servidor, este evento se puede detectar y actúa sobre ella:</span><span class="sxs-lookup"><span data-stu-id="bb83d-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="bb83d-119">[![Moviendo el control deslizante desencadena una devolución de datos](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bb83d-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="bb83d-120">Moviendo el control deslizante activa una devolución ([haga clic aquí para ver imagen en tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bb83d-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="bb83d-121">[![Después, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="bb83d-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="bb83d-122">Después, la fecha de este cambio se escribe en la etiqueta ([haga clic aquí para ver imagen en tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bb83d-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bb83d-123">Siguiente</span><span class="sxs-lookup"><span data-stu-id="bb83d-123">Next</span></span>](databinding-the-slider-control-cs.md)
