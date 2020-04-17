---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Creación de rutas personalizadas (VB) Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd12307f685fa064832bb4198df06df2c3f2d3be
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542747"
---
# <a name="creating-custom-routes-vb"></a>Crear rutas personalizadas (VB)

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax.

En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación ASP.NET MVC. Aprenderá a modificar la tabla de rutas predeterminada en el archivo Global.asax con una ruta personalizada.

En ASP.NET aplicaciones MVC, la tabla de rutas predeterminada funcionará perfectamente. Sin embargo, es posible que descubra que tiene necesidades de enrutamiento especializadas. En ese caso, puede crear una ruta personalizada.

Imagine, por ejemplo, que está creando una aplicación de blog. Es posible que desee controlar las solicitudes entrantes que se ven así:

/Archive/12-25-2009

Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog que corresponde a la fecha 12/25/2009. Para manejar este tipo de petición, usted necesita crear una ruta personalizada.

El archivo Global.asax del listado 1 contiene una nueva ruta personalizada, denominada Blog, que controla las solicitudes que se parecen a /Archive/ fecha de*entrada*.

**Listado 1 - Global.asax (con ruta personalizada)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

El orden de las rutas que se agregan a la tabla de rutas es importante. Nuestra nueva ruta de blog personalizada se agrega antes de la ruta predeterminada existente. Si ha invertido el orden, siempre se llamará a la ruta predeterminada en lugar de a la ruta personalizada.

La ruta blog personalizada coincide con cualquier solicitud que comience por /Archive/. Por lo tanto, coincide con todas las siguientes direcciones URL:

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archivo/manzana

La ruta personalizada asigna la solicitud entrante a un controlador denominado Archive e invoca la acción Entry(). Cuando se llama al método Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.

Puede usar la ruta personalizada Blog con el controlador en el listado 2.

**Listado 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Observe que el método Entry() del listado 2 acepta un parámetro de tipo DateTime. El marco MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un DateTime valor automáticamente. Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un DateTime, se produce un error (consulte la figura 1).

**Figura 1 - Error al convertir el parámetro**

[![Cuadro de diálogo Nuevo proyecto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Figura 01**: Error al convertir el parámetro ([Haga clic para ver la imagen de tamaño completo](creating-custom-routes-vb/_static/image2.png))

## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo se puede crear una ruta personalizada. Ha aprendido a agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa entradas de blog. Hemos discutido cómo asignar solicitudes de entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominada Entry().

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-controller-overview-vb.md)
> [Siguiente](creating-a-route-constraint-vb.md)
