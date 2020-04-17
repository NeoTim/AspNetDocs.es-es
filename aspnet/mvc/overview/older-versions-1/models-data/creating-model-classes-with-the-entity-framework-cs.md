---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Creación de clases de modelo con Entity Framework (C-) Microsoft Docs
author: rick-anderson
description: En este tutorial, aprenderá a usar ASP.NET MVC con Microsoft Entity Framework. Aprenderá a usar el Asistente para entidades para crear una ADO.NET Entity Da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 716d34167258b1005b25b1cd11bfaa6d80763320
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542669"
---
# <a name="creating-model-classes-with-the-entity-framework-c"></a>Crear clases de modelo con Entity Framework (C#)

por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC con Microsoft Entity Framework. Aprenderá a usar el Asistente para entidades para crear una ADO.NET Entity Data Model. En el transcurso de este tutorial, creamos una aplicación web que ilustra cómo seleccionar, insertar, actualizar y eliminar datos de base de datos mediante Entity Framework.

El objetivo de este tutorial es explicar cómo puede crear clases de acceso a datos mediante Microsoft Entity Framework al compilar una aplicación ASP.NET MVC. En este tutorial no se supone ningún conocimiento previo de Microsoft Entity Framework. Al final de este tutorial, comprenderá cómo usar Entity Framework para seleccionar, insertar, actualizar y eliminar registros de base de datos.

Microsoft Entity Framework es una herramienta de asignación relacional de objetos (O/RM) que le permite generar automáticamente una capa de acceso a datos a partir de una base de datos. Entity Framework le permite evitar el tedioso trabajo de crear las clases de acceso a datos a mano.

Para ilustrar cómo puede usar Microsoft Entity Framework con ASP.NET MVC, crearemos una aplicación de ejemplo sencilla. Crearemos una aplicación de base de datos de películas que le permite mostrar y editar registros de base de datos de películas.

En este tutorial se supone que tiene Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. Necesita Service Pack 1 para utilizar Entity Framework. Puede descargar Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 desde la siguiente dirección:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

> [!NOTE] 
> 
> No hay ninguna conexión esencial entre ASP.NET MVC y Microsoft Entity Framework. Hay varias alternativas a Entity Framework que puede usar con ASP.NET MVC. Por ejemplo, puede crear las clases de modelo MVC con otras herramientas de O/RM como Microsoft LINQ to SQL, NHibernate o SubSonic.

## <a name="creating-the-movie-sample-database"></a>Creación de la base de datos de ejemplo de película

La aplicación Movie Database utiliza una tabla de base de datos denominada Movies que contiene las siguientes columnas:

| Nombre de columna | Tipo de datos | ¿Permitir Valores NULL? | ¿Es la clave principal? |
| --- | --- | --- | --- |
| Identificador | int | False | True |
| Título | nvarchar(100) | False | False |
| Director | nvarchar(100) | False | False |

Puede agregar esta tabla a un proyecto de ASP.NET MVC siguiendo estos pasos:

1. Haga clic con\_el botón derecho en la carpeta Datos de la aplicación en la ventana Explorador de soluciones y seleccione la opción de menú **Agregar, Nuevo elemento.**
2. En el cuadro de diálogo **Agregar nuevo elemento,** seleccione Base de datos de **SQL Server**, asigne a la base de datos el nombre MoviesDB.mdf y haga clic en el botón **Agregar.**
3. Haga doble clic en el archivo MoviesDB.mdf para abrir la ventana Explorador de servidores/Explorador de bases de datos.
4. Expanda la conexión de base de datos MoviesDB.mdf, haga clic con el botón derecho en la carpeta Tablas y seleccione la opción de menú **Agregar nueva tabla**.
5. En el Diseñador de tablas, agregue las columnas Id, Title y Director.
6. Haga clic en el botón **Guardar** (tiene el icono del disquete) para guardar la nueva tabla con el nombre Películas.

Después de crear la tabla de base de datos Movies, debe agregar algunos datos de ejemplo a la tabla. Haga clic con el botón derecho en la tabla Películas y seleccione la opción de menú **Mostrar datos**de tabla . Puede introducir datos de película falsos en la cuadrícula que aparece.

## <a name="creating-the-adonet-entity-data-model"></a>Creación del modelo de datos de entidad ADO.NET

Para usar Entity Framework, debe crear un Entity Data Model. Puede aprovechar el Asistente *para Visual* Studio Entity Data Model para generar un Entity Data Model a partir de una base de datos automáticamente.

Siga estos pasos:

1. Haga clic con el botón derecho en la carpeta Modelos de la ventana Explorador de soluciones y seleccione la opción de menú **Agregar, Nuevo elemento**.
2. En el cuadro de diálogo **Agregar nuevo elemento,** seleccione la categoría Datos (consulte la figura 1).
3. Seleccione la ADO.NET Entity **Data Model** plantilla, asigne al Entity Data Model el nombre MoviesDBModel.edmx y haga clic en el botón **Agregar.** Al hacer clic en el botón **Agregar** se inicia el Asistente para modelos de datos.
4. En el paso Elegir contenido del **modelo,** elija la opción **Generar a partir de una base** de datos y haga clic en el botón **Siguiente** (consulte la figura 2).
5. En el paso Elegir la conexión de **datos,** seleccione la conexión de base de datos MoviesDB.mdf, escriba el nombre de configuración de conexiones de entidades MoviesDBEntities y haga clic en el botón **Siguiente** (consulte la figura 3).
6. En el paso Elegir los objetos de base de **datos,** seleccione la tabla Base de datos de películas y haga clic en el botón **Finalizar** (consulte la figura 4).

Después de completar estos pasos, se abre el diseñador de ADO.NET Entity Data Model Designer (Entity Designer).

**Figura 1 – Creación de un nuevo Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Figura 2 – Elija el paso de contenido del modelo**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Figura 3 – Elija su conexión de datos**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Figura 4 – Elija los objetos de la base de datos**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modificación del modelo de datos de entidad de ADO.NET

Después de crear un Entity Data Model, puede modificar el modelo aprovechando Entity Designer (consulte la figura 5). Puede abrir Entity Designer en cualquier momento haciendo doble clic en el archivo MoviesDBModel.edmx contenido en la carpeta Modelos dentro de la ventana Explorador de soluciones.

**Figura 5: el Diseñador de ADO.NET Entity Data Model**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Por ejemplo, puede usar Entity Designer para cambiar los nombres de las clases que genera el Asistente para datos de Entity Model. El asistente creó una nueva clase de acceso a datos denominada Movies. En otras palabras, el asistente dio a la clase el mismo nombre que la tabla de base de datos. Dado que usaremos esta clase para representar una instancia de Movie determinada, debemos cambiar el nombre de la clase de Películas a Película.

Si desea cambiar el nombre de una clase de entidad, puede hacer doble clic en el nombre de clase en Entity Designer y escribir un nuevo nombre (consulte la figura 6). Como alternativa, puede cambiar el nombre de una entidad en la ventana Propiedades después de seleccionar una entidad en Entity Designer.

**Figura 6 – Cambio del nombre de una entidad**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Recuerde guardar el Entity Data Model después de realizar una modificación haciendo clic en el botón Guardar (el icono del disquete). En segundo plano, Entity Designer genera un conjunto de clases de C. Puede ver estas clases abriendo el archivo MoviesDBModel.Designer.cs desde la ventana Explorador de soluciones.

No modifique el código del archivo Designer.cs ya que los cambios se sobrescribirán la próxima vez que use Entity Designer. Si desea ampliar la funcionalidad de las clases de entidad definidas en el archivo Designer.cs, puede crear *clases parciales* en archivos independientes.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Selección de registros de base de datos con Entity Framework

Comencemos a crear nuestra aplicación Movie Database creando una página que muestre una lista de registros de películas. El controlador Home del listado 1 expone una acción denominada Index(). La acción Index() devuelve todos los registros de película de la tabla de base de datos Movie aprovechando Entity Framework.

**Listado 1 – Controllers-HomeController.cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Observe que el controlador en el listado 1 incluye un constructor. El constructor inicializa un campo \_de nivel de clase denominado db. El \_campo db representa las entidades de base de datos generadas por Microsoft Entity Framework. El \_campo db es una instancia de la clase MoviesDBEntities generada por Entity Designer.

Para usar la claseMoviesDBEntities en el controlador Home, debe importar el espacio de nombres MovieEntityApp.Models (*MVCProjectName*. Modelos).

El \_campo db se utiliza dentro de la acción Index() para recuperar los registros de la tabla de base de datos Movies. La \_expresión db. MovieSet representa todos los registros de la tabla de base de datos Movies. El método ToList() se utiliza para convertir el conjunto de&lt;películas en una colección genérica de objetos Movie (List Movie&gt;).

Los registros de película se recuperan con la ayuda de LINQ to Entities. La acción Index() del listado 1 utiliza la *sintaxis* del método LINQ para recuperar el conjunto de registros de base de datos. Si lo prefiere, puede usar la *sintaxis* de consulta LINQ en su lugar. Las dos sentencias siguientes hacen lo mismo:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Use la sintaxis LINQ (sintaxis de método o sintaxis de consulta) que encuentre más intuitiva. No hay diferencia en el rendimiento entre los dos enfoques: la única diferencia es el estilo.

La vista del listado 2 se utiliza para mostrar los registros de película.

**Listado 2 – Vistas-Home-Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

La vista del listado 2 contiene un bucle **foreach** que recorre en iteración cada registro de película y muestra los valores de las propiedades Title y Director del registro de película. Observe que se muestra un vínculo Editar y eliminar junto a cada registro. Además, aparece un vínculo Agregar película en la parte inferior de la vista (consulte la figura 7).

**Figura 7 – La vista de índice**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

La vista de índice es una *vista con tipo.* La vista de &lt;índice incluye&gt; una directiva % , % de página % con un *atributo Inherits* que convierte la propiedad Model en una colección List genérica fuertemente tipada de objetos Movie (List&lt;Movie).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insertar registros de base de datos con Entity Framework

Puede usar Entity Framework para facilitar la inserción de nuevos registros en una tabla de base de datos. El listado 3 contiene dos nuevas acciones agregadas a la clase de controlador Home que puede usar para insertar nuevos registros en la tabla de base de datos Movie.

**Listado 3 – Controllers-HomeController.cs (Add methods)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

La primera acción Add() simplemente devuelve una vista. La vista contiene un formulario para agregar un nuevo registro de base de datos de películas (consulte la figura 8). Al enviar el formulario, se invoca la segunda acción Add().

Observe que la segunda acción Add() está decorada con el atributo AcceptVerbs. Esta acción solo se puede invocar al realizar una operación HTTP POST. En otras palabras, esta acción solo se puede invocar al publicar un formulario HTML.

La segunda acción Add() crea una nueva instancia de la clase Entity Framework Movie con la ayuda del método TryUpdateModel() de ASP.NET MVC. El método TryUpdateModel() toma los campos de FormCollection pasados al método Add() y asigna los valores de estos campos de formulario HTML a la clase Movie.

Al usar Entity Framework, debe proporcionar una "lista blanca" de propiedades al usar los métodos TryUpdateModel o UpdateModel para actualizar las propiedades de una clase de entidad.

A continuación, la acción Add() realiza una validación de formulario sencilla. La acción comprueba que las propiedades Title y Director tienen valores. Si hay un error de validación, se agrega un mensaje de error de validación a ModelState.

Si no hay errores de validación, se agrega un nuevo registro de película a la tabla de base de datos Movies con la ayuda de Entity Framework. El nuevo registro se agrega a la base de datos con las dos líneas de código siguientes:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

La primera línea de código agrega la nueva Movie entidad al conjunto de películas que realiza el seguimiento de Entity Framework. La segunda línea de código guarda los cambios realizados en el Movies que se realiza el seguimiento en la base de datos subyacente.

**Figura 8 – La vista Agregar**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Actualización de registros de base de datos con Entity Framework

Puede seguir casi el mismo enfoque para editar un registro de base de datos con Entity Framework como el enfoque que acabamos de seguir para insertar un nuevo registro de base de datos. El listado 4 contiene dos nuevas acciones de controlador denominadas Edit(). La primera acción Edit() devuelve un formulario HTML para editar un registro de película. La segunda acción Edit() intenta actualizar la base de datos.

**Listado 4 – Controllers-HomeController.cs (Métodos de edición)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

La segunda acción Edit() comienza recuperando el registro Movie de la base de datos que coincide con el identificador de la película que se está editando. La siguiente instrucción LINQ to Entities captura el primer registro de base de datos que coincide con un identificador determinado:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

A continuación, se utiliza el método TryUpdateModel() para asignar los valores de los campos de formulario HTML a las propiedades de la entidad de película. Observe que se proporciona una lista blanca para especificar las propiedades exactas que se va a actualizar.

A continuación, se realiza una validación sencilla para comprobar que las propiedades Título de película y Director tienen valores. Si a cualquiera de las propiedades le falta un valor, se agrega un mensaje de error de validación a ModelState y ModelState.IsValid devuelve el valor false.

Por último, si no hay errores de validación, la tabla de base de datos Movies subyacente se actualiza con cualquier cambio llamando al método SaveChanges().

Al editar registros de base de datos, debe pasar el identificador del registro que se está editando a la acción del controlador que realiza la actualización de la base de datos. De lo contrario, la acción del controlador no sabrá qué registro actualizar en la base de datos subyacente. La vista Editar, contenida en el listado 5, incluye un campo de formulario oculto que representa el identificador del registro de base de datos que se está editando.

**Listado 5 – Vistas-Home-Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminación de registros de base de datos con Entity Framework

La operación final de la base de datos, que necesitamos abordar en este tutorial, es eliminar los registros de base de datos. Puede utilizar la acción de controlador en el listado 6 para eliminar un registro de base de datos determinado.

**Listado 6 -- "Controllers" (eliminar acción)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

La acción Delete() recupera primero la entidad Movie que coincide con el identificador pasado a la acción. A continuación, la película se elimina de la base de datos llamando al método DeleteObject() seguido del método SaveChanges(). Por último, se redirige al usuario a la vista de índice.

## <a name="summary"></a>Resumen

El propósito de este tutorial era demostrar cómo puede crear aplicaciones web controladas por base de datos aprovechando ASP.NET MVC y Microsoft Entity Framework. Ha aprendido a crear una aplicación que le permite seleccionar, insertar, actualizar y eliminar registros de base de datos.

En primer lugar, se describe cómo puede usar el Asistente para Entity Data Model para generar un Entity Data Model desde Visual Studio. A continuación, aprenderá a usar LINQ to Entities para recuperar un conjunto de registros de base de datos de una tabla de base de datos. Por último, usamos Entity Framework para insertar, actualizar y eliminar registros de base de datos.

> [!div class="step-by-step"]
> [Siguiente](creating-model-classes-with-linq-to-sql-cs.md)
