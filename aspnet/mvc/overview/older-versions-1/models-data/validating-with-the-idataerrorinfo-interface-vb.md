---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validación de la interfaz IDataErrorInfo (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther muestra cómo mostrar los mensajes de error de validación personalizada implementando la interfaz IDataErrorInfo en una clase de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8ff3adc5db8114dcca2c66d937e1706f0bac0d30
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65117546"
---
# <a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="7fb3c-103">Validación de la interfaz IDataErrorInfo (VB)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-103">Validating with the IDataErrorInfo Interface (VB)</span></span>

<span data-ttu-id="7fb3c-104">by [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="7fb3c-105">Stephen Walther muestra cómo mostrar los mensajes de error de validación personalizada implementando la interfaz IDataErrorInfo en una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>

<span data-ttu-id="7fb3c-106">El objetivo de este tutorial es explicar un método para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="7fb3c-107">Aprenda a evitar que alguien envíe un formulario HTML sin proporcionar valores para los campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="7fb3c-108">En este tutorial, aprenderá a realizar la validación mediante la interfaz IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="7fb3c-109">Suposiciones</span><span class="sxs-lookup"><span data-stu-id="7fb3c-109">Assumptions</span></span>

<span data-ttu-id="7fb3c-110">En este tutorial, usará la base de datos MoviesDB y la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="7fb3c-111">Esta tabla tiene las columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="7fb3c-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>

| <span data-ttu-id="7fb3c-112">**Nombre de columna**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-112">**Column Name**</span></span> | <span data-ttu-id="7fb3c-113">**Tipo de datos**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-113">**Data Type**</span></span> | <span data-ttu-id="7fb3c-114">**Permitir valores null**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7fb3c-115">Id.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-115">Id</span></span> | <span data-ttu-id="7fb3c-116">Valor int.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-116">Int</span></span> | <span data-ttu-id="7fb3c-117">False</span><span class="sxs-lookup"><span data-stu-id="7fb3c-117">False</span></span> |
| <span data-ttu-id="7fb3c-118">Título</span><span class="sxs-lookup"><span data-stu-id="7fb3c-118">Title</span></span> | <span data-ttu-id="7fb3c-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-119">Nvarchar(100)</span></span> | <span data-ttu-id="7fb3c-120">False</span><span class="sxs-lookup"><span data-stu-id="7fb3c-120">False</span></span> |
| <span data-ttu-id="7fb3c-121">Director</span><span class="sxs-lookup"><span data-stu-id="7fb3c-121">Director</span></span> | <span data-ttu-id="7fb3c-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-122">Nvarchar(100)</span></span> | <span data-ttu-id="7fb3c-123">False</span><span class="sxs-lookup"><span data-stu-id="7fb3c-123">False</span></span> |
| <span data-ttu-id="7fb3c-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="7fb3c-124">DateReleased</span></span> | <span data-ttu-id="7fb3c-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="7fb3c-125">DateTime</span></span> | <span data-ttu-id="7fb3c-126">False</span><span class="sxs-lookup"><span data-stu-id="7fb3c-126">False</span></span> |

<span data-ttu-id="7fb3c-127">En este tutorial, usar Microsoft Entity Framework para generar mis clases de modelo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="7fb3c-128">La clase Movie generada por Entity Framework se muestra en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>

<span data-ttu-id="7fb3c-129">[![La entidad de película](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="7fb3c-130">**Figura 01**: La entidad de la película ([haga clic aquí para ver imagen en tamaño completo](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7fb3c-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7fb3c-131">Para más información sobre cómo usar Entity Framework para generar las clases de modelo de base de datos, vea que mi tutorial titulada Crear clases de modelo con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>

## <a name="the-controller-class"></a><span data-ttu-id="7fb3c-132">La clase de controlador</span><span class="sxs-lookup"><span data-stu-id="7fb3c-132">The Controller Class</span></span>

<span data-ttu-id="7fb3c-133">Se usa el controlador Home para películas de lista y creación nuevas películas.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="7fb3c-134">El código para esta clase está contenido en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="7fb3c-135">**Listing 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="7fb3c-136">La clase de controlador Home en el listado 1 contiene dos acciones Create().</span><span class="sxs-lookup"><span data-stu-id="7fb3c-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="7fb3c-137">La primera acción muestra el formulario HTML para crear una película nueva.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="7fb3c-138">La segunda acción Create() realiza la inserción real de la película nuevo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="7fb3c-139">La segunda acción Create() se invoca cuando se envía el formulario mostrado por la primera acción Create() al servidor.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="7fb3c-140">Tenga en cuenta que la segunda acción Create() contiene las siguientes líneas de código:</span><span class="sxs-lookup"><span data-stu-id="7fb3c-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="7fb3c-141">La propiedad IsValid devuelve false cuando hay un error de validación.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="7fb3c-142">En ese caso, se vuelve a mostrar en la vista de creación que contiene el formulario HTML para crear una película.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="7fb3c-143">Creación de una clase parcial</span><span class="sxs-lookup"><span data-stu-id="7fb3c-143">Creating a Partial Class</span></span>

<span data-ttu-id="7fb3c-144">La clase Movie se genera por Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="7fb3c-145">Puede ver el código de la clase Movie si expandir el archivo MoviesDBModel.edmx en la ventana Explorador de soluciones y abra el archivo MoviesDBModel.Designer.vb en el Editor de código (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="7fb3c-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>

<span data-ttu-id="7fb3c-146">[![El código de la entidad de película](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="7fb3c-147">**Figura 02**: El código de la entidad de la película ([haga clic aquí para ver imagen en tamaño completo](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="7fb3c-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>

<span data-ttu-id="7fb3c-148">La clase Movie es una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-148">The Movie class is a partial class.</span></span> <span data-ttu-id="7fb3c-149">Esto significa que podemos agregar otra clase parcial con el mismo nombre para ampliar la funcionalidad de la clase de la película.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="7fb3c-150">Vamos a agregar la lógica de validación a la nueva clase parcial.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="7fb3c-151">En la carpeta Models, agregue la clase en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="7fb3c-152">**Listado 2 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="7fb3c-153">Tenga en cuenta que la clase en el listado 2 incluye la *parcial* modificador.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="7fb3c-154">Los métodos o propiedades que se agregan a esta clase se convierten en parte de la clase Movie generada por Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="7fb3c-155">Agregar OnChanging y métodos parciales OnChanged</span><span class="sxs-lookup"><span data-stu-id="7fb3c-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="7fb3c-156">Cuando Entity Framework genera una clase de entidad, Entity Framework agrega automáticamente a la clase los métodos parciales.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="7fb3c-157">Entity Framework genera OnChanging y OnChanged métodos parciales que corresponden a cada propiedad de la clase.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="7fb3c-158">En el caso de la clase Movie, Entity Framework crea los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="7fb3c-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="7fb3c-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="7fb3c-159">OnIdChanging</span></span>
- <span data-ttu-id="7fb3c-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="7fb3c-160">OnIdChanged</span></span>
- <span data-ttu-id="7fb3c-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="7fb3c-161">OnTitleChanging</span></span>
- <span data-ttu-id="7fb3c-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="7fb3c-162">OnTitleChanged</span></span>
- <span data-ttu-id="7fb3c-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="7fb3c-163">OnDirectorChanging</span></span>
- <span data-ttu-id="7fb3c-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="7fb3c-164">OnDirectorChanged</span></span>
- <span data-ttu-id="7fb3c-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="7fb3c-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="7fb3c-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="7fb3c-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="7fb3c-167">Antes de que cambie la propiedad correspondiente, se llama al método de OnChanging derecho.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="7fb3c-168">Después de cambiar la propiedad, se llama al método de OnChanged derecho.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="7fb3c-169">Puede aprovechar estos métodos parciales para agregar lógica de validación a la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="7fb3c-170">La actualización de la clase de la película en el listado 3 comprueba que las propiedades Title y Director se asignan valores no vacíos.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7fb3c-171">Un método parcial es un método definido en una clase que no son necesarios para implementar.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="7fb3c-172">Si no se implementa un método parcial, a continuación, el compilador quita la firma del método y todas las llamadas al método ahí no están costos de tiempo de ejecución asociados con el método parcial.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="7fb3c-173">En el Editor de código de Visual Studio, puede agregar un método parcial escribiendo la palabra clave *parcial* seguida por un espacio para ver una lista de parciales para implementar.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>

<span data-ttu-id="7fb3c-174">**Listado 3 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="7fb3c-175">Por ejemplo, si intenta asignar una cadena vacía a la propiedad Title, un mensaje de error se asigna a un diccionario denominado \_errores.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="7fb3c-176">En este momento, realmente no ocurre nada cuando se asigna una cadena vacía a la propiedad Title y se agrega un error a la privada \_campo de errores.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="7fb3c-177">Es necesario implementar la interfaz IDataErrorInfo para exponer estos errores de validación para el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="7fb3c-178">Implementar la interfaz IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="7fb3c-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="7fb3c-179">La interfaz IDataErrorInfo ha formado parte de .NET framework desde la primera versión.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="7fb3c-180">Esta interfaz es una interfaz muy simple:</span><span class="sxs-lookup"><span data-stu-id="7fb3c-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="7fb3c-181">Si una clase implementa la interfaz IDataErrorInfo, el marco de MVC de ASP.NET usará esta interfaz cuando se crea una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="7fb3c-182">Por ejemplo, el controlador Home Create() acción acepta una instancia de la clase Movie:</span><span class="sxs-lookup"><span data-stu-id="7fb3c-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="7fb3c-183">El marco de MVC de ASP.NET crea la instancia de la película que se pasan a la acción Create() mediante el uso de un enlazador de modelos (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="7fb3c-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="7fb3c-184">El enlazador de modelos es responsable de crear una instancia del objeto de película enlazando los campos del formulario HTML en una instancia del objeto de película.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="7fb3c-185">DefaultModelBinder detecta si una clase implementa la interfaz IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="7fb3c-186">Si una clase implementa esta interfaz, a continuación, el enlazador de modelos invoca el indizador IDataErrorInfo.this para cada propiedad de la clase.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="7fb3c-187">Si el indizador devuelve un mensaje de error, a continuación, el enlazador de modelos agrega este mensaje de error para modelar el estado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="7fb3c-188">DefaultModelBinder también comprueba la propiedad IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="7fb3c-189">Esta propiedad está pensada para representar errores de validación específica que no son de propiedad asociados a la clase.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="7fb3c-190">Por ejemplo, es posible que desee aplicar una regla de validación que depende de los valores de varias propiedades de la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="7fb3c-191">En ese caso, se devolvería un error de validación de la propiedad de Error.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="7fb3c-192">La clase Movie actualizada en el listado 4 implementa la interfaz IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="7fb3c-193">**Listado 4 - Models\Movie.vb (implementa IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="7fb3c-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="7fb3c-194">En el listado 4, se comprueba la propiedad de indizador el \_colección de errores para ver si contiene una clave que se corresponde con el nombre de propiedad se pasa al indizador.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="7fb3c-195">Si no hay ningún error de validación asociado con la propiedad se devuelve una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="7fb3c-196">No es necesario modificar el controlador Home en alguna forma de usar la clase Movie modificada.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="7fb3c-197">La página se muestra en la figura 3 muestra lo que sucede cuando se especifica ningún valor para el título o Director campos del formulario.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>

<span data-ttu-id="7fb3c-198">[![Creación automática de los métodos de acción](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="7fb3c-199">**Figura 03**: Un formulario con los valores que faltan ([haga clic aquí para ver imagen en tamaño completo](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7fb3c-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>

<span data-ttu-id="7fb3c-200">Tenga en cuenta que el valor DateReleased se valida automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="7fb3c-201">Dado que la propiedad DateReleased no acepta valores NULL, el DefaultModelBinder genera automáticamente un error de validación para esta propiedad cuando no tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="7fb3c-202">Si desea modificar el mensaje de error para la propiedad DateReleased, a continuación, deberá crear un enlazador de modelos personalizado.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="7fb3c-203">Resumen</span><span class="sxs-lookup"><span data-stu-id="7fb3c-203">Summary</span></span>

<span data-ttu-id="7fb3c-204">En este tutorial, ha aprendido a usar la interfaz IDataErrorInfo para generar mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="7fb3c-205">En primer lugar, creamos una clase parcial de película que extiende la funcionalidad de la clase parcial de película generada por Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="7fb3c-206">A continuación, agregamos la lógica de validación a la película clase OnTitleChanging() y OnDirectorChanging() los métodos parciales.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="7fb3c-207">Por último, se implementa la interfaz IDataErrorInfo para exponer estos mensajes de validación para el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7fb3c-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7fb3c-208">[Anterior](performing-simple-validation-vb.md)
> [Siguiente](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7fb3c-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
