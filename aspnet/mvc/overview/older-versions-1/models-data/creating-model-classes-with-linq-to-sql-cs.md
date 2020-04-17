---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Creación de clases de modelo con LINQ to SQLLINQ to SQL (C-) Microsoft Docs
author: rick-anderson
description: El objetivo de este tutorial es explicar un método de creación de clases de modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a construir el modelo c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: eac6fec3a4cfd833b810378d946874a246d9a2c3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542734"
---
# <a name="creating-model-classes-with-linq-to-sql-c"></a>Crear clases de modelo con LINQ to SQL (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> El objetivo de este tutorial es explicar un método de creación de clases de modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a crear clases de modelo y a realizar el acceso a la base de datos aprovechando Microsoft LINQ to SQL.

El objetivo de este tutorial es explicar un método de creación de clases de modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a crear clases de modelo y a realizar el acceso a la base de datos aprovechando Microsoft LINQ to SQL

En este tutorial, creamos una aplicación básica de base de datos Movie. Comenzamos creando la aplicación de base de datos Movie de la forma más rápida y sencilla posible. Realizamos todos nuestros datos de acceso directamente desde nuestras acciones de controlador.

A continuación, aprenderá a utilizar el patrón Derepositorio. El uso del patrón Derepositorio requiere un poco más de trabajo. Sin embargo, la ventaja de adoptar este patrón es que le permite crear aplicaciones que son adaptables al cambio y se pueden probar fácilmente.

## <a name="what-is-a-model-class"></a>¿Qué es una clase de modelo?

Un modelo MVC contiene toda la lógica de aplicación que no está contenida en una vista MVC o controlador MVC. En particular, un modelo MVC contiene toda la lógica de acceso a datos y empresarial de la aplicación.

Puede usar una variedad de tecnologías diferentes para implementar la lógica de acceso a datos. Por ejemplo, puede crear las clases de acceso a datos mediante las clases Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

En este tutorial, uso LINQ to SQLLINQ to SQL para consultar y actualizar la base de datos. LINQ to SQLLINQ to SQL proporciona un método muy sencillo para interactuar con una base de datos de Microsoft SQL Server. Sin embargo, es importante comprender que el marco de ASP.NET MVC no está vinculado a LINQ to SQLLINQ to SQL de ninguna manera. ASP.NET MVC es compatible con cualquier tecnología de acceso a datos.

## <a name="create-a-movie-database"></a>Crear una base de datos de películas

En este tutorial, para ilustrar cómo puede crear clases de modelo, creamos una aplicación de base de datos Movie simple. El primer paso es crear una nueva base de datos. Haga clic con\_el botón derecho en la carpeta Datos de la aplicación en la ventana Explorador de soluciones y seleccione la opción de menú **Agregar, Nuevo elemento**. Seleccione la plantilla de base de datos de **SQL Server,** asíbórse el nombre MoviesDB.mdf y haga clic en el botón **Agregar** (consulte la figura 1).

[![Agregar una nueva base de datos de SQL Server](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Figura 01**: Adición de una nueva base de datos de SQL Server[(Haga clic para ver la imagen de tamaño completo)](creating-model-classes-with-linq-to-sql-cs/_static/image3.png)

Después de crear la nueva base de datos, puede abrir la base de\_datos haciendo doble clic en el archivo MoviesDB.mdf en la carpeta Datos de la aplicación. Al hacer doble clic en el archivo MoviesDB.mdf se abre la ventana Explorador de servidores (consulte la figura 2).

La ventana Explorador de servidores se denomina ventana Explorador de bases de datos cuando se usa Visual Web Developer.

[![Uso de la ventana Explorador de servidores](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Figura 02**: Uso de la ventana Explorador de servidores[(Haga clic para ver la imagen de tamaño completo)](creating-model-classes-with-linq-to-sql-cs/_static/image6.png)

Necesitamos agregar una tabla a nuestra base de datos que represente nuestras películas. Haga clic con el botón derecho en la carpeta Tablas y seleccione la opción de menú **Agregar nueva tabla**. Al seleccionar esta opción de menú se abre el Diseñador de tablas (consulte la figura 3).

[![Uso de la ventana Explorador de servidores](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Figura 03**: El Diseñador de tablas ([Haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))

Necesitamos agregar las siguientes columnas a nuestra tabla de base de datos:

| **Nombre de columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Identificador | Int | False |
| Título | Nvarchar(200) | False |
| Director | Nvarchar(50) | False |

Debe hacer dos cosas especiales a la columna Id. En primer lugar, debe marcar la columna Id como una columna de clave principal seleccionando la columna en el Diseñador de tablas y haciendo clic en el icono de una clave. LINQ to SQLLINQ to SQL requiere que especifique las columnas de clave principal al realizar inserciones o actualizaciones en la base de datos.

A continuación, debe marcar la columna Id como una columna Identity asignando el valor Sí a la propiedad **Is Identity** (consulte la figura 3). Una columna Identity es una columna a la que se asigna un nuevo número automáticamente cada vez que se agrega una nueva fila de datos a una tabla.

## <a name="create-linq-to-sql-classes"></a>Crear clases LINQ to SQLLINQ to SQL

Nuestro modelo MVC contendrá LINQ to SQLLINQ to SQL clases que representan la tabla de base de datos tblMovie. La forma más fácil de crear estas clases LINQ to SQLLINQ to SQL es hacer clic con el botón secundario en la carpeta Modelos, seleccionar **Agregar, Nuevo elemento**, seleccionar la plantilla LINQ to SQL Classes, asignar a las clases el nombre Movie.dbml y hacer clic en el botón **Agregar** (consulte la figura 4).

[![Creación de clases LINQ to SQLLINQ to SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Figura 04**: Creación de clases LINQ to SQLLINQ to SQL ( Haga clic para ver la[imagen de tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))

Inmediatamente después de crear las clases Movie LINQ to SQL, aparece object Relational Designer. Puede arrastrar tablas de base de datos desde la ventana Explorador de servidores hasta el Diseñador relacional de objetos para crear LINQ to SQL Classes que representen determinadas tablas de base de datos. Necesitamos agregar la tabla de base de datos tblMovie al Diseñador relacional de objetos (consulte la figura 5).

[![Uso del Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Figura 05**: Uso del Diseñador relacional de objetos ( Haga clic para ver la[imagen de tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))

De forma predeterminada, Object Relational Designer crea una clase con el mismo nombre que la tabla de base de datos que se arrastra al diseñador. Sin embargo, no queremos `tblMovie`llamar a nuestra clase. Por lo tanto, haga clic en el nombre de la clase en designer y cambie el nombre de la clase a Movie.

Por último, recuerde hacer clic en el botón **Guardar** (la imagen del disquete) para guardar las clases LINQ to SQLLINQ to SQL. De lo contrario, el Diseñador relacional de objetos no generará las clases LINQ to SQLLINQ to SQL.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Uso de LINQ to SQLLINQ to SQL en una acción de controlador

Ahora que tenemos nuestras clases LINQ to SQLLINQ to SQL, podemos usar estas clases para recuperar datos de la base de datos. En esta sección, aprenderá a usar LINQ to SQLLINQ to SQL clases directamente dentro de una acción de controlador. Mostraremos la lista de películas de la tabla de base de datos tblMovies en una vista MVC.

En primer lugar, necesitamos modificar la clase HomeController. Esta clase se puede encontrar en la carpeta Controllers de la aplicación. Modifique la clase para que se parezca a la clase en el listado 1.

**Listado 1 –`Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

La `Index()` acción del listado 1 usa una clase `MovieDataContext`LINQ to `MoviesDB` SQL DataContext (la ) para representar la base de datos. La `MoveDataContext` clase la generó el Diseñador relacional de objetos de Visual Studio.

Se realiza una consulta LINQ en DataContext para `tblMovies` recuperar todas las películas de la tabla de base de datos. La lista de películas se `movies`asigna a una variable local denominada . Por último, la lista de películas se pasa a la vista a través de los datos de vista.

Para mostrar las películas, a continuación necesitamos modificar la vista de índice. Puede encontrar la vista `Views\Home\` de índice en la carpeta. Actualice la vista de índice para que se parezca a la vista del listado 2.

**Listado 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Observe que la vista `<%@ import namespace %>` de índice modificada incluye una directiva en la parte superior de la vista. Esta directiva `MvcApplication1.Models namespace`importa el archivo . Necesitamos este espacio de nombres `model` para trabajar con `Movie` las clases, en particular, la clase, en la vista.

La vista en el `foreach` listado 2 contiene un bucle que recorre `ViewData.Model` en iteración todos los elementos representados por la propiedad. El valor `Title` de la propiedad `movie`se muestra para cada archivo .

Observe que el `ViewData.Model` valor de la `IEnumerable`propiedad se convierte en un archivo . Esto es necesario para recorrer el `ViewData.Model`contenido de . Otra opción aquí es crear un `view`archivo . fuertemente tipado. Cuando se `view`crea un tipo fuerte, `ViewData.Model` se convierte la propiedad en un tipo determinado en la clase de código subyacente de una vista.

Si ejecuta la aplicación `HomeController` después de modificar la clase y la vista de índice, obtendrá una página en blanco. Obtendrá una página en blanco porque no hay `tblMovies` registros de película en la tabla de base de datos.

Para agregar registros `tblMovies` a la tabla de `tblMovies` base de datos, haga clic con el botón derecho en la tabla de base de datos en la ventana Explorador de servidores (ventana Explorador de bases de datos en Visual Web Developer) y seleccione la opción de menú Mostrar datos de tabla. Puede insertar `movie` registros mediante la cuadrícula que aparece (consulte la figura 6).

[![Inserción de películas](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Figura 06**: Insertar películas[(Haga clic para ver la imagen de tamaño completo)](creating-model-classes-with-linq-to-sql-cs/_static/image18.png)

Después de agregar algunos `tblMovies` registros de base de datos a la tabla y ejecutar la aplicación, verá la página en la figura 7. Todos los registros de base de datos de películas se muestran en una lista con viñetas.

[![Visualización de películas con la vista de índice](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Figura 07**: Visualización de películas con la vista de índice[(Haga clic para ver la imagen de tamaño completo)](creating-model-classes-with-linq-to-sql-cs/_static/image21.png)

## <a name="using-the-repository-pattern"></a>Uso del patrón de repositorio

En la sección anterior, usamos LINQ to SQLLINQ to SQL clases directamente dentro de una acción de controlador. Usamos `MovieDataContext` la clase `Index()` directamente desde la acción del controlador. No hay nada de malo en hacer esto en el caso de una aplicación simple. Sin embargo, trabajar directamente con LINQ to SQLLINQ to SQL en una clase de controlador crea problemas cuando necesita crear una aplicación más compleja.

El uso de LINQ to SQLLINQ to SQL dentro de una clase de controlador hace que sea difícil cambiar las tecnologías de acceso a datos en el futuro. Por ejemplo, puede decidir cambiar de usar Microsoft LINQ to SQL a usar Microsoft Entity Framework como tecnología de acceso a datos. En ese caso, deberá volver a escribir todos los controladores que tienen acceso a la base de datos dentro de la aplicación.

El uso de LINQ to SQLLINQ to SQL dentro de una clase de controlador también dificulta la compilación de pruebas unitarias para la aplicación. Normalmente, no desea interactuar con una base de datos al realizar pruebas unitarias. Desea utilizar las pruebas unitarias para probar la lógica de la aplicación y no el servidor de base de datos.

Para crear una aplicación MVC que sea más adaptable a los cambios futuros y que se pueda probar más fácilmente, debe considerar el uso del patrón de repositorio. Cuando se utiliza el patrón Repository, se crea una clase de repositorio independiente que contiene toda la lógica de acceso a la base de datos.

Cuando se crea la clase de repositorio, se crea una interfaz que representa todos los métodos utilizados por la clase de repositorio. Dentro de los controladores, escriba el código en la interfaz en lugar del repositorio. De este modo, puede implementar el repositorio utilizando diferentes tecnologías de acceso a datos en el futuro.

La interfaz en `IMovieRepository` el listado 3 se `ListAll()`nombra y representa un único método denominado .

**Listado 3 –`Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

La clase de repositorio en `IMovieRepository` el listado 4 implementa la interfaz. Observe que contiene un `ListAll()` método denominado que corresponde `IMovieRepository` al método requerido por la interfaz.

**Listado 4 –`Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Por último, la `MoviesController` clase del listado 5 utiliza el patrón Repository. Ya no usa clases LINQ to SQLLINQ to SQL directamente.

**Listado 5 –`Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Observe que `MoviesController` la clase del listado 5 tiene dos constructores. Se llama al primer constructor, el constructor sin parámetros, cuando se ejecuta la aplicación. Este constructor crea una `MovieRepository` instancia de la clase y la pasa al segundo constructor.

El segundo constructor tiene un `IMovieRepository` único parámetro: un parámetro. Este constructor simplemente asigna el valor del parámetro a `_repository`un campo de nivel de clase denominado .

La `MoviesController` clase está aprovechando un patrón de diseño de software llamado patrón de inserción de dependencias. En particular, está utilizando algo llamado Inyección de dependencia de constructor. Puede leer más sobre este patrón leyendo el siguiente artículo de Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Observe que todo el `MoviesController` código de la clase (con la excepción del primer constructor) interactúa con la `IMovieRepository` interfaz en lugar de la clase real. `MovieRepository` El código interactúa con una interfaz abstracta en lugar de una implementación concreta de la interfaz.

Si desea modificar la tecnología de acceso a datos utilizada `IMovieRepository` por la aplicación, simplemente puede implementar la interfaz con una clase que utiliza la tecnología de acceso de base de datos alternativa. Por ejemplo, podría `EntityFrameworkMovieRepository` crear una `SubSonicMovieRepository` clase o una clase. Dado que la clase de controlador está programada en `IMovieRepository` la interfaz, puede pasar una nueva implementación de a la clase de controlador y la clase seguiría funcionando.

Además, si desea `MoviesController` probar la clase, puede pasar una `HomeController`clase de repositorio de películas falsas a la . Puede implementar `IMovieRepository` la clase con una clase que no tenga acceso a `IMovieRepository` la base de datos, pero que contenga todos los métodos necesarios de la interfaz. De este modo, puede `MoviesController` probar unitariamente la clase sin tener que acceder a una base de datos real.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo crear clases de modelo MVC aprovechando Microsoft LINQ to SQL. Examinamos dos estrategias para mostrar los datos de la base de datos en una aplicación ASP.NET MVC. En primer lugar, creamos LINQ to SQLLINQ to SQL clases y usamos las clases directamente dentro de una acción de controlador. El uso de LINQ to SQLLINQ to SQL clases dentro de un controlador le permite mostrar rápida y fácilmente los datos de base de datos en una aplicación MVC.

A continuación, exploramos una ruta un poco más difícil, pero definitivamente más virtuosa, para mostrar los datos de la base de datos. Aprovechamos el patrón Repository y colocamos toda nuestra lógica de acceso a la base de datos en una clase de repositorio independiente. En nuestro controlador, escribimos todo nuestro código contra una interfaz en lugar de una clase concreta. La ventaja del patrón Repository es que nos permite cambiar fácilmente las tecnologías de acceso a bases de datos en el futuro y nos permite probar fácilmente nuestras clases de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-the-entity-framework-cs.md)
> [Siguiente](displaying-a-table-of-database-data-cs.md)
