---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Visualización de una tabla de datos de base de datos (C-) Microsoft Docs
author: rick-anderson
description: En este tutorial, muesto dos métodos para mostrar un conjunto de registros de base de datos. Muestro dos métodos para dar formato a un conjunto de registros de base de datos en un ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3fca8ac9e9b4e549ca76ffa282cc03aa4cc50e88
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542059"
---
# <a name="displaying-a-table-of-database-data-c"></a>Mostrar una tabla de los datos de la base de datos (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> En este tutorial, muesto dos métodos para mostrar un conjunto de registros de base de datos. Muestro dos métodos para dar formato a un conjunto de registros de base de datos en una tabla HTML. En primer lugar, muestro cómo puede dar formato a los registros de base de datos directamente dentro de una vista. A continuación, demuesoré cómo puede aprovechar los parciales al dar formato a los registros de base de datos.

El objetivo de este tutorial es explicar cómo puede mostrar una tabla HTML de datos de base de datos en una aplicación ASP.NET MVC. En primer lugar, aprenderá a usar las herramientas de scaffolding incluidas en Visual Studio para generar una vista que muestra un conjunto de registros automáticamente. A continuación, aprenderá a usar un parcial como plantilla al dar formato a los registros de base de datos.

## <a name="create-the-model-classes"></a>Crear las clases de modelo

Vamos a mostrar el conjunto de registros de la tabla de base de datos Movies. La tabla de base de datos Movies contiene las siguientes columnas:

<a id="0.3_table01"></a>

| **Nombre de columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Identificador | Int | False |
| Título | Nvarchar(200) | False |
| Director | NVarchar(50) | False |
| DateReleased | DateTime | False |

Para representar la tabla Movies en nuestra aplicación MVC ASP.NET, necesitamos crear una clase de modelo. En este tutorial, usamos Microsoft Entity Framework para crear nuestras clases de modelo.

> [!NOTE] 
> 
> En este tutorial, usamos Microsoft Entity Framework. Sin embargo, es importante comprender que puede usar una variedad de tecnologías diferentes para interactuar con una base de datos desde una aplicación ASP.NET MVC, incluidaLINQ to SQL, NHibernate o ADO.NET.

Siga estos pasos para iniciar el Asistente para Entity Data Model:

1. Haga clic con el botón derecho en la carpeta Modelos de la ventana Explorador de soluciones y seleccione la opción de menú **Agregar, Nuevo elemento**.
2. Seleccione la categoría **Datos** y seleccione la plantilla **ADO.NET Entity Data Model.**
3. Asigne al modelo de datos el nombre *MoviesDBModel.edmx* y haga clic en el botón **Agregar.**

Después de hacer clic en el botón Agregar, aparece el Asistente para Entity Data Model (consulte la figura 1). Siga estos pasos para completar el asistente:

1. En el paso Elegir contenido del **modelo,** seleccione la opción Generar a partir de la base de **datos.**
2. En el paso Elegir la conexión de **datos,** use la conexión de datos *MoviesDB.mdf* y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el botón **Siguiente**.
3. En el paso Elegir los objetos de base de **datos,** expanda el nodo Tablas y seleccione la tabla Películas. Escriba los *modelos* de espacio de nombres y haga clic en el botón **Finalizar.**

[![Creación de clases LINQ to SQLLINQ to SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figura 01**: Creación de clases LINQ to SQLLINQ to SQL ( Haga clic para ver la[imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image2.png))

Después de completar el Asistente para Entity Data Model, se abre Entity Data Model Designer. El diseñador debe mostrar el Movies entidad (consulte la figura 2).

[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figura 02**: Entity Data Model Designer ( Haga clic para ver la[imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image4.png))

Tenemos que hacer un cambio antes de continuar. El Asistente para datos de entidad genera una clase de modelo denominada *Movies* que representa la tabla de base de datos Movies. Dado que usaremos la clase Movies para representar una película determinada, necesitamos modificar el nombre de la clase para que sea *Movie* en lugar de *Movies* (singular en lugar de plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambie el nombre de la clase de Películas a Película. Después de realizar este cambio, haga clic en el botón **Guardar** (el icono del disquete) para generar la clase Movie.

## <a name="create-the-movies-controller"></a>Crear el controlador de películas

Ahora que tenemos una manera de representar nuestros registros de base de datos, podemos crear un controlador que devuelva la colección de películas. En la ventana Explorador de soluciones de Visual Studio, haga clic con el botón secundario en la carpeta Controladores y seleccione la opción de menú **Agregar, Controlador** (consulte la figura 3).

[![El menú Agregar controlador](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figura 03**: El menú Agregar controlador[(Haga clic para ver la imagen de tamaño completo)](displaying-a-table-of-database-data-cs/_static/image6.png)

Cuando aparezca el cuadro de diálogo **Agregar controlador,** escriba el nombre del controlador MovieController (consulte la figura 4). Haga clic en el botón **Agregar** para agregar el nuevo controlador.

[![El cuadro de diálogo Agregar controlador](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figura 04**: El cuadro de diálogo Agregar controlador[(Haga clic para ver la imagen de tamaño completo)](displaying-a-table-of-database-data-cs/_static/image8.png)

Necesitamos modificar la acción Index() expuesta por el controlador Movie para que devuelva el conjunto de registros de base de datos. Modifique el controlador para que se parezca al controlador en el listado 1.

**Listado 1 – Controllers-MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

En el listado 1, la clase MoviesDBEntities se utiliza para representar la base de datos MoviesDB. Para usar esta clase, debe importar el espacio de nombres MvcApplication1.Models de la siguiente manera:

uso de MvcApplication1.Models;

Las *entidades de expresión. MovieSet.ToList()* devuelve el conjunto de todas las películas de la tabla de base de datos Movies.

## <a name="create-the-view"></a>Crear la vista

La forma más fácil de mostrar un conjunto de registros de base de datos en una tabla HTML es aprovechar el scaffolding proporcionado por Visual Studio.

Compile la aplicación seleccionando la opción de menú **Compilar, Compilar solución**. Debe compilar la aplicación antes de abrir el cuadro de diálogo **Agregar vista** o las clases de datos no aparecerán en el cuadro de diálogo.

Haga clic con el botón derecho en la acción Index() y seleccione la opción de menú **Agregar vista** (consulte la figura 5).

[![Adición de una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figura 05**: Adición de una vista[(Haga clic para ver la imagen de tamaño completo)](displaying-a-table-of-database-data-cs/_static/image10.png)

En el cuadro de diálogo **Agregar vista,** active la casilla de **verificación Crear una vista fuertemente tipada**. Seleccione la clase Movie como clase de datos de **vista.** Seleccione *la lista* como el contenido de la **vista** (véase el cuadro 6). Al seleccionar estas opciones se generará una vista fuertemente tipada que muestra una lista de películas.

[![El cuadro de diálogo Agregar vista](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figura 06**: El cuadro de diálogo Agregar vista[(Haga clic para ver la imagen de tamaño completo)](displaying-a-table-of-database-data-cs/_static/image12.png)

Después de hacer clic en el botón **Agregar,** la vista en el listado 2 se genera automáticamente. Esta vista contiene el código necesario para recorrer en iteración la colección de películas y mostrar cada una de las propiedades de una película.

**Listado 2 – Vistas-Película-Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Puede ejecutar la aplicación seleccionando la opción de menú **Depurar, Iniciar depuración** (o pulsando la tecla F5). Al ejecutar la aplicación, se inicia Internet Explorer. Si navega a la dirección URL /Movie, verá la página en la Figura 7.

[![Una mesa de películas](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figura 07**: Una tabla de películas[(Haga clic para ver la imagen de tamaño completo)](displaying-a-table-of-database-data-cs/_static/image14.png)

Si no le gusta nada sobre la apariencia de la cuadrícula de registros de base de datos en la figura 7, simplemente puede modificar la vista de índice. Por ejemplo, puede cambiar el encabezado *DateReleased* a *Date Released* modificando la vista Index.

## <a name="create-a-template-with-a-partial"></a>Crear una plantilla con un parcial

Cuando una vista se vuelve demasiado complicada, es una buena idea empezar a dividir la vista en parciales. El uso de parciales hace que sus vistas sean más fáciles de entender y mantener. Crearemos un parcial que podemos usar como plantilla para dar formato a cada uno de los registros de base de datos de películas.

Siga estos pasos para crear el parcial:

1. Haga clic con el botón derecho del botón derecho en la carpeta Vistas/Película y seleccione la opción de menú **Agregar vista**.
2. Marque la casilla de verificación crear *una vista parcial (.ascx).*
3. Asigne al archivo *MovieTemplate*el nombre parcial .
4. Marque la casilla de verificación **Crear una vista fuertemente tipada**.
5. Seleccione Película como clase de datos de *vista*.
6. Seleccione Vaciar como contenido de la *vista*.
7. Haga clic en el botón **Agregar** para agregar el parcial al proyecto.

Después de completar estos pasos, modifique el MovieTemplate parcial para que tenga el aspecto de Listado 3.

**Listado 3 – Vistas-Película-MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

El parcial del listado 3 contiene una plantilla para una sola fila de registros.

La vista de índice modificada en el listado 4 utiliza el MovieTemplate parcial.

**Listado 4 – Vistas-Película-Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La vista del listado 4 contiene un bucle foreach que recorre en iteración todas las películas. Para cada película, el MovieTemplate parcial se utiliza para dar formato a la película. El MovieTemplate se representa mediante una llamada a la RenderPartial() método auxiliar.

La vista de índice modificada representa la misma tabla HTML de registros de base de datos. Sin embargo, la opinión se ha simplificado en gran medida.

El método RenderPartial() es diferente de la mayoría de los otros métodos auxiliares porque no devuelve una cadena. Por lo tanto, debe llamar al &lt;método RenderPartial() utilizando % Html.RenderPartial(); %&gt; en &lt;lugar de % html.renderpartial(); %&gt;.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede mostrar un conjunto de registros de base de datos en una tabla HTML. En primer lugar, aprendió a devolver un conjunto de registros de base de datos de una acción de controlador aprovechando Microsoft Entity Framework. A continuación, aprendió a usar el scaffolding de Visual Studio para generar una vista que muestra una colección de elementos automáticamente. Por último, aprendió a simplificar la vista aprovechando un parcial. Ha aprendido a usar un parcial como plantilla para que pueda dar formato a cada registro de base de datos.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-linq-to-sql-cs.md)
> [Siguiente](performing-simple-validation-cs.md)
