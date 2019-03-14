---
title: Activación de middleware basada en Factory en ASP.NET Core
author: guardrex
description: Aprenda a usar middleware fuertemente tipado con la implementación de una activación basada en Factory en ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 566a5c5f642a3f55e72a8e070c69d2bfddaee3a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052362"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="5e53a-103">Activación de middleware basada en Factory en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e53a-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="5e53a-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e53a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5e53a-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) es un punto de extensibilidad para la activación de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5e53a-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="5e53a-106">Los métodos de extensión `UseMiddleware` comprueban si un tipo registrado de middleware implementa `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="5e53a-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="5e53a-107">Si es así, la instancia `IMiddlewareFactory` registrada en el contenedor se usa para resolver la implementación `IMiddleware` en lugar de usar la lógica de activación de middleware basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="5e53a-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="5e53a-108">El middleware se registra como un servicio con ámbito o transitorio en el contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e53a-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="5e53a-109">Ventajas:</span><span class="sxs-lookup"><span data-stu-id="5e53a-109">Benefits:</span></span>

* <span data-ttu-id="5e53a-110">Activación a petición (inyección de servicios con ámbito)</span><span class="sxs-lookup"><span data-stu-id="5e53a-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="5e53a-111">Tipado fuerte de middleware</span><span class="sxs-lookup"><span data-stu-id="5e53a-111">Strong typing of middleware</span></span>

<span data-ttu-id="5e53a-112">`IMiddleware` se activa a petición, por lo que los servicios se pueden insertar en el constructor del middleware.</span><span class="sxs-lookup"><span data-stu-id="5e53a-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="5e53a-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e53a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5e53a-114">La aplicación de ejemplo muestra middleware activado por:</span><span class="sxs-lookup"><span data-stu-id="5e53a-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="5e53a-115">Convención.</span><span class="sxs-lookup"><span data-stu-id="5e53a-115">Convention.</span></span> <span data-ttu-id="5e53a-116">Para obtener más información sobre la activación de middleware convencional, consulte el tema [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5e53a-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="5e53a-117">Una implementación de [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware).</span><span class="sxs-lookup"><span data-stu-id="5e53a-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="5e53a-118">La [clase MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) predeterminada activa el middleware.</span><span class="sxs-lookup"><span data-stu-id="5e53a-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="5e53a-119">Las implementaciones de middleware funcionan de forma idéntica y registran el valor proporcionado por un parámetro de cadena de consulta (`key`).</span><span class="sxs-lookup"><span data-stu-id="5e53a-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="5e53a-120">El middleware usa un contexto de base de datos insertado (un servicio con ámbito) para registrar el valor de cadena de consulta en una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="5e53a-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="5e53a-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="5e53a-121">IMiddleware</span></span>

<span data-ttu-id="5e53a-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) define el middleware para la canalización de solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e53a-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="5e53a-123">El método [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) controla las solicitudes y devuelve una `Task` que representa la ejecución del middleware.</span><span class="sxs-lookup"><span data-stu-id="5e53a-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="5e53a-124">Middleware activado por convención:</span><span class="sxs-lookup"><span data-stu-id="5e53a-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="5e53a-125">Middleware activado por `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="5e53a-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="5e53a-126">Las extensiones se crean para los middlewares:</span><span class="sxs-lookup"><span data-stu-id="5e53a-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="5e53a-127">No se pueden pasar objetos al middleware activado por Factory con `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="5e53a-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="5e53a-128">El middleware activado por Factory se agrega al contenedor integrado en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5e53a-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="5e53a-129">Ambos middlewares se registran en la canalización de procesamiento de solicitudes en `Configure`:</span><span class="sxs-lookup"><span data-stu-id="5e53a-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="5e53a-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="5e53a-130">IMiddlewareFactory</span></span>

<span data-ttu-id="5e53a-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) proporciona métodos para crear middleware.</span><span class="sxs-lookup"><span data-stu-id="5e53a-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="5e53a-132">La implementación de Middleware Factory se registra en el contenedor como un servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="5e53a-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="5e53a-133">La implementación `IMiddlewareFactory` predeterminada, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), se encuentra en el paquete [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="5e53a-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e53a-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5e53a-134">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
