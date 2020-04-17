---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Autenticación de usuarios con autenticación de Windows (VB) Microsoft Docs
author: rick-anderson
description: Aprenda a usar la autenticación de Windows en el contexto de una aplicación MVC. Aprenderá a habilitar la autenticación de Windows dentro de la aplicación co...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 446dcc338f61e1f76478c1085773e7ad089c73f4
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540602"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Autenticar a los usuarios con la autenticación de Windows (VB)

por [Microsoft](https://github.com/microsoft)

> Aprenda a usar la autenticación de Windows en el contexto de una aplicación MVC. Aprenderá a habilitar la autenticación de Windows en el archivo de configuración web de la aplicación y a configurar la autenticación con IIS. Por último, aprenderá a usar el atributo [Authorize] para restringir el acceso a las acciones del controlador a determinados usuarios o grupos de Windows.

El objetivo de este tutorial es explicar cómo puede aprovechar las características de seguridad integradas en Internet Information Services para proteger con contraseña las vistas en las aplicaciones MVC. Aprenderá a permitir que las acciones del controlador sean invocadas solo por usuarios de Windows concretos o usuarios que sean miembros de grupos de Windows concretos.

El uso de la autenticación de Windows tiene sentido cuando se crea un sitio web interno de la empresa (un sitio de intranet) y desea que los usuarios puedan usar sus nombres de usuario y contraseñas estándar de Windows al acceder al sitio web. Si está creando un sitio web orientado hacia el exterior (un sitio web de Internet) considere el uso de la autenticación de formularios en su lugar.

#### <a name="enabling-windows-authentication"></a>Habilitación de la autenticación de Windows

Cuando se crea una nueva aplicación ASP.NET MVC, la autenticación de Windows no está habilitada de forma predeterminada. Autenticación de formularios es el tipo de autenticación predeterminado habilitado para aplicaciones MVC. Debe habilitar la autenticación de Windows modificando el archivo de configuración web (web.config) de la aplicación MVC. Busque &lt;la&gt; sección de autenticación y modifíquela para usar Windows en lugar de la autenticación de formularios como esta:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Al habilitar la autenticación de Windows, el servidor web se hace responsable de autenticar a los usuarios. Normalmente, hay dos tipos diferentes de servidores web que se usan al crear e implementar una aplicación MVC ASP.NET.

En primer lugar, al desarrollar una aplicación MVC, use el servidor web de desarrollo de ASP.NET incluido con Visual Studio. De forma predeterminada, el servidor web de desarrollo de ASP.NET ejecuta todas las páginas en el contexto de la cuenta actual de Windows (sea cual sea la cuenta que usó para iniciar sesión en Windows).

El servidor web de desarrollo de ASP.NET también admite la autenticación NTLM. Puede habilitar la autenticación NTLM haciendo clic con el botón secundario en el nombre del proyecto en la ventana Explorador de soluciones y seleccionando Propiedades. Después, seleccione la lengueta Web y marque la casilla de verificación NTLM (véase el cuadro 1).

**Figura 1 – Habilitación de la autenticación NTLM para el servidor web de desarrollo de ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Para una aplicación web de producción, por el lado, use IIS como servidor web. IIS admite varios tipos de autenticación, entre ellos:

- Autenticación básica: se define como parte del protocolo HTTP 1.0. Envía nombres de usuario y contraseñas en texto no cifrado (codificado en Base64) a través de Internet. - Autenticación implícita – Envía un hash de una contraseña, en lugar de la propia contraseña, a través de Internet. - Autenticación integrada de Windows (NTLM): el mejor tipo de autenticación para usar en entornos de intranet mediante Windows. - Autenticación de certificados: habilita la autenticación mediante un certificado del lado cliente. El certificado se asigna a una cuenta de usuario de Windows.

> [!NOTE] 
> 
> Para obtener una descripción más detallada [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx)de estos diferentes tipos de autenticación, consulte .

Puede usar el Administrador de Internet Information Services para habilitar un tipo determinado de autenticación. Tenga en cuenta que todos los tipos de autenticación no están disponibles en el caso de todos los sistemas operativos. Además, si usa IIS 7.0 con Windows Vista, deberá habilitar los diferentes tipos de autenticación de Windows antes de que aparezcan en el Administrador de Internet Information Services. Abra el Panel de **control, Programas, programas y características, Active o desactive**las características de Windows y expanda el nodo Servicios de Internet Information Server (consulte la figura 2).

**Figura 2 – Habilitación de las características de IIS de Windows**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Con Internet Information Services, puede habilitar o deshabilitar diferentes tipos de autenticación. Por ejemplo, la Figura 3 ilustra cómo deshabilitar la autenticación anónima y habilitar la autenticación integrada de Windows (NTLM) al usar IIS 7.0.

**Figura 3 – Habilitación de la autenticación integrada de Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorización de usuarios y grupos de Windows

Después de habilitar la autenticación &lt;&gt; de Windows, puede usar el atributo Authorize para controlar el acceso a controladores o acciones de controlador. Este atributo se puede aplicar a un controlador MVC completo o a una acción de controlador determinada.

Por ejemplo, el controlador Home del listado 1 expone tres acciones denominadas Index(), CompanySecrets() y StephenSecrets(). Cualquiera puede invocar la acción Index(). Sin embargo, solo los miembros del grupo Administradores locales de Windows pueden invocar la acción CompanySecrets(). Por último, solo el usuario de dominio de Windows llamado Stephen (en el dominio redmond) puede invocar la acción StephenSecrets().

**Listado 1 – Controllers-HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Debido al Control de cuentas de usuario (UAC) de Windows, al trabajar con Windows Vista o Windows Server 2008, el grupo administradores local se comportará de forma diferente que otros grupos. El &lt;&gt; atributo Authorize no reconocerá correctamente a un miembro del grupo Administradores local a menos que modifique la configuración de UAC del equipo.

Exactamente lo que sucede cuando intenta invocar una acción de controlador sin ser los permisos correctos depende del tipo de autenticación habilitada. De forma predeterminada, al usar el ASP.NET servidor de desarrollo, simplemente obtendrá una página en blanco. La página se sirve con un estado de respuesta HTTP **no autorizado 401.**

Si, por otro lado, usa IIS con autenticación anónima deshabilitada y autenticación básica habilitada, seguirá recibiendo un mensaje de diálogo de inicio de sesión cada vez que solicite la página protegida (consulte la figura 4).

**Figura 4 – Diálogo de inicio de sesión de autenticación básica**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Resumen

En este tutorial se explica cómo puede usar la autenticación de Windows en el contexto de una aplicación ASP.NET MVC. Ha aprendido a habilitar la autenticación de Windows en el archivo de configuración web de la aplicación y a configurar la autenticación con IIS. Por último, aprendió &lt;a&gt; usar el atributo Authorize para restringir el acceso a las acciones del controlador a determinados usuarios o grupos de Windows.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-forms-authentication-vb.md)
> [Siguiente](preventing-javascript-injection-attacks-vb.md)
