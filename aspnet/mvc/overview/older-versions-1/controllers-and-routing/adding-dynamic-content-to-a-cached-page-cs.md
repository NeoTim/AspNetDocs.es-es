---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Adición de contenido dinámico a una página almacenada en caché (C-) Microsoft Docs
author: rick-anderson
description: Obtén información sobre cómo mezclar contenido dinámico y almacenado en caché en la misma página. La sustitución posterior a la caché le permite mostrar contenido dinámico, como anuncios de banner o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c8cd70a15c1ae93f7cf9b0a026b37b07e489040
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542292"
---
# <a name="adding-dynamic-content-to-a-cached-page-c"></a>Agregar contenido dinámico a una página almacenada en caché (C#)

por [Microsoft](https://github.com/microsoft)

> Obtén información sobre cómo mezclar contenido dinámico y almacenado en caché en la misma página. La sustitución posterior a la caché le permite mostrar contenido dinámico, como anuncios de banner o elementos de noticias, dentro de una página que se ha almacenado en caché.

Aprovechando el almacenamiento en caché de resultados, puede mejorar drásticamente el rendimiento de una aplicación MVC ASP.NET. En lugar de regenerar una página cada vez que se solicita la página, la página se puede generar una vez y almacenarla en caché en memoria para varios usuarios.

Pero hay un problema. ¿Qué sucede si necesita mostrar contenido dinámico en la página? Por ejemplo, imagine que desea mostrar un anuncio de banner en la página. No desea que el anuncio de banner se almacene en caché para que cada usuario ve el mismo anuncio. ¡No ganarías dinero de esa manera!

Afortunadamente, hay una solución fácil. Puede aprovechar una característica del marco de trabajo de ASP.NET denominada *sustitución posterior*a la caché. La sustitución posterior a la caché le permite sustituir el contenido dinámico en una página que se ha almacenado en caché en la memoria.

Normalmente, cuando se genera una memoria caché de una página mediante el atributo [OutputCache], la página se almacena en caché tanto en el servidor como en el cliente (el explorador web). Cuando se utiliza la sustitución posterior a la caché, una página se almacena en caché solo en el servidor.

#### <a name="using-post-cache-substitution"></a>Uso de la sustitución posterior a la caché

El uso de la sustitución posterior a la caché requiere dos pasos. En primer lugar, debe definir un método que devuelva una cadena que represente el contenido dinámico que desea mostrar en la página almacenada en caché. A continuación, llame al método HttpResponse.WriteSubstitution() para insertar el contenido dinámico en la página.

Imagine, por ejemplo, que desea mostrar aleatoriamente diferentes elementos de noticias en una página almacenada en caché. La clase del listado 1 expone un único método, denominado RenderNews(), que devuelve aleatoriamente una noticia de una lista de tres elementos de noticias.

**Listado 1 – Modelos-News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Para aprovechar la sustitución posterior a la caché, se llama al método HttpResponse.WriteSubstitution(). El método WriteSubstitution() configura el código para reemplazar una región de la página almacenada en caché por contenido dinámico. El método WriteSubstitution() se utiliza para mostrar el elemento de noticias aleatorio en la vista del listado 2.

**Listado 2 – Vistas-Home-Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

El método RenderNews se pasa al método WriteSubstitution(). Observe que no se llama al método RenderNews (no hay paréntesis). En su lugar, se pasa una referencia al método a WriteSubstitution().

La vista de índice se almacena en caché. El controlador devuelve la vista en el listado 3. Observe que la acción Index() está decorada con un atributo [OutputCache] que hace que la vista Index se almacene en caché durante 60 segundos.

**Listado 3 – Controllers-HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Aunque la vista de índice se almacena en caché, se muestran diferentes elementos de noticias aleatorios cuando se solicita la página de índice. Cuando se solicita la página de índice, la hora mostrada por la página no cambia durante 60 segundos (consulte la figura 1). El hecho de que el tiempo no cambie demuestra que la página está almacenada en caché. Sin embargo, el contenido inyectado por el método WriteSubstitution() – el elemento de noticias aleatoria – cambia con cada solicitud .

**Figura 1 – Inyección de elementos de noticias dinámicos en una página almacenada en caché**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Uso de la sustitución posterior a la caché en métodos auxiliares

Una manera más fácil de aprovechar la sustitución posterior a la caché es encapsular la llamada al método WriteSubstitution() dentro de un método auxiliar personalizado. Este enfoque se ilustra mediante el método auxiliar en el listado 4.

**Listado 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

El listado 4 contiene una clase estática que expone dos métodos: RenderBanner() y RenderBannerInternal(). El método RenderBanner() representa el método auxiliar real. Este método extiende el estándar ASP.NET MVC HtmlHelper clase para que pueda llamar a Html.RenderBanner() en una vista al igual que cualquier otro método auxiliar.

El método RenderBanner() llama al método HttpResponse.WriteSubstitution() pasando el método RenderBannerInternal() al método WriteSubstitution().

El método RenderBannerInternal() es un método privado. Este método no se expondrá como un método auxiliar. El método RenderBannerInternal() devuelve aleatoriamente una imagen de anuncio de banner de una lista de tres imágenes de anuncio de banner.

La vista de índice modificada en el listado 5 ilustra cómo se puede utilizar el método auxiliar RenderBanner(). Tenga en &lt;cuenta que&gt; se incluye una directiva adicional % - Import % en la parte superior de la vista para importar el espacio de nombres MvcApplication1.Helpers. Si descuida importar este espacio de nombres, el método RenderBanner() no aparecerá como método en la propiedad Html.

**Listado 5 – Views-Home-Index.aspx (con el método RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Cuando solicita la página representada por la vista en el listado 5, se muestra un anuncio de banner diferente con cada solicitud (consulte la figura 2). La página se almacena en caché, pero el anuncio de banner se inyecta dinámicamente mediante el método auxiliar RenderBanner().

**Figura 2 – La vista de índice que muestra un anuncio de banner aleatorio**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Resumen

En este tutorial se explica cómo se puede actualizar dinámicamente el contenido en una página almacenada en caché. Ha aprendido a utilizar el método HttpResponse.WriteSubstitution() para permitir que el contenido dinámico se inyecte en una página almacenada en caché. También aprendió a encapsular la llamada al método WriteSubstitution() dentro de un método auxiliar HTML.

Aproveche el almacenamiento en caché siempre que sea posible: puede tener un impacto dramático en el rendimiento de sus aplicaciones web. Como se explica en este tutorial, puede aprovechar el almacenamiento en caché incluso cuando necesite mostrar contenido dinámico en sus páginas.

> [!div class="step-by-step"]
> [Anterior](improving-performance-with-output-caching-cs.md)
> [Siguiente](creating-a-controller-cs.md)
