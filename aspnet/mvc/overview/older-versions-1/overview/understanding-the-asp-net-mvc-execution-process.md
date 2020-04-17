---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Comprender el proceso de ejecución de ASP.NET MVC Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo el marco de trabajo de ASP.NET MVC procesa una solicitud de explorador paso a paso.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 48afbbe7349b80e0ed0b9bab987ae3ccda493aca
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541031"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Descripción del proceso de ejecución de ASP.NET MVC

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo el marco de trabajo de ASP.NET MVC procesa una solicitud de explorador paso a paso.

Las solicitudes a una aplicación web basada en ASP.NET MVC primero pasan a través de la **UrlRoutingModule** objeto, que es un módulo HTTP. Este módulo analiza la solicitud y realiza la selección de la ruta. El objeto **UrlRoutingModule** selecciona el primer objeto de ruta que coincide con la solicitud actual. (Un objeto de ruta es una clase que implementa **RouteBase**y suele ser una instancia de la clase **Route.)** Si no coincide ninguna ruta, el objeto **UrlRoutingModule** no hace nada y permite que la solicitud vuelva al procesamiento de solicitudes IIS o ASP.NET normal.

Desde el objeto **Route** seleccionado, el objeto **UrlRoutingModule** obtiene el objeto **IRouteHandler** asociado al objeto **Route.** Normalmente, en una aplicación MVC, será una instancia de **MvcRouteHandler**. La instancia de **IRouteHandler** crea un objeto **IHttpHandler** y le pasa el objeto **IHttpContext.** De forma predeterminada, la instancia de **IHttpHandler** para MVC es el objeto **MvcHandler.** El **objeto MvcHandler, a** continuación, selecciona el controlador que, en última instancia, controlará la solicitud.

> [!NOTE]
> Cuando una aplicación web ASP.NET MVC se ejecuta en IIS 7.0, no se requiere una extensión de nombre de archivo para los proyectos de MVC. Sin embargo, en IIS 6.0, el controlador requiere que asigne la extensión de nombre de archivo .mvc a la DLL de ISAPI de ASP.NET.

El módulo y el controlador son los puntos de entrada al marco de ASP.NET MVC. Realizan las siguientes acciones:

- Seleccionar el controlador adecuado en una aplicación web MVC.
- Obtener una instancia del controlador concreta.
- Llame al método **Execute** del controlador.

A continuación se enumeran las etapas de ejecución de un proyecto web MVC:

- Recibir la primera solicitud para la aplicación 

    - En el archivo Global.asax, los objetos **Route** se agregan al objeto **RouteTable.**
- Realizar el enrutamiento 

    - El módulo **UrlRoutingModule** utiliza el primer objeto **Route** coincidente de la colección **RouteTable** para crear el objeto **RouteData,** que, a continuación, utiliza para crear un objeto **RequestContext** (**IHttpContext**).
- Crear el controlador de solicitudes de MVC 

    - El objeto **MvcRouteHandler** crea una instancia de la clase **MvcHandler** y le pasa la instancia **de RequestContext.**
- Crear el controlador 

    - El objeto **MvcHandler** utiliza la instancia **de RequestContext** para identificar el objeto **IControllerFactory** (normalmente una instancia de la clase **DefaultControllerFactory)** para crear la instancia del controlador.
- Ejecutar controlador: la instancia **de MvcHandler** llama al controlador s **Execute** método. |
- Invocar la acción 

    - La mayoría de los controladores heredan de la clase base **Controller.** Para los controladores que lo hacen, el **ControllerActionInvoker** objeto que está asociado con el controlador determina qué método de acción de la clase de controlador para llamar y, a continuación, llama a ese método.
- Ejecutar el resultado 

    - Un método de acción típico puede recibir la entrada del usuario, preparar los datos de respuesta adecuados y, a continuación, ejecutar el resultado devolviendo un tipo de resultado. Entre los tipos de resultados integrados que se pueden ejecutar se incluyen los siguientes: **ViewResult** (que representa una vista y es el tipo de resultado más utilizado), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**y **EmptyResult**.
