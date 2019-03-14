---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)'
author: rick-anderson
description: En este tutorial, leerá y mostrará datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: cf8733e1e806c4be0c4b217fc45c7a338a03a3ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061862"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

En este tutorial, se leen y se muestran datos relacionados. Los datos relacionados son los que EF Core carga en las propiedades de navegación.

Si experimenta problemas que no puede resolver, [descargue o vea la aplicación completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). [Instrucciones de descarga](xref:index#how-to-download-a-sample).

En las ilustraciones siguientes se muestran las páginas completadas para este tutorial:

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Carga diligente, explícita y diferida de datos relacionados

EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:

* [Carga diligente](/ef/core/querying/related-data#eager-loading). La carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas. Cuando se lee la entidad, se recuperan sus datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. EF Core emitirá varias consultas para algunos tipos de carga diligente. La emisión de varias consultas puede ser más eficaz de lo que eran algunas consultas de EF6 cuando había una sola consulta. La carga diligente se especifica con los métodos `Include` y `ThenInclude`.

  ![Ejemplo de carga diligente](read-related-data/_static/eager-loading.png)
 
  La carga diligente envía varias consultas cuando se incluye una propiedad de navegación de colección:

  * Una consulta para la consulta principal 
  * Una consulta para cada colección "perimetral" en el árbol de la carga.

* Separe las consultas con `Load`: los datos se pueden recuperar en distintas consultas y EF Core "corrige" las propiedades de navegación. "Corregir" significa que EF Core rellena automáticamente las propiedades de navegación. Separar las consultas con `Load` es más parecido a la carga explícita que a la carga diligente.

  ![Ejemplo de consultas independientes](read-related-data/_static/separate-queries.png)

  Nota: EF Core corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia del contexto. Incluso si los datos de una propiedad de navegación *no* se incluyen explícitamente, es posible que la propiedad se siga rellenando si algunas o todas las entidades relacionadas se cargaron previamente.

* [Carga explícita](/ef/core/querying/related-data#explicit-loading). Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Se debe escribir código para recuperar los datos relacionados cuando sea necesario. La carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos. Con la carga explícita, el código especifica las propiedades de navegación que se van a cargar. Use el método `Load` para realizar la carga explícita. Por ejemplo:

  ![Ejemplo de carga explícita](read-related-data/_static/explicit-loading.png)

* [Carga diferida](/ef/core/querying/related-data#lazy-loading). [Se ha agregado la carga diferida a EF Core en la versión 2.1](/ef/core/querying/related-data#lazy-loading). Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. La primera vez que se obtiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Cada vez que se obtiene acceso a una propiedad de navegación, se envía una consulta a la base de datos.

* El operador `Select` solo carga los datos relacionados necesarios.

## <a name="create-a-course-page-that-displays-department-name"></a>Crear una página de cursos en la que se muestre el nombre de departamento

La entidad Course incluye una propiedad de navegación que contiene la entidad `Department`. La entidad `Department` contiene el departamento al que se asigna el curso.

Para mostrar el nombre del departamento asignado en una lista de cursos:

* Obtenga la propiedad `Name` desde la entidad `Department`.
* La entidad `Department` procede de la propiedad de navegación `Course.Department`.

![course.Departament](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Aplicar scaffolding al modelo de Course

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Course` para la clase de modelo.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

 Ejecute el siguiente comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

------

El comando anterior aplica scaffolding al modelo `Course`. Abra el proyecto en Visual Studio.

Abra *Pages/Courses/Index.cshtml.cs* y examine el método `OnGetAsync`. El motor de scaffolding especificado realiza la carga diligente de la propiedad de navegación `Department`. El método `Include` especifica la carga diligente.

Ejecute la aplicación y haga clic en el vínculo **Courses**. En la columna Department se muestra el `DepartmentID`, lo que no resulta útil.

Actualice el método `OnGetAsync` con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

El código anterior agrega `AsNoTracking`. `AsNoTracking` mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas. No se realiza el seguimiento de las entidades porque no se actualizan en el contexto actual.

Actualice *Pages/Courses/Index.cshtml* con el marcado resaltado siguiente:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Se han realizado los cambios siguientes en el código con scaffolding:

* Ha cambiado el título de Index a Courses.
* Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`. De forma predeterminada, las claves principales no tienen scaffolding porque normalmente no tienen sentido para los usuarios finales. Pero en este caso, la clave principal es significativa.
* Ha cambiado la columna **Department** para mostrar el nombre del departamento. El código muestra la propiedad `Name` de la entidad `Department` que se carga en la propiedad de navegación `Department`:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Ejecute la aplicación y haga clic en la pestaña **Courses** para ver la lista con los nombres de departamento.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Carga de datos relacionados con Select

El método `OnGetAsync` carga los datos relacionados con el método `Include`:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

El operador `Select` solo carga los datos relacionados necesarios. Para elementos individuales, como el `Department.Name`, se usa INNER JOIN de SQL. En las colecciones, se usa otro acceso de base de datos, como también hace el operador `Include` en las colecciones.

En el código siguiente se cargan los datos relacionados con el método `Select`:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

El `CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Vea [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Crear una página de instructores en la que se muestran los cursos y las inscripciones

En esta sección, se crea la página de instructores.

<a name="IP"></a>
![Página de índice de instructores](read-related-data/_static/instructors-index.png)

En esta página se leen y muestran los datos relacionados de las maneras siguientes:

* En la lista de instructores se muestran datos relacionados de la entidad `OfficeAssignment` (Office en la imagen anterior). Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno. Para las entidades `OfficeAssignment` se usa la carga diligente. Normalmente la carga diligente es más eficaz cuando es necesario mostrar los datos relacionados. En este caso, se muestran las asignaciones de oficina para los instructores.
* Cuando el usuario selecciona un instructor (Harui en la imagen anterior), se muestran las entidades `Course` relacionadas. Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios. La carga diligente se usa con las entidades `Course` y sus entidades `Department` relacionadas. En este caso, es posible que las consultas independientes sean más eficaces porque solo se necesitan cursos para el instructor seleccionado. En este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación en entidades que se encuentran en propiedades de navegación.
* Cuando el usuario selecciona un curso (Chemistry [Química] en la imagen anterior), se muestran los datos relacionados de la entidad `Enrollments`. En la imagen anterior, se muestra el nombre del alumno y la calificación. Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice de instructores

En la página Instructors se muestran datos de tres tablas diferentes. Se crea un modelo de vista que incluye las tres entidades que representan las tres tablas.

En la carpeta *SchoolViewModels*, cree *InstructorIndexData.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Aplicar scaffolding al modelo de Instructor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Instructor` para la clase de modelo.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

 Ejecute el siguiente comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

------

El comando anterior aplica scaffolding al modelo `Instructor`. Ejecute la aplicación y vaya a la página de instructores.

Reemplace *Pages/Instructors/Index.cshtml.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

El método `OnGetAsync` acepta datos de ruta opcionales para el identificador del instructor seleccionado.

Examine la consulta en el archivo *Pages/Instructors/Index.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

La consulta tiene dos instrucciones include:

* `OfficeAssignment`: se muestra en la [vista de instructores](#IP).
* `CourseAssignments`: muestra los cursos impartidos.


### <a name="update-the-instructors-index-page"></a>Actualizar la página de índice de instructores

Actualice *Pages/Instructors/Index.cshtml* con el marcado siguiente:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

En el marcado anterior se realizan los cambios siguientes:

* Se actualiza la directiva `page` de `@page` a `@page "{id:int?}"`. `"{id:int?}"` es una plantilla de ruta. La plantilla de ruta cambia las cadenas de consulta enteras de la dirección URL por datos de ruta. Por ejemplo, al hacer clic en el vínculo **Select** de un instructor con únicamente la directiva `@page`, se genera una dirección URL similar a la siguiente:

    `http://localhost:1234/Instructors?id=2`

    Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL anterior es:

    `http://localhost:1234/Instructors/2`

* El título de página es **Instructors**.
* Se ha agregado una columna **Office** en la que se muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es NULL. Dado que se trata de una relación de uno a cero o uno, es posible que no haya una entidad OfficeAssignment relacionada.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Se ha agregado una columna **Courses** en la que se muestran los cursos que imparte cada instructor. Vea [Transición de línea explícita con `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información sobre esta sintaxis de Razor.

* Ha agregado código que agrega dinámicamente `class="success"` al elemento `tr` del instructor seleccionado. Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Se ha agregado un hipervínculo nuevo con la etiqueta **Select**. Este vínculo envía el identificador del instructor seleccionado al método `Index` y establece un color de fondo.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Ejecute la aplicación y haga clic en la pestaña **Instructors**. En la página se muestra la `Location` (oficina) de la entidad `OfficeAssignment` relacionada. Si OfficeAssignment es NULL, se muestra una celda de tabla vacía.

![Página de índice de instructores sin ninguna selección](read-related-data/_static/instructors-index-no-selection.png)

Haga clic en el vínculo **Select**. El estilo de la fila cambia.

### <a name="add-courses-taught-by-selected-instructor"></a>Agregar cursos impartidos por el instructor seleccionado

Actualice el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Agregue `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Examine la consulta actualizada:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

En la consulta anterior se agregan las entidades `Department`.

El código siguiente se ejecuta cuando se selecciona un instructor (`id != null`). El instructor seleccionado se recupera de la lista de instructores del modelo de vista. Se carga la propiedad `Courses` del modelo de vista con las entidades `Course` de la propiedad de navegación `CourseAssignments` de ese instructor.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

El método `Where` devuelve una colección. En el método `Where` anterior, solo se devuelve una entidad `Instructor`. El método `Single` convierte la colección en una sola entidad `Instructor`. La entidad `Instructor` proporciona acceso a la propiedad `CourseAssignments`. `CourseAssignments` proporciona acceso a las entidades `Course` relacionadas.

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

El método `Single` se usa en una colección cuando la colección tiene un solo elemento. El método `Single` inicia una excepción si la colección está vacía o hay más de un elemento. Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (NULL, en este caso) si la colección está vacía. El uso de `SingleOrDefault` en una colección vacía:

* Inicia una excepción (al tratar de buscar una propiedad `Courses` en una referencia nula).
* El mensaje de excepción indicará con menos claridad la causa del problema.

El código siguiente rellena la propiedad `Enrollments` del modelo de vista cuando se selecciona un curso:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Agregue el siguiente marcado al final de la página de Razor *Pages/Instructors/Index.cshtml*:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

En el marcado anterior se muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

Pruebe la aplicación. Haga clic en un vínculo **Select** en la página de instructores.

![Instructor seleccionado en la página de índice de instructores](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Mostrar datos de estudiante

En esta sección, la aplicación se actualiza para mostrar los datos de estudiante para un curso seleccionado.

Actualice la consulta en el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Actualice *Pages/Instructors/Index.cshtml*. Agregue el marcado siguiente al final del archivo:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

En el marcado anterior se muestra una lista de los estudiantes que están inscritos en el curso seleccionado.

Actualice la página y seleccione un instructor. Seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.

![Instructor y curso seleccionados en la página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Uso de Single

Se puede pasar el método `Single` en la condición `Where` en lugar de llamar al método `Where` por separado:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

El enfoque de `Single` anterior no ofrece ninguna ventaja con respecto a `Where`. Algunos desarrolladores prefieren el estilo del enfoque de `Single`.

## <a name="explicit-loading"></a>Carga explícita

En el código actual se especifica la carga diligente para `Enrollments` y `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Imagine que los usuarios rara vez querrán ver las inscripciones en un curso. En ese caso, una optimización sería cargar solamente los datos de inscripción si se solicitan. En esta sección, se actualiza `OnGetAsync` para usar la carga explícita de `Enrollments` y `Students`.

Actualice `OnGetAsync` con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

En el código anterior se quitan las llamadas al método *ThenInclude* para los datos de inscripción y estudiantes. Si se selecciona un curso, el código resaltado recupera lo siguiente:

* Las entidades `Enrollment` para el curso seleccionado.
* Las entidades `Student` para cada `Enrollment`.

Tenga en cuenta que en el código anterior `.AsNoTracking()` se convierte en comentario. Las propiedades de navegación solo se pueden cargar explícitamente para las entidades sometidas a seguimiento.

Pruebe la aplicación. Desde la perspectiva de los usuarios, la aplicación se comporta exactamente igual a la versión anterior.

En el siguiente tutorial se muestra cómo actualizar datos relacionados.

>[!div class="step-by-step"]
>[Anterior](xref:data/ef-rp/complex-data-model)
>[Siguiente](xref:data/ef-rp/update-related-data)
