---
ms.openlocfilehash: 96790edbeb4313fd9de585fc176611946b313355
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063732"
---

En el próximo tutorial hablaremos de [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6). El atributo [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate"). El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de los datos (Date), así que la información de hora almacenada en el campo no se muestra.

La anotación de datos `[Column(TypeName = "decimal(18, 2)")]` es necesaria para que Entity Framework Core asigne correctamente `Price` a la moneda en la base de datos. Para más información, vea [Tipos de datos](/ef/core/modeling/relational/data-types).

Vaya al controlador `Movies` y mantenga el puntero del mouse sobre un vínculo **Edit** (Editar) para ver la dirección URL de destino.

![Ventana del explorador con el mouse sobre el vínculo Edit (Editar) donde se muestra una dirección URL de vínculo http://localhost:1234/Movies/Edit/5](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

Los vínculos **Edit** (Editar), **Details** (Detalles) y **Delete** (Eliminar) se generan mediante el asistente de etiquetas de delimitador de MVC Core en el archivo *Views/Movies/Index.cshtml*.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor. En el código anterior, `AnchorTagHelper` genera dinámicamente el valor del atributo HTML `href` a partir del identificador de ruta y el método de acción del controlador. Use **Ver código fuente** en su explorador preferido o use las herramientas de desarrollo para examinar el marcado generado. A continuación se muestra una parte del HTML generado:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Recupere el formato para el [enrutamiento](xref:mvc/controllers/routing) establecido en el archivo *Startup.cs*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core traduce `http://localhost:1234/Movies/Edit/4` en una solicitud al método de acción `Edit` del controlador `Movies` con el parámetro `Id` de 4. (Los métodos de controlador también se denominan métodos de acción).

Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) son una de las nuevas características más populares de ASP.NET Core. Para más información, vea [Recursos adicionales](#additional-resources).

Abra el controlador `Movies` y examine los dos métodos de acción `Edit`. En el código siguiente se muestra el método `HTTP GET Edit`, que captura la película y rellena el formulario de edición generado por el archivo de Razor *Edit.cshtml*.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

En el código siguiente se muestra el método `HTTP POST Edit`, que procesa los valores de película publicados:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

En el código siguiente se muestra el método `HTTP POST Edit`, que procesa los valores de película publicados:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

El atributo `[Bind]` es una manera de proteger contra el [exceso de publicación](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Solo debe incluir propiedades en el atributo `[Bind]` que quiera cambiar. Para más información, vea [Protección del controlador frente al exceso de publicación](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application). [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) ofrece un enfoque alternativo para evitar el exceso de publicaciones.

Observe que el segundo método de acción `Edit` va precedido del atributo `[HttpPost]`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

El atributo `HttpPost` especifica que este método `Edit` se puede invocar *solamente* para solicitudes `POST`. Podría aplicar el atributo `[HttpGet]` al primer método de edición, pero no es necesario hacerlo porque `[HttpGet]` es el valor predeterminado.

El atributo `ValidateAntiForgeryToken` se usa para [impedir la falsificación de una solicitud](xref:security/anti-request-forgery) y se empareja con un token antifalsificación generado en el archivo de vista de edición (*Views/Movies/Edit.cshtml*). El archivo de vista de edición genera el token antifalsificación con el [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms).

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

El [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms) genera un token antifalsificación oculto que debe coincidir con el token antifalsificación generado por `[ValidateAntiForgeryToken]` en el método `Edit` del controlador Movies. Para más información, vea [Prevención de ataques de falsificación de solicitudes](xref:security/anti-request-forgery).

El método `HttpGet Edit` toma el parámetro `ID` de la película, busca la película con el método `SingleOrDefaultAsync` de Entity Framework y devuelve la película seleccionada a la vista de edición. Si no se encuentra una película, se devuelve `NotFound` (HTTP 404).

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

::: moniker-end

Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. En el ejemplo siguiente se muestra la vista de edición que generó el sistema de scaffolding de Visual Studio:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

Observe cómo la plantilla de vista tiene una instrucción `@model MvcMovie.Models.Movie` en la parte superior del archivo. `@model MvcMovie.Models.Movie` especifica que la vista espera que el modelo de la plantilla de vista sea del tipo `Movie`.

El código con scaffolding usa varios métodos del asistente de etiquetas para simplificar el marcado HTML. El [asistente de etiquetas](xref:mvc/views/working-with-forms) muestra el nombre del campo: "Title" (Título), "ReleaseDate" (Fecha de lanzamiento), "Genre" (Género) o "Price" (Precio). El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) representa un elemento HTML `<input>`. El [asistente de etiquetas de validación](xref:mvc/views/working-with-forms) muestra cualquier mensaje de validación asociado a esa propiedad.

Ejecute la aplicación y navegue a la URL `/Movies`. Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. El código HTML generado para el elemento `<form>` se muestra abajo.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

Los elementos `<input>` se muestran en un elemento `HTML <form>` cuyo atributo `action` se establece para publicar en la dirección URL `/Movies/Edit/id`. Los datos del formulario se publicarán en el servidor cuando se haga clic en el botón `Save`. La última línea antes del cierre del elemento `</form>` muestra el token [XSRF](xref:security/anti-request-forgery) oculto generado por el [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `[HttpPost]` del método de acción `Edit`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

El atributo `[ValidateAntiForgeryToken]` valida el token [XSRF](xref:security/anti-request-forgery) oculto generado por el generador de tokens antifalsificación en el [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms).

El sistema de [enlace de modelos](xref:mvc/models/model-binding) toma los valores de formulario publicados y crea un objeto `Movie` que se pasa como el parámetro `movie`. El método `ModelState.IsValid` comprueba que los datos presentados en el formulario pueden usarse para modificar (editar o actualizar) un objeto `Movie`. Si los datos son válidos, se guardan. Los datos de película actualizados (o modificados) se guardan en la base de datos mediante una llamada al método `SaveChangesAsync` del contexto de base de datos. Después de guardar los datos, el código redirige al usuario al método de acción `Index` de la clase `MoviesController`, que muestra la colección de películas, incluidos los cambios que se acaban de hacer.

Antes de que el formulario se envíe al servidor, la validación del lado cliente comprueba cualquier regla de validación en los campos. Si hay errores de validación, se muestra un mensaje de error y no se publica el formulario. Si JavaScript está deshabilitado, no dispondrá de la validación del lado cliente, sino que el servidor detectará los valores publicados que no son válidos y los valores de formulario se volverán a mostrar con mensajes de error. Más adelante en el tutorial se examina la [validación de modelos](xref:mvc/models/validation) con más detalle. El [asistente de etiquetas de validación](xref:mvc/views/working-with-forms) en la plantilla de vista *Views/Movies/Edit.cshtml* se encarga de mostrar los mensajes de error correspondientes.

![Vista de edición: excepción de un valor de precio incorrecto de los estados abc que indica que el campo de precio debe ser un número. Excepción para un valor incorrecto de fecha de lanzamiento de los estados xyz. Vuelva a escribir una fecha válida.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Todos los métodos `HttpGet` del controlador de películas siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index`) y pasan el objeto (modelo) a la vista. El método `Create` pasa un objeto de película vacío a la vista `Create`. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `[HttpPost]` del método. La modificación de datos en un método `HTTP GET` supone un riesgo de seguridad. La modificación de datos en un método `HTTP GET` también infringe procedimientos recomendados de HTTP y el patrón de arquitectura [REST](http://rest.elkstein.org/), que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debería ser una operación segura sin efectos secundarios, que no modifica los datos persistentes.

## <a name="additional-resources"></a>Recursos adicionales

* [Globalización y localización](xref:fundamentals/localization)
* [Introducción a los asistentes de etiquetas](xref:mvc/views/tag-helpers/intro)
* [Creación de asistentes de etiquetas](xref:mvc/views/tag-helpers/authoring)
* [Prevención de ataques de falsificación de solicitudes](xref:security/anti-request-forgery)
* Protección del controlador frente al [exceso de publicación](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Asistente de etiquetas de formulario](xref:mvc/views/working-with-forms)
* [Asistente de etiquetas de entrada](xref:mvc/views/working-with-forms)
* [Asistente de etiquetas de elementos de etiqueta](xref:mvc/views/working-with-forms)
* [Asistente de etiquetas de selección](xref:mvc/views/working-with-forms)
* [Asistente de etiquetas de validación](xref:mvc/views/working-with-forms)
