---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Probar la seguridad de una contraseña (C#) | Microsoft Docs
author: wenz
description: Prácticamente cualquier lugar, las contraseñas son necesarias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El control PasswordStrength en ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: b71744df46f8b8d20efafa333a10370397c37d2f
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048412"
---
<a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="4c84a-104">Probar la seguridad de una contraseña (C#)</span><span class="sxs-lookup"><span data-stu-id="4c84a-104">Testing the Strength of a Password (C#)</span></span>
====================
<span data-ttu-id="4c84a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4c84a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4c84a-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4c84a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="4c84a-107">Prácticamente cualquier lugar, las contraseñas son necesarias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar.</span><span class="sxs-lookup"><span data-stu-id="4c84a-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="4c84a-108">El control PasswordStrength de ASP.NET AJAX Control Toolkit puede comprobar la calidad es de una contraseña.</span><span class="sxs-lookup"><span data-stu-id="4c84a-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="4c84a-109">Información general</span><span class="sxs-lookup"><span data-stu-id="4c84a-109">Overview</span></span>

<span data-ttu-id="4c84a-110">Prácticamente cualquier lugar, las contraseñas son necesarias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar.</span><span class="sxs-lookup"><span data-stu-id="4c84a-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="4c84a-111">El `PasswordStrength` control de ASP.NET AJAX Control Toolkit puede comprobar la calidad es de una contraseña.</span><span class="sxs-lookup"><span data-stu-id="4c84a-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="4c84a-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="4c84a-112">Steps</span></span>

<span data-ttu-id="4c84a-113">El `PasswordStrength` control extiende un cuadro de texto y comprueba si la contraseña es suficientemente buena.</span><span class="sxs-lookup"><span data-stu-id="4c84a-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="4c84a-114">Ofrece una gran variedad de opciones a través de los atributos. Estos son sólo algunas de ellas:</span><span class="sxs-lookup"><span data-stu-id="4c84a-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="4c84a-115">`MinimumNumericCharacters` número mínimo de caracteres numéricos que requiere la contraseña</span><span class="sxs-lookup"><span data-stu-id="4c84a-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="4c84a-116">`MinimumSymbolCharacters` número mínimo de caracteres de símbolos (no letras y dígitos) requerido la contraseña</span><span class="sxs-lookup"><span data-stu-id="4c84a-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="4c84a-117">`PreferredPasswordLength` longitud mínima de la contraseña</span><span class="sxs-lookup"><span data-stu-id="4c84a-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="4c84a-118">`RequiresUpperAndLowerCaseCharacters` Si la contraseña debe usar mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="4c84a-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="4c84a-119">El `StrengthIndicatorType` proporciona la información de presentación de la seguridad de la contraseña, como texto (valor `"Text"`) o como un tipo de barra de progreso (valor `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="4c84a-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="4c84a-120">En el `DisplayPosition` atributo, configurar dónde aparece la información.</span><span class="sxs-lookup"><span data-stu-id="4c84a-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="4c84a-121">Este es un ejemplo completo, incluido ASP.NET AJAX `ScriptManager` (control), el `PasswordStrength` control y por supuesto un cuadro de texto donde el usuario puede escribir una contraseña.</span><span class="sxs-lookup"><span data-stu-id="4c84a-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="4c84a-122">Modo de demostración, el campo de formulario de este último es un campo de texto normal y no un campo de contraseña para que se puedan ver lo que está escribiendo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4c84a-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="4c84a-123">Ejecute la página y escriba distancia: Después de escribir letras minúsculas, letras mayúsculas, dígitos y símbolos, la contraseña se considera como indivisible.</span><span class="sxs-lookup"><span data-stu-id="4c84a-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="4c84a-124">[![Ahora, la contraseña es válida (bastante)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4c84a-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="4c84a-125">Ahora, la contraseña es válida (bastante) ([haga clic aquí para ver imagen en tamaño completo](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4c84a-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4c84a-126">Siguiente</span><span class="sxs-lookup"><span data-stu-id="4c84a-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
