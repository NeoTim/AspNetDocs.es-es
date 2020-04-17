---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Iteración #1: crear la aplicación (C-) Microsoft Docs'
author: rick-anderson
description: 'En la primera iteración, creamos Contact Manager de la manera más sencilla posible. Agregamos soporte para operaciones básicas de bases de datos: Crear, Leer, Actualizar y D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: ecc3c1201c784e20c6b2601735bee3d4ce721f22
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542552"
---
# <a name="iteration-1--create-the-application-c"></a>Iteración n.º 1: Crear la aplicación (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> En la primera iteración, creamos Contact Manager de la manera más sencilla posible. Agregamos compatibilidad con operaciones básicas de base de datos: Crear, leer, actualizar y eliminar (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de administración de contactos ASP.NET MVC (VB)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto (nombres, números de teléfono y direcciones de correo electrónico) para una lista de personas.

Creamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de iteración múltiple es permitirle comprender el motivo de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, creamos Contact Manager de la manera más sencilla posible. Agregamos compatibilidad con operaciones básicas de base de datos: Crear, leer, actualizar y eliminar (CRUD).

- Iteración #2 - Hacer que la aplicación se vea bien. En esta iteración, mejoramos el aspecto de la aplicación modificando la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos validación básica del formulario. Evitamos que las personas envíen un formulario sin completar los campos de formulario obligatorios. También validamos direcciones de correo electrónico y números de teléfono.

- Iteración #4: haga que la aplicación se acople libremente. En esta cuarta iteración, aprovechamos varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, refactorizamos nuestra aplicación para usar el patrón Repository y el patrón Dependency Injection.

- Iteración #5: crear pruebas unitarias. En la quinta iteración, hacemos que nuestra aplicación sea más fácil de mantener y modificar agregando pruebas unitarias. Nos burlamos de nuestras clases de modelo de datos y creamos pruebas unitarias para nuestros controladores y lógica de validación.

- #6 de iteración: use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- de #7 iteración: agregue la funcionalidad DebAjax. En la séptima iteración, mejoramos la capacidad de respuesta y el rendimiento de nuestra aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta primera iteración, creamos la aplicación básica. El objetivo es construir el Contact Manager de la manera más rápida y sencilla posible. En iteraciones posteriores, mejoramos el diseño de la aplicación.

La aplicación Contact Manager es una aplicación básica controlada por bases de datos. Puede utilizar la aplicación para crear nuevos contactos, editar contactos existentes y eliminar contactos.

En esta iteración, completamos los siguientes pasos:

1. ASP.NET aplicación MVC
2. Crear una base de datos para almacenar nuestros contactos
3. Genere una clase de modelo para nuestra base de datos con Microsoft Entity Framework
4. Crear una acción de controlador y vista que nos permite enumerar todos los contactos en la base de datos
5. Crear acciones de controlador y una vista que nos permita crear un nuevo contacto en la base de datos
6. Crear acciones de controlador y una vista que nos permita editar un contacto existente en la base de datos
7. Crear acciones de controlador y una vista que nos permita eliminar un contacto existente en la base de datos

## <a name="software-prerequisites"></a>Requisitos previos de software

En ASP.NET aplicaciones MVC, debe tener Visual Studio 2008 o Visual Web Developer 2008 instalado en el equipo (Visual Web Developer es una versión gratuita de Visual Studio que no incluye todas las características avanzadas de Visual Studio). Puede descargar la versión de prueba de Visual Studio 2008 o Visual Web Developer desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Para ASP.NET aplicaciones MVC con Visual Web Developer, debe tener Visual Web Developer Service Pack 1 instalado. Sin Service Pack 1, no puede crear proyectos de aplicación web.

ASP.NET marco MVC. Puede descargar el marco de ASP.NET MVC desde la siguiente dirección:

[https://www.asp.net/mvc](../../../index.md)

En este tutorial, usamos Microsoft Entity Framework para tener acceso a una base de datos. Entity Framework se incluye con .NET Framework 3.5 Service Pack 1. Puede descargar este Service Pack desde la siguiente ubicación:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang-en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como alternativa a realizar cada una de estas descargas una por una, puede aprovechar el Instalador de plataforma web (Web PI). Puede descargar el Web PI desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>proyecto MVC ASP.NET

ASP.NET proyecto de aplicación web MVC. Inicie Visual Studio y seleccione la opción de menú **Archivo, Nuevo proyecto**. Aparece el cuadro de diálogo **Nuevo proyecto** (consulte la figura 1). Seleccione el tipo de proyecto **Web** y la plantilla **de aplicación web ASP.NET MVC.** Asigne un nombre al nuevo proyecto *ContactManager* y haga clic en el botón Aceptar.

Asegúrese de que tiene .NET Framework 3.5 seleccionado en la lista desplegable en la parte superior derecha del cuadro de diálogo **Nuevo proyecto.** De lo contrario, la plantilla de aplicación web ASP.NET MVC no aparecerá.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figura 01**: El cuadro de diálogo Nuevo proyecto[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image2.png)

ASP.NET aplicación MVC, aparece el cuadro de diálogo Crear proyecto de **prueba unitaria.** Puede usar este cuadro de diálogo para indicar que desea crear y agregar un proyecto de prueba unitaria a la solución al crear la aplicación ASP.NET MVC. Aunque no vamos a crear pruebas unitarias en esta iteración, debe seleccionar la opción **Sí, crear un proyecto** de prueba unitaria porque planeamos agregar pruebas unitarias en una iteración posterior. Agregar un proyecto de prueba cuando se crea por primera vez un nuevo proyecto de ASP.NET MVC es mucho más fácil que agregar un proyecto de prueba después de que se haya creado el proyecto de ASP.NET MVC.

> [!NOTE] 
> 
> Dado que Visual Web Developer no admite proyectos de prueba, no se obtiene el cuadro de diálogo Crear proyecto de prueba unitaria al usar Visual Web Developer.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figura 02**: El cuadro de diálogo Crear proyecto de prueba unitaria (Haga clic para ver la[imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image4.png)

ASP.NET aplicación MVC aparece en la ventana Explorador de soluciones de Visual Studio (consulte la figura 3). Si no ve la ventana Explorador de soluciones, puede abrir esta ventana seleccionando la opción de menú **Ver, Explorador**de soluciones . Tenga en cuenta que la solución contiene dos proyectos: el proyecto ASP.NET MVC y el proyecto de prueba. El proyecto ASP.NET MVC se denomina ContactManager y el proyecto de prueba se denomina ContactManager.Tests.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figura 03**: La ventana Explorador de soluciones[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image6.png)

## <a name="deleting-the-project-sample-files"></a>Eliminación de los archivos de ejemplo de proyecto

La plantilla de proyecto ASP.NET MVC incluye archivos de ejemplo para controladores y vistas. Antes de crear una nueva aplicación ASP.NET MVC, debe eliminar estos archivos. Puede eliminar archivos y carpetas en la ventana Explorador de soluciones haciendo clic con el botón derecho en un archivo o carpeta y seleccionando la opción de menú **Eliminar**.

Debe eliminar los siguientes archivos del proyecto ASP.NET MVC:

- •Controllers-HomeController.cs

- .Views-Home-About.aspx

- .Views-Home-Index.aspx

Además, debe eliminar el siguiente archivo del proyecto de prueba:

•Controllers-HomeControllerTest.cs

## <a name="creating-the-database"></a>Creación de la base de datos

La aplicación Contact Manager es una aplicación web controlada por base de datos. Utilizamos una base de datos para almacenar la información de contacto.

El ASP.NET framework MVC con cualquier base de datos moderna, incluidas las bases de datos microsoft SQL Server, Oracle, MySQL e IBM DB2. En este tutorial, usamos una base de datos de Microsoft SQL Server. Al instalar Visual Studio, se le proporciona la opción de instalar Microsoft SQL Server Express, que es una versión gratuita de la base de datos de Microsoft SQL Server.

Cree una nueva base de datos\_haciendo clic con el botón derecho en la carpeta Datos de la aplicación en la ventana Explorador de soluciones y seleccionando la opción de menú **Agregar, Nuevo elemento**. En el **agregar nuevo elemento** cuadro de diálogo, seleccione la categoría de **datos** y la plantilla de base de datos de **SQL Server** (consulte la figura 4). Asigne a la nueva base de datos el nombre ContactManagerDB.mdf y haga clic en el botón Aceptar.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figura 04**: Creación de una nueva base de datos de Microsoft SQL Server Express ( Haga clic para ver la[imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image8.png))

Después de crear la nueva base\_de datos, la base de datos aparece en la carpeta Datos de la aplicación en la ventana Explorador de soluciones. Haga doble clic en el archivo ContactManager.mdf para abrir la ventana Explorador de servidores y conectarse a la base de datos.

> [!NOTE] 
> 
> La ventana Explorador de servidores se denomina ventana Explorador de bases de datos en el caso de Microsoft Visual Web Developer.

Puede usar la ventana Explorador de servidores para crear nuevos objetos de base de datos, como tablas de base de datos, vistas, desencadenadores y procedimientos almacenados. Haga clic con el botón derecho en la carpeta Tablas y seleccione la opción de menú **Agregar nueva tabla**. Aparece el Diseñador de tablas de base de datos (consulte la figura 5).

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figura 05**: El Diseñador de tablas de base de datos[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image10.png)

Necesitamos crear una tabla que contenga las siguientes columnas:

<a id="0.1_table01"></a>

| **Nombre de columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Identificador | int | False |
| Nombre | nvarchar(50) | False |
| Apellidos | nvarchar(50) | False |
| Teléfono | nvarchar(50) | False |
| Email | nvarchar(255) | False |

La primera columna, la columna Id, es especial. Debe marcar la columna Id como una columna Identity y una columna Primary Key. Indica que una columna es una columna Identity expandiendo Propiedades de columna (mira en la parte inferior de la figura 6) y desplazándose hacia abajo hasta la propiedad Especificación de identidad. Establezca la propiedad **(Is Identity)** en el valor **Yes**.

Para marcar una columna como una columna de clave principal, seleccione la columna y haga clic en el botón con el icono de una clave. Después de marcar una columna como una columna de clave principal, aparece un icono de una clave junto a la columna (consulte la figura 6).

Cuando haya terminado de crear la tabla, haga clic en el botón Guardar (el botón con un icono de un disquete) para guardar la nueva tabla. Asigne a la nueva tabla el nombre *Contactos*.

Después de terminar de crear la tabla de base de datos De contactos, debe agregar algunos registros a la tabla. Haga clic con el botón derecho en la tabla Contactos en la ventana Explorador de servidores y seleccione la opción de menú **Mostrar datos**de tabla . Introduzca uno o más contactos en la cuadrícula que aparece.

## <a name="creating-the-data-model"></a>Creación del modelo de datos

La aplicación ASP.NET MVC consta de modelos, vistas y controladores. Comenzamos creando una clase Model que representa la tabla Contacts que creamos en la sección anterior.

En este tutorial, usamos Microsoft Entity Framework para generar automáticamente una clase de modelo a partir de la base de datos.

> [!NOTE] 
> 
> El marco de ASP.NET MVC no está vinculado a Microsoft Entity Framework de ninguna manera. Puede usar ASP.NET MVC con tecnologías de acceso de base de datos alternativas, como NHibernate, LINQ to SQL o ADO.NET.

Siga estos pasos para crear las clases de modelo de datos:

1. Haga clic con el botón derecho en la carpeta Modelos de la ventana Explorador de soluciones y seleccione **Agregar, Nuevo elemento**. Aparece el cuadro de diálogo **Agregar nuevo elemento** (consulte la figura 6).
2. Seleccione la categoría **Datos** y la plantilla **ADO.NET Entity Data Model.** Asigne al modelo de datos el nombre *ContactManagerModel.edmx* y haga clic en el botón **Agregar.** Aparece el asistente de Entity Data Model (consulte la figura 7).
3. En el paso Elegir contenido del **modelo,** seleccione **Generar a partir de la base** de datos (consulte la figura 7).
4. En el paso Elegir la conexión de **datos,** seleccione la base de datos ContactManagerDB.mdf y escriba el nombre *ContactManagerDBEntities* para la configuración de conexión de entidad (consulte la figura 8).
5. En el paso Elegir los objetos de base de **datos,** seleccione la casilla de verificación Tablas etiquetadas (consulte la figura 9). El modelo de datos incluirá todas las tablas contenidas en la base de datos (solo hay una, la tabla Contactos). Escriba el espacio de nombres *Modelos*. Haga clic en el botón Finalizar para completar el asistente.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figura 06**: El cuadro de diálogo Agregar nuevo elemento[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image12.png)

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figura 07**: Elija Contenido del modelo ([Haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image14.png))

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figura 08**: Elija su conexión de datos ([Haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image16.png))

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figura 09**: Elija los objetos de base de datos ([Haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image18.png))

Después de completar el Asistente para Entity Data Model, aparece Entity Data Model Designer. El diseñador muestra una clase que corresponde a cada tabla que se está modelando. Debería ver una clase denominada Contactos.

El Asistente para Entity Data Model genera nombres de clase basados en nombres de tabla de base de datos. Casi siempre es necesario cambiar el nombre de la clase generada por el asistente. Haga clic con el botón derecho en la clase Contactos del diseñador y seleccione la opción de menú **Cambiar nombre**. Cambie el nombre de la clase de Contactos (plural) a Contacto (singular). Después de cambiar el nombre de clase, la clase debe aparecer como la figura 10.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figura 10**: La clase Contact[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image20.png)

En este punto, hemos creado nuestro modelo de base de datos. Podemos usar la clase Contact para representar un registro de contacto determinado en nuestra base de datos.

## <a name="creating-the-home-controller"></a>Creación del controlador de inicio

El siguiente paso es crear nuestro controlador Home. El controlador de inicio es el controlador predeterminado invocado en una aplicación ASP.NET MVC.

Cree la clase de controlador home haciendo clic con el botón secundario en la carpeta Controladores en la ventana Explorador de soluciones y seleccionando la opción de menú **Agregar, Controlador** (consulte la figura 11). Observe la casilla de verificación denominada Agregar métodos de **acción para escenarios crear, actualizar y detalles**. Asegúrese de que esta casilla de verificación está marcada antes de hacer clic en el botón **Agregar.**

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figura 11**: Adición del controlador de inicio[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image22.png)

Cuando se crea el controlador de inicio, se obtiene la clase en el listado 1.

**Listado 1 - Controllers-HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Listado de los Contactos

Para mostrar los registros en la tabla de base de datos Contacts, necesitamos crear una acción Index() y una vista Index.

El controlador Home ya contiene una acción Index(). Necesitamos modificar este método para que se parezca al listado 2.

**Listado 2 - Controllers-HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Observe que la clase de controlador Home \_en el listado 2 contiene un campo privado denominado entities. El \_campo entities representa las entidades del modelo de datos. Usamos \_el campo entities para comunicarnos con la base de datos.

El método Index() devuelve una vista que representa todos los contactos de la tabla de base de datos Contacts. Las \_entidades de expresión. ContactSet.ToList() devuelve la lista de contactos como una lista genérica.

Ahora que hemos creado el controlador index, a continuación necesitamos crear la vista de índice. Antes de crear la vista de índice, compile la aplicación seleccionando la opción de menú **Compilar, Compilar solución**. Siempre debe compilar el proyecto antes de agregar una vista para que la lista de clases de modelo se muestre en el cuadro de diálogo **Agregar vista.**

Para crear la vista de índice, haga clic con el botón derecho en el método Index() y seleccione la opción de menú **Agregar vista** (consulte la figura 12). Al seleccionar esta opción de menú se abre el cuadro de diálogo **Agregar vista** (consulte la figura 13).

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figura 12**: Adición de la vista de índice[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image24.png)

En el cuadro de diálogo **Agregar vista,** active la casilla de **verificación Crear una vista fuertemente tipada**. Seleccione la clase de datos De vista ContactManager.Models.Contact y la lista de contenido Ver. Al seleccionar estas opciones se genera una vista que muestra una lista de registros de contacto.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figura 13**: El cuadro de diálogo Agregar vista[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image26.png)

Al hacer clic en el botón **Agregar,** se genera la vista de índice en el listado 3. Tenga &lt;en cuenta&gt; la directiva % , % de página % que aparece en la parte superior del archivo. La vista de índice hereda&lt;de&lt;la ViewPage&gt; &gt; IEnumerable ContactManager.Models.Contact clase. En otras palabras, la clase Model de la vista representa una lista de entidades Contact.

El cuerpo de la vista Index contiene un bucle foreach que recorre en iteración cada uno de los contactos representados por la clase Model. El valor de cada propiedad de la clase Contact se muestra dentro de una tabla HTML.

**Listado 3 - Vistas-Inicio-Index.aspx (sin modificar)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Necesitamos hacer una modificación en la vista de índice. Dado que no estamos creando una vista Detalles, podemos quitar el vínculo Detalles. Busque y quite el siguiente código de la vista de índice:

• id-item. Id)%&gt;

Después de modificar la vista de índice, puede ejecutar la aplicación Contact Manager. Seleccione la opción de menú Depurar, Iniciar depuración o simplemente presione F5. La primera vez que ejecute la aplicación, obtendrá el cuadro de diálogo en la figura 14. Seleccione la opción **Modificar el archivo Web.config para habilitar** la depuración y haga clic en el botón Aceptar.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figura 14**: Habilitación de la depuración[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image28.png)

La vista de índice se devuelve de forma predeterminada. Esta vista enumera todos los datos de la tabla de base de datos de contactos (consulte la figura 15).

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figura 15**: La vista de índice ([Haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image30.png))

Observe que la vista de índice incluye un vínculo con la etiqueta Crear nuevo en la parte inferior de la vista. En la siguiente sección, aprenderás a crear nuevos contactos.

## <a name="creating-new-contacts"></a>Creación de nuevos contactos

Para permitir que los usuarios creen nuevos contactos, necesitamos agregar dos acciones Create() al controlador Home. Necesitamos crear una acción Create() que devuelva un formulario HTML para crear un nuevo contacto. Necesitamos crear una segunda acción Create() que realice la inserción de base de datos real del nuevo contacto.

Los nuevos métodos Create() que necesitamos agregar al controlador Home se incluyen en el listado 4.

**Listado 4 - Controllers-HomeController.cs (con métodos Create)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

El primer método Create() se puede invocar con un HTTP GET, mientras que el segundo método Create() solo se puede invocar mediante http POST. En otras palabras, el segundo método Create() solo se puede invocar al publicar un formulario HTML. El primer método Create() simplemente devuelve una vista que contiene el formulario HTML para crear un nuevo contacto. El segundo método Create() es mucho más interesante: añade el nuevo contacto a la base de datos.

Observe que el segundo método Create() se ha modificado para aceptar una instancia de la clase Contact. Los valores de formulario publicados desde el formulario HTML están enlazados a esta clase Contact por el marco de ASP.NET MVC automáticamente. Cada campo de formulario del formulario Crear HTML se asigna a una propiedad del parámetro Contacto.

Observe que el parámetro Contact está decorado con un atributo [Bind]. El atributo [Bind] se utiliza para excluir la propiedad Contact Id del enlace. Dado que el Id propiedad representa un Identity propiedad, no queremos establecer el Id propiedad.

En el cuerpo del método Create(), Entity Framework se utiliza para insertar el nuevo contacto en la base de datos. El nuevo contacto se agrega al conjunto existente de contactos y se llama al método SaveChanges() para devolver estos cambios a la base de datos subyacente.

Puede generar un formulario HTML para crear nuevos contactos haciendo clic con el botón derecho en cualquiera de los dos métodos Create() y seleccionando la opción de menú **Agregar vista** (consulte la figura 16).

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figura 16**: Adición de la vista Crear[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image32.png)

En el cuadro de diálogo **Agregar vista,** seleccione la clase **ContactManager.Models.Contact** y la opción **Crear** para el contenido de la vista (consulte la figura 17). Al hacer clic en el botón **Agregar,** se genera automáticamente una vista Crear.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figura 17**: Ver una página explosionada[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image34.png)

La vista Crear contiene campos de formulario para cada una de las propiedades de la clase Contact. El código de la vista Crear se incluye en el listado 5.

**Listado 5 - Vistas-Home-Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Después de modificar los métodos Create() y agregar la vista Crear, puede ejecutar la aplicación Contact Manager y crear nuevos contactos. Haga clic en el vínculo **Crear nuevo** que aparece en la vista índice para navegar a la vista Crear. Debería ver la vista en la figura 18.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figura 18**: Crear vista[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image36.png)

## <a name="editing-contacts"></a>Edición de contactos

Agregar la funcionalidad para editar un registro de contacto es muy similar a agregar la funcionalidad para crear nuevos registros de contacto. En primer lugar, necesitamos agregar dos nuevos editalos a la clase de controlador Home. Estos nuevos métodos Edit() se encuentran en el listado 6.

**Listado 6 - Controllers-HomeController.cs (con métodos Edit)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

Una operación HTTP GET invoca el primer método Edit(). Un id parámetro se pasa a este método que representa el identificador del registro de contacto que se está editando. Entity Framework se usa para recuperar un contacto que coincide con el identificador. Se devuelve una vista que contiene un formulario HTML para editar un registro.

El segundo método Edit() realiza la actualización real de la base de datos. Este método acepta una instancia de la Contact clase como un parámetro. El ASP.NET marco de MVC enlaza automáticamente los campos de formulario del formulario Editar a esta clase. Tenga en cuenta que no incluye el atributo [Bind] al editar un contacto (necesitamos el valor de la propiedad Id).

Entity Framework se usa para guardar el contacto modificado en la base de datos. El contacto original debe recuperarse primero de la base de datos. A continuación, se llama al método ApplyPropertyChanges() de Entity Framework para registrar los cambios en el contacto. Por último, se llama al método SaveChanges() de Entity Framework para conservar los cambios en la base de datos subyacente.

Puede generar la vista que contiene el formulario Editar haciendo clic con el botón derecho en el método Edit() y seleccionando la opción de menú Agregar vista. En el cuadro de diálogo Agregar vista, seleccione la clase **ContactManager.Models.Contact** y el contenido de la vista **Editar** (consulte la figura 19).

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figura 19**: Adición de una vista de edición[(haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image38.png)

Al hacer clic en el botón Agregar, se genera automáticamente una nueva vista Editar. El formulario HTML que se genera contiene campos que corresponden a cada una de las propiedades de la clase Contact (consulte el listado 7).

**Listado 7 - Vistas-Home-Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminación de contactos

Si desea eliminar contactos, debe agregar dos acciones Delete() a la clase de controlador Home. La primera acción Delete() muestra un formulario de confirmación de eliminación. La segunda acción Delete() realiza la eliminación real.

> [!NOTE] 
> 
> Más adelante, en iteración #7, modificamos el Contact Manager para que admita una eliminación de Ajax de un paso.

Los dos nuevos métodos Delete() se encuentran en el listado 8.

**Listado 8 - Controllers-HomeController.cs (Métodos de eliminación)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

El primer método Delete() devuelve un formulario de confirmación para eliminar un registro de contacto de la base de datos (consulte la figura 20). El segundo método Delete() realiza la operación de eliminación real en la base de datos. Una vez recuperado el contacto original de la base de datos, se llama a los métodos DeleteObject() y SaveChanges() de Entity Framework para realizar la eliminación de la base de datos.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figura 20**: La vista de confirmación de eliminación[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image40.png)

Necesitamos modificar la vista de índice para que contenga un vínculo para eliminar registros de contacto (consulte la figura 21). Debe agregar el siguiente código a la misma celda de tabla que contiene el vínculo Editar:

Html.ActionLink( á id-item. Id. ) %&gt;

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figura 21**: Vista de índice con vínculo Editar[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image42.png)

A continuación, necesitamos crear la vista de confirmación de eliminación. Haga clic con el botón derecho en el método Delete() en la clase de controlador Home y seleccione la opción de menú Agregar vista. Aparece el cuadro de diálogo Agregar vista (consulte el cuadro 22).

A diferencia de las vistas Lista, Crear y Editar, el cuadro de diálogo Agregar vista no contiene una opción para crear una vista Eliminar. En su lugar, seleccione la clase de datos **ContactManager.Models.Contact** y el contenido de la vista **vacía.** Si selecciona la opción Contenido de vista vacía, nos pedirá que creamos la vista nosotros mismos.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figura 22**: Adición de la vista de confirmación de eliminación[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image44.png)

El contenido de la vista Eliminar se encuentra en el listado 9. Esta vista contiene un formulario que confirma si se debe eliminar o no un contacto determinado (consulte la figura 21).

**Listado 9 - Vistas-Inicio-Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Cambiar el nombre del controlador predeterminado

Puede molestarle que el nombre de nuestra clase de controlador para trabajar con contactos se denomina la clase HomeController. ¿No debería llamarse ContactController el controlador?

Este problema es bastante fácil de solucionar. En primer lugar, tenemos que refactorizar el nombre del controlador de inicio. Abra la clase HomeController en el Editor de código de Visual Studio, haga clic con el botón derecho en el nombre de la clase y seleccione la opción de menú **Refactorizar, Cambiar nombre**. Al seleccionar esta opción de menú se abre el cuadro de diálogo Cambiar nombre.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figura 23**: Refactorización de un nombre de controlador[(Haga clic para ver la imagen de tamaño completo)](iteration-1-create-the-application-cs/_static/image46.png)

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figura 24**: Uso del cuadro de diálogo Cambiar nombre ([Haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image48.png))

Si cambia el nombre de la clase de controlador, Visual Studio actualizará el nombre de la carpeta en la carpeta Vistas también. Visual Studio cambiará el nombre de la carpeta de inicio de las vistas a la carpeta .

Después de realizar este cambio, la aplicación ya no tendrá un controlador de inicio. Cuando ejecute la aplicación, obtendrá la página de error en la figura 25.

[![Cuadro de diálogo Nuevo proyecto](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figura 25**: No hay controlador predeterminado[(Haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image50.png))

Necesitamos actualizar la ruta predeterminada en el archivo Global.asax para usar el controlador de contacto en lugar del controlador de inicio. Abra el archivo Global.asax y modifique el controlador predeterminado utilizado por la ruta predeterminada (consulte el listado 10).

**Listado 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Después de realizar estos cambios, el Administrador de contactos se ejecutará correctamente. Ahora, usará la clase de controlador de contacto como controlador predeterminado.

## <a name="summary"></a>Resumen

En esta primera iteración, creamos una aplicación básica de Contact Manager de la forma más rápida posible. Aprovechamos Visual Studio para generar automáticamente el código inicial para nuestros controladores y vistas. También aprovechamos Entity Framework para generar automáticamente nuestras clases de modelo de base de datos.

Actualmente, podemos enumerar, crear, editar y eliminar registros de contacto con la aplicación Contact Manager. En otras palabras, podemos realizar todas las operaciones básicas de base de datos requeridas por una aplicación web controlada por base de datos.

Desafortunadamente, nuestra aplicación tiene algunos problemas. En primer lugar y dudo en admitir esto, la aplicación Contact Manager no es la aplicación más atractiva. Necesita un poco de trabajo de diseño. En la siguiente iteración, veremos cómo podemos cambiar la página maestra de vista predeterminada y la hoja de estilos en cascada para mejorar el aspecto de la aplicación.

En segundo lugar, no hemos implementado ninguna validación de formulario. Por ejemplo, no hay nada que le impida enviar el formulario Crear formulario de contacto sin especificar valores para ninguno de los campos de formulario. Además, puede introducir números de teléfono y direcciones de correo electrónico no válidos. Comenzamos a abordar el problema de la validación de formularios en #3 de iteración.

Por último, y lo más importante, la iteración actual de la aplicación Contact Manager no se puede modificar ni mantener fácilmente. Por ejemplo, la lógica de acceso a la base de datos se introduce directamente en las acciones del controlador. Esto significa que no podemos modificar nuestro código de acceso a datos sin modificar nuestros controladores. En iteraciones posteriores, exploramos patrones de diseño de software que podemos implementar para que Contact Manager sea más resistente a los cambios.

> [!div class="step-by-step"]
> [Siguiente](iteration-2-make-the-application-look-nice-cs.md)
