---
title: 'Tutorial: Creación de una API web con ASP.NET Core MVC'
author: rick-anderson
description: Compilación de una API web con ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 02/4/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 686397cd25248ce7b37e505c7129a3b56d4ada1b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045782"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a>Tutorial: Creación de una API web con ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial se enseñan los conceptos básicos de la compilación de una API web con ASP.NET Core.

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Crear un proyecto de API web.
> * Agregar una clase de modelo.
> * Crear el contexto de la base de datos.
> * Registrar el contexto de la base de datos.
> * Agregar un controlador.
> * Agregar métodos CRUD.
> * Configurar el enrutamiento y las rutas de dirección URL.
> * Especificar los valores devueltos.
> * Llamar a la API web con Postman.
> * Llamar a la API web con jQuery.

Al final, tendrá una API web que pueda administrar las tareas "pendientes" almacenadas en una base de datos relacional.

## <a name="overview"></a>Información general

En este tutorial se crea la siguiente API:

|API | Descripción | Cuerpo de la solicitud | Cuerpo de la respuesta |
|--- | ---- | ---- | ---- |
|GET /api/todo | Obtener todas las tareas pendientes | Ninguna | Matriz de tareas pendientes|
|GET /api/todo/{id} | Obtener un elemento por identificador | Ninguna | Tarea pendiente|
|POST /api/todo | Incorporación de un nuevo elemento | Tarea pendiente | Tarea pendiente |
|PUT /api/todo/{id} | Actualizar un elemento existente &nbsp; | Tarea pendiente | Ninguna |
|DELETE /api/todo/{id} &nbsp; &nbsp; | Eliminar un elemento &nbsp; &nbsp; | Ninguna | Ninguna|

En el diagrama siguiente, se muestra el diseño de la aplicación.

![El cliente está representado por un cuadro a la izquierda, envía una solicitud y recibe una respuesta de la aplicación, representada por un cuadro en la parte derecha. En el cuadro de la aplicación, hay tres cuadros que representan el controlador, el modelo y la capa de acceso a datos. La solicitud entra en el controlador de la aplicación y se producen operaciones de lectura/escritura entre el controlador y la capa de acceso a datos. El modelo se serializa y se devuelve al cliente en la respuesta.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a>Creación de un proyecto web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.
* Seleccione la plantilla **Aplicación web ASP.NET Core**. Asigne al proyecto el nombre *TodoApi* y haga clic en **Aceptar**.
* En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la versión de ASP.NET Core. Seleccione la plantilla **API** y haga clic en **Aceptar**. **No** seleccione **Habilitar compatibilidad con Docker**.

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambie el directorio (`cd`) a la carpeta que va a contener la carpeta del proyecto.
* Ejecute los comandos siguientes:

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Estos comandos crean un nuevo proyecto de API web y abren una nueva instancia de Visual Studio Code en la carpeta del proyecto nuevo.

* Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Seleccione **Archivo** > **Nueva solución**.

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* Seleccione **Aplicación .NET Core** > **API web de ASP.NET Core** > **Siguiente**.

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de **.NET Core 2.2*.

* Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>Prueba de la API

La plantilla del proyecto crea una API `values`. Llame al método `Get` desde un explorador para probar la aplicación.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Presione Ctrl+F5 para ejecutar la aplicación. Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.

Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**. En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Presione Ctrl+F5 para ejecutar la aplicación. En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Seleccione **Ejecutar** > **Iniciar con depuración** para iniciar la aplicación. Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente. Se devuelve un error HTTP 404 (No encontrado). Anexe `/api/values` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/api/values`).

---

Se devuelve el siguiente JSON:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Incorporación de una clase de modelo

Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra. El modelo para esta aplicación es una clase `TodoItem` única.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto. Seleccione **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

* Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**. Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.

* Reemplace el código de plantilla por el código siguiente:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Agregue una carpeta denominada *Models*.

* Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en el proyecto. Seleccione **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.

* Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.

* Reemplace el código de plantilla por el código siguiente:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

La propiedad `Id` funciona como clave única en una base de datos relacional.

Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.

## <a name="add-a-database-context"></a>Incorporación de un contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos. Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**. Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* Agregue una clase `TodoContext` a la carpeta *Models*.

---

* Reemplace el código de plantilla por el código siguiente:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Registro del contexto de base de datos

En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection). El contenedor proporciona el servicio a los controladores.

Actualice *Startup.cs* con el siguiente código resaltado:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

El código anterior:

* Elimina las declaraciones `using` no utilizadas.
* Agrega el contexto de base de datos para el contenedor de DI.
* Especifica que el contexto de base de datos usará una base de datos en memoria.

## <a name="add-a-controller"></a>Incorporación de un controlador

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Haga clic con el botón derecho en la carpeta *Controllers*.
* Seleccione **Agregar** > **Nuevo elemento**.
* En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.
* Asigne a la clase el nombre *TodoController* y seleccione **Agregar**.

  ![Cuadro de diálogo Agregar nuevo elemento con la palabra "controller" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* En la carpeta *Controllers*, cree una clase denominada `TodoController`.

---

* Reemplace el código de plantilla por el código siguiente:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

El código anterior:

* Define una clase de controlador de API sin métodos.
* Decora la clase con el atributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute). Este atributo indica que el controlador responde a las solicitudes de la API web. Para obtener información sobre los comportamientos específicos que permite el atributo, consulte [Anotación con el atributo ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).
* Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador. El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.
* Si la base de datos está vacía, le agrega un elemento denominado `Item1`. Este código está en el constructor, de manera que se ejecuta cada vez que hay una nueva solicitud HTTP. Si elimina todos los elementos, el constructor volverá a crear `Item1` la próxima vez que se llame a un método de API. De este modo, es posible que parezca que la eliminación no ha funcionado, cuando en realidad sí lo ha hecho.

## <a name="add-get-methods"></a>Incorporación de métodos Get

Para proporcionar una API que recupere tareas pendientes, agregue estos métodos a la clase `TodoController`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Estos métodos implementan dos puntos de conexión GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Llame a los dos puntos de conexión desde un explorador para probar la aplicación. Por ejemplo:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

La llamada a `GetTodoItems` genera la siguiente respuesta HTTP:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>Enrutamiento y rutas URL

El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET. La ruta de dirección URL para cada método se construye como sigue:

* Comience por la cadena de plantilla en el atributo `Route` del controlador:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller". En este ejemplo, el nombre de clase de controlador es **Todo**Controller; por tanto, el nombre del controlador es "todo". El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.
* Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso. En este ejemplo no se usa una plantilla. Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente. Al invocar a `GetTodoItem`, el valor `"{id}"` de la dirección URL se proporciona al método en su parámetro `id`.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Valores devueltos

El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta. El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada. Las excepciones no controladas se convierten en errores 5xx.

Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP. Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:

* Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).
* En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON. Devolver `item` genera una respuesta HTTP 200.

## <a name="test-the-gettodoitems-method"></a>Prueba del método GetTodoItems

En este tutorial se usa Postman para probar la API web.

* Instale [Postman](https://www.getpostman.com/apps).
* Inicie la aplicación web.
* Inicie Postman.
* Deshabilite **Comprobación del certificado SSL**.
  
  * En **Archivo > Configuración** (pestaña **General*), deshabilite **Comprobación del certificado SSL**.
    > [!WARNING]
    > Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.

* Cree una nueva solicitud.
  * Establezca el método HTTP en **GET**.
  * Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/todo`. Por ejemplo: `https://localhost:5001/api/todo`.
* Establezca **Vista de dos paneles** en Postman.
* Seleccione **Enviar**.

![Postman con solicitud Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Incorporación de un método Create

Agregue el siguiente método `PostTodoItem`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.

El método `CreatedAtAction` realiza las acciones siguientes:

* Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente. HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.
* Agrega un encabezado `Location` a la respuesta. El encabezado `Location` especifica el identificador URI de la tarea pendiente recién creada. Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`. La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Prueba del método PostTodoItem

* Compile el proyecto.
* En Postman, establezca el método HTTP en `POST`.
* Seleccione la pestaña **Cuerpo**.
* Seleccione el botón de radio **Raw** (Sin formato).
* Establezca el tipo en **JSON (application/json)**.
* En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Seleccione **Enviar**.

  ![Postman con solicitud de creación](first-web-api/_static/create.png)

  Si recibe un error 405 (Método no permitido), probablemente sea el resultado de no haber compilado el proyecto después de agregar el método `PostTodoItem`.

### <a name="test-the-location-header-uri"></a>Prueba del URI del encabezado de ubicación

* Seleccione la pestaña **Encabezados** en el panel **Respuesta**.
* Copie el valor de encabezado **Ubicación**:

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/pmc2.png)

* Establezca el método en GET.
* Pegue el URI (por ejemplo, `https://localhost:5001/api/Todo/2`).
* Seleccione **Enviar**.

## <a name="add-a-puttodoitem-method"></a>Incorporación de un método PutTodoItem

Agregue el siguiente método `PutTodoItem`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT. La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios. Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Si obtiene un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.

### <a name="test-the-puttodoitem-method"></a>Prueba del método PutTodoItem

En este ejemplo se usa una base de datos en memoria que se debe iniciar cada vez que se inicia la aplicación. Debe haber un elemento en la base de datos antes de que realice una llamada PUT. Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.

Actualice la tarea pendiente que tiene el id. = 1 y establezca su nombre en "feed fish":

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

En la imagen siguiente, se muestra la actualización de Postman:

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>Incorporación de un método DeleteTodoItem

Agregue el siguiente método `DeleteTodoItem`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La respuesta de `DeleteTodoItem` es [204 (Sin contenido)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Prueba del método DeleteTodoItem

Use Postman para eliminar una tarea pendiente:

* Establezca el método en `DELETE`.
* Establezca el URI del objeto que quiera eliminar, por ejemplo, `https://localhost:5001/api/todo/1`.
* Seleccione **Enviar**.

La aplicación de ejemplo permite eliminar todos los elementos, pero al eliminar el último elemento, se creará uno nuevo en el constructor de clase de modelo la próxima vez que se llame a la API.

## <a name="call-the-api-with-jquery"></a>Llamada a la API con jQuery

En esta sección, se agrega una página HTML que usa jQuery para llamar a la API web. jQuery inicia la solicitud y actualiza la página con los detalles de la respuesta de la API.

Configure la aplicación para [atender archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [habilitar la asignación de archivos predeterminada](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
Cree una carpeta *wwwroot* en el directorio del proyecto.
::: moniker-end

Agregue un archivo HTML denominado *index.html* al directorio *wwwroot*. Reemplace el contenido por el siguiente marcado:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot*. Reemplace el contenido por el siguiente código:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para probar la página HTML localmente:

* Abra *Properties\launchSettings.json*.
* Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.

Existen varias formas de obtener jQuery. En el fragmento de código anterior, la biblioteca se carga desde una red CDN.

En este ejemplo se llama a todos los métodos CRUD de la API. A continuación, encontrará algunas explicaciones de las llamadas a la API.

### <a name="get-a-list-of-to-do-items"></a>Obtención de una lista de tareas pendientes

La función de JQuery [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `GET` a la API, que devuelve código JSON que representa una matriz de tareas pendientes. La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente. En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Incorporación de una tarea pendiente

La función [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `POST` con la tarea pendiente en su cuerpo. Las opciones `accepts` y `contentType` se establecen en `application/json` para especificar el tipo de medio que se va a recibir y a enviar. La tarea pendiente se convierte en JSON mediante [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Actualizar una tarea pendiente

El hecho de actualizar una tarea pendiente es similar al de agregar una. El valor `url` cambia para agregar el identificador único del elemento, y `type` es `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Eliminar una tarea pendiente

Para eliminar una tarea pendiente, hay que establecer el valor `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a>Recursos adicionales

[Vea o descargue el código de ejemplo para este tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Vea [cómo descargarlo](xref:index#how-to-download-a-sample).

Para obtener más información, vea los siguientes recursos:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear un proyecto de API web.
> * Agregar una clase de modelo.
> * Crear el contexto de la base de datos.
> * Registrar el contexto de la base de datos.
> * Agregar un controlador.
> * Agregar métodos CRUD.
> * Configurar el enrutamiento y las rutas de dirección URL.
> * Especificar los valores devueltos.
> * Llamar a la API web con Postman.
> * Llamar a la API web con jQuery.

Pase al siguiente tutorial para obtener información sobre cómo generar páginas de ayuda de API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
