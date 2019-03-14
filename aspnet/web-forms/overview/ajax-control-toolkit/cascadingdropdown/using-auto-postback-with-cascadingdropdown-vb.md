---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Usar el Postback automático con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 31f24374714371f102a1e6bc7e8977713876b228
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055362"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="8f37c-103">Usar el postback automático con CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="8f37c-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="8f37c-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8f37c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8f37c-105">[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8f37c-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="8f37c-106">El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8f37c-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8f37c-107">Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de la red DropDownList no funciona, ya que se carga asincrónicamente datos en la lista genera una respuesta (innecesario) por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="8f37c-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="8f37c-108">Con algún código JavaScript, se puede evitar este efecto.</span><span class="sxs-lookup"><span data-stu-id="8f37c-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="8f37c-109">Información general</span><span class="sxs-lookup"><span data-stu-id="8f37c-109">Overview</span></span>

<span data-ttu-id="8f37c-110">El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8f37c-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8f37c-111">(Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de la red DropDownList no funciona, ya que se carga asincrónicamente datos en la lista genera una respuesta (innecesario) por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="8f37c-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="8f37c-112">Con algún código JavaScript, se puede evitar este efecto.</span><span class="sxs-lookup"><span data-stu-id="8f37c-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="8f37c-113">Pasos</span><span class="sxs-lookup"><span data-stu-id="8f37c-113">Steps</span></span>

<span data-ttu-id="8f37c-114">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del &lt; `form` &gt; elemento):</span><span class="sxs-lookup"><span data-stu-id="8f37c-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="8f37c-115">A continuación, se requiere un control de DropDownList:</span><span class="sxs-lookup"><span data-stu-id="8f37c-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="8f37c-116">Para esta lista, se agrega un extensor CascadingDropDown, que proporciona información de dirección URL y el método de servicio web:</span><span class="sxs-lookup"><span data-stu-id="8f37c-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="8f37c-117">El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la firma del método siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f37c-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="8f37c-118">El método devuelve una matriz de tipo de valor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="8f37c-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="8f37c-119">El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="8f37c-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="8f37c-120">Carga de la página en el explorador rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionado.</span><span class="sxs-lookup"><span data-stu-id="8f37c-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="8f37c-121">Además, ASP.NET define el `__doPostBack()` método JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8f37c-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="8f37c-122">Una vez que se ha cargado la página, esta llamada de JavaScript se agrega a la lista desplegable, pero solo si hay elementos en ella.</span><span class="sxs-lookup"><span data-stu-id="8f37c-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="8f37c-123">Si no hay ningún elemento en la lista, el Kit de herramientas de Control está cargando actualmente, por lo que el código de JavaScript usa un tiempo de espera y se intenta de nuevo en un medio segundo.</span><span class="sxs-lookup"><span data-stu-id="8f37c-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="8f37c-124">De este modo, una devolución de datos solo se ejecuta cuando hay elementos realmente en la lista y el usuario selecciona una entrada.</span><span class="sxs-lookup"><span data-stu-id="8f37c-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="8f37c-125">[![Al seleccionar un elemento de lista, una devolución de datos](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f37c-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="8f37c-126">Al seleccionar un elemento de lista, una devolución de datos ([haga clic aquí para ver imagen en tamaño completo](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f37c-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8f37c-127">Anterior</span><span class="sxs-lookup"><span data-stu-id="8f37c-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
