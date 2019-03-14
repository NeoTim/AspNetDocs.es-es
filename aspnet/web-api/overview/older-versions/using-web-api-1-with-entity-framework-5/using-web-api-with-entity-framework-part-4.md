---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Agregar una vista de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: ff46c7cbdf13a048e57ad88ab39077357e35c5b3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042742"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="ec6bf-102">Parte 4: Agregar una vista de administración</span><span class="sxs-lookup"><span data-stu-id="ec6bf-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="ec6bf-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ec6bf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ec6bf-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ec6bf-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="ec6bf-105">Agregar una vista de administración</span><span class="sxs-lookup"><span data-stu-id="ec6bf-105">Add an Admin View</span></span>

<span data-ttu-id="ec6bf-106">Ahora se podrá activar en el lado del cliente y agregue una página que puede consumir datos desde el controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="ec6bf-107">La página permitirá a los usuarios crear, editar o eliminar los productos, mediante el envío de solicitudes AJAX al controlador.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="ec6bf-108">En el Explorador de soluciones, expanda la carpeta Controllers y abra el archivo denominado HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="ec6bf-109">Este archivo contiene un controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-109">This file contains an MVC controller.</span></span> <span data-ttu-id="ec6bf-110">Agregue un método denominado `Admin`:</span><span class="sxs-lookup"><span data-stu-id="ec6bf-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="ec6bf-111">El **HttpRouteUrl** método crea el identificador URI a la API web y la almacenamos esto en el contenedor de vista para su uso posterior.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="ec6bf-112">A continuación, coloque el cursor de texto dentro de la `Admin` método de acción, a continuación, secundario y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="ec6bf-113">Esto le llevará a la **agregar vista** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="ec6bf-114">En el **agregar vista** cuadro de diálogo, nombre de la vista "Admin".</span><span class="sxs-lookup"><span data-stu-id="ec6bf-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="ec6bf-115">Seleccione la casilla de verificación con la etiqueta **crear una vista fuertemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="ec6bf-116">En **clase modelo**, seleccione "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="ec6bf-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="ec6bf-117">Deje el resto de opciones como sus valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="ec6bf-118">Al hacer clic en **agregar** agrega un archivo denominado Admin.cshtml en vistas/inicio.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="ec6bf-119">Abra este archivo y agregue el siguiente código HTML.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="ec6bf-120">Este código HTML define la estructura de la página, pero ninguna funcionalidad está dispuesta todavía.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="ec6bf-121">Crear un vínculo a la página de administración</span><span class="sxs-lookup"><span data-stu-id="ec6bf-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="ec6bf-122">En el Explorador de soluciones, expanda la carpeta Views y, a continuación, expanda la carpeta Shared.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="ec6bf-123">Abra el archivo denominado \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="ec6bf-124">Busque el **ul** elemento con id = "menu" y un vínculo de acción para la vista de administración:</span><span class="sxs-lookup"><span data-stu-id="ec6bf-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="ec6bf-125">En el proyecto de ejemplo, he realizado algunos otros cambios cosméticos, por ejemplo, reemplazando la cadena "Su logotipo aquí".</span><span class="sxs-lookup"><span data-stu-id="ec6bf-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="ec6bf-126">Estos no afectan a la funcionalidad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="ec6bf-127">Puede descargar el proyecto y compare los archivos.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="ec6bf-128">Ejecute la aplicación y haga clic en el vínculo "Admin" que aparece en la parte superior de la página principal.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="ec6bf-129">La página de administración debe tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="ec6bf-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="ec6bf-130">Derecha ahora, la página no hace nada.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="ec6bf-131">En la siguiente sección, vamos a usar Knockout.js para crear una interfaz de usuario dinámica.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="ec6bf-132">Agregar autorización</span><span class="sxs-lookup"><span data-stu-id="ec6bf-132">Add Authorization</span></span>

<span data-ttu-id="ec6bf-133">La página de administración está actualmente accesible a cualquier persona que visite el sitio.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="ec6bf-134">Vamos a cambiar esta opción para restringir los permisos a los administradores.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="ec6bf-135">Empiece agregando un rol de "Administrador" y un usuario administrador.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="ec6bf-136">En el Explorador de soluciones, expanda la carpeta filtros y abra el archivo denominado InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="ec6bf-137">Busque el `SimpleMembershipInitializer` constructor.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="ec6bf-138">Después de llamar a **WebSecurity.InitializeDatabaseConnection**, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec6bf-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="ec6bf-139">Se trata de una manera de agregar el rol "Administrador" y cree un usuario para el rol rápidas en sucio.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="ec6bf-140">En el Explorador de soluciones, expanda la carpeta Controllers y abra el archivo HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="ec6bf-141">Agregar el **Authorize** atributo a la `Admin` método.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="ec6bf-142">Abra el archivo AdminController.cs y agregue el **Authorize** atributo a toda la `AdminController` clase.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="ec6bf-143">MVC y Web API definen **Authorize** atributos, en diferentes espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="ec6bf-144">MVC usa **System.Web.Mvc.AuthorizeAttribute**, mientras que usa la API Web **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="ec6bf-145">Ahora solo los administradores pueden ver la página de administración.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="ec6bf-146">Además, si envía una solicitud HTTP al controlador de administración, la solicitud debe contener una cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="ec6bf-147">Si no es así, el servidor envía una respuesta HTTP 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="ec6bf-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="ec6bf-148">Puede ver esto en Fiddler, envíe una solicitud GET a `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="ec6bf-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ec6bf-149">[Anterior](using-web-api-with-entity-framework-part-3.md)
> [Siguiente](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="ec6bf-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
