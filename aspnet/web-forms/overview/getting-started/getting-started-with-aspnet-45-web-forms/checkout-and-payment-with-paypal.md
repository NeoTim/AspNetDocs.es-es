---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Pago y pago con PayPal ? Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675692"
---
# <a name="checkout-and-payment-with-paypal"></a>Finalización de la compra y pago con PayPal

por [Erik Reitan](https://github.com/Erikre)

[Descargar Wingtip Toys Sample Project (C)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Descargar E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un proyecto de Visual Studio 2013 con código fuente de [C-](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.

En este tutorial se describe cómo modificar la aplicación de ejemplo Wingtip Toys para incluir la autorización del usuario, el registro y el pago mediante PayPal. Solo los usuarios que hayan iniciado sesión tendrán autorización para comprar productos. La funcionalidad de registro de usuario integrada de la plantilla de proyecto de formularios Web Forms ASP.NET 4.5 ya incluye gran parte de lo que necesita. Añadirá la funcionalidad PayPal Express Checkout. En este tutorial está utilizando el entorno de pruebas para desarrolladores de PayPal, por lo que no se transferirán fondos reales. Al final del tutorial, probará la aplicación seleccionando productos para agregar al carro de la compra, haciendo clic en el botón de pago y transfiriendo datos al sitio web de pruebas de PayPal. En el sitio web de pruebas de PayPal, confirmará su información de envío y pago y luego regresará a la aplicación de muestra local de Wingtip Toys para confirmar y completar la compra.

Hay varios procesadores de pago de terceros experimentados que se especializan en compras en línea que abordan la escalabilidad y la seguridad. ASP.NET desarrolladores deben tener en cuenta las ventajas de utilizar una solución de pago de terceros antes de implementar una solución de compra y compra.

> [!NOTE] 
> 
> La aplicación de ejemplo Wingtip Toys se diseñó para mostrar conceptos de ASP.NET específicos y características disponibles para ASP.NET desarrolladores web. Esta aplicación de ejemplo no se ha optimizado para todas las circunstancias posibles en cuanto a escalabilidad y seguridad.

## <a name="what-youll-learn"></a>Temas que se abordarán:

- Cómo restringir el acceso a páginas específicas de una carpeta.
- Cómo crear un carrito de compras conocido a partir de un carrito de compras anónimo.
- Cómo habilitar SSL para el proyecto.
- Cómo agregar un proveedor de OAuth al proyecto.
- Cómo utilizar PayPal para comprar productos utilizando el entorno de pruebas de PayPal.
- Cómo mostrar detalles de PayPal en un **control DetailsView.**
- Cómo actualizar la base de datos de la aplicación Wingtip Toys con los detalles obtenidos de PayPal.

## <a name="adding-order-tracking"></a>Adición de seguimiento de pedidos

En este tutorial, creará dos nuevas clases para realizar un seguimiento de los datos del orden en que ha creado un usuario. Las clases realizarán un seguimiento de los datos relativos a la información de envío, el total de la compra y la confirmación del pago.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Agregue las clases de modelo Order y OrderDetail

Anteriormente en esta serie de tutoriales, definió el esquema para `Category`categorías, productos y elementos del carro de la compra mediante la creación de las clases , `Product`, y `CartItem` en la carpeta *Modelos.* Ahora agregará dos nuevas clases para definir el esquema para el pedido de producto y los detalles del pedido.

1. En la carpeta **Modelos,** agregue una nueva clase denominada *Order.cs*.   
   El nuevo archivo de clase se muestra en el editor.
2. Reemplace el código predeterminado con lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Agregue una clase *OrderDetail.cs* a la carpeta *Modelos.*
4. Reemplace el código predeterminado por el siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Las `Order` `OrderDetail` clases y contienen el esquema para definir la información de pedido utilizada para la compra y el envío.

Además, deberá actualizar la clase de contexto de base de datos que administra las clases de entidad y que proporciona acceso a datos a la base de datos. Para ello, agregará las clases `OrderDetail` Order y `ProductContext` model recién creadas a la clase.

1. En **el Explorador de**soluciones , busque y abra el archivo *ProductContext.cs.*
2. Agregue el código resaltado al archivo *de ProductContext.cs* como se muestra a continuación:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Como se mencionó anteriormente en *ProductContext.cs* esta serie de `System.Data.Entity` tutoriales, el código del archivo ProductContext.cs agrega el espacio de nombres para que tenga acceso a toda la funcionalidad principal de Entity Framework. Esta funcionalidad incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados. El código anterior `ProductContext` de la clase agrega acceso `Order` `OrderDetail` de Entity Framework a las clases recién agregadas.

## <a name="adding-checkout-access"></a>Adición del acceso de pago

La aplicación de ejemplo Wingtip Toys permite a los usuarios anónimos revisar y agregar productos a un carro de la compra. Sin embargo, cuando los usuarios anónimos eligen comprar los productos que agregaron al carro de la compra, deben iniciar sesión en el sitio. Una vez que han iniciado sesión, pueden acceder a las páginas restringidas de la aplicación web que controlan el proceso de pago y compra. Estas páginas restringidas se encuentran en la carpeta *de desprotección* de la aplicación.

### <a name="add-a-checkout-folder-and-pages"></a>Añadir una carpeta y páginas de pago

Ahora creará la carpeta *de pago* y las páginas que el cliente verá durante el proceso de pago. Actualizará estas páginas más adelante en este tutorial.

1. Haga clic con el botón derecho en el nombre del proyecto (**Wingtip Toys**) en el **Explorador** de soluciones y seleccione **Agregar una nueva carpeta**. 

    ![Pago y pago con PayPal - Nueva carpeta](checkout-and-payment-with-paypal/_static/image1.png)
2. Asigne a la nueva carpeta el nombre *Checkout*.
3. Haga clic con el botón derecho en la carpeta *Desprotección* y, a continuación, seleccione **Agregar**-&gt;**nuevo elemento**. 

    ![Pago y pago con PayPal - Nuevo artículo](checkout-and-payment-with-paypal/_static/image2.png)
4. Se mostrará el cuadro de diálogo **Agregar nuevo elemento** .
5. Seleccione el grupo plantillas **web** de **Visual C.**  - &gt; a la izquierda. A continuación, en el panel central, seleccione **Formulario web con página maestra**y asíténlo *CheckoutStart.aspx*. 

    ![Pago y pago con PayPal - Añadir nuevo cuadro de diálogo de artículo](checkout-and-payment-with-paypal/_static/image3.png)
6. Como antes, seleccione el archivo *Site.Master* como página maestra.
7. Agregue las siguientes páginas adicionales a la carpeta *de desprotección* siguiendo los mismos pasos anteriores:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Agregar un archivo Web.config

Al agregar un nuevo archivo *Web.config* a la carpeta *de desprotección,* podrá restringir el acceso a todas las páginas contenidas en la carpeta.

1. Haga clic con el botón derecho en la carpeta *Desprotección* y seleccione **Agregar**  - &gt; **nuevo elemento**.  
   Se mostrará el cuadro de diálogo **Agregar nuevo elemento** .
2. Seleccione el grupo plantillas **web** de **Visual C.**  - &gt; a la izquierda. A continuación, en el panel central, seleccione Archivo de **configuración web**, acepte el nombre predeterminado de *Web.config*y, a continuación, seleccione **Agregar**.
3. Reemplace el contenido XML del archivo *Web.config* por el siguiente:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Guarde el archivo *Web.config* .

El archivo *Web.config* especifica que se debe denegar el acceso a todas las páginas contenidas en la carpeta *de desprotección* a todos los usuarios desconocidos de la aplicación web. Sin embargo, si el usuario ha registrado una cuenta y ha iniciado sesión, será un usuario conocido y tendrá acceso a las páginas de la carpeta *de* pago.

Es importante tener en cuenta que ASP.NET configuración sigue una jerarquía, donde cada archivo *Web.config* aplica la configuración a la carpeta en la que se encuentra y a todos los directorios secundarios debajo de ella.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Habilitación de SSL para el proyecto

 Capa de sockets seguros (SSL) es un protocolo definido para permitir a los servidores y clientes web comunicarse de forma más segura mediante el uso de cifrado. Cuando no se usa SSL, cualquiera que tenga acceso físico a la red puede abrir los datos que se envían entre el cliente y el servidor para examinar el paquete. Además, varios esquemas de autenticación habituales no son seguros en HTTP plano. En particular, la autenticación básica y la autenticación mediante formularios envían credenciales no cifradas. Para ser seguros, estos esquemas de autenticación deben usar SSL. 

1. En **el Explorador**de soluciones , haga clic en el **WingtipToys** proyecto y, a continuación, presione **F4** para mostrar el **propiedades** ventana.
2. Cambie **SSL** habilitado `true`a .
3. Copie la **Dirección URL de SSL** para usarla más adelante.   
 La URL SSL `https://localhost:44300/` será a menos que haya creado previamente Sitios Web SSL (como se muestra a continuación).   
    ![Project Properties](checkout-and-payment-with-paypal/_static/image4.png)
4. En el **Explorador**de soluciones , haga clic con el botón derecho en el proyecto **WingtipToys** y haga clic en **Propiedades**.
5. En el panel izquierdo, haga clic en **Web**.
6. Cambie la **dirección URL** del proyecto para usar la dirección **URL SSL** que guardó anteriormente.   
    ![Propiedades de Project Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Presione **CTRL+S**para guardar la página.
8. Presione **Ctrl+F5** para ejecutar la aplicación. Visual Studio mostrará una opción que le permite evitar advertencias de SSL.
9. Haga clic en **Sí** para confiar en el certificado SSL de IIS Express y continúe.   
    ![Detalles del certificado SSL de IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
  Se mostrará una advertencia de seguridad.
10. Haga clic en **Sí** para instalar el certificado en el host local.   
    ![Cuadro de diálogo Advertencia de seguridad](checkout-and-payment-with-paypal/_static/image7.png)  
  Se mostrará la ventana del explorador.

Ahora puede probar fácilmente su aplicación web localmente mediante SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Incorporación de un proveedor de OAuth 2.0

ASP.NET Web Forms proporciona opciones mejoradas para suscripciones y autenticación. Estas mejoras incluyen OAuth. OAuth es un protocolo abierto que ofrece autorización segura a través de un método estándar sencillo para aplicaciones web, móviles y de escritorio. La plantilla ASP.NET formularios Web Forms usa OAuth para exponer Facebook, Twitter, Google y Microsoft como proveedores de autenticación. Aunque este tutorial solo utiliza Google como proveedor de autenticación, puede modificar fácilmente el código para utilizar cualquiera de los proveedores. Los pasos que hay que seguir para implementar otros proveedores son muy similares a los que verá en este tutorial.

Además de la autenticación, este tutorial también utiliza roles para implementar la autorización. Únicamente los usuarios que agregue al rol `canEdit` podrán cambiar datos (crear, editar o eliminar contactos).

> [!NOTE] 
> 
> Las aplicaciones de Windows Live solo aceptan una dirección URL activa para un sitio web en funcionamiento, por lo que no puede usar una dirección URL de sitio web local para probar los inicios de sesión.

Los pasos siguientes permiten agregar un proveedor de autenticación de Google.

1. Abra el archivo de Inicio de la *aplicación/Startup.Auth.cs.\_*
2. Quite los caracteres de comentario del método `app.UseGoogleAuthentication()` para que tenga el siguiente aspecto: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navegue a la [consola de desarrolladores de Google](https://console.developers.google.com/). Debe iniciar sesión también con su cuenta de correo electrónico de desarrollador de Google (gmail.com). Si no tiene una cuenta de Google, seleccione el vínculo **Crear una cuenta** .   
   A continuación, verá la **consola de desarrolladores de Google**.   
    ![consola de desarrolladores de Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Haga clic en el botón **Crear proyecto** e introduzca un nombre de proyecto e ID (puede usar los valores predeterminados). A continuación, haga clic en la casilla de **verificación de acuerdo** y en el botón **Crear.**  

    ![Google: Nuevo proyecto](checkout-and-payment-with-paypal/_static/image9.png)

   En unos segundos se creará el nuevo proyecto y su navegador mostrará la nueva página de proyectos.
5. En la pestaña izquierda, haga clic en **API &amp; auth**y, a continuación, haga clic en **Credentials**.
6. Haga clic en **Crear nuevo ID** de cliente en **OAuth**.   
   Se mostrará el cuadro de diálogo **Crear id. de cliente** .   
    ![Google - Crear ID de cliente](checkout-and-payment-with-paypal/_static/image10.png)
7. En el cuadro de diálogo Crear ID de **cliente,** mantenga la **aplicación web** predeterminada para el tipo de aplicación.
8. Establezca los **Orígenes de JavaScript autorizados** en la`https://localhost:44300/` dirección URL SSL que utilizó anteriormente en este tutorial (a menos que haya creado otros proyectos SSL).   
   Esta dirección URL es el origen de su aplicación. Para este ejemplo, solo proporcionará la dirección URL de prueba del localhost. Sin embargo, puede escribir varias direcciones URL para tener en cuenta localhost y producción.
9. Establezca el **URI de redireccionamiento autorizado** en lo siguiente: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Este valor es el URI que utiliza ASP.NET OAuth para comunicarse con el servidor OAuth de Google. Recuerde la URL SSL `https://localhost:44300/` que utilizó anteriormente (a menos que haya creado otros proyectos SSL).
10. Haga clic en el botón **Crear ID de cliente.**
11. En el menú de la consola de Google Developers, haz clic en el elemento de menú de la **pantalla Consentimiento** y, a continuación, establece la dirección de correo electrónico y el nombre del producto. Cuando haya completado el formulario, haga clic en **Guardar**.
12. Haga clic en el elemento de menú **API,** desplácese hacia abajo y haga clic en el botón **de apagado** situado junto a la API de **Google+**.   
    Si aceptaesta opción, se habilitará la API de Google+.
13. También debe actualizar el paquete **NuGet Microsoft.Owin** a la versión 3.0.0.   
    En el menú **Herramientas** , seleccione Administrador de **paquetes NuGet** y, a continuación, seleccione **Administrar paquetes NuGet para la solución**.  
    En la ventana **Administrar paquetes NuGet,** busque y actualice el paquete **Microsoft.Owin** a la versión 3.0.0.
14. En Visual Studio, `UseGoogleAuthentication` actualice el método de la página *Startup.Auth.cs* copiando y pegando el identificador de **cliente** y el secreto de **cliente** en el método. Los valores **de ID** de cliente y secreto de **cliente** que se muestran a continuación son ejemplos y no funcionarán. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el vínculo **Iniciar sesión** .
16. En **Usar otro servicio para iniciar sesión**, haga clic en **Google**.  
    ![Iniciar sesión](checkout-and-payment-with-paypal/_static/image11.png)
17. Si tiene que proporcionar credenciales, se le redirigirá al sitio de Google donde podrá especificarlas.  
    ![Google: Inicio de sesión](checkout-and-payment-with-paypal/_static/image12.png)
18. Después de introducir sus credenciales, se le pedirá que conceda permisos a la aplicación web que acaba de crear.  
    ![Cuenta de servicio de proyecto predeterminada](checkout-and-payment-with-paypal/_static/image13.png)
19. Haga clic en **Aceptar**. Ahora se le redirigirá de nuevo a la página **Registrar de** la aplicación **WingtipToys,** donde podrá registrar su cuenta de Google.  
    ![Registro con una cuenta de Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Tiene la opción de cambiar el nombre de registro de correo electrónico local que usa para su cuenta de Gmail, pero suele ser más práctico mantener el mismo alias de correo electrónico predeterminado (es decir, el que usó para la autenticación). Haga clic **en Iniciar sesión** como se muestra arriba.

### <a name="modifying-login-functionality"></a>Modificación de la funcionalidad de inicio de sesión

Como se mencionó anteriormente en esta serie de tutoriales, gran parte de la funcionalidad de registro de usuario se ha incluido en la plantilla de formularios Web Forms de ASP.NET de forma predeterminada. Ahora modificará las páginas *Login.aspx* y *Register.aspx* predeterminadas para llamar al `MigrateCart` método. El `MigrateCart` método asocia a un usuario recién iniciado sesión con un carro de la compra anónimo. Al asociar el usuario y el carrito de la compra, la aplicación de ejemplo Wingtip Toys será capaz de mantener el carro de la compra del usuario entre visitas.

1. En el Explorador de **soluciones**, busque y abra la carpeta *Cuenta.*
2. Modifique la página de código subyacente denominada *Login.aspx.cs* para incluir el código resaltado en amarillo, de modo que aparezca de la siguiente manera:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Guarde el archivo *Login.aspx.cs.*

Por ahora, puede omitir la advertencia de `MigrateCart` que no hay ninguna definición para el método. Lo añadirá un poco más adelante en este tutorial.

El *archivo de* código subyacente Login.aspx.cs admite un método LogIn. Al inspeccionar el Login.aspx página, verá que esta página incluye un botón "Iniciar sesión" que al hacer clic desencadena el `LogIn` controlador en el código subyacente.

Cuando `Login` se llama al método en el *Login.aspx.cs,* `usersShoppingCart` se crea una nueva instancia del carro de la compra denominado. El identificador del carro de la compra (un `cartId` GUID) se recupera y se establece en la variable. A continuación, se llama `MigrateCart` al `cartId` método, pasando el nombre y el nombre del usuario que ha iniciado sesión a este método. Cuando se migra el carro de la compra, el GUID utilizado para identificar el carro de la compra anónimo se reemplaza por el nombre de usuario.

Además de modificar el archivo de código subyacente *de Login.aspx.cs* para migrar el carro de la compra cuando el usuario inicia sesión, también debe modificar el archivo de código subyacente de *Register.aspx.cs* para migrar el carro de la compra cuando el usuario crea una nueva cuenta e inicia sesión.

1. En la carpeta *Cuenta,* abra el archivo de código subyacente denominado *Register.aspx.cs*.
2. Modifique el archivo de código subyacente incluyendo el código en amarillo, de modo que aparezca de la siguiente manera:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Guarde el archivo *Register.aspx.cs.* Una vez más, ignore `MigrateCart` la advertencia sobre el método.

Tenga en cuenta que `CreateUser_Click` el código que usó en el `LogIn` controlador de eventos es muy similar al código que usó en el método. Cuando el usuario se registra o inicia sesión `MigrateCart` en el sitio, se realizará una llamada al método.

## <a name="migrating-the-shopping-cart"></a>Migración del carrito de la compra

Ahora que tiene actualizado el proceso de inicio de sesión y registro, `MigrateCart` puede agregar el código para migrar el carro de la compra mediante el método.

1. En el Explorador de **soluciones**, busque la carpeta *Logic* y abra el archivo de clase *ShoppingCartActions.cs.*
2. Agregue el código resaltado en amarillo al código existente en el archivo *ShoppingCartActions.cs,* de modo que el código del archivo *ShoppingCartActions.cs* aparezca de la siguiente manera:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

El `MigrateCart` método utiliza el cartId existente para encontrar el carro de la compra del usuario. A continuación, el código recorre todos los elementos del carro de la compra y reemplaza la `CartId` propiedad (según lo especificado por el `CartItem` esquema) por el nombre de usuario que ha iniciado sesión.

### <a name="updating-the-database-connection"></a>Actualización de la conexión de base de datos

Si sigue este tutorial con la aplicación de ejemplo Wingtip Toys **precompilada,** debe volver a crear la base de datos de pertenencia predeterminada. Al modificar la cadena de conexión predeterminada, la base de datos de pertenencia se creará la próxima vez que se ejecute la aplicación.

1. Abra el archivo *Web.config* en la raíz del proyecto.
2. Actualice la cadena de conexión predeterminada para que aparezca de la siguiente manera:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integración de PayPal

PayPal es una plataforma de facturación basada en la web que acepta pagos por parte de comerciantes en línea. En este tutorial se explica cómo integrar la funcionalidad Express Checkout de PayPal en su aplicación. Express Checkout permite a sus clientes usar PayPal para pagar los artículos que han añadido a su carrito de compras.

### <a name="create-paypal-test-accounts"></a>Crear cuentas de prueba de PayPal

Para usar el entorno de prueba de PayPal, debe crear y verificar una cuenta de prueba para desarrolladores. Usará la cuenta de prueba de desarrollador para crear una cuenta de prueba de comprador y una cuenta de prueba de vendedor. Las credenciales de la cuenta de prueba para desarrolladores también permitirán que la aplicación de ejemplo Wingtip Toys acceda al entorno de pruebas de PayPal.

1. En un explorador, vaya al sitio de pruebas para desarrolladores de PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Si no tienes una cuenta de desarrollador de PayPal, crea una nueva cuenta haciendo clic en **Registrarse**y siguiendo los pasos de registro. Si ya tiene una cuenta de desarrollador de PayPal, inicie sesión haciendo clic **en Iniciar sesión**. Necesitará su cuenta de desarrollador de PayPal para probar la aplicación de ejemplo Wingtip Toys más adelante en este tutorial.
3. Si acabas de registrarte para obtener tu cuenta de desarrollador de PayPal, es posible que debas verificar tu cuenta de desarrollador de PayPal con PayPal. Puedes verificar tu cuenta siguiendo los pasos que PayPal envió a tu cuenta de correo electrónico. Una vez que haya verificado su cuenta de desarrollador de PayPal, vuelva a iniciar sesión en el sitio de pruebas para desarrolladores de PayPal.
4. Después de iniciar sesión en el sitio para desarrolladores de PayPal con su cuenta de desarrollador de PayPal, debe crear una cuenta de prueba de comprador de PayPal si aún no tiene una. Para crear una cuenta de prueba de comprador, en el sitio de PayPal, haga clic en la pestaña **Aplicaciones** y, a continuación, haga clic en **Cuentas de espacio aislado**.   
 Se muestra la página Cuentas de prueba de **espacio aislado.**   

    > [!NOTE] 
    > 
    > El sitio para desarrolladores de PayPal ya proporciona una cuenta de prueba de comerciante.

    ![Pago y pago con PayPal - Cuentas de prueba de Sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. En la página Cuentas de prueba de espacio aislado, haga clic en **Crear cuenta**.
6. En la página Crear cuenta de **prueba,** elija un correo electrónico y una contraseña de cuenta de prueba del comprador de su elección.   

    > [!NOTE] 
    > 
    > Necesitará las direcciones de correo electrónico y la contraseña del comprador para probar la aplicación de ejemplo Wingtip Toys al final de este tutorial.

    ![Pago y pago con PayPal - Cuentas de prueba de Sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Cree la cuenta de prueba del comprador haciendo clic en el botón **Crear cuenta.**  
 Se muestra la página Cuentas de prueba de **espacio aislado.** 

    ![Pago y pago con PayPal - Cuentas PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. En la página Cuentas de prueba de **espacio aislado,** haga clic en la cuenta de correo electrónico **del facilitador.**  
    **Aparecen** las opciones Perfil y **Notificación.**
9. Seleccione la opción **Perfil** y, a continuación, haga clic en **Credenciales** de API para ver las credenciales de la API de la cuenta de prueba de comerciante.
10. Copie las credenciales de la API test en el bloc de notas.

Necesitará las credenciales de la API de prueba clásica mostradas (nombre de usuario, contraseña y firma) para realizar llamadas a la API desde la aplicación de ejemplo Wingtip Toys al entorno de pruebas de PayPal. Agregará las credenciales en el paso siguiente.

### <a name="add-paypal-class-and-api-credentials"></a>Agregar credenciales de api y clase de PayPal

Colocará la mayoría del código de PayPal en una sola clase. Esta clase contiene los métodos utilizados para comunicarse con PayPal. Además, agregará sus credenciales de PayPal a esta clase.

1. En la aplicación de ejemplo Wingtip Toys en Visual Studio, haga clic con el botón secundario en la carpeta **Logic** y, a continuación, seleccione **Agregar**  - &gt; **nuevo elemento**.   
   Se mostrará el cuadro de diálogo **Agregar nuevo elemento** .
2. En **Visual C.** en el panel **Instalado** de la izquierda, seleccione **Código**.
3. En el panel central, seleccione **Clase**. Asigne a esta nueva clase el nombre **PayPalFunctions.cs**.
4. Haga clic en **Agregar**.  
   El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado por el siguiente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Agregue las credenciales de Merchant API (nombre de usuario, contraseña y firma) que mostró anteriormente en este tutorial para que pueda realizar llamadas de función al entorno de pruebas de PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> En esta aplicación de ejemplo, simplemente está agregando credenciales a un archivo de C- (.cs). Sin embargo, en una solución implementada, debe considerar la posibilidad de cifrar las credenciales en un archivo de configuración.

La clase NVPAPICaller contiene la mayoría de la funcionalidad de PayPal. El código de la clase proporciona los métodos necesarios para realizar una compra de prueba desde el entorno de pruebas de PayPal. Las tres funciones de PayPal siguientes se utilizan para realizar compras:

- Función `SetExpressCheckout`
- Función `GetExpressCheckoutDetails`
- Función `DoExpressCheckoutPayment`

El `ShortcutExpressCheckout` método recopila la información de compra de prueba `SetExpressCheckout` y los detalles del producto desde el carro de la compra y llama a la función PayPal. El `GetCheckoutDetails` método confirma los detalles de compra y llama a la `GetExpressCheckoutDetails` función PayPal antes de realizar la compra de prueba. El `DoCheckoutPayment` método completa la compra de prueba `DoExpressCheckoutPayment` desde el entorno de prueba llamando a la función PayPal. El código restante admite los métodos y el proceso de PayPal, como la codificación de cadenas, la descodificación de cadenas, el procesamiento de matrices y la determinación de credenciales.

> [!NOTE] 
> 
> PayPal le permite incluir detalles de compra opcionales basados en la [especificación de api de PayPal.](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout) Al extender el código en la aplicación de ejemplo Wingtip Toys, puede incluir detalles de localización, descripciones de productos, impuestos, un número de servicio al cliente, así como muchos otros campos opcionales.

Tenga en cuenta que las direcciones URL de devolución y cancelación que se especifican en el **método ShortcutExpressCheckout** usan un número de puerto.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Cuando Visual Web Developer ejecuta un proyecto web mediante SSL, normalmente se usa el puerto 44300 para el servidor web. Como se muestra arriba, el número de puerto es 44300. Al ejecutar la aplicación, podría ver un número de puerto diferente. El número de puerto debe establecerse correctamente en el código para que pueda ejecutar correctamente la aplicación de ejemplo Wingtip Toys al final de este tutorial. En la siguiente sección de este tutorial se explica cómo recuperar el número de puerto de host local y actualizar la clase PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Actualice el número de puerto localHost en la clase PayPal

La aplicación de ejemplo Wingtip Toys compra productos navegando al sitio de pruebas de PayPal y volviendo a la instancia local de la aplicación de ejemplo Wingtip Toys. Para que PayPal vuelva a la URL correcta, debe especificar el número de puerto de la aplicación de ejemplo que se ejecuta localmente en el código de PayPal mencionado anteriormente.

1. Haga clic con el botón derecho en el nombre del proyecto (**WingtipToys**) en el **Explorador** de soluciones y seleccione **Propiedades**.
2. En la columna izquierda, seleccione la pestaña **Web.**
3. Recuperar el número de puerto desde el cuadro Dirección URL del **proyecto.**
4. Si es `returnURL` necesario, `cancelURL` actualice la clase`NVPAPICaller`de PayPal ( ) y en el archivo *PayPalFunctions.cs* para utilizar el número de puerto de la aplicación web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Ahora el código que agregó coincidirá con el puerto esperado para la aplicación web local. PayPal podrá volver a la URL correcta en su equipo local.

### <a name="add-the-paypal-checkout-button"></a>Añadir el botón de pago de PayPal

Ahora que las funciones principales de PayPal se han agregado a la aplicación de ejemplo, puede comenzar a agregar el marcado y el código necesarios para llamar a estas funciones. En primer lugar, debe agregar el botón de pago que el usuario verá en la página del carro de la compra.

1. Abra el archivo *ShoppingCart.aspx.*
2. Desplázate hasta la parte `<!--Checkout Placeholder -->` inferior del archivo y busca el comentario.
3. Reemplace el comentario `ImageButton` por un control para que el marcado se reemplace de la siguiente manera:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. En el *archivo ShoppingCart.aspx.cs,* después del `UpdateBtn_Click` controlador de `CheckOutBtn_Click` eventos cerca del final del archivo, agregue el controlador de eventos:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. También en el archivo *ShoppingCart.aspx.cs,* `CheckoutBtn`agregue una referencia al archivo , para que se haga referencia al nuevo botón de imagen de la siguiente manera:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Guarde los cambios en el archivo *ShoppingCart.aspx* y en el archivo *ShoppingCart.aspx.cs.*
7. En el menú, seleccione **Depurar compilación**-&gt;**WingtipToys**.  
   El proyecto se volverá a generar con el control **ImageButton** recién agregado.

### <a name="send-purchase-details-to-paypal"></a>Enviar detalles de compra a PayPal

Cuando el usuario hace clic en el botón **de desprotección** en la página del carro de la compra (*ShoppingCart.aspx*), comenzará el proceso de compra. El código siguiente llama a la primera función de PayPal necesaria para comprar productos.

1. En la carpeta *Desprotección,* abra el archivo de código subyacente denominado *CheckoutStart.aspx.cs*.   
   Asegúrese de abrir el archivo de código subyacente.
2. Reemplace el código existente por el siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Cuando el usuario de la aplicación hace clic en el botón **de pago** en la página del carro de la compra, el explorador navegará a la *CheckoutStart.aspx* página. Cuando se carga la página `ShortcutExpressCheckout` *CheckoutStart.aspx,* se llama al método. En este punto, el usuario se transfiere al sitio web de pruebas de PayPal. En el sitio de PayPal, el usuario introduce sus credenciales de PayPal, revisa los detalles de compra, `ShortcutExpressCheckout` acepta el acuerdo de PayPal y vuelve a la aplicación de ejemplo Wingtip Toys donde se completa el método. Cuando `ShortcutExpressCheckout` se completa el método, redirigirá al usuario a la `ShortcutExpressCheckout` *CheckoutReview.aspx* página especificada en el método. Esto permite al usuario revisar los detalles del pedido desde la aplicación de ejemplo Wingtip Toys.

### <a name="review-order-details"></a>Revisar los detalles del pedido

Después de volver de PayPal, el *CheckoutReview.aspx* página de la Wingtip Toys aplicación de ejemplo muestra los detalles del pedido. Esta página permite al usuario revisar los detalles del pedido antes de comprar los productos. El *CheckoutReview.aspx* página debe crearse de la siguiente manera:

1. En la carpeta *Desprotección* , abra la página denominada *CheckoutReview.aspx*.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Abra la página de código subyacente denominada *CheckoutReview.aspx.cs* y reemplace el código existente por lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

El **DetailsView** control se utiliza para mostrar los detalles del pedido que se han devuelto desde PayPal. Además, el código anterior guarda los detalles del `OrderDetail` pedido en la base de datos Wingtip Toys como un objeto. Cuando el usuario hace clic en el completar **pedido** botón, se redirigen a la *CheckoutComplete.aspx* página.

> [!NOTE] 
> 
> **Sugerencia**
> 
> En el marcado de la *CheckoutReview.aspx* página, observe que la `<ItemStyle>` etiqueta se usa para cambiar el estilo de los elementos dentro de la **DetailsView** control cerca de la parte inferior de la página. Al ver la página en la **vista Diseño** (seleccionando **Diseño** en la esquina inferior izquierda de Visual Studio), a continuación, seleccionando el control **DetailsView** y seleccionando la **etiqueta inteligente** (el icono de flecha en la parte superior derecha del control), podrá ver las tareas **de DetailsView**.
> 
> ![Pago y pago con PayPal - Editar campos](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Al seleccionar **Editar campos**, aparecerá el cuadro de diálogo **Campos.** En este cuadro de diálogo puede controlar fácilmente las propiedades visuales, como **ItemStyle**, de la **DetailsView** control.
> 
> ![Pago y pago con PayPal - Diálogo de campos](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Compra completa

*CheckoutComplete.aspx* página realiza la compra de PayPal. Como se mencionó anteriormente, el usuario debe hacer clic en el completar **pedido** botón antes de que la aplicación navegará a la *CheckoutComplete.aspx* página.

1. En la carpeta *Desprotección* , abra la página denominada *CheckoutComplete.aspx*.
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Abra la página de código subyacente denominada *CheckoutComplete.aspx.cs* y reemplace el código existente por lo siguiente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Cuando se carga la página *CheckoutComplete.aspx,* se llama al `DoCheckoutPayment` método. Como se mencionó `DoCheckoutPayment` anteriormente, el método completa la compra desde el entorno de pruebas de PayPal. Una vez que PayPal ha completado la compra del pedido, `ID` la página *CheckoutComplete.aspx* muestra una transacción de pago al comprador.

### <a name="handle-cancel-purchase"></a>Manejar Cancelar compra

Si el usuario decide cancelar la compra, se le dirigirá a la *CheckoutCancel.aspx* página donde verá que su pedido se ha cancelado.

1. Abra la página denominada *CheckoutCancel.aspx* en la carpeta *de desprotección.*
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Manejar errores de compra

Los errores durante el proceso de compra se controlarán mediante el *CheckoutError.aspx* página. El código subyacente de la *CheckoutStart.aspx* página, el *CheckoutReview.aspx* página y el *CheckoutComplete.aspx* página redireccionará a la *CheckoutError.aspx* página si se produce un error.

1. Abra la página denominada *CheckoutError.aspx* en la carpeta *de desprotección.*
2. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

El *CheckoutError.aspx* página se muestra con los detalles del error cuando se produce un error durante el proceso de desprotección.

## <a name="running-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación para ver cómo comprar productos. Tenga en cuenta que se va a ejecutar en el entorno de pruebas de PayPal. No se está intercambiando dinero real.

1. Asegúrese de que todos los archivos se guardan en Visual Studio.
2. Abra un explorador web [https://developer.paypal.com](https://developer.paypal.com/)y vaya a .
3. Inicie sesión con su cuenta de desarrollador de PayPal que creó anteriormente en este tutorial.  
   Para el entorno limitado para desarrolladores de [https://developer.paypal.com](https://developer.paypal.com/) PayPal, debes iniciar sesión para probar el pago exprés. Esto solo se aplica a las pruebas de sandbox de PayPal, no al entorno en vivo de PayPal.
4. En Visual Studio, presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
   Después de que la base de datos se reconstruye, el explorador se abrirá y mostrará el *Default.aspx* página.
5. Agregue tres productos diferentes al carro de la compra seleccionando la categoría de producto, como "Coches" y, a continuación, haciendo clic en **Agregar al carrito** junto a cada producto.  
   El carro de la compra mostrará el producto que ha seleccionado.
6. Haga clic en el botón **PayPal** para realizar el pago. 

    ![Pago y pago con PayPal - Carrito](checkout-and-payment-with-paypal/_static/image20.png)

   La desproteger requerirá que tenga una cuenta de usuario para la aplicación de ejemplo Wingtip Toys.
7. Haz clic en el enlace de **Google** situado a la derecha de la página para iniciar sesión con una cuenta de correo electrónico gmail.com existente.  
   Si no tiene una cuenta de gmail.com, puede crear una con fines de prueba en [www.gmail.com](https://www.gmail.com/). También puede utilizar una cuenta local estándar haciendo clic en "Registrarse". 

    ![Pago y pago con PayPal - Iniciar sesión](checkout-and-payment-with-paypal/_static/image21.png)
8. Inicia sesión con tu cuenta de gmail y contraseña. 

    ![Pago y pago con PayPal - Gmail Inicia sesión](checkout-and-payment-with-paypal/_static/image22.png)
9. Haga clic en el botón **Iniciar sesión** para registrar su cuenta de gmail con su nombre de usuario de aplicación de ejemplo Wingtip Toys. 

    ![Pago y pago con PayPal - Registrar cuenta](checkout-and-payment-with-paypal/_static/image23.png)
10. En el sitio de prueba de PayPal, agregue la dirección de correo electrónico y la contraseña del **comprador** que creó anteriormente en este tutorial y, a continuación, haga clic en el botón **Iniciar sesión.** 

    ![Pago y pago con PayPal - PayPal Inicia sesión](checkout-and-payment-with-paypal/_static/image24.png)
11. Acepte la política de PayPal y haga clic en el botón **Aceptar y continuar.**  
    Tenga en cuenta que esta página solo se muestra la primera vez que utiliza esta cuenta PayPal. Una vez más tenga en cuenta que esta es una cuenta de prueba, no se intercambia dinero real. 

    ![Pago y pago con PayPal - Política de PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Revise la información del pedido en la página de revisión del entorno de pruebas de PayPal y haga clic en **Continuar**. 

    ![Pago y pago con PayPal - Información de revisión](checkout-and-payment-with-paypal/_static/image26.png)
13. En el *CheckoutReview.aspx* página, compruebe el importe del pedido y ver la dirección de envío generada. A continuación, haga clic en el botón **Completar pedido.** 

    ![Pago y pago con PayPal - Revisión de pedidos](checkout-and-payment-with-paypal/_static/image27.png)
14. El **CheckoutComplete.aspx** página se muestra con un identificador de transacción de pago. 

    ![Pago y pago con PayPal - Checkout Completo](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisión de la base de datos

Al revisar los datos actualizados en la base de datos de aplicación de ejemplo Wingtip Toys después de ejecutar la aplicación, puede ver que la aplicación registró correctamente la compra de los productos.

Puede inspeccionar los datos contenidos en el archivo de base de datos *Wingtiptoys.mdf* mediante la ventana Explorador de **bases** de datos (ventana**Explorador** de servidores en Visual Studio) como lo hizo anteriormente en esta serie de tutoriales.

1. Cierre la ventana del navegador si sigue abierta.
2. En Visual Studio, seleccione el icono **Mostrar todos los archivos** en la parte superior del **Explorador** de soluciones para permitirle expandir la carpeta Datos de la **aplicación.\_**
3. Expanda la carpeta Datos de la **aplicación.\_**  
 Es posible que deba seleccionar el icono **Mostrar todos los archivos** de la carpeta.
4. Haga clic con el botón derecho en el archivo de base de datos *Wingtiptoys.mdf* y seleccione **Abrir**.  
    **Se** muestra el Explorador de servidores.
5. Expanda la carpeta **Tablas** .
6. Haga clic con el botón derecho en la tabla **Pedidos**y seleccione **Mostrar datos**de tabla .  
 Se muestra la tabla **Pedidos.**
7. Revise la columna **PaymentTransactionID** para confirmar las transacciones correctas. 

    ![Pago y pago con PayPal - Base de datos de opiniones](checkout-and-payment-with-paypal/_static/image29.png)
8. Cierre la ventana de la tabla **Pedidos.**
9. En el Explorador de servidores, haga clic con el botón derecho en la tabla **OrderDetails** y seleccione **Mostrar datos**de tabla .
10. Revise `OrderId` los `Username` valores y los valores de la tabla **OrderDetails.** Tenga en cuenta `OrderId` que `Username` estos valores coinciden con los valores incluidos en la tabla **Pedidos.**
11. Cierre la ventana de la tabla **OrderDetails.**
12. Haga clic con el botón derecho en el archivo de base de datos Wingtip Toys (*Wingtiptoys.mdf*) y seleccione **Cerrar conexión**.
13. Si no ve la ventana **Explorador** de soluciones, haga clic en **Explorador** de soluciones en la parte inferior de la ventana **Explorador** de servidores para volver a mostrar el **Explorador** de soluciones.

## <a name="summary"></a>Resumen

En este tutorial ha agregado esquemas de detalle de pedido y pedido para realizar un seguimiento de la compra de productos. También integró la funcionalidad de PayPal en la aplicación de ejemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionales

[Descripción general de la configuración de ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Implemente una aplicación Secure ASP.NET Web Forms con Membership, OAuth y SQL Database en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - Prueba gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Renuncia de responsabilidades

Este tutorial contiene código de ejemplo. Dicho código de muestra se proporciona "tal cual" sin garantía de ningún tipo. En consecuencia, Microsoft no garantiza la precisión, integridad o calidad del código de ejemplo. Usted acepta utilizar el código de muestra bajo su propio riesgo. Bajo ninguna circunstancia Microsoft será responsable ante usted de ninguna manera por cualquier código de muestra, contenido, incluyendo pero no limitado a, cualquier error u omisión en cualquier código de muestra, contenido, o cualquier pérdida o daño de cualquier tipo incurrido como resultado del uso de cualquier código de muestra. Por la presente se le notifica y acepta indemnizar, guardar y eximir a Microsoft de cualquier pérdida, reclamación de pérdida, lesión o daño de cualquier tipo, incluidos, entre otros, los ocasionados o derivados del material en el que publique, transmita, utilice o confíe, incluidas, entre otras, las opiniones expresadas en las mismas.

> [!div class="step-by-step"]
> [Anterior](shopping-cart.md)
> [Siguiente](membership-and-administration.md)
