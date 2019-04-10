---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Crear la vista (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408243"
---
# <a name="create-the-view-ui"></a>Crear la vista (IU)

por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, se iniciará definir el código HTML de la aplicación y Agregar enlace de datos entre el código HTML y el modelo de vista.

Abra el archivo Views/Home/Index.cshtml. Reemplace todo el contenido de ese archivo por el siguiente.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La mayoría de los `div` elementos existen para [Bootstrap](http://getbootstrap.com/) estilo. Los elementos importantes son aquellas con `data-bind` atributos. Este atributo vincula el código HTML para el modelo de vista.

Por ejemplo:

[!code-html[Main](part-7/samples/sample2.html)]

En este ejemplo, el &quot; `text` &quot; enlace hace que el `<p>` elemento para mostrar el valor de la `error` propiedad desde el modelo de vista. Recuerde que `error` se ha declarado como un `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Cada vez que se asigna un nuevo valor a `error`, Knockout actualiza el texto en el `<p>` elemento.

El `foreach` enlace indica a Knockout que recorra en iteración el contenido de la `books` matriz. Para cada elemento de la matriz, Knockout crea un nuevo &lt;li&gt; elemento. Los enlaces dentro del contexto de la `foreach` hacen referencia a propiedades en el elemento de matriz. Por ejemplo:

[!code-html[Main](part-7/samples/sample4.html)]

Aquí el `text` enlace lee la propiedad del autor de cada libro.

Si ejecuta la aplicación ahora, debe ver así:

![](part-7/_static/image1.png)

La lista de los libros en pantalla se carga de forma asincrónica, una vez cargada la página. Ahora, el &quot;detalles&quot; vínculos no son funcionales. Vamos a agregar esta funcionalidad en la sección siguiente.

> [!div class="step-by-step"]
> [Anterior](part-6.md)
> [Siguiente](part-8.md)
