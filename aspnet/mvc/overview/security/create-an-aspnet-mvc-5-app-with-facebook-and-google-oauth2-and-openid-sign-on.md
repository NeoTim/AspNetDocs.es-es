---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Crear MVC 5 App con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C) Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo crear una aplicación web ASP.NET MVC 5 que permite a los usuarios iniciar sesión con OAuth 2.0 con credenciales de un authenti externo...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675920"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Crear una aplicación de ASP.NET MVC 5 con el inicio de sesión OAuth2 de Facebook, Twitter, LinkedIn y Google (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> En este tutorial se muestra cómo crear una aplicación web ASP.NET MVC 5 que permite a los usuarios iniciar sesión con [OAuth 2.0](http://oauth.net/2/) con credenciales de un proveedor de autenticación externo, como Facebook, Twitter, LinkedIn, Microsoft o Google. Para simplificar, este tutorial se centra en trabajar con credenciales de Facebook y Google.
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja significativa porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden estar más inclinados a registrarse en su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales.
> 
> Vea también [ASP.NET aplicación MVC 5 con SMS y correo electrónico Autenticación de dos factores](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> El tutorial también muestra cómo agregar datos de perfil para el usuario y cómo usar la API de pertenencia para agregar roles. Este tutorial fue escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) [@RickAndMSFT](https://twitter.com/RickAndMSFT) ( Por favor sígueme en Twitter: ).

<a id="start"></a>
## <a name="getting-started"></a>Introducción

Comience instalando y ejecutando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o Visual Studio [2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior. Para obtener ayuda con Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! y más, consulte este proyecto de [ejemplo.](https://github.com/matthewdunsdon/oauthforaspnet)

> [!NOTE]
> Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior para usar Google OAuth 2 y depurar localmente sin advertencias SSL.

Haga clic en **Nuevo proyecto** en la página **Inicio,** o puede usar el menú y seleccionar **Archivo**y, a continuación, **Nuevo proyecto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Creación de su primera aplicación

Haga clic en **Nuevo proyecto**y, a continuación, seleccione **Visual C.** a la izquierda y, a continuación, **Web** y, a continuación, seleccione ASP.NET **Aplicación web**. Asigne al proyecto el nombre "MvcAuth" y, a continuación, haga clic en **Aceptar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

En el cuadro de diálogo **Nuevo ASP.NET proyecto** , haga clic en **MVC**. Si la Autenticación no es **Cuentas de usuario individuales**, haga clic en el botón **Cambiar autenticación** y seleccione **Cuentas de usuario individuales**. Al comprobar **Host en la nube**, la aplicación será muy fácil de hospedar en Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si ha seleccionado **Host en la nube**, complete el cuadro de diálogo de configuración.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Use NuGet para actualizar al middleware OWIN más reciente

Utilice el administrador de paquetes NuGet para actualizar el [middleware OWIN.](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md) Seleccione **Actualizaciones** en el menú de la izquierda. Puede hacer clic en el botón **Actualizar todo** o puede buscar solo paquetes OWIN (mostrados en la siguiente imagen):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

En la imagen de abajo, solamente se muestran los paquetes OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Desde la consola del Administrador de paquetes (PMC), puede escribir el `Update-Package` comando, que actualizará todos los paquetes.

Presione **F5** o **Ctrl+F5** para ejecutar la aplicación. En la imagen de abajo, el número de puerto es 1234. Cuando ejecute la aplicación, verá un número de puerto diferente.

Dependiendo del tamaño de la ventana del navegador, es posible que deba hacer clic en el icono de navegación para ver los vínculos **Inicio**, **Acerca de**, **Contacto**, **Registrar** e **Iniciar sesión.**

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configuración de SSL en el proyecto

Para conectarse a proveedores de autenticación como Google y Facebook, deberá configurar IIS-Express para usar SSL. Es importante seguir usando SSL después de iniciar sesión y no volver a HTTP, su cookie de inicio de sesión es tan secreta como su nombre de usuario y contraseña, y sin usar SSL lo está enviando en texto no cifrado a través de la conexión. Además, ya se ha tomado el tiempo para realizar el protocolo de enlace y proteger el canal (que es la mayor parte de lo que hace que HTTPS sea más lento que HTTP) antes de ejecutar la canalización MVC, por lo que redirigir de nuevo a HTTP después de que haya iniciado sesión no hará que la solicitud actual o las solicitudes futuras sean mucho más rápidas.

1. En el **Explorador de soluciones**, haga clic en el proyecto **MvcAuth.**
2. Pulse la tecla F4 para mostrar las propiedades del proyecto. Como alternativa, en el menú **Ver** puede seleccionar **Ventana Propiedades**.
3. Cambie **SSL habilitado** a True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copie la URL SSL `https://localhost:44300/` (que será a menos que haya creado otros proyectos SSL).
5. En **el Explorador**de soluciones , haga clic con el botón derecho en el proyecto **MvcAuth** y seleccione **Propiedades**.
6. Seleccione la pestaña **Web** y, a continuación, pegue la dirección URL SSL en el cuadro Dirección URL del **proyecto.** Guarde el archivo (Ctl+S). Necesitarás esta URL para configurar las aplicaciones de autenticación de Facebook y Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Agregue el atributo [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) al `Home` controlador para requerir que todas las solicitudes deben usar HTTPS. Un enfoque más seguro es agregar el filtro [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) a la aplicación. Consulte la &quot;sección Proteger la aplicación&quot; con SSL y el atributo Authorize en mi tutorial [Crear una aplicación MVC ASP.NET con auth y SQL DB e implementar](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)en Azure App Service . Una parte del controlador de inicio se muestra a continuación.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Presione CTRL+F5 para ejecutar la aplicación. Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y saltar a Creación de una aplicación de [Google para OAuth 2 y conectar la aplicación al proyecto,](#goog)de lo contrario, siga las instrucciones para confiar en el certificado autofirmado que IIS Express ha generado.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lea el cuadro de diálogo Advertencia de **seguridad** y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE muestra la página *Inicio* , sin ninguna advertencia de SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome también acepta el certificado y mostrará contenido HTTPS sin previo aviso. Firefox utiliza su propio almacén de certificados, por lo que mostrará una advertencia. Para nuestra aplicación puede hacer clic de forma segura en **Entender los riesgos**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto

> [!WARNING]
> Para obtener instrucciones actuales de Google OAuth, consulte Configuración de la autenticación de [Google en ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Navegue a la [consola de desarrolladores de Google](https://console.developers.google.com/).
2. Si no ha creado un proyecto antes, seleccione **Credenciales** en la pestaña izquierda y, a continuación, seleccione **Crear**.
3. En la pestaña izquierda, haga clic en **Credenciales**.
4. Haga clic en **Crear credenciales y,** a continuación, en ID de cliente de **OAuth**. 

    1. En el cuadro de diálogo Crear ID de **cliente,** mantenga la **aplicación web** predeterminada para el tipo de aplicación.
    2. Establezca los orígenes **de JavaScript autorizados** `https://localhost:44300/` en la dirección URL SSL que utilizó anteriormente (a menos que haya creado otros proyectos SSL)
    3. Establezca el URI de **redireccionamiento autorizado** en:  
         `https://localhost:44300/signin-google`
5. Haga clic en el elemento de menú de la pantalla OAuth Consent y, a continuación, establezca su dirección de correo electrónico y el nombre del producto. Cuando haya completado el formulario, haga clic en **Guardar**.
6. Haga clic en el elemento de menú Biblioteca, busque en la API de **Google+,** haga clic en él y, a continuación, pulse Habilitar.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   La imagen siguiente muestra las API habilitadas.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. En el Administrador de API de API de API de Google, visite la pestaña **Credenciales** para obtener el ID de **cliente.** Descargar para guardar un archivo JSON con secretos de aplicación. Copie y pegue **ClientId** y `UseGoogleAuthentication` **ClientSecret** en el método que se encuentra en el archivo *Startup.Auth.cs* de la carpeta *App_Start.* Los valores **ClientId** y **ClientSecret** que se muestran a continuación son ejemplos y no funcionan.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Seguridad: nunca almacene datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para mantener el ejemplo simple. Consulte [Prácticas recomendadas para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service.](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
8. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el vínculo **Iniciar sesión** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. En **Usar otro servicio para iniciar sesión**, haga clic en **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si se pierde cualquiera de los pasos anteriores, obtendrá un error HTTP 401. Vuelva a comprobar los pasos anteriores. Si olvida una configuración obligatoria (por ejemplo, el nombre del **producto),** agregue el elemento que falta y guárdelo; la autenticación puede tardar unos minutos en funcionar.
10. Se le redirigirá al sitio de Google donde introducirá sus credenciales.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Una vez que haya proporcionado sus credenciales, el sistema le pedirá que le dé permisos a la aplicación web que acaba de crear: 
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Haga clic en **Aceptar**. Ahora se le redirigirá de nuevo a la página **Registrar** de la aplicación MvcAuth, donde podrá registrar su cuenta de Google. Tiene la opción de cambiar el nombre de registro de correo electrónico local que usa para su cuenta de Gmail, pero suele ser más práctico mantener el mismo alias de correo electrónico predeterminado (es decir, el que usó para la autenticación). Haga clic en **Registrar**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Crear la aplicación en Facebook y conectar la aplicación al proyecto

> [!WARNING]
> Para ver las instrucciones de autenticación actuales de Facebook OAuth2, consulta [Configuración de la autenticación de Facebook](/aspnet/core/security/authentication/social/facebook-logins)

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examinar los datos de membresía

En el menú **Ver** , haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expanda **DefaultConnection (MvcAuth)**, expanda **Tablas**, haga clic con el botón derecho en **AspNetUsers** y haga clic en **Mostrar datos**de tabla .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de tabla aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Adición de datos de perfil a la clase de usuario

En esta sección agregará la fecha de nacimiento y la ciudad natal a los datos del usuario durante el registro, como se muestra en la siguiente imagen.

![reg con la ciudad natal y Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Abra el archivo *Models-IdentityModels.cs* y agregue la fecha de nacimiento y las propiedades de la ciudad natal:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Abra el archivo *Models-AccountViewModels.cs* y la fecha `ExternalLoginConfirmationViewModel`de nacimiento establecida y las propiedades de la ciudad natal en .

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Abra el archivo *Controllers-AccountController.cs* y agregue el código `ExternalLoginConfirmation` para la fecha de nacimiento y la ciudad natal en el método de acción como se muestra:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Agregue la fecha de nacimiento y la ciudad natal al archivo *Views-Account-ExternalLoginConfirmation.cshtml:*

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Elimine la base de datos de miembros para que pueda registrar de nuevo su cuenta de Facebook con su aplicación y verificar que puede agregar la nueva fecha de nacimiento y la información del perfil de la ciudad natal.

En **el Explorador**de soluciones , haga clic en el icono Mostrar todos los **archivos** y, a continuación, haga clic con el botón secundario en Agregar *\_datos, aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* y haga clic en **Eliminar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

En el menú **Herramientas,** haga clic en Administrador de **paquetes NuGet**y, a continuación, haga clic en Consola del Administrador de **paquetes** (PMC). Introduzca los siguientes comandos en el PMC.

1. Habilitar migraciones
2. Add-Migration Init
3. Update-Database

Ejecute la aplicación y utilice FaceBook y Google para iniciar sesión y registrar algunos usuarios.

## <a name="examine-the-membership-data"></a>Examinar los datos de membresía

En el menú **Ver** , haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Haga clic con el botón derecho en **AspNetUsers** y haga clic en **Mostrar datos**de tabla .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Los `HomeTown` `BirthDate` campos y se muestran a continuación.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Cerrar sesión en la aplicación e iniciar sesión con otra cuenta

Si inicias sesión en tu aplicación con Facebook y, a continuación, cierras sesión e intentas iniciar sesión de nuevo con otra cuenta de Facebook (con el mismo navegador), entrarás inmediatamente en sesión en la cuenta de Facebook anterior que usaste. Para usar otra cuenta, debes navegar a Facebook y cerrar sesión en Facebook. La misma regla se aplica a cualquier otro proveedor de autenticación de 3a parte. Como alternativa, puede iniciar sesión con otra cuenta mediante un explorador diferente.

## <a name="next-steps"></a>Pasos siguientes

Consulte Introducción a los proveedores de seguridad de [OAuth de Yahoo y LinkedIn para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para Yahoo y LinkedIn. Consulte los botones de inicio de sesión social de Jerrie para obtener ASP.NET MVC 5 para habilitar los botones de inicio de sesión social.

Siga mi tutorial [Crear una aplicación MVC ASP.NET con autenticación y SQL DB e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que continúa este tutorial y muestra lo siguiente:

1. Cómo implementar la aplicación en Azure.
2. Cómo proteger la aplicación con roles.
3. Cómo proteger la aplicación con los filtros [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize.](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)
4. Cómo usar la API de pertenencia para agregar usuarios y roles.

Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar. También puede solicitar nuevos temas en [Mostrarcómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Incluso puede pedir y votar nuevas características que se agregarán a ASP.NET. Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Para obtener una buena explicación de cómo funcionan ASP.NET servicios de autenticación externa, consulte Servicios de [autenticación externa](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray . El artículo de Robert también entra en detalle al habilitar la autenticación de Microsoft y Twitter. El excelente [tutorial de EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) de Tom Dykstra muestra cómo trabajar con Entity Framework.
