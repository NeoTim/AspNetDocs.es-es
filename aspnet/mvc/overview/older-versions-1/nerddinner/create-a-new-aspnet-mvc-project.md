---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Crear un proyecto de nueva ASP.NET MVC Microsoft Docs
author: rick-anderson
description: El paso 1 muestra cómo poner la estructura básica de la aplicación NerdDinner en su lugar.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 240c8a04cec3c07f434182775d1c519aab029761
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541486"
---
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="85c4d-103">Crear un proyecto de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="85c4d-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="85c4d-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="85c4d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="85c4d-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="85c4d-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="85c4d-106">Este es el paso 1 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="85c4d-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="85c4d-107">El paso 1 muestra cómo poner la estructura básica de la aplicación NerdDinner en su lugar.</span><span class="sxs-lookup"><span data-stu-id="85c4d-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="85c4d-108">Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="85c4d-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="85c4d-109">NerdDinner Paso 1:&gt;Archivo- Nuevo Proyecto</span><span class="sxs-lookup"><span data-stu-id="85c4d-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="85c4d-110">Comenzaremos nuestra aplicación NerdDinner seleccionando el elemento de menú **Archivo-&gt;Nuevo proyecto** dentro de Visual Studio 2008 o visual web Developer 2008 Express gratuito.</span><span class="sxs-lookup"><span data-stu-id="85c4d-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="85c4d-111">Esto abrirá el cuadro de diálogo "Nuevo proyecto".</span><span class="sxs-lookup"><span data-stu-id="85c4d-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="85c4d-112">Para crear una nueva aplicación ASP.NET MVC, seleccionaremos el nodo "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elegiremos la plantilla de proyecto "ASP.NET MVC Web Application" a la derecha:</span><span class="sxs-lookup"><span data-stu-id="85c4d-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="85c4d-113">*Importante: Asegúrese de que ha descargado e instalado ASP.NET MVC- de lo contrario no aparecerá en el cuadro de diálogo Nuevo proyecto. Puede usar V2 del Instalador de [plataforma web](https://www.microsoft.com/web/downloads/platform.aspx) de Microsoft si aún no lo ha&gt;instalado (ASP.NET MVC está disponible en la sección "Plataformas web- marcos y tiempos de ejecución").*</span><span class="sxs-lookup"><span data-stu-id="85c4d-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="85c4d-114">Vamos a nombrar el nuevo proyecto que vamos a crear "NerdDinner" y luego haga clic en el botón "ok" para crearlo.</span><span class="sxs-lookup"><span data-stu-id="85c4d-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="85c4d-115">Al hacer clic en "aceptar" Visual Studio mostrará un cuadro de diálogo adicional que nos pedirá que, opcionalmente, también creemos un proyecto de prueba unitaria para la nueva aplicación.</span><span class="sxs-lookup"><span data-stu-id="85c4d-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="85c4d-116">Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueban la funcionalidad y el comportamiento de nuestra aplicación (algo que cubriremos cómo hacer más adelante en este tutorial).</span><span class="sxs-lookup"><span data-stu-id="85c4d-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="85c4d-117">El menú desplegable "Marco de pruebas" del cuadro de diálogo anterior se rellena con todas las plantillas de proyecto de prueba unitaria de ASP.NET MVC disponibles instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="85c4d-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="85c4d-118">Las versiones se pueden descargar para NUnit, MBUnit y XUnit.</span><span class="sxs-lookup"><span data-stu-id="85c4d-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="85c4d-119">También se admite el marco de pruebas unitarias de Visual Studio integrado.</span><span class="sxs-lookup"><span data-stu-id="85c4d-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="85c4d-120">*Nota: El marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si está utilizando VS 2008 Standard Edition o Visual Web Developer 2008 Express, tendrá que descargar e instalar las extensiones NUnit, MBUnit o XUnit para ASP.NET MVC para que se muestre este cuadro de diálogo. El cuadro de diálogo no se mostrará si no hay ningún marco de prueba instalado.*</span><span class="sxs-lookup"><span data-stu-id="85c4d-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="85c4d-121">Usaremos el nombre predeterminado "NerdDinner.Tests" para el proyecto de prueba que creamos y usaremos la opción de marco de trabajo "Prueba unitaria de Visual Studio".</span><span class="sxs-lookup"><span data-stu-id="85c4d-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="85c4d-122">Al hacer clic en el botón "ok" Visual Studio creará una solución para nosotros con dos proyectos en él - uno para nuestra aplicación web y otro para nuestras pruebas unitarias:</span><span class="sxs-lookup"><span data-stu-id="85c4d-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="85c4d-123">Examen de la estructura de directorios NerdDinner</span><span class="sxs-lookup"><span data-stu-id="85c4d-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="85c4d-124">Cuando se crea una nueva aplicación ASP.NET MVC con Visual Studio, agrega automáticamente una serie de archivos y directorios al proyecto:</span><span class="sxs-lookup"><span data-stu-id="85c4d-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="85c4d-125">ASP.NET proyectos MVC de forma predeterminada tienen seis directorios de nivel superior:</span><span class="sxs-lookup"><span data-stu-id="85c4d-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="85c4d-126">**Directorio**</span><span class="sxs-lookup"><span data-stu-id="85c4d-126">**Directory**</span></span> | <span data-ttu-id="85c4d-127">**Propósito**</span><span class="sxs-lookup"><span data-stu-id="85c4d-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="85c4d-128">**/Controladores**</span><span class="sxs-lookup"><span data-stu-id="85c4d-128">**/Controllers**</span></span> | <span data-ttu-id="85c4d-129">Dónde coloca clases de controlador que controlan las solicitudes de dirección URL</span><span class="sxs-lookup"><span data-stu-id="85c4d-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="85c4d-130">**/Modelos**</span><span class="sxs-lookup"><span data-stu-id="85c4d-130">**/Models**</span></span> | <span data-ttu-id="85c4d-131">Donde se colocan clases que representan y manipulan datos</span><span class="sxs-lookup"><span data-stu-id="85c4d-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="85c4d-132">**/Vistas**</span><span class="sxs-lookup"><span data-stu-id="85c4d-132">**/Views**</span></span> | <span data-ttu-id="85c4d-133">Dónde se colocan los archivos de plantilla de interfaz de usuario que son responsables de representar la salida</span><span class="sxs-lookup"><span data-stu-id="85c4d-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="85c4d-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="85c4d-134">**/Scripts**</span></span> | <span data-ttu-id="85c4d-135">Dónde se colocan los archivos y scripts de la biblioteca JavaScript (.js)</span><span class="sxs-lookup"><span data-stu-id="85c4d-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="85c4d-136">**/Contenido**</span><span class="sxs-lookup"><span data-stu-id="85c4d-136">**/Content**</span></span> | <span data-ttu-id="85c4d-137">Dónde se colocan archivos CSS e imagen, y otro contenido no dinámico/no JavaScript</span><span class="sxs-lookup"><span data-stu-id="85c4d-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="85c4d-138">**/Datos\_de la aplicación**</span><span class="sxs-lookup"><span data-stu-id="85c4d-138">**/App\_Data**</span></span> | <span data-ttu-id="85c4d-139">Donde almacena los archivos de datos que desea leer/escribir.</span><span class="sxs-lookup"><span data-stu-id="85c4d-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="85c4d-140">ASP.NET MVC no requiere esta estructura.</span><span class="sxs-lookup"><span data-stu-id="85c4d-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="85c4d-141">De hecho, los desarrolladores que trabajan en aplicaciones grandes normalmente dividirán la aplicación en varios proyectos para hacerla más manejable (por ejemplo: las clases de modelo de datos a menudo van en un proyecto de biblioteca de clases independiente de la aplicación web).</span><span class="sxs-lookup"><span data-stu-id="85c4d-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="85c4d-142">La estructura de proyecto predeterminada, sin embargo, proporciona una convención de directorio predeterminada agradable que podemos usar para mantener nuestras preocupaciones de la aplicación limpias.</span><span class="sxs-lookup"><span data-stu-id="85c4d-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="85c4d-143">Cuando expandimos el directorio /Controllers, encontraremos que Visual Studio agregó dos clases de controlador ( HomeController y AccountController) de forma predeterminada al proyecto:</span><span class="sxs-lookup"><span data-stu-id="85c4d-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="85c4d-144">Cuando expandimos el directorio /Views, encontraremos tres subdirectorios – /Home, /Account y /Shared – así como varios archivos de plantilla dentro de ellos también se agregaron al proyecto de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="85c4d-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="85c4d-145">Cuando expandimos los directorios /Content y /Scripts, encontraremos un archivo Site.css que se utiliza para aplicar estilo a todo HTML en el sitio, así como bibliotecas JavaScript que pueden habilitar ASP.NET compatibilidad con AJAX y jQuery dentro de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="85c4d-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="85c4d-146">Cuando expandamos el proyecto NerdDinner.Tests, encontraremos dos clases que contienen pruebas unitarias para nuestras clases de controlador:</span><span class="sxs-lookup"><span data-stu-id="85c4d-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="85c4d-147">Estos archivos predeterminados agregados por Visual Studio nos proporcionan una estructura básica para una aplicación de trabajo- completa con página de inicio, acerca de la página, inicio de sesión de cuenta / inicio de sesión / páginas de registro y una página de error no controlada (todo cableado y trabajando de forma inmediata).</span><span class="sxs-lookup"><span data-stu-id="85c4d-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="85c4d-148">Ejecución de la aplicación NerdDinner</span><span class="sxs-lookup"><span data-stu-id="85c4d-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="85c4d-149">Podemos ejecutar el proyecto eligiendo los elementos de menú **Depurar-&gt;Iniciar depuración** o **Depurar-&gt;Iniciar sin depurar:**</span><span class="sxs-lookup"><span data-stu-id="85c4d-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="85c4d-150">Esto iniciará el servidor Web ASP.NET integrado que viene con Visual Studio y ejecutará nuestra aplicación:</span><span class="sxs-lookup"><span data-stu-id="85c4d-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="85c4d-151">A continuación se muestra la página de inicio de nuestro nuevo proyecto (URL: "/") cuando se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="85c4d-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="85c4d-152">Al hacer clic en la pestaña "Acerca de" se muestra una página sobre (URL: "/Inicio/Acerca de"):</span><span class="sxs-lookup"><span data-stu-id="85c4d-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="85c4d-153">Al hacer clic en el enlace "Iniciar sesión" en la parte superior derecha, nos lleva a una página de inicio de sesión (URL: "/Cuenta/LogOn")</span><span class="sxs-lookup"><span data-stu-id="85c4d-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="85c4d-154">Si no tenemos una cuenta de inicio de sesión, podemos hacer clic en el enlace de registro (URL: "/Cuenta/Registro") para crear una:</span><span class="sxs-lookup"><span data-stu-id="85c4d-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="85c4d-155">El código para implementar la funcionalidad de inicio anterior, y cierre de sesión/registro, se agregó de forma predeterminada cuando creamos nuestro nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="85c4d-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="85c4d-156">Lo usaremos como punto de partida de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="85c4d-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="85c4d-157">Prueba de la aplicación NerdDinner</span><span class="sxs-lookup"><span data-stu-id="85c4d-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="85c4d-158">Si estamos usando la edición Professional o una versión posterior de Visual Studio 2008, podemos usar la compatibilidad integrada con el IDE de pruebas unitarias en Visual Studio para probar el proyecto:</span><span class="sxs-lookup"><span data-stu-id="85c4d-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="85c4d-159">La elección de una de las opciones anteriores abrirá el panel "Resultados de la prueba" dentro del IDE y nos proporcionará el estado de aprobación/fallo en las 27 pruebas unitarias incluidas en nuestro nuevo proyecto que cubren la funcionalidad integrada:</span><span class="sxs-lookup"><span data-stu-id="85c4d-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="85c4d-160">Más adelante en este tutorial hablaremos más sobre las pruebas automatizadas y agregaremos pruebas unitarias adicionales que cubran la funcionalidad de la aplicación que implementamos.</span><span class="sxs-lookup"><span data-stu-id="85c4d-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="85c4d-161">siguiente paso</span><span class="sxs-lookup"><span data-stu-id="85c4d-161">Next Step</span></span>

<span data-ttu-id="85c4d-162">Ahora tenemos una estructura de aplicación básica en su lugar.</span><span class="sxs-lookup"><span data-stu-id="85c4d-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="85c4d-163">Ahora vamos [a crear una base](create-a-database.md)de datos para almacenar los datos de nuestra aplicación .</span><span class="sxs-lookup"><span data-stu-id="85c4d-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85c4d-164">[Anterior](introducing-the-nerddinner-tutorial.md)
> [Siguiente](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="85c4d-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
