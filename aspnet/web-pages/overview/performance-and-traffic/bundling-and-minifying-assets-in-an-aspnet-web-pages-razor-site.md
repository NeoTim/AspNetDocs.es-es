---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Agrupar y Minificar recursos en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: microsoft
description: Unión y minificación son formas para agilizar su sitio. La Unión permite combinar varios archivos de JavaScript (.js) o múltiples hoja de estilo (...)
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400456"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="9ae66-104">Agrupar y minificar recursos en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="9ae66-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="9ae66-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9ae66-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="9ae66-106">Unión y minificación son formas para agilizar su sitio.</span><span class="sxs-lookup"><span data-stu-id="9ae66-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="9ae66-107">La unión le permite combinar varios JavaScript (*.js*) archivos o varias hojas de estilo en cascada (*.css*) archivos para que se pueden descargar como una unidad, en lugar de uno en uno.</span><span class="sxs-lookup"><span data-stu-id="9ae66-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="9ae66-108">Minificación encoge out espacios en blanco y realiza otros tipos de compresión para que los archivos descargados como pequeñas un posible.</span><span class="sxs-lookup"><span data-stu-id="9ae66-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="9ae66-109">La versión RC de ASP.NET Web Pages 2 no admite la unión y minificación porque aún no está disponible en Microsoft WebMatrix del paquete que contiene los elementos necesarios.</span><span class="sxs-lookup"><span data-stu-id="9ae66-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="9ae66-110">Disculpe las molestias.</span><span class="sxs-lookup"><span data-stu-id="9ae66-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="9ae66-111">El paquete se espera que esté disponible en la versión final de ASP.NET Web Pages 2 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="9ae66-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
