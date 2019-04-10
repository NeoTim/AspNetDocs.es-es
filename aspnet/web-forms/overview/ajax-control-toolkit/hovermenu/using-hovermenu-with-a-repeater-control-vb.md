---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Usar HoverMenu con un Control Repeater (VB) | Microsoft Docs
author: wenz
description: 'El control HoverMenu en AJAX Control Toolkit proporciona un efecto emergente simple: Cuando el puntero del mouse se sitúa sobre un elemento, aparece un mensaje emergente en un específicamente...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 963850e1048d4fde573f28244fd32d0c4232fda4
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59399195"
---
# <a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="7392a-103">Usar HoverMenu con un control Repeater (VB)</span><span class="sxs-lookup"><span data-stu-id="7392a-103">Using HoverMenu with a Repeater Control (VB)</span></span>

<span data-ttu-id="7392a-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7392a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7392a-105">[Descargar código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7392a-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="7392a-106">El control HoverMenu en AJAX Control Toolkit proporciona un efecto emergente simple: Cuando el puntero del mouse se sitúa sobre un elemento, aparece un mensaje emergente en una posición especificada.</span><span class="sxs-lookup"><span data-stu-id="7392a-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="7392a-107">También es posible usar este control dentro de un control repeater.</span><span class="sxs-lookup"><span data-stu-id="7392a-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="7392a-108">Información general</span><span class="sxs-lookup"><span data-stu-id="7392a-108">Overview</span></span>

<span data-ttu-id="7392a-109">El `HoverMenu` control de AJAX Control Toolkit proporciona un efecto emergente simple: Cuando el puntero del mouse se sitúa sobre un elemento, aparece un mensaje emergente en una posición especificada.</span><span class="sxs-lookup"><span data-stu-id="7392a-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="7392a-110">También es posible usar este control dentro de un control repeater.</span><span class="sxs-lookup"><span data-stu-id="7392a-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="7392a-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="7392a-111">Steps</span></span>

<span data-ttu-id="7392a-112">En primer lugar, es necesario un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="7392a-112">First of all, a data source is required.</span></span> <span data-ttu-id="7392a-113">Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="7392a-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="7392a-114">La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="7392a-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="7392a-115">La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="7392a-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="7392a-116">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7392a-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="7392a-117">Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7392a-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="7392a-118">Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7392a-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="7392a-119">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="7392a-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="7392a-120">A continuación, agregue un origen de datos a la página.</span><span class="sxs-lookup"><span data-stu-id="7392a-120">Then, add a data source to the page.</span></span> <span data-ttu-id="7392a-121">Para poder usar una cantidad limitada de datos, se selecciona solo las cinco primeras entradas en la tabla de proveedor de la base de datos AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="7392a-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="7392a-122">Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="7392a-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="7392a-123">El marcado siguiente muestra la sintaxis correcta:</span><span class="sxs-lookup"><span data-stu-id="7392a-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="7392a-124">A continuación, agregue un panel que actúa como el elemento emergente modal:</span><span class="sxs-lookup"><span data-stu-id="7392a-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="7392a-125">Ahora, el `HoverMenuExtender` entra en juego.</span><span class="sxs-lookup"><span data-stu-id="7392a-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="7392a-126">Para que todos los elementos del origen de datos obtiene su propia ventana emergente, se debe poner el extensor del control Repeater del `<ItemTemplate>` sección.</span><span class="sxs-lookup"><span data-stu-id="7392a-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="7392a-127">Aquí está el marcado:</span><span class="sxs-lookup"><span data-stu-id="7392a-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="7392a-128">Ahora, todos los elementos del origen de datos muestran un menú emergente a la derecha (`PopupPosition` atributo) después de un retraso de 50 milisegundos (`PopDelay` atributo).</span><span class="sxs-lookup"><span data-stu-id="7392a-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


[![T<span data-ttu-id="7392a-129">menú de desplazamiento aparece junto a cada elemento del repetidor]</span><span class="sxs-lookup"><span data-stu-id="7392a-129">he hover menu appears next to each item in the repeater]</span></span>(using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

<span data-ttu-id="7392a-130">El menú de desplazamiento que aparece junto a cada elemento en el control repeater ([haga clic aquí para ver imagen en tamaño completo](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7392a-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7392a-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="7392a-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
