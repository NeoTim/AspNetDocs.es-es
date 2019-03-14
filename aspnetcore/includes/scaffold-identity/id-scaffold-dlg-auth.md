---
ms.openlocfilehash: cb9041a466309a400b19cdecd76f653499c6ac4b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57053982"
---
Ejecute el proveedor de scaffolding de identidad:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Desde **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.
* En el panel izquierdo de la **agregar Scaffold** cuadro de diálogo, seleccione **identidad** > **agregar**.
* En el **identidad de ADD** cuadro de diálogo, seleccione las opciones que desee.
  * Seleccione la página de diseño existente, o se sobrescribirá el archivo de diseño con formato incorrecto. Cuando una existente  *\_Layout.cshtml* archivo seleccionado, es **no** sobrescribe.

 Por ejemplo `~/Pages/Shared/_Layout.cshtml` para las páginas de Razor `~/Views/Shared/_Layout.cshtml` para los proyectos de MVC
* Para usar el contexto de datos existente, seleccione al menos un archivo para invalidar. Debe seleccionar al menos un archivo para agregar el contexto de datos.
  * Seleccione la clase de contexto de datos.
  * Seleccione **agregar**.
* Para crear un nuevo contexto de usuario y, posiblemente, cree una clase de usuario personalizada para la identidad:
  * Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.
  * Seleccione **agregar**.

Nota: Si va a crear un nuevo contexto de usuario, no tienes que seleccionar un archivo para invalidar.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al proyecto (\*.csproj) archivo. Ejecute el siguiente comando en el directorio del proyecto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:

```cli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute el proveedor de scaffolding de identidad con las opciones que desee. Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando. Use el nombre completo correcto para el contexto de base de datos:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

PowerShell usa el punto y coma como separador de comandos. Cuando se usa powershell, el punto y coma en la lista de archivos de escape o coloque la lista de archivos en comillas dobles. Por ejemplo:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Si ejecuta el proveedor de scaffolding de identidad sin especificar el `--files` marca o `--useDefaultUI` marca todas las páginas disponibles de la interfaz de usuario de identidad se creará en el proyecto.

-------------
