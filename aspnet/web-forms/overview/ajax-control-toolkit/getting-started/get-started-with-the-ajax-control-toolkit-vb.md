---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Introducción al Kit de herramientas de control de AJAX (VB) Microsoft Docs
author: rick-anderson
description: Aprenda todo lo que necesita saber para empezar a usar el Kit de herramientas de control de AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 6af5e293a35ce3e3ba35a0f7abfd2d92d538c84c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543566"
---
# <a name="get-started-with-the-ajax-control-toolkit-vb"></a>Introducción a AJAX Control Toolkit (VB)

por [Microsoft](https://github.com/microsoft)

> Aprenda todo lo que necesita saber para empezar a usar el Kit de herramientas de control de AJAX.

El kit de herramientas de control de AJAX contiene más de 30 controles gratuitos que puede utilizar en las aplicaciones de ASP.NET. En este tutorial, aprenderá a descargar el Kit de herramientas de control de AJAX y agregar los controles del kit de herramientas a la caja de herramientas de Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Descarga del kit de herramientas de control de AJAX

[AJAX Control Toolkit](http://devexpress.com/act) es un proyecto de código abierto desarrollado por los miembros de la comunidad ASP.NET y el equipo de ASP.NET.

[![Descarga del kit de herramientas de control de AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Figura 01**: Descargar el kit de herramientas de control de AJAX[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png)

Después de descargar el archivo, debe desbloquear el archivo. Haga clic con el botón derecho en el archivo, seleccione Propiedades y haga clic en el botón **Desbloquear** (consulte la figura 2).

[![Desbloqueo del archivo ZIP de AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Figura 02**: Desbloqueo del archivo ZIP de AJAX Control Toolkit[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png)

Después de desbloquear el archivo, puede descomprimir el archivo: haga clic con el botón derecho en el archivo y seleccione la opción de menú **Extraer todo.** Ahora, estamos listos para agregar el kit de herramientas a la caja de herramientas de Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Adición del Kit de herramientas de control de AJAX a Toolbox

La forma más fácil de usar el Kit de herramientas de control de AJAX es agregar el kit de herramientas a la caja de herramientas de Visual Studio/Visual Web Developer (consulte la figura 3). De este modo, puede arrastrar simplemente un control de kit de herramientas a una página cuando desee usarlo.

[![AJAX Control Toolkit aparece en la caja de herramientas](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Figura 03**: AJAX Control Toolkit aparece en la caja de herramientas[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png)

En primer lugar, debe agregar una pestaña AJAX Control Toolkit a la caja de herramientas. Siga estos pasos.

1. Cree un nuevo sitio web de ASP.NET seleccionando la opción de menú Archivo, Nuevo sitio web. Haga doble clic en Default.aspx en el Explorador de soluciones ventana para abrir el archivo en el editor.
2. Haga clic con el botón derecho en el cuadro de herramientas debajo de la ficha General y seleccione la opción de menú **Agregar pestaña** (consulte la figura 4).
3. Introduzca una nueva pestaña denominada AJAX Control Toolkit.

[![Adición de una nueva pestaña](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Figura 04**: Adición de una nueva pestaña[(Haga clic para ver la imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png)

A continuación, debe agregar los controles del Kit de herramientas de control de AJAX a la nueva pestaña.

- Haga clic con el botón derecho debajo de la pestaña Ajax Control Toolkit y seleccione la opción de menú **Elegir elementos (consulte la figura 5).**
- Vaya a la ubicación donde descomprimió el Kit de herramientas de control de AJAX y seleccione el ensamblado AjaxControlToolkit.dll.

[![Elija los elementos que desea agregar a la caja de herramientas](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Figura 05**: Elija los elementos que desea agregar a la caja de herramientas (Haga clic para ver la[imagen de tamaño completo)](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png)

Después de completar estos pasos, todos los controles del kit de herramientas aparecerán en la caja de herramientas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Actualización a una nueva versión del kit de herramientas

Si estaba utilizando una versión anterior del kit de herramientas y ahora necesita pasar a una versión posterior, estos son los pasos recomendados:

- Binarios: elimine la versión anterior del ensamblado AjaxControlToolkit.dll de la carpeta Bin del sitio web.
- Elementos del cuadro de herramientas: elimine la pestaña Ajax Control Toolkit y siga los pasos anteriores para volver a crear la pestaña con la nueva versión del ensamblado AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Anterior](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Siguiente](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
