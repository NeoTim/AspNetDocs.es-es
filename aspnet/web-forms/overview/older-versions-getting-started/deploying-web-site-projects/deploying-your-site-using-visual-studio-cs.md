---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Implementar el sitio con Visual Studio (C#) | Microsoft Docs
author: rick-anderson
description: Visual Studio incluye herramientas para implementar un sitio Web. Más información acerca de estas herramientas en este tutorial.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: f669e86ceeb53469f99fa3ea4619b4666c4019a0
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65133462"
---
# <a name="deploying-your-site-using-visual-studio-c"></a>Implementar el sitio con Visual Studio (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) o [descargar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio incluye herramientas para implementar un sitio Web. Más información acerca de estas herramientas en este tutorial.

## <a name="introduction"></a>Introducción

El tutorial anterior examinado cómo implementar una sencilla aplicación web ASP.NET a un proveedor de hospedaje web. En concreto, el tutorial que se ha explicado cómo usar a un cliente FTP como FileZilla para transferir los archivos necesarios del entorno de desarrollo al entorno de producción. Visual Studio también ofrece herramientas integradas que facilitan la implementación de un proveedor de hospedaje web. Este tutorial examinan dos de estas herramientas: la herramienta Copiar sitio Web, donde puede mover archivos a y desde un servidor web remoto mediante FTP o extensiones de servidor de FrontPage; y la herramienta de publicación, que copia todo el sitio Web en una ubicación especificada.

> [!NOTE]
> Otras herramientas relacionadas con la implementación que ofrece Visual Studio son [proyectos de instalación Web](https://msdn.microsoft.com/library/wx3b589t.aspx) y [proyectos de implementación Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Proyectos de instalación Web paquete de contenido y la información de configuración en un único archivo MSI de un sitio Web. Esta opción es muy útil para los sitios Web que se implementan dentro de una intranet o para las compañías que venden una aplicación web previamente empaquetados que los clientes instalen en sus propios servidores web. Agregar proyectos de implementación de Web es que un Visual Studio complemento que facilita la especificación de las diferencias de configuración entre compilaciones para entornos de desarrollo y entornos de producción. Proyectos de instalación Web no se tratan en esta serie de tutoriales Proyectos de implementación Web se resumen en la [ *Common Configuration diferencias entre desarrollo y producción* ](common-configuration-differences-between-development-and-production-cs.md) tutorial.

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Implementar el sitio mediante la herramienta Copiar sitio Web

Herramienta Copiar sitio Web de Visual Studio es una funcionalidad similar a un cliente FTP independiente. En pocas palabras, la herramienta Copiar sitio Web le permite conectarse a un sitio web remoto a través de FTP o extensiones de servidor. La interfaz de usuario Copiar sitio Web similar a la interfaz de usuario de FileZilla, consta de dos paneles: el panel izquierdo enumera los archivos locales mientras el panel derecho muestra esos archivos en el servidor de destino.

> [!NOTE]
> La herramienta Copiar sitio Web solo está disponible para proyectos de sitio Web. Visual Studio ofrece esta herramienta cuando se trabaja con un proyecto de aplicación Web.

Echemos un vistazo al uso de la herramienta Copiar sitio Web para publicar la aplicación de revisión del libro en producción. Con esta herramienta con el proyecto BookReviewsWSP solo podemos examinar porque la herramienta Copiar sitio Web solo funciona con proyectos que usan el modelo de proyecto de sitio Web. Abrir ese proyecto.

Iniciar el proyecto de la herramienta Copiar sitio Web haciendo clic en el icono Copiar sitio Web en el Explorador de soluciones (este icono está en un círculo en la figura 1); como alternativa, puede seleccionar la opción de Copiar sitio Web en el menú del sitio Web. Cualquiera de los enfoques inicia la interfaz de usuario Copiar sitio Web que se muestra en la figura 1; se rellena solo en el panel izquierdo en la figura 1 porque todavía tenemos que conectarnos a un servidor remoto.

[![Interfaz de usuario de la herramienta Copiar sitio Web está dividido en dos paneles](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Figura 1**: Interfaz de usuario de la herramienta Copiar sitio Web está dividido en dos paneles ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image3.png))

Para implementar nuestro sitio, es necesario para conectarse en primer lugar el proveedor de hospedaje web. Haga clic en el botón conectar en la parte superior de la interfaz de usuario Copiar sitio Web. Esto muestra el cuadro de diálogo Abrir sitio Web que se muestra en la figura 2.

Puede conectarse al sitio Web de destino seleccionando una de las cuatro opciones de la izquierda:

- **Sistema de archivos** : seleccione esta opción para implementar su sitio en una carpeta o recurso compartido accesible desde su equipo.
- **IIS local** -Utilice esta opción para implementar el sitio al servidor web IIS instalado en el equipo.
- **Sitio FTP** -conectarse a un sitio web remoto mediante FTP.
- **Sitio remoto** -conectarse a un sitio Web remoto mediante extensiones de servidor.

La mayoría de los proveedores de host de web admiten FTP, pero menos ofrecen compatibilidad con las extensiones de servidor de FrontPage. Por ese motivo, he seleccionado la opción de sitio FTP y, a continuación, escribió la información de conexión como se muestra en la figura 2.

[![Especifique el sitio Web de destino](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Figura 2**: Especifique el sitio Web de destino ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image6.png))

Después de conectarse, la herramienta Copiar sitio Web carga los archivos en el sitio remoto en el panel derecho e indica el estado de cada archivo: Nuevas, eliminadas, modificadas o sin cambios. Puede copiar un archivo desde el sitio local en el sitio remoto, o viceversa una.

Vamos a agregar una nueva página al proyecto BookReviewsWSP y, a continuación, implementarlo para que podamos ver la herramienta Copiar sitio Web en acción. Crear una nueva página ASP.NET en Visual Studio en el directorio raíz denominado `Privacy.aspx`. Tiene la página que utilice la página principal `Site.master` y agregar la directiva de privacidad de su sitio en esta página. Figura 3 se muestra Visual Studio después de esta página se ha creado.

[![Agregar una nueva página denominada &lt;código&gt;Privacy.aspx&lt;/código&gt; a carpeta de raíz del sitio Web](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Figura 3**: Agregar una nueva página denominada `Privacy.aspx` a carpeta de raíz del sitio Web ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image9.png))

A continuación, vuelva a la interfaz de usuario Copiar sitio Web. Como se muestra en la figura 4, el panel izquierdo ahora incluye los nuevos archivos - `Policy.aspx` y `Policy.aspx.cs`. Además, estos archivos se marcan con un icono de flecha y un estado de nuevo, que indica que existen en el sitio local, pero no en el sitio remoto.

[![La herramienta Copiar sitio Web incluye el nuevo &lt;código&gt;Privacy.aspx&lt;/código&gt; página en el panel de la izquierda](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Figura 4**: La herramienta Copiar sitio Web incluye el nuevo `Privacy.aspx` página en el panel de la izquierda ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image12.png))

Para implementar los nuevos archivos, selecciónelos y, a continuación, haga clic en el icono de flecha para transferirlos al sitio remoto. Una vez completada la transferencia del `Policy.aspx` y `Policy.aspx.cs` archivos existen en los sitios locales y remotos con el estado Unchanged.

Junto con la lista de nuevos archivos, la herramienta Copiar sitio Web resalta los archivos que difieren entre los sitios locales y remotos. Para ver esto en acción, vuelva a la `Privacy.aspx` página y agregue algunas palabras más a la directiva de privacidad. Guarde la página y, a continuación, volver a la herramienta Copiar sitio Web. Como se muestra en la figura 5, el `Privacy.aspx` página en el panel izquierdo tiene un estado de Changed que indica que está sincronizada con el sitio remoto.

[![La herramienta Copiar sitio Web que indica la &lt;código&gt;Privacy.aspx&lt;/código&gt; página ha cambiado](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Figura 5**: La herramienta Copiar sitio Web que indica la `Privacy.aspx` página ha cambiado ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image15.png))

La herramienta Copiar sitio Web también indica si se ha eliminado un archivo desde la última operación de copia. Eliminar el `Privacy.aspx` desde el proyecto local y actualizar la herramienta Copiar sitio Web. El `Privacy.aspx` y `Privacy.aspx.cs` archivos permanezca en la lista en el panel izquierdo, pero tienen un estado eliminado, que indica que se han quitado desde la última operación de copia.

## <a name="publishing-a-web-application"></a>Publicar una aplicación Web

Otra manera de implementar la aplicación web desde dentro de Visual Studio es usar la opción de publicación, que es accesible a través del menú compilar. La opción publicar explícitamente compila la aplicación y, a continuación, copia todos los archivos necesarios hasta el sitio remoto especificado. Como veremos en breve, la opción de publicación es más obvio que la herramienta Copiar sitio Web. Mientras que la herramienta Copiar sitio Web le permite examinar los archivos en los sitios locales y remotos y le permite cargar o descargar archivos individuales según sea necesario, la opción Publicar implementa toda la aplicación web.

Además de copiar todos los archivos necesarios para el sitio remoto especificado, la opción publicar explícitamente también compila la aplicación. Dado que los proyectos de aplicación Web deba ser compilado explícitamente debe proceder como no nos debe sorprender que la opción Publicar está disponible para proyectos de aplicación Web. ¿Qué puede resultar un poco sorprendente es que la opción de publicación también está disponible para los proyectos de sitio Web. Como se indicó en el [ *determinar qué archivos se deben implementarse* ](determining-what-files-need-to-be-deployed-cs.md) tutorial, los proyectos de sitio Web se puede compilar explícitamente a través de un proceso se denomina *precompilación*. En este tutorial se centra en usar la opción Publicar con proyectos de aplicación Web. un futuro tutorial explorará la precompilación, momento en que usaremos para contemplar usar la opción Publicar con proyectos de sitio Web.

> [!NOTE]
> Aunque la opción Publicar está disponible en Visual Studio para proyectos de sitios Web y proyectos de aplicación Web, Visual Web Developer ofrece solo la opción publicar proyectos de aplicación Web.

Echemos un vistazo a implementar la aplicación de reseñas de libros con la opción de publicación. Comience abriendo BookReviewsWAP (el proyecto de aplicación Web) en Visual Studio. En el menú publicar elija el proyecto de compilación BookReviewsWAP. Se abrirá un cuadro de diálogo que solicita la ubicación de destino, entre otras opciones de configuración (consulte la figura 6). Mucho al igual que con la herramienta Copiar sitio Web puede especificar una ubicación a la que apunta a una carpeta local, un sitio Web local en IIS, un sitio Web remoto que admite extensiones de servidor o una dirección del servidor FTP. Puede elegir si desea reemplazar los archivos en el servidor web remoto con los archivos implementados o eliminar todo el contenido en el sitio remoto antes de la publicación. También puede especificar si se deben copiar:

- Solo los archivos en el proyecto necesario para ejecutar la aplicación, que omite el código fuente innecesarios y archivos relacionados con el proyecto.
- Todos los archivos de proyecto, que incluye los archivos de código fuente y archivos de proyecto de Visual Studio, como el archivo de solución.
- Todos los archivos en la carpeta de proyecto de origen, que copia todos los archivos en la carpeta de proyecto de origen, independientemente de si están incluidos en el proyecto.

También hay una opción para cargar el contenido de la `App_Data` carpeta.

[![Especifique el sitio Web de destino](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Figura 6**: Especifique el sitio Web de destino ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image18.png))

Para la aplicación de la reseña de libro en el sitio remoto contiene los archivos implementados al copiar el proyecto BookReviewsWSP mediante la herramienta Copiar sitio Web. Por lo tanto, vamos a tener la opción Publicar iniciar mediante la eliminación de todo el contenido existente. Además, vamos a simplemente copie los archivos necesarios en lugar de ocupar el entorno de producción con los archivos de código y proyecto de origen innecesarios. Después de especificar estas opciones, haga clic en el botón Publicar. A través de los segundos siguientes Visual Studio implementará los archivos necesarios en el sitio de destino, muestra su progreso en la ventana de salida.

Figura 7 muestra los archivos en el sitio FTP una vez completada la operación de publicación. Tenga en cuenta que se han cargado solamente las páginas de marcado y los archivos de compatibilidad necesario sever - y del lado cliente.

[![Solo los archivos necesarios se publicaron en el entorno de producción](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Figura 7**: Solo los necesarios archivos se publicaron en el entorno de producción ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image21.png))

La opción de publicación es una herramienta matizada menor que la herramienta Copiar sitio Web. Mientras que la herramienta Copiar sitio Web le permite inspeccionar los archivos en los sitios locales y remotos y ver cómo difieren, la opción publicar no proporciona ninguna interfaz de este tipo. Además, la herramienta Copiar sitio Web le permite realizar cambios y excepcionales, cargar o eliminar archivos individuales. La opción publicar no permite un control tan específico; en su lugar, publica el *todo* aplicación. Este comportamiento tiene sus ventajas y desventajas. En el lado positivo, se sabe cuándo usar la opción Publicar que no se olvide cargar un archivo importante. Imagine qué ocurre que si ha realizado un pequeño cambio en un sitio Web de gran - con la opción publicar no se puede actualizar dicha página o dos que se ha modificado, pero en su lugar, debe esperar mientras Visual Studio implementa todo el sitio.

No es raro que haya ciertos archivos cuyo contenido es diferente entre los entornos de desarrollo y producción. Un ejemplo de clave es el archivo de configuración de la aplicación, `Web.config`. Dado que la opción Publicar ciegamente copia los archivos de aplicación web sobrescribe los archivos de configuración personalizada del entorno de producción con la versión en el entorno de desarrollo. El tutorial posterior aún más en este tema explora y ofrece sugerencias para implementar una aplicación web cuando existen tales diferencias.

## <a name="summary"></a>Resumen

Implementar un sitio Web implica copiar los archivos necesarios del entorno de desarrollo al entorno de producción. El tutorial anterior mostró cómo transferir archivos mediante un cliente FTP como FileZilla. En este tutorial se examina dos herramientas de implementación de Visual Studio: la herramienta Copiar sitio Web y la opción de publicación. La herramienta Copiar sitio Web es similar a un cliente FTP porque tiene una interfaz de dos paneles enumerar los archivos en el equipo local y un equipo remoto especificado que facilita la tarea cargar o descargar archivos entre los dos equipos. La opción de publicación es una herramienta más obvio que explícitamente se compila el proyecto y, a continuación, implementa toda la aplicación en el destino especificado.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Copiar sitio Web con la herramienta Copiar sitio Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [¿Cómo lo hago?: Implementar un sitio Web mediante la herramienta Copiar sitio Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vídeo)
- [Cómo: Publicar proyectos de aplicación Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Cómo: Publicar sitios Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [El programa de instalación y la implementación de proyectos en Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-an-ftp-client-cs.md)
> [Siguiente](common-configuration-differences-between-development-and-production-cs.md)
