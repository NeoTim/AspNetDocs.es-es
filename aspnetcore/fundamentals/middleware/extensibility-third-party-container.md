---
title: Activación de middleware con un contenedor de terceros en ASP.NET Core
author: guardrex
description: Aprenda a usar middleware fuertemente tipado con la implementación de una activación basada en Factory y un contenedor de terceros en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 6af775c66a1de7f1a4f06a4a639ade20c6493b2a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57026302"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Activación de middleware con un contenedor de terceros en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En este artículo se explica cómo usar [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) e [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) como un punto de extensibilidad para la activación de [middleware](xref:fundamentals/middleware/index) con un contenedor de terceros. Para obtener información introductoria sobre `IMiddlewareFactory` e `IMiddleware`, vea el tema [Activación de middleware basada en Factory](xref:fundamentals/middleware/extensibility).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

En la aplicación de ejemplo se muestra una activación de middleware por medio de una implementación de `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`. En el ejemplo se usa el contenedor de inserción de dependencias [Simple Injector](https://simpleinjector.org).

La implementación de middleware del ejemplo registra el valor proporcionado por un parámetro de cadena de consulta (`key`). El middleware usa un contexto de base de datos insertado (un servicio con ámbito) para registrar el valor de cadena de consulta en una base de datos en memoria.

> [!NOTE]
> En la aplicación de ejemplo se usa [Simple Injector](https://github.com/simpleinjector/SimpleInjector) única y exclusivamente con fines de demostración. El uso de Simple Injector no está avalado. Los métodos de activación de middleware descritos en la documentación de Simple Injector y los problemas de GitHub está recomendado por los responsables de Simple Injector. Para más información, vea la [documentación de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) y el [repositorio de GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) proporciona métodos para crear middleware.

En la aplicación de ejemplo, se implementa un Middleware Factory para crear una instancia de `SimpleInjectorActivatedMiddleware`. Ese Middleware Factory usa el contenedor de Simple Injector para resolver el middleware:

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) define el middleware para la canalización de solicitudes de la aplicación.

Middleware activado por una implementación de `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Se crea una extensión para el middleware (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` debe realizar varias tareas:

* Configurar el contenedor de Simple Injector.
* Registrar tanto el Factory como el Middleware.
* Poner disponible el contexto de base de datos de la aplicación en el contenedor de Simple Injector para una página de Razor.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

El middleware se registra en la canalización de procesamiento de solicitudes en `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Recursos adicionales

* [Middleware](xref:fundamentals/middleware/index)
* [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) (Activación de middleware basada en Factory)
* [Repositorio de GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector)
* [Documentación de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html)
