---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Aplicaciones seguras que utilizan autenticación y autorización ? Microsoft Docs
author: rick-anderson
description: El paso 9 muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, por lo que los usuarios necesitan registrarse e iniciar sesión en el sitio para crear...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d96f2763f6e62f9dd599fdb7977a97993d218305
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542578"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Proteger las aplicaciones mediante la autenticación y la autorización

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 9 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> El paso 9 muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, por lo que los usuarios necesitan registrarse e iniciar sesión en el sitio para crear nuevas cenas, y solo el usuario que hospeda una cena puede editarla más adelante.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner Paso 9: Autenticación y autorización

En este momento nuestra aplicación NerdDinner otorga a cualquier persona que visite el sitio la capacidad de crear y editar los detalles de cualquier cena. Vamos a cambiar esto para que los usuarios necesiten registrarse e iniciar sesión en el sitio para crear nuevas cenas, y agreguen una restricción para que solo el usuario que hospeda una cena pueda editarla más adelante.

Para habilitar esto usaremos la autenticación y autorización para proteger nuestra aplicación.

### <a name="understanding-authentication-and-authorization"></a>Comprender la autenticación y la autorización

*La autenticación* es el proceso de identificar y validar la identidad de un cliente que accede a una aplicación. En pocas palabras, se trata de identificar "quién" es el usuario final cuando visita un sitio web. ASP.NET admite varias formas de autenticar a los usuarios del explorador. Para las aplicaciones web de Internet, el enfoque de autenticación más común utilizado se denomina "Autenticación de formularios". Autenticación de formularios permite a un desarrollador crear un formulario de inicio de sesión HTML dentro de su aplicación y, a continuación, validar el nombre de usuario/contraseña que un usuario final envía en una base de datos u otro almacén de credenciales de contraseña. Si la combinación de nombre de usuario y contraseña es correcta, el desarrollador puede pedir a ASP.NET emita una cookie HTTP cifrada para identificar al usuario en futuras solicitudes. Usaremos la autenticación de formularios con nuestra aplicación NerdDinner.

*La autorización* es el proceso de determinar si un usuario autenticado tiene permiso para acceder a una dirección URL o recurso determinada o para realizar alguna acción. Por ejemplo, dentro de nuestra aplicación NerdDinner queremos autorizar que solo los usuarios que han iniciado sesión puedan acceder a la dirección URL */Dinners/Create* y crear nuevas cenas. También queremos agregar lógica de autorización para que solo el usuario que hospeda una cena pueda editarla y denegar el acceso de edición a todos los demás usuarios.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticación de formularios y AccountController

La plantilla de proyecto de Visual Studio predeterminada para ASP.NET MVC habilita automáticamente la autenticación de formularios cuando se crean nuevas ASP.NET se crean aplicaciones MVC. También agrega automáticamente una implementación de página de inicio de sesión de cuenta precompilada al proyecto, lo que hace que sea muy fácil integrar la seguridad dentro de un sitio.

La página maestra Site.master predeterminada muestra un vínculo "Iniciar sesión" en la parte superior derecha del sitio cuando el usuario que accede a él no está autenticado:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Al hacer clic en el vínculo "Iniciar sesión", un usuario se encuentra en la dirección URL *de /Account/LogOn:*

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Los visitantes que no se hayan registrado pueden hacerlo haciendo clic en el enlace "Registrarse", que los llevará a la URL */Account/Register* y les permitirá introducir los detalles de la cuenta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Al hacer clic en el botón "Registrarse" se creará un nuevo usuario dentro del sistema de pertenencia ASP.NET y autenticar al usuario en el sitio mediante la autenticación de formularios.

Cuando un usuario inicia sesión, Site.master cambia la parte superior derecha de la página para generar un "Bienvenido [nombre de usuario]!" mensaje y representa un enlace "Cerrar sesión" en lugar de uno "Iniciar sesión". Al hacer clic en el enlace "Cerrar sesión" se cierra la sesión del usuario:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funcionalidad de inicio de sesión, cierre de sesión y registro anterior se implementa dentro de la clase AccountController que Visual Studio agregó a nuestro proyecto cuando creó el proyecto. La interfaz de usuario para el AccountController se implementa mediante plantillas de vista dentro del directorio de la cuenta de views:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La clase AccountController utiliza el sistema de autenticación de formularios ASP.NET para emitir cookies de autenticación cifradas y la API de pertenencia ASP.NET para almacenar y validar nombres de usuario/contraseñas. La API de pertenencia ASP.NET es extensible y permite usar cualquier almacén de credenciales de contraseña. ASP.NET se incluye con implementaciones de proveedor de pertenencia integradas que almacenan nombres de usuario/contraseñas dentro de una base de datos SQL o dentro de Active Directory.

Podemos configurar qué proveedor de pertenencia debe usar nuestra aplicación NerdDinner abriendo el archivo "web.config" en la raíz del proyecto y buscando la &lt;sección de pertenencia&gt; dentro de él. El archivo web.config predeterminado agregado cuando se creó el proyecto registra el proveedor de pertenencia SQL y lo configura para usar una cadena de conexión denominada "ApplicationServices" para especificar la ubicación de la base de datos.

La cadena de conexión predeterminada "ApplicationServices" &lt;(que&gt; se especifica en la sección connectionStrings del archivo web.config) está configurada para usar SQL Express. Señala a una base de datos de SQL Express denominada "ASPNETDB. MDF" en el directorio\_"Datos de la aplicación" de la aplicación. Si esta base de datos no existe la primera vez que se usa la API de pertenencia dentro de la aplicación, ASP.NET creará automáticamente la base de datos y aprovisionará el esquema de base de datos de pertenencia adecuado dentro de ella:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si en lugar de usar SQL Express queríamos usar una instancia completa de SQL Server (o conectarnos a una base de datos remota), todo lo que tendríamos que hacer es actualizar la cadena de conexión "ApplicationServices" dentro del archivo web.config y asegurarnos de que se ha agregado el esquema de pertenencia adecuado a la base de datos a la que apunta. Puede ejecutar la utilidad\_"aspnet regsql.exe" en el ASP.NET directorio .

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorización de la dirección URL /Dinners/Create mediante el filtro [Authorize]

No tuvimos que escribir ningún código para habilitar una implementación segura de autenticación y administración de cuentas para la aplicación NerdDinner. Los usuarios pueden registrar nuevas cuentas con nuestra aplicación e iniciar sesión/cerrar sesión en el sitio.

Ahora podemos agregar lógica de autorización a la aplicación y usar el estado de autenticación y el nombre de usuario de los visitantes para controlar lo que pueden y no pueden hacer dentro del sitio. Comencemos agregando lógica de autorización a los métodos de acción "Crear" de nuestra clase DinnersController. Específicamente, necesitaremos que los usuarios que accedan a la dirección URL */Dinners/Create* deben iniciar sesión. Si no han iniciado sesión, los redirigiremos a la página de inicio de sesión para que puedan iniciar sesión.

Implementar esta lógica es bastante fácil. Todo lo que necesitamos hacer es agregar un atributo de filtro [Authorize] a nuestros métodos de acción Create de esta forma:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC admite la capacidad de crear "filtros de acción" que se pueden usar para implementar lógica reutilizable que se puede aplicar mediante declaración a los métodos de acción. El filtro [Authorize] es uno de los filtros de acción integrados proporcionados por ASP.NET MVC y permite a un desarrollador aplicar mediante declaración reglas de autorización a métodos de acción y clases de controlador.

Cuando se aplica sin ningún parámetro (como el anterior) el filtro [Authorize] exige que el usuario que realiza la solicitud de método de acción debe iniciar sesión, y redirigirá automáticamente el explorador a la dirección URL de inicio de sesión si no lo está. Al hacer esta redirección, la dirección URL solicitada originalmente se pasa como argumento de cadena de consulta (por ejemplo: /Account/LogOn? ReturnUrl-%2fDinners%2fCreate). A continuación, AccountController redirigirá al usuario a la dirección URL solicitada originalmente una vez que inicie sesión.

El filtro [Autorizar] admite opcionalmente la capacidad de especificar una propiedad "Usuarios" o "Roles" que se puede usar para requerir que el usuario haya iniciado sesión y esté dentro de una lista de usuarios permitidos o un miembro de un rol de seguridad permitido. Por ejemplo, el código siguiente solo permite que dos usuarios específicos, "scottgu" y "billg", accedan a la DIRECCIÓN URL /Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Incrustar nombres de usuario específicos en el código tiende a ser bastante inmantenible sin embargo. Un mejor enfoque consiste en definir "roles" de nivel superior con los que se comprueba el código y, a continuación, asignar a los usuarios al rol mediante una base de datos o un sistema de directorio activo (lo que permite que la lista de asignación de usuarios real se almacene externamente desde el código). ASP.NET incluye una API de administración de roles integrada, así como un conjunto integrado de proveedores de roles (incluidos los de SQL y Active Directory) que pueden ayudar a realizar esta asignación de usuario/rol. A continuación, podríamos actualizar el código para permitir que solo los usuarios dentro de un rol específico de "administrador" accedan a la dirección URL /Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Uso de la propiedad User.Identity.Name al crear cenas

Podemos recuperar el nombre de usuario del usuario que ha iniciado sesión actualmente de una solicitud mediante la propiedad User.Identity.Name expuesta en la clase base Controller.

Anteriormente, cuando implementamos la versión HTTP-POST de nuestro método de acción Create(), habíamos codificado de forma rígida la propiedad "HostedBy" de Dinner en una cadena estática. Ahora podemos actualizar este código para usar en su lugar la propiedad User.Identity.Name, así como agregar automáticamente un RSVP para el host que crea la cena:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Dado que hemos agregado un atributo [Authorize] al método Create(), ASP.NET MVC garantiza que el método action solo se ejecuta si el usuario que visita la dirección URL /Dinners/Create ha iniciado sesión en el sitio. Como tal, el valor de propiedad User.Identity.Name siempre contendrá un nombre de usuario válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Uso de la propiedad User.Identity.Name al editar cenas

Ahora vamos a agregar alguna lógica de autorización que restrinja a los usuarios para que solo puedan editar las propiedades de las cenas que ellos mismos hospedan.

Para ayudar con esto, primero agregaremos un método auxiliar "IsHostedBy(username)" a nuestro objeto Dinner (dentro de la clase Dinner.cs parcial que creamos anteriormente). Este método auxiliar devuelve true o false dependiendo de si un nombre de usuario proporcionado coincide con la propiedad Dinner HostedBy y encapsula la lógica necesaria para realizar una comparación de cadenas sin distinción entre mayúsculas y minúsculas de ellos:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

A continuación, agregaremos un atributo [Authorize] a los métodos de acción Edit() dentro de nuestra clase DinnersController. Esto garantizará que los usuarios deben iniciar sesión para solicitar una dirección URL */Dinners/Edit/[id].*

A continuación, podemos agregar código a nuestros métodos Edit que usa el método auxiliar Dinner.IsHostedBy(username) para comprobar que el usuario que ha iniciado sesión coincide con el host Dinner. Si el usuario no es el host, mostraremos una vista "InvalidOwner" y terminaremos la solicitud. El código para hacer esto se ve como a continuación:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

A continuación, podemos hacer clic con el botón derecho&gt;en el directorio "Vistas" y elegir el comando de menú Agregar- Vista para crear una nueva vista "InvalidOwner". Lo rellenaremos con el siguiente mensaje de error:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Y ahora, cuando un usuario intenta editar una cena que no es de su propiedad, recibirá un mensaje de error:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Podemos repetir los mismos pasos para los métodos de acción Delete() dentro de nuestro controlador para bloquear el permiso para eliminar cenas también, y asegurarnos de que solo el host de una cena puede eliminarlo.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrar/Ocultar Editar y Eliminar enlaces

Estamos vinculando al método de acción Editar y eliminar de nuestra clase DinnersController desde nuestra URL de detalles:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actualmente estamos mostrando los vínculos de acción Editar y Eliminar, independientemente de si el visitante de la URL de detalles es el anfitrión de la cena. Vamos a cambiar esto para que los enlaces solo se muestren si el usuario visitante es el propietario de la cena.

El método de acción Details() dentro de nuestro DinnersController recupera un objeto Dinner y, a continuación, lo pasa como el objeto de modelo a nuestra plantilla de vista:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos actualizar nuestra plantilla de vista para mostrar/ocultar condicionalmente los enlaces Editar y Eliminar utilizando el método auxiliar Dinner.IsHostedBy() como se muestra a continuación:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Pasos siguientes

Veamos ahora cómo podemos habilitar a los usuarios autenticados a RSVP para las cenas con AJAX.

> [!div class="step-by-step"]
> [Anterior](implement-efficient-data-paging.md)
> [Siguiente](use-ajax-to-deliver-dynamic-updates.md)
