---
ms.openlocfilehash: a0546c44284a78ce0e8d06b0f2b9f65ecf66fac7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031692"
---

La anotación de datos `[Column(TypeName = "decimal(18, 2)")]` es necesaria para que Entity Framework Core asigne correctamente `Price` a la moneda en la base de datos. Para más información, vea [Tipos de datos](/ef/core/modeling/relational/data-types).

Modelo completado:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1)]

En el próximo tutorial, hablaremos de [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6). El atributo [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate"). El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de los datos (Date), así que la información de hora almacenada en el campo no se muestra.

Vaya a Pages/Movies y mantenga el mouse sobre un vínculo de **edición** para ver la dirección URL de destino.

![Ventana del explorador con el mouse sobre el vínculo Edit (Editar) donde se muestra una dirección URL de vínculo http://localhost:1234/Movies/Edit/5](~/tutorials/razor-pages/da1/edit7.png)

Los vínculos **Edit**, **Details** y **Delete** son generados por el [asistente de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Movies/Index.cshtml*.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor. En el código anterior, `AnchorTagHelper` genera de forma dinámica el valor del atributo `href` HTML desde la página de Razor (la ruta es relativa), el elemento `asp-page` y el identificador de ruta (`asp-route-id`). Vea [Generación de direcciones URL para las páginas](xref:razor-pages/index#url-generation-for-pages) para obtener más información.

Use **Ver código fuente** en su explorador preferido para examinar el marcado generado. A continuación se muestra una parte del HTML generado:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Los vínculos generados de forma dinámica pasan el identificador de la película con una cadena de consulta (por ejemplo, `http://localhost:5000/Movies/Details?id=2`).

Actualice las páginas de edición, detalles y eliminación de Razor para usar la plantilla de ruta "{id:int}". Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`. Ejecute la aplicación y luego vea el origen. El HTML generado agrega el identificador a la parte de la ruta de acceso de la dirección URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya el entero devolverá un error HTTP 404 (no encontrado). Por ejemplo, `http://localhost:5000/Movies/Details` devolverá un error 404. Para que el identificador sea opcional, anexe `?` a la restricción de ruta:

 ```cshtml
@page "{id:int?}"
```

::: moniker range="= aspnetcore-2.0"

### <a name="update-concurrency-exception-handling"></a>Actualización del control de excepciones de simultaneidad

Actualice el método `OnPostAsync` en el archivo *Pages/Movies/Edit.cshtml.cs*. En el código resaltado siguiente se muestran los cambios:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

El código anterior solo detecta las excepciones de simultaneidad cuando el primer cliente simultáneo elimina la película y el segundo cliente simultáneo publica cambios en ella.

Para probar el bloque `catch`:

* Establecer un punto de interrupción en `catch (DbUpdateConcurrencyException)`
* Edite una película.
* En otra ventana del explorador, seleccione el vínculo de **eliminación** de la misma película y luego elimínela.
* En la ventana anterior del explorador, publique los cambios en la película.

El código de producción por lo general debería detectar conflictos de simultaneidad cuando dos o más clientes actualizan un registro de forma simultánea. Vea [Administración de conflictos de simultaneidad](xref:data/ef-rp/concurrency) para más información.

### <a name="posting-and-binding-review"></a>Revisión de publicaciones y enlaces

Examine el archivo *Pages/Movies/Edit.cshtml.cs*:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

Cuando se realiza una solicitud HTTP GET a la página Movies/Edit (por ejemplo, `http://localhost:5000/Movies/Edit/2`):

* El método `OnGetAsync` obtiene la película en la base de datos y devuelve el método `Page`. 
* El método `Page` presenta la página de Razor *Pages/Movies/Edit.cshtml*. El archivo *Pages/Movies/Edit.cshtml* contiene la directiva de modelo (`@model RazorPagesMovie.Pages.Movies.EditModel`), que hace que el modelo de película esté disponible en la página.
* Se abre el formulario de edición con los valores de la película.

Cuando se publica la página Movies/Edit:

* Los valores del formulario de la página se enlazan a la propiedad `Movie`. El atributo `[BindProperty]` habilita el [enlace de modelos](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Si hay errores en el estado del modelo (por ejemplo, `ReleaseDate` no se puede convertir en una fecha), el formulario se vuelve a publicar con los valores enviados.
* Si no hay ningún error en el modelo, se guarda la película.

Los métodos HTTP GET de las páginas de índice, creación y eliminación de Razor siguen un patrón similar. El método HTTP POST `OnPostAsync` de la página de creación de Razor sigue un patrón similar al del método `OnPostAsync` de la página de edición de Razor.

La búsqueda se incluye en el tutorial siguiente.
