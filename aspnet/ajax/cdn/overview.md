---
uid: ajax/cdn/overview
title: Content Delivery Network de Microsoft Ajax | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 58e2d62da2181108f34bbf3e1e948977241e1dee
ms.sourcegitcommit: b3c0e67bb231dbb0db9e8dfccd51cf715481edcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2020
ms.locfileid: "88161362"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="23630-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="23630-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="23630-103">Las aplicaciones de producción no deben tener una dependencia fuerte en los recursos de la red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="23630-104">Las aplicaciones deben probar el recurso de red CDN al que se hace referencia y utilizar un recurso de reserva cuando la red CDN no está disponible.</span><span class="sxs-lookup"><span data-stu-id="23630-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="23630-105">La red CDN de Microsoft Ajax no tiene un contrato de nivel de servicio superior y más allá con un Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="23630-106">Use [este problema de github](https://github.com/dotnet/AspNetDocs/issues/116) para notificar problemas con la red CDN de Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="23630-106">Use [this GitHub issue](https://github.com/dotnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="23630-107">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="23630-107">Table of Contents</span></span>

<span data-ttu-id="23630-108">**[ajax.microsoft.com ha cambiado de nombre a ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="23630-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="23630-109">**[Compatibilidad con Visual Studio. vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="23630-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="23630-110">**[Uso de ASP.NET AJAX desde la red CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="23630-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="23630-111">**[Usar jQuery desde la red CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="23630-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="23630-112">**[Uso de jQuery UI desde la red CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="23630-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="23630-113">**[Archivos de terceros en la red CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="23630-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="23630-114">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="23630-115">Versiones de jQuery Migrate en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="23630-116">Versiones de jQuery UI en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="23630-117">Versiones de validación de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="23630-118">Versiones móviles de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="23630-119">Versiones de plantillas de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="23630-120">Versiones de ciclo de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="23630-121">Versiones de DataTable de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="23630-122">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="23630-123">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="23630-124">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="23630-125">Versiones de globalización en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="23630-126">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="23630-127">Versiones de bootstrap en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="23630-128">Versiones de bootstrap TouchCarousel en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="23630-129">Hammer.js versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="23630-130">Formularios Web Forms de ASP.NET y versiones de Ajax en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="23630-131">Versiones de ASP.NET MVC en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="23630-132">Versiones de Signalr ASP.NET en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="23630-133">Microsoft Ajax Content Delivery Network (CDN) hospeda bibliotecas de JavaScript de terceros populares, como jQuery, y le permite agregarlas fácilmente a sus aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="23630-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="23630-134">Por ejemplo, puede empezar a usar jQuery, que se hospeda en esta red CDN, simplemente agregando una &lt; etiqueta de script &gt; a la página que apunta a Ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="23630-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="23630-135">Al aprovechar la CDN, puede mejorar significativamente el rendimiento de las aplicaciones AJAX.</span><span class="sxs-lookup"><span data-stu-id="23630-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="23630-136">El contenido de la red CDN se almacena en caché en los servidores ubicados en todo el mundo.</span><span class="sxs-lookup"><span data-stu-id="23630-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="23630-137">Además, la red CDN permite que los exploradores reutilicen archivos JavaScript de terceros almacenados en caché para sitios web que se encuentran en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="23630-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="23630-138">La red CDN es compatible con SSL (HTTPS) en caso de que tenga que servir una página web con el Capa de sockets seguros.</span><span class="sxs-lookup"><span data-stu-id="23630-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="23630-139">La red CDN hospeda las siguientes bibliotecas de scripts de terceros que se han cargado y a los que los propietarios de esas bibliotecas les conceden licencias:</span><span class="sxs-lookup"><span data-stu-id="23630-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="23630-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="23630-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="23630-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="23630-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="23630-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="23630-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="23630-143">Validación de jQuery (https://jqueryvalidation.org/)</span><span class="sxs-lookup"><span data-stu-id="23630-143">jQuery Validation (https://jqueryvalidation.org/)</span></span>
- <span data-ttu-id="23630-144">Ciclo de jQuery (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="23630-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="23630-145">DataTables de jQuery (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="23630-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="23630-146">La red CDN de Microsoft Ajax también incluye las siguientes bibliotecas que Microsoft ha cargado:</span><span class="sxs-lookup"><span data-stu-id="23630-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="23630-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="23630-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="23630-148">Archivos JavaScript de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="23630-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="23630-149">Archivos JavaScript de ASP.NET Signalr</span><span class="sxs-lookup"><span data-stu-id="23630-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="23630-150">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="23630-151">Los propietarios de los derechos de autor de las bibliotecas le otorgan licencias para estas bibliotecas.</span><span class="sxs-lookup"><span data-stu-id="23630-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="23630-152">Cualquier derecho que tenga que descargar y usar estas bibliotecas se concede únicamente a los propietarios de los derechos de autor respectivos.</span><span class="sxs-lookup"><span data-stu-id="23630-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="23630-153">Dado que no son bibliotecas de Microsoft, Microsoft no ofrece ninguna garantía ni licencias de derechos de propiedad intelectual (incluidos derechos de patente implícitos) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="23630-154">Si desea enviar la biblioteca de JavaScript y la biblioteca es una de las principales bibliotecas de JavaScript (como se muestra en http://trends.builtwith.com) o extensiones/complementos a estas bibliotecas (a) populares; o (b) resulta útil para el uso en ASP.net, póngase en contacto con AjaxCDNSubmission@Microsoft.com .</span><span class="sxs-lookup"><span data-stu-id="23630-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="23630-155">ajax.microsoft.com ha cambiado de nombre a ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="23630-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="23630-156">La red CDN usada para usar el nombre de dominio de microsoft.com y se ha cambiado para usar el nombre de dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="23630-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="23630-157">Este cambio se realizó para aumentar el rendimiento porque, cuando un explorador hacía referencia al dominio microsoft.com, enviaría cualquier cookie del dominio a través de la conexión con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="23630-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="23630-158">El cambio de nombre a un nombre de dominio distinto del rendimiento de microsoft.com puede aumentarse en un 25%.</span><span class="sxs-lookup"><span data-stu-id="23630-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="23630-159">Nota ajax.microsoft.com continuará funcionando, pero se recomienda ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="23630-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="23630-160">Formato antiguo:https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="23630-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="23630-161">Nuevo formato:https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="23630-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="23630-162">Compatibilidad con Visual Studio. vsdoc</span><span class="sxs-lookup"><span data-stu-id="23630-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="23630-163">Para usar los archivos. vsdoc correctamente con Visual Studio 2008, debe asegurarse de que tiene VS 2008 SP1 instalado y la revisión para los archivos vsdoc instalados.</span><span class="sxs-lookup"><span data-stu-id="23630-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="23630-164">Puede obtenerlos aquí:</span><span class="sxs-lookup"><span data-stu-id="23630-164">You can get these from here:</span></span>

- [<span data-ttu-id="23630-165">Descargar Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="23630-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Descargar Visual Studio 2008 SP1")
- [<span data-ttu-id="23630-166">Descarga de la revisión. vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="23630-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Descarga de la revisión. vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="23630-167">Visual Studio 2010 admite archivos. vsdoc sin revisiones adicionales.</span><span class="sxs-lookup"><span data-stu-id="23630-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="23630-168">Uso de ASP.NET AJAX desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="23630-169">Al usar ASP.NET 4, puede redirigir todas las solicitudes de scripts de ASP.NET Framework a la red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="23630-170">La recuperación de scripts desde la red CDN en lugar de su servidor Web local puede mejorar considerablemente el rendimiento de los sitios Web ASP.NET públicos.</span><span class="sxs-lookup"><span data-stu-id="23630-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="23630-171">Use la propiedad EnableCDN de ScriptManager para redirigir todas las solicitudes de script de ASP.NET Framework a la red CDN de Microsoft Ajax:</span><span class="sxs-lookup"><span data-stu-id="23630-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="23630-172">Usar jQuery desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="23630-173">Puede usar los scripts de jQuery hospedados en la red CDN en la aplicación web agregando el siguiente elemento de script a una página:</span><span class="sxs-lookup"><span data-stu-id="23630-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="23630-174">La red CDN también incluye la versión reducida del script jQuery, que puede obtener mediante el siguiente elemento:</span><span class="sxs-lookup"><span data-stu-id="23630-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="23630-175">Para permitir que la página reproduzca la carga de jQuery desde una ruta de acceso local en su propio sitio web si la red CDN no está disponible, agregue el siguiente elemento inmediatamente después del elemento que hace referencia a la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="23630-176">En la página de ejemplo siguiente se usa la versión de la red CDN de la biblioteca de jQuery (con retroceso a una copia local) para mostrar el contenido de un elemento div al hacer clic en un botón.</span><span class="sxs-lookup"><span data-stu-id="23630-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="23630-177">Para obtener más información sobre jQuery y descargar una copia local de jQuery, visite el sitio web de [jQuery](http://jquery.com/) .</span><span class="sxs-lookup"><span data-stu-id="23630-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="23630-178">Uso de jQuery UI desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="23630-179">La red CDN también hospeda la biblioteca de la interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="23630-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="23630-180">La biblioteca de interfaz de usuario de jQuery incluye un amplio conjunto de widgets y efectos que puede usar en las aplicaciones de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="23630-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="23630-181">Por ejemplo, en la página siguiente se muestra cómo puede usar el DatePicker de la interfaz de usuario de jQuery en el contexto de una aplicación de formularios Web Forms de ASP.NET para mostrar un calendario emergente:</span><span class="sxs-lookup"><span data-stu-id="23630-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="23630-182">Cuando se mueve el foco al cuadro de texto mediante el teclado, se muestra un calendario:</span><span class="sxs-lookup"><span data-stu-id="23630-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario emergente creado con DatePicker](overview/_static/image1.png)

<span data-ttu-id="23630-184">Tenga en cuenta que debe incluir tres archivos de la red CDN en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="23630-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="23630-185">La biblioteca de jQuery &mdash; la biblioteca de la interfaz de usuario de jQuery depende de la biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="23630-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="23630-186">Debe agregar la biblioteca de jQuery a la página antes de agregar la biblioteca de jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="23630-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="23630-187">La biblioteca de interfaz de usuario de jQuery &mdash; contiene todos los efectos y widgets de la interfaz de usuario de jQuery, como el widget DatePicker usado en la página anterior.</span><span class="sxs-lookup"><span data-stu-id="23630-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="23630-188">Un tema de jQuery UI &mdash; la interfaz de usuario de jQuery admite distintos temas.</span><span class="sxs-lookup"><span data-stu-id="23630-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="23630-189">La página anterior incluye un vínculo a un archivo CSS para importar el tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="23630-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="23630-190">Todos los temas de la interfaz de usuario de jQuery estándar se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="23630-191">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en la red CDN de Microsoft Ajax") para ver las miniaturas de cada tema.</span><span class="sxs-lookup"><span data-stu-id="23630-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="23630-192">Para obtener más información sobre la biblioteca de interfaz de usuario de jQuery, visite el [sitio web oficial de jQuery UI](http://jQueryUI.com "sitio web de jQuery UI").</span><span class="sxs-lookup"><span data-stu-id="23630-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="23630-193">Archivos de terceros en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="23630-194">La red CDN hospeda algunas de las bibliotecas de JavaScript de terceros más populares.</span><span class="sxs-lookup"><span data-stu-id="23630-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="23630-195">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="23630-196">Los propietarios de los derechos de autor de las bibliotecas le otorgan licencias para estas bibliotecas.</span><span class="sxs-lookup"><span data-stu-id="23630-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="23630-197">Cualquier derecho que tenga que descargar y usar estas bibliotecas se concede únicamente a los propietarios de los derechos de autor respectivos.</span><span class="sxs-lookup"><span data-stu-id="23630-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="23630-198">Dado que no son bibliotecas de Microsoft, Microsoft no ofrece ninguna garantía ni licencias de derechos de propiedad intelectual (incluidos derechos de patente implícitos) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="23630-199">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="23630-200">Las siguientes versiones de jQuery se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-351"></a><span data-ttu-id="23630-201">jQuery versión 3.5.1</span><span class="sxs-lookup"><span data-stu-id="23630-201">jQuery version 3.5.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.min.map

#### <a name="jquery-version-350"></a><span data-ttu-id="23630-202">jQuery versión 3.5.0</span><span class="sxs-lookup"><span data-stu-id="23630-202">jQuery version 3.5.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.min.map

#### <a name="jquery-version-341"></a><span data-ttu-id="23630-203">versión de jQuery 3.4.1</span><span class="sxs-lookup"><span data-stu-id="23630-203">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="23630-204">versión de jQuery 3.4.0</span><span class="sxs-lookup"><span data-stu-id="23630-204">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="23630-205">jQuery versión 3.3.1</span><span class="sxs-lookup"><span data-stu-id="23630-205">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="23630-206">jQuery versión 3.2.1</span><span class="sxs-lookup"><span data-stu-id="23630-206">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="23630-207">versión de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-207">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="23630-208">jQuery versión 3.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-208">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="23630-209">jQuery versión 3.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-209">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="23630-210">jQuery versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-210">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="23630-211">jQuery versión 2.2.4</span><span class="sxs-lookup"><span data-stu-id="23630-211">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="23630-212">jQuery versión 2.2.3</span><span class="sxs-lookup"><span data-stu-id="23630-212">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="23630-213">jQuery versión 2.2.2</span><span class="sxs-lookup"><span data-stu-id="23630-213">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="23630-214">jQuery versión 2.2.1</span><span class="sxs-lookup"><span data-stu-id="23630-214">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="23630-215">versión de jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-215">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="23630-216">jQuery versión 2.1.4</span><span class="sxs-lookup"><span data-stu-id="23630-216">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="23630-217">jQuery versión 2.1.3</span><span class="sxs-lookup"><span data-stu-id="23630-217">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="23630-218">jQuery versión 2.1.2</span><span class="sxs-lookup"><span data-stu-id="23630-218">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="23630-219">jQuery versión 2.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-219">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="23630-220">jQuery versión 2.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-220">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="23630-221">versión de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="23630-221">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="23630-222">jQuery versión 2.0.2</span><span class="sxs-lookup"><span data-stu-id="23630-222">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="23630-223">jQuery versión 2.0.1</span><span class="sxs-lookup"><span data-stu-id="23630-223">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="23630-224">jQuery versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-224">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="23630-225">versión de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="23630-225">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="23630-226">versión de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="23630-226">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="23630-227">versión de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="23630-227">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="23630-228">versión de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="23630-228">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="23630-229">versión de jQuery 1.12.0,</span><span class="sxs-lookup"><span data-stu-id="23630-229">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="23630-230">versión de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="23630-230">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="23630-231">versión de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="23630-231">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="23630-232">versión de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="23630-232">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="23630-233">versión de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="23630-233">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="23630-234">versión de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="23630-234">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="23630-235">versión de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="23630-235">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="23630-236">versión de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="23630-236">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="23630-237">versión de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="23630-237">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="23630-238">versión de jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="23630-238">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="23630-239">versión de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="23630-239">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="23630-240">versión de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="23630-240">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="23630-241">versión de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="23630-241">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="23630-242">versión de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="23630-242">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="23630-243">versión de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="23630-243">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="23630-244">versión de jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="23630-244">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="23630-245">jQuery versión 1,7</span><span class="sxs-lookup"><span data-stu-id="23630-245">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="23630-246">versión de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="23630-246">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="23630-247">versión de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="23630-247">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="23630-248">versión de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="23630-248">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="23630-249">versión de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="23630-249">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="23630-250">jQuery versión 1,6</span><span class="sxs-lookup"><span data-stu-id="23630-250">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="23630-251">versión de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="23630-251">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="23630-252">versión de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="23630-252">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="23630-253">jQuery versión 1,5</span><span class="sxs-lookup"><span data-stu-id="23630-253">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="23630-254">versión de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="23630-254">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="23630-255">versión de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="23630-255">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="23630-256">jQuery versión 1.4.2</span><span class="sxs-lookup"><span data-stu-id="23630-256">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="23630-257">jQuery versión 1.4.1</span><span class="sxs-lookup"><span data-stu-id="23630-257">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="23630-258">jQuery versión 1,4</span><span class="sxs-lookup"><span data-stu-id="23630-258">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="23630-259">jQuery, versión 1.3.2</span><span class="sxs-lookup"><span data-stu-id="23630-259">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="23630-260">Versiones de jQuery Migrate en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-260">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="23630-261">Las siguientes versiones de jQuery Migrate se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-261">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="23630-262">jQuery Migrate versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-262">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="23630-263">jQuery Migrate versión 1.2.1</span><span class="sxs-lookup"><span data-stu-id="23630-263">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="23630-264">jQuery Migrate versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-264">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="23630-265">jQuery Migrate versión 1.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-265">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="23630-266">jQuery Migrate versión 1.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-266">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="23630-267">jQuery Migrate versión 1.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-267">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="23630-268">Versiones de jQuery UI en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-268">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="23630-269">Las siguientes versiones de la biblioteca de interfaz de usuario de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-269">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="23630-270">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-270">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-271">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="23630-271">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-272">jQuery UI 1.12.0,</span><span class="sxs-lookup"><span data-stu-id="23630-272">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-273">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="23630-273">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-274">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="23630-274">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-275">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="23630-275">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-276">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="23630-276">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-277">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="23630-277">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-278">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="23630-278">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-279">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="23630-279">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-280">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="23630-280">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-281">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="23630-281">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-282">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="23630-282">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-283">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="23630-283">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-284">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="23630-284">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-285">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="23630-285">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-286">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="23630-286">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-287">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="23630-287">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-288">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="23630-288">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-289">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="23630-289">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-290">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="23630-290">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-291">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="23630-291">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-292">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="23630-292">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-293">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="23630-293">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-294">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="23630-294">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-295">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="23630-295">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-296">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="23630-296">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-297">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="23630-297">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-298">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="23630-298">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-299">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="23630-299">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-300">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="23630-300">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-301">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="23630-301">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-302">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="23630-302">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-303">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="23630-303">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-304">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="23630-304">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-305">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="23630-305">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="23630-306">Versiones de validación de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-306">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="23630-307">Las siguientes versiones del complemento de [validación de jQuery](https://jqueryvalidation.org/ "Complemento de validación de jQuery") se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-307">The following releases of the [jQuery Validation](https://jqueryvalidation.org/ "jQuery Validation Plugin") plugin are hosted on this CDN.</span></span> <span data-ttu-id="23630-308">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-308">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-309">Validar 1.19.2 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-309">jQuery Validate 1.19.2</span></span>](jquery-validate/cdnjqueryvalidate1192.md "1.19.2 de validación de jQuery")
- [<span data-ttu-id="23630-310">Validar 1.19.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-310">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "1.19.1 de validación de jQuery")
- [<span data-ttu-id="23630-311">Validar 1.19.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-311">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "1.19.0 de validación de jQuery")
- [<span data-ttu-id="23630-312">Validar 1.17.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-312">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 de validación de jQuery")
- [<span data-ttu-id="23630-313">Validar 1.16.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-313">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="23630-314">Validar 1.15.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-314">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="23630-315">Validar 1.15.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-315">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="23630-316">Validar 1.14.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-316">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="23630-317">Validar 1.13.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-317">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="23630-318">Validar 1.13.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-318">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="23630-319">Validar 1.12.0, de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-319">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="23630-320">Validar 1.11.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-320">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="23630-321">Validar 1.11.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-321">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="23630-322">Validar 1.10.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-322">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="23630-323">Validación de jQuery 1,9</span><span class="sxs-lookup"><span data-stu-id="23630-323">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "Versión 1.9 de jquery.validate")
- [<span data-ttu-id="23630-324">Validar 1.8.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-324">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "Versión 1.8.1 de jquery.validate")
- [<span data-ttu-id="23630-325">Validación de jQuery 1,8</span><span class="sxs-lookup"><span data-stu-id="23630-325">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "Versión 1.8 de jquery.validate")
- [<span data-ttu-id="23630-326">Validación de jQuery 1,7</span><span class="sxs-lookup"><span data-stu-id="23630-326">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "Versión 1.7 de jquery.validate")
- [<span data-ttu-id="23630-327">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="23630-327">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="23630-328">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="23630-328">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="23630-329">Versiones móviles de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-329">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="23630-330">Las siguientes versiones de la biblioteca de jQuery Mobile se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-330">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="23630-331">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-331">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-332">1.4.5 de jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="23630-332">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-333">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="23630-333">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-334">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="23630-334">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-335">1.4.0 de jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="23630-335">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-336">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="23630-336">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-337">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="23630-337">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-338">1.3.0 de jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="23630-338">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-339">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-339">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-340">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="23630-340">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-341">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-341">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-342">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-342">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-343">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="23630-343">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-344">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="23630-344">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-345">jQuery Mobile 1,0</span><span class="sxs-lookup"><span data-stu-id="23630-345">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-346">jQuery Mobile 1,0 RC 2</span><span class="sxs-lookup"><span data-stu-id="23630-346">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-347">jQuery Mobile 1,0 RC 1</span><span class="sxs-lookup"><span data-stu-id="23630-347">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="23630-348">jQuery Mobile 1,0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="23630-348">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 en la red CDN de Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="23630-349">Versiones de plantillas de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-349">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="23630-350">Las siguientes versiones del complemento de plantillas de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-350">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="23630-351">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-352">jQuery Templates Beta 1</span><span class="sxs-lookup"><span data-stu-id="23630-352">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery Templates Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="23630-353">Versiones de ciclo de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-353">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="23630-354">Las siguientes versiones del complemento de ciclo de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-354">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="23630-355">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-355">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-356">Ciclo de jQuery 2,99</span><span class="sxs-lookup"><span data-stu-id="23630-356">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="23630-357">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="23630-357">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="23630-358">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="23630-358">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="23630-359">Versiones de DataTable de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-359">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="23630-360">Las siguientes versiones del complemento jQuery DataTables se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-360">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="23630-361">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-361">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-362">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="23630-362">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="23630-363">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="23630-363">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="23630-364">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="23630-364">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="23630-365">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="23630-365">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="23630-366">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="23630-366">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="23630-367">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="23630-367">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="23630-368">Tablas de 1.9.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="23630-368">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="23630-369">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="23630-369">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="23630-370">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-370">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="23630-371">Las siguientes versiones de [modernizr](http://www.modernizr.com "Modernizr") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-371">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="23630-372">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-372">JSHint Releases on the CDN</span></span>

<span data-ttu-id="23630-373">Las siguientes versiones de [JSHint](http://www.jshint.com "JSHint") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-373">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="23630-374">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-374">Knockout Releases on the CDN</span></span>

<span data-ttu-id="23630-375">Las siguientes versiones de [cobertura](http://www.knockoutjs.com "Cobertura") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-375">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="23630-376">Versiones de globalización en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-376">Globalize Releases on the CDN</span></span>

<span data-ttu-id="23630-377">Las siguientes versiones de la [globalización](https://github.com/jquery/globalize "Globalizar") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-377">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="23630-378">Globalization versión 1.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-378">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="23630-379">Globalizar versión 0.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-379">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="23630-380">todas las referencias culturales</span><span class="sxs-lookup"><span data-stu-id="23630-380">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="23630-381">Reemplace "{Culture-Code}" por el código de referencia cultural deseado; por ejemplo, globalize.culture.en-GB.js= = archivos de Microsoft en la red CDN = = Microsoft cargó estas bibliotecas.</span><span class="sxs-lookup"><span data-stu-id="23630-381">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="23630-382">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-382">Respond Releases on the CDN</span></span>

<span data-ttu-id="23630-383">Las siguientes versiones de [Response](https://github.com/scottjehl/Respond "Respuesta") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-383">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="23630-384">Respuesta a la versión 1.4.2</span><span class="sxs-lookup"><span data-stu-id="23630-384">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="23630-385">Respuesta a la versión 1.4.1</span><span class="sxs-lookup"><span data-stu-id="23630-385">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="23630-386">Respuesta de la versión 1.4.0</span><span class="sxs-lookup"><span data-stu-id="23630-386">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="23630-387">Respuesta de la versión 1.3.0</span><span class="sxs-lookup"><span data-stu-id="23630-387">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="23630-388">Respuesta a la versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-388">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="23630-389">Versiones de bootstrap en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-389">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="23630-390">Las siguientes versiones de bootstrap de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-390">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-450"></a><span data-ttu-id="23630-391">Versión de bootstrap 4.5.0</span><span class="sxs-lookup"><span data-stu-id="23630-391">Bootstrap version 4.5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-441"></a><span data-ttu-id="23630-392">Versión de bootstrap 4.4.1</span><span class="sxs-lookup"><span data-stu-id="23630-392">Bootstrap version 4.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-431"></a><span data-ttu-id="23630-393">Bootstrap versión 4.3.1</span><span class="sxs-lookup"><span data-stu-id="23630-393">Bootstrap version 4.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-421"></a><span data-ttu-id="23630-394">Versión 4.2.1 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="23630-394">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="23630-395">Bootstrap versión 4.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-395">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="23630-396">Bootstrap versión 4.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-396">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-341"></a><span data-ttu-id="23630-397">Versión de bootstrap 3.4.1</span><span class="sxs-lookup"><span data-stu-id="23630-397">Bootstrap version 3.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-340"></a><span data-ttu-id="23630-398">Versión de bootstrap 3.4.0</span><span class="sxs-lookup"><span data-stu-id="23630-398">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="23630-399">Versión de bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="23630-399">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="23630-400">Versión de bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="23630-400">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="23630-401">Versión de bootstrap 3.3.5</span><span class="sxs-lookup"><span data-stu-id="23630-401">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="23630-402">Versión de bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="23630-402">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="23630-403">Versión 3.3.2 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="23630-403">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="23630-404">Versión 3.3.1 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="23630-404">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="23630-405">Versión de bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="23630-405">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="23630-406">Versión de bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-406">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="23630-407">Versión 3.1.1 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="23630-407">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="23630-408">Bootstrap versión 3.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-408">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="23630-409">Versión de bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="23630-409">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="23630-410">Versión 3.0.2 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="23630-410">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="23630-411">Bootstrap versión 3.0.1</span><span class="sxs-lookup"><span data-stu-id="23630-411">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="23630-412">Versión 3.0.0 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="23630-412">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="23630-413">Versión de bootstrap 2.3.2</span><span class="sxs-lookup"><span data-stu-id="23630-413">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="23630-414">Versión de bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="23630-414">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="23630-415">Versiones de bootstrap TouchCarousel en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-415">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="23630-416">Las siguientes versiones de [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") bootstrap TouchCarousel se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-416">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="23630-417">Bootstrap TouchCarousel versión 0.8.0</span><span class="sxs-lookup"><span data-stu-id="23630-417">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="23630-418">Hammer.js versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-418">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="23630-419">Las siguientes versiones de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-419">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="23630-420">Hammer.js versión 2.0.4</span><span class="sxs-lookup"><span data-stu-id="23630-420">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="23630-421">Formularios Web Forms de ASP.NET y versiones de Ajax en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-421">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="23630-422">Las siguientes versiones de la biblioteca de ASP.NET AJAX se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="23630-422">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="23630-423">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="23630-423">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="23630-424">Formularios Web Forms de ASP.NET y Ajax versión 4.5.2</span><span class="sxs-lookup"><span data-stu-id="23630-424">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Formularios Web Forms de ASP.NET y Ajax 4.5.2")
- [<span data-ttu-id="23630-425">Formularios Web Forms de ASP.NET y Ajax versión 4</span><span class="sxs-lookup"><span data-stu-id="23630-425">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms and Ajax 4")
- [<span data-ttu-id="23630-426">ASP.NET AJAX versión 3,5</span><span class="sxs-lookup"><span data-stu-id="23630-426">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="23630-427">Versiones de ASP.NET MVC en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-427">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="23630-428">Los siguientes archivos JavaScript de ASP.NET MVC se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-428">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="23630-429">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="23630-429">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="23630-430">ASP.NET MVC 5,1</span><span class="sxs-lookup"><span data-stu-id="23630-430">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="23630-431">ASP.NET MVC 5,0</span><span class="sxs-lookup"><span data-stu-id="23630-431">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="23630-432">MVC 4.0 de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="23630-432">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="23630-433">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="23630-433">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="23630-434">ASP.NET MVC 2,0</span><span class="sxs-lookup"><span data-stu-id="23630-434">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="23630-435">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="23630-435">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="23630-436">Versiones de Signalr ASP.NET en la red CDN</span><span class="sxs-lookup"><span data-stu-id="23630-436">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="23630-437">Los siguientes archivos JavaScript de Signalr ASP.NET se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="23630-437">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="23630-438">ASP.NET Signalr 2.2.2</span><span class="sxs-lookup"><span data-stu-id="23630-438">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="23630-439">ASP.NET Signalr 2.2.1</span><span class="sxs-lookup"><span data-stu-id="23630-439">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="23630-440">ASP.NET Signalr 2.2.0</span><span class="sxs-lookup"><span data-stu-id="23630-440">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="23630-441">ASP.NET Signalr 2.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-441">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="23630-442">ASP.NET Signalr 2.0.3</span><span class="sxs-lookup"><span data-stu-id="23630-442">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="23630-443">ASP.NET Signalr 2.0.2</span><span class="sxs-lookup"><span data-stu-id="23630-443">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="23630-444">ASP.NET Signalr 2.0.1</span><span class="sxs-lookup"><span data-stu-id="23630-444">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="23630-445">ASP.NET Signalr 2.0.0</span><span class="sxs-lookup"><span data-stu-id="23630-445">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="23630-446">ASP.NET Signalr 1.1.3</span><span class="sxs-lookup"><span data-stu-id="23630-446">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="23630-447">ASP.NET Signalr 1.1.2</span><span class="sxs-lookup"><span data-stu-id="23630-447">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="23630-448">ASP.NET Signalr 1.1.1</span><span class="sxs-lookup"><span data-stu-id="23630-448">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="23630-449">ASP.NET Signalr 1.1.0</span><span class="sxs-lookup"><span data-stu-id="23630-449">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="23630-450">ASP.NET Signalr 1.0.1</span><span class="sxs-lookup"><span data-stu-id="23630-450">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="23630-451">Para obtener información acerca de los términos de uso de la red CDN, consulte [términos de uso de la red CDN de Microsoft Ajax](https://www.asp.net/terms-of-use "Términos de uso de la red CDN de Microsoft Ajax").</span><span class="sxs-lookup"><span data-stu-id="23630-451">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
