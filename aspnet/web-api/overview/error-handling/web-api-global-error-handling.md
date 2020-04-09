---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Control global de errores en ASP.NET Web API 2 - ASP.NET 4.x
author: davidmatson
description: Una visión general del control de errores global en ASP.NET Web API 2 para ASP.NET 4.x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 5ff54d2e4ed881ce927d0a401fb79d9b8bc5b8a1
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675428"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Control global de errores en ASP.NET Web API 2

por [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tema se proporciona información general sobre el control global de errores en ASP.NET Web API 2 para ASP.NET 4.x. Hoy en día no hay una manera fácil en Web API de registrar o controlar errores globalmente. Algunas excepciones no controladas se pueden procesar mediante filtros de [excepciones,](exception-handling.md)pero hay una serie de casos que los filtros de excepciones no pueden controlar. Por ejemplo:

1. Excepciones iniciadas por constructores del controlador.
2. Excepciones iniciadas por controladores de mensajes.
3. Excepciones iniciadas durante el enrutamiento.
4. Excepciones iniciadas durante la serialización del contenido de respuesta.

Queremos proporcionar una forma sencilla y coherente de registrar y controlar (cuando sea posible) estas excepciones. 

Hay dos casos principales para controlar excepciones, el caso en el que podemos enviar una respuesta de error y el caso en el que todo lo que podemos hacer es registrar la excepción. Un ejemplo para este último caso es cuando se produce una excepción en medio del contenido de respuesta de streaming; en ese caso, es demasiado tarde para enviar un nuevo mensaje de respuesta ya que el código de estado, los encabezados y el contenido parcial ya han pasado a través de la conexión, por lo que simplemente anulamos la conexión. Aunque la excepción no se puede controlar para generar un nuevo mensaje de respuesta, todavía se admite el registro de la excepción. En los casos en los que podemos detectar un error, podemos devolver una respuesta de error adecuada como se muestra a continuación:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opciones existentes

Además de los filtros de [excepciones,](exception-handling.md)los [controladores](../advanced/http-message-handlers.md) de mensajes se pueden usar hoy en día para observar todas las respuestas de 500 niveles, pero actuar en esas respuestas es difícil, ya que carecen de contexto sobre el error original. Los controladores de mensajes también tienen algunas de las mismas limitaciones que los filtros de excepciones con respecto a los casos que pueden controlar. Aunque La API web tiene una infraestructura de seguimiento que captura las condiciones de error, la infraestructura de seguimiento tiene fines de diagnóstico y no está diseñada ni es adecuada para ejecutarse en entornos de producción. El control y el registro de excepciones globales deben ser servicios que se pueden ejecutar durante la producción y conectarse a soluciones de supervisión existentes (por ejemplo, [ELMAH).](https://code.google.com/p/elmah/)

### <a name="solution-overview"></a>Información general de la solución

 Proporcionamos dos nuevos servicios reemplazables por el usuario, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, para registrar y controlar excepciones no controladas. Los servicios son muy similares, con dos diferencias principales:

1. Admitimos el registro de varios registradores de excepciones, pero solo un único controlador de excepciones.
2. Los registradores de excepciones siempre reciben llamadas, incluso si estamos a punto de anular la conexión. Solo se llama a los controladores de excepciones cuando todavía podemos elegir qué mensaje de respuesta enviar.

Ambos servicios proporcionan acceso a un contexto de excepción que contiene información relevante desde el punto donde se detectó la excepción, especialmente [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), la excepción iniciada y el origen de la excepción (detalles a continuación).

### <a name="design-principles"></a>Principios de diseño

1. **Sin cambios importantes** Dado que esta funcionalidad se agrega en una versión secundaria, una restricción importante que afecta a la solución es que no hay cambios importantes, ya sea para escribir contratos o para el comportamiento. Esta restricción descartó alguna limpieza que nos gustaría haber hecho en términos de bloques de captura existentes que convierten las excepciones en 500 respuestas. Esta limpieza adicional es algo que podríamos considerar para una versión principal posterior. Si esto es importante para usted, por favor vote sobre él en ASP.NET voz del usuario de [la API web.](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)
2. **Mantener la coherencia con las construcciones** de la API web La canalización de filtros de la API web es una excelente manera de controlar las preocupaciones transversales con la flexibilidad de aplicar la lógica en un ámbito global, específico de controlador o específico de la acción. Los filtros, incluidos los filtros de excepción, siempre tienen contextos de acción y controlador, incluso cuando se registran en el ámbito global. Ese contrato tiene sentido para los filtros, pero significa que los filtros de excepción, incluso los con ámbito global, no son adecuados para algunos casos de control de excepciones, como las excepciones de los controladores de mensajes, donde no existe ningún contexto de acción o controlador. Si queremos usar el ámbito flexible que ofrecen los filtros para el control de excepciones, todavía necesitamos filtros de excepción. Pero si necesitamos controlar la excepción fuera de un contexto de controlador, también necesitamos una construcción independiente para el control de errores global completo (algo sin el contexto del controlador y las restricciones de contexto de acción).

### <a name="when-to-use"></a>Cuándo usar

- Los registradores de excepciones son la solución para ver todas las excepciones no controladas detectadas por Web API.
- Los controladores de excepciones son la solución para personalizar todas las posibles respuestas a las excepciones no controladas detectadas por la API web.
- Los filtros de excepciones son la solución más fácil para procesar las excepciones no controladas del subconjunto relacionadas con una acción o controlador específico.

### <a name="service-details"></a>Detalles del servicio

 Las interfaces de servicio de controlador y registrador de excepciones son métodos asincrónicos simples que toman los contextos respectivos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 También proporcionamos clases base para ambas interfaces. Reemplazar los métodos principales (sincronizar o asincrónicos) es todo lo que se requiere para registrar o controlar en los momentos recomendados. Para el `ExceptionLogger` registro, la clase base se asegurará de que el método de registro principal solo se llama una vez para cada excepción (incluso si posteriormente se propaga más arriba de la pila de llamadas y se detecta de nuevo). La `ExceptionHandler` clase base llamará al método de control principal solo para las excepciones en la parte superior de la pila de llamadas, ignorando los bloques de captura anidados heredados. (Las versiones simplificadas de estas clases base se encuentran en el apéndice siguiente.) Ambos `IExceptionLogger` `IExceptionHandler` y recibir información sobre `ExceptionContext`la excepción a través de un archivo .

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Cuando el marco de trabajo llama a un registrador `Exception` de `Request`excepciones o un controlador de excepciones, siempre proporcionará un archivo y un archivo . A excepción de las pruebas `RequestContext`unitarias, también siempre proporcionará un archivo . Rara vez proporcionará `ControllerContext` a `ActionContext` y (sólo cuando se llama desde el bloque catch para los filtros de excepción). Muy rara vez proporcionará `Response`un (sólo en ciertos casos de IIS cuando en el medio de tratar de escribir la respuesta). Tenga en cuenta que, `null` dado que algunas de `null` estas propiedades pueden ser, depende del consumidor comprobar antes de tener acceso a los miembros de la clase de excepción.`CatchBlock` es una cadena que indica qué bloque catch vio la excepción. Las cadenas de bloque catch son las siguientes:

- HttpServer (método SendAsync)
- HttpControllerDispatcher (método SendAsync)
- HttpBatchHandler (método SendAsync)
- IExceptionFilter (procesamiento de ApiController de la canalización de filtro de excepción en ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (para almacenar en búfer)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (para la salida de streaming)
- Host web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (para la salida de almacenamiento en búfer)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (para la salida de streaming)
    - HttpControllerHandler.WriteErrorResponseContentAsync (para errores en la recuperación de errores en modo de salida almacenado en búfer)

La lista de cadenas de bloque catch también está disponible a través de propiedades estáticas readonly. (La cadena de bloque catch principal se encuentra en la estática ExceptionCatchBlocks; el resto aparece en una clase estática cada uno para OWIN y host web).`IsTopLevelCatchBlock` es útil para seguir el patrón recomendado de control de excepciones solo en la parte superior de la pila de llamadas. En lugar de convertir las excepciones en 500 respuestas en cualquier lugar donde se produzca un bloque catch anidado, un controlador de excepciones puede permitir que las excepciones se propaguen hasta que el host las vea.

Además `ExceptionContext`de la , un registrador obtiene una `ExceptionLoggerContext`pieza más de información a través de la completa :

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La segunda `CanBeHandled`propiedad, , permite a un registrador identificar una excepción que no se puede controlar. Cuando la conexión está a punto de anularse y no se puede enviar ningún mensaje de respuesta nuevo, se llamará a los registradores, pero ***no*** se llamará al controlador y los registradores pueden identificar este escenario desde esta propiedad.

Además `ExceptionContext`de la , un controlador obtiene una `ExceptionHandlerContext` propiedad más que puede establecer en el conjunto para controlar la excepción:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un controlador de excepciones indica que ha `Result` controlado una excepción estableciendo la propiedad en un resultado de acción (por ejemplo, un [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)o un resultado personalizado). Si `Result` la propiedad es null, la excepción no se controla y se volverá a producir la excepción original.

Para las excepciones en la parte superior de la pila de llamadas, hemos tomado un paso adicional para garantizar que la respuesta sea adecuada para los llamadores de API. Si la excepción se propaga hasta el host, el autor de la llamada vería la pantalla amarilla de la muerte o alguna respuesta proporcionada por algún otro host que normalmente es HTML y no suele ser una respuesta de error de API adecuada. En estos casos, el resultado se inicia sin null y solo si `null` un controlador de excepciones personalizado lo establece explícitamente en (no controlado) se propagará la excepción al host. Establecer `Result` `null` en estos casos puede ser útil para dos escenarios:

1. API web hospedada en OWIN con middleware de control de excepciones personalizado registrado antes o fuera de la API web.
2. Depuración local a través de un explorador, donde la pantalla amarilla de la muerte es en realidad una respuesta útil para una excepción no controlada.

Tanto para los registradores de excepciones como para los controladores de excepciones, no hacemos nada para recuperar si el propio registrador o controlador produce una excepción. (Aparte de dejar que la excepción se propague, deje comentarios en la parte inferior de esta página si tiene un mejor enfoque.) El contrato para registradores de excepciones y controladores es que no deben permitir que las excepciones se propaguen a sus llamadores; de lo contrario, la excepción simplemente se propagará, a menudo hasta el host, lo que resulta en un error HTML (como ASP. Pantalla amarilla de NET) que se devuelve al cliente (que normalmente no es la opción preferida para los llamadores de API que esperan JSON o XML).

## <a name="examples"></a>Ejemplos

### <a name="tracing-exception-logger"></a>Seguimiento del registrador de excepciones

El registrador de excepciones siguiente envía datos de excepción a orígenes de seguimiento configurados (incluida la ventana de salida Depurar en Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Controlador de excepciones de mensajes de error personalizados

El controlador de excepciones siguiente genera una respuesta de error personalizada a los clientes, incluida una dirección de correo electrónico para ponerse en contacto con el soporte técnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registro de filtros de excepción

Si usa la plantilla de proyecto "ASP.NET MVC 4 Web Application" para `WebApiConfig` crear el proyecto, coloque el código de configuración de La API web dentro de la clase, en la carpeta *App_Start:*

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apéndice: Detalles de la clase base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
