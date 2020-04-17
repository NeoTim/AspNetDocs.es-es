---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Autenticación de usuarios con autenticación de formularios (C-) Microsoft Docs
author: rick-anderson
description: Aprenda a usar el atributo [Authorize] para proteger con contraseña determinadas páginas de la aplicación MVC. También aprenderá a usar la administración del sitio web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: f14f996c0b3a438647b5d181675457735e473354
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540979"
---
# <a name="authenticating-users-with-forms-authentication-c"></a>Autenticar a los usuarios con la autenticación de formularios (C#)

por [Microsoft](https://github.com/microsoft)

> Aprenda a usar el atributo [Authorize] para proteger con contraseña determinadas páginas de la aplicación MVC. Aprenderá a usar la herramienta de administración de sitios web para crear y administrar usuarios y roles. También aprenderá a configurar dónde se almacena la información de la cuenta de usuario y del rol.

El objetivo de este tutorial es explicar cómo puede usar la autenticación de formularios para proteger con contraseña las vistas en las aplicaciones MVC ASP.NET. Aprenderá a usar la herramienta de administración de sitios web para crear usuarios y roles. También aprenderá a evitar que los usuarios no autorizados invoquen acciones de controlador. Por último, aprenderá a configurar dónde se almacenan los nombres de usuario y las contraseñas.

#### <a name="using-the-web-site-administration-tool"></a>Uso de la herramienta de administración del sitio web

Antes de hacer cualquier otra cosa, debemos empezar por crear algunos usuarios y roles. La forma más fácil de crear nuevos usuarios y roles es aprovechar la herramienta de administración del sitio web de Visual Studio 2008. Puede iniciar esta herramienta seleccionando la opción de menú **Proyecto, ASP.NET Configuración**. Como alternativa, puede iniciar la herramienta de administración de sitios web haciendo clic en el icono (algo aterrador) del martillo que golpea el mundo que aparece en la parte superior de la ventana del Explorador de soluciones (consulte la figura 1).

**Figura 1 – Lanzamiento de la herramienta de administración de sitios web**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

En la Herramienta de administración de sitios web, puede crear **Create user** nuevos usuarios y roles seleccionando la ficha Seguridad. Proporcione al usuario Stephen cualquier contraseña que desee (por ejemplo, *secreta).*

**Figura 2 – Creación de un nuevo usuario**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Puede crear nuevos roles habilitando primero roles y definiendo uno o varios roles. Habilite los roles haciendo clic en el vínculo **Habilitar roles.** A continuación, cree un rol denominado *Administradores* haciendo clic en el vínculo **Crear o administrar roles** (consulte la figura 3).

**Figura 3 – Creación de un nuevo rol**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Por último, cree un nuevo usuario denominado Sally y asocie Sally con el rol Administradores haciendo clic en el vínculo Crear usuario y seleccionando Administradores al crear Sally (consulte la figura 4).

**Figura 4 – Agregar un usuario a un rol**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Cuando todo está dicho y hecho, usted debe tener dos nuevos usuarios llamados Stephen y Sally. También debe tener un nuevo rol denominado Administradores. Sally es miembro del rol Administradores y Stephen no.

#### <a name="requiring-authorization"></a>Exigir autorización

Puede requerir que un usuario se autentique antes de que el usuario invoque una acción de controlador agregando el atributo [Authorize] a la acción. Puede aplicar el atributo [Authorize] a una acción de controlador individual o puede aplicar este atributo a una clase de controlador completa.

Por ejemplo, el controlador del listado 1 expone una acción denominada CompanySecrets(). Dado que esta acción está decorada con el atributo [Authorize], esta acción no se puede invocar a menos que se autentique un usuario.

**Listado 1 – Controllers-HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Si invoca la acción CompanySecrets() introduciendo la dirección URL /Home/CompanySecrets en la barra de direcciones de su navegador y no es un usuario autenticado, se le redirigirá automáticamente a la vista Inicio de sesión (consulte la figura 5).

**Figura 5 – La vista de inicio de sesión**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Puede utilizar la vista Inicio de sesión para introducir su nombre de usuario y contraseña. Si usted no es un usuario registrado entonces usted puede hacer clic el link del **registro** para navegar a la vista del registro (véase el cuadro 6). Puede usar la vista Registrar para crear una nueva cuenta de usuario.

**Figura 6 – La vista Registro**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Después de iniciar sesión correctamente, puede ver la vista CompanySecrets (consulte la figura 7). De forma predeterminada, seguirá sesión hasta que cierre la ventana del navegador.

**Figura 7 – La vista CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorización por nombre de usuario o rol de usuario

Puede usar el atributo [Authorize] para restringir el acceso a una acción de controlador a un conjunto determinado de usuarios o a un conjunto determinado de roles de usuario. Por ejemplo, el controlador Home modificado en el listado 2 contiene dos nuevas acciones denominadas StephenSecrets() y AdministratorSecrets().

**Listado 2 – Controllers-HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Solo un usuario con el nombre de usuario Stephen puede invocar la acción StephenSecrets(). Todos los demás usuarios se redirigen a la vista de inicio de sesión. La propiedad Users acepta una lista separada por comas de nombres de cuenta de usuario.

Solo los usuarios del rol Administradores pueden invocar la acción AdministratorSecrets(). Por ejemplo, dado que Sally es miembro del grupo Administradores, puede invocar la acción AdministratorSecrets(). Todos los demás usuarios se redirigen a la vista de inicio de sesión. La propiedad Roles acepta una lista separada por comas de nombres de rol.

#### <a name="configuring-authentication"></a>Configuración de la autenticación

En este punto, es posible que se pregunte dónde se almacena la información de la cuenta de usuario y del rol. De forma predeterminada, la información se almacena en una base de datos SQL Express (RANU) denominada ASPNETDB.mdf ubicada en la carpeta App\_Data de la aplicación MVC. Esta base de datos se genera automáticamente por el marco de trabajo de ASP.NET cuando se empieza a usar la pertenencia.

Para ver la base de datos ASPNETDB.mdf en la ventana Explorador de soluciones, primero debe seleccionar la opción de menú Proyecto, Mostrar todos los archivos.

El uso de la base de datos SQL Express predeterminada está bien al desarrollar una aplicación. Sin embargo, lo más probable es que no desee usar la base de datos ASPNETDB.mdf predeterminada para una aplicación de producción. En ese caso, puede cambiar dónde se almacena la información de la cuenta de usuario completando los dos pasos siguientes:

1. Agregar los objetos de base de datos de Application Services a la base de datos de producción: cambie la cadena de conexión de la aplicación para que apunte a la base de datos de producción

El primer paso es agregar todos los objetos de base de datos necesarios (tablas y procedimientos almacenados) a la base de datos de producción. La forma más fácil de agregar estos objetos a una nueva base de datos es aprovechar las ventajas del Asistente para la instalación de ASP.NET SQL Server (consulte la figura 8). Puede iniciar esta herramienta abriendo el símbolo del sistema de Visual Studio 2008 desde el grupo de programas de Microsoft Visual Studio 2008 y ejecutando el siguiente comando desde el símbolo del sistema:

aspnet\_regsql

**Figura 8: asistente de instalación de ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

El Asistente para la instalación de SQL ServerSQL Server ASP.NET permite seleccionar una base de datos de SQL Server en la red e instalar todos los objetos de base de datos requeridos por los servicios de aplicación ASP.NET. No es necesario que el servidor de base de datos se encuentre en el equipo local.

> [!NOTE] 
> 
> Si no desea utilizar el Asistente para la instalación de SQL Serversql Server ASP.NET, puede encontrar scripts SQL para agregar los objetos de base de datos de servicios de aplicación en la carpeta siguiente:
> 
> > C:-Windows-Microsoft.NET-Framework-v2.0.50727

Después de crear los objetos de base de datos necesarios, debe modificar la conexión de base de datos utilizada por la aplicación MVC. Modifique la cadena de conexión ApplicationServices en el archivo de configuración web (web.config) para que apunte a la base de datos de producción. Por ejemplo, la conexión modificada en el listado 3 apunta a una base de datos denominada MyProductionDB (se ha comentado la cadena de conexión original de ApplicationServices).

**Listado 3 – Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configuración de permisos de base de datos

Si utiliza Integrated Security para conectarse a la base de datos, deberá agregar la cuenta de usuario de Windows correcta como inicio de sesión a la base de datos. La cuenta correcta depende de si está utilizando el servidor de desarrollo de ASP.NET o Internet Information Services como servidor web. La cuenta de usuario correcta también depende de su sistema operativo.

Si usa el servidor de desarrollo de ASP.NET (el servidor web predeterminado utilizado por Visual Studio), la aplicación se ejecuta en el contexto de su cuenta de usuario de Windows. En ese caso, debe agregar su cuenta de usuario de Windows como inicio de sesión del servidor de base de datos.

Como alternativa, si usa Internet Information Services, debe agregar la cuenta ASPNET o la cuenta NT AUTHORITY/NETWORK SERVICE como inicio de sesión del servidor de base de datos. Si usa Windows XP, agregue la cuenta ASPNET como inicio de sesión a la base de datos. Si utiliza un sistema operativo más reciente, como Windows Vista o Windows Server 2008, agregue la cuenta NT AUTHORITY/NETWORK SERVICE como inicio de sesión de base de datos.

Puede agregar una nueva cuenta de usuario a la base de datos mediante Microsoft SQL Server Management Studio (consulte la figura 9).

**Figura 9 – Creación de un nuevo inicio de sesión de Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Después de crear el inicio de sesión necesario, debe asignar el inicio de sesión a un usuario de base de datos con los roles de base de datos adecuados. Haga doble clic en el inicio de sesión y seleccione la pestaña Asignación de usuarios. Por ejemplo, para autenticar a los usuarios,\_\_debe habilitar el rol de base de datos aspnet Membership BasicAccess. Para crear nuevos usuarios, debe habilitar\_el\_rol de base de datos FullAccess de pertenencia a aspnet (consulte la figura 10).

**Figura 10 – Adición de roles de base de datos de Application Services**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Resumen

En este tutorial, aprendió a usar la autenticación de formularios al compilar una aplicación mvc ASP.NET. En primer lugar, aprendió a crear nuevos usuarios y roles aprovechando la herramienta de administración de sitios web. A continuación, aprendió a usar el atributo [Authorize] para evitar que los usuarios no autorizados invoquen acciones de controlador. Por último, aprendió a configurar la aplicación MVC para almacenar información de usuario y rol en una base de datos de producción.

> [!div class="step-by-step"]
> [Siguiente](authenticating-users-with-windows-authentication-cs.md)
