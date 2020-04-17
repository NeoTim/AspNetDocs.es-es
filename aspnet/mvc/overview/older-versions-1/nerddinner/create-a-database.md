---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creacíón de una base de datos | Microsoft Docs
author: rick-anderson
description: El paso 2 muestra los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: d0b87e4a6a27b37d2dbaa6d5b871da477b25586d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541616"
---
# <a name="create-a-database"></a><span data-ttu-id="3f5de-103">Crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="3f5de-103">Create a Database</span></span>

<span data-ttu-id="3f5de-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3f5de-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="3f5de-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="3f5de-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="3f5de-106">Este es el paso 2 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="3f5de-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="3f5de-107">El paso 2 muestra los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="3f5de-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="3f5de-108">Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="3f5de-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="3f5de-109">NerdDinner Paso 2: Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="3f5de-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="3f5de-110">Usaremos una base de datos para almacenar todos los datos de Dinner y RSVP para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="3f5de-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="3f5de-111">Los pasos siguientes muestran la creación de la base de datos mediante la edición gratuita de SQL Server Express (que puede instalar fácilmente mediante V2 del Instalador de plataforma web de [Microsoft).](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="3f5de-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="3f5de-112">Todo el código que escribiremos funciona con SQL Server Express y con el servidor SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="3f5de-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="3f5de-113">Creación de una nueva base de datos de SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="3f5de-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="3f5de-114">Comenzaremos haciendo clic con el botón derecho en nuestro proyecto web y, a continuación, seleccionaremos el comando de menú **Agregar&gt;nuevo elemento:**</span><span class="sxs-lookup"><span data-stu-id="3f5de-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="3f5de-115">Esto abrirá el cuadro de diálogo "Agregar nuevo elemento" de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f5de-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="3f5de-116">Filtraremos por la categoría "Datos" y seleccionaremos la plantilla de elemento "Base de datos de SQL Server":</span><span class="sxs-lookup"><span data-stu-id="3f5de-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="3f5de-117">Nombraremos la base de datos de SQL Server Express que queremos crear "NerdDinner.mdf" y pulsaremos ok.</span><span class="sxs-lookup"><span data-stu-id="3f5de-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="3f5de-118">Visual Studio nos preguntará si queremos agregar este\_archivo a nuestro directorio de datos de aplicaciones (que es un directorio que ya está configurado con ACL de seguridad de lectura y escritura):</span><span class="sxs-lookup"><span data-stu-id="3f5de-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="3f5de-119">Haremos clic en "Sí" y nuestra nueva base de datos se creará y agregará a nuestro Explorador de soluciones:</span><span class="sxs-lookup"><span data-stu-id="3f5de-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="3f5de-120">Creación de tablas dentro de nuestra base de datos</span><span class="sxs-lookup"><span data-stu-id="3f5de-120">Creating Tables within our Database</span></span>

<span data-ttu-id="3f5de-121">Ahora tenemos una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="3f5de-121">We now have a new empty database.</span></span> <span data-ttu-id="3f5de-122">Vamos a agregarle algunas tablas.</span><span class="sxs-lookup"><span data-stu-id="3f5de-122">Let's add some tables to it.</span></span>

<span data-ttu-id="3f5de-123">Para ello navegaremos a la ventana de pestaña "Explorador de servidores" dentro de Visual Studio, que nos permite administrar bases de datos y servidores.</span><span class="sxs-lookup"><span data-stu-id="3f5de-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="3f5de-124">Las bases de datos de\_SQL Server Express almacenadas en la carpeta de datos de la aplicación de nuestra aplicación se mostrarán automáticamente en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="3f5de-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="3f5de-125">Opcionalmente, podemos usar el icono "Conectar a la base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar bases de datos adicionales de SQL Server (tanto locales como remotas) a la lista también:</span><span class="sxs-lookup"><span data-stu-id="3f5de-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="3f5de-126">Agregaremos dos tablas a nuestra base de datos NerdDinner: una para almacenar nuestras cenas y la otra para realizar un seguimiento de las aceptaciones de RSVP en ellas.</span><span class="sxs-lookup"><span data-stu-id="3f5de-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="3f5de-127">Podemos crear nuevas tablas haciendo clic con el botón derecho en la carpeta "Tablas" dentro de nuestra base de datos y eligiendo el comando de menú "Agregar nueva tabla":</span><span class="sxs-lookup"><span data-stu-id="3f5de-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="3f5de-128">Esto abrirá un diseñador de tablas que nos permite configurar el esquema de nuestra tabla.</span><span class="sxs-lookup"><span data-stu-id="3f5de-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="3f5de-129">Para nuestra tabla "Cenas" agregaremos 10 columnas de datos:</span><span class="sxs-lookup"><span data-stu-id="3f5de-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="3f5de-130">Queremos que la columna "DinnerID" sea una clave principal única para la tabla.</span><span class="sxs-lookup"><span data-stu-id="3f5de-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="3f5de-131">Podemos configurar esto haciendo clic con el botón derecho en la columna "DinnerID" y eligiendo el elemento de menú "Establecer clave principal":</span><span class="sxs-lookup"><span data-stu-id="3f5de-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="3f5de-132">Además de convertir DinnerID en una clave principal, también queremos configurarla como una columna de "identidad" cuyo valor se incrementa automáticamente a medida que se agregan nuevas filas de datos a la tabla (lo que significa que la primera fila Dinner insertada tendrá un DinnerID de 1, la segunda fila insertada tendrá un DinnerID de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="3f5de-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="3f5de-133">Podemos hacer esto seleccionando la columna "DinnerID" y luego usar el editor "Propiedades de columna" para establecer la propiedad "(Is Identity)" en la columna en "Sí".</span><span class="sxs-lookup"><span data-stu-id="3f5de-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="3f5de-134">Usaremos los valores predeterminados de identidad estándar (comienza en 1 e incrementa 1 en cada nueva fila de la cena):</span><span class="sxs-lookup"><span data-stu-id="3f5de-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="3f5de-135">A continuación, guardaremos nuestra tabla escribiendo Ctrl-S o mediante el comando de menú **&gt;Archivo- Guardar.**</span><span class="sxs-lookup"><span data-stu-id="3f5de-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="3f5de-136">Esto nos pedirá que asignemos un nombre a la tabla.</span><span class="sxs-lookup"><span data-stu-id="3f5de-136">This will prompt us to name the table.</span></span> <span data-ttu-id="3f5de-137">Lo llamaremos "Cenas":</span><span class="sxs-lookup"><span data-stu-id="3f5de-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="3f5de-138">Nuestra nueva tabla Cenas aparecerá dentro de nuestra base de datos en el explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="3f5de-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="3f5de-139">A continuación, repetiremos los pasos anteriores y crearemos una tabla "RSVP".</span><span class="sxs-lookup"><span data-stu-id="3f5de-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="3f5de-140">Esta tabla con tienen 3 columnas.</span><span class="sxs-lookup"><span data-stu-id="3f5de-140">This table with have 3 columns.</span></span> <span data-ttu-id="3f5de-141">Configuraremos la columna RsvpID como la clave principal y también la convertiremos en una columna de identidad:</span><span class="sxs-lookup"><span data-stu-id="3f5de-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="3f5de-142">Lo guardaremos y le daremos el nombre "RSVP".</span><span class="sxs-lookup"><span data-stu-id="3f5de-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="3f5de-143">Configuración de una relación de clave externa entre tablas</span><span class="sxs-lookup"><span data-stu-id="3f5de-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="3f5de-144">Ahora tenemos dos tablas dentro de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f5de-144">We now have two tables within our database.</span></span> <span data-ttu-id="3f5de-145">Nuestro último paso de diseño de esquema será configurar una relación "uno a varios" entre estas dos tablas, de modo que podamos asociar cada fila Dinner con cero o más filas RSVP que se apliquen a ella.</span><span class="sxs-lookup"><span data-stu-id="3f5de-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="3f5de-146">Para ello, configuraremos la columna "DinnerID" de la tabla RSVP para que tenga una relación de clave externa con la columna "DinnerID" de la tabla "Cenas".</span><span class="sxs-lookup"><span data-stu-id="3f5de-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="3f5de-147">Para ello, abriremos la tabla RSVP dentro del diseñador de tablas haciendo doble clic en ella en el explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="3f5de-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="3f5de-148">A continuación, seleccionaremos la columna "DinnerID" dentro de ella, haga clic con el botón derecho y elegiremos "Relaciones..." comando de menú contextual:</span><span class="sxs-lookup"><span data-stu-id="3f5de-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="3f5de-149">Esto abrirá un cuadro de diálogo que podemos usar para configurar relaciones entre tablas:</span><span class="sxs-lookup"><span data-stu-id="3f5de-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="3f5de-150">Haremos clic en el botón "Agregar" para agregar una nueva relación al cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="3f5de-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="3f5de-151">Una vez que se ha agregado una relación, expandiremos el nodo de vista de árbol "Tablas y especificación de columna" dentro de la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, haremos clic en "..." botón a la derecha de la misma:</span><span class="sxs-lookup"><span data-stu-id="3f5de-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="3f5de-152">Al hacer clic en el botón "..." botón abrirá otro cuadro de diálogo que nos permite especificar qué tablas y columnas están involucradas en la relación, así como nos permite nombrar la relación.</span><span class="sxs-lookup"><span data-stu-id="3f5de-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="3f5de-153">Cambiaremos la tabla de claves principal para que sea "Cenas" y seleccionaremos la columna "DinnerID" dentro de la tabla Cenas como clave principal.</span><span class="sxs-lookup"><span data-stu-id="3f5de-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="3f5de-154">Nuestra tabla RSVP será la tabla de claves externas, y el RSVP. La columna DinnerID se asociará como clave externa:</span><span class="sxs-lookup"><span data-stu-id="3f5de-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="3f5de-155">Ahora cada fila de la tabla RSVP se asociará a una fila en la tabla Dinner.</span><span class="sxs-lookup"><span data-stu-id="3f5de-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="3f5de-156">SQL ServerSQL Server mantendrá la integridad referencial para nosotros y nos impedirá agregar una nueva fila RSVP si no apunta a una fila Dinner válida.</span><span class="sxs-lookup"><span data-stu-id="3f5de-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="3f5de-157">También nos impedirá eliminar una fila De cena si todavía hay filas RSVP que hacen referencia a ella.</span><span class="sxs-lookup"><span data-stu-id="3f5de-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="3f5de-158">Adición de datos a nuestras tablas</span><span class="sxs-lookup"><span data-stu-id="3f5de-158">Adding Data to our Tables</span></span>

<span data-ttu-id="3f5de-159">Terminemos agregando algunos datos de muestra a nuestra tabla Cenas.</span><span class="sxs-lookup"><span data-stu-id="3f5de-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="3f5de-160">Podemos agregar datos a una tabla haciendo clic con el botón derecho en ella en el Explorador de servidores y eligiendo el comando "Mostrar datos de tabla":</span><span class="sxs-lookup"><span data-stu-id="3f5de-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="3f5de-161">Agregaremos algunas filas de datos de cena que podemos usar más adelante a medida que comenzamos a implementar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3f5de-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="3f5de-162">siguiente paso</span><span class="sxs-lookup"><span data-stu-id="3f5de-162">Next Step</span></span>

<span data-ttu-id="3f5de-163">Hemos terminado de crear nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f5de-163">We've finished creating our database.</span></span> <span data-ttu-id="3f5de-164">Ahora vamos a crear clases de modelo que podemos usar para consultarlo y actualizarlo.</span><span class="sxs-lookup"><span data-stu-id="3f5de-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3f5de-165">[Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="3f5de-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
