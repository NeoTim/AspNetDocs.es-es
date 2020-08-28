---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 26d1a66cbc022664af14e4dfe4c4b4892d409b95
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045174"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Obtener acceso a los datos del modelo desde un controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

En esta sección, creará una nueva `MoviesController` clase y escribirá código que recupera los datos de la película y los muestra en el explorador con una plantilla de vista.

**Compile la aplicación** antes de pasar al paso siguiente. Si no compila la aplicación, obtendrá un error al agregar un controlador.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta *Controladores* y, a continuación, haga clic en **Agregar**y luego en **controlador**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

En el cuadro de diálogo **Agregar scaffold** , haga clic en **controlador de MVC 5 con vistas, utilizando Entity Framework**y, a continuación, haga clic en **Agregar**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Seleccione **película (MvcMovie. Models)** para la clase modelo.
- Seleccione **MovieDBContext (MvcMovie. Models)** para la clase de contexto de datos.
- En el nombre del controlador, escriba **MoviesController**.

  En la imagen siguiente se muestra el cuadro de diálogo completado.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Haga clic en **Agregar**. (Si recibe un error, probablemente no haya compilado la aplicación antes de empezar a agregar el controlador). Visual Studio crea los siguientes archivos y carpetas:

- *Un archivo MoviesController.CS* en la carpeta *Controllers* .
- Una carpeta *Views\Movies* .
- *Cree. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* en la nueva carpeta *Views\Movies*

Visual Studio crea automáticamente las vistas y los métodos de acción [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) automáticamente (la creación automática de métodos y vistas de acción CRUD se conoce como scaffolding). Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar entradas de películas.

Ejecute la aplicación y haga clic en el vínculo de **película de MVC** (o vaya al `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador). Dado que la aplicación se basa en el enrutamiento predeterminado (definido en el *archivo \_ Start\RouteConfig.CS* de la aplicación), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta al `Index` método de acción predeterminado del `Movies` controlador. En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es realmente la misma que la solicitud del explorador `http://localhost:xxxxx/Movies/Index` . El resultado es una lista vacía de películas, ya que aún no ha agregado ninguna.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Crear una película

Seleccione el vínculo **Crear nuevo**. Escriba algunos detalles sobre una película y, a continuación, haga clic en el botón **crear** .

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Es posible que no pueda escribir comas o puntos decimales en el campo precio. Para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma ( &quot; , &quot; ) para un separador decimal y formatos de fecha que no son de Inglés de EE. UU., debe incluir *globalize.js* y el archivo de *globalize.cultures.jso referencia cultural * específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) y JavaScript que se va a usar `Globalize.parseFloat` . Mostraré cómo hacerlo en el siguiente tutorial. Por ahora, escriba solamente números enteros como 10.

Al hacer clic en el botón **crear** , el formulario se envía al servidor, donde se guarda la información de la película en la base de datos. A continuación, se le redirigirá a la dirección URL de */Movies* , donde podrá ver la película recién creada en la lista.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.

## <a name="examining-the-generated-code"></a>Examinar el código generado

Abra el archivo *Controllers\MoviesController.CS* y examine el `Index` método generado. A continuación se muestra una parte del controlador de película con el `Index` método.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Una solicitud al `Movies` controlador devuelve todas las entradas de la `Movies` tabla y, a continuación, pasa los resultados a la `Index` vista. La línea siguiente de la `MoviesController` clase crea una instancia de un contexto de la base de datos de película, tal y como se ha descrito anteriormente. Puede usar el contexto de la base de datos de películas para consultar, editar y eliminar películas.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fuertemente tipados y la @model palabra clave

Anteriormente en este tutorial, vio cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante el `ViewBag` objeto. `ViewBag`Es un objeto dinámico que proporciona un modo de enlace en tiempo de ejecución adecuado para pasar información a una vista.

MVC también proporciona la capacidad de pasar objetos *fuertemente tipados* a una plantilla de vista. Este enfoque fuertemente tipado permite una mejor comprobación en tiempo de compilación del código y [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) más enriquecida en el editor de Visual Studio. El mecanismo de scaffolding de Visual Studio usó este enfoque (es decir, pasando un modelo *fuertemente tipado* ) con la `MoviesController` clase y las plantillas de vista cuando creó los métodos y las vistas.

En el archivo *Controllers\MoviesController.CS* , examine el `Details` método generado. El `Details` método se muestra a continuación.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id`Por lo general, el parámetro se pasa como datos de ruta; por ejemplo, `http://localhost:1234/movies/details/1` establecerá el controlador en el controlador de película, la acción en `details` y `id` en 1. También puede pasar el identificador con una cadena de consulta como se indica a continuación:

`http://localhost:1234/movies/details?id=1`

Si `Movie` se encuentra, se pasa una instancia del `Movie` modelo a la `Details` vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examine el contenido del archivo *Views\Movies\Details.cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Al incluir una `@model` instrucción en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera la vista. Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado. Por ejemplo, en la plantilla *details. cshtml* , el código pasa cada campo de película a las `DisplayNameFor` aplicaciones auxiliares HTML y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) con el objeto fuertemente tipado `Model` . Los `Create` `Edit` métodos y y las plantillas de vista también pasan un objeto de modelo de película.

Examine la plantilla de vista *index. cshtml* y el `Index` método en el archivo *MoviesController.CS* . Observe cómo el código crea un [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto cuando llama al `View` método auxiliar en el método de `Index` acción. A continuación, el código pasa esta `Movies` lista del `Index` método de acción a la vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Al crear el controlador de película, Visual Studio incluye automáticamente la siguiente `@model` instrucción en la parte superior del archivo *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Esta `@model` directiva permite tener acceso a la lista de películas que el controlador pasó a la vista usando un `Model` objeto fuertemente tipado. Por ejemplo, en la plantilla *index. cshtml* , el código recorre en bucle las películas mediante una `foreach` instrucción sobre el objeto fuertemente tipado `Model` :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Dado que el `Model` objeto está fuertemente tipado (como un `IEnumerable<Movie>` objeto), cada `item` objeto del bucle se escribe como `Movie` . Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y la compatibilidad completa con IntelliSense en el editor de código:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Trabajar con SQL Server LocalDB

Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó apuntaba a una `Movies` base de datos que aún no existía, por lo que Code First creó la base de datos automáticamente. Para comprobar que se ha creado, busque en la carpeta de * \_ datos* de la aplicación. Si no ve el archivo *movies. MDF* , haga clic en el botón **Mostrar todos los archivos** en la barra de herramientas **Explorador de soluciones** , haga clic en el botón **Actualizar** y, a continuación, expanda la carpeta * \_ datos* de la aplicación.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Haga doble clic en *movies. MDF* para abrir el **Explorador de servidores**y, a continuación, expanda la carpeta **tablas** para ver la tabla películas. Observe el icono de llave junto a ID. De forma predeterminada, EF convertirá una propiedad denominada ID en la clave principal. Para obtener más información sobre EF y MVC, consulte el excelente tutorial de Tom Dykstra sobre [MVC y EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Haga clic con el botón secundario en la `Movies` tabla y seleccione **Mostrar datos de tabla** para ver los datos que ha creado.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Haga clic con el botón secundario en la `Movies` tabla y seleccione **abrir definición de tabla** para ver la estructura de la tabla que Entity Framework Code First crear automáticamente.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Observe cómo se asigna el esquema de la `Movies` tabla a la `Movie` clase que creó anteriormente. Entity Framework Code First crea automáticamente este esquema en función de su `Movie` clase.

Cuando haya terminado, cierre la conexión haciendo clic con el botón secundario en *MovieDBContext* y seleccionando **cerrar conexión**. (Si no cierra la conexión, es posible que reciba un error la próxima vez que ejecute el proyecto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos. En el siguiente tutorial, examinaremos el resto del código con scaffolding y agregaremos un `SearchIndex` método y una `SearchIndex` vista que le permita buscar películas en esta base de datos. Para obtener más información sobre el uso de Entity Framework con MVC, vea [creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-connection-string.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)
