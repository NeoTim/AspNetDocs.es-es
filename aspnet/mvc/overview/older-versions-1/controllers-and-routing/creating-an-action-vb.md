---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Creación de una acción (VB) Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo agregar una nueva acción a un controlador MVC ASP.NET. Obtenga información sobre los requisitos para que un método sea una acción.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: dd651155bdb931cb8358d369b3c0c2c98abb86b2
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542253"
---
# <a name="creating-an-action-vb"></a>Crear una acción (VB)

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo agregar una nueva acción a un controlador MVC ASP.NET. Obtenga información sobre los requisitos para que un método sea una acción.

El objetivo de este tutorial es explicar cómo se puede crear una nueva acción de controlador. Obtendrá información sobre los requisitos de un método de acción. También aprenderá a evitar que un método se exponga como una acción.

## <a name="adding-an-action-to-a-controller"></a>Adición de una acción a un controlador

Agregar una nueva acción a un controlador mediante la adición de un nuevo método al controlador. Por ejemplo, el controlador del listado 1 contiene una acción denominada Index() y una acción denominada SayHello(). Ambos métodos se exponen como acciones.

**Listado 1 - Controllers-HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Para estar expuesto al universo como una acción, un método debe cumplir ciertos requisitos:

- El método debe ser público.
- El método no puede ser un método estático.
- El método no puede ser un método de extensión.
- El método no puede ser un constructor, getter o setter.
- El método no puede tener tipos genéricos abiertos.
- El método no es un método de la clase base del controlador.
- El método no puede contener parámetros **ref** o **out.**

Tenga en cuenta que no hay restricciones en el tipo de valor devuelto de una acción de controlador. Una acción de controlador puede devolver una cadena, un DateTime, una instancia de la Random clase o void. El marco de ASP.NET MVC convertirá cualquier tipo de valor devuelto que no sea un resultado de acción en una cadena y representará la cadena en el explorador.

Cuando se agrega cualquier método que no infringe estos requisitos a un controlador, el método se expone como una acción de controlador. Ten cuidado aquí. Cualquier persona conectada a Internet puede invocar una acción de controlador. Por ejemplo, no cree una acción de controlador DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Evitar que se invoque un método público

Si necesita crear un método público en una clase de controlador y no desea exponer el método como una &lt;acción&gt; de controlador, puede evitar que se invoque el método mediante el atributo NonAction. Por ejemplo, el controlador del listado 2 contiene un método público &lt;denominado&gt; CompanySecrets() que está decorado con el atributo NonAction.

**Listado 2 - Controllers-WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Si intenta invocar la acción del controlador CompanySecrets() escribiendo /Work/CompanySecrets en la barra de direcciones de su navegador, obtendrá el mensaje de error en la Figura 1.

[![Invocar un método NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: Invocar un método NonAction(Haga[clic para ver la imagen de tamaño completo)](creating-an-action-vb/_static/image2.png)

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-vb.md)
> [Siguiente](aspnet-mvc-controllers-overview-cs.md)
