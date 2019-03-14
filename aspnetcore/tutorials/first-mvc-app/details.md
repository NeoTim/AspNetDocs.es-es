---
title: Examinar los métodos Details y Delete de una aplicación ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la vista y el método de controlador Details en una aplicación básica ASP.NET Core MVC.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f674ca1761f85ce127121603286c97d5936f6716
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043412"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Examinar los métodos Details y Delete de una aplicación ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Abra el controlador Movie y examine el método `Details`:

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

El motor de scaffolding de MVC que creó este método de acción agrega un comentario en el que se muestra una solicitud HTTP que invoca el método. En este caso se trata de una solicitud GET con tres segmentos de dirección URL, el controlador `Movies`, el método `Details` y un valor `id`. Recuerde que estos segmentos se definen en *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF facilita el proceso de búsqueda de datos mediante el método `FirstOrDefaultAsync`. Una característica de seguridad importante integrada en el método es que el código comprueba que el método de búsqueda haya encontrado una película antes de intentar hacer nada con ella. Por ejemplo, un pirata informático podría introducir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` a algo parecido a `http://localhost:xxxx/Movies/Details/12345` (o algún otro valor que no represente una película real). Si no comprobara una película null, la aplicación generaría una excepción.

Examine los métodos `Delete` y `DeleteConfirmed`.

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

Tenga en cuenta que el método `HTTP GET Delete` no elimina la película especificada, sino que devuelve una vista de la película donde puede enviar (HttpPost) la eliminación. La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad.

El método `[HttpPost]` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

Common Language Runtime (CLR) requiere métodos sobrecargados para disponer de una firma de parámetro única (mismo nombre de método, pero lista de parámetros diferente). En cambio, aquí necesita dos métodos `Delete` (uno para GET y otro para POST) que tienen la misma firma de parámetro (ambos deben aceptar un número entero como parámetro).

Hay dos enfoques para este problema. Uno consiste en proporcionar nombres diferentes a los métodos, que es lo que hizo el mecanismo de scaffolding en el ejemplo anterior. Pero esto implica un pequeño problema: ASP.NET asigna los segmentos de una dirección URL a los métodos de acción por nombre, de modo que si cambia el nombre de un método, el enrutamiento seguramente no podrá encontrar dicho método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. Ese atributo efectúa la asignación para el sistema de enrutamiento para que una dirección URL que incluya /Delete/ para una solicitud POST busque el método `DeleteConfirmed`.

Otra solución alternativa común para los métodos que tienen nombres y firmas idénticos consiste en cambiar la firma del método POST artificialmente para incluir un parámetro adicional (sin usar). Es lo que hicimos en una publicación anterior, cuando agregamos el parámetro `notUsed`. Podría hacer lo mismo aquí para el método `[HttpPost] Delete`:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publicar en Azure

Para obtener información sobre la implementación en Azure, consulte [Tutorial: Creación de una aplicación .NET Core y SQL Database en Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).

> [!div class="step-by-step"]
> [Anterior](validation.md)
