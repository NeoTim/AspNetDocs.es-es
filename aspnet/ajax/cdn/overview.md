---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: c153dd56fea6f19a818f8785691b022c90391b71
ms.sourcegitcommit: a256895f6160acc28d75424b8ab5d03b4e74412e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67471398"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="86251-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="86251-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="86251-103">Las aplicaciones de producción no deberían adoptar una dependencia fuerte de activos de la red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="86251-104">Las aplicaciones deben comprobar el recurso de la red CDN al que hace referencia y utilizar un recurso de reserva cuando la red CDN no está disponible.</span><span class="sxs-lookup"><span data-stu-id="86251-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="86251-105">La CDN de Microsoft Ajax no tiene ningún SLA más allá de una red CDN de Azure.</span><span class="sxs-lookup"><span data-stu-id="86251-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="86251-106">Use [este problema de GitHub](https://github.com/aspnet/AspNetDocs/issues/116) para notificar los problemas con la red CDN de Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="86251-106">Use [this GitHub issue](https://github.com/aspnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="86251-107">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="86251-107">Table of Contents</span></span>

<span data-ttu-id="86251-108">**[AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="86251-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="86251-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="86251-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="86251-110">**[Uso de Ajax de ASP.NET desde la red CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="86251-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="86251-111">**[Uso de jQuery desde la red CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="86251-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="86251-112">**[Mediante jQuery UI desde la red CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="86251-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="86251-113">**[Archivos de terceros en la red CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="86251-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="86251-114">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="86251-115">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="86251-116">las versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="86251-117">las versiones de validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="86251-118">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="86251-119">las versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="86251-120">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="86251-121">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="86251-122">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="86251-123">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="86251-124">Versiones de knockout en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="86251-125">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="86251-126">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="86251-127">Versiones de bootstrap en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="86251-128">Versiones de bootstrap TouchCarousel en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="86251-129">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="86251-130">Formularios Web Forms ASP.NET y las versiones de Ajax en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="86251-131">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="86251-132">ASP.NET SignalR se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="86251-133">Microsoft Ajax Content Delivery Network (CDN) hospeda las bibliotecas de JavaScript de terceros populares como jQuery y podrá agregarlos fácilmente a sus aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="86251-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="86251-134">Por ejemplo, puede empezar a usar jQuery que se hospeda en esta red CDN mediante la adición de una etiqueta &lt;script&gt; a la página que apunta a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="86251-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="86251-135">Aprovechando las ventajas de la red CDN, puede mejorar significativamente el rendimiento de las aplicaciones Ajax.</span><span class="sxs-lookup"><span data-stu-id="86251-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="86251-136">El contenido de la red CDN se almacena en caché en servidores ubicados en todo el mundo.</span><span class="sxs-lookup"><span data-stu-id="86251-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="86251-137">Además, la red CDN permite que los exploradores reutilicen los archivos de JavaScript de terceros en caché para sitios web que se encuentran en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="86251-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="86251-138">La red CDN admite SSL (HTTPS) en caso de que necesite servir una página web mediante la capa de Sockets seguros.</span><span class="sxs-lookup"><span data-stu-id="86251-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="86251-139">La red CDN hospeda las siguientes bibliotecas de scripts de terceros que se han cargado y se licencian a usted, los propietarios de esas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="86251-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="86251-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="86251-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="86251-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="86251-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="86251-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="86251-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="86251-143">Validación (www.jquery.com) de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="86251-144">Ciclo de jQuery (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="86251-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="86251-145">DataTables de jQuery (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="86251-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="86251-146">Microsoft Ajax CDN también incluye las siguientes bibliotecas que se han cargado por Microsoft:</span><span class="sxs-lookup"><span data-stu-id="86251-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="86251-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="86251-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="86251-148">Archivos de JavaScript de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="86251-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="86251-149">Archivos SignalR JavaScript ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86251-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="86251-150">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="86251-151">Los propietarios de las bibliotecas de derechos de autor licencian estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="86251-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="86251-152">Se conceden los derechos que es posible que deba descargar y usar estas bibliotecas únicamente por los propietarios de propiedad intelectual respectivos.</span><span class="sxs-lookup"><span data-stu-id="86251-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="86251-153">Dado que no son las bibliotecas de Microsoft, Microsoft no ofrece ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patente implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="86251-154">Si desea enviar la biblioteca de JavaScript y la biblioteca es una de las bibliotecas de JavaScript superiores (como se muestra en http://trends.builtwith.com) o complementos o extensiones para estas bibliotecas son (a) populares; o (b) útil para su uso en ASP.NET, a continuación, póngase en contacto con AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="86251-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="86251-155">AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="86251-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="86251-156">La red CDN usa el nombre de dominio microsoft.com y se ha cambiado para usar el nombre de dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="86251-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="86251-157">Este cambio se realizó para aumentar el rendimiento porque cuando un explorador al que hace referencia el dominio microsoft.com tendría que enviar las cookies desde ese dominio a través de la conexión con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="86251-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="86251-158">Al cambiar el nombre de dominio que no sea de microsoft.com, el rendimiento se puede aumentar hasta un 25%.</span><span class="sxs-lookup"><span data-stu-id="86251-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="86251-159">Tenga en cuenta que ajax.microsoft.com seguirá funcionando pero se recomienda ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="86251-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="86251-160">Formato antiguo: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="86251-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="86251-161">Nuevo formato: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="86251-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="86251-162">Compatibilidad de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="86251-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="86251-163">Para usar los archivos de .vsdoc correctamente con Visual Studio 2008 debe asegurarse de que dispone de VS 2008 SP1, instale y la revisión de los archivos de vsdoc.</span><span class="sxs-lookup"><span data-stu-id="86251-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="86251-164">Puede obtener desde aquí:</span><span class="sxs-lookup"><span data-stu-id="86251-164">You can get these from here:</span></span>

- [<span data-ttu-id="86251-165">Descargar Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="86251-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "descargar Visual Studio 2008 SP1")
- [<span data-ttu-id="86251-166">Descargar la revisión de .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="86251-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "descargar la revisión de .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="86251-167">Visual Studio 2010 es compatible con archivos .vsdoc sin las revisiones adicionales.</span><span class="sxs-lookup"><span data-stu-id="86251-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="86251-168">Uso de Ajax de ASP.NET desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="86251-169">Cuando se usa ASP.NET 4, puede redirigir todas las solicitudes de scripts del marco ASP.NET a la red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="86251-170">Recuperando las secuencias de comandos de la red CDN en lugar de su servidor web local puede mejorar considerablemente el rendimiento de sitios Web públicos de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="86251-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="86251-171">Utilice la propiedad ScriptManager EnableCDN para redirigir todas las solicitudes de script de marco ASP.NET a la red CDN Ajax de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="86251-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="86251-172">Uso de jQuery desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="86251-173">Puede usar scripts de jQuery hospedados en la red CDN en su aplicación Web agregando el siguiente elemento de secuencia de comandos a una página:</span><span class="sxs-lookup"><span data-stu-id="86251-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="86251-174">La red CDN también incluye la versión minimizada de la secuencia de comandos de jQuery, que se puede obtener mediante el siguiente elemento:</span><span class="sxs-lookup"><span data-stu-id="86251-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="86251-175">Para permitir que la página de retroceder a la carga de jQuery desde una ruta de acceso local en su propio sitio Web si se produce la red CDN no está disponible, agregue el siguiente elemento inmediatamente después del elemento que hacen referencia a la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="86251-176">La página de ejemplo siguiente usa la versión de la red CDN de la biblioteca de jQuery (con la reserva de una copia local) para mostrar el contenido de un elemento div cuando se hace clic en un botón.</span><span class="sxs-lookup"><span data-stu-id="86251-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="86251-177">Puede obtener más información acerca de jQuery y descargar una copia local visitando el [sitio Web](http://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="86251-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="86251-178">Mediante jQuery UI desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="86251-179">La red CDN también hospeda la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="86251-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="86251-180">La biblioteca de jQuery UI incluye un amplio conjunto de widgets y efectos que puede usar en sus aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="86251-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="86251-181">Por ejemplo, la siguiente página muestra cómo puede usar jQuery Datepicker de la interfaz de usuario en el contexto de una aplicación de formularios ASP.NET Web Forms para mostrar un calendario emergente:</span><span class="sxs-lookup"><span data-stu-id="86251-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="86251-182">Al mover el foco al cuadro de texto con el teclado, se muestra un calendario:</span><span class="sxs-lookup"><span data-stu-id="86251-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario emergente creado por Datepicker](overview/_static/image1.png)

<span data-ttu-id="86251-184">Tenga en cuenta que debe incluir tres archivos de la red CDN en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="86251-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="86251-185">La biblioteca de jQuery &mdash; la biblioteca de interfaz de usuario de jQuery depende de la biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="86251-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="86251-186">Debe agregar la biblioteca de jQuery a la página antes de agregar la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="86251-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="86251-187">La biblioteca de jQuery UI &mdash; la biblioteca de jQuery UI (o interfaz de usuario) contiene todos los efectos de la interfaz de usuario de jQuery y widgets, como el widget Datepicker utilizado en la página anterior.</span><span class="sxs-lookup"><span data-stu-id="86251-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="86251-188">Un tema de la interfaz de usuario de jQuery &mdash; la interfaz de usuario de jQuery es compatible con diferentes temas.</span><span class="sxs-lookup"><span data-stu-id="86251-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="86251-189">La página anterior incluye un vínculo a un archivo CSS para importar el tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="86251-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="86251-190">Todos los temas de la interfaz de usuario estándar de jQuery se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="86251-191">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en la red CDN de Microsoft Ajax") para ver las miniaturas para cada tema.</span><span class="sxs-lookup"><span data-stu-id="86251-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="86251-192">Para obtener más información acerca de la biblioteca de jQuery UI, visite el sitio oficial [de la interfaz de usuario de jQuery](http://jQueryUI.com "sitio Web de la interfaz de usuario de jQuery").</span><span class="sxs-lookup"><span data-stu-id="86251-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="86251-193">Archivos de terceros en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="86251-194">La red CDN hospeda algunas de las bibliotecas de JavaScript de terceros más populares.</span><span class="sxs-lookup"><span data-stu-id="86251-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="86251-195">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="86251-196">Los propietarios de las bibliotecas de derechos de autor licencian estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="86251-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="86251-197">Se conceden los derechos que es posible que deba descargar y usar estas bibliotecas únicamente por los propietarios de propiedad intelectual respectivos.</span><span class="sxs-lookup"><span data-stu-id="86251-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="86251-198">Dado que no son las bibliotecas de Microsoft, Microsoft no ofrece ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patente implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="86251-199">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="86251-200">Las siguientes versiones de jQuery se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-341"></a><span data-ttu-id="86251-201">versión de jQuery 3.4.1</span><span class="sxs-lookup"><span data-stu-id="86251-201">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="86251-202">versión de jQuery 3.4.0</span><span class="sxs-lookup"><span data-stu-id="86251-202">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="86251-203">versión de jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="86251-203">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="86251-204">versión de jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="86251-204">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="86251-205">versión de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="86251-205">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="86251-206">versión de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-206">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="86251-207">versión de jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="86251-207">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="86251-208">jQuery versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="86251-208">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="86251-209">versión de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="86251-209">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="86251-210">versión 2.2.3 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-210">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="86251-211">versión 2.2.2 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-211">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="86251-212">versión 2.2.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-212">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="86251-213">versión 2.2.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-213">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="86251-214">versión de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="86251-214">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="86251-215">versión 2.1.3 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-215">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="86251-216">versión de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="86251-216">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="86251-217">versión de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-217">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="86251-218">versión 2.1.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-218">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="86251-219">versión de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="86251-219">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="86251-220">versión de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="86251-220">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="86251-221">versión de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="86251-221">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="86251-222">versión 2.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-222">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="86251-223">versión de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="86251-223">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="86251-224">versión de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="86251-224">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="86251-225">versión de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="86251-225">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="86251-226">versión de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="86251-226">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="86251-227">versión de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="86251-227">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="86251-228">versión de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="86251-228">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="86251-229">versión de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="86251-229">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="86251-230">versión de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="86251-230">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="86251-231">versión de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="86251-231">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="86251-232">versión de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="86251-232">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="86251-233">versión de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="86251-233">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="86251-234">versión de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="86251-234">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="86251-235">versión de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="86251-235">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="86251-236">versión 1.9.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-236">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="86251-237">versión de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="86251-237">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="86251-238">versión de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="86251-238">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="86251-239">versión 1.8.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-239">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="86251-240">versión de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="86251-240">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="86251-241">versión de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="86251-241">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="86251-242">versión de jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="86251-242">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="86251-243">versión de jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="86251-243">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="86251-244">versión de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="86251-244">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="86251-245">versión de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="86251-245">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="86251-246">versión de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="86251-246">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="86251-247">jQuery versión 1.6.1</span><span class="sxs-lookup"><span data-stu-id="86251-247">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="86251-248">versión de jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="86251-248">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="86251-249">versión de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="86251-249">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="86251-250">versión de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="86251-250">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="86251-251">versión de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="86251-251">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="86251-252">versión de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="86251-252">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="86251-253">versión de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="86251-253">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="86251-254">versión de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="86251-254">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="86251-255">versión 1.4.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-255">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="86251-256">versión 1.4 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-256">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="86251-257">versión 1.3.2 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-257">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="86251-258">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-258">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="86251-259">Las siguientes versiones de jQuery Migrate se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-259">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="86251-260">Migrar la versión 3.0.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-260">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="86251-261">Migrar la versión 1.2.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-261">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="86251-262">Migrar la versión 1.2.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-262">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="86251-263">Migrar la versión 1.1.1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-263">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="86251-264">Migrar la versión 1.1.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-264">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="86251-265">Migrar la versión 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-265">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="86251-266">las versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-266">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="86251-267">Las siguientes versiones de la biblioteca de interfaz de usuario de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-267">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="86251-268">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-268">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-269">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="86251-269">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-270">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="86251-270">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-271">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="86251-271">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-272">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="86251-272">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-273">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="86251-273">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-274">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="86251-274">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-275">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="86251-275">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-276">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="86251-276">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-277">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="86251-277">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-278">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="86251-278">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-279">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="86251-279">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-280">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="86251-280">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-281">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="86251-281">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-282">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="86251-282">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-283">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="86251-283">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-284">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="86251-284">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-285">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="86251-285">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-286">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="86251-286">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-287">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="86251-287">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-288">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="86251-288">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-289">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="86251-289">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-290">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="86251-290">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-291">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="86251-291">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-292">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="86251-292">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-293">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="86251-293">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-294">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="86251-294">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-295">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="86251-295">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-296">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="86251-296">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-297">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="86251-297">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-298">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="86251-298">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-299">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="86251-299">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-300">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="86251-300">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-301">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="86251-301">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-302">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="86251-302">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-303">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="86251-303">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="86251-304">las versiones de validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-304">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="86251-305">Las siguientes versiones de la biblioteca de validación de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-305">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="86251-306">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-306">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-307">1.19.1 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-307">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "1.19.1 de validación de jQuery")
- [<span data-ttu-id="86251-308">1.19.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-308">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "1.19.0 de validación de jQuery")
- [<span data-ttu-id="86251-309">1.17.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-309">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 de validación de jQuery")
- [<span data-ttu-id="86251-310">1.16.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-310">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "1.16.0 de validación de jQuery")
- [<span data-ttu-id="86251-311">1.15.1 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-311">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "1.15.1 de validación de jQuery")
- [<span data-ttu-id="86251-312">1.15.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-312">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "1.15.0 de validación de jQuery")
- [<span data-ttu-id="86251-313">1.14.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-313">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "1.14.0 de validación de jQuery")
- [<span data-ttu-id="86251-314">1.13.1 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-314">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "1.13.1 de validación de jQuery")
- [<span data-ttu-id="86251-315">1.13.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-315">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "1.13.0 de validación de jQuery")
- [<span data-ttu-id="86251-316">1.12.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-316">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "1.12.0 de validación de jQuery")
- [<span data-ttu-id="86251-317">1.11.1 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-317">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "1.11.1 de validación de jQuery")
- [<span data-ttu-id="86251-318">1.11.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-318">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "1.11.0 de validación de jQuery")
- [<span data-ttu-id="86251-319">1.10.0 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-319">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "1.10.0 de validación de jQuery")
- [<span data-ttu-id="86251-320">1.9 validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-320">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "versión 1.9 de jquery.validate")
- [<span data-ttu-id="86251-321">1.8.1 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-321">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "versión 1.8.1 de jquery.validate")
- [<span data-ttu-id="86251-322">1.8 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-322">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "versión 1.8 de jquery.validate")
- [<span data-ttu-id="86251-323">1.7 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-323">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "versión 1.7 de jquery.validate")
- [<span data-ttu-id="86251-324">1.6 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-324">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "1.6 de validación de jQuery")
- [<span data-ttu-id="86251-325">1.5.5 de validación de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-325">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "1.5.5 de validación de jQuery")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="86251-326">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-326">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="86251-327">Las siguientes versiones de la biblioteca de jQuery Mobile se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-327">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="86251-328">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-328">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-329">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="86251-329">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-330">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="86251-330">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-331">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="86251-331">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-332">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="86251-332">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-333">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="86251-333">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-334">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="86251-334">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-335">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="86251-335">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-336">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="86251-336">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-337">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="86251-337">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-338">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-338">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-339">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="86251-339">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-340">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="86251-340">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-341">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="86251-341">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-342">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="86251-342">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-343">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="86251-343">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-344">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="86251-344">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 en la red CDN de Microsoft Ajax")
- [<span data-ttu-id="86251-345">versión beta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="86251-345">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 en la red CDN de Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="86251-346">las versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-346">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="86251-347">Las siguientes versiones del complemento de plantillas de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-347">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="86251-348">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-349">las plantillas de versión Beta 1 de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-349">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery plantillas Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="86251-350">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="86251-350">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="86251-351">Las siguientes versiones del complemento de ciclo de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-351">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="86251-352">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-352">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-353">jQuery ciclo 2,99</span><span class="sxs-lookup"><span data-stu-id="86251-353">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="86251-354">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="86251-354">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "2.94 de ciclo de jQuery")
- [<span data-ttu-id="86251-355">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="86251-355">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="86251-356">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-356">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="86251-357">Las siguientes versiones del complemento DataTables de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-357">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="86251-358">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-358">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-359">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="86251-359">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "1.10.5 DataTables de jQuery")
- [<span data-ttu-id="86251-360">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="86251-360">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "1.10.4 DataTables de jQuery")
- [<span data-ttu-id="86251-361">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="86251-361">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "1.9.4 DataTables de jQuery")
- [<span data-ttu-id="86251-362">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="86251-362">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "1.9.3 DataTables de jQuery")
- [<span data-ttu-id="86251-363">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="86251-363">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "1.9.2 DataTables de jQuery")
- [<span data-ttu-id="86251-364">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="86251-364">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery 1.9.1 DataTables")
- [<span data-ttu-id="86251-365">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="86251-365">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "1.9.0 DataTables de jQuery")
- [<span data-ttu-id="86251-366">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="86251-366">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "1.8.2 DataTables de jQuery")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="86251-367">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-367">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="86251-368">Las siguientes versiones de [Modernizr](http://www.modernizr.com "Modernizr") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-368">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="86251-369">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-369">JSHint Releases on the CDN</span></span>

<span data-ttu-id="86251-370">Las siguientes versiones de [JSHint](http://www.jshint.com "JSHint") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-370">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="86251-371">Versiones de knockout en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-371">Knockout Releases on the CDN</span></span>

<span data-ttu-id="86251-372">Las siguientes versiones de [Knockout](http://www.knockoutjs.com "Knockout") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-372">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

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

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="86251-373">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-373">Globalize Releases on the CDN</span></span>

<span data-ttu-id="86251-374">Las siguientes versiones de [Globalize](https://github.com/jquery/globalize "Globalize") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-374">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="86251-375">Globalizar versión 1.0.0</span><span class="sxs-lookup"><span data-stu-id="86251-375">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="86251-376">Globalizar versión 0.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-376">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="86251-377">Todas las referencias culturales</span><span class="sxs-lookup"><span data-stu-id="86251-377">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="86251-378">Reemplace "{-código de referencia cultural}" con el código de la referencia cultural deseada, por ejemplo, Microsoft globalize.culture.en GB.js== archivos en la red CDN == estas bibliotecas se cargaron por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="86251-378">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="86251-379">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-379">Respond Releases on the CDN</span></span>

<span data-ttu-id="86251-380">Los siguientes versiones de [responder](https://github.com/scottjehl/Respond "responder") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-380">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="86251-381">Responder versión 1.4.2</span><span class="sxs-lookup"><span data-stu-id="86251-381">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="86251-382">Versión 1.4.1 de responder</span><span class="sxs-lookup"><span data-stu-id="86251-382">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="86251-383">Responder a la versión 1.4.0</span><span class="sxs-lookup"><span data-stu-id="86251-383">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="86251-384">Responder a la versión 1.3.0</span><span class="sxs-lookup"><span data-stu-id="86251-384">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="86251-385">Responder a la versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="86251-385">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="86251-386">Versiones de bootstrap en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-386">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="86251-387">Las siguientes versiones de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-387">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-431"></a><span data-ttu-id="86251-388">Bootstrap versión 4.3.1</span><span class="sxs-lookup"><span data-stu-id="86251-388">Bootstrap version 4.3.1</span></span>

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

#### <a name="bootstrap-version-421"></a><span data-ttu-id="86251-389">Versión 4.2.1 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="86251-389">Bootstrap version 4.2.1</span></span>

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

#### <a name="bootstrap-version-411"></a><span data-ttu-id="86251-390">Versión de bootstrap 4.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-390">Bootstrap version 4.1.1</span></span>

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

#### <a name="bootstrap-version-400"></a><span data-ttu-id="86251-391">Bootstrap versión 4.0.0</span><span class="sxs-lookup"><span data-stu-id="86251-391">Bootstrap version 4.0.0</span></span>

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

#### <a name="bootstrap-version-341"></a><span data-ttu-id="86251-392">Versión de bootstrap 3.4.1</span><span class="sxs-lookup"><span data-stu-id="86251-392">Bootstrap version 3.4.1</span></span>

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

#### <a name="bootstrap-version-340"></a><span data-ttu-id="86251-393">Bootstrap versión 3.4.0</span><span class="sxs-lookup"><span data-stu-id="86251-393">Bootstrap version 3.4.0</span></span>

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

#### <a name="bootstrap-version-337"></a><span data-ttu-id="86251-394">Versión de bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="86251-394">Bootstrap version 3.3.7</span></span>

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

#### <a name="bootstrap-version-336"></a><span data-ttu-id="86251-395">Versión de bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="86251-395">Bootstrap version 3.3.6</span></span>

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

#### <a name="bootstrap-version-335"></a><span data-ttu-id="86251-396">Versión de bootstrap 3.3.5</span><span class="sxs-lookup"><span data-stu-id="86251-396">Bootstrap version 3.3.5</span></span>

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

#### <a name="bootstrap-version-334"></a><span data-ttu-id="86251-397">Versión de bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="86251-397">Bootstrap version 3.3.4</span></span>

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

#### <a name="bootstrap-version-332"></a><span data-ttu-id="86251-398">Versión de bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="86251-398">Bootstrap version 3.3.2</span></span>

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

#### <a name="bootstrap-version-331"></a><span data-ttu-id="86251-399">Versión de bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="86251-399">Bootstrap version 3.3.1</span></span>

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

#### <a name="bootstrap-version-330"></a><span data-ttu-id="86251-400">Bootstrap versión 3.3.0</span><span class="sxs-lookup"><span data-stu-id="86251-400">Bootstrap version 3.3.0</span></span>

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

#### <a name="bootstrap-version-320"></a><span data-ttu-id="86251-401">Versión de bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="86251-401">Bootstrap version 3.2.0</span></span>

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

#### <a name="bootstrap-version-311"></a><span data-ttu-id="86251-402">Bootstrap versión 3.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-402">Bootstrap version 3.1.1</span></span>

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

#### <a name="bootstrap-version-310"></a><span data-ttu-id="86251-403">Bootstrap versión 3.1.0</span><span class="sxs-lookup"><span data-stu-id="86251-403">Bootstrap version 3.1.0</span></span>

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

#### <a name="bootstrap-version-303"></a><span data-ttu-id="86251-404">Versión de bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="86251-404">Bootstrap version 3.0.3</span></span>

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

#### <a name="bootstrap-version-302"></a><span data-ttu-id="86251-405">Versión de bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="86251-405">Bootstrap version 3.0.2</span></span>

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

#### <a name="bootstrap-version-301"></a><span data-ttu-id="86251-406">Bootstrap versión 3.0.1</span><span class="sxs-lookup"><span data-stu-id="86251-406">Bootstrap version 3.0.1</span></span>

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

#### <a name="bootstrap-version-300"></a><span data-ttu-id="86251-407">Bootstrap versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="86251-407">Bootstrap version 3.0.0</span></span>

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

#### <a name="bootstrap-version-232"></a><span data-ttu-id="86251-408">Versión 2.3.2 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="86251-408">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="86251-409">Bootstrap versión 2.3.1</span><span class="sxs-lookup"><span data-stu-id="86251-409">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="86251-410">Versiones de bootstrap TouchCarousel en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-410">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="86251-411">Las siguientes versiones de [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-411">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="86251-412">Bootstrap TouchCarousel versión 0.8.0</span><span class="sxs-lookup"><span data-stu-id="86251-412">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="86251-413">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-413">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="86251-414">Las siguientes versiones de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versiones se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-414">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="86251-415">Hammer.js versión 2.0.4</span><span class="sxs-lookup"><span data-stu-id="86251-415">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="86251-416">Formularios Web Forms ASP.NET y las versiones de Ajax en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-416">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="86251-417">Las siguientes versiones de la biblioteca de Ajax de ASP.NET se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="86251-417">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="86251-418">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="86251-418">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="86251-419">Versión de formularios Web Forms ASP.NET y Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="86251-419">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularios Web Forms ASP.NET y Ajax 4.5.2")
- [<span data-ttu-id="86251-420">Versión de formularios Web Forms ASP.NET y Ajax 4</span><span class="sxs-lookup"><span data-stu-id="86251-420">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularios Web Forms ASP.NET y Ajax 4")
- [<span data-ttu-id="86251-421">Ajax de ASP.NET versión 3.5</span><span class="sxs-lookup"><span data-stu-id="86251-421">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax de ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="86251-422">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-422">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="86251-423">Los siguientes archivos JavaScript de ASP.NET MVC se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-423">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="86251-424">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="86251-424">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="86251-425">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="86251-425">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="86251-426">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="86251-426">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="86251-427">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="86251-427">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="86251-428">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="86251-428">ASP.NET MVC 3.0</span></span>

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

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="86251-429">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="86251-429">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="86251-430">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="86251-430">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="86251-431">ASP.NET SignalR se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="86251-431">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="86251-432">Los siguientes archivos JavaScript SignalR de ASP.NET se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="86251-432">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="86251-433">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="86251-433">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="86251-434">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="86251-434">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="86251-435">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="86251-435">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="86251-436">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="86251-436">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="86251-437">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="86251-437">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="86251-438">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="86251-438">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="86251-439">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="86251-439">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="86251-440">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="86251-440">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="86251-441">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="86251-441">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="86251-442">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="86251-442">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="86251-443">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="86251-443">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="86251-444">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="86251-444">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="86251-445">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="86251-445">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="86251-446">Para obtener información acerca de los términos de uso de la red CDN, consulte [Microsoft Ajax CDN términos de uso](https://www.asp.net/terms-of-use "Ajax CDN términos de uso de Microsoft").</span><span class="sxs-lookup"><span data-stu-id="86251-446">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
