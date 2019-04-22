---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Mostrar detalles del elemento | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59389055"
---
# <a name="display-item-details"></a><span data-ttu-id="9e9d6-102">Mostrar detalles del elemento</span><span class="sxs-lookup"><span data-stu-id="9e9d6-102">Display Item Details</span></span>

<span data-ttu-id="9e9d6-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9e9d6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9e9d6-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="9e9d6-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="9e9d6-105">En esta sección, agregará la capacidad de ver los detalles de cada libro.</span><span class="sxs-lookup"><span data-stu-id="9e9d6-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="9e9d6-106">En app.js, agregue el código siguiente en el modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="9e9d6-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="9e9d6-107">En Views/Home/Index.cshtml, agregue un elemento de enlace de datos para el vínculo de detalles:</span><span class="sxs-lookup"><span data-stu-id="9e9d6-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="9e9d6-108">Esto enlaza el controlador de clics para el &lt;un&gt; elemento a la `getBookDetail` función en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="9e9d6-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="9e9d6-109">En el mismo archivo, reemplace la marca de seguridad siguiente:</span><span class="sxs-lookup"><span data-stu-id="9e9d6-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="9e9d6-110">por este:</span><span class="sxs-lookup"><span data-stu-id="9e9d6-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="9e9d6-111">Este marcado crea una tabla que está enlazado a datos a las propiedades de la `detail` perceptible en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="9e9d6-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="9e9d6-112">El "&lt;!--ko--&gt; &quot; sintaxis permite incluir un enlace de Knockout fuera de un elemento de DOM.</span><span class="sxs-lookup"><span data-stu-id="9e9d6-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="9e9d6-113">En este caso, el `if` enlace hace que esta sección de marcado que se mostrará solo cuando `details` es distinto de null.</span><span class="sxs-lookup"><span data-stu-id="9e9d6-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="9e9d6-114">Ahora, si ejecuta la aplicación y haga clic en uno de los &quot;detalle&quot; vínculos, la aplicación mostrará los detalles del libro.</span><span class="sxs-lookup"><span data-stu-id="9e9d6-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="9e9d6-115">[Anterior](part-7.md)
> [Siguiente](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="9e9d6-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
