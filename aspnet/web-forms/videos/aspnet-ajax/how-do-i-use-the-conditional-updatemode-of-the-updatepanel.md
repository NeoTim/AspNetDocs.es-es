---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[¿Cómo lo hago?:] ¿Usar el UpdateMode condicional de UpdatePanel? | Microsoft Docs'
author: JoeStagner
description: ASP.NET AJAX UpdatePanel incluye una propiedad de UpdateMode que puede establecerse en "Siempre" o "Condicional". El valor predeterminado es siempre, en cuyo caso el UpdatePan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: 96d7bc95fd80e410feb332264695835e68793ae2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050902"
---
<a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="be9f8-105">[¿Cómo lo hago?:] ¿Usar el UpdateMode condicional de UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="be9f8-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>
====================
<span data-ttu-id="be9f8-106">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="be9f8-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="be9f8-107">ASP.NET AJAX UpdatePanel incluye una propiedad de UpdateMode que puede establecerse en "Siempre" o "Condicional".</span><span class="sxs-lookup"><span data-stu-id="be9f8-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="be9f8-108">El valor predeterminado es siempre, en cuyo caso el UpdatePanel siempre actualiza su contenido durante un postback asincrónico.</span><span class="sxs-lookup"><span data-stu-id="be9f8-108">The default is Always, in which case the UpdatePanel will always update its content during an asychronous postback.</span></span> <span data-ttu-id="be9f8-109">En este vídeo, obtenga información sobre cómo podemos establecer el UpdateMode en Conditional, en el que caso UpdatePanel solo actualizará su contenido cuando el código del lado servidor llama a su método de actualización.</span><span class="sxs-lookup"><span data-stu-id="be9f8-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="be9f8-110">Esto le permite usar lógica condicional en su código C# o Visual Basic para determinar si el control UpdatePanel actualizará su contenido durante el postback asincrónico actual.</span><span class="sxs-lookup"><span data-stu-id="be9f8-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="be9f8-111">&#9654;Vea el vídeo (13 minutos)</span><span class="sxs-lookup"><span data-stu-id="be9f8-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="be9f8-112">[Anterior](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Siguiente](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="be9f8-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>