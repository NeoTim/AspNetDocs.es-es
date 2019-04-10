---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introducción a ASP.NET MVC | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: dcc2e703829cfa0b77575870feff451fd0738f56
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59416498"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="3dae3-104">Introducción a ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3dae3-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="3dae3-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3dae3-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="3dae3-106">Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="3dae3-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="3dae3-107">El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3dae3-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="3dae3-108">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3dae3-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="3dae3-109">Creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3dae3-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="3dae3-110">Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="3dae3-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="3dae3-111">Vamos a crear nuestra primera aplicación Web ASP.NET MVC usando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="3dae3-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="3dae3-112">Nos aseguraremos de hacer una pequeña aplicación de lista de películas que nos gustaría crear y lista de películas.</span><span class="sxs-lookup"><span data-stu-id="3dae3-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="3dae3-113">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="3dae3-113">What You'll Build</span></span>

<span data-ttu-id="3dae3-114">Estos son dos capturas de pantalla de la aplicación que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="3dae3-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="3dae3-115">Tendrá una sencilla tabla de películas con varias columnas.</span><span class="sxs-lookup"><span data-stu-id="3dae3-115">You will have a simple table of movies with various columns.</span></span>

[![M<span data-ttu-id="3dae3-116">ovie lista - Windows Internet Explorer (12)]</span><span class="sxs-lookup"><span data-stu-id="3dae3-116">ovie List - Windows Internet Explorer (12)]</span></span>(getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

<span data-ttu-id="3dae3-117">Y tendrá un formulario de creación, por lo que podemos agregar a la lista de películas.</span><span class="sxs-lookup"><span data-stu-id="3dae3-117">And you'll have a Create Form so we can add movies to the list.</span></span>

[![C<span data-ttu-id="3dae3-118">rear una película - Windows Internet Explorer (2)]</span><span class="sxs-lookup"><span data-stu-id="3dae3-118">reate a Movie - Windows Internet Explorer (2)]</span></span>(getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="3dae3-119">Habilidades que aprenderá</span><span class="sxs-lookup"><span data-stu-id="3dae3-119">Skills You'll Learn</span></span>

<span data-ttu-id="3dae3-120">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3dae3-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="3dae3-121">Aprenderá lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3dae3-121">You'll learn:</span></span>

- <span data-ttu-id="3dae3-122">Cómo crear un nuevo proyecto de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3dae3-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="3dae3-123">Cómo crear una nueva base de datos con SQL Server</span><span class="sxs-lookup"><span data-stu-id="3dae3-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="3dae3-124">Cómo crear vistas y controladores de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3dae3-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="3dae3-125">Cómo recuperar y mostrar datos</span><span class="sxs-lookup"><span data-stu-id="3dae3-125">How to retrieve and display data</span></span>
- <span data-ttu-id="3dae3-126">Cómo editar los datos y habilitar la validación de datos</span><span class="sxs-lookup"><span data-stu-id="3dae3-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="3dae3-127">Cómo actualizar el esquema de base de datos</span><span class="sxs-lookup"><span data-stu-id="3dae3-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="3dae3-128">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="3dae3-128">Get Started</span></span>

<span data-ttu-id="3dae3-129">Comience ejecutando Visual Web Developer 2010 Express (yo lo llamaré "VWD" de ahora en adelante) y seleccione Nuevo proyecto desde la pantalla de inicio.</span><span class="sxs-lookup"><span data-stu-id="3dae3-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="3dae3-130">Visual Web Developer es un IDE o entorno de desarrollo integrado.</span><span class="sxs-lookup"><span data-stu-id="3dae3-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="3dae3-131">Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="3dae3-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="3dae3-132">Hay una barra de herramientas en la parte superior que muestra diversas opciones disponibles para, así como el menú también podría haber utilizado para seleccionar archivo | Nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="3dae3-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

[![M<span data-ttu-id="3dae3-133">icrosoft Visual Web Developer 2010 Express]</span><span class="sxs-lookup"><span data-stu-id="3dae3-133">icrosoft Visual Web Developer 2010 Express]</span></span>(getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="3dae3-134">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="3dae3-134">Creating Your First Application</span></span>

<span data-ttu-id="3dae3-135">Puede crear aplicaciones con Visual Basic o Visual C#.</span><span class="sxs-lookup"><span data-stu-id="3dae3-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="3dae3-136">Por ahora, seleccione Visual C# a la izquierda y, a continuación, elija "Aplicación Web de ASP.NET MVC 2."</span><span class="sxs-lookup"><span data-stu-id="3dae3-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="3dae3-137">Nombre del proyecto "Movies" y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="3dae3-137">Name your project "Movies" and click OK.</span></span>

[![N<span data-ttu-id="3dae3-138">Proyecto ueva]</span><span class="sxs-lookup"><span data-stu-id="3dae3-138">ew Project]</span></span>(getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

<span data-ttu-id="3dae3-139">En el lado derecho es el Explorador de soluciones que muestra todos los archivos y carpetas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3dae3-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="3dae3-140">La ventana grande en la parte central es donde se edite el código y pasan la mayor parte del tiempo.</span><span class="sxs-lookup"><span data-stu-id="3dae3-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="3dae3-141">Visual Studio usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada!</span><span class="sxs-lookup"><span data-stu-id="3dae3-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="3dae3-142">Se trata de un simple "Hola a todos</span><span class="sxs-lookup"><span data-stu-id="3dae3-142">This is a simple "Hello World!</span></span> <span data-ttu-id="3dae3-143">proyecto y es un buen punto de partida para nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="3dae3-143">project, and it is a good place to start for our application.</span></span>

[![M<span data-ttu-id="3dae3-144">icrosoft Visual Web Developer 2010 Express]</span><span class="sxs-lookup"><span data-stu-id="3dae3-144">icrosoft Visual Web Developer 2010 Express]</span></span>(getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

<span data-ttu-id="3dae3-145">Seleccione el botón "reproducir" para la barra de herramientas.</span><span class="sxs-lookup"><span data-stu-id="3dae3-145">Select the "play" button to the toolbar.</span></span>

![Iniciar depuración](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="3dae3-147">Es una flecha verde hacia la derecha que se compila el programa e iniciar la aplicación en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="3dae3-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

*<span data-ttu-id="3dae3-148">NOTA: En su lugar, puede presionar F5 en el teclado, o seleccionar depurar -&gt;iniciar la depuración desde el menú "Depurar".</span><span class="sxs-lookup"><span data-stu-id="3dae3-148">NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.</span></span>*

<span data-ttu-id="3dae3-149">Esto hará que Visual Web Developer iniciar un servidor web de desarrollo y ejecutar la aplicación web (no hay ninguna configuración o pasos manuales necesarios para habilitar esta opción).</span><span class="sxs-lookup"><span data-stu-id="3dae3-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="3dae3-150">A continuación, se ejecutará un explorador y configurarlo para que examine la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3dae3-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="3dae3-151">Tenga en cuenta que a continuación que la barra de direcciones del explorador dice "localhost" y no algo como ejemplo.com.</span><span class="sxs-lookup"><span data-stu-id="3dae3-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="3dae3-152">Eso es porque localhost siempre apunta a su propio equipo local - que en este caso, se ejecuta la aplicación que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="3dae3-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

[![H<span data-ttu-id="3dae3-153">Página ome]</span><span class="sxs-lookup"><span data-stu-id="3dae3-153">ome Page]</span></span>(getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

<span data-ttu-id="3dae3-154">Fuera de la casilla de esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica.</span><span class="sxs-lookup"><span data-stu-id="3dae3-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="3dae3-155">Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso.</span><span class="sxs-lookup"><span data-stu-id="3dae3-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="3dae3-156">Cierre el explorador y permite cambiar parte del código.</span><span class="sxs-lookup"><span data-stu-id="3dae3-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3dae3-157">Siguiente</span><span class="sxs-lookup"><span data-stu-id="3dae3-157">Next</span></span>](getting-started-with-mvc-part2.md)
