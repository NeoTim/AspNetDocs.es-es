---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Crear direcciones URL legibles en ASP.NET Web Pages (Razor) sitios | Microsoft Docs
author: Rick-Anderson
description: Este artículo describe el enrutamiento en un sitio Web de ASP.NET Web Pages (Razor), y cómo se puede usar direcciones URL que sean más legibles y mejor para SEO. Deberá...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 26d8f94b2e38fe5205a37e3d37b4e3bd509a3874
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061492"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="f6570-104">Crear direcciones URL legibles en sitios de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f6570-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="f6570-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f6570-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f6570-106">Este artículo describe el enrutamiento en un sitio Web de ASP.NET Web Pages (Razor), y cómo se puede usar direcciones URL que sean más legibles y mejor para SEO.</span><span class="sxs-lookup"><span data-stu-id="f6570-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="f6570-107">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="f6570-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f6570-108">Cómo ASP.NET usa el enrutamiento para que le permiten usar más legibles y que se puede buscar las direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="f6570-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f6570-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f6570-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f6570-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f6570-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f6570-111">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="f6570-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="f6570-112">Acerca del enrutamiento</span><span class="sxs-lookup"><span data-stu-id="f6570-112">About Routing</span></span>

<span data-ttu-id="f6570-113">Las direcciones URL para las páginas de su sitio pueden tener un impacto sobre cómo funciona el sitio.</span><span class="sxs-lookup"><span data-stu-id="f6570-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="f6570-114">Una dirección URL que &quot;descriptivo&quot; puede ser más fácil para los usuarios a usar el sitio.</span><span class="sxs-lookup"><span data-stu-id="f6570-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="f6570-115">También puede ayudar con la optimización de motor de búsqueda (SEO) para el sitio.</span><span class="sxs-lookup"><span data-stu-id="f6570-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="f6570-116">Sitios Web ASP.NET incluyen la capacidad para usar direcciones URL descriptivas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="f6570-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="f6570-117">ASP.NET permite crear direcciones URL significativas que describen las acciones del usuario en lugar de simplemente apunta a un archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f6570-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="f6570-118">Tenga en cuenta estas direcciones URL para un blog ficticio:</span><span class="sxs-lookup"><span data-stu-id="f6570-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="f6570-119">Compare estas direcciones URL en las siguientes:</span><span class="sxs-lookup"><span data-stu-id="f6570-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="f6570-120">En el primer par, un usuario tendría que saber que el blog se muestra utilizando el *blog.cshtml* página y, a continuación, tendría que construir una cadena de consulta que obtiene el intervalo derecho de la categoría o fecha.</span><span class="sxs-lookup"><span data-stu-id="f6570-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="f6570-121">Es mucho más fácil de entender y crear el segundo conjunto de ejemplos.</span><span class="sxs-lookup"><span data-stu-id="f6570-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="f6570-122">Las direcciones URL para el primer ejemplo también señalar directamente a un archivo específico (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="f6570-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="f6570-123">Si por alguna razón el blog de se han movido a otra carpeta en el servidor, o si se reescribieron el blog para usar una página distinta, los vínculos sería incorrectos.</span><span class="sxs-lookup"><span data-stu-id="f6570-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="f6570-124">El segundo conjunto de direcciones URL no apunta a una página específica, por lo que incluso si cambia la implementación de blog o la ubicación, las direcciones URL todavía sería válidas.</span><span class="sxs-lookup"><span data-stu-id="f6570-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="f6570-125">En ASP.NET Web Pages, puede crear direcciones URL más descriptivas, como aquellos en los ejemplos anteriores ya que ASP.NET utiliza *enrutamiento*.</span><span class="sxs-lookup"><span data-stu-id="f6570-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="f6570-126">Enrutamiento crea la asignación lógica desde una dirección URL a una página (o páginas) que puede satisfacer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="f6570-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="f6570-127">Dado que la asignación es lógica (no físico, a un archivo específico), el enrutamiento proporciona gran flexibilidad en cómo definir las direcciones URL para el sitio.</span><span class="sxs-lookup"><span data-stu-id="f6570-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="f6570-128">Cómo funciona el enrutamiento</span><span class="sxs-lookup"><span data-stu-id="f6570-128">How Routing Works</span></span>

<span data-ttu-id="f6570-129">Cuando ASP.NET procesa una solicitud, lee la dirección URL para determinar cómo enrutarlo.</span><span class="sxs-lookup"><span data-stu-id="f6570-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="f6570-130">ASP.NET intenta hacer coincidir los segmentos individuales de la dirección URL a los archivos de disco, van de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="f6570-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="f6570-131">Si hay una coincidencia, nada se queda en la dirección URL se pasa a la página como *información de ruta de acceso*.</span><span class="sxs-lookup"><span data-stu-id="f6570-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="f6570-132">Imagine que alguien realiza una solicitud con esta dirección URL:</span><span class="sxs-lookup"><span data-stu-id="f6570-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="f6570-133">La búsqueda es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="f6570-133">The search goes like this:</span></span>

1. <span data-ttu-id="f6570-134">¿Hay un archivo con la ruta de acceso y el nombre de */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="f6570-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="f6570-135">Si es así, ejecute esa página y no pasar ninguna información a él.</span><span class="sxs-lookup"><span data-stu-id="f6570-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="f6570-136">En caso contrario...</span><span class="sxs-lookup"><span data-stu-id="f6570-136">Otherwise ...</span></span>
2. <span data-ttu-id="f6570-137">¿Hay un archivo con la ruta de acceso y el nombre de */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="f6570-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="f6570-138">Si es así, ejecute esa página y pase el valor `c` a él.</span><span class="sxs-lookup"><span data-stu-id="f6570-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="f6570-139">En caso contrario...</span><span class="sxs-lookup"><span data-stu-id="f6570-139">Otherwise …</span></span>
3. <span data-ttu-id="f6570-140">¿Hay un archivo con la ruta de acceso y el nombre de */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="f6570-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="f6570-141">Si es así, ejecute esa página y pase el valor `b/c` a él.</span><span class="sxs-lookup"><span data-stu-id="f6570-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="f6570-142">Si coincide con la búsqueda encuentra no exacta para *.cshtml* archivos en sus carpetas especificadas, ASP.NET continúa buscando estos archivos a su vez:</span><span class="sxs-lookup"><span data-stu-id="f6570-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="f6570-143">*/a/b/c/default.cshtml* (no hay información de ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="f6570-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="f6570-144">*/a/b/c/index.cshtml* (no hay información de ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="f6570-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="f6570-145">Para ser claros, las solicitudes de páginas específicas (es decir, las solicitudes que incluyen la *.cshtml* extensión de nombre de archivo) funcionan tal como se esperaría.</span><span class="sxs-lookup"><span data-stu-id="f6570-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="f6570-146">Una solicitud como `http://www.contoso.com/a/b.cshtml` ejecutará la página *b.cshtml* perfectamente.</span><span class="sxs-lookup"><span data-stu-id="f6570-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="f6570-147">Dentro de una página, puede obtener la información de ruta de acceso a través de la página `UrlData` propiedad, que es un diccionario.</span><span class="sxs-lookup"><span data-stu-id="f6570-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="f6570-148">Imagine que tiene un archivo denominado *ViewCustomers.cshtml* y su sitio obtiene esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="f6570-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="f6570-149">Como se describe en las reglas anteriores, la solicitud irá a la página.</span><span class="sxs-lookup"><span data-stu-id="f6570-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="f6570-150">Dentro de la página, puede usar código similar al siguiente para obtener y mostrar la información de ruta de acceso (en este caso, el valor &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="f6570-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="f6570-151">Dado que el enrutamiento no implica que los nombres de archivo completo, puede haber ambigüedad si tiene páginas que tienen el mismo nombre pero diferentes extensiones de nombre de archivo (por ejemplo, *MyPage.cshtml* y *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="f6570-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="f6570-152">Con el fin de evitar problemas con el enrutamiento, es mejor asegurarse de que no tienen páginas de su sitio cuyos nombres difieren solo en su extensión.</span><span class="sxs-lookup"><span data-stu-id="f6570-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f6570-153">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f6570-153">Additional Resources</span></span>

<span data-ttu-id="f6570-154">[WebMatrix - direcciones URL, UrlData y enrutamiento de SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="f6570-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="f6570-155">Este artículo blog de Mike Brind proporciona algunos detalles adicionales sobre cómo funciona el enrutamiento en ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="f6570-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
