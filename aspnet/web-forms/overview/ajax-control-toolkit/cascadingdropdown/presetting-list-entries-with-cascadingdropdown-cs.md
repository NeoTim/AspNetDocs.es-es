---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Preconfigurar entradas de lista con CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 188f98d013707178e50858f8ea26d8cf2af06bea
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59406020"
---
# <a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="66be7-103">Preconfigurar entradas de lista con CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="66be7-103">Presetting List Entries with CascadingDropDown (C#)</span></span>

<span data-ttu-id="66be7-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="66be7-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="66be7-105">[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="66be7-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="66be7-106">El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="66be7-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="66be7-107">Es posible que un elemento de lista es el valor una vez que los datos se ha cargado dinámicamente con un poco de código.</span><span class="sxs-lookup"><span data-stu-id="66be7-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="66be7-108">Información general</span><span class="sxs-lookup"><span data-stu-id="66be7-108">Overview</span></span>

<span data-ttu-id="66be7-109">El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="66be7-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="66be7-110">(Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) Es posible que un elemento de lista es el valor una vez que los datos se ha cargado dinámicamente con un poco de código.</span><span class="sxs-lookup"><span data-stu-id="66be7-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="66be7-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="66be7-111">Steps</span></span>

<span data-ttu-id="66be7-112">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="66be7-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="66be7-113">A continuación, se requiere un control de DropDownList:</span><span class="sxs-lookup"><span data-stu-id="66be7-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="66be7-114">Para esta lista, se agrega un extensor CascadingDropDown, que proporciona información de dirección URL y el método de servicio web:</span><span class="sxs-lookup"><span data-stu-id="66be7-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="66be7-115">El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la firma del método siguiente:</span><span class="sxs-lookup"><span data-stu-id="66be7-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="66be7-116">El método devuelve una matriz de tipo de valor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="66be7-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="66be7-117">El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="66be7-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="66be7-118">Si el tercer argumento se establece en true, la lista de elemento se selecciona automáticamente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="66be7-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="66be7-119">Carga de la página en el explorador rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionado.</span><span class="sxs-lookup"><span data-stu-id="66be7-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


[![T<span data-ttu-id="66be7-120">lista se rellena y preseleccionado automáticamente]</span><span class="sxs-lookup"><span data-stu-id="66be7-120">he list is filled and preselected automatically]</span></span>(presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

<span data-ttu-id="66be7-121">La lista se rellena y preseleccionada automáticamente ([haga clic aquí para ver imagen en tamaño completo](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="66be7-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="66be7-122">[Anterior](using-cascadingdropdown-with-a-database-cs.md)
> [Siguiente](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="66be7-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
