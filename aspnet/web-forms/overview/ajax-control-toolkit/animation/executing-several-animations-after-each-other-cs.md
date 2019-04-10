---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Ejecutar varias animaciones una tras otra (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 644af2485c1a51f2de209e968ba1b3475350fa47
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59394073"
---
# <a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="2a58f-104">Ejecutar varias animaciones una tras otra (C#)</span><span class="sxs-lookup"><span data-stu-id="2a58f-104">Executing Several Animations after Each Other (C#)</span></span>

<span data-ttu-id="2a58f-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2a58f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2a58f-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2a58f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="2a58f-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="2a58f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2a58f-108">Permite ejecutar varias animaciones una tras otra.</span><span class="sxs-lookup"><span data-stu-id="2a58f-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="2a58f-109">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="2a58f-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2a58f-110">Permite ejecutar varias animaciones una tras otra.</span><span class="sxs-lookup"><span data-stu-id="2a58f-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="2a58f-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="2a58f-111">Steps</span></span>

<span data-ttu-id="2a58f-112">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="2a58f-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="2a58f-113">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="2a58f-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="2a58f-114">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="2a58f-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="2a58f-115">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria</span><span class="sxs-lookup"><span data-stu-id="2a58f-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory</span></span> `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="2a58f-116">Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="2a58f-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="2a58f-117">Por lo general, `<OnLoad>` solo acepta una animación.</span><span class="sxs-lookup"><span data-stu-id="2a58f-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="2a58f-118">El marco de animación le permite unir varias animaciones en uno solo mediante la `<Sequence>` elemento.</span><span class="sxs-lookup"><span data-stu-id="2a58f-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="2a58f-119">Todas las animaciones en `<Sequence>` son ejecutada uno tras otro.</span><span class="sxs-lookup"><span data-stu-id="2a58f-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="2a58f-120">Aquí está el un marcado posible para el `AnimationExtender` control realizar primero la más amplia de panel y, a continuación, reduce su alto:</span><span class="sxs-lookup"><span data-stu-id="2a58f-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="2a58f-121">Al ejecutar este script, el panel de primera obtiene más anchos y, a continuación, más pequeños.</span><span class="sxs-lookup"><span data-stu-id="2a58f-121">When you run this script, the panel first gets wider and then smaller.</span></span>


[![F<span data-ttu-id="2a58f-122">imer que se aumenta el ancho]</span><span class="sxs-lookup"><span data-stu-id="2a58f-122">irst the width is increased]</span></span>(executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

<span data-ttu-id="2a58f-123">En primer lugar se aumenta el ancho ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2a58f-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


[![T<span data-ttu-id="2a58f-124">uando que se reduce el alto]</span><span class="sxs-lookup"><span data-stu-id="2a58f-124">hen the height is decreased]</span></span>(executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

<span data-ttu-id="2a58f-125">A continuación, se reduce el alto ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2a58f-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a58f-126">[Anterior](executing-several-animations-at-the-same-time-cs.md)
> [Siguiente](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2a58f-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
