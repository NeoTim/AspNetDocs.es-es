---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Agregar validación al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65122779"
---
# <a name="adding-validation-to-the-model"></a><span data-ttu-id="fc814-104">Agregar la validación al modelo</span><span class="sxs-lookup"><span data-stu-id="fc814-104">Adding Validation to the Model</span></span>

<span data-ttu-id="fc814-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fc814-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fc814-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc814-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fc814-107">Creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc814-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fc814-108">Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="fc814-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="fc814-109">En esta sección vamos a implementar la compatibilidad necesaria para habilitar la validación de entrada dentro de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc814-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="fc814-110">Le garantizamos que el contenido de la base de datos siempre es correcto y proporcionar mensajes de error útiles a los usuarios finales cuando pruebe y escriba los datos de la película que no es válidos.</span><span class="sxs-lookup"><span data-stu-id="fc814-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="fc814-111">Empezaremos por agregar lógica de validación de un poco a la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="fc814-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="fc814-112">Haga clic con el botón derecho en la carpeta de modelo y seleccione Agregar clase.</span><span class="sxs-lookup"><span data-stu-id="fc814-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="fc814-113">Nombre de la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="fc814-113">Name your class Movie.</span></span>

<span data-ttu-id="fc814-114">Cuando se creó el modelo de entidades de película, el IDE crea una clase llamada Movie.</span><span class="sxs-lookup"><span data-stu-id="fc814-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="fc814-115">De hecho, puede ser parte de la clase de la película en un archivo y parte en otro.</span><span class="sxs-lookup"><span data-stu-id="fc814-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="fc814-116">Esto se denomina una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="fc814-116">This is called a Partial Class.</span></span> <span data-ttu-id="fc814-117">Vamos a ampliar la clase Movie de otro archivo.</span><span class="sxs-lookup"><span data-stu-id="fc814-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="fc814-118">Vamos a crear una clase parcial de película que apunta a un amigo "class" con algunos atributos que proporcionan las sugerencias de validación en el sistema.</span><span class="sxs-lookup"><span data-stu-id="fc814-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="fc814-119">Se deberá marcar el título y el precio según sea necesario y también insistir en que el precio ser dentro de un intervalo determinado.</span><span class="sxs-lookup"><span data-stu-id="fc814-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="fc814-120">A la derecha, haga clic en la carpeta Models y seleccione Agregar clase.</span><span class="sxs-lookup"><span data-stu-id="fc814-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="fc814-121">Nombre de la clase Movie y haga clic en el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="fc814-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="fc814-122">Este es nuestro parcial película clase aspecto.</span><span class="sxs-lookup"><span data-stu-id="fc814-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="fc814-123">Vuelva a ejecutar la aplicación e intenta insertar una película con un precio superior a 100.</span><span class="sxs-lookup"><span data-stu-id="fc814-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="fc814-124">Obtendrá un error después de haber enviado el formulario.</span><span class="sxs-lookup"><span data-stu-id="fc814-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="fc814-125">El error se captura en el servidor y se produce después de que el formulario se publica.</span><span class="sxs-lookup"><span data-stu-id="fc814-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="fc814-126">Tenga en cuenta cómo aplicaciones auxiliares de HTML integradas de ASP.NET MVC eran suficientemente inteligentes como para mostrar el mensaje de error y mantenga los valores para que podamos dentro de los elementos de cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="fc814-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="fc814-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc814-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="fc814-128">Esto funciona bien, pero sería bueno si podemos decirle al usuario en el lado cliente, inmediatamente, antes de que el servidor se implique.</span><span class="sxs-lookup"><span data-stu-id="fc814-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="fc814-129">Vamos a habilitar alguna validación del lado cliente con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fc814-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="fc814-130">Agregar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="fc814-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="fc814-131">Dado que nuestra clase Movie ya tiene algunos atributos de validación, sólo necesitaremos agregar algunos archivos de JavaScript a nuestra plantilla de vista Create.aspx y agregar una línea de código para habilitar la validación del lado cliente que tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fc814-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="fc814-132">Desde VWD vaya la carpeta vistas/película y abra Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="fc814-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="fc814-133">Abra la carpeta de Scripts en el Explorador de soluciones y arrastre las siguientes tres secuencias de comandos dentro de la &lt;head&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="fc814-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="fc814-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="fc814-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="fc814-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="fc814-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="fc814-136">Desea que estos archivos de script que aparecen en este orden.</span><span class="sxs-lookup"><span data-stu-id="fc814-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="fc814-137">Además, agregue esta línea por encima de la Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="fc814-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="fc814-138">Este es el código que se muestra en el IDE.</span><span class="sxs-lookup"><span data-stu-id="fc814-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="fc814-139">[![Películas - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fc814-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="fc814-140">Ejecute la aplicación y vuelve a visitar /Movies/Create y haga clic en crear sin especificar ningún dato.</span><span class="sxs-lookup"><span data-stu-id="fc814-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="fc814-141">Los mensajes de error aparecen inmediatamente sin la página que se asocia con el envío de datos de flash remontándose al servidor.</span><span class="sxs-lookup"><span data-stu-id="fc814-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="fc814-142">Esto es porque ASP.NET MVC ahora se está validando la entrada tanto en el cliente (mediante JavaScript) y en el servidor.</span><span class="sxs-lookup"><span data-stu-id="fc814-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="fc814-143">[![Crear - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fc814-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="fc814-144">¡Esto es genial!</span><span class="sxs-lookup"><span data-stu-id="fc814-144">This is looking good!</span></span> <span data-ttu-id="fc814-145">Ahora agreguemos una columna adicional a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc814-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc814-146">[Anterior](getting-started-with-mvc-part6.md)
> [Siguiente](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="fc814-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
