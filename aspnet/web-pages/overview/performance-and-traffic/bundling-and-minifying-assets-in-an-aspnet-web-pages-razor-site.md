---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Agrupación y minificación de activos en un sitio de páginas web ASP.NET (Razor) Microsoft Docs
author: rick-anderson
description: Agrupación y minificación son formas de hacer que su sitio sea más rápido. La agrupación le permite combinar varios archivos JavaScript ( .js ) o varias hojas de estilos en cascada (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 2a877c1e1a06ea2357f96b37ec4ae72f9f9c9ff3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81539933"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="66712-104">Agrupar y minificar recursos en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="66712-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="66712-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="66712-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="66712-106">Agrupación y minificación son formas de hacer que su sitio sea más rápido.</span><span class="sxs-lookup"><span data-stu-id="66712-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="66712-107">La agrupación le permite combinar varios archivos JavaScript (*.js*) o varios archivos de hoja de estilos en cascada (*.css*) para que se puedan descargar como una unidad, en lugar de uno a la vez.</span><span class="sxs-lookup"><span data-stu-id="66712-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="66712-108">La minificación exprime el espacio en blanco y realiza otros tipos de compresión para que los archivos descargados sean lo más pequeños posible.</span><span class="sxs-lookup"><span data-stu-id="66712-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="66712-109">La versión RC de ASP.NET Web Pages 2 no admite la agrupación y la minificación porque el paquete que contiene los elementos necesarios aún no está disponible en Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="66712-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="66712-110">Sentimos las molestias.</span><span class="sxs-lookup"><span data-stu-id="66712-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="66712-111">Se espera que el paquete esté disponible en la versión final de ASP.NET Web Pages 2 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="66712-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
