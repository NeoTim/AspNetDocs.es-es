---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Membresía ? Microsoft Docs
author: rick-anderson
description: ASP.NET pertenencia se basa en el éxito del modelo de autenticación de formularios a partir de ASP.NET 1.x. ASP.NET autenticación de formularios proporciona una manera conveniente de...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: ed48c11cbd483de088239bad7c2452b7fc60a1cf
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543774"
---
# <a name="membership"></a>Pertenencia

por [Microsoft](https://github.com/microsoft)

> ASP.NET pertenencia se basa en el éxito del modelo de autenticación de formularios a partir de ASP.NET 1.x. ASP.NET autenticación de formularios proporciona una manera cómoda de incorporar un formulario de inicio de sesión en la aplicación ASP.NET y validar a los usuarios con una base de datos u otro almacén de datos.

ASP.NET pertenencia se basa en el éxito del modelo de autenticación de formularios a partir de ASP.NET 1.x. ASP.NET autenticación de formularios proporciona una manera cómoda de incorporar un formulario de inicio de sesión en la aplicación ASP.NET y validar a los usuarios con una base de datos u otro almacén de datos. Los miembros de la clase FormsAuthentication permiten gestionar las cookies para la autenticación, comprobar si hay un inicio de sesión válido, cerrar la sesión de un usuario, etc. Sin embargo, la implementación de la autenticación de formularios en una aplicación ASP.NET 1.x puede requerir una cantidad justa de código.

La membresía en ASP.NET 2.0 es un avance importante sobre el uso de la autenticación de formularios por sí solo. (La pertenencia es más sólida cuando se combina con la autenticación de formularios, pero el uso de la autenticación de formularios no es un requisito.) Como verá pronto, puede usar ASP.NET membership y los controles de inicio de sesión en ASP.NET 2.0 para implementar un sistema de pertenencia eficaz sin escribir mucho código en absoluto.

## <a name="implementing-membership-in-aspnet-20"></a>Implementación de la membresía en ASP.NET 2.0

La membresía se implementa siguiendo cuatro pasos. Tenga en cuenta que hay muchos subpasos que están implicados, así como la configuración opcional que se puede implementar también. Estos pasos están diseñados para ilustrar el panorama general de la configuración de la pertenencia.

1. Cree la base de datos de pertenencia (si SQL Server se usa como almacén de pertenencia.)
2. Especifique las opciones de pertenencia en los archivos de configuración de las aplicaciones. (La pertenencia está habilitada de forma predeterminada.)
3. Determine el tipo de almacén de pertenencia que desea usar. Las opciones son: 

    - Microsoft SQL Server (versión 7.0 o posterior)
    - Almacén de Active Directory
    - Proveedor de membresía personalizado
4. Configure la aplicación para la autenticación de formularios ASP.NET. Una vez más, la pertenencia está diseñada para aprovechar la autenticación de formularios, pero el uso de la autenticación de formularios no es un requisito.
5. Defina cuentas de usuario para la pertenencia y configure roles si lo desea.

## <a name="creating-the-membership-database"></a>Creación de la base de datos de miembros

Si usa SQL Server 7.0 o posterior como almacén de\_pertenencia, puede usar la utilidad aspnet regsql (disponible más fácilmente desde el símbolo del sistema de Visual Studio .NET 2005) para configurar la base de datos. La utilidad\_aspnet regsql se puede utilizar como una herramienta de símbolo del sistema o a través de un asistente de GUI. El método del asistente es la forma más fácil de configurar la base de datos. Para acceder al asistente, simplemente ejecute el siguiente comando:

`aspnet_regsql W`

Una vez que ejecute ese comando, se le presentará el Asistente para la instalación de SQL ServerSQL Server ASP.NET como se muestra a continuación.

![](membership/_static/image1.jpg)

**Figura 1**

El Asistente para la instalación de SQL ServerSQL Server ASP.NET crea el sitio Web en la instancia que especifique en el asistente. Sin embargo, ASP.NET usará la cadena de conexión en el archivo machine.config para conectarse a la base de datos. De forma predeterminada, esta cadena de conexión apuntará a una instancia de SQL Server 2005, por lo que si usa una instancia de SQL Server 2000 o SQL Server 7.0, deberá modificar la cadena de conexión en el archivo machine.config. Esa cadena de conexión se puede encontrar aquí:

[!code-xml[Main](membership/samples/sample1.xml)]

Desafortunadamente, si no modifica la cadena de conexión, ASP.NET no le dará un error descriptivo. Simplemente seguirá quejándose diciendo que no ha creado la base de datos. En el caso anterior, he modificado la cadena de conexión para que apunte a mi instancia local de SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Especificar la configuración y agregar usuarios y roles

El siguiente paso para configurar membership es agregar la información necesaria al archivo web.config de la aplicación. En ASP.NET 1.x, modificar el archivo web.config era a veces difícil debido al uso de lowerCamelCase y la falta de Intellisense. Visual Studio .NET 2005 facilita mucho la tarea con Intellisense para los archivos de configuración, pero ASP.NET 2.0 va un paso más allá al proporcionar una interfaz web para editar archivos de configuración.

Puede iniciar la interfaz Web haciendo clic en el botón Configuración de ASP.NET de la barra de herramientas del Explorador de soluciones, como se muestra a continuación. También puede iniciar la interfaz Web a través de ventanas emergentes que se muestran cuando se insertan los controles de inicio de sesión.

![](membership/_static/image2.jpg)

**Figura 2**

Esto inicia la herramienta de administración de sitios web ASP.NET que se muestra a continuación. La administración de sitios web de ASP.NET es una interfaz de cuatro pestañas que facilita la administración de la configuración de la aplicación. Las siguientes pestañas están disponibles:

- **Inicio**
- **Seguridad** Configure usuarios, roles y acceso.
- **Aplicación** Configure los ajustes de la aplicación.
- **Proveedor** Configure y pruebe el proveedor de pertenencia a aplicaciones.

La herramienta de administración de sitios web le permite crear fácilmente nuevos usuarios, crear nuevos roles y administrar usuarios y roles. Esta capacidad no está disponible en la interfaz de Windows. La interfaz de Windows le permite definir fácilmente la configuración de autorización y agregar, eliminar y administrar proveedores, capacidades que no están en la Herramienta de administración de sitios web.

Para iniciar la interfaz de Windows, abra el complemento Internet Information Services, haga clic con el botón derecho en la aplicación y elija Propiedades. Haga clic en la pestaña ASP.NET y, a continuación, haga clic en el botón Editar configuración. (La aplicación debe ejecutarse en ASP.NET 2.0 para que se habilite el botón Editar configuración. También puede configurar la versión ASP.NET en el cuadro de diálogo ASP.NET.) El cuadro de diálogo Configuración de ASP.NET se muestra como se muestra a continuación.

![](membership/_static/image3.jpg)

**Figura 3**

En la pestaña General, se muestran las cadenas de conexión y la configuración de la aplicación. Cualquier configuración en cursiva se define en un archivo de configuración principal (ya sea machine.config o web.config en un nivel superior) y los valores que no están en cursiva son del archivo de configuración de aplicaciones. Si se agrega, quita o edita una configuración en el nivel de aplicación, ASP.NET agregará, quitará o modificará la configuración en los niveles de aplicación web.config en lugar de quitar la configuración del archivo de configuración del que se hereda.

La pestaña Autenticación se muestra a continuación. Aquí es donde configurará sus ajustes de membresía. La configuración de autenticación de formularios, los proveedores de pertenencia y los proveedores de roles se pueden configurar aquí.

![](membership/_static/image4.jpg)

**Figura 4**

## <a name="implementing-membership-in-your-application"></a>Implementación de la membresía en su solicitud

La forma más fácil de implementar ASP.NET pertenencia 2.0 a la aplicación es usar los controles de inicio de sesión proporcionados. Este método le permite implementar los conceptos básicos de ASP.NET pertenencia 2.0 sin escribir ningún código en absoluto.

Los siguientes controles de inicio de sesión están disponibles en ASP.NET 2.0:

## <a name="login-control"></a>Control de inicio de sesión

El control de inicio de sesión proporciona una interfaz para que alguien inicie sesión en su sistema de pertenencia. Le proporciona un cuadro de texto de nombre de usuario y contraseña y un botón de inicio de sesión. Muchas otras características comunes como un enlace para registrarse para las personas que aún no lo han hecho, una casilla de verificación que permite al usuario iniciar sesión automáticamente en visitas posteriores, un enlace para un recordatorio de contraseña, etc. Todas las características del control de inicio de sesión se pueden personalizar a través de las propiedades del control.

En ASP.NET 1.x, los desarrolladores tenían que escribir una buena cantidad de código para realizar una búsqueda al usar la autenticación de formularios. Con ASP.NET membresía 2.0, puede validar a los usuarios sin escribir ningún código en absoluto. ASP.NET hará automáticamente la búsqueda del usuario por usted. (Si usa el control Login sin usar ASP.NET pertenencia, puede usar el método **OnAuthenticate** para validar el usuario.)

## <a name="loginview-control"></a>Control LoginView

El control LoginView es un control con plantilla que proporciona dos plantillas de forma predeterminada; El AnonymousTemplate y el LoggedInTemplate. La plantilla que se muestra viene determinada por si el usuario ha iniciado sesión o no en el sistema de pertenencia. Este control se utiliza normalmente para mostrar un control de inicio de sesión cuando un usuario aún no ha iniciado sesión y un control LoginStatus u otros controles de inicio de sesión cuando el usuario ha iniciado sesión. Si usa la administración de roles en la aplicación ASP.NET, el control LoginView puede mostrar una plantilla específica basada en el rol de usuarios. (Más adelante se tratará más adelante sobre ASP.NET administración de roles.)

## <a name="passwordrecovery-control"></a>PasswordRecovery Control

El control PasswordRecovery permite a los usuarios recibir un correo electrónico con su contraseña actual o restablecer su contraseña. El texto claro y las contraseñas cifradas se pueden recuperar y enviar por correo electrónico a los usuarios. Si se aplica un algoritmo hash a la contraseña, no se puede recuperar. En su lugar, el usuario deberá realizar un restablecimiento de contraseña.

## <a name="loginstatus-control"></a>LoginStatus Control

El control LoginStatus se utiliza para mostrar un indicador de inicio de sesión a los usuarios que no han iniciado sesión y un indicador de cierre de sesión a los usuarios que han iniciado sesión actualmente. La propiedad Request.IsAuthenticated se utiliza para determinar qué indicador mostrar. El indicador mostrado por el LoginStatus control puede ser texto (implementado a través de la **LoginText** y **LogoutText** propiedades) o imágenes (implementado a través de la **LoginImageUrl** y **LogoutImageUrl** propiedades.)

Cuando un usuario cierra la sesión a través del control LoginStatus, se le redirige a la dirección URL especificada por la propiedad **LogoutPageUrl.** Si no se establece esa propiedad, se actualiza la página actual. Dado que el sitio probablemente está protegido por la autenticación de formularios, la actualización de la página actual redirigirá al usuario a la página de inicio de sesión del sitio.

## <a name="loginname-control"></a>LoginName Control

El control LoginName muestra el nombre de usuario del usuario que ha iniciado sesión actualmente en el sitio.

## <a name="createuserwizard-control"></a>CreateUserWizard Control

El control CreateUserWizard proporciona a los usuarios una forma cómoda de registrarse en el sistema de pertenencia. Puede agregar pasos (implementados como una colección de WizardSteps) a través de la interfaz que se muestra a continuación.

![](membership/_static/image5.jpg)

**Figura 5**

El CreateUserWizard es un control con plantilla que deriva de la Wizard clase y proporciona las siguientes plantillas:

- **HeaderTemplate** Esta plantilla controla la apariencia del encabezado del asistente.
- **SidebarTemplate** Esta plantilla controla el aspecto de la barra lateral del asistente.
- **StartNavigationTemplate** Esta plantilla controla la apariencia de la navegación son del asistente en el paso de inicio.
- **StepNavigationTemplate** Esta plantilla controla la apariencia del área de navegación cuando no está en el paso de inicio o fin.
- **FinishNavigationTemplate** Esta plantilla controla la apariencia del área de navegación cuando se encuentra en el paso de finalización.

Además, para cada paso que agregue al Asistente, ASP.NET creará una plantilla personalizada que contiene ContentTemplate y CustomNavigationTemplate para ese paso. Para obtener información detallada sobre cómo personalizar CreateUserWizard, consulte la documentación de VS.NET 2005:

## <a name="changepassword-control"></a>ChangePassword Control

El control ChangePassword permite a los usuarios cambiar su contraseña. Si el DisplayUserName propiedad es true (es false de forma predeterminada), el usuario puede cambiar su contraseña cuando no han iniciado sesión. Si el usuario ya *ha* iniciado sesión y la propiedad DisplayUserName es true, el usuario podrá cambiar la contraseña de otro usuario que no ha iniciado sesión siempre que conozca el ID de usuario de ese usuario.

Tenga en cuenta que si desea que los usuarios puedan cambiar las contraseñas sin tener que iniciar sesión, deberá asegurarse de que la página en la que se muestra el control ChangePassword permite el acceso anónimo. Obviamente, los usuarios tendrán que proporcionar su contraseña antigua con el fin de cambiar su contraseña.

## <a name="role-management"></a>Administración de roles

La administración de roles le permite asignar usuarios a un rol determinado y, a continuación, restringir el acceso a determinados archivos o carpetas en función de ese rol. La administración de roles también proporciona una API para que pueda determinar mediante programación el rol de alguien o determinar todos los usuarios de un rol determinado y responder en consecuencia.

La administración de roles no es un requisito en ASP.NET pertenencia, ni la pertenencia es un requisito para usar la administración de roles. Sin embargo, los dos se complementan muy bien y es probable que los desarrolladores los utilicen junto sin igual.

Para habilitar la administración de roles en la aplicación, realice el siguiente cambio en el archivo web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Cuando el atributo **cacheRolesInCookie** se establece en true, ASP.NET almacena en caché una pertenencia a un rol de usuario en una cookie en el cliente. Esto permite que las búsquedas de roles se produzcan sin llamadas a RoleProvider. Al usar este atributo, se recomienda a los desarrolladores que se aseguren de que el atributo **cookieProtection** está establecido en All. (Esta es la configuración predeterminada.) Esto garantiza que los datos de las cookies estén cifrados y ayuda a garantizar que el contenido de las cookies no se haya modificado. Los roles se pueden agregar mediante la Herramienta de administración de sitios web. Le permite definir fácilmente roles, configurar el acceso a partes del sitio en función de esos roles y asignar usuarios a roles.

![](membership/_static/image6.jpg)

**Figura 6**

Como se muestra arriba, se pueden agregar nuevos roles simplemente introduciendo el nombre del rol y, a continuación, haciendo clic en Agregar rol. Los roles existentes se pueden administrar o eliminar haciendo clic en el vínculo adecuado de la lista de roles existentes.

Al administrar un rol, puede agregar o quitar usuarios como se muestra a continuación.

![](membership/_static/image7.jpg)

**Figura 7**

Al marcar la casilla de verificación Usuario está en rol, puede agregar fácilmente un usuario a un rol específico. ASP.NET actualizará automáticamente su base de datos de miembros con las entradas adecuadas. También querrá configurar reglas de acceso para la aplicación. ASP.NET desarrolladores 1.x están familiarizados con hacer esto a través del &lt;elemento de autorización&gt; en el archivo web.config, y esa opción todavía está disponible en ASP.NET 2.0. Sin embargo, es más fácil administrar las reglas de acceso mediante la herramienta de administración de sitios web, como se muestra a continuación.

![](membership/_static/image8.jpg)

**Figura 8**

En este caso, se resalta la carpeta Administración (es difícil de ver porque la herramienta la resalta en gris claro) y se ha concedido acceso al rol Administradores. Se deniega nado a todos los demás usuarios. Puede hacer clic en el icono de la cabeza para seleccionar una regla y luego utilizar los botones Subir y Bajar para organizar las reglas. Al igual &lt;que&gt; con el elemento de autorización ASP.NET, las reglas se procesan en el orden en que aparecen. En otras palabras, si se invirtieya el orden de las reglas en la toma anterior, nadie tendría acceso a la carpeta Administración porque la primera regla que ASP.NET encontraría sería la regla que deniega a todos a la carpeta.

ASP.NET 2.0 agrega un archivo web.config a la carpeta para la que está especificando una regla de acceso. Las reglas de acceso se pueden editar a través del archivo de configuración o a través de la Herramienta de administración del sitio web. En otras palabras, la Herramienta de administración de sitios web es simplemente una interfaz a través de la cual el archivo de configuración se puede editar en un entorno fácil de usar.

## <a name="using-roles-in-code"></a>Uso de roles en el código

La API para la administración de roles no ha cambiado desde la versión 1.x. El **IsInRole** método se utiliza para determinar si un usuario está en un rol determinado.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET también crea una instancia de RolePrincipal como miembro del contexto actual. El objeto RolePrincipal se puede utilizar para obtener todos los roles a los que pertenece el usuario de la siguiente manera:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Uso de RoleGroups con el control LoginView

Ahora que tiene una comprensión de la administración de roles y la pertenencia, permite discutir brevemente cómo el control LoginView aprovecha esta capacidad en ASP.NET 2.0. Como se ha explicado anteriormente, el control LoginView es un control con plantilla que contiene dos plantillas de forma predeterminada; El AnonymousTemplate y el LoggedInTemplate. Dentro del cuadro de diálogo Tareas de LoginView hay un vínculo (que se muestra a continuación) que le permite editar RoleGroups.

![](membership/_static/image9.jpg)

**Figura 9**

Cada objeto RoleGroup contiene una matriz de cadenas que define a qué roles se aplica RoleGroup. Para agregar un nuevo RoleGroup al control LoginView, haga clic en el vínculo Editar Grupos de roles. En la imagen anterior, puede ver que he agregado un nuevo RoleGroup para administradores. Al seleccionar ese RoleGroup (RoleGroup[0]) en la lista desplegable Vistas, puedo configurar una plantilla que solo se mostrará a los miembros del rol Administradores. En la imagen siguiente, he agregado un nuevo RoleGroup que se aplica a los miembros del rol Sales y el rol de distribución. Esto agrega un segundo RoleGroup a la vistas desplegable en el inicio de sesión tareas cuadro de diálogo y cualquier cosa agregada a esa plantilla será visible para cualquier usuario en el Sales o Distribución rol.

![](membership/_static/image10.jpg)

**Figura 10**

## <a name="overriding-the-existing-membership-provider"></a>Reemplazar al Proveedor de Membresía Existente

Hay un par de maneras en las que puede ampliar la funcionalidad de ASP.NET pertenencia. En primer lugar, obviamente puede cambiar la funcionalidad existente de la SqlMembershipProvider clase heredando de ella y reemplazando sus métodos. Por ejemplo, si desea implementar su propia funcionalidad cuando se crean usuarios, puede crear su propia clase que hereda de SqlMembershipProvider de la siguiente manera:

[!code-csharp[Main](membership/samples/sample5.cs)]

Si, por otro lado, desea crear su propio proveedor (para almacenar la información de pertenencia en una base de datos de Access, por ejemplo), puede crear su propio proveedor.

## <a name="creating-your-own-membership-provider"></a>Creación de su propio proveedor de membresía

Para crear su propio proveedor de pertenencia, primero tendrá que crear una clase que herede de la MembershipProvider clase. Si usa VB.NET, Visual Studio 2005 agregará los códigos auxiliares para todos los métodos que necesita invalidar. Si está utilizando C, depende de usted para agregar los stubs.

Deberá anular lo siguiente:

- Propiedad ApplicationName
- Función ChangePassword
- Función ChangePasswordQuestionAndAnswer
- Función CreateUser
- Función DeleteUser
- Propiedad EnablePasswordReset
- Propiedad EnablePasswordRetrieval
- Función FindUsersByEmail
- Función FindUsersByName
- Función GetAllUsers
- Función GetNumberOfUsersOnline
- Función GetPassword
- Función GetUser
- Función GetUserNameByEmail
- Propiedad MaxInvalidPasswordAttempts
- Propiedad MinRequiredNonAlphanumericCharacters
- Propiedad MinRequiredPasswordLength
- Propiedad PasswordAttemptWindow
- Propiedad PasswordFormat
- Propiedad PasswordStrengthRegularExpression
- Requierela propiedad QuestionAndAnswer
- Requierela propiedadUniqueEmail
- Función ResetPassword
- Desbloquear función de usuario
- UpdateFunción UpdateUser
- Función ValidateUser

Esa es una lista bastante para implementar como un desarrollador de C. Es posible que le resulte más fácil crear la clase en VB.NET sin ninguna implementación y, a continuación, use .NET Reflector o una herramienta similar para convertir el código a C.

La cadena de conexión y otras propiedades deben establecerse en sus valores predeterminados en el Initialize método. (El Initialize método se desencadena cuando el proveedor se carga en tiempo de ejecución.) El segundo parámetro para el Initialize método es de tipo System.Collections.Specialized.NameValueCollection y es una referencia al elemento &lt;add&gt; que está asociado con el proveedor personalizado en el archivo web.config. Esa entrada tiene el siguiente aspecto:

[!code-xml[Main](membership/samples/sample6.xml)]

Este es un ejemplo de la Initialize método.

[!code-csharp[Main](membership/samples/sample7.cs)]

Para validar al usuario cuando envíe su formulario de inicio de sesión, deberá utilizar el método ValidateUser. Este método se desencadena cuando el usuario hace clic en el botón de inicio de sesión en el control de inicio de sesión. Colocará el código que realiza la búsqueda de usuarios en este método.

Como puede ver, escribir su propio proveedor de membresía no es difícil y le permite ampliar esta poderosa funcionalidad de ASP.NET 2.0.
