---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Uso de SSL en Web API | Microsoft Docs
author: MikeWasson
description: Muestra cómo usar SSL con ASP.NET Web API, incluido el uso de certificados de cliente SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484417"
---
# <a name="working-with-ssl-in-web-api"></a><span data-ttu-id="565fb-103">Trabajar con SSL en Web API</span><span class="sxs-lookup"><span data-stu-id="565fb-103">Working with SSL in Web API</span></span>

<span data-ttu-id="565fb-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="565fb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="565fb-105">Varios esquemas de autenticación comunes no son seguros en HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="565fb-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="565fb-106">En particular, la autenticación básica y la autenticación mediante formularios envían credenciales no cifradas.</span><span class="sxs-lookup"><span data-stu-id="565fb-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="565fb-107">Para que sea seguro, estos esquemas de autenticación *deben* usar SSL.</span><span class="sxs-lookup"><span data-stu-id="565fb-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="565fb-108">Además, los certificados de cliente SSL se pueden usar para autenticar a los clientes.</span><span class="sxs-lookup"><span data-stu-id="565fb-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="565fb-109">Habilitación de SSL en el servidor</span><span class="sxs-lookup"><span data-stu-id="565fb-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="565fb-110">Para configurar SSL en IIS 7 o posterior:</span><span class="sxs-lookup"><span data-stu-id="565fb-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="565fb-111">Cree u obtenga un certificado.</span><span class="sxs-lookup"><span data-stu-id="565fb-111">Create or get a certificate.</span></span> <span data-ttu-id="565fb-112">Para las pruebas, puede crear un certificado autofirmado.</span><span class="sxs-lookup"><span data-stu-id="565fb-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="565fb-113">Agregue un enlace HTTPS.</span><span class="sxs-lookup"><span data-stu-id="565fb-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="565fb-114">Para obtener más información, consulte [configuración de SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="565fb-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="565fb-115">En el caso de las pruebas locales, puede habilitar SSL en IIS Express desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="565fb-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="565fb-116">En el ventana Propiedades, establezca **SSL habilitado** en **true**.</span><span class="sxs-lookup"><span data-stu-id="565fb-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="565fb-117">Anote el valor de **dirección URL de SSL**; Use esta dirección URL para probar las conexiones HTTPS.</span><span class="sxs-lookup"><span data-stu-id="565fb-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="565fb-118">Aplicación de SSL en un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="565fb-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="565fb-119">Si tiene un enlace HTTP y HTTPS, los clientes pueden seguir usando HTTP para tener acceso al sitio.</span><span class="sxs-lookup"><span data-stu-id="565fb-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="565fb-120">Podría permitir que algunos recursos estén disponibles a través de HTTP, mientras que otros requieren SSL.</span><span class="sxs-lookup"><span data-stu-id="565fb-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="565fb-121">En ese caso, use un filtro de acción para requerir SSL para los recursos protegidos.</span><span class="sxs-lookup"><span data-stu-id="565fb-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="565fb-122">El código siguiente muestra un filtro de autenticación de API web que comprueba el estado de SSL:</span><span class="sxs-lookup"><span data-stu-id="565fb-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="565fb-123">Añada este filtro a todas las acciones de API web que requieran SSL:</span><span class="sxs-lookup"><span data-stu-id="565fb-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="565fb-124">Certificados de cliente SSL</span><span class="sxs-lookup"><span data-stu-id="565fb-124">SSL Client Certificates</span></span>

<span data-ttu-id="565fb-125">SSL proporciona autenticación mediante certificados de infraestructura de clave pública.</span><span class="sxs-lookup"><span data-stu-id="565fb-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="565fb-126">El servidor debe proporcionar un certificado que autentique el servidor en el cliente.</span><span class="sxs-lookup"><span data-stu-id="565fb-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="565fb-127">Es menos común que el cliente proporcione un certificado al servidor, pero esta es una opción para la autenticación de clientes.</span><span class="sxs-lookup"><span data-stu-id="565fb-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="565fb-128">Para usar certificados de cliente con SSL, necesita una manera de distribuir certificados firmados a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="565fb-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="565fb-129">Para muchos tipos de aplicaciones, esto no será una buena experiencia del usuario, pero en algunos entornos (por ejemplo, Enterprise) puede ser factible.</span><span class="sxs-lookup"><span data-stu-id="565fb-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="565fb-130">Ventajas</span><span class="sxs-lookup"><span data-stu-id="565fb-130">Advantages</span></span> | <span data-ttu-id="565fb-131">Desventajas</span><span class="sxs-lookup"><span data-stu-id="565fb-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="565fb-132">-Las credenciales de certificado son más seguras que el nombre de usuario y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="565fb-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="565fb-133">-SSL proporciona un canal seguro completo, con autenticación, integridad de mensajes y cifrado de mensajes.</span><span class="sxs-lookup"><span data-stu-id="565fb-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="565fb-134">-Debe obtener y administrar certificados PKI.</span><span class="sxs-lookup"><span data-stu-id="565fb-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="565fb-135">-La plataforma cliente debe admitir certificados de cliente SSL.</span><span class="sxs-lookup"><span data-stu-id="565fb-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="565fb-136">Para configurar IIS para que acepte certificados de cliente, abra el administrador de IIS y realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="565fb-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="565fb-137">Haga clic en el nodo sitio en la vista de árbol.</span><span class="sxs-lookup"><span data-stu-id="565fb-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="565fb-138">Haga doble clic en la característica **configuración de SSL** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="565fb-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="565fb-139">En **certificados de cliente**, seleccione una de estas opciones:</span><span class="sxs-lookup"><span data-stu-id="565fb-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="565fb-140">**Aceptar**: IIS aceptará un certificado del cliente, pero no requiere uno.</span><span class="sxs-lookup"><span data-stu-id="565fb-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="565fb-141">**Requerir**: requiere un certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="565fb-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="565fb-142">(Para habilitar esta opción, debe seleccionar también "requerir SSL")</span><span class="sxs-lookup"><span data-stu-id="565fb-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="565fb-143">También puede establecer estas opciones en el archivo ApplicationHost. config:</span><span class="sxs-lookup"><span data-stu-id="565fb-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="565fb-144">La marca **SslNegotiateCert** significa que IIS aceptará un certificado del cliente, pero no requiere uno (equivalente a la opción "Aceptar" en el administrador de IIS).</span><span class="sxs-lookup"><span data-stu-id="565fb-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="565fb-145">Para requerir un certificado, establezca la marca **SslRequireCert** .</span><span class="sxs-lookup"><span data-stu-id="565fb-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="565fb-146">Para las pruebas, también puede establecer estas opciones en IIS Express, en el ApplicationHost local. Archivo de configuración, que se encuentra en "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="565fb-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="565fb-147">Creación de un certificado de cliente para pruebas</span><span class="sxs-lookup"><span data-stu-id="565fb-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="565fb-148">Con fines de prueba, puede usar [Makecert. exe](/windows/desktop/SecCrypto/makecert) para crear un certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="565fb-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="565fb-149">En primer lugar, cree una entidad de certificación raíz de prueba:</span><span class="sxs-lookup"><span data-stu-id="565fb-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="565fb-150">Makecert le pedirá que escriba una contraseña para la clave privada.</span><span class="sxs-lookup"><span data-stu-id="565fb-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="565fb-151">A continuación, agregue el certificado al almacén "entidades de certificación raíz de confianza" del servidor de prueba, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="565fb-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="565fb-152">Abra MMC.</span><span class="sxs-lookup"><span data-stu-id="565fb-152">Open MMC.</span></span>
2. <span data-ttu-id="565fb-153">En **archivo**, seleccione **Agregar o quitar complemento**.</span><span class="sxs-lookup"><span data-stu-id="565fb-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="565fb-154">Seleccione **cuenta de equipo**.</span><span class="sxs-lookup"><span data-stu-id="565fb-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="565fb-155">Seleccione **equipo local** y complete el asistente.</span><span class="sxs-lookup"><span data-stu-id="565fb-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="565fb-156">En el panel de navegación, expanda el nodo "entidades de certificación raíz de confianza".</span><span class="sxs-lookup"><span data-stu-id="565fb-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="565fb-157">En el menú **acción** , seleccione **todas las tareas**y, a continuación, haga clic en **importar** para iniciar el Asistente para importación de certificados.</span><span class="sxs-lookup"><span data-stu-id="565fb-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="565fb-158">Busque el archivo de certificado TempCA. cer.</span><span class="sxs-lookup"><span data-stu-id="565fb-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="565fb-159">Haga clic en **abrir**y, a continuación, haga clic en **siguiente** y complete el asistente.</span><span class="sxs-lookup"><span data-stu-id="565fb-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="565fb-160">(Se le pedirá que vuelva a escribir la contraseña).</span><span class="sxs-lookup"><span data-stu-id="565fb-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="565fb-161">Ahora cree un certificado de cliente firmado por el primer certificado:</span><span class="sxs-lookup"><span data-stu-id="565fb-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="565fb-162">Uso de certificados de cliente en Web API</span><span class="sxs-lookup"><span data-stu-id="565fb-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="565fb-163">En el lado del servidor, puede obtener el certificado de cliente mediante una llamada a [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) en el mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="565fb-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="565fb-164">El método devuelve NULL si no hay ningún certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="565fb-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="565fb-165">De lo contrario, devuelve una instancia de **X509Certificate2** .</span><span class="sxs-lookup"><span data-stu-id="565fb-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="565fb-166">Utilice este objeto para obtener información del certificado, como el emisor y el asunto.</span><span class="sxs-lookup"><span data-stu-id="565fb-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="565fb-167">Después, puede usar esta información para la autenticación y/o la autorización.</span><span class="sxs-lookup"><span data-stu-id="565fb-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
