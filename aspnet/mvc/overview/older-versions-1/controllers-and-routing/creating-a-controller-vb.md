---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Creación de un controlador (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther demuestra cómo puede agregar un controlador a una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14ceff17d15b1a8460bad41429b82cb9504a24
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035162"
---
<a name="creating-a-controller-vb"></a>Crear un controlador (VB)
====================
by [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther demuestra cómo puede agregar un controlador a una aplicación ASP.NET MVC.


El objetivo de este tutorial es explicar cómo se puede crear controladores de MVC de ASP.NET nuevo. Aprenda a crear controladores mediante la opción de menú Agregar controlador de Visual Studio y mediante la creación de un archivo de clase a mano.

### <a name="using-the-add-controller-menu-option"></a>Mediante el agregar la opción de menú del controlador

Es la manera más fácil de crear un nuevo controlador secundario en la carpeta controladores en la ventana Explorador de soluciones de Visual Studio y seleccione el **Add, controlador** opción de menú (consulte la figura 1). Al seleccionar esta opción de menú se abre el **Agregar controlador** cuadro de diálogo (consulte la figura 2).


[![El cuadro de diálogo nuevo proyecto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Figura 01**: Agregar un nuevo controlador ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-vb/_static/image2.png))


[![El cuadro de diálogo nuevo proyecto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Figura 02**: El cuadro de diálogo Agregar controlador ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-vb/_static/image4.png))


Tenga en cuenta que la primera parte del nombre del controlador se resalta en el **Agregar controlador** cuadro de diálogo. Cada nombre de controlador debe finalizar con el sufijo *controlador*. Por ejemplo, puede crear un controlador denominado *ProductController* pero no un controlador denominado *producto*.


Si crea un controlador que falta el *controlador* sufijo, a continuación, no podrá invocar el controlador. No hace esto, he en vano incontables horas de mi vida después cometer este error.


**Listing 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Siempre debe crear los controladores en la carpeta Controllers. En caso contrario, deberá se infringen las convenciones de ASP.NET MVC y otros desarrolladores tendrán un tiempo más difícil comprender la aplicación.

### <a name="scaffolding-action-methods"></a>Métodos de acción de scaffolding

Cuando se crea un controlador, tiene la opción para generar automáticamente los métodos de acción Create, Update y los detalles (consulte la figura 3). Si selecciona esta opción, se genera la clase de controlador en el listado 2.


[![Creación automática de los métodos de acción](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Figura 03**: Creación automática de los métodos de acción ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-vb/_static/image6.png))


**Listing 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Estos métodos generados son los métodos auxiliares. Debe agregar la lógica real para crear, actualizar y mostrar los detalles de un cliente. Sin embargo, los métodos de código auxiliar proporcionan un buen punto de partida.

### <a name="creating-a-controller-class"></a>Creación de una clase de controlador

El controlador de MVC de ASP.NET es simplemente una clase. Si lo prefiere, puede omitir el scaffolding de controlador adecuado de Visual Studio y crear una clase de controlador a mano. Siga estos pasos:

1. Haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, nuevo elemento** y seleccione el **clase** plantilla (consulte la figura 4).
2. Nombre de la nueva clase PersonController.vb y haga clic en el **agregar** botón.
3. Modifique el archivo resultante de la clase para que la clase hereda de la clase base System.Web.Mvc.Controller (consulte el listado 3).


[![Crear una nueva clase](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Figura 04**: Crear una nueva clase ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-vb/_static/image8.png))


**Listing 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

El controlador en el listado 3 expone una acción denominada Index() que devuelve la cadena "Hello World!". Puede invocar esta acción de controlador mediante la ejecución de la aplicación y solicitar una dirección URL similar al siguiente:

`http://localhost:40071/Person`

> [!NOTE]
> 
> El servidor de desarrollo de ASP.NET utiliza un número de puerto aleatorio (por ejemplo, 40071). Cuando escriba una dirección URL para invocar un controlador, deberá proporcionar el número de puerto de la derecha. Puede determinar el número de puerto desplazando el puntero del mouse sobre el icono para el servidor de desarrollo de ASP.NET en el área de notificación de Windows (esquina inferior derecha de la pantalla).
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-vb.md)
> [Siguiente](creating-an-action-vb.md)