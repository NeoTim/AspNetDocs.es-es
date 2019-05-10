---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Seleccionar una animación de una lista (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. El marco de trabajo también permitir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: dd22d80775ebe3571fcf9d3225135766669ef85b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65128711"
---
# <a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="b4584-104">Seleccionar una animación de una lista (C#)</span><span class="sxs-lookup"><span data-stu-id="b4584-104">Picking One Animation Out Of a List (C#)</span></span>

<span data-ttu-id="b4584-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b4584-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b4584-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b4584-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="b4584-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="b4584-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b4584-108">El marco también permite al programador seleccionar una animación de una lista de animaciones, dependiendo de la evaluación de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b4584-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="b4584-109">Información general</span><span class="sxs-lookup"><span data-stu-id="b4584-109">Overview</span></span>

<span data-ttu-id="b4584-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="b4584-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b4584-111">El marco también permite al programador seleccionar una animación de una lista de animaciones, dependiendo de la evaluación de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b4584-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="b4584-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="b4584-112">Steps</span></span>

<span data-ttu-id="b4584-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="b4584-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="b4584-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="b4584-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="b4584-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="b4584-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="b4584-116">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="b4584-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="b4584-117">Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="b4584-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="b4584-118">En lugar de una de las animaciones regulares, el `<Case>` elemento entra en juego.</span><span class="sxs-lookup"><span data-stu-id="b4584-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="b4584-119">Se evalúa el valor de su atributo SelectScript; el valor devuelto debe ser numérico.</span><span class="sxs-lookup"><span data-stu-id="b4584-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="b4584-120">Según este número, una de las subanimaciones dentro de &lt;caso&gt; se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="b4584-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="b4584-121">Por ejemplo, si se evalúa como SelectScript a 2, el Kit de herramientas de Control se ejecuta la animación terceros dentro de &lt;caso&gt; (contando comienza en 0).</span><span class="sxs-lookup"><span data-stu-id="b4584-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="b4584-122">El marcado siguiente define las tres Subanimaciones: El ancho, el cambio de tamaño el alto y atenúa, el cambio de tamaño. El código de JavaScript (`Math.floor(3 * Math.random())`), a continuación, elige un número entre 0 y 2, por lo que se ejecuta una de las tres animaciones:</span><span class="sxs-lookup"><span data-stu-id="b4584-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

<span data-ttu-id="b4584-123">[![Una de las animaciones de tres posibles: El panel obtiene más amplio](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b4584-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="b4584-124">Una de las animaciones de tres posibles: El panel obtiene más amplio ([haga clic aquí para ver imagen en tamaño completo](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b4584-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b4584-125">[Anterior](animation-depending-on-a-condition-cs.md)
> [Siguiente](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b4584-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
