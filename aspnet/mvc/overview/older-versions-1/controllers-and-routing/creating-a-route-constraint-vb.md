---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Crear una restricción de ruta (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther demuestra cómo puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de las restricciones de ruta con expresiones regulares.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a8e540a00d852d5b710bfdbf63a68f6e6d280ee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032192"
---
<a name="creating-a-route-constraint-vb"></a>Crear una restricción de ruta (VB)
====================
by [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther demuestra cómo puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de las restricciones de ruta con expresiones regulares.


Utilice las restricciones de ruta para restringir las solicitudes del explorador que coinciden con una ruta determinada. Puede usar una expresión regular para especificar una restricción de ruta.

Por ejemplo, imagine que ha definido la ruta en el listado 1 en el archivo Global.asax.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Listado 1 contiene una ruta con el nombre de producto. Puede usar la ruta de producto para asignar las solicitudes del explorador a ProductController contenido en el listado 2.

**Listing 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Tenga en cuenta que la acción Details() expuesta por el controlador del producto acepta un solo parámetro denominado productId. Este parámetro es un entero.

La ruta definida en el listado 1 coincide con cualquiera de las direcciones URL siguientes:

- / Product/23
- / Product/7

Por desgracia, la ruta también coincidirá con las direcciones URL siguientes:

- / Product/bLa, bla
- / Product/apple

Dado que la acción Details() espera un parámetro entero, que realiza una solicitud que contiene un valor distinto de un valor entero se producirá un error. Por ejemplo, si escribe la dirección URL /Product/apple en el explorador, a continuación, obtendrá la página de error en la figura 1.


[![El cuadro de diálogo nuevo proyecto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: Puede ver una página explode ([haga clic aquí para ver imagen en tamaño completo](creating-a-route-constraint-vb/_static/image2.png))


Lo realmente desea es que solo coinciden con las direcciones URL que contienen un productId entero adecuado. Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta. La ruta de producto modificada en el listado 3 contiene una restricción de expresión regular que coincida solo con enteros.

**Listing 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

La expresión regular de \d+ coincide con uno o más números enteros. Esta restricción hace que la ruta de productos para que coincida con las direcciones URL siguientes:

- / Product/3
- / Product/8999

Pero no las direcciones URL siguientes:

- / Product/apple
- O producto

Estas solicitudes de explorador se controlarán mediante otra ruta o, si no hay ninguna ruta coincidente, un *no se encontró el recurso* , se devolverá el error.

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-vb.md)
> [Siguiente](creating-a-custom-route-constraint-vb.md)
