---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creación de una aplicación MVC 3 con Razor y JavaScript discreto Microsoft Docs
author: rick-anderson
description: La aplicación web de ejemplo Lista de usuarios muestra lo sencillo que es crear ASP.NET aplicaciones MVC 3 mediante el motor de vista de Razor. La aplicación de ejemplo s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: c57e19f8eeca15e3676b3d490b08f69786d44f93
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542461"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="50fed-104">Crear una aplicación de MVC 3 con Razor y JavaScript discreto</span><span class="sxs-lookup"><span data-stu-id="50fed-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="50fed-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="50fed-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="50fed-106">La aplicación web de ejemplo Lista de usuarios muestra lo sencillo que es crear ASP.NET aplicaciones MVC 3 mediante el motor de vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="50fed-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="50fed-107">La aplicación de ejemplo muestra cómo usar el nuevo motor de vista de Razor con ASP.NET MVC versión 3 y Visual Studio 2010 para crear un sitio web ficticio de lista de usuarios que incluye funciones como crear, mostrar, editar y eliminar usuarios.</span><span class="sxs-lookup"><span data-stu-id="50fed-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="50fed-108">En este tutorial se describen los pasos que se realizaron para compilar el ejemplo Lista de usuarios ASP.NET aplicación MVC 3.</span><span class="sxs-lookup"><span data-stu-id="50fed-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="50fed-109">Un proyecto de Visual Studio con código fuente de C- y VB está disponible para acompañar este tema: [Descargar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="50fed-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="50fed-110">Si tiene preguntas sobre este tutorial, por favor publíquelas en el [foro MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="50fed-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="50fed-111">Información general</span><span class="sxs-lookup"><span data-stu-id="50fed-111">Overview</span></span>

<span data-ttu-id="50fed-112">La aplicación que va a crear es un sitio web de lista de usuarios simple.</span><span class="sxs-lookup"><span data-stu-id="50fed-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="50fed-113">Los usuarios pueden introducir, ver y actualizar la información del usuario.</span><span class="sxs-lookup"><span data-stu-id="50fed-113">Users can enter, view, and update user information.</span></span>

![Sitio de muestra](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="50fed-115">Puede descargar el proyecto completado de VB y C- [aquí](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="50fed-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="50fed-116">Creación de la aplicación web</span><span class="sxs-lookup"><span data-stu-id="50fed-116">Creating the Web Application</span></span>

<span data-ttu-id="50fed-117">Para iniciar el tutorial, abra Visual Studio 2010 y cree un nuevo proyecto mediante la plantilla *aplicación web ASP.NET MVC 3.*</span><span class="sxs-lookup"><span data-stu-id="50fed-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="50fed-118">Asigne a &quot;la&quot;aplicación el nombre Mvc3Razor .</span><span class="sxs-lookup"><span data-stu-id="50fed-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="50fed-119">[![Nuevo proyecto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="50fed-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="50fed-120">En el cuadro de diálogo **Proyecto Nueva ASP.NET MVC 3** , seleccione Aplicación de **Internet**, seleccione el motor de vista Razor y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="50fed-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Cuadro de diálogo Proyecto New ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="50fed-122">En este tutorial no usará el proveedor de pertenencia ASP.NET, por lo que puede eliminar todos los archivos asociados con el inicio de sesión y la pertenencia.</span><span class="sxs-lookup"><span data-stu-id="50fed-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="50fed-123">En el Explorador de **soluciones**, quite los siguientes archivos y directorios:</span><span class="sxs-lookup"><span data-stu-id="50fed-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="50fed-124">*Controladores-AccountController*</span><span class="sxs-lookup"><span data-stu-id="50fed-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="50fed-125">*Modelos-AccountModels*</span><span class="sxs-lookup"><span data-stu-id="50fed-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="50fed-126">*Vistas _LogOnPartial\\compartidos*</span><span class="sxs-lookup"><span data-stu-id="50fed-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="50fed-127">*Vistas-Cuenta* (y todos los archivos de este directorio)</span><span class="sxs-lookup"><span data-stu-id="50fed-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="50fed-129">Edite `<div>` el `logindisplay` <em>&quot;</em>&quot; <em> \_archivo Layout.cshtml</em> y reemplace el marcado dentro del elemento denominado con el mensaje Login Disabled .</span><span class="sxs-lookup"><span data-stu-id="50fed-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="50fed-130">En el ejemplo siguiente se muestra el nuevo marcado:</span><span class="sxs-lookup"><span data-stu-id="50fed-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="50fed-131">Adición del modelo</span><span class="sxs-lookup"><span data-stu-id="50fed-131">Adding the Model</span></span>

<span data-ttu-id="50fed-132">En el **Explorador**de soluciones , haga clic con el botón secundario en la carpeta *Modelos,* seleccione **Agregar**y, a continuación, haga clic en **Clase**.</span><span class="sxs-lookup"><span data-stu-id="50fed-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nueva clase User Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="50fed-134">Asigne `UserModel` como nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="50fed-134">Name the class `UserModel`.</span></span> <span data-ttu-id="50fed-135">Reemplace el contenido del archivo *UserModel* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="50fed-136">La `UserModel` clase representa a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="50fed-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="50fed-137">Cada miembro de la clase se anota con el [atributo Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) del espacio de [nombres DataAnnotations.](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)</span><span class="sxs-lookup"><span data-stu-id="50fed-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="50fed-138">Los atributos del espacio de [nombres DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporcionan validación automática del lado cliente y del servidor para aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="50fed-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="50fed-139">Abra `HomeController` la clase `using` y agregue una directiva `UserModel` `Users` para que pueda acceder a las clases y:</span><span class="sxs-lookup"><span data-stu-id="50fed-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="50fed-140">Justo después de la `HomeController` declaración, agregue `Users` el siguiente comentario y la referencia a una clase:</span><span class="sxs-lookup"><span data-stu-id="50fed-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="50fed-141">La `Users` clase es un almacén de datos simplificado en memoria que usará en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="50fed-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="50fed-142">En una aplicación real se utilizaría una base de datos para almacenar información de usuario.</span><span class="sxs-lookup"><span data-stu-id="50fed-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="50fed-143">Las primeras líneas `HomeController` del archivo se muestran en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="50fed-144">Compile la aplicación para que el modelo de usuario esté disponible para el asistente de scaffolding en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="50fed-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="50fed-145">Creación de la vista predeterminada</span><span class="sxs-lookup"><span data-stu-id="50fed-145">Creating the Default View</span></span>

<span data-ttu-id="50fed-146">El siguiente paso es agregar un método de acción y una vista para mostrar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="50fed-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="50fed-147">Elimine el archivo *existente de Views-Home-Index.*</span><span class="sxs-lookup"><span data-stu-id="50fed-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="50fed-148">Creará un nuevo archivo *de índice* para mostrar los usuarios.</span><span class="sxs-lookup"><span data-stu-id="50fed-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="50fed-149">En `HomeController` la clase, reemplace `Index` el contenido del método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="50fed-150">Haga clic con `Index` el botón derecho en el método y, a continuación, haga clic en **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="50fed-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Añadir vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="50fed-152">Seleccione la opción **Crear una vista fuertemente tipada.**</span><span class="sxs-lookup"><span data-stu-id="50fed-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="50fed-153">En **View (Clase de datos),** seleccione **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="50fed-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="50fed-154">(Si no ve **Mvc3Razor.Models.UserModel** en el cuadro De vista de clase de **datos,** debe compilar el proyecto.) Asegúrese de que el motor de vista está establecido en **Razor**.</span><span class="sxs-lookup"><span data-stu-id="50fed-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="50fed-155">Establezca **Ver contenido en** **Lista** y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="50fed-155">Set **View content** to **List** and then click **Add**.</span></span>

![Agregar vista de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="50fed-157">La nueva vista aplica scaffolding automáticamente a los `Index` datos de usuario que se pasan a la vista.</span><span class="sxs-lookup"><span data-stu-id="50fed-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="50fed-158">Examine el archivo de vistas recién *generado, Home, Index.*</span><span class="sxs-lookup"><span data-stu-id="50fed-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="50fed-159">Los vínculos **Crear nuevo**, **Editar**, **Detalles**y **Eliminar** no funcionan, pero el resto de la página es funcional.</span><span class="sxs-lookup"><span data-stu-id="50fed-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="50fed-160">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="50fed-160">Run the page.</span></span> <span data-ttu-id="50fed-161">Verá una lista de usuarios.</span><span class="sxs-lookup"><span data-stu-id="50fed-161">You see a list of users.</span></span>

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="50fed-163">Abra el archivo *Index.cshtml* y `ActionLink` reemplace el marcado de **Editar**, **Detalles**y **Eliminar** con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="50fed-164">El nombre de usuario se utiliza como identificador para buscar el registro seleccionado en los vínculos **Editar**, **Detalles**y **Eliminar.**</span><span class="sxs-lookup"><span data-stu-id="50fed-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="50fed-165">Creación de la vista de detalles</span><span class="sxs-lookup"><span data-stu-id="50fed-165">Creating the Details View</span></span>

<span data-ttu-id="50fed-166">El siguiente paso es `Details` agregar un método de acción y una vista para mostrar los detalles del usuario.</span><span class="sxs-lookup"><span data-stu-id="50fed-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="50fed-168">Agregue el `Details` siguiente método al controlador principal:</span><span class="sxs-lookup"><span data-stu-id="50fed-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="50fed-169">Haga clic con `Details` el botón derecho dentro del método y, a continuación, seleccione <strong>Agregar vista</strong>.</span><span class="sxs-lookup"><span data-stu-id="50fed-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="50fed-170">Compruebe que el cuadro Clase de <strong>datos View</strong> contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="50fed-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="50fed-171">Establezca <strong>Ver contenido</strong> en <strong>Detalles</strong> y, a continuación, haga clic en <strong>Agregar</strong>.</span><span class="sxs-lookup"><span data-stu-id="50fed-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Añadir vista de detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="50fed-173">Ejecute la aplicación y seleccione un vínculo de detalles.</span><span class="sxs-lookup"><span data-stu-id="50fed-173">Run the application and select a details link.</span></span> <span data-ttu-id="50fed-174">El andamiaje automático muestra cada propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="50fed-174">The automatic scaffolding shows each property in the model.</span></span>

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="50fed-176">Creación de la vista Editar</span><span class="sxs-lookup"><span data-stu-id="50fed-176">Creating the Edit View</span></span>

<span data-ttu-id="50fed-177">Agregue el `Edit` siguiente método al controlador principal.</span><span class="sxs-lookup"><span data-stu-id="50fed-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="50fed-178">Agregue una vista como en los pasos anteriores, pero establezca **Ver contenido** en **Editar**.</span><span class="sxs-lookup"><span data-stu-id="50fed-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Añadir vista de edición](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="50fed-180">Ejecute la aplicación y edite el nombre y el apellido de uno de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="50fed-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="50fed-181">Si infringe `DataAnnotation` las restricciones que se `UserModel` han aplicado a la clase, al enviar el formulario, verá errores de validación generados por el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="50fed-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="50fed-182">Por ejemplo, si cambia &quot;el&quot; nombre &quot;&quot;Ann a A , al enviar el formulario, se muestra el siguiente error en el formulario:</span><span class="sxs-lookup"><span data-stu-id="50fed-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="50fed-183">En este tutorial, está tratando el nombre de usuario como la clave principal.</span><span class="sxs-lookup"><span data-stu-id="50fed-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="50fed-184">Por lo tanto, la propiedad de nombre de usuario no se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="50fed-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="50fed-185">En el *edit.cshtml* archivo, `Html.BeginForm` justo después de la instrucción, establezca el nombre de usuario para que sea un campo oculto.</span><span class="sxs-lookup"><span data-stu-id="50fed-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="50fed-186">Esto hace que la propiedad se pase en el modelo.</span><span class="sxs-lookup"><span data-stu-id="50fed-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="50fed-187">El siguiente fragmento de código `Hidden` muestra la ubicación de la instrucción:</span><span class="sxs-lookup"><span data-stu-id="50fed-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="50fed-188">Reemplace `TextBoxFor` el `ValidationMessageFor` marcado y para `DisplayFor` el nombre de usuario con una llamada.</span><span class="sxs-lookup"><span data-stu-id="50fed-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="50fed-189">El `DisplayFor` método muestra la propiedad como un elemento de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="50fed-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="50fed-190">En el siguiente ejemplo se muestra el marcado completado.</span><span class="sxs-lookup"><span data-stu-id="50fed-190">The following example shows the completed markup.</span></span> <span data-ttu-id="50fed-191">El `TextBoxFor` original `ValidationMessageFor` y las llamadas se comentan con los`@* *@`caracteres de comienzo-comentario y final de Razor ( )</span><span class="sxs-lookup"><span data-stu-id="50fed-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="50fed-192">Habilitación de la validación del lado del cliente</span><span class="sxs-lookup"><span data-stu-id="50fed-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="50fed-193">Para habilitar la validación del lado cliente en ASP.NET MVC 3, debe establecer dos indicadores y debe incluir tres archivos JavaScript.</span><span class="sxs-lookup"><span data-stu-id="50fed-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="50fed-194">Abra el archivo *Web.config* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="50fed-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="50fed-195">Compruebe `that ClientValidationEnabled` `UnobtrusiveJavaScriptEnabled` y se establecen en true en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="50fed-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="50fed-196">El siguiente fragmento del archivo *Web.config* raíz muestra la configuración correcta:</span><span class="sxs-lookup"><span data-stu-id="50fed-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="50fed-197">Establecer `UnobtrusiveJavaScriptEnabled` en true permite la validación discreta de Ajax y de cliente discreto.</span><span class="sxs-lookup"><span data-stu-id="50fed-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="50fed-198">Cuando se utiliza la validación discreta, las reglas de validación se convierten en atributos HTML5.</span><span class="sxs-lookup"><span data-stu-id="50fed-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="50fed-199">Los nombres de atributo HTML5 solo pueden constar de letras minúsculas, números y guiones.</span><span class="sxs-lookup"><span data-stu-id="50fed-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="50fed-200">Establecer `ClientValidationEnabled` en true habilita la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="50fed-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="50fed-201">Al establecer estas claves en el archivo *Web.config* de la aplicación, se habilita la validación de cliente y JavaScript discreto para toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="50fed-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="50fed-202">También puede habilitar o deshabilitar esta configuración en vistas individuales o en métodos de controlador mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="50fed-203">También debe incluir varios archivos JavaScript en la vista representada.</span><span class="sxs-lookup"><span data-stu-id="50fed-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="50fed-204">Una manera fácil de incluir el JavaScript en todas las vistas es agregarlos a la *Views-Shared\\_Layout.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="50fed-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="50fed-205">Reemplace `<head>` el elemento del \* \_archivo Layout.cshtml\* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="50fed-206">Los dos primeros scripts de jQuery están hospedados por Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="50fed-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="50fed-207">Aprovechando la CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento de primer impacto de sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="50fed-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="50fed-208">Ejecute la aplicación y haga clic en un vínculo de edición.</span><span class="sxs-lookup"><span data-stu-id="50fed-208">Run the application and click an edit link.</span></span> <span data-ttu-id="50fed-209">Ver el origen de la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="50fed-209">View the page's source in the browser.</span></span> <span data-ttu-id="50fed-210">El origen del explorador muestra `data-val` muchos atributos del formulario (para la validación de datos).</span><span class="sxs-lookup"><span data-stu-id="50fed-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="50fed-211">Cuando se habilita la validación de cliente y JavaScript discreto, los `data-val="true"` campos de entrada con una regla de validación de cliente contienen el atributo para desencadenar la validación de cliente discreta.</span><span class="sxs-lookup"><span data-stu-id="50fed-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="50fed-212">Por ejemplo, `City` el campo del modelo se ha decorado con el atributo [Required,](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) lo que da como resultado el código HTML que se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="50fed-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="50fed-213">Para cada regla de validación de cliente, `data-val-rulename="message"`se agrega un atributo que tiene el formulario .</span><span class="sxs-lookup"><span data-stu-id="50fed-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="50fed-214">Mediante `City` el ejemplo de campo mostrado anteriormente, `data-val-required` la regla &quot;de validación&quot;de cliente necesaria genera el atributo y el mensaje Se requiere el campo Ciudad .</span><span class="sxs-lookup"><span data-stu-id="50fed-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="50fed-215">Ejecute la aplicación, edite uno de `City` los usuarios y desactive el campo.</span><span class="sxs-lookup"><span data-stu-id="50fed-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="50fed-216">Al salir del campo, verá un mensaje de error de validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="50fed-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ciudad requerida](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="50fed-218">De forma similar, para cada parámetro de la regla de `data-val-rulename-paramname=paramvalue`validación de cliente, se agrega un atributo que tiene el formulario .</span><span class="sxs-lookup"><span data-stu-id="50fed-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="50fed-219">Por ejemplo, `FirstName` la propiedad se anota con el atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) y especifica una longitud mínima de 3 y una longitud máxima de 8.</span><span class="sxs-lookup"><span data-stu-id="50fed-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="50fed-220">La regla de `length` validación `max` de datos denominada tiene el nombre del parámetro y el valor del parámetro 8.</span><span class="sxs-lookup"><span data-stu-id="50fed-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="50fed-221">A continuación se muestra el código `FirstName` HTML que se genera para el campo al editar uno de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="50fed-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="50fed-222">Para obtener más información acerca de la validación de cliente discreta, vea la entrada [Validación de cliente discreta en ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) en el blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="50fed-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="50fed-223">En ASP.NET MVC 3 Beta, a veces es necesario enviar el formulario para iniciar la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="50fed-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="50fed-224">Esto podría cambiarse para la versión final.</span><span class="sxs-lookup"><span data-stu-id="50fed-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="50fed-225">Creación de la vista Crear</span><span class="sxs-lookup"><span data-stu-id="50fed-225">Creating the Create View</span></span>

<span data-ttu-id="50fed-226">El siguiente paso es `Create` agregar un método de acción y una vista para permitir que el usuario cree un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="50fed-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="50fed-227">Agregue el `Create` siguiente método al controlador principal:</span><span class="sxs-lookup"><span data-stu-id="50fed-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="50fed-228">Agregue una vista como en los pasos anteriores, pero establezca **Ver contenido** en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="50fed-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Creación de vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="50fed-230">Ejecute la aplicación, seleccione el vínculo **Crear** y agregue un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="50fed-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="50fed-231">El `Create` método aprovecha automáticamente la validación del lado cliente y del servidor.</span><span class="sxs-lookup"><span data-stu-id="50fed-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="50fed-232">Intente introducir un nombre de usuario que &quot;contenga&quot;espacios en blanco, como Ben X .</span><span class="sxs-lookup"><span data-stu-id="50fed-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="50fed-233">Al salir del campo de nombre de usuario,`White space is not allowed`se muestra un error de validación del lado cliente ( ).</span><span class="sxs-lookup"><span data-stu-id="50fed-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="50fed-234">Agregue el método Delete</span><span class="sxs-lookup"><span data-stu-id="50fed-234">Add the Delete method</span></span>

<span data-ttu-id="50fed-235">Para completar el tutorial, `Delete` agregue el siguiente método al controlador principal:</span><span class="sxs-lookup"><span data-stu-id="50fed-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="50fed-236">Agregue `Delete` una vista como en los pasos anteriores, estableciendo **Ver contenido** en **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="50fed-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Eliminar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="50fed-238">Ahora tiene una aplicación simple pero totalmente funcional ASP.NET MVC 3 con validación.</span><span class="sxs-lookup"><span data-stu-id="50fed-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
