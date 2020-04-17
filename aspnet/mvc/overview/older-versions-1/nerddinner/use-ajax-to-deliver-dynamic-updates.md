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
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Usar AJAX para entregar actualizaciones dinámicas

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 10 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 10 implementa soporte para los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, utilizando un enfoque basado en Ajax integrado en la página de detalles de la cena.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner Paso 10: AJAX Habilitación RSVPs acepta

Ahora vamos a implementar soporte para los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena. Lo habilitaremos mediante un enfoque basado en AJAX integrado en la página de detalles de la cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Indicando si el usuario es RSVP'd

Los usuarios pueden visitar la URL */Dinners/Details/[id*] para ver detalles sobre una cena en particular:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

El método de acción Details() se implementa de la siguiente forma:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Nuestro primer paso para implementar la compatibilidad con RSVP será agregar un método auxiliar "IsUserRegistered(username)" a nuestro objeto Dinner (dentro del Dinner.cs clase parcial que creamos anteriormente). Este método auxiliar devuelve true o false dependiendo de si el usuario es actualmente RSVP'd para la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

A continuación, podemos agregar el código siguiente a nuestra plantilla de vista Details.aspx para mostrar un mensaje adecuado que indica si el usuario está registrado o no para el evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Y ahora, cuando un usuario visita una Cena, está registrado, verá este mensaje:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Y cuando visiten una Cena no están registrados para ver el siguiente mensaje:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementación del método de acción de registro

Ahora vamos a agregar la funcionalidad necesaria para permitir a los usuarios a RSVP para una cena desde la página de detalles.

Para implementar esto, vamos a crear una nueva clase "RSVPController" haciendo clic con&gt;el botón derecho en el directorio de controladores y eligiendo el comando de menú Agregar controlador.

Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto Dinner adecuado, comprueba si el usuario que ha iniciado sesión está actualmente en la lista de usuarios que se han registrado para él y, si no, agrega un objeto RSVP para ellos:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Observe anteriormente cómo estamos devolviendo una cadena simple como salida del método de acción. Podríamos haber incrustado este mensaje dentro de una plantilla de vista, pero como es tan pequeño, solo usaremos el método auxiliar Content() en la clase base del controlador y devolveremos un mensaje de cadena como el anterior.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Llamar al método de acción RSVPForEvent mediante AJAX

Usaremos AJAX para invocar el método de acción Register desde nuestra vista Detalles. Implementar esto es bastante fácil. Primero agregaremos dos referencias de biblioteca de scripts:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La primera biblioteca hace referencia al núcleo ASP.NET biblioteca de scripts del lado cliente de AJAX. Este archivo tiene aproximadamente 24k de tamaño (comprimido) y contiene la funcionalidad principal de AJAX del lado cliente. La segunda biblioteca contiene funciones de utilidad que se integran con ASP.NET métodos auxiliares AJAX integrados de MVC (que usaremos en breve).

A continuación, podemos actualizar el código de plantilla de vista que agregamos anteriormente para que en lugar de generar un mensaje "No está registrado para este evento", representamos un vínculo que cuando se inserta realiza una llamada AJAX que invoca nuestro método de acción RSVPForEvent en nuestro controlador RSVP y RSVP el usuario:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

El método auxiliar Ajax.ActionLink() utilizado anteriormente está integrado en ASP.NET MVC y es similar al método auxiliar Html.ActionLink(), excepto que en lugar de realizar una navegación estándar realiza una llamada AJAX al método de acción cuando se hace clic en el vínculo. Arriba estamos llamando al método de acción "Register" en el controlador "RSVP" y pasando el DinnerID como el parámetro "id" a él. El parámetro final AjaxOptions que estamos pasando indica que queremos tomar el &lt;contenido&gt; devuelto por el método de acción y actualizar el elemento div HTML en la página cuyo identificador es "rsvpmsg".

Y ahora, cuando un usuario navega a una cena para la que aún no está registrado, verá un enlace a RSVP para ello:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Si hacen clic en el enlace "RSVP para este evento" harán una llamada AJAX al método de acción Register en el controlador RSVP, y cuando se complete verán un mensaje actualizado como el siguiente:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

El ancho de banda de red y el tráfico implicados al realizar esta llamada AJAX son realmente ligeros. Cuando el usuario hace clic en el link "RSVP para este evento", una pequeña petición de red HTTP POST se hace a la *URL /Dinners/Register/1* que se parece a continuación en el cable:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Y la respuesta de nuestro método de acción Register es simplemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.

### <a name="adding-a-jquery-animation"></a>Adición de una animación jQuery

La funcionalidad AJAX que implementamos funciona bien y rápido. A veces puede suceder tan rápido, sin embargo, que un usuario no pudo notar que el link RSVP se ha reemplazado con el nuevo texto. Para que el resultado sea un poco más obvio, podemos añadir una animación sencilla para llamar la atención sobre el mensaje de actualización.

La plantilla de proyecto ASP.NET MVC predeterminada incluye jQuery: una excelente (y muy popular) biblioteca JavaScript de código abierto que también es compatible con Microsoft. jQuery proporciona una serie de características, incluyendo una buena selección HTML DOM y biblioteca de efectos.

Para usar jQuery primero agregaremos una referencia de script a él. Debido a que vamos a usar jQuery dentro de una variedad de lugares dentro de nuestro sitio, agregaremos la referencia de script dentro de nuestro archivo de página maestra Site.master para que todas las páginas puedan usarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Consejo: asegúrese de que ha instalado la revisión intellisense de JavaScript para VS 2008 SP1 que permite una compatibilidad más completa con intellisense para archivos JavaScript (incluida jQuery). Puede descargarlo desde:http://tinyurl.com/vs2008javascripthotfix*

El código escrito con JQuery a menudo utiliza un método JavaScript global "$()" que recupera uno o más elementos HTML mediante un selector CSS. Por ejemplo, *$("#rsvpmsg")* selecciona cualquier elemento HTML con el id de rsvpmsg, mientras que *$(".something")* seleccionaría todos los elementos con el nombre de clase CSS "algo". También puede escribir consultas más avanzadas como "devolver todos los botones de opción marcados" utilizando una consulta de selector como: *$("input[@type@checked?radio][ ]")*.

Una vez que haya seleccionado elementos, puede llamar a métodos para que tomen medidas, como ocultarlos: *$("#rsvpmsg"). hide();*

Para nuestro escenario RSVP, definiremos una función JavaScript simple denominada "AnimateRSVPMessage" &lt;que&gt; selecciona el div "rsvpmsg" y anima el tamaño de su contenido de texto. El siguiente código inicia el texto pequeño y luego hace que aumente durante un período de tiempo de 400 milisegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

A continuación, podemos conectar esta función JavaScript a la que se llamará después de que nuestra llamada AJAX se complete correctamente pasando su nombre a nuestro método auxiliar Ajax.ActionLink() (a través de la propiedad de evento AjaxOptions "OnSuccess"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Y ahora, cuando se hace clic en el enlace "RSVP para este evento" y nuestra llamada AJAX se completa correctamente, el mensaje de contenido enviado de vuelta animará y crecerá grande:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Además de proporcionar un evento "OnSuccess", el objeto AjaxOptions expone eventos OnBegin, OnFailure y OnComplete que puede controlar (junto con una variedad de otras propiedades y opciones útiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpieza - Refactorizar una vista parcial RSVP

Nuestra plantilla de vista de detalles está empezando a ser un poco larga, lo que las horas extras harán que sea un poco más difícil de entender. Para ayudar a mejorar la legibilidad del código, terminemos creando una vista parcial, RSVPStatus.ascx, que encapsula todo el código de vista RSVP para nuestra página Detalles.

Para ello, haga clic con el botón derecho en la carpeta&gt;"Vistas" y, a continuación, elija el comando de menú Agregar- Vista. Haremos que tome un objeto Dinner como su ViewModel fuertemente tipado. A continuación, podemos copiar y pegar el contenido RSVP de nuestra vista Details.aspx en él.

Una vez hecho eso, vamos a crear también otra vista parcial , EditAndDeleteLinks.ascx , que encapsula nuestro código de vista de vínculo Editar y eliminar. También haremos que tome un objeto Dinner como su ViewModel fuertemente tipado y copie y pegue la lógica Edit and Delete de nuestra vista Details.aspx en él.

Nuestra plantilla de vista de detalles sólo puede incluir dos llamadas al método Html.RenderPartial() en la parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Esto hace que el código sea más limpio para leer y mantener.

### <a name="next-step"></a>siguiente paso

Veamos ahora cómo podemos usar AJAX aún más y agregar compatibilidad con mapas interactivos a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)
