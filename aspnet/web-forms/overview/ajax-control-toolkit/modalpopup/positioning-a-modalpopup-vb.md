---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Colocar un ModalPopup (VB) | Microsoft Docs
author: wenz
description: El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo el control no ofrece un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: e37d2f4450c697f963d954c2fbb58e3ed20a1566
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59421152"
---
# <a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="e88df-104">Colocar un ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="e88df-104">Positioning a ModalPopup (VB)</span></span>

<span data-ttu-id="e88df-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e88df-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e88df-106">[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e88df-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="e88df-107">El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="e88df-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e88df-108">Sin embargo el control no ofrece una funcionalidad integrada para colocar el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="e88df-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="e88df-109">Información general</span><span class="sxs-lookup"><span data-stu-id="e88df-109">Overview</span></span>

<span data-ttu-id="e88df-110">El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="e88df-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e88df-111">Sin embargo el control no ofrece una funcionalidad integrada para colocar el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="e88df-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="e88df-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="e88df-112">Steps</span></span>

<span data-ttu-id="e88df-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="e88df-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="e88df-114">control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="e88df-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="e88df-115">A continuación, agregue un panel que actúa como el elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="e88df-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="e88df-116">Se utiliza un botón para cerrar la ventana emergente:</span><span class="sxs-lookup"><span data-stu-id="e88df-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="e88df-117">Cada vez que se muestra la ventana emergente, deberá se coloca en un lugar determinado en la página.</span><span class="sxs-lookup"><span data-stu-id="e88df-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="e88df-118">Para esta tarea, se crea una función de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="e88df-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="e88df-119">En primer lugar intenta tener acceso al panel.</span><span class="sxs-lookup"><span data-stu-id="e88df-119">It first tries to access the panel.</span></span> <span data-ttu-id="e88df-120">Si se realiza correctamente, la posición del panel se establece con CSS y JavaScript (cambiar la posición del elemento emergente en le).</span><span class="sxs-lookup"><span data-stu-id="e88df-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="e88df-121">Sin embargo, el `ModalPopupExtender` control también intenta colocar la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="e88df-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="e88df-122">Por lo tanto, el código JavaScript repetidamente coloca la ventana emergente, cada décima de segundo.</span><span class="sxs-lookup"><span data-stu-id="e88df-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="e88df-123">Como puede ver, el valor devuelto de la `setTimeout()` método JavaScript se guarda en una variable global.</span><span class="sxs-lookup"><span data-stu-id="e88df-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="e88df-124">Esto permite detener el posicionamiento repetidas de la ventana emergente a petición, utilizando el `clearTimeout()` método:</span><span class="sxs-lookup"><span data-stu-id="e88df-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="e88df-125">Ahora todo lo que queda por hacer es hacer que el explorador llamar a estas funciones siempre que sea apropiado.</span><span class="sxs-lookup"><span data-stu-id="e88df-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="e88df-126">El `movePanel()` se debe llamar la función de JavaScript cuando se presiona el botón que desencadena el panel:</span><span class="sxs-lookup"><span data-stu-id="e88df-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="e88df-127">Y el `stopMoving()` función entra en juego cuando se cierra el menú emergente puede activarse en el `ModalPopupExtender` control:</span><span class="sxs-lookup"><span data-stu-id="e88df-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="e88df-128">[![El elemento emergente modal aparece en la posición designada](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e88df-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="e88df-129">El elemento emergente modal aparece en la posición designada ([haga clic aquí para ver imagen en tamaño completo](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e88df-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e88df-130">Anterior</span><span class="sxs-lookup"><span data-stu-id="e88df-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
