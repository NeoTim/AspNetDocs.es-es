---
title: Registro de alto rendimiento con LoggerMessage en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar LoggerMessage para crear delegados almacenables en caché que requieren menos asignaciones de objetos que los escenarios de registro de alto rendimiento.
ms.author: riande
ms.date: 11/03/2017
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: a0080a20fed2d8fc295e55822c11d5731c6910ca
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048622"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="8d673-103">Registro de alto rendimiento con LoggerMessage en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d673-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="8d673-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d673-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8d673-105">Las características de [LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) crean delegados almacenables en caché que requieren menos asignaciones de objetos y una menor sobrecarga computacional en comparación con los [métodos de extensión del registrador](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), como `LogInformation`, `LogDebug` y `LogError`.</span><span class="sxs-lookup"><span data-stu-id="8d673-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="8d673-106">Para escenarios de registro de alto rendimiento, use el patrón `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="8d673-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="8d673-107">`LoggerMessage` proporciona las siguientes ventajas de rendimiento frente a los métodos de extensión del registrador:</span><span class="sxs-lookup"><span data-stu-id="8d673-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="8d673-108">Los métodos de extensión del registrador requieren la conversión boxing de tipos de valor, como `int`, en `object`.</span><span class="sxs-lookup"><span data-stu-id="8d673-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="8d673-109">El patrón `LoggerMessage` impide la conversión boxing mediante métodos de extensión y campos `Action` estáticos con parámetros fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="8d673-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="8d673-110">Los métodos de extensión del registrador deben analizar la plantilla de mensaje (cadena de formato con nombre) cada vez que se escribe un mensaje de registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="8d673-111">`LoggerMessage` solo necesita analizar una vez una plantilla cuando se define el mensaje.</span><span class="sxs-lookup"><span data-stu-id="8d673-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="8d673-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d673-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8d673-113">La aplicación de ejemplo muestra las características de `LoggerMessage` con un sistema de seguimiento de citas básico.</span><span class="sxs-lookup"><span data-stu-id="8d673-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="8d673-114">La aplicación agrega y elimina citas mediante una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="8d673-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="8d673-115">A medida que se producen estas operaciones, se generan mensajes de registro mediante el patrón `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="8d673-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="8d673-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="8d673-116">LoggerMessage.Define</span></span>

<span data-ttu-id="8d673-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crea un delegado `Action` para registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="8d673-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="8d673-118">Las sobrecargas `Define` permiten pasar hasta seis parámetros de tipo a una cadena de formato con nombre (plantilla).</span><span class="sxs-lookup"><span data-stu-id="8d673-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="8d673-119">La cadena proporcionada al método `Define` es una plantilla y no una cadena interpolada.</span><span class="sxs-lookup"><span data-stu-id="8d673-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="8d673-120">Los marcadores de posición se rellenan en el orden en que se especifican los tipos.</span><span class="sxs-lookup"><span data-stu-id="8d673-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="8d673-121">Los nombres de los marcadores de posición en la plantilla deben ser descriptivos y coherentes entre las plantillas.</span><span class="sxs-lookup"><span data-stu-id="8d673-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="8d673-122">Sirven como nombres de propiedad en los datos estructurados del registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="8d673-123">Se recomienda el uso de la [grafía Pascal](/dotnet/standard/design-guidelines/capitalization-conventions) para los nombres de los marcadores de posición.</span><span class="sxs-lookup"><span data-stu-id="8d673-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="8d673-124">Por ejemplo: `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="8d673-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="8d673-125">Cada mensaje de registro es un delegado `Action` que se mantiene en un campo estático creado por `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="8d673-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="8d673-126">Por ejemplo, la aplicación de ejemplo crea un campo que describe un mensaje de registro para una solicitud GET para la página de índice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8d673-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="8d673-127">Especifique lo siguiente para el delegado `Action`:</span><span class="sxs-lookup"><span data-stu-id="8d673-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="8d673-128">El nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-128">The log level.</span></span>
* <span data-ttu-id="8d673-129">Un identificador de evento único ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) con el nombre del método de extensión estático.</span><span class="sxs-lookup"><span data-stu-id="8d673-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="8d673-130">La plantilla de mensaje (cadena de formato con nombre).</span><span class="sxs-lookup"><span data-stu-id="8d673-130">The message template (named format string).</span></span> 

<span data-ttu-id="8d673-131">Una solicitud para la página de índice de la aplicación de ejemplo establece:</span><span class="sxs-lookup"><span data-stu-id="8d673-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="8d673-132">El nivel de registro en `Information`.</span><span class="sxs-lookup"><span data-stu-id="8d673-132">Log level to `Information`.</span></span>
* <span data-ttu-id="8d673-133">El identificador de evento en `1` con el nombre del método `IndexPageRequested`.</span><span class="sxs-lookup"><span data-stu-id="8d673-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="8d673-134">La plantilla de mensaje (cadena de formato con nombre) en una cadena.</span><span class="sxs-lookup"><span data-stu-id="8d673-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="8d673-135">Los almacenes de registro estructurado pueden usar el nombre de evento cuando se suministra con el identificador de evento para enriquecer el registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="8d673-136">Por ejemplo, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa el nombre de evento.</span><span class="sxs-lookup"><span data-stu-id="8d673-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="8d673-137">El delegado `Action` se invoca mediante un método de extensión fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="8d673-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="8d673-138">El método `IndexPageRequested` registra un mensaje para una solicitud GET de página de índice en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d673-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="8d673-139">Se llama a `IndexPageRequested` en el registrador en el método `OnGetAsync` en *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8d673-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="8d673-140">Inspeccione la salida de la consola de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8d673-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="8d673-141">Para pasar parámetros a un mensaje de registro, defina hasta seis tipos al crear el campo estático.</span><span class="sxs-lookup"><span data-stu-id="8d673-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="8d673-142">La aplicación de ejemplo registra una cadena cuando se agrega una cita. Para ello, define un tipo `string` para el campo `Action`:</span><span class="sxs-lookup"><span data-stu-id="8d673-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="8d673-143">La plantilla de mensaje de registro del delegado recibe sus valores de marcador de posición a partir de los tipos proporcionados.</span><span class="sxs-lookup"><span data-stu-id="8d673-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="8d673-144">La aplicación de ejemplo define un delegado para agregar una cita cuando el parámetro de la cita es `string`:</span><span class="sxs-lookup"><span data-stu-id="8d673-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="8d673-145">El método de extensión estático para agregar una cita, `QuoteAdded`, recibe el valor de argumento de la cita y lo pasa al delegado `Action`:</span><span class="sxs-lookup"><span data-stu-id="8d673-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="8d673-146">En el modelo de página para la página de índice (*Pages/Index.cshtml.cs*), se llama a `QuoteAdded` para registrar el mensaje:</span><span class="sxs-lookup"><span data-stu-id="8d673-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="8d673-147">Inspeccione la salida de la consola de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8d673-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="8d673-148">La aplicación de ejemplo implementa un patrón `try`&ndash;`catch` para la eliminación de la cita.</span><span class="sxs-lookup"><span data-stu-id="8d673-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="8d673-149">Se registra un mensaje informativo si se realiza correctamente una operación de eliminación.</span><span class="sxs-lookup"><span data-stu-id="8d673-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="8d673-150">Se registra un mensaje de error para una operación de eliminación si se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="8d673-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="8d673-151">El mensaje de registro de la operación de eliminación con error incluye el seguimiento de la pila de excepciones (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8d673-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="8d673-152">Observe cómo se pasa la excepción al delegado en `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="8d673-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="8d673-153">En el modelo de página para la página de índice, una operación correcta de eliminación de cita llama al método `QuoteDeleted` en el registrador.</span><span class="sxs-lookup"><span data-stu-id="8d673-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="8d673-154">Cuando no se encuentra una cita para su eliminación, se produce una excepción `ArgumentNullException`.</span><span class="sxs-lookup"><span data-stu-id="8d673-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="8d673-155">La excepción se captura mediante la instrucción `try`&ndash;`catch` y se registra mediante una llamada al método `QuoteDeleteFailed` en el registrador en el bloque `catch` (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8d673-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="8d673-156">Cuando se elimine correctamente una cita, inspeccione la salida de la consola de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8d673-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="8d673-157">Cuando se produzca un error en la eliminación de una cita, inspeccione la salida de la consola de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d673-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="8d673-158">Tenga en cuenta que la excepción se incluye en el mensaje del registro:</span><span class="sxs-lookup"><span data-stu-id="8d673-158">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="8d673-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="8d673-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="8d673-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crea un delegado `Func` para definir un [ámbito de registro](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="8d673-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="8d673-161">Las sobrecargas `DefineScope` permiten pasar hasta tres parámetros de tipo a una cadena de formato con nombre (plantilla).</span><span class="sxs-lookup"><span data-stu-id="8d673-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="8d673-162">Al igual que sucede con el método `Define`, la cadena proporcionada al método `DefineScope` es una plantilla y no una cadena interpolada.</span><span class="sxs-lookup"><span data-stu-id="8d673-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="8d673-163">Los marcadores de posición se rellenan en el orden en que se especifican los tipos.</span><span class="sxs-lookup"><span data-stu-id="8d673-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="8d673-164">Los nombres de los marcadores de posición en la plantilla deben ser descriptivos y coherentes entre las plantillas.</span><span class="sxs-lookup"><span data-stu-id="8d673-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="8d673-165">Sirven como nombres de propiedad en los datos estructurados del registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="8d673-166">Se recomienda el uso de la [grafía Pascal](/dotnet/standard/design-guidelines/capitalization-conventions) para los nombres de los marcadores de posición.</span><span class="sxs-lookup"><span data-stu-id="8d673-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="8d673-167">Por ejemplo: `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="8d673-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="8d673-168">Defina un [ámbito de registro](xref:fundamentals/logging/index#log-scopes) para aplicarlo a una serie de mensajes de registro mediante el método [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope).</span><span class="sxs-lookup"><span data-stu-id="8d673-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="8d673-169">La aplicación de ejemplo tiene un botón **Borrar todo** para eliminar todas las citas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d673-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="8d673-170">Para eliminar las citas, se van quitando de una en una.</span><span class="sxs-lookup"><span data-stu-id="8d673-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="8d673-171">Cada vez que se elimina una cita, se llama al método `QuoteDeleted` en el registrador.</span><span class="sxs-lookup"><span data-stu-id="8d673-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="8d673-172">Se agrega un ámbito de registro a estos mensajes de registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="8d673-173">Habilite `IncludeScopes` en la sección del registrador de la consola de *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8d673-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

<span data-ttu-id="8d673-174">Para crear un ámbito de registro, agregue un campo para que contenga un delegado `Func` para el ámbito.</span><span class="sxs-lookup"><span data-stu-id="8d673-174">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="8d673-175">La aplicación de ejemplo crea un campo denominado `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8d673-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="8d673-176">Use `DefineScope` para crear el delegado.</span><span class="sxs-lookup"><span data-stu-id="8d673-176">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="8d673-177">Pueden especificarse hasta tres tipos para usarlos como argumentos de plantilla cuando se invoca el delegado.</span><span class="sxs-lookup"><span data-stu-id="8d673-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="8d673-178">La aplicación de ejemplo usa una plantilla de mensaje que incluye el número de citas eliminadas (un tipo `int`):</span><span class="sxs-lookup"><span data-stu-id="8d673-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="8d673-179">Proporcione un método de extensión estático para el mensaje de registro.</span><span class="sxs-lookup"><span data-stu-id="8d673-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="8d673-180">Incluya todos los parámetros de tipo para propiedades con nombre que aparezcan en la plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="8d673-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="8d673-181">La aplicación de ejemplo toma un valor de número `count` de citas que se van a eliminar y devuelve `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="8d673-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="8d673-182">El ámbito encapsula las llamadas a la extensión de registro en un bloque `using`:</span><span class="sxs-lookup"><span data-stu-id="8d673-182">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="8d673-183">Inspeccione los mensajes de registro en la salida de la consola de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d673-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="8d673-184">En el resultado siguiente se muestran tres citas eliminadas con el mensaje del ámbito de registro incluido:</span><span class="sxs-lookup"><span data-stu-id="8d673-184">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a><span data-ttu-id="8d673-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8d673-185">Additional resources</span></span>

* [<span data-ttu-id="8d673-186">Registro</span><span class="sxs-lookup"><span data-stu-id="8d673-186">Logging</span></span>](xref:fundamentals/logging/index)
