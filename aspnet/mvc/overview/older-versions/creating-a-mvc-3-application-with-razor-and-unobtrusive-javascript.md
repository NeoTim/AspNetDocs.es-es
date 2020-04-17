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
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Crear una aplicación de MVC 3 con Razor y JavaScript discreto

por [Microsoft](https://github.com/microsoft)

> La aplicación web de ejemplo Lista de usuarios muestra lo sencillo que es crear ASP.NET aplicaciones MVC 3 mediante el motor de vista de Razor. La aplicación de ejemplo muestra cómo usar el nuevo motor de vista de Razor con ASP.NET MVC versión 3 y Visual Studio 2010 para crear un sitio web ficticio de lista de usuarios que incluye funciones como crear, mostrar, editar y eliminar usuarios.
> 
> En este tutorial se describen los pasos que se realizaron para compilar el ejemplo Lista de usuarios ASP.NET aplicación MVC 3. Un proyecto de Visual Studio con código fuente de C- y VB está disponible para acompañar este tema: [Descargar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Si tiene preguntas sobre este tutorial, por favor publíquelas en el [foro MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Información general

La aplicación que va a crear es un sitio web de lista de usuarios simple. Los usuarios pueden introducir, ver y actualizar la información del usuario.

![Sitio de muestra](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Puede descargar el proyecto completado de VB y C- [aquí](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Creación de la aplicación web

Para iniciar el tutorial, abra Visual Studio 2010 y cree un nuevo proyecto mediante la plantilla *aplicación web ASP.NET MVC 3.* Asigne a &quot;la&quot;aplicación el nombre Mvc3Razor .

[![Nuevo proyecto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

En el cuadro de diálogo **Proyecto Nueva ASP.NET MVC 3** , seleccione Aplicación de **Internet**, seleccione el motor de vista Razor y, a continuación, haga clic en **Aceptar**.

![Cuadro de diálogo Proyecto New ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

En este tutorial no usará el proveedor de pertenencia ASP.NET, por lo que puede eliminar todos los archivos asociados con el inicio de sesión y la pertenencia. En el Explorador de **soluciones**, quite los siguientes archivos y directorios:

- *Controladores-AccountController*
- *Modelos-AccountModels*
- *Vistas _LogOnPartial\\compartidos*
- *Vistas-Cuenta* (y todos los archivos de este directorio)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Edite `<div>` el `logindisplay` <em>&quot;</em>&quot; <em> \_archivo Layout.cshtml</em> y reemplace el marcado dentro del elemento denominado con el mensaje Login Disabled . En el ejemplo siguiente se muestra el nuevo marcado:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Adición del modelo

En el **Explorador**de soluciones , haga clic con el botón secundario en la carpeta *Modelos,* seleccione **Agregar**y, a continuación, haga clic en **Clase**.

![Nueva clase User Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Asigne `UserModel` como nombre de la clase. Reemplace el contenido del archivo *UserModel* por el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La `UserModel` clase representa a los usuarios. Cada miembro de la clase se anota con el [atributo Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) del espacio de [nombres DataAnnotations.](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Los atributos del espacio de [nombres DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporcionan validación automática del lado cliente y del servidor para aplicaciones web.

Abra `HomeController` la clase `using` y agregue una directiva `UserModel` `Users` para que pueda acceder a las clases y:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Justo después de la `HomeController` declaración, agregue `Users` el siguiente comentario y la referencia a una clase:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La `Users` clase es un almacén de datos simplificado en memoria que usará en este tutorial. En una aplicación real se utilizaría una base de datos para almacenar información de usuario. Las primeras líneas `HomeController` del archivo se muestran en el ejemplo siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compile la aplicación para que el modelo de usuario esté disponible para el asistente de scaffolding en el paso siguiente.

## <a name="creating-the-default-view"></a>Creación de la vista predeterminada

El siguiente paso es agregar un método de acción y una vista para mostrar a los usuarios.

Elimine el archivo *existente de Views-Home-Index.* Creará un nuevo archivo *de índice* para mostrar los usuarios.

En `HomeController` la clase, reemplace `Index` el contenido del método por el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Haga clic con `Index` el botón derecho en el método y, a continuación, haga clic en **Agregar vista**.

![Añadir vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Seleccione la opción **Crear una vista fuertemente tipada.** En **View (Clase de datos),** seleccione **Mvc3Razor.Models.UserModel**. (Si no ve **Mvc3Razor.Models.UserModel** en el cuadro De vista de clase de **datos,** debe compilar el proyecto.) Asegúrese de que el motor de vista está establecido en **Razor**. Establezca **Ver contenido en** **Lista** y, a continuación, haga clic en **Agregar**.

![Agregar vista de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nueva vista aplica scaffolding automáticamente a los `Index` datos de usuario que se pasan a la vista. Examine el archivo de vistas recién *generado, Home, Index.* Los vínculos **Crear nuevo**, **Editar**, **Detalles**y **Eliminar** no funcionan, pero el resto de la página es funcional. Ejecute la página. Verá una lista de usuarios.

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Abra el archivo *Index.cshtml* y `ActionLink` reemplace el marcado de **Editar**, **Detalles**y **Eliminar** con el código siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

El nombre de usuario se utiliza como identificador para buscar el registro seleccionado en los vínculos **Editar**, **Detalles**y **Eliminar.**

## <a name="creating-the-details-view"></a>Creación de la vista de detalles

El siguiente paso es `Details` agregar un método de acción y una vista para mostrar los detalles del usuario.

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Agregue el `Details` siguiente método al controlador principal:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Haga clic con `Details` el botón derecho dentro del método y, a continuación, seleccione <strong>Agregar vista</strong>. Compruebe que el cuadro Clase de <strong>datos View</strong> contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Establezca <strong>Ver contenido</strong> en <strong>Detalles</strong> y, a continuación, haga clic en <strong>Agregar</strong>.

![Añadir vista de detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Ejecute la aplicación y seleccione un vínculo de detalles. El andamiaje automático muestra cada propiedad del modelo.

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Creación de la vista Editar

Agregue el `Edit` siguiente método al controlador principal.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Agregue una vista como en los pasos anteriores, pero establezca **Ver contenido** en **Editar**.

![Añadir vista de edición](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Ejecute la aplicación y edite el nombre y el apellido de uno de los usuarios. Si infringe `DataAnnotation` las restricciones que se `UserModel` han aplicado a la clase, al enviar el formulario, verá errores de validación generados por el código de servidor. Por ejemplo, si cambia &quot;el&quot; nombre &quot;&quot;Ann a A , al enviar el formulario, se muestra el siguiente error en el formulario:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

En este tutorial, está tratando el nombre de usuario como la clave principal. Por lo tanto, la propiedad de nombre de usuario no se puede cambiar. En el *edit.cshtml* archivo, `Html.BeginForm` justo después de la instrucción, establezca el nombre de usuario para que sea un campo oculto. Esto hace que la propiedad se pase en el modelo. El siguiente fragmento de código `Hidden` muestra la ubicación de la instrucción:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Reemplace `TextBoxFor` el `ValidationMessageFor` marcado y para `DisplayFor` el nombre de usuario con una llamada. El `DisplayFor` método muestra la propiedad como un elemento de solo lectura. En el siguiente ejemplo se muestra el marcado completado. El `TextBoxFor` original `ValidationMessageFor` y las llamadas se comentan con los`@* *@`caracteres de comienzo-comentario y final de Razor ( )

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Habilitación de la validación del lado del cliente

Para habilitar la validación del lado cliente en ASP.NET MVC 3, debe establecer dos indicadores y debe incluir tres archivos JavaScript.

Abra el archivo *Web.config* de la aplicación. Compruebe `that ClientValidationEnabled` `UnobtrusiveJavaScriptEnabled` y se establecen en true en la configuración de la aplicación. El siguiente fragmento del archivo *Web.config* raíz muestra la configuración correcta:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Establecer `UnobtrusiveJavaScriptEnabled` en true permite la validación discreta de Ajax y de cliente discreto. Cuando se utiliza la validación discreta, las reglas de validación se convierten en atributos HTML5. Los nombres de atributo HTML5 solo pueden constar de letras minúsculas, números y guiones.

Establecer `ClientValidationEnabled` en true habilita la validación del lado cliente. Al establecer estas claves en el archivo *Web.config* de la aplicación, se habilita la validación de cliente y JavaScript discreto para toda la aplicación. También puede habilitar o deshabilitar esta configuración en vistas individuales o en métodos de controlador mediante el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

También debe incluir varios archivos JavaScript en la vista representada. Una manera fácil de incluir el JavaScript en todas las vistas es agregarlos a la *Views-Shared\\_Layout.cshtml* archivo. Reemplace `<head>` el elemento del * \_archivo Layout.cshtml* por el código siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Los dos primeros scripts de jQuery están hospedados por Microsoft Ajax Content Delivery Network (CDN). Aprovechando la CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento de primer impacto de sus aplicaciones.

Ejecute la aplicación y haga clic en un vínculo de edición. Ver el origen de la página en el explorador. El origen del explorador muestra `data-val` muchos atributos del formulario (para la validación de datos). Cuando se habilita la validación de cliente y JavaScript discreto, los `data-val="true"` campos de entrada con una regla de validación de cliente contienen el atributo para desencadenar la validación de cliente discreta. Por ejemplo, `City` el campo del modelo se ha decorado con el atributo [Required,](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) lo que da como resultado el código HTML que se muestra en el ejemplo siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Para cada regla de validación de cliente, `data-val-rulename="message"`se agrega un atributo que tiene el formulario . Mediante `City` el ejemplo de campo mostrado anteriormente, `data-val-required` la regla &quot;de validación&quot;de cliente necesaria genera el atributo y el mensaje Se requiere el campo Ciudad . Ejecute la aplicación, edite uno de `City` los usuarios y desactive el campo. Al salir del campo, verá un mensaje de error de validación del lado cliente.

![Ciudad requerida](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

De forma similar, para cada parámetro de la regla de `data-val-rulename-paramname=paramvalue`validación de cliente, se agrega un atributo que tiene el formulario . Por ejemplo, `FirstName` la propiedad se anota con el atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) y especifica una longitud mínima de 3 y una longitud máxima de 8. La regla de `length` validación `max` de datos denominada tiene el nombre del parámetro y el valor del parámetro 8. A continuación se muestra el código `FirstName` HTML que se genera para el campo al editar uno de los usuarios:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Para obtener más información acerca de la validación de cliente discreta, vea la entrada [Validación de cliente discreta en ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) en el blog de Brad Wilson.

> [!NOTE]
> En ASP.NET MVC 3 Beta, a veces es necesario enviar el formulario para iniciar la validación del lado cliente. Esto podría cambiarse para la versión final.

## <a name="creating-the-create-view"></a>Creación de la vista Crear

El siguiente paso es `Create` agregar un método de acción y una vista para permitir que el usuario cree un nuevo usuario. Agregue el `Create` siguiente método al controlador principal:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Agregue una vista como en los pasos anteriores, pero establezca **Ver contenido** en **Crear**.

![Creación de vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Ejecute la aplicación, seleccione el vínculo **Crear** y agregue un nuevo usuario. El `Create` método aprovecha automáticamente la validación del lado cliente y del servidor. Intente introducir un nombre de usuario que &quot;contenga&quot;espacios en blanco, como Ben X . Al salir del campo de nombre de usuario,`White space is not allowed`se muestra un error de validación del lado cliente ( ).

## <a name="add-the-delete-method"></a>Agregue el método Delete

Para completar el tutorial, `Delete` agregue el siguiente método al controlador principal:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Agregue `Delete` una vista como en los pasos anteriores, estableciendo **Ver contenido** en **Eliminar**.

![Eliminar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Ahora tiene una aplicación simple pero totalmente funcional ASP.NET MVC 3 con validación.
