---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con autenticación en dos fases. Debe completar la aplicación web de crear una ASP.NET MVC 5 segura con...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 25d21efaf2f01ee1c162408a3caf699ac818aaa7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384966"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="4cb9a-104">Aplicación de ASP.NET MVC 5 con autenticación en dos fases por SMS y correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4cb9a-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="4cb9a-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="4cb9a-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="4cb9a-106">Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="4cb9a-107">Debe completar [crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="4cb9a-108">Puede descargar la aplicación completada [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="4cb9a-109">La descarga contiene aplicaciones auxiliares de depuración que le permiten probar la confirmación por correo electrónico y SMS sin tener que configurar un correo electrónico o el proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="4cb9a-110">En este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="4cb9a-111">Crear una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4cb9a-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="4cb9a-112">Configuración de SMS para la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="4cb9a-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="4cb9a-113">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="4cb9a-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="4cb9a-114">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4cb9a-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="4cb9a-115">Crear una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4cb9a-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="4cb9a-116">Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="4cb9a-117">Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb9a-118">Advertencia: Debe completar [crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="4cb9a-119">Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="4cb9a-120">Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="4cb9a-121">Formularios Web Forms también es compatible con ASP.NET Identity, por lo que podría seguir pasos similares en una aplicación de formularios web.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="4cb9a-122">Deje la autenticación predeterminada como **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="4cb9a-123">Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="4cb9a-124">Más adelante en el tutorial se implementará en Azure.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="4cb9a-125">También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="4cb9a-126">Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="4cb9a-127">Configuración de SMS para la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="4cb9a-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="4cb9a-128">Este tutorial proporciona instrucciones para el uso de Twilio o ASPSMS, pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. **<span data-ttu-id="4cb9a-129">Creación de una cuenta de usuario con un proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="4cb9a-129">Creating a User Account with an SMS provider</span></span>**  
  
   <span data-ttu-id="4cb9a-130">Crear un [Twilio](https://www.twilio.com/try-twilio) o un [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) cuenta.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. **<span data-ttu-id="4cb9a-131">Instalar paquetes adicionales o agregar referencias de servicio</span><span class="sxs-lookup"><span data-stu-id="4cb9a-131">Installing additional packages or adding service references</span></span>**  
  
   <span data-ttu-id="4cb9a-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-132">Twilio:</span></span>  
   <span data-ttu-id="4cb9a-133">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="4cb9a-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-134">ASPSMS:</span></span>  
   <span data-ttu-id="4cb9a-135">Se debe agregar la referencia de servicio siguientes:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="4cb9a-136">Dirección:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="4cb9a-137">Espacio de nombres: </span><span class="sxs-lookup"><span data-stu-id="4cb9a-137">Namespace:</span></span>  
    `ASPSMSX2`
3. **<span data-ttu-id="4cb9a-138">Averiguar las credenciales de usuario del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="4cb9a-138">Figuring out SMS Provider User credentials</span></span>**  
  
   <span data-ttu-id="4cb9a-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-139">Twilio:</span></span>  
   <span data-ttu-id="4cb9a-140">Desde el **panel** pestaña de la cuenta de Twilio, copia el **SID de cuenta** y **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="4cb9a-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-141">ASPSMS:</span></span>  
   <span data-ttu-id="4cb9a-142">En la configuración de su cuenta, vaya a **Userkey** y cópielo junto con su autodefinido **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="4cb9a-143">Más adelante, almacenaremos estos valores en el *web.config* archivo dentro de las claves `"SMSAccountIdentification"` y `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="4cb9a-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. **<span data-ttu-id="4cb9a-144">Especifica el Id. de remitente / originador</span><span class="sxs-lookup"><span data-stu-id="4cb9a-144">Specifying SenderID / Originator</span></span>**  
  
   <span data-ttu-id="4cb9a-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-145">Twilio:</span></span>  
   <span data-ttu-id="4cb9a-146">Desde el **números** pestaña, copie el número de teléfono de Twilio.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="4cb9a-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-147">ASPSMS:</span></span>  
   <span data-ttu-id="4cb9a-148">Dentro de la **desbloquear originadores** menú, desbloquear uno o varios de los originadores o elija un originador alfanumérico (no admite todas las redes).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="4cb9a-149">Más adelante se almacenará este valor en el *web.config* archivo dentro de la clave `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="4cb9a-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. **<span data-ttu-id="4cb9a-150">Transferir las credenciales del proveedor SMS en la aplicación</span><span class="sxs-lookup"><span data-stu-id="4cb9a-150">Transferring SMS provider credentials into app</span></span>**  
  
   <span data-ttu-id="4cb9a-151">Facilitar las credenciales y el número de teléfono del remitente a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="4cb9a-152">Para simplificar las cosas se almacenará estos valores en el *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="4cb9a-153">Cuando se implementa en Azure, podemos almacenar los valores de forma segura en el **configuración de la aplicación** sección en el sitio web de la pestaña de configuración.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="4cb9a-154">Seguridad: nunca almacene de datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="4cb9a-155">La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="4cb9a-156">Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. **<span data-ttu-id="4cb9a-157">Implementación de transferencia de datos al proveedor SMS</span><span class="sxs-lookup"><span data-stu-id="4cb9a-157">Implementation of data transfer to SMS provider</span></span>**  
  
   <span data-ttu-id="4cb9a-158">Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="4cb9a-159">En función del proveedor SMS usado activar cualquiera el **Twilio** o **ASPSMS** sección:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="4cb9a-160">Actualización de la *Views\Manage\Index.cshtml* vista de Razor: (Nota: simplemente no quite los comentarios en el código existente, use el código siguiente.)</span><span class="sxs-lookup"><span data-stu-id="4cb9a-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="4cb9a-161">Compruebe el `EnableTwoFactorAuthentication` y `DisableTwoFactorAuthentication` métodos de acción en el `ManageController` tienen la[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atributo:</span><span class="sxs-lookup"><span data-stu-id="4cb9a-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="4cb9a-162">Ejecute la aplicación e inicie sesión con la cuenta que registró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="4cb9a-163">Haga clic en el identificador de usuario, que activa el `Index` los métodos de acción `Manage` controlador.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="4cb9a-164">Haga clic en Agregar.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="4cb9a-165">El `AddPhoneNumber` método de acción muestra un cuadro de diálogo para especificar un número de teléfono que pueda recibir mensajes SMS.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="4cb9a-166">En unos pocos segundos obtendrá un mensaje de texto con el código de verificación.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="4cb9a-167">Escríbala y presione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="4cb9a-168">La vista de administración se muestra que el número de teléfono se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="4cb9a-169">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="4cb9a-169">Enable two-factor authentication</span></span>

<span data-ttu-id="4cb9a-170">En la aplicación de la plantilla generada, deberá usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="4cb9a-171">Para habilitar 2FA, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="4cb9a-172">Haga clic en Habilitar 2FA.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="4cb9a-173">Cierre de sesión, a continuación, vuelva a iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-173">Logout, then log back in.</span></span> <span data-ttu-id="4cb9a-174">Si ha activado el correo electrónico (consulte mi [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o correo electrónico para 2FA.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="4cb9a-175">Se muestra la página de códigos de comprobación donde puede escribir el código (de SMS o correo electrónico).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="4cb9a-176">Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de usar 2FA para iniciar sesión cuando se usa el explorador y el dispositivo donde se ha activado la casilla.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="4cb9a-177">Siempre y cuando los usuarios malintencionados no pueden obtener acceso al dispositivo, lo que permite 2FA y haga clic en el **recordar este explorador** le proporcionará acceso cómodo de contraseña de un solo paso, mientras se conservan la protección segura 2FA para todos los accesos desde dispositivos que no son de confianza.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="4cb9a-178">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="4cb9a-179">Este tutorial proporciona una introducción rápida a habilitar 2FA en una nueva aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="4cb9a-180">Mi tutorial [autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra en detalle en el código subyacente del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="4cb9a-181">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4cb9a-181">Additional Resources</span></span>

- <span data-ttu-id="4cb9a-182">[Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra en detalle en la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="4cb9a-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="4cb9a-183">Recursos recomendados de vínculos a ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="4cb9a-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="4cb9a-184">[La cuenta de confirmación y la recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de recuperación y cuenta.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="4cb9a-185">[Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación ASP.NET MVC 5 con Facebook y Google OAuth 2 la autorización.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="4cb9a-186">También muestra cómo agregar datos adicionales a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="4cb9a-187">[Implementar una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="4cb9a-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="4cb9a-188">Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con los roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.</span><span class="sxs-lookup"><span data-stu-id="4cb9a-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="4cb9a-189">Creación de una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4cb9a-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="4cb9a-190">Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4cb9a-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="4cb9a-191">Configuración de SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="4cb9a-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="4cb9a-192">Cómo configurar el entorno de desarrollo de C# y ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4cb9a-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
