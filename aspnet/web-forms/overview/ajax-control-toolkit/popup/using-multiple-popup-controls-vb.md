---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usar varios controles emergentes (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. También es posible usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: ee66e166d24bb80008671c84f6512d5c54707fcf
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59389822"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="2109f-104">Usar varios controles emergentes (VB)</span><span class="sxs-lookup"><span data-stu-id="2109f-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="2109f-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2109f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2109f-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2109f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="2109f-107">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="2109f-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="2109f-108">También es posible usar más de un control emergente en una sola página.</span><span class="sxs-lookup"><span data-stu-id="2109f-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="2109f-109">Información general</span><span class="sxs-lookup"><span data-stu-id="2109f-109">Overview</span></span>

<span data-ttu-id="2109f-110">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="2109f-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="2109f-111">También es posible usar más de un control emergente en una sola página.</span><span class="sxs-lookup"><span data-stu-id="2109f-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="2109f-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="2109f-112">Steps</span></span>

<span data-ttu-id="2109f-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="2109f-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="2109f-114">A continuación, agregue un panel que actúa como el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="2109f-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="2109f-115">En el escenario actual, el panel contiene un `Calendar` control.</span><span class="sxs-lookup"><span data-stu-id="2109f-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="2109f-116">Para evitar la actualización de la página causados por las devoluciones de datos del calendario, el panel se coloca dentro de un `UpdatePanel` control:</span><span class="sxs-lookup"><span data-stu-id="2109f-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="2109f-117">La página también contiene dos cuadros de texto.</span><span class="sxs-lookup"><span data-stu-id="2109f-117">The page also contains two text boxes.</span></span> <span data-ttu-id="2109f-118">Para cada cuadro de texto, el calendario emergente debe aparecer una vez que se activa el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="2109f-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="2109f-119">Ahora ampliar cada uno de los dos cuadros de texto con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="2109f-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="2109f-120">El `TargetControlID` atributo proporciona el identificador del control asociado el extensor puede aceptar.</span><span class="sxs-lookup"><span data-stu-id="2109f-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="2109f-121">El `PopupControlID` atributo contiene el Id. del panel emergente.</span><span class="sxs-lookup"><span data-stu-id="2109f-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="2109f-122">En este caso, ambos extensores muestran el mismo panel, pero también son posibles, paneles diferentes.</span><span class="sxs-lookup"><span data-stu-id="2109f-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="2109f-123">Ahora al hacer clic dentro de un campo de texto, un calendario aparece debajo del campo, que le permite seleccionar una fecha.</span><span class="sxs-lookup"><span data-stu-id="2109f-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="2109f-124">(Obtener la fecha seleccionada en los cuadros de texto se tratarán en otro tutorial.)</span><span class="sxs-lookup"><span data-stu-id="2109f-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


[![T<span data-ttu-id="2109f-125">él calendario aparece cuando el usuario hace clic en el cuadro de texto]</span><span class="sxs-lookup"><span data-stu-id="2109f-125">he Calendar appears when the user clicks into the textbox]</span></span>(using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

<span data-ttu-id="2109f-126">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2109f-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2109f-127">[Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Siguiente](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2109f-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
