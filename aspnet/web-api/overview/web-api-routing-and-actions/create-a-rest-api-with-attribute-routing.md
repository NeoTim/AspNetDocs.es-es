---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creación de una API de REST con enrutamiento de atributos en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: f6ff5fa18a44b3e6717ec0141ebe101bcdc0bee4
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045187"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Creación de una API de REST con enrutamiento de atributos en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

Web API 2 admite un nuevo tipo de enrutamiento, denominado *enrutamiento de atributos*. Para obtener información general sobre el enrutamiento de atributos, consulte [enrutamiento de atributos en Web API 2](attribute-routing-in-web-api-2.md). En este tutorial, usará el enrutamiento de atributos para crear una API de REST para una colección de libros. La API admitirá las siguientes acciones:

| Acción | URI de ejemplo |
| --- | --- |
| Obtiene una lista de todos los libros. | /api/books |
| Obtiene un libro por identificador. | /api/books/1 |
| Obtener los detalles de un libro. | /api/books/1/details |
| Obtiene una lista de libros por género. | /api/books/fantasy |
| Obtiene una lista de libros por fecha de publicación. | /API/Books/Date/2013-02-16/API/Books/Date/2013/02/16 (forma alternativa) |
| Obtener una lista de libros por un autor determinado. | /api/authors/1/books |

Todos los métodos son de solo lectura (solicitudes HTTP GET).

En el nivel de datos, usaremos Entity Framework. Los registros del libro tendrán los campos siguientes:

- Id.
- Título
- Género
- fecha de publicación
- Price
- Descripción
- AuthorID (clave externa a una tabla de autores)

Sin embargo, para la mayoría de las solicitudes, la API devolverá un subconjunto de estos datos (título, autor y género). Para obtener el registro completo, el cliente solicita `/api/books/{id}/details` .

## <a name="prerequisites"></a>Requisitos previos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition.

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

En primer lugar, ejecuta Visual Studio. En el menú **Archivo**, seleccione **Nuevo** y haga clic en **Proyecto**.

Expanda la categoría **instalado**de  >  **Visual C#** . En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.net (.NET Framework)**. Asigne al proyecto el nombre &quot; BooksAPI &quot; .

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

En el cuadro de diálogo **nueva aplicación Web de ASP.net** , seleccione la plantilla **vacía** . En "Agregar carpetas y referencias principales para", active la casilla **API Web** . Haga clic en **OK**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Esto crea un proyecto de esqueleto que está configurado para la funcionalidad de la API Web.

### <a name="domain-models"></a>Modelos de dominio

A continuación, agregue clases para los modelos de dominio. En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models. Seleccione **Agregar**y, a continuación, seleccione **clase**. Asigne `Author` como nombre de la clase.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Reemplace el código de Author.cs por lo siguiente:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Ahora, agregue otra clase denominada `Book` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Incorporación de un controlador de API Web

En este paso, vamos a agregar un controlador de API Web que usa Entity Framework como capa de datos.

Presione Ctrl+Mayús+B para compilar el proyecto. Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado para crear el esquema de la base de datos.

En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Controllers. Seleccione **Agregar**y, a continuación, seleccione **controlador**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

En el cuadro de diálogo **Agregar scaffold** , seleccione **controlador de Web API 2 con acciones mediante Entity Framework**.

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

En el cuadro de diálogo **Agregar controlador** , en **nombre del controlador**, escriba &quot; BooksController &quot; . Active la &quot; casilla usar acciones de controlador Async &quot; . En **clase de modelo**, seleccione &quot; libro &quot; . (Si no ve la `Book` clase que aparece en la lista desplegable, asegúrese de que ha compilado el proyecto). A continuación, haga clic en el botón "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Haga clic en **Agregar** en el cuadro de diálogo **nuevo contexto de datos** .

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Haga clic en **Agregar** en el cuadro de diálogo **Agregar controlador** . El scaffolding agrega una clase denominada `BooksController` que define el controlador de API. También agrega una clase denominada `BooksAPIContext` en la carpeta models, que define el contexto de datos para Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Inicializar la base de datos

En el menú herramientas, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.

En la ventana Package Manager Console, escriba el siguiente comando:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Este comando crea una carpeta Migrations y agrega un nuevo archivo de código denominado Configuration.cs. Abra este archivo y agregue el código siguiente al `Configuration.Seed` método.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Estos comandos crean una base de datos local e invocan el método de inicialización para rellenar la base de datos.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Agregar clases DTO

Si ejecuta la aplicación ahora y envía una solicitud GET a/API/Books/1, la respuesta tendrá un aspecto similar al siguiente. (He agregado sangría para mejorar la legibilidad).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

En su lugar, deseo que esta solicitud devuelva un subconjunto de los campos. Además, quiero devolver el nombre del autor, en lugar del identificador del autor. Para ello, modificaremos los métodos de controlador para devolver un *objeto de transferencia de datos* (DTO) en lugar del modelo EF. Un DTO es un objeto que está diseñado únicamente para transportar datos.

En explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Agregar**  |  **nueva carpeta**. Asigne a la carpeta el nombre &quot; Dto &quot; . Agregue una clase denominada `BookDto` a la carpeta dto, con la siguiente definición:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Agregue otra clase llamada `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

A continuación, actualice la `BooksController` clase para devolver `BookDto` las instancias. Usaremos el método [Queryable. Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) para proyectar `Book` instancias de en `BookDto` instancias de. Este es el código actualizado para la clase de controlador.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Eliminé los `PutBook` `PostBook` métodos, y `DeleteBook` , porque no son necesarios para este tutorial.

Ahora, si ejecuta la aplicación y solicita/API/Books/1, el cuerpo de la respuesta debe ser similar al siguiente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Agregar atributos de ruta

A continuación, convertiremos el controlador para usar el enrutamiento de atributos. En primer lugar, agregue un atributo **RoutePrefix** al controlador. Este atributo define los segmentos de URI iniciales para todos los métodos de este controlador.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

A continuación, agregue los atributos **[Route]** a las acciones del controlador, como se indica a continuación:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

La plantilla de ruta de cada método de controlador es el prefijo más la cadena especificada en el atributo **Route** . Para el `GetBook` método, la plantilla de ruta incluye la cadena parametrizada &quot; {ID: int} &quot; , que coincide si el segmento del URI contiene un valor entero.

| Método | Plantilla de ruta | URI de ejemplo |
| --- | --- | --- |
| `GetBooks` | "API/libros" | `http://localhost/api/books` |
| `GetBook` | "API/books/{id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Obtener detalles del libro

Para obtener detalles del libro, el cliente enviará una solicitud GET a `/api/books/{id}/details` , donde *{ID}* es el identificador del libro.

Agregue el siguiente método a la clase `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Si solicita `/api/books/1/details` , la respuesta tiene el siguiente aspecto:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Obtener libros por género

Para obtener una lista de libros en un género específico, el cliente enviará una solicitud GET a `/api/books/genre` , donde *Genre* es el nombre del género. (Por ejemplo, `/api/books/fantasy`).

Agregue el método siguiente a `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Aquí estamos definiendo una ruta que contiene un parámetro {Genre} en la plantilla URI. Tenga en cuenta que la API Web puede distinguir estos dos URI y enrutarlos a distintos métodos:

`/api/books/1`

`/api/books/fantasy`

Esto se debe a que el `GetBook` método incluye una restricción de que el segmento "ID" debe ser un valor entero:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Si solicita/API/Books/Fantasy, la respuesta tiene el siguiente aspecto:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Obtener libros por autor

Para obtener una lista de los libros de un autor determinado, el cliente enviará una solicitud GET a `/api/authors/id/books` , donde *ID* es el identificador del autor.

Agregue el método siguiente a `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Este ejemplo es interesante porque los &quot; libros &quot; se tratan como un recurso secundario de los &quot; autores &quot; . Este patrón es bastante común en las API de RESTful.

La tilde (~) de la plantilla de ruta invalida el prefijo de ruta en el atributo **RoutePrefix** .

## <a name="get-books-by-publication-date"></a>Obtener libros por fecha de publicación

Para obtener una lista de libros por fecha de publicación, el cliente enviará una solicitud GET a `/api/books/date/yyyy-mm-dd` , donde *AAAA-MM-DD* es la fecha.

Esta es una manera de hacerlo:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

El `{pubdate:datetime}` parámetro está restringido para que coincida con un valor **DateTime** . Esto funciona, pero en realidad es más permisivo de lo que nos gustaría. Por ejemplo, estos URI también coincidirán con la ruta:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

No hay ningún problema al permitir estos URI. Sin embargo, puede restringir la ruta a un formato determinado agregando una restricción de expresión regular a la plantilla de ruta:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Ahora solo coincidirán las fechas con el formato &quot; AAAA-MM-DD &quot; . Tenga en cuenta que no usamos el regex para validar que obtuvimos una fecha real. Esto se controla cuando Web API intenta convertir el segmento del URI en una instancia de **DateTime** . Una fecha no válida, como ' 2012-47-99 ', no se convertirá y el cliente obtendrá un error 404.

También puede admitir un separador de barra diagonal ( `/api/books/date/yyyy/mm/dd` ) agregando otro atributo **[Route]** con una regex diferente.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Hay un detalle sutil pero importante aquí. La segunda plantilla de ruta tiene un carácter comodín ( \* ) al principio del parámetro {pubDate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.txt)]

Esto indica al motor de enrutamiento que {pubDate} debe coincidir con el resto del URI. De forma predeterminada, un parámetro de plantilla coincide con un único segmento de URI. En este caso, queremos que {pubDate} abarque varios segmentos de URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Código del controlador

Este es el código completo de la clase BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Resumen

El enrutamiento de atributos proporciona más control y mayor flexibilidad a la hora de diseñar los URI para la API.
