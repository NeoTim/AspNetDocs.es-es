---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Registro y la desprotección | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 9 cubre el registro y la desprotección.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: c7151351b087439f17457b254cd9e373af21cae3
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59380904"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="6334e-104">Parte 9: Registro y finalización de la compra</span><span class="sxs-lookup"><span data-stu-id="6334e-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="6334e-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6334e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6334e-106">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="6334e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6334e-107">El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="6334e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6334e-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6334e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6334e-109">Parte 9 cubre el registro y la desprotección.</span><span class="sxs-lookup"><span data-stu-id="6334e-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="6334e-110">En esta sección, crearemos un CheckoutController que recopile información de pago y dirección del comprador.</span><span class="sxs-lookup"><span data-stu-id="6334e-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="6334e-111">Se requerirá que los usuarios se registren con nuestro sitio antes de desproteger, por lo que este controlador requerirá autorización.</span><span class="sxs-lookup"><span data-stu-id="6334e-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="6334e-112">Los usuarios navegará al proceso de pago de su carro de la compra haciendo clic en el botón "Desproteger".</span><span class="sxs-lookup"><span data-stu-id="6334e-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="6334e-113">Si el usuario no ha iniciado sesión, se pedirá a.</span><span class="sxs-lookup"><span data-stu-id="6334e-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="6334e-114">Cuando inician sesión correctamente, el usuario, a continuación, se muestra la vista de direcciones y pagos.</span><span class="sxs-lookup"><span data-stu-id="6334e-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="6334e-115">Una vez que hayan rellenado el formulario y de enviar el pedido, se mostrará la pantalla de confirmación de pedido.</span><span class="sxs-lookup"><span data-stu-id="6334e-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="6334e-116">Al intentar ver un orden que no existe o un pedido que no pertenece a la se mostrará la vista de Error.</span><span class="sxs-lookup"><span data-stu-id="6334e-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="6334e-117">Migrar el carro de la compra</span><span class="sxs-lookup"><span data-stu-id="6334e-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="6334e-118">Mientras el proceso de compra es anónimo, cuando el usuario hace clic en el botón Checkout, le pedirá que registre e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="6334e-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="6334e-119">Los usuarios esperan que se mantendrá su información del carro de la compra entre las visitas, por lo que necesitamos asociar la información del carro de la compra a un usuario cuando se completa el registro o inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6334e-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="6334e-120">Esto es realmente muy sencillo hacer, como nuestra clase ShoppingCart ya tiene un método que se asociará a todos los elementos en el carro actual con un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="6334e-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="6334e-121">Sólo necesitamos llamar a este método cuando un usuario completa el registro o inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6334e-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="6334e-122">Abra el **AccountController** clase que agregamos al que estábamos Configurar pertenencia y autorización.</span><span class="sxs-lookup"><span data-stu-id="6334e-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="6334e-123">Agregar un utilizando la instrucción que haga referencia MvcMusicStore.Models, a continuación, agregue el siguiente método MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="6334e-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="6334e-124">A continuación, modifique la acción posterior a la de inicio de sesión para llamar a MigrateShoppingCart después de que se haya validado el usuario, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="6334e-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="6334e-125">Realizar el mismo cambio en el registro de registrar la acción, inmediatamente después de que se haya creado correctamente la cuenta de usuario:</span><span class="sxs-lookup"><span data-stu-id="6334e-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="6334e-126">Eso es todo: ahora un carro de compra anónimo se transferirán automáticamente a una cuenta de usuario tras el inicio de sesión o registro correcto.</span><span class="sxs-lookup"><span data-stu-id="6334e-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="6334e-127">Crear el CheckoutController</span><span class="sxs-lookup"><span data-stu-id="6334e-127">Creating the CheckoutController</span></span>

<span data-ttu-id="6334e-128">Haga doble clic en la carpeta Controllers y agregue un nuevo controlador al proyecto denominado CheckoutController mediante la plantilla de controlador en blanco.</span><span class="sxs-lookup"><span data-stu-id="6334e-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="6334e-129">En primer lugar, agregue el atributo Authorize encima de la declaración de clase de controlador para requerir a los usuarios registrar antes de la desprotección:</span><span class="sxs-lookup"><span data-stu-id="6334e-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="6334e-130">*Nota: Esto es similar al cambio que hemos realizado anteriormente en el StoreManagerController, pero en ese caso, el atributo Authorize requiere que el usuario sea en un rol de administrador. En el controlador de desprotección, estamos requerir al usuario iniciar sesión pero no se requiera que sean administradores.*</span><span class="sxs-lookup"><span data-stu-id="6334e-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="6334e-131">Por simplicidad, no se trabaja con información de pago en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="6334e-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="6334e-132">En su lugar, se permite que los usuarios desproteger un código promocional.</span><span class="sxs-lookup"><span data-stu-id="6334e-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="6334e-133">Almacenamos este código promocional con una constante denominada PromoCode.</span><span class="sxs-lookup"><span data-stu-id="6334e-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="6334e-134">Como se muestra en el StoreController, declararemos un campo que contenga una instancia de la clase MusicStoreEntities, denominada storeDB.</span><span class="sxs-lookup"><span data-stu-id="6334e-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="6334e-135">Para hacer que el uso de la clase MusicStoreEntities, necesitamos agregar un mediante declaración para el espacio de nombres MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="6334e-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="6334e-136">La parte superior de nuestro controlador desprotección aparece debajo.</span><span class="sxs-lookup"><span data-stu-id="6334e-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="6334e-137">El CheckoutController tendrá las siguientes acciones de controlador:</span><span class="sxs-lookup"><span data-stu-id="6334e-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="6334e-138">**AddressAndPayment (método GET)** mostrará un formulario para permitir al usuario que escriba su información.</span><span class="sxs-lookup"><span data-stu-id="6334e-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="6334e-139">**AddressAndPayment (método POST)** validará la entrada y procesar el pedido.</span><span class="sxs-lookup"><span data-stu-id="6334e-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="6334e-140">**Completa** se mostrará después de que un usuario ha finalizado correctamente el proceso de pago.</span><span class="sxs-lookup"><span data-stu-id="6334e-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="6334e-141">Esta vista incluirá el número de pedido del usuario, como confirmación.</span><span class="sxs-lookup"><span data-stu-id="6334e-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="6334e-142">En primer lugar, vamos a cambiar el nombre de la acción Index del controlador (que se generó cuando se creó el controlador) para AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="6334e-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="6334e-143">Esta acción de controlador solo muestra el formulario de comprobación, por lo que no requiere ninguna información de modelo.</span><span class="sxs-lookup"><span data-stu-id="6334e-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="6334e-144">El método POST AddressAndPayment seguirá el mismo patrón que hemos usado en el StoreManagerController: se intentará acepte el envío del formulario y completar el pedido y volver a mostrará el formulario si se produce un error.</span><span class="sxs-lookup"><span data-stu-id="6334e-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="6334e-145">Después de validar la entrada de formulario cumple con nuestros requisitos de validación para un pedido, se comprobará el valor de formulario PromoCode directamente.</span><span class="sxs-lookup"><span data-stu-id="6334e-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="6334e-146">Suponiendo que todo es correcto, que se guardará la información actualizada con el orden, indicarle al objeto ShoppingCart completar el proceso de pedido y redirigir a completar la acción.</span><span class="sxs-lookup"><span data-stu-id="6334e-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="6334e-147">Tras la finalización correcta del proceso de retirada, los usuarios se redirigirán a la acción del controlador completo.</span><span class="sxs-lookup"><span data-stu-id="6334e-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="6334e-148">Esta acción llevará a cabo una comprobación simple para validar que el orden realmente pertenecen al usuario que ha iniciado sesión antes de mostrar el número de pedido como una confirmación.</span><span class="sxs-lookup"><span data-stu-id="6334e-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="6334e-149">*Nota: La vista de Error se creó automáticamente para nosotros en la carpeta /Views/Shared cuando empezamos con el proyecto.*</span><span class="sxs-lookup"><span data-stu-id="6334e-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="6334e-150">El código completo de CheckoutController es como sigue:</span><span class="sxs-lookup"><span data-stu-id="6334e-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="6334e-151">Agregar la vista de AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="6334e-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="6334e-152">Ahora, vamos a crear la vista AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="6334e-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="6334e-153">Haga doble clic en una de las acciones de controlador AddressAndPayment y agregar una vista denominada AddressAndPayment que está fuertemente tipada como un pedido y utiliza la plantilla de edición, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="6334e-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="6334e-154">Esta vista hará que el uso de dos de las técnicas que hemos examinados durante la compilación de la vista StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="6334e-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="6334e-155">Usaremos Html.EditorForModel() para mostrar los campos de formulario para el modelo de orden</span><span class="sxs-lookup"><span data-stu-id="6334e-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="6334e-156">Aprovechamos las reglas de validación mediante una clase Order con atributos de validación</span><span class="sxs-lookup"><span data-stu-id="6334e-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="6334e-157">Comenzaremos por actualizar el código del formulario para que use Html.EditorForModel(), seguido de un cuadro de texto adicional para el código de promoción.</span><span class="sxs-lookup"><span data-stu-id="6334e-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="6334e-158">El código completo de la vista AddressAndPayment se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="6334e-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="6334e-159">Definir reglas de validación para el pedido</span><span class="sxs-lookup"><span data-stu-id="6334e-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="6334e-160">Ahora que nuestro punto de vista se ha configurado, se configurará las reglas de validación para nuestro modelo de pedido como hicimos anteriormente para el modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="6334e-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="6334e-161">Haga doble clic en la carpeta Models y agregue una clase denominada pedido.</span><span class="sxs-lookup"><span data-stu-id="6334e-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="6334e-162">Además de los atributos de validación que se ha usado previamente para el álbum, también usaremos una expresión Regular para validar la dirección de correo electrónico del usuario.</span><span class="sxs-lookup"><span data-stu-id="6334e-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="6334e-163">Intenta enviar el formulario que falta o información no válida ahora mostrará el mensaje de error con la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="6334e-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="6334e-164">Bien, lo que hicimos la mayoría del trabajo duro para el proceso de retirada; sólo tenemos unos misceláneos para finalizar.</span><span class="sxs-lookup"><span data-stu-id="6334e-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="6334e-165">Necesitamos agregar dos vistas simples y necesitamos que se encargue de la entrega de la información del carro de compra durante el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6334e-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="6334e-166">Agregar la vista completa de desprotección</span><span class="sxs-lookup"><span data-stu-id="6334e-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="6334e-167">La vista completa de desprotección es bastante sencilla, ya que solo se necesita para mostrar el identificador de pedido.</span><span class="sxs-lookup"><span data-stu-id="6334e-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="6334e-168">Haga doble clic en la acción de controlador completa y agregar una vista denominada Complete que está fuertemente tipada como entero.</span><span class="sxs-lookup"><span data-stu-id="6334e-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="6334e-169">Ahora actualizaremos el código de vista para mostrar el identificador de pedido, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="6334e-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="6334e-170">Actualizando la vista de Error</span><span class="sxs-lookup"><span data-stu-id="6334e-170">Updating The Error view</span></span>

<span data-ttu-id="6334e-171">La plantilla predeterminada incluye una vista de Error en la carpeta de vistas compartidas, por lo que se pueda volver a usar en otro lugar en el sitio.</span><span class="sxs-lookup"><span data-stu-id="6334e-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="6334e-172">Esta vista de Error contiene un error muy simple y no utiliza nuestro sitio de diseño, por lo que se actualizará.</span><span class="sxs-lookup"><span data-stu-id="6334e-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="6334e-173">Puesto que se trata de una página de error genérico, el contenido es muy sencillo.</span><span class="sxs-lookup"><span data-stu-id="6334e-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="6334e-174">Incluiremos un mensaje y un vínculo para ir a la página anterior del historial si el usuario desea volver a intentar su acción.</span><span class="sxs-lookup"><span data-stu-id="6334e-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="6334e-175">[Anterior](mvc-music-store-part-8.md)
> [Siguiente](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="6334e-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
