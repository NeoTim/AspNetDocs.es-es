---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Controlar los postbacks desde un ModalPopup (VB) | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Se debe tener especial cuidado cuando un PDV...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504025"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="c9b4e-104">Controlar postbacks desde un ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="c9b4e-104">Handling Postbacks from a ModalPopup (VB)</span></span>

<span data-ttu-id="c9b4e-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c9b4e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c9b4e-106">[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c9b4e-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="c9b4e-107">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c9b4e-108">Se debe tener especial cuidado cuando se crea un postback desde el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="c9b4e-109">Información general</span><span class="sxs-lookup"><span data-stu-id="c9b4e-109">Overview</span></span>

<span data-ttu-id="c9b4e-110">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c9b4e-111">Se debe tener especial cuidado cuando se crea un postback desde el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="c9b4e-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="c9b4e-112">Steps</span></span>

<span data-ttu-id="c9b4e-113">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="c9b4e-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="c9b4e-114">A continuación, agregue un panel que actúe como elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="c9b4e-115">Allí, el usuario puede escribir un nombre y una dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="c9b4e-116">Se usa un botón para cerrar el elemento emergente y guardar la información.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="c9b4e-117">Tenga en cuenta que el atributo `OnClick` está establecido para que se produzca un postback al hacer clic en este botón:</span><span class="sxs-lookup"><span data-stu-id="c9b4e-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="c9b4e-118">La propia página consta de dos etiquetas para exactamente la misma información: nombre y dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="c9b4e-119">Se usa un botón para desencadenar el elemento emergente modal:</span><span class="sxs-lookup"><span data-stu-id="c9b4e-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="c9b4e-120">Para que aparezca el elemento emergente, agregue el control `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="c9b4e-121">Establezca el atributo `PopupControlID` en el identificador del panel y `TargetControlID` en el identificador del botón:</span><span class="sxs-lookup"><span data-stu-id="c9b4e-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="c9b4e-122">Ahora, siempre que se haga clic en el botón `Save` en el menú emergente modal, se ejecutará el método de `SaveData()` del servidor.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="c9b4e-123">Allí, puede guardar los datos especificados en un almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="c9b4e-124">Por simplicidad, los nuevos datos se generan en la etiqueta:</span><span class="sxs-lookup"><span data-stu-id="c9b4e-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="c9b4e-125">Además, los controles de cuadro de texto dentro del elemento emergente modal deben rellenarse con el nombre y el correo electrónico actuales.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="c9b4e-126">Sin embargo, esto solo es necesario cuando no se produce un postback.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="c9b4e-127">Si hay un postback, la característica ASP.NET ViewState rellenará automáticamente los cuadros de texto con los valores adecuados.</span><span class="sxs-lookup"><span data-stu-id="c9b4e-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

<span data-ttu-id="c9b4e-128">[![el elemento emergente modal produce un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9b4e-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="c9b4e-129">El elemento emergente modal produce un postback ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c9b4e-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9b4e-130">[Anterior](using-modalpopup-with-a-repeater-control-vb.md)
> [Siguiente](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c9b4e-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
