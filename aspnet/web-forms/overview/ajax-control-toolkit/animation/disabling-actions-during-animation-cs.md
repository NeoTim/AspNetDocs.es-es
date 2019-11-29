---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Deshabilitar acciones durante la animaciónC#() | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También admite la acción...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599721"
---
# <a name="disabling-actions-during-animation-c"></a><span data-ttu-id="b7deb-104">Deshabilitar las acciones durante la animación (C#)</span><span class="sxs-lookup"><span data-stu-id="b7deb-104">Disabling Actions during Animation (C#)</span></span>

<span data-ttu-id="b7deb-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b7deb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b7deb-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b7deb-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="b7deb-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="b7deb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b7deb-108">También admite acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="b7deb-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="b7deb-109">Sin embargo, cuando un clic del mouse inicia una animación, es conveniente deshabilitar los clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="b7deb-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="b7deb-110">Información general del</span><span class="sxs-lookup"><span data-stu-id="b7deb-110">Overview</span></span>

<span data-ttu-id="b7deb-111">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="b7deb-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b7deb-112">También admite acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="b7deb-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="b7deb-113">Sin embargo, cuando un clic del mouse inicia una animación, es conveniente deshabilitar los clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="b7deb-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="b7deb-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="b7deb-114">Steps</span></span>

<span data-ttu-id="b7deb-115">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="b7deb-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="b7deb-116">La animación se aplicará a un botón HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="b7deb-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="b7deb-117">Tenga en cuenta que se usa un control HTML en lugar de un control Web, ya que no se desea que el botón cree un postback; solo se iniciará la animación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="b7deb-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="b7deb-118">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:</span><span class="sxs-lookup"><span data-stu-id="b7deb-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="b7deb-119">Dentro del nodo `<Animations>`, `<OnClick>` es el elemento adecuado para controlar el clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="b7deb-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="b7deb-120">Sin embargo, también se puede hacer clic en el botón durante la animación.</span><span class="sxs-lookup"><span data-stu-id="b7deb-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="b7deb-121">El elemento `<EnableAction>` puede encargarse de ello.</span><span class="sxs-lookup"><span data-stu-id="b7deb-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="b7deb-122">Al establecer `Enabled="false"`, se deshabilita el botón como parte de la animación.</span><span class="sxs-lookup"><span data-stu-id="b7deb-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="b7deb-123">Dado que usamos varias animaciones individuales (deshabilitando el botón y las animaciones reales), el elemento `<Parallel>` es necesario para pegar las animaciones individuales en una sola.</span><span class="sxs-lookup"><span data-stu-id="b7deb-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="b7deb-124">Este es el marcado completo de `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="b7deb-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="b7deb-125">También sería posible volver a habilitar para el botón después de la animación, utilizando el siguiente elemento XML al final de la lista:</span><span class="sxs-lookup"><span data-stu-id="b7deb-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="b7deb-126">Sin embargo, en el escenario dado esto sería inútil, ya que el botón se atenúa y no está visible al final de la animación.</span><span class="sxs-lookup"><span data-stu-id="b7deb-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="b7deb-127">[![el botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b7deb-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="b7deb-128">El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic para ver la imagen de tamaño completo](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b7deb-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7deb-129">[Anterior](animating-in-response-to-user-interaction-cs.md)
> [Siguiente](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b7deb-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
