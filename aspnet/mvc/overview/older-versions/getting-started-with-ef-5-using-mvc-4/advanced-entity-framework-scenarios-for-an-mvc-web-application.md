---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Escenarios de Entity Framework avanzados para una aplicación web MVC (10 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: f8f079f6d8ea663c6888456be422a2bae93a4b87
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "86163578"
---
# <a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Escenarios de Entity Framework avanzados para una aplicación web MVC (10 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En el tutorial anterior, implementó los patrones de repositorio y unidad de trabajo. En este tutorial se tratan los temas siguientes:

- Realización de consultas SQL sin procesar.
- Realización de consultas sin seguimiento.
- Examinar consultas enviadas a la base de datos.
- Trabajar con clases de proxy.
- Deshabilitar la detección automática de cambios.
- Deshabilitar la validación al guardar los cambios.
- [Errores y solución de problemas](#errors)

Para la mayoría de estos temas, trabajará con las páginas que ya ha creado. Para usar SQL sin procesar para realizar actualizaciones masivas, creará una nueva página que actualiza el número de créditos de todos los cursos en la base de datos:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Y para usar una consulta de no seguimiento, agregará una nueva lógica de validación a la página de edición del Departamento:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Realización de consultas SQL sin formato

La API de Entity Framework Code First incluye métodos que le permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones:

- Use el método `DbSet.SqlQuery` para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por el `DbSet` objeto y el contexto de la base de datos realiza un seguimiento de ellos automáticamente a menos que se desactive el seguimiento. (Vea la siguiente sección sobre el `AsNoTracking` método).
- Use el `Database.SqlQuery` método para las consultas que devuelven tipos que no son entidades. No se realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si usa este método para recuperar tipos de entidad.
- Use el [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) para los comandos que no son de consulta.

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado estrechamente a un método concreto de almacenamiento de datos. Lo consigue mediante la generación de consultas SQL y comandos, lo que también le evita tener que escribirlos usted mismo. Pero hay escenarios excepcionales en los que es necesario ejecutar consultas SQL específicas que se han creado manualmente, y estos métodos permiten controlar dichas excepciones.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques por inyección de código SQL. Una manera de hacerlo es mediante consultas parametrizadas para asegurarse de que las cadenas enviadas por una página web no se pueden interpretar como comandos SQL. En este tutorial usará las consultas con parámetros al integrar la entrada de usuario en una consulta.

### <a name="calling-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve entidades

Supongamos que desea que la `GenericRepository` clase proporcione flexibilidad adicional de filtrado y ordenación sin necesidad de crear una clase derivada con métodos adicionales. Una manera de lograrlo sería agregar un método que acepte una consulta SQL. Después, puede especificar cualquier tipo de filtrado u ordenación que desee en el controlador, como una `Where` cláusula que depende de una combinación o una subconsulta. En esta sección, verá cómo implementar este tipo de método.

Cree el `GetWithRawSql` método agregando el código siguiente a *GenericRepository.CS*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

En *CourseController.CS*, llame al método nuevo desde el `Details` método, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

En este caso, podría haber usado el `GetByID` método, pero está utilizando el `GetWithRawSql` método para comprobar que el `GetWithRawSQL` método funciona.

Ejecute la página de detalles para comprobar que funciona la consulta Select (seleccione la pestaña **Course (curso** ) y, después, los **detalles** de un curso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Llamar a una consulta que devuelve otros tipos de objetos

Anteriormente creó una cuadrícula de estadísticas de alumno de la página About que mostraba el número de alumnos para cada fecha de inscripción. El código que lo hace en *HomeController.CS* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Supongamos que desea escribir el código que recupera estos datos directamente en SQL en lugar de usar LINQ. Para ello, debe ejecutar una consulta que devuelva un valor que no sea objetos entidad, lo que significa que debe utilizar el `Database.SqlQuery` método.

En *HomeController.CS*, reemplace la instrucción LINQ en el `About` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Ejecute la página about. Muestra los mismos datos que anteriormente.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Llamar a una consulta Update

Supongamos que los administradores de Contoso University quieren realizar cambios masivos en la base de datos, como cambiar el número de créditos para cada curso. Si la universidad tiene un gran número de cursos, sería poco eficaz recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección, implementará una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y realizará el cambio mediante la ejecución de una `UPDATE` instrucción SQL. La página web tendrá el mismo aspecto que la ilustración siguiente:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

En el tutorial anterior, usó el repositorio genérico para leer y actualizar `Course` entidades en el `Course` controlador. Para esta operación de actualización masiva, debe crear un nuevo método de repositorio que no esté en el repositorio genérico. Para ello, creará una `CourseRepository` clase dedicada que deriva de la `GenericRepository` clase.

En la carpeta *Dal* , cree *CourseRepository.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

En *UnitOfWork.CS*, cambie el `Course` tipo de repositorio de `GenericRepository<Course>` a`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

En *CourseController.CS*, agregue un `UpdateCourseCredits` método:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Este método se utilizará para `HttpGet` y `HttpPost` . Cuando `HttpGet` `UpdateCourseCredits` se ejecute el método, la `multiplier` variable será NULL y la vista mostrará un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.

Cuando se haga clic en el botón **Actualizar** y se `HttpPost` ejecute el método, tendrá `multiplier` el valor especificado en el cuadro de texto. A continuación, el código llama al `UpdateCourseCredits` método Repository, que devuelve el número de filas afectadas y ese valor se almacena en el `ViewBag` objeto. Cuando la vista recibe el número de filas afectadas en el `ViewBag` objeto, muestra ese número en lugar del cuadro de texto y el botón de envío, tal y como se muestra en la siguiente ilustración:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Cree una vista en la carpeta *Views\Course* para la página actualizar créditos del curso:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

En *Views\Course\UpdateCourseCredits.cshtml*, reemplace el código de plantilla por el código siguiente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Ejecute el método `UpdateCourseCredits` seleccionando la pestaña **Courses**, después, agregue "/UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Haga clic en **Actualizar**. Verá el número de filas afectadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Haga clic en **Volver a la lista** para ver la lista de cursos con el número de créditos revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obtener más información acerca de las consultas SQL sin formato, consulte [consultas SQL sin formato](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) en el blog del equipo de Entity Framework.

## <a name="no-tracking-queries"></a>consultas de no seguimiento

Cuando un contexto de base de datos recupera filas de base de datos y crea objetos entidad que las representan, de forma predeterminada realiza un seguimiento de si las entidades en memoria están sincronizadas con lo que hay en la base de datos. Los datos en memoria actúan como una caché y se usan cuando se actualiza una entidad. Este almacenamiento en caché suele ser necesario en una aplicación web porque las instancias de contexto normalmente son de corta duración (para cada solicitud se crea una y se elimina) y el contexto que lee una entidad normalmente se elimina antes de volver a usar esa entidad.

Puede especificar si el contexto realiza un seguimiento de los objetos de entidad de una consulta mediante el `AsNoTracking` método. Los siguientes son escenarios típicos en los que es posible que quiera hacer esto:

- La consulta recupera un gran volumen de datos que la desactivación del seguimiento puede mejorar notablemente el rendimiento.
- Quiere adjuntar una entidad para actualizarla, pero antes recuperó la misma entidad para un propósito diferente. Como el contexto de base de datos ya está realizando el seguimiento de la entidad, no se puede adjuntar la entidad que se quiere cambiar. Una manera de evitar que esto suceda es usar la `AsNoTracking` opción con la consulta anterior.

En esta sección se implementará la lógica de negocios que ilustra el segundo de estos escenarios. En concreto, se aplicará una regla de negocios que indica que un instructor no puede ser el administrador de más de un departamento.

En *DepartmentController.CS*, agregue un nuevo método al que pueda llamar desde los `Edit` métodos y para asegurarse de `Create` que dos departamentos no tienen el mismo administrador:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Agregue código en el `try` bloque del `HttpPost` `Edit` método para llamar a este nuevo método si no hay ningún error de validación. El `try` bloque ahora es similar al ejemplo siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Ejecute la página de edición del Departamento e intente cambiar el administrador de un departamento a un instructor que ya sea el administrador de otro departamento. Obtiene el mensaje de error esperado:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ahora vuelva a ejecutar la página de edición del Departamento y esta vez cambie la cantidad del **presupuesto** . Al hacer clic en **Guardar**, verá una página de error:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

El mensaje de error de excepción es " `An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.` " Esto ocurrió debido a la siguiente secuencia de eventos:

- El `Edit` método llama al `ValidateOneAdministratorAssignmentPerInstructor` método, que recupera todos los departamentos que tienen Kim Alberti como administrador. Esto hace que se lea el Departamento de inglés. Dado que ese es el Departamento que se está editando, no se genera ningún error. Como resultado de esta operación de lectura, sin embargo, el contexto de la base de datos está realizando el seguimiento de la entidad del Departamento inglés leída en la base de datos.
- El `Edit` método intenta establecer la `Modified` marca en la entidad del Departamento inglés creada por el enlazador de modelos de MVC, pero se produce un error porque el contexto ya está realizando el seguimiento de una entidad para el Departamento de inglés.

Una solución a este problema es evitar que el contexto realice el seguimiento de las entidades del Departamento en memoria recuperadas por la consulta de validación. Esto no supone ningún inconveniente, ya que no se va a actualizar esta entidad ni a leerla de nuevo de manera que se beneficie de que se almacene en la memoria caché.

En *DepartmentController.CS*, en el `ValidateOneAdministratorAssignmentPerInstructor` método, no especifique ningún seguimiento, como se muestra a continuación:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Repita el intento de editar la cantidad de **presupuesto** de un departamento. Esta vez, la operación se realiza correctamente y el sitio vuelve según lo previsto en la página de índice de los departamentos, mostrando el valor de presupuesto revisado.

## <a name="examining-queries-sent-to-the-database"></a>Examinar consultas enviadas a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. Para ello, puede examinar una variable de consulta en el depurador o llamar al método de la consulta `ToString` . Para probarlo, verá una consulta simple y, a continuación, examinará lo que le sucede a medida que agrega opciones como la carga diligente, el filtrado y la ordenación.

En *Controllers/CourseController*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Ahora establezca un punto de interrupción en *GenericRepository.CS* en las `return query.ToList();` `return orderBy(query).ToList();` instrucciones y del `Get` método. Ejecute el proyecto en modo de depuración y seleccione la página de índice del curso. Cuando el código alcance el punto de interrupción, examine la `query` variable. Verá la consulta que se envía a SQL Server. Se trata de una `Select` instrucción sencilla:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Las consultas pueden ser demasiado largas para mostrarlas en las ventanas de depuración de Visual Studio. Para ver toda la consulta, puede copiar el valor de la variable y pegarlo en un editor de texto:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Ahora agregará una lista desplegable a la página de índice del curso para que los usuarios puedan filtrar por un departamento determinado. Ordenará los cursos por título y especificará la carga diligente para la propiedad de `Department` navegación. En *CourseController.CS*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

El método recibe el valor seleccionado de la lista desplegable en el `SelectedDepartment` parámetro. Si no se selecciona nada, este parámetro será null.

Una `SelectList` colección que contiene todos los departamentos se pasa a la vista de la lista desplegable. Los parámetros que se pasan al `SelectList` constructor especifican el nombre del campo de valor, el nombre del campo de texto y el elemento seleccionado.

En el caso del `Get` método del `Course` repositorio, el código especifica una expresión de filtro, un criterio de ordenación y la carga diligente para la `Department` propiedad de navegación. La expresión de filtro siempre devuelve `true` si no hay nada seleccionado en la lista desplegable (es decir, `SelectedDepartment` es null).

En *Views\Course\Index.cshtml*, inmediatamente antes de la `table` etiqueta de apertura, agregue el código siguiente para crear la lista desplegable y un botón de envío:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Con los puntos de interrupción todavía establecidos en la `GenericRepository` clase, ejecute la página de índice del curso. Continúe con las dos primeras veces que el código llega a un punto de interrupción, de modo que la página se muestre en el explorador. Seleccione un departamento en la lista desplegable y haga clic en **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Esta vez, el primer punto de interrupción será para la consulta de departamentos de la lista desplegable. Omitir y ver la `query` variable la próxima vez que el código alcance el punto de interrupción para ver el `Course` aspecto de la consulta. Verá algo parecido a lo siguiente:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Puede ver que la consulta es ahora una `JOIN` consulta que carga `Department` datos junto con los `Course` datos y que incluye una `WHERE` cláusula.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Trabajar con clases de proxy

Cuando el Entity Framework crea instancias de entidad (por ejemplo, al ejecutar una consulta), suele crearlas como instancias de un tipo derivado generado dinámicamente que actúa como un proxy para la entidad. Este proxy invalida algunas propiedades virtuales de la entidad para insertar enlaces para realizar acciones automáticamente cuando se tiene acceso a la propiedad. Por ejemplo, este mecanismo se usa para admitir la carga diferida de relaciones.

La mayoría de las veces no es necesario tener en cuenta este uso de servidores proxy, pero hay excepciones:

- En algunos escenarios podría querer evitar que el Entity Framework Cree instancias de proxy. Por ejemplo, la serialización de instancias que no son de proxy podría ser más eficaz que serializar las instancias de proxy.
- Cuando se crean instancias de una clase de entidad mediante el `new` operador, no se obtiene una instancia de proxy. Esto significa que no obtendrá funcionalidad como la carga diferida y el seguimiento automático de cambios. Normalmente está bien. por lo general, no se necesita la carga diferida, ya que se está creando una nueva entidad que no está en la base de datos y, por lo general, no se necesita seguimiento de cambios si se marca explícitamente la entidad como `Added` . Sin embargo, si necesita una carga diferida y necesita seguimiento de cambios, puede crear nuevas instancias de entidad con servidores proxy mediante el `Create` método de la `DbSet` clase.
- Es posible que desee obtener un tipo de entidad real a partir de un tipo de proxy. Puede usar el `GetObjectType` método de la `ObjectContext` clase para obtener el tipo de entidad real de una instancia de tipo de proxy.

Para obtener más información, vea [trabajar con servidores proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) en el blog del equipo de Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Deshabilitación de la detección automática de cambios

Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que hay que enviar a la base de datos) comparando los valores actuales de una entidad con los valores originales. Los valores originales se almacenan cuando se consulta o adjunta la entidad. Algunos de los métodos que provocan la detección de cambios automática son los siguientes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si está realizando el seguimiento de un gran número de entidades y llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas desactivando temporalmente la detección de cambios automática mediante la propiedad [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) . Para obtener más información, consulte [detección automática de cambios](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Deshabilitar la validación al guardar cambios

Cuando se llama al `SaveChanges` método, de forma predeterminada, el Entity Framework valida los datos de todas las propiedades de todas las entidades modificadas antes de actualizar la base de datos. Si ha actualizado un gran número de entidades y ya ha validado los datos, este trabajo no es necesario y puede hacer que el proceso de guardar los cambios tarde menos tiempo en desactivar la validación de forma temporal. Puede hacerlo mediante la propiedad [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) . Para obtener más información, consulte [validación](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Resumen

Con esto se completa esta serie de tutoriales sobre el uso de la Entity Framework en una aplicación ASP.NET MVC. Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obtener más información sobre cómo implementar la aplicación web después de haberla creado, vea [mapa de contenido de implementación de ASP.net](https://msdn.microsoft.com/library/bb386521.aspx) en MSDN Library.

Para obtener información sobre otros temas relacionados con MVC, como la autenticación y la autorización, vea los [recursos recomendados de MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Agradecimientos

- Tom Dykstra escribió la versión original de este tutorial y es un redactor de programación Senior en el equipo de contenido de herramientas y plataforma web de Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) ha creado conjuntamente este tutorial y ha realizado la mayor parte del trabajo que lo ha actualizado para EF 5 y MVC 4. Rick es un redactor de programación Senior para Microsoft que se centra en Azure y MVC.
- [Rowan Miller](http://www.romiller.com) y otros miembros del equipo de Entity Framework asistieron a las revisiones de código y ayudó a depurar muchos problemas con las migraciones que surgieron mientras actualizamos el tutorial de EF 5.

## <a name="vb"></a>VB

Cuando se generó originalmente el tutorial, se proporcionan las versiones C# y VB del proyecto de descarga completado. Con esta actualización, se proporciona un proyecto descargable de C# para cada capítulo para que sea más fácil empezar a trabajar en cualquier parte de la serie, pero debido a las limitaciones de tiempo y otras prioridades que no hacemos para VB. Si compila un proyecto de VB con estos tutoriales y desea compartirlo con otros, háganoslo saber.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Errores y soluciones alternativas

### <a name="cannot-createshadow-copy"></a>No se puede crear o copiar instantáneas

Mensaje de error:

*No se puede crear o instantánea ' DotNetOpenAuth. OpenId ' cuando ese archivo ya existe.*

Solución:

Espere unos segundos y actualice la página.

### <a name="update-database-not-recognized"></a>Update: no se reconoce la base de datos

Mensaje de error:

*El término ' Update-Database ' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o bien, si se incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta y vuelva a intentarlo.* (Desde el *`Update-Database`* comando en la PMC).

Solución:

Salga de Visual Studio. Vuelva a abrir el proyecto e inténtelo de nuevo.

### <a name="validation-failed"></a>Error de validación

Mensaje de error:

*Error de validación de una o más entidades. Vea la propiedad ' EntityValidationErrors ' para obtener más detalles.* (Desde el *`Update-Database`* comando en la PMC).

Solución:

Una causa de este problema son los errores de validación cuando `Seed` se ejecuta el método. Vea [bases de los Entity Framework de propagación y depuración (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obtener sugerencias sobre cómo depurar el `Seed` método.

### <a name="http-50019-error"></a>Error HTTP 500,19

Mensaje de error:

*Error HTTP 500,19-error interno del servidor no se puede tener acceso a la página solicitada porque los datos de configuración relacionados para la página no son válidos.*

Solución:

Una manera de obtener este error es que se pueden obtener varias copias de la solución, cada una de ellas con el mismo número de puerto. Normalmente puede resolver este problema saliendo de todas las instancias de Visual Studio y, a continuación, reinicie el proyecto en el que está trabajando. Si eso no funciona, pruebe a cambiar el número de puerto. Haga clic con el botón derecho en el archivo de proyecto y después haga clic en propiedades. Seleccione la pestaña **Web** y, a continuación, cambie el número de puerto en el cuadro de texto **dirección URL del proyecto** .

### <a name="error-locating-sql-server-instance"></a>Error al buscar la instancia de SQL Server

Mensaje de error:

*Se produjo un error relacionado con la red o específico de la instancia al establecer una conexión con SQL Server. No se encontró el servidor o no se pudo obtener acceso a él. Compruebe que el nombre de la instancia es correcto y que SQL Server está configurado para permitir conexiones remotas. (proveedor: interfaces de red de SQL, error: 26-error al buscar el servidor/instancia especificado)*

Solución:

Compruebe la cadena de conexión. Si ha eliminado manualmente la base de datos, cambie el nombre de la base de datos en la cadena de construcción.

> [!div class="step-by-step"]
> [Anterior](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Siguiente](building-the-ef5-mvc4-chapter-downloads.md)
