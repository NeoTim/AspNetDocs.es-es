---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Crear casillas de verificación mutuamente excluyentes (VB) | Microsoft Docs
author: wenz
description: 'Cuando se puede seleccionar solo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Sin embargo, hay un inconveniente: Una vez un botón de radio en un grupo se selecciona...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 45ea3c3dbcf7816f67081a61230c4b055a90fcf5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393631"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="ebe7a-104">Crear casillas de verificación mutuamente excluyentes (VB)</span><span class="sxs-lookup"><span data-stu-id="ebe7a-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="ebe7a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ebe7a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ebe7a-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ebe7a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="ebe7a-107">Cuando se puede seleccionar solo uno de un conjunto de opciones, normalmente se utilizan los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="ebe7a-108">Sin embargo, hay un inconveniente: Una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="ebe7a-109">Las casillas de verificación puede desactivarse en cualquier momento, sin embargo, no son mutuamente excluyentes.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="ebe7a-110">Este tutorial ofrece lo mejor de ambos enfoques: las casillas de verificación que se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="ebe7a-111">Información general</span><span class="sxs-lookup"><span data-stu-id="ebe7a-111">Overview</span></span>

<span data-ttu-id="ebe7a-112">Cuando se puede seleccionar solo uno de un conjunto de opciones, normalmente se utilizan los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="ebe7a-113">Sin embargo, hay un inconveniente: Una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="ebe7a-114">Las casillas de verificación puede desactivarse en cualquier momento, sin embargo, no son mutuamente excluyentes.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="ebe7a-115">Este tutorial ofrece lo mejor de ambos enfoques: las casillas de verificación que se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="ebe7a-116">Pasos</span><span class="sxs-lookup"><span data-stu-id="ebe7a-116">Steps</span></span>

<span data-ttu-id="ebe7a-117">ASP.NET AJAX Control Toolkit contiene los extensores MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="ebe7a-118">Esto permite a los programadores asignar cualquier casilla a un nombre de grupo (`Key` atributo).</span><span class="sxs-lookup"><span data-stu-id="ebe7a-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="ebe7a-119">En todas las casillas de verificación dentro del mismo grupo, sólo uno puede seleccionarse al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="ebe7a-120">Comencemos con la colocación de dos casillas de verificación en una nueva página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="ebe7a-121">Puede haber más, pero dos de ellos es suficiente para demostrar el principio:</span><span class="sxs-lookup"><span data-stu-id="ebe7a-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="ebe7a-122">Para ambas casillas de verificación, se debe colocar un control MutuallyExclusiveCheckBoxExtender en la página.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="ebe7a-123">Ambos atributos de clave deben tener el mismo valor, al igual que el valor de los atributos de elementos de botón de radio HTML deben ser idénticos para denotar el grupo al que pertenece.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="ebe7a-124">La propiedad TargetControlID del extensor apunta al identificador de la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="ebe7a-125">Por último, se incluyen ASP.NET AJAX `ScriptManager` que es necesario para todos los elementos de ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="ebe7a-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="ebe7a-126">Guarde y ejecute la página: Se puede activar y desactivar ambas casillas de verificación, pero en ningún momento ambas casillas de verificación se puede comprobar.</span><span class="sxs-lookup"><span data-stu-id="ebe7a-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


[![O<span data-ttu-id="ebe7a-127">una casilla de verificación solo se puede comprobar a la vez]</span><span class="sxs-lookup"><span data-stu-id="ebe7a-127">nly one checkbox can be checked at a time]</span></span>(creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

<span data-ttu-id="ebe7a-128">Se puede comprobar solo una casilla a la vez ([haga clic aquí para ver imagen en tamaño completo](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ebe7a-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ebe7a-129">Anterior</span><span class="sxs-lookup"><span data-stu-id="ebe7a-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
