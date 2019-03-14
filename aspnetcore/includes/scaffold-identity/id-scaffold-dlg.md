---
ms.openlocfilehash: e5c80c80380dadfce9cff5ec268535076258d980
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57041122"
---
<span data-ttu-id="e56cd-101">Ejecute el proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="e56cd-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e56cd-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e56cd-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e56cd-103">Desde **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="e56cd-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e56cd-104">En el panel izquierdo de la **agregar Scaffold** cuadro de diálogo, seleccione **identidad** > **agregar**.</span><span class="sxs-lookup"><span data-stu-id="e56cd-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="e56cd-105">En el **identidad de ADD** cuadro de diálogo, seleccione las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="e56cd-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="e56cd-106">Seleccione la página de diseño existente, o se sobrescribirá el archivo de diseño con formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="e56cd-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="e56cd-107">Por ejemplo `~/Pages/Shared/_Layout.cshtml` para las páginas de Razor `~/Views/Shared/_Layout.cshtml` para los proyectos de MVC</span><span class="sxs-lookup"><span data-stu-id="e56cd-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="e56cd-108">Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="e56cd-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="e56cd-109">Seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="e56cd-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e56cd-110">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="e56cd-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e56cd-111">Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:</span><span class="sxs-lookup"><span data-stu-id="e56cd-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="e56cd-112">Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al proyecto (\*.csproj) archivo.</span><span class="sxs-lookup"><span data-stu-id="e56cd-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="e56cd-113">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="e56cd-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="e56cd-114">Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="e56cd-114">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="e56cd-115">En la carpeta del proyecto, ejecute el proveedor de scaffolding de identidad con las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="e56cd-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="e56cd-116">Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e56cd-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

-------------
