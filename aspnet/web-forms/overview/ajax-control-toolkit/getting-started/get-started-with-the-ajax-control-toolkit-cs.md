---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introducción al Kit de herramientas de control de AJAX (C- Microsoft Docs
author: rick-anderson
description: Aprenda todo lo que necesita saber para empezar a usar el Kit de herramientas de control de AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: b5954019ec3312f06f38012e4d3f9b1f71573f76
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543592"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="96641-103">Introducción a AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="96641-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="96641-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="96641-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="96641-105">Aprenda todo lo que necesita saber para empezar a usar el Kit de herramientas de control de AJAX.</span><span class="sxs-lookup"><span data-stu-id="96641-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>

<span data-ttu-id="96641-106">El kit de herramientas de control de AJAX contiene más de 30 controles gratuitos que puede utilizar en las aplicaciones de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="96641-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="96641-107">En este tutorial, aprenderá a descargar el Kit de herramientas de control de AJAX y agregar los controles del kit de herramientas a la caja de herramientas de Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="96641-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="96641-108">Descarga del kit de herramientas de control de AJAX</span><span class="sxs-lookup"><span data-stu-id="96641-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="96641-109">[AJAX Control Toolkit](http://devexpress.com/act) es un proyecto de código abierto desarrollado por los miembros de la comunidad ASP.NET y el equipo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="96641-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 

<span data-ttu-id="96641-110">[![Descarga del kit de herramientas de control de AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="96641-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="96641-111">**Figura 01**: Descargar el kit de herramientas de control de AJAX[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="96641-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>

<span data-ttu-id="96641-112">Después de descargar el archivo, debe desbloquear el archivo.</span><span class="sxs-lookup"><span data-stu-id="96641-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="96641-113">Haga clic con el botón derecho en el archivo, seleccione Propiedades y haga clic en el botón **Desbloquear** (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="96641-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>

<span data-ttu-id="96641-114">[![Desbloqueo del archivo ZIP de AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="96641-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="96641-115">**Figura 02**: Desbloqueo del archivo ZIP de AJAX Control Toolkit[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="96641-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>

<span data-ttu-id="96641-116">Después de desbloquear el archivo, puede descomprimir el archivo: haga clic con el botón derecho en el archivo y seleccione la opción de menú **Extraer todo.**</span><span class="sxs-lookup"><span data-stu-id="96641-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="96641-117">Ahora, estamos listos para agregar el kit de herramientas a la caja de herramientas de Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="96641-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="96641-118">Adición del Kit de herramientas de control de AJAX a Toolbox</span><span class="sxs-lookup"><span data-stu-id="96641-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="96641-119">La forma más fácil de usar el Kit de herramientas de control de AJAX es agregar el kit de herramientas a la caja de herramientas de Visual Studio/Visual Web Developer (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="96641-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="96641-120">De este modo, puede arrastrar simplemente un control de kit de herramientas a una página cuando desee usarlo.</span><span class="sxs-lookup"><span data-stu-id="96641-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>

<span data-ttu-id="96641-121">[![AJAX Control Toolkit aparece en la caja de herramientas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="96641-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="96641-122">**Figura 03**: AJAX Control Toolkit aparece en la caja de herramientas[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="96641-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>

<span data-ttu-id="96641-123">En primer lugar, debe agregar una pestaña AJAX Control Toolkit a la caja de herramientas.</span><span class="sxs-lookup"><span data-stu-id="96641-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="96641-124">Siga estos pasos.</span><span class="sxs-lookup"><span data-stu-id="96641-124">Follow these steps.</span></span>

1. <span data-ttu-id="96641-125">Cree un nuevo sitio web de ASP.NET seleccionando la opción de menú Archivo, Nuevo sitio web.</span><span class="sxs-lookup"><span data-stu-id="96641-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="96641-126">Haga doble clic en Default.aspx en el Explorador de soluciones ventana para abrir el archivo en el editor.</span><span class="sxs-lookup"><span data-stu-id="96641-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="96641-127">Haga clic con el botón derecho en el cuadro de herramientas debajo de la ficha General y seleccione la opción de menú **Agregar pestaña** (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="96641-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="96641-128">Introduzca una nueva pestaña denominada AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="96641-128">Enter a new tab named AJAX Control Toolkit.</span></span>

<span data-ttu-id="96641-129">[![Adición de una nueva pestaña](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="96641-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="96641-130">**Figura 04**: Adición de una nueva pestaña[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="96641-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>

<span data-ttu-id="96641-131">A continuación, debe agregar los controles del Kit de herramientas de control de AJAX a la nueva pestaña.</span><span class="sxs-lookup"><span data-stu-id="96641-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="96641-132">Haga clic con el botón derecho debajo de la pestaña Ajax Control Toolkit y seleccione la opción de menú **Elegir elementos (consulte la figura 5).**</span><span class="sxs-lookup"><span data-stu-id="96641-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="96641-133">Vaya a la ubicación donde descomprimió el Kit de herramientas de control de AJAX y seleccione el ensamblado AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="96641-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>

<span data-ttu-id="96641-134">[![Elija los elementos que desea agregar a la caja de herramientas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="96641-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="96641-135">**Figura 05**: Elija los elementos que desea agregar a la caja de herramientas (Haga clic para ver la[imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="96641-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>

<span data-ttu-id="96641-136">Después de completar estos pasos, todos los controles del kit de herramientas aparecerán en la caja de herramientas.</span><span class="sxs-lookup"><span data-stu-id="96641-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="96641-137">Actualización a una nueva versión del kit de herramientas</span><span class="sxs-lookup"><span data-stu-id="96641-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="96641-138">Si estaba utilizando una versión anterior del kit de herramientas y ahora necesita pasar a una versión posterior, estos son los pasos recomendados:</span><span class="sxs-lookup"><span data-stu-id="96641-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="96641-139">Binarios: elimine la versión anterior del ensamblado AjaxControlToolkit.dll de la carpeta Bin del sitio web.</span><span class="sxs-lookup"><span data-stu-id="96641-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="96641-140">Elementos del cuadro de herramientas: elimine la pestaña Ajax Control Toolkit y siga los pasos anteriores para volver a crear la pestaña con la nueva versión del ensamblado AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="96641-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="96641-141">Siguiente</span><span class="sxs-lookup"><span data-stu-id="96641-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
