---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar AJAX para entregar actualizaciones dinámicas ? Microsoft Docs
author: rick-anderson
description: El paso 10 implementa soporte para los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, utilizando un enfoque basado en Ajax integrado dentro del detalle de la cena...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 13680b91edc665852fd4af56e4fc79551e6de15e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541252"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="59423-103">Usar AJAX para entregar actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="59423-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="59423-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59423-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="59423-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="59423-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="59423-106">Este es el paso 10 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="59423-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="59423-107">El paso 10 implementa soporte para los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, utilizando un enfoque basado en Ajax integrado en la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="59423-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="59423-108">Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="59423-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="59423-109">NerdDinner Paso 10: AJAX Habilitación RSVPs acepta</span><span class="sxs-lookup"><span data-stu-id="59423-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="59423-110">Ahora vamos a implementar soporte para los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena.</span><span class="sxs-lookup"><span data-stu-id="59423-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="59423-111">Lo habilitaremos mediante un enfoque basado en AJAX integrado en la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="59423-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="59423-112">Indicando si el usuario es RSVP'd</span><span class="sxs-lookup"><span data-stu-id="59423-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="59423-113">Los usuarios pueden visitar la URL */Dinners/Details/[id*] para ver detalles sobre una cena en particular:</span><span class="sxs-lookup"><span data-stu-id="59423-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="59423-114">El método de acción Details() se implementa de la siguiente forma:</span><span class="sxs-lookup"><span data-stu-id="59423-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="59423-115">Nuestro primer paso para implementar la compatibilidad con RSVP será agregar un método auxiliar "IsUserRegistered(username)" a nuestro objeto Dinner (dentro del Dinner.cs clase parcial que creamos anteriormente).</span><span class="sxs-lookup"><span data-stu-id="59423-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="59423-116">Este método auxiliar devuelve true o false dependiendo de si el usuario es actualmente RSVP'd para la cena:</span><span class="sxs-lookup"><span data-stu-id="59423-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="59423-117">A continuación, podemos agregar el código siguiente a nuestra plantilla de vista Details.aspx para mostrar un mensaje adecuado que indica si el usuario está registrado o no para el evento:</span><span class="sxs-lookup"><span data-stu-id="59423-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="59423-118">Y ahora, cuando un usuario visita una Cena, está registrado, verá este mensaje:</span><span class="sxs-lookup"><span data-stu-id="59423-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="59423-119">Y cuando visiten una Cena no están registrados para ver el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="59423-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="59423-120">Implementación del método de acción de registro</span><span class="sxs-lookup"><span data-stu-id="59423-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="59423-121">Ahora vamos a agregar la funcionalidad necesaria para permitir a los usuarios a RSVP para una cena desde la página de detalles.</span><span class="sxs-lookup"><span data-stu-id="59423-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="59423-122">Para implementar esto, vamos a crear una nueva clase "RSVPController" haciendo clic con&gt;el botón derecho en el directorio de controladores y eligiendo el comando de menú Agregar controlador.</span><span class="sxs-lookup"><span data-stu-id="59423-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="59423-123">Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto Dinner adecuado, comprueba si el usuario que ha iniciado sesión está actualmente en la lista de usuarios que se han registrado para él y, si no, agrega un objeto RSVP para ellos:</span><span class="sxs-lookup"><span data-stu-id="59423-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="59423-124">Observe anteriormente cómo estamos devolviendo una cadena simple como salida del método de acción.</span><span class="sxs-lookup"><span data-stu-id="59423-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="59423-125">Podríamos haber incrustado este mensaje dentro de una plantilla de vista, pero como es tan pequeño, solo usaremos el método auxiliar Content() en la clase base del controlador y devolveremos un mensaje de cadena como el anterior.</span><span class="sxs-lookup"><span data-stu-id="59423-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="59423-126">Llamar al método de acción RSVPForEvent mediante AJAX</span><span class="sxs-lookup"><span data-stu-id="59423-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="59423-127">Usaremos AJAX para invocar el método de acción Register desde nuestra vista Detalles.</span><span class="sxs-lookup"><span data-stu-id="59423-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="59423-128">Implementar esto es bastante fácil.</span><span class="sxs-lookup"><span data-stu-id="59423-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="59423-129">Primero agregaremos dos referencias de biblioteca de scripts:</span><span class="sxs-lookup"><span data-stu-id="59423-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="59423-130">La primera biblioteca hace referencia al núcleo ASP.NET biblioteca de scripts del lado cliente de AJAX.</span><span class="sxs-lookup"><span data-stu-id="59423-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="59423-131">Este archivo tiene aproximadamente 24k de tamaño (comprimido) y contiene la funcionalidad principal de AJAX del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="59423-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="59423-132">La segunda biblioteca contiene funciones de utilidad que se integran con ASP.NET métodos auxiliares AJAX integrados de MVC (que usaremos en breve).</span><span class="sxs-lookup"><span data-stu-id="59423-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="59423-133">A continuación, podemos actualizar el código de plantilla de vista que agregamos anteriormente para que en lugar de generar un mensaje "No está registrado para este evento", representamos un vínculo que cuando se inserta realiza una llamada AJAX que invoca nuestro método de acción RSVPForEvent en nuestro controlador RSVP y RSVP el usuario:</span><span class="sxs-lookup"><span data-stu-id="59423-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="59423-134">El método auxiliar Ajax.ActionLink() utilizado anteriormente está integrado en ASP.NET MVC y es similar al método auxiliar Html.ActionLink(), excepto que en lugar de realizar una navegación estándar realiza una llamada AJAX al método de acción cuando se hace clic en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="59423-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="59423-135">Arriba estamos llamando al método de acción "Register" en el controlador "RSVP" y pasando el DinnerID como el parámetro "id" a él.</span><span class="sxs-lookup"><span data-stu-id="59423-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="59423-136">El parámetro final AjaxOptions que estamos pasando indica que queremos tomar el &lt;contenido&gt; devuelto por el método de acción y actualizar el elemento div HTML en la página cuyo identificador es "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="59423-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="59423-137">Y ahora, cuando un usuario navega a una cena para la que aún no está registrado, verá un enlace a RSVP para ello:</span><span class="sxs-lookup"><span data-stu-id="59423-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="59423-138">Si hacen clic en el enlace "RSVP para este evento" harán una llamada AJAX al método de acción Register en el controlador RSVP, y cuando se complete verán un mensaje actualizado como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="59423-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="59423-139">El ancho de banda de red y el tráfico implicados al realizar esta llamada AJAX son realmente ligeros.</span><span class="sxs-lookup"><span data-stu-id="59423-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="59423-140">Cuando el usuario hace clic en el link "RSVP para este evento", una pequeña petición de red HTTP POST se hace a la *URL /Dinners/Register/1* que se parece a continuación en el cable:</span><span class="sxs-lookup"><span data-stu-id="59423-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="59423-141">Y la respuesta de nuestro método de acción Register es simplemente:</span><span class="sxs-lookup"><span data-stu-id="59423-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="59423-142">Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.</span><span class="sxs-lookup"><span data-stu-id="59423-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="59423-143">Adición de una animación jQuery</span><span class="sxs-lookup"><span data-stu-id="59423-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="59423-144">La funcionalidad AJAX que implementamos funciona bien y rápido.</span><span class="sxs-lookup"><span data-stu-id="59423-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="59423-145">A veces puede suceder tan rápido, sin embargo, que un usuario no pudo notar que el link RSVP se ha reemplazado con el nuevo texto.</span><span class="sxs-lookup"><span data-stu-id="59423-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="59423-146">Para que el resultado sea un poco más obvio, podemos añadir una animación sencilla para llamar la atención sobre el mensaje de actualización.</span><span class="sxs-lookup"><span data-stu-id="59423-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="59423-147">La plantilla de proyecto ASP.NET MVC predeterminada incluye jQuery: una excelente (y muy popular) biblioteca JavaScript de código abierto que también es compatible con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="59423-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="59423-148">jQuery proporciona una serie de características, incluyendo una buena selección HTML DOM y biblioteca de efectos.</span><span class="sxs-lookup"><span data-stu-id="59423-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="59423-149">Para usar jQuery primero agregaremos una referencia de script a él.</span><span class="sxs-lookup"><span data-stu-id="59423-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="59423-150">Debido a que vamos a usar jQuery dentro de una variedad de lugares dentro de nuestro sitio, agregaremos la referencia de script dentro de nuestro archivo de página maestra Site.master para que todas las páginas puedan usarlo.</span><span class="sxs-lookup"><span data-stu-id="59423-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="59423-151">*Consejo: asegúrese de que ha instalado la revisión intellisense de JavaScript para VS 2008 SP1 que permite una compatibilidad más completa con intellisense para archivos JavaScript (incluida jQuery). Puede descargarlo desde:http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="59423-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="59423-152">El código escrito con JQuery a menudo utiliza un método JavaScript global "$()" que recupera uno o más elementos HTML mediante un selector CSS.</span><span class="sxs-lookup"><span data-stu-id="59423-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="59423-153">Por ejemplo, *$("#rsvpmsg")* selecciona cualquier elemento HTML con el id de rsvpmsg, mientras que *$(".something")* seleccionaría todos los elementos con el nombre de clase CSS "algo".</span><span class="sxs-lookup"><span data-stu-id="59423-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="59423-154">También puede escribir consultas más avanzadas como "devolver todos los botones de opción marcados" utilizando una consulta de selector como: *$("input[@type@checked?radio][ ]")*.</span><span class="sxs-lookup"><span data-stu-id="59423-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="59423-155">Una vez que haya seleccionado elementos, puede llamar a métodos para que tomen medidas, como ocultarlos: *$("#rsvpmsg"). hide();*</span><span class="sxs-lookup"><span data-stu-id="59423-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="59423-156">Para nuestro escenario RSVP, definiremos una función JavaScript simple denominada "AnimateRSVPMessage" &lt;que&gt; selecciona el div "rsvpmsg" y anima el tamaño de su contenido de texto.</span><span class="sxs-lookup"><span data-stu-id="59423-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="59423-157">El siguiente código inicia el texto pequeño y luego hace que aumente durante un período de tiempo de 400 milisegundos:</span><span class="sxs-lookup"><span data-stu-id="59423-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="59423-158">A continuación, podemos conectar esta función JavaScript a la que se llamará después de que nuestra llamada AJAX se complete correctamente pasando su nombre a nuestro método auxiliar Ajax.ActionLink() (a través de la propiedad de evento AjaxOptions "OnSuccess"):</span><span class="sxs-lookup"><span data-stu-id="59423-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="59423-159">Y ahora, cuando se hace clic en el enlace "RSVP para este evento" y nuestra llamada AJAX se completa correctamente, el mensaje de contenido enviado de vuelta animará y crecerá grande:</span><span class="sxs-lookup"><span data-stu-id="59423-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="59423-160">Además de proporcionar un evento "OnSuccess", el objeto AjaxOptions expone eventos OnBegin, OnFailure y OnComplete que puede controlar (junto con una variedad de otras propiedades y opciones útiles).</span><span class="sxs-lookup"><span data-stu-id="59423-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="59423-161">Limpieza - Refactorizar una vista parcial RSVP</span><span class="sxs-lookup"><span data-stu-id="59423-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="59423-162">Nuestra plantilla de vista de detalles está empezando a ser un poco larga, lo que las horas extras harán que sea un poco más difícil de entender.</span><span class="sxs-lookup"><span data-stu-id="59423-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="59423-163">Para ayudar a mejorar la legibilidad del código, terminemos creando una vista parcial, RSVPStatus.ascx, que encapsula todo el código de vista RSVP para nuestra página Detalles.</span><span class="sxs-lookup"><span data-stu-id="59423-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="59423-164">Para ello, haga clic con el botón derecho en la carpeta&gt;"Vistas" y, a continuación, elija el comando de menú Agregar- Vista.</span><span class="sxs-lookup"><span data-stu-id="59423-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="59423-165">Haremos que tome un objeto Dinner como su ViewModel fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="59423-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="59423-166">A continuación, podemos copiar y pegar el contenido RSVP de nuestra vista Details.aspx en él.</span><span class="sxs-lookup"><span data-stu-id="59423-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="59423-167">Una vez hecho eso, vamos a crear también otra vista parcial , EditAndDeleteLinks.ascx , que encapsula nuestro código de vista de vínculo Editar y eliminar.</span><span class="sxs-lookup"><span data-stu-id="59423-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="59423-168">También haremos que tome un objeto Dinner como su ViewModel fuertemente tipado y copie y pegue la lógica Edit and Delete de nuestra vista Details.aspx en él.</span><span class="sxs-lookup"><span data-stu-id="59423-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="59423-169">Nuestra plantilla de vista de detalles sólo puede incluir dos llamadas al método Html.RenderPartial() en la parte inferior:</span><span class="sxs-lookup"><span data-stu-id="59423-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="59423-170">Esto hace que el código sea más limpio para leer y mantener.</span><span class="sxs-lookup"><span data-stu-id="59423-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="59423-171">siguiente paso</span><span class="sxs-lookup"><span data-stu-id="59423-171">Next Step</span></span>

<span data-ttu-id="59423-172">Veamos ahora cómo podemos usar AJAX aún más y agregar compatibilidad con mapas interactivos a nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="59423-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="59423-173">[Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="59423-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
