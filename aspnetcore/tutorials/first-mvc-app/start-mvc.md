---
title: Introducción a ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: c09c06f55c4179e9e2174f0063ab7387b7e4c31b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059252"
---
# <a name="get-started-with-aspnet-core-mvc"></a>Introducción a ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.

La aplicación administra una base de datos de títulos de películas. Aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación web.
> * Agregar un modelo y aplicarle scaffolding.
> * Trabajar con una base de datos.
> * Agregar búsqueda y validación.

Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>Creación de una aplicación web

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

Complete el cuadro de diálogo **Nuevo proyecto**:

* En el panel izquierdo, seleccione **.NET Core**.
* En el panel central, seleccione **Aplicación web ASP.NET Core (.NET Core)**
* Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).
* Seleccione **Aceptar**.

![Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:

* En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.2**.
* Seleccione **Aplicación web (Modelo-Vista-Controlador)**.
* Seleccione **Aceptar**.

![Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear. Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa. Se trata de un proyecto introductorio básico, pero es un buen punto de partida.

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Para realizar el tutorial debe estar familiarizado con VS Code. Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.
* Ejecute el siguiente comando:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).  Seleccione **Sí**.

  * `dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.
  * `code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Seleccione **Archivo** > **Nueva solución**.

  ![macOS: Nueva solución](~/tutorials/first-web-api-mac/_static/sln.png)

* Seleccione **Aplicación .NET Core** > **ASP.NET Core** > **Aplicación web ASP.NET Core (MVC)** > **Siguiente**.

  ![Cuadro de diálogo de nuevo proyecto de macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de **.NET Core 2.2*.

* Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a>Ejecutar la aplicación

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.
* El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.
* Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* Puede depurar la aplicación seleccionando el botón **IIS Express**.

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

Presione Ctrl+F5 para ejecutarla sin el depurador.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`. En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local.

  El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación. Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.

[!INCLUDE[](~/includes/trustCertMac.md)]

* En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. Al ejecutar la aplicación verá otro puerto distinto.
* Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.

------

* Seleccione **Aceptar** para dar su consentimiento al seguimiento. Esta aplicación no lleva un seguimiento de la información personal. El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.

> [!div class="step-by-step"]
> [Siguiente](adding-controller.md)  
