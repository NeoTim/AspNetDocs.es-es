---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Combatir los Bots (C#) | Microsoft Docs
author: wenz
description: Inicios automáticos pegar los weblogs y otros sitios Web con el correo no deseado, envío de formularios de comentario sin ninguna interacción del usuario. El control NoBot en la desventaja de AJAX de ASP.NET...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 52ed34e7640cd125a3b4c3b50ab760a7c1d713f1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035402"
---
<a name="fighting-bots-c"></a><span data-ttu-id="ab3a5-104">Combatir los bots (C#)</span><span class="sxs-lookup"><span data-stu-id="ab3a5-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="ab3a5-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ab3a5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ab3a5-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ab3a5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="ab3a5-107">Inicios automáticos pegar los weblogs y otros sitios Web con el correo no deseado, envío de formularios de comentario sin ninguna interacción del usuario.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="ab3a5-108">El control NoBot de ASP.NET AJAX Control Toolkit puede ayudar a combatir los bots.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="ab3a5-109">Información general</span><span class="sxs-lookup"><span data-stu-id="ab3a5-109">Overview</span></span>

<span data-ttu-id="ab3a5-110">Inicios automáticos pegar los weblogs y otros sitios Web con el correo no deseado, envío de formularios de comentario sin ninguna interacción del usuario.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="ab3a5-111">El control NoBot de ASP.NET AJAX Control Toolkit puede ayudar a combatir los bots.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="ab3a5-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="ab3a5-112">Steps</span></span>

<span data-ttu-id="ab3a5-113">Un método común para combatir los bots es usar CAPTCHAs completamente automatizada pública test de Turing para indicar a los equipos y los seres humanos separados.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="ab3a5-114">Un test de Turing era originalmente una prueba donde alguien necesarios para decidir si un asociado de comunicación es una persona o una máquina.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="ab3a5-115">En la web, un CAPTCHA normalmente consta de una imagen con algunas letras distorsionadas en él.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="ab3a5-116">La idea es que sólo una persona puede leer las letras de la imagen, mientras que los algoritmos de reconocimiento óptico de caracteres se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="ab3a5-117">Hay varias ventajas y desventajas con este enfoque, pero una explicación de este queda fuera del ámbito de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="ab3a5-118">Sin embargo hay un control de ASP.NET AJAX Control Toolkit que proporciona un enfoque similar: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="ab3a5-119">Es más fácil solución que un CAPTCHA, pero es muy fácil de usar y tarifas muy bien en los sitios Web, como blogs, donde se considera correcta si la mayoría de los intentos de correo no deseado se rechaza, lo que el `NoBot` control puede hacer.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="ab3a5-120">`NoBot` intercepta la devolución de datos del formulario web ASP.NET actual si se cumple al menos una de estas condiciones:</span><span class="sxs-lookup"><span data-stu-id="ab3a5-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="ab3a5-121">Se produce un error en el explorador resolver un rompecabezas de JavaScript (por ejemplo cuando JavaScript está desactivado)</span><span class="sxs-lookup"><span data-stu-id="ab3a5-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="ab3a5-122">El usuario enviado el formulario a rápida</span><span class="sxs-lookup"><span data-stu-id="ab3a5-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="ab3a5-123">La dirección IP de cliente había enviado el formulario con demasiada frecuencia en un período de tiempo determinado.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="ab3a5-124">Para comprobar estas condiciones, el `NoBot` control requiere estos atributos (todos ellos opcionales):</span><span class="sxs-lookup"><span data-stu-id="ab3a5-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="ab3a5-125">`ResponseMinimumDelaySeconds` cantidad mínima de segundos entre las devoluciones de datos</span><span class="sxs-lookup"><span data-stu-id="ab3a5-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="ab3a5-126">`CutoffWindowSeconds` longitud del intervalo de tiempo en el que las devoluciones de datos desde una dirección IP son medidas</span><span class="sxs-lookup"><span data-stu-id="ab3a5-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="ab3a5-127">`CutoffMaximumInstances` cantidad máxima de segundos por intervalo de tiempo</span><span class="sxs-lookup"><span data-stu-id="ab3a5-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="ab3a5-128">Las demandas de marcado siguiente que al menos dos segundos deben transcurrir entre las devoluciones de datos y que hay devoluciones de datos solo cinco o menos dentro de un intervalo de 30 segundos:</span><span class="sxs-lookup"><span data-stu-id="ab3a5-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="ab3a5-129">A continuación, como de costumbre no olvide incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="ab3a5-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="ab3a5-130">Porque la mayoría de las comprobaciones de `NoBot` está haciendo se producen en el servidor, deberá comprobar el resultado de estas validaciones.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="ab3a5-131">Esto puede hacerse mediante una llamada a `NoBot`del `IsValid()` método.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="ab3a5-132">Tiene un argumento (como un `out` parámetro /`ByRef` parámetro) que es de tipo `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="ab3a5-133">Representación de cadena contiene el motivo, cuando se produce un error en la comprobación y `Valid` en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="ab3a5-134">El código siguiente genera un mensaje conforme a `NoBot`del resultado:</span><span class="sxs-lookup"><span data-stu-id="ab3a5-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="ab3a5-135">Por último, necesita un formulario para enviar y un elemento de etiqueta para el mensaje de salida, y ha terminado!</span><span class="sxs-lookup"><span data-stu-id="ab3a5-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="ab3a5-136">Al ejecutar esta secuencia de comandos y desactivar JavaScript o enviar el formulario dentro de los primeros dos segundos o enviar el formulario siete veces dentro de treinta segundos, obtendrá un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="ab3a5-137">Sin embargo con inteligencia a usar este control, puesto que sólo unos 90 95% de los usuarios tienen activados en JavaScript, por lo tanto, 5-10% de los usuarios se producirá un error `NoBot`de la prueba.</span><span class="sxs-lookup"><span data-stu-id="ab3a5-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="ab3a5-138">[![Este mensaje de error podría deberse a un bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ab3a5-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="ab3a5-139">Este mensaje de error podría deberse a un bot ([haga clic aquí para ver imagen en tamaño completo](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ab3a5-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab3a5-140">Siguiente</span><span class="sxs-lookup"><span data-stu-id="ab3a5-140">Next</span></span>](fighting-bots-vb.md)
