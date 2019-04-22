---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Representación ASP.NET Web Pages (Razor) sitios para dispositivos móviles | Microsoft Docs
author: Rick-Anderson
description: 'Este artículo describe cómo crear páginas en un sitio de ASP.NET Web Pages (Razor) que se representará correctamente en los dispositivos móviles. Lo que aprenderá: Cómo se...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dbcd25331387f8606343e551302bc3ed1f9b2c25
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59379513"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Representar sitios de ASP.NET Web Pages (Razor) para dispositivos móviles

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe cómo crear páginas en un sitio de ASP.NET Web Pages (Razor) que se representará correctamente en los dispositivos móviles.
> 
> Lo que aprenderá:
> 
> - Cómo usar una convención de nomenclatura para especificar que una página está diseñado específicamente para dispositivos móviles.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


ASP.NET Web Pages le permite crear pantallas personalizadas para representar el contenido en dispositivos móviles o de otros dispositivos.

La manera más sencilla de crear la página específica del dispositivo en un sitio de ASP.NET Web Pages es mediante un patrón de nomenclatura de archivos similar al siguiente: *FileName.Mobile.cshtml*. Puede crear dos versiones de una página (por ejemplo, uno denominado *MyFile.cshtml* y otro llamado *MyFile.Mobile.cshtml*). En tiempo de ejecución, cuando se solicita un dispositivo móvil *MyFile.cshtml*, ASP.NET representa el contenido de *MyFile.Mobile.cshtml*. En caso contrario, *MyFile.cshtml* se representa.

El ejemplo siguiente muestra cómo habilitar la representación de dispositivos móvil mediante la adición de una página de contenido para dispositivos móviles. *Page1.cshtml* contiene contenido además de una barra lateral de navegación. *Page1.Mobile.cshtml* contiene el mismo contenido, pero omite la barra lateral.

1. En un sitio de ASP.NET Web Pages, cree un archivo denominado *Page1.cshtml* y reemplace el contenido actual por el marcado siguiente.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Cree un archivo denominado *Page1.Mobile.cshtml* y reemplace el contenido existente por el marcado siguiente. Tenga en cuenta que la versión móvil de la página omite la sección de exploración para representar mejor en una pantalla más pequeña.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Ejecute un explorador de escritorio y vaya a *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Ejecute un explorador móvil (o un emulador de dispositivos móviles) y vaya a *Page1.cshtml*. (Tenga en cuenta que no incluye *.mobile.* como parte de la dirección URL). Aunque la solicitud es *Page1.cshtml*, ASP.NET representa *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Para probar las páginas móviles, puede usar un simulador de dispositivos móviles que se ejecuta en un equipo de escritorio. Esta herramienta le permite probar las páginas web tal como aparecen en los dispositivos móviles (es decir, normalmente con mucho menor Mostrar área). Un ejemplo de un simulador es el [complemento modificador del agente de usuario](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que le permite emular varios exploradores móviles desde una versión de escritorio de Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Emulador de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
