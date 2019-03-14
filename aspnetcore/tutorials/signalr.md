---
title: Introducción a SignalR de ASP.NET Core
author: bradygaster
description: En este tutorial, creará una aplicación de chat en la que se usa SignalR de ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 53ec924c2d7b4fac227be0c0bf24d93476528167
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029032"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Tutorial: Introducción a SignalR de ASP.NET Core

En este tutorial se describen los conceptos básicos de la creación de una aplicación en tiempo real con SignalR. Aprenderá a:

> [!div class="checklist"]
> * Cree un proyecto web.
> * Agregar la biblioteca cliente de SignalR.
> * Crear un concentrador de SignalR.
> * Configurar el proyecto para usar SignalR.
> * Agregar código que envía mensajes desde cualquier cliente a todos los clientes conectados.

Al final, tendrá una aplicación de chat funcional:

![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a>Creación de un proyecto web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* En el menú, seleccione **Archivo > Nuevo proyecto**.

* En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado > Visual C# > Web > Aplicación web ASP.NET Core**. Asigne al proyecto el nombre *SignalRChat*.

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Seleccione **Aplicación web** para crear un proyecto en el que se use Razor Pages.

* Seleccione una plataforma de destino de **.NET Core**, seleccione **ASP.NET Core 2.2** y haga clic en **Aceptar**.

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) en la carpeta en la que vaya a crear la del proyecto nuevo.

* Ejecute los comandos siguientes:

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el menú, seleccione **Archivo > Nueva solución**.

* Seleccione **.NET Core > Aplicación > Aplicación web ASP.NET Core** (no seleccione **Aplicación web de ASP.NET Core (MVC)**).

* Seleccione **Siguiente**.

* Asigne el nombre *SignalRChat* al proyecto y, después, haga clic en **Crear**.

---

## <a name="add-the-signalr-client-library"></a>Agregar la biblioteca cliente de SignalR

La biblioteca de servidor de SignalR se incluye en el metapaquete `Microsoft.AspNetCore.App`. La biblioteca cliente de JavaScript no se incluye automáticamente en el proyecto. En este tutorial, usará el Administrador de bibliotecas (LibMan) para obtener la biblioteca cliente de *unpkg*. unpkg es una red de entrega de contenido (CDN) que puede entregar todo lo que encuentre en npm, el administrador de paquetes de Node.js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Client-Side Library** (Biblioteca del lado cliente).

* En el cuadro de diálogo **Add Client-Side Library** (Agregar biblioteca del lado cliente), en **Proveedor**, seleccione **unpkg**. 

* En **Biblioteca**, escriba `@aspnet/signalr@1` y seleccione la versión más reciente que no sea una versión preliminar.

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de la biblioteca](signalr/_static/libman1.png)

* Seleccione **Choose specific files** (Elegir archivos específicos), expanda la carpeta *dist/browser* y seleccione *signalr.js* y *signalr.min.js*.

* Establezca **Ubicación de destino** en *wwwroot/lib/signalr/* y seleccione **Instalar**.

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de archivos y destino](signalr/_static/libman2.png)

  LibMan crea una carpeta *wwwroot/lib/signalr* y copia en ella los archivos seleccionados.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* En el terminal integrado, ejecute el comando siguiente para instalar LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan. Puede que deba esperar unos segundos antes de ver la salida.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Los parámetros especifican las opciones siguientes:
  * Use el proveedor unpkg.
  * Copie los archivos en el destino *wwwroot/lib/signalr*.
  * Copie solo los archivos especificados.

  La salida se parece al ejemplo siguiente:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el **terminal**, ejecute el siguiente comando para instalar LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).

* Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Los parámetros especifican las opciones siguientes:
  * Use el proveedor unpkg.
  * Copie los archivos en el destino *wwwroot/lib/signalr*.
  * Copie solo los archivos especificados.

  La salida se parece al ejemplo siguiente:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Creación de un concentrador de SignalR

Un *concentrador* es una clase que actúa como una canalización general que controla la comunicación entre el cliente y el servidor.

* En la carpeta del proyecto SignalRChat, cree una carpeta *Hubs*.

* En la carpeta *Hubs*, cree un archivo *ChatHub.cs* con el código siguiente:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  La clase `ChatHub` hereda de la clase `Hub` de SignalR. La clase `Hub` administra las conexiones, los grupos y la mensajería.

  Puede llamarse al método `SendMessage` mediante un cliente conectado para enviar un mensaje a todos los clientes. El código de cliente de JavaScript que llama al método se muestra más adelante en el tutorial. El código de SignalR es asincrónico para proporcionar la máxima escalabilidad.

## <a name="configure-signalr"></a>Configuración de SignalR

El servidor de SignalR se debe configurar para que pase las solicitudes de SignalR a SignalR.

* Agregue el código resaltado siguiente al archivo *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Estos cambios agregan SignalR al sistema de inserción de dependencias de ASP.NET Core y a la canalización de software intermedio.

## <a name="add-signalr-client-code"></a>Adición del código de cliente de SignalR

* Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  El código anterior:

  * Crea cuadros de texto para el nombre y el mensaje de texto, y un botón de envío.
  * Crea una lista con `id="messagesList"` para mostrar los mensajes que se reciben desde el concentrador SignalR.
  * Incluye las referencias de script en SignalR y el código de aplicación de *chat.js* que se va a crear en el paso siguiente.

* En la carpeta *wwwroot/js*, cree un archivo *chat.js* con el código siguiente:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  El código anterior:

  * Crea e inicia una conexión.
  * Agrega al botón de envío un controlador que envía mensajes al concentrador.
  * Agrega al objeto de conexión un controlador que recibe mensajes desde el concentrador y los agrega a la lista.

## <a name="run-the-app"></a>Ejecutar la aplicación

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Presione **CTRL+F5** para ejecutar la aplicación sin depurar.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* En el terminal integrado, ejecute el comando siguiente:

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el menú, seleccione **Ejecutar > Iniciar sin depurar**.

---

* Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.

* Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar mensaje**.

  El nombre y el mensaje se muestran en ambas páginas al instante.

  ![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Si la aplicación no funciona, abra las herramientas para desarrolladores del explorador (F12) y vaya a la consola. Es posible que vea errores relacionados con el código HTML y JavaScript. Por ejemplo, suponga que coloca *signalr.js* en una carpeta distinta a la indicada. En ese caso, la referencia a ese archivo no funcionará y verá un error 404 en la consola.
> ![Error: signalr.js no encontrado](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear un proyecto de aplicación web.
> * Agregar la biblioteca cliente de SignalR.
> * Crear un concentrador de SignalR.
> * Configurar el proyecto para usar SignalR.
> * Agregar código que usa el concentrador para enviar mensajes desde cualquier cliente a todos los clientes conectados.

Para obtener más información sobre SignalR, vea la introducción:

> [!div class="nextstepaction"]
> [Introducción a SignalR de ASP.NET Core](xref:signalr/introduction)
