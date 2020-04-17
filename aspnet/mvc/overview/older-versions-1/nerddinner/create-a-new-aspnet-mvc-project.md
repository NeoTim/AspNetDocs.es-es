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
# <a name="create-a-new-aspnet-mvc-project"></a>Crear un proyecto de ASP.NET MVC

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 1 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 1 muestra cómo poner la estructura básica de la aplicación NerdDinner en su lugar.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner Paso 1:&gt;Archivo- Nuevo Proyecto

Comenzaremos nuestra aplicación NerdDinner seleccionando el elemento de menú **Archivo-&gt;Nuevo proyecto** dentro de Visual Studio 2008 o visual web Developer 2008 Express gratuito.

Esto abrirá el cuadro de diálogo "Nuevo proyecto". Para crear una nueva aplicación ASP.NET MVC, seleccionaremos el nodo "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elegiremos la plantilla de proyecto "ASP.NET MVC Web Application" a la derecha:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Asegúrese de que ha descargado e instalado ASP.NET MVC- de lo contrario no aparecerá en el cuadro de diálogo Nuevo proyecto. Puede usar V2 del Instalador de [plataforma web](https://www.microsoft.com/web/downloads/platform.aspx) de Microsoft si aún no lo ha&gt;instalado (ASP.NET MVC está disponible en la sección "Plataformas web- marcos y tiempos de ejecución").*

Vamos a nombrar el nuevo proyecto que vamos a crear "NerdDinner" y luego haga clic en el botón "ok" para crearlo.

Al hacer clic en "aceptar" Visual Studio mostrará un cuadro de diálogo adicional que nos pedirá que, opcionalmente, también creemos un proyecto de prueba unitaria para la nueva aplicación. Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueban la funcionalidad y el comportamiento de nuestra aplicación (algo que cubriremos cómo hacer más adelante en este tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

El menú desplegable "Marco de pruebas" del cuadro de diálogo anterior se rellena con todas las plantillas de proyecto de prueba unitaria de ASP.NET MVC disponibles instaladas en el equipo. Las versiones se pueden descargar para NUnit, MBUnit y XUnit. También se admite el marco de pruebas unitarias de Visual Studio integrado.

*Nota: El marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si está utilizando VS 2008 Standard Edition o Visual Web Developer 2008 Express, tendrá que descargar e instalar las extensiones NUnit, MBUnit o XUnit para ASP.NET MVC para que se muestre este cuadro de diálogo. El cuadro de diálogo no se mostrará si no hay ningún marco de prueba instalado.*

Usaremos el nombre predeterminado "NerdDinner.Tests" para el proyecto de prueba que creamos y usaremos la opción de marco de trabajo "Prueba unitaria de Visual Studio". Al hacer clic en el botón "ok" Visual Studio creará una solución para nosotros con dos proyectos en él - uno para nuestra aplicación web y otro para nuestras pruebas unitarias:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examen de la estructura de directorios NerdDinner

Cuando se crea una nueva aplicación ASP.NET MVC con Visual Studio, agrega automáticamente una serie de archivos y directorios al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET proyectos MVC de forma predeterminada tienen seis directorios de nivel superior:

| **Directorio** | **Propósito** |
| --- | --- |
| **/Controladores** | Dónde coloca clases de controlador que controlan las solicitudes de dirección URL |
| **/Modelos** | Donde se colocan clases que representan y manipulan datos |
| **/Vistas** | Dónde se colocan los archivos de plantilla de interfaz de usuario que son responsables de representar la salida |
| **/Scripts** | Dónde se colocan los archivos y scripts de la biblioteca JavaScript (.js) |
| **/Contenido** | Dónde se colocan archivos CSS e imagen, y otro contenido no dinámico/no JavaScript |
| **/Datos\_de la aplicación** | Donde almacena los archivos de datos que desea leer/escribir. |

ASP.NET MVC no requiere esta estructura. De hecho, los desarrolladores que trabajan en aplicaciones grandes normalmente dividirán la aplicación en varios proyectos para hacerla más manejable (por ejemplo: las clases de modelo de datos a menudo van en un proyecto de biblioteca de clases independiente de la aplicación web). La estructura de proyecto predeterminada, sin embargo, proporciona una convención de directorio predeterminada agradable que podemos usar para mantener nuestras preocupaciones de la aplicación limpias.

Cuando expandimos el directorio /Controllers, encontraremos que Visual Studio agregó dos clases de controlador ( HomeController y AccountController) de forma predeterminada al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Cuando expandimos el directorio /Views, encontraremos tres subdirectorios – /Home, /Account y /Shared – así como varios archivos de plantilla dentro de ellos también se agregaron al proyecto de forma predeterminada:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Cuando expandimos los directorios /Content y /Scripts, encontraremos un archivo Site.css que se utiliza para aplicar estilo a todo HTML en el sitio, así como bibliotecas JavaScript que pueden habilitar ASP.NET compatibilidad con AJAX y jQuery dentro de la aplicación:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Cuando expandamos el proyecto NerdDinner.Tests, encontraremos dos clases que contienen pruebas unitarias para nuestras clases de controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Estos archivos predeterminados agregados por Visual Studio nos proporcionan una estructura básica para una aplicación de trabajo- completa con página de inicio, acerca de la página, inicio de sesión de cuenta / inicio de sesión / páginas de registro y una página de error no controlada (todo cableado y trabajando de forma inmediata).

### <a name="running-the-nerddinner-application"></a>Ejecución de la aplicación NerdDinner

Podemos ejecutar el proyecto eligiendo los elementos de menú **Depurar-&gt;Iniciar depuración** o **Depurar-&gt;Iniciar sin depurar:**

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Esto iniciará el servidor Web ASP.NET integrado que viene con Visual Studio y ejecutará nuestra aplicación:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

A continuación se muestra la página de inicio de nuestro nuevo proyecto (URL: "/") cuando se ejecuta:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Al hacer clic en la pestaña "Acerca de" se muestra una página sobre (URL: "/Inicio/Acerca de"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Al hacer clic en el enlace "Iniciar sesión" en la parte superior derecha, nos lleva a una página de inicio de sesión (URL: "/Cuenta/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si no tenemos una cuenta de inicio de sesión, podemos hacer clic en el enlace de registro (URL: "/Cuenta/Registro") para crear una:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

El código para implementar la funcionalidad de inicio anterior, y cierre de sesión/registro, se agregó de forma predeterminada cuando creamos nuestro nuevo proyecto. Lo usaremos como punto de partida de nuestra aplicación.

### <a name="testing-the-nerddinner-application"></a>Prueba de la aplicación NerdDinner

Si estamos usando la edición Professional o una versión posterior de Visual Studio 2008, podemos usar la compatibilidad integrada con el IDE de pruebas unitarias en Visual Studio para probar el proyecto:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

La elección de una de las opciones anteriores abrirá el panel "Resultados de la prueba" dentro del IDE y nos proporcionará el estado de aprobación/fallo en las 27 pruebas unitarias incluidas en nuestro nuevo proyecto que cubren la funcionalidad integrada:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Más adelante en este tutorial hablaremos más sobre las pruebas automatizadas y agregaremos pruebas unitarias adicionales que cubran la funcionalidad de la aplicación que implementamos.

### <a name="next-step"></a>siguiente paso

Ahora tenemos una estructura de aplicación básica en su lugar. Ahora vamos [a crear una base](create-a-database.md)de datos para almacenar los datos de nuestra aplicación .

> [!div class="step-by-step"]
> [Anterior](introducing-the-nerddinner-tutorial.md)
> [Siguiente](create-a-database.md)
