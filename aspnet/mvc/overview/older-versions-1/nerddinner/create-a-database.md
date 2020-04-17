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
# <a name="create-a-database"></a>Crear una base de datos

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 2 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 2 muestra los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner Paso 2: Creación de la base de datos

Usaremos una base de datos para almacenar todos los datos de Dinner y RSVP para nuestra aplicación NerdDinner.

Los pasos siguientes muestran la creación de la base de datos mediante la edición gratuita de SQL Server Express (que puede instalar fácilmente mediante V2 del Instalador de plataforma web de [Microsoft).](https://www.microsoft.com/web/downloads/platform.aspx) Todo el código que escribiremos funciona con SQL Server Express y con el servidor SQL Server completo.

### <a name="creating-a-new-sql-server-express-database"></a>Creación de una nueva base de datos de SQL Server Express

Comenzaremos haciendo clic con el botón derecho en nuestro proyecto web y, a continuación, seleccionaremos el comando de menú **Agregar&gt;nuevo elemento:**

![](create-a-database/_static/image1.png)

Esto abrirá el cuadro de diálogo "Agregar nuevo elemento" de Visual Studio. Filtraremos por la categoría "Datos" y seleccionaremos la plantilla de elemento "Base de datos de SQL Server":

![](create-a-database/_static/image2.png)

Nombraremos la base de datos de SQL Server Express que queremos crear "NerdDinner.mdf" y pulsaremos ok. Visual Studio nos preguntará si queremos agregar este\_archivo a nuestro directorio de datos de aplicaciones (que es un directorio que ya está configurado con ACL de seguridad de lectura y escritura):

![](create-a-database/_static/image3.png)

Haremos clic en "Sí" y nuestra nueva base de datos se creará y agregará a nuestro Explorador de soluciones:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Creación de tablas dentro de nuestra base de datos

Ahora tenemos una nueva base de datos vacía. Vamos a agregarle algunas tablas.

Para ello navegaremos a la ventana de pestaña "Explorador de servidores" dentro de Visual Studio, que nos permite administrar bases de datos y servidores. Las bases de datos de\_SQL Server Express almacenadas en la carpeta de datos de la aplicación de nuestra aplicación se mostrarán automáticamente en el Explorador de servidores. Opcionalmente, podemos usar el icono "Conectar a la base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar bases de datos adicionales de SQL Server (tanto locales como remotas) a la lista también:

![](create-a-database/_static/image5.png)

Agregaremos dos tablas a nuestra base de datos NerdDinner: una para almacenar nuestras cenas y la otra para realizar un seguimiento de las aceptaciones de RSVP en ellas. Podemos crear nuevas tablas haciendo clic con el botón derecho en la carpeta "Tablas" dentro de nuestra base de datos y eligiendo el comando de menú "Agregar nueva tabla":

![](create-a-database/_static/image6.png)

Esto abrirá un diseñador de tablas que nos permite configurar el esquema de nuestra tabla. Para nuestra tabla "Cenas" agregaremos 10 columnas de datos:

![](create-a-database/_static/image7.png)

Queremos que la columna "DinnerID" sea una clave principal única para la tabla. Podemos configurar esto haciendo clic con el botón derecho en la columna "DinnerID" y eligiendo el elemento de menú "Establecer clave principal":

![](create-a-database/_static/image8.png)

Además de convertir DinnerID en una clave principal, también queremos configurarla como una columna de "identidad" cuyo valor se incrementa automáticamente a medida que se agregan nuevas filas de datos a la tabla (lo que significa que la primera fila Dinner insertada tendrá un DinnerID de 1, la segunda fila insertada tendrá un DinnerID de 2, etc.).

Podemos hacer esto seleccionando la columna "DinnerID" y luego usar el editor "Propiedades de columna" para establecer la propiedad "(Is Identity)" en la columna en "Sí". Usaremos los valores predeterminados de identidad estándar (comienza en 1 e incrementa 1 en cada nueva fila de la cena):

![](create-a-database/_static/image9.png)

A continuación, guardaremos nuestra tabla escribiendo Ctrl-S o mediante el comando de menú **&gt;Archivo- Guardar.** Esto nos pedirá que asignemos un nombre a la tabla. Lo llamaremos "Cenas":

![](create-a-database/_static/image10.png)

Nuestra nueva tabla Cenas aparecerá dentro de nuestra base de datos en el explorador de servidores.

A continuación, repetiremos los pasos anteriores y crearemos una tabla "RSVP". Esta tabla con tienen 3 columnas. Configuraremos la columna RsvpID como la clave principal y también la convertiremos en una columna de identidad:

![](create-a-database/_static/image11.png)

Lo guardaremos y le daremos el nombre "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configuración de una relación de clave externa entre tablas

Ahora tenemos dos tablas dentro de nuestra base de datos. Nuestro último paso de diseño de esquema será configurar una relación "uno a varios" entre estas dos tablas, de modo que podamos asociar cada fila Dinner con cero o más filas RSVP que se apliquen a ella. Para ello, configuraremos la columna "DinnerID" de la tabla RSVP para que tenga una relación de clave externa con la columna "DinnerID" de la tabla "Cenas".

Para ello, abriremos la tabla RSVP dentro del diseñador de tablas haciendo doble clic en ella en el explorador de servidores. A continuación, seleccionaremos la columna "DinnerID" dentro de ella, haga clic con el botón derecho y elegiremos "Relaciones..." comando de menú contextual:

![](create-a-database/_static/image12.png)

Esto abrirá un cuadro de diálogo que podemos usar para configurar relaciones entre tablas:

![](create-a-database/_static/image13.png)

Haremos clic en el botón "Agregar" para agregar una nueva relación al cuadro de diálogo. Una vez que se ha agregado una relación, expandiremos el nodo de vista de árbol "Tablas y especificación de columna" dentro de la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, haremos clic en "..." botón a la derecha de la misma:

![](create-a-database/_static/image14.png)

Al hacer clic en el botón "..." botón abrirá otro cuadro de diálogo que nos permite especificar qué tablas y columnas están involucradas en la relación, así como nos permite nombrar la relación.

Cambiaremos la tabla de claves principal para que sea "Cenas" y seleccionaremos la columna "DinnerID" dentro de la tabla Cenas como clave principal. Nuestra tabla RSVP será la tabla de claves externas, y el RSVP. La columna DinnerID se asociará como clave externa:

![](create-a-database/_static/image15.png)

Ahora cada fila de la tabla RSVP se asociará a una fila en la tabla Dinner. SQL ServerSQL Server mantendrá la integridad referencial para nosotros y nos impedirá agregar una nueva fila RSVP si no apunta a una fila Dinner válida. También nos impedirá eliminar una fila De cena si todavía hay filas RSVP que hacen referencia a ella.

### <a name="adding-data-to-our-tables"></a>Adición de datos a nuestras tablas

Terminemos agregando algunos datos de muestra a nuestra tabla Cenas. Podemos agregar datos a una tabla haciendo clic con el botón derecho en ella en el Explorador de servidores y eligiendo el comando "Mostrar datos de tabla":

![](create-a-database/_static/image16.png)

Agregaremos algunas filas de datos de cena que podemos usar más adelante a medida que comenzamos a implementar la aplicación:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>siguiente paso

Hemos terminado de crear nuestra base de datos. Ahora vamos a crear clases de modelo que podemos usar para consultarlo y actualizarlo.

> [!div class="step-by-step"]
> [Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)
