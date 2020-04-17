---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Uso de un sitio de CAPTCHA para evitar que los bots usen su ASP.NET Web Razor) Microsoft Docs
author: rick-anderson
description: En este artículo se explica cómo utilizar ReCaptcha (una medida de seguridad) para evitar que los programas automatizados (bots) realicen tareas en una ASP.NET Web Pages (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 65f414ae3fed5e2fa28b1e57f5327c6411a43d55
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543761"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Uso de un sitio CAPTCHA para evitar que los bots usen su ASP.NET Web Razor)

por [Microsoft](https://github.com/microsoft)

> En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que los programas automatizados (bots) realicen tareas en un sitio web de ASP.NET Web Pages (Razor).
> 
> **Temas que se abordarán:** 
> 
> - Cómo agregar una prueba CAPTCHA a su sitio.
> 
> Estas son las características ASP.NET introducidas en el artículo:
> 
> - El `ReCaptcha` ayudante.
> 
> > [!NOTE]
> > La información de este artículo se aplica a ASP.NET páginas Web 1.0 y páginas Web 2.

## <a name="about-captchas"></a>Acerca de CAPTCHAs

Cada vez que deje que las personas se registren en su sitio, o incluso simplemente introduzca un nombre y una URL (como para un comentario de blog), es posible que obtenga una avalancha de nombres falsos. Estos son a menudo dejados por programas automatizados (bots) que tratan de dejar URLs en cada sitio web que pueden encontrar. (Una motivación común es publicar las URL de los productos a la venta.)

Puede ayudar a asegurarse de que un usuario es una persona real y no un programa informático mediante el uso de un *CAPTCHA* para validar a los usuarios cuando se registran o de otro modo escriben su nombre y sitio. CAPTCHA significa prueba de Turing Público Completamente Automatizado para decirle a Computadoras y Humanos Aparte. Un CAPTCHA es una prueba de *desafío-respuesta* en la que se le pide al usuario que haga algo que es fácil de hacer para una persona, pero difícil de hacer para un programa automatizado. El tipo más común de CAPTCHA es uno donde se ven algunas letras distorsionadas y se les pide que las escriban. (Se supone que la distorsión hace que sea difícil para los bots descifrar las letras.)

## <a name="adding-a-recaptcha-test"></a>Adición de una prueba de ReCaptcha

En ASP.NET páginas, `ReCaptcha` puede utilizar la aplicación auxiliar para representar una prueba CAPTCHA basada en el servicio ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). La `ReCaptcha` aplicación auxiliar muestra una imagen de dos palabras distorsionadas que los usuarios tienen que introducir correctamente antes de validar la página. La respuesta del usuario la valida el servicio ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registre su sitio[http://recaptcha.net](http://recaptcha.net)web en ReCaptcha.Net ( ). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregue la biblioteca de aplicaciones auxiliares web ASP.NET a su sitio web como se describe en Instalación de aplicaciones auxiliares en un sitio de [páginas web ASP.NET,](https://go.microsoft.com/fwlink/?LinkId=252372)si aún no lo ha hecho.
3. Si aún no tiene un * \_archivo AppStart.cshtml,* en la carpeta raíz de un sitio web cree un archivo denominado * \_AppStart.cshtml*.
4. Agregue la `Recaptcha` siguiente configuración auxiliar en el * \_archivo AppStart.cshtml:* 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Establezca `PublicKey` las `PrivateKey` propiedades y con sus propias claves públicas y privadas.
6. Guarde el * \_archivo AppStart.cshtml y ciérrelo.*
7. En la carpeta raíz de un sitio web, cree una nueva página denominada *Recaptcha.cshtml*.
8. Reemplace el contenido existente por el siguiente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Ejecute la página *Recaptcha.cshtml* en un explorador. Si `PrivateKey` el valor es válido, la página muestra el control ReCaptcha y un botón. Si no hubiera establecido las claves globalmente en * \_AppStart.html*, la página mostraría un error. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Introduzca las palabras de la prueba. Si pasa la prueba de ReCaptcha, verá un mensaje en ese sentido. De lo contrario, verá un mensaje de error y se vuelve a mostrar el control ReCaptcha.

> [!NOTE]
> Si el equipo está en un dominio que utiliza `defaultproxy` el servidor proxy, es posible que deba configurar el elemento del archivo *Web.config.* En el ejemplo siguiente se muestra `defaultproxy` un archivo *Web.config* con el elemento configurado para permitir que el servicio ReCaptcha funcione.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Personalización del comportamiento de todo el sitio para ASP.NET sitios de páginas web](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sitio de ReCaptcha](https://www.google.com/recaptcha)
