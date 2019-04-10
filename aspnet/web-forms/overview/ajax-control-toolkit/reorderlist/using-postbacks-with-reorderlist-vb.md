---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Usar Postbacks con ReorderList (VB) | Microsoft Docs
author: wenz
description: El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. Cada vez que se puede reordenar la lista, un pedido de compra...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 22c7eecb841ff67196d21e6efeeda63a3456c5cb
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59409088"
---
# <a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="1b290-104">Usar postbacks con ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="1b290-104">Using Postbacks with ReorderList (VB)</span></span>

<span data-ttu-id="1b290-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1b290-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1b290-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1b290-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="1b290-107">El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="1b290-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="1b290-108">Cada vez que se puede reordenar la lista, una devolución informará al servidor del cambio.</span><span class="sxs-lookup"><span data-stu-id="1b290-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="1b290-109">Información general</span><span class="sxs-lookup"><span data-stu-id="1b290-109">Overview</span></span>

<span data-ttu-id="1b290-110">El `ReorderList` control de AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="1b290-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="1b290-111">Cada vez que se puede reordenar la lista, una devolución informará al servidor del cambio.</span><span class="sxs-lookup"><span data-stu-id="1b290-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="1b290-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="1b290-112">Steps</span></span>

<span data-ttu-id="1b290-113">Hay varios orígenes de datos posibles para el `ReorderList` control.</span><span class="sxs-lookup"><span data-stu-id="1b290-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="1b290-114">Una consiste en usar un `XmlDataSource` control:</span><span class="sxs-lookup"><span data-stu-id="1b290-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="1b290-115">Para enlazar este XML a un `ReorderList` se deben establecer el control y habilitar las devoluciones de datos, los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="1b290-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- `DataSourceID`<span data-ttu-id="1b290-116">: El identificador del origen de datos</span><span class="sxs-lookup"><span data-stu-id="1b290-116">: The ID of the data source</span></span>
- `SortOrderField`<span data-ttu-id="1b290-117">: La propiedad para ordenar por</span><span class="sxs-lookup"><span data-stu-id="1b290-117">: The property to sort by</span></span>
- `AllowReorder`<span data-ttu-id="1b290-118">: Si se permite al usuario reordenar los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="1b290-118">: Whether to allow the user to reorder the list elements</span></span>
- `PostBackOnReorder`<span data-ttu-id="1b290-119">: Si desea crear una devolución de datos cada vez que se reorganiza la lista</span><span class="sxs-lookup"><span data-stu-id="1b290-119">: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="1b290-120">Este es el formato adecuado para el control:</span><span class="sxs-lookup"><span data-stu-id="1b290-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="1b290-121">Dentro de la `ReorderList` control, los datos específicos del origen de datos puede estar enlazado con el `Eval()` método:</span><span class="sxs-lookup"><span data-stu-id="1b290-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="1b290-122">En una posición arbitraria en la página, una etiqueta contendrá la información cuando ocurrió la última reordenación:</span><span class="sxs-lookup"><span data-stu-id="1b290-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="1b290-123">Esta etiqueta se rellena con texto en el código de servidor, que controla la devolución de datos:</span><span class="sxs-lookup"><span data-stu-id="1b290-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="1b290-124">Por último, para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en la página:</span><span class="sxs-lookup"><span data-stu-id="1b290-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![E<span data-ttu-id="1b290-125">reordenación ACH desencadena una devolución de datos]</span><span class="sxs-lookup"><span data-stu-id="1b290-125">ach reordering triggers a postback]</span></span>(using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

<span data-ttu-id="1b290-126">Cada reordenación desencadena una devolución de datos ([haga clic aquí para ver imagen en tamaño completo](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1b290-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1b290-127">[Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Siguiente](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1b290-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
