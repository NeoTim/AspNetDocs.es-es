---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Introducción al enrutamiento de ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther explica cómo el marco de MVC de ASP.NET asigna las solicitudes del explorador a acciones del controlador.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: c135bacd7f9f87fac87a3c8cf41bad359bb3c0a2
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379188"
---
# <a name="aspnet-mvc-routing-overview-vb"></a>Información general sobre el enrutamiento de ASP.NET MVC (VB)

by [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther explica cómo el marco de MVC de ASP.NET asigna las solicitudes del explorador a acciones del controlador.


En este tutorial, se le presenta una característica importante de cada aplicación de ASP.NET MVC llama *enrutamiento de ASP.NET*. El módulo de enrutamiento de ASP.NET es responsable de asignar las solicitudes entrantes a acciones del controlador MVC concreto. Al final de este tutorial, comprenderá cómo la tabla de enrutamiento estándar asigna las solicitudes a acciones del controlador.

## <a name="using-the-default-route-table"></a>Uso de la tabla de ruta predeterminada

Cuando se crea una nueva aplicación de ASP.NET MVC, la aplicación ya está configurada para usar el enrutamiento de ASP.NET. El enrutamiento de ASP.NET es el programa de instalación en dos lugares.

En primer lugar, el enrutamiento de ASP.NET está habilitado en el archivo de configuración de la aplicación Web (archivo Web.config). Hay cuatro secciones en el archivo de configuración que son relevantes para el enrutamiento: la sección system.web.httpModules, la sección system.web.httpHandlers, la sección system.webserver.modules y la sección system.webserver.handlers. Tenga cuidado de no eliminar estas secciones porque sin estas secciones enrutamiento dejarán de funcionar.

En segundo lugar y más importante aún, se crea una tabla de rutas en el archivo Global.asax de la aplicación. El archivo Global.asax es un archivo especial que contiene los controladores de eventos para eventos de ciclo de vida de aplicación de ASP.NET. Durante el evento de inicio de la aplicación se crea la tabla de rutas.

El archivo en el listado 1 contiene el archivo Global.asax de forma predeterminada para una aplicación ASP.NET MVC.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Cuando se inicia por primera vez una aplicación MVC, la aplicación\_se llama al método Start(). Este método, a su vez, llama al método de RegisterRoutes(). El método RegisterRoutes() crea la tabla de rutas.

La tabla de enrutamiento predeterminada contiene una única ruta (denominada de forma predeterminada). La ruta predeterminada asigna el primer segmento de una dirección URL a un nombre de controlador, el segundo segmento de una dirección URL a una acción de controlador y el tercer segmento a un parámetro denominado **id**.

Imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador web:

/Home/Index/3

La ruta predeterminada asigna esta dirección URL a los siguientes parámetros:

- controlador = Home

- acción = índice

- id = 3

Cuando se solicita/Home de dirección URL/índice/3, se ejecuta el código siguiente:

HomeController.Index(3)

La ruta predeterminada incluye valores predeterminados para los tres parámetros. Si no se proporciona un controlador, a continuación, el parámetro de controlador tiene como valor predeterminado el valor **inicio**. Si no se proporciona una acción, el parámetro de acción tiene como valor predeterminado el valor **índice**. Por último, si no proporciona un identificador, el parámetro de Id. predeterminado es una cadena vacía.

Echemos un vistazo a algunos ejemplos de cómo la ruta predeterminada asigna las direcciones URL a acciones del controlador. Imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:

/Home

Debido a los valores predeterminados de parámetros de ruta de forma predeterminada, al escribir esta dirección URL hará que el método de Index() de la clase HomeController en el listado 2 para llamarlo.

**Listado 2 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

En el listado 2, la clase HomeController incluye un método denominado Index() que acepta un solo parámetro denominado identificador. El/Home URL hace que el método Index() se llama con el valor Nothing como el valor del parámetro del identificador.

Debido al modo en que el marco de MVC invoca las acciones de controlador, el/Home URL también coincide con el método Index() de la clase HomeController en el listado 3.

**Listado 3 - HomeController.vb (acción del índice sin ningún parámetro)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

El método Index() en el listado 3 no acepta ningún parámetro. El/Home URL hará que este método Index() al que llamar. / Home de dirección URL/índice/3 también invoca este método (el Id. se omite).

El/Home URL también coincide con el método Index() de la clase HomeController en el listado 4.

**Listado 4 - HomeController.vb (acción del índice con el parámetro que acepta valores NULL)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

En el listado 4, el método Index() tiene un parámetro de entero. Dado que el parámetro es un parámetro que acepta valores null (puede tener el valor Nothing), se puede llamar el Index() sin provocar un error.

Por último, al invocar el método Index() en el listado 5 con la/Home URL provoca una excepción desde el parámetro Id *no* un parámetro que acepta valores NULL. Si se intenta invocar el método Index() obtendrá el error mostrado en la figura 1.

**Listado 5 - HomeController.vb (acción del índice con el parámetro de Id.)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![Iuna acción de controlador que espera un valor de parámetro nvoking](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Figura 01**: Invocar una acción de controlador que espera un valor de parámetro ([haga clic aquí para ver imagen en tamaño completo](asp-net-mvc-routing-overview-vb/_static/image2.png))


/ Home de dirección URL/índice/3, por otro lado, funciona bien con la acción de controlador de índice en el listado 5. La solicitud /Home/Index/3 hace que el método Index() se llamen con un parámetro de identificador que tiene el valor 3.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era para proporcionarle una breve introducción al enrutamiento de ASP.NET. Se examina la tabla de rutas predeterminadas que se obtiene con una nueva aplicación MVC de ASP.NET. Ha aprendido cómo la ruta predeterminada asigna las direcciones URL a acciones del controlador.

> [!div class="step-by-step"]
> [Anterior](creating-an-action-cs.md)
> [Siguiente](understanding-action-filters-vb.md)
