---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implementación Web de ASP.NET con Visual Studio: Implementar una actualización de código | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: a3d181f3ec8db74781c550720ba4331bdf6478b3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047112"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Implementación Web de ASP.NET con Visual Studio: Implementar una actualización de código
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Tras la implementación inicial, continúa el trabajo de mantenimiento y desarrollo de su sitio web y, en poco tiempo desea implementar una actualización. Este tutorial le guiará por el proceso de implementación de una actualización en el código de aplicación. La actualización que implementar en este tutorial no implica un cambio de base de datos; podrá ver cuál es la diferencia acerca de cómo implementar un cambio de base de datos en el siguiente tutorial.

Aviso: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).

## <a name="make-a-code-change"></a>Realizar un código de cambio

Como ejemplo sencillo de una actualización a la aplicación, agregará el **instructores** página una lista de cursos impartidos por el instructor seleccionado.

Si ejecuta el **instructores** página, observará que hay **seleccione** vínculos en la cuadrícula, pero no es algo distinto de make el color gris a su vez de fila en segundo plano.

![Página de instructores con selección](deploying-a-code-update/_static/image1.png)

Ahora agregará código que se ejecuta cuando el **seleccione** vínculo está seleccionado y muestra una lista de cursos impartidos por el instructor seleccionado.

1. En *Instructors.aspx*, agregue el marcado siguiente inmediatamente después de la **ErrorMessageLabel** `Label` control:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Ejecute la página y seleccione un instructor. Vea una lista de cursos impartidos por ese instructor.

    ![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image2.png)
3. Cierre el explorador.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Implementar la actualización del código en el entorno de prueba

Para poder usar los perfiles de publicación para implementar en la prueba, ensayo y producción, deberá cambiar las opciones de publicación de base de datos. Ya no necesita ejecutar los scripts de implementación de concesión y los datos para la base de datos de pertenencia.

1. Abra el **publicación Web** Asistente haciendo clic en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Haga clic en el **prueba** de perfil en el **perfil** lista desplegable.
3. Haga clic en el **configuración** ficha.
4. En **DefaultConnection** en el **bases de datos** sección, desactive la **Actualizar base de datos** casilla de verificación.
5. Haga clic en el **perfil** pestaña y, a continuación, haga clic en el **ensayo** de perfil en el **perfil** lista desplegable.
6. Cuando se le pregunte si desea guardar los cambios realizados en el **prueba** de perfil, haga clic en **Sí**.
7. Realizar el mismo cambio en el perfil de almacenamiento provisional.
8. Repita el proceso para realizar el mismo cambio en el perfil de producción.
9. Cerrar la **publicación Web** asistente.

Implementación en el entorno de prueba es ahora tan sencillo ejecutar con un solo clic, vuelva a publicar. Para agilizar este proceso, puede usar el **una publicación en Web clic** barra de herramientas.

1. En el **vista** menú, elija **las barras de herramientas** y, a continuación, seleccione **una publicación en Web clic**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity.
3. el **una publicación en Web clic** barra de herramientas, elija la **prueba** perfil de publicación y, a continuación, haga clic en **publicación Web** (el icono con flechas que señalan al left y right).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio implementa la aplicación actualizada, y el explorador se abre automáticamente en la página principal.
5. Ejecute la página de instructores y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

Lo haría normalmente también hacer las pruebas de regresión (es decir, el resto del sitio para asegurarse de que el nuevo cambio no interrumpa ninguna funcionalidad existente de prueba). Pero para este tutorial podrá omitir ese paso y continuar para implementar la actualización en ensayo y producción.

Al implementar de nuevo, Web Deploy determina automáticamente qué archivos han cambiado y copias de solo archivos cambiados en el servidor. De forma predeterminada, Web Deploy usa fechas último cambio en archivos para determinar cuáles se han cambiado. Algunos sistemas de control de código fuente cambian archivo fechas incluso cuando no cambie el contenido del archivo. En ese caso, es posible que desee configurar Web Deploy para usar sumas de comprobación para determinar qué archivos han cambiado. Para obtener más información, consulte [¿por qué todos mis archivos se vuelven a implementar aunque no cambiarlos?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) en las preguntas más frecuentes de implementación de ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Desactivar la aplicación sin conexión durante la implementación

El cambio que va a implementar ahora es un simple cambio en una sola página. Pero a veces de implementar cambios mayores, o implementar los cambios de código y de base de datos y el sitio es posible que se comporte incorrectamente si un usuario solicita una página antes de que finalice la implementación. Para evitar que los usuarios acceso al sitio mientras la implementación está en curso, puede usar un *aplicación\_offline.htm* archivo. Cuando coloca un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación. Por lo que colocar para evitar el acceso durante la implementación, *aplicación\_offline.htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quite *aplicación\_offline.htm* después correcta implementación.

Puede configurar Web Deploy para colocar automáticamente el valor predeterminado es *aplicación\_offline.htm* en el servidor de archivos cuando se inicia la implementación y quitarlo cuando haya terminado. Para hacer todo lo que debe hacer es agregar el siguiente elemento XML a su archivo de perfil (.pubxml) de la publicación:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

En este tutorial verá cómo crear y utilizar una personalizada *aplicación\_offline.htm* archivo.

Uso de *aplicación\_offline.htm* en el sitio de ensayo no es necesario, porque no tiene acceso al sitio de ensayo de los usuarios. Pero es una buena práctica para usar el almacenamiento provisional para probar todo lo que se va a implementar en producción.

### <a name="create-appofflinehtm"></a>Crear aplicación\_offline.htm

1. En **el Explorador de soluciones**, haga clic en la solución y haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento**.
2. Crear un **página HTML** denominado *aplicación\_offline.htm* (eliminar el último "l" en el *.html* extensión de Visual Studio crea de forma predeterminada).
3. Reemplace el marcado de plantilla con el siguiente marcado:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Guarde y cierre el archivo.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Copiar la aplicación\_offline.htm a la carpeta raíz del sitio web

Puede usar cualquier herramienta FTP para copiar archivos en el sitio web. [FileZilla](http://filezilla-project.org/) es una popular herramienta FTP y se muestra en las capturas de pantalla.

Para usar una herramienta FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña.

La dirección URL se muestra en la página del panel del sitio web en el Portal de administración de Azure y el nombre de usuario y contraseña de FTP pueden encontrarse en el *.publishsettings* archivo que descargó anteriormente. Los pasos siguientes muestran cómo obtener estos valores.

1. En el Portal de administración de Azure, haga clic en **sitios Web** pestaña y, a continuación, haga clic en el sitio web de ensayo.
2. En el **panel** página, desplácese hacia abajo hasta encontrar el nombre de host FTP en el **vista rápida** sección.

    ![Nombre de host FTP](deploying-a-code-update/_static/image5.png)
3. Abra el almacenamiento provisional *.publishsettings* archivo en el Bloc de notas u otro editor de texto.
4. Buscar el `publishProfile` (elemento) para el perfil de FTP.
5. Copia el `userName` y `userPWD` valores.

    ![Credenciales de FTP](deploying-a-code-update/_static/image6.png)
6. Abra la herramienta FTP e iniciar sesión en la dirección URL de FTP.
7. Copia *aplicación\_offline.htm* desde la carpeta de soluciones para la */site/wwwroot* carpeta en el sitio de ensayo.

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. Vaya a la dirección URL de su sitio de ensayo. Verá que el *aplicación\_offline.htm* página aparece ahora en lugar de la página principal.

    ![App_offline.htm en la ventana del explorador](deploying-a-code-update/_static/image8.png)

Ahora está listo para implementar en ensayo.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Implementar la actualización del código en ensayo y producción

1. En el **una publicación en Web clic** barra de herramientas, elija la **ensayo** perfil de publicación y, a continuación, haga clic en **publicación Web**.

    Visual Studio implementa la aplicación actualizada y abre el explorador a la página principal del sitio. El *aplicación\_offline.htm* se muestra el archivo. Antes de poder probar para comprobar la implementación correcta, debe quitar la *aplicación\_offline.htm* archivo.
2. Volver a la herramienta FTP y eliminar **aplicación\_offline.htm** desde el sitio de ensayo.
3. En el explorador, abra la página de instructores en el sitio de ensayo y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.
4. Siga el mismo procedimiento para la producción como hizo el almacenamiento provisional.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisar los cambios y archivos específicos de implementación

Visual Studio 2012 también le ofrece la posibilidad de implementar los archivos individuales. Para un archivo seleccionado puede ver las diferencias entre la versión local y la versión implementada, implemente el archivo en el entorno de destino o copie el archivo desde el entorno de destino en el proyecto local. En esta sección del tutorial verá cómo usar estas características.

### <a name="make-a-change-to-deploy"></a>Realiza un cambio en la implementación

1. Abra *Content/Site.css*y busque el bloque para el `body` etiqueta.
2. Cambie el valor de `background-color` desde `#fff` a `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Ver el cambio en la ventana de vista previa de publicación

Cuando se usa el **publicación Web** Asistente para publicar el proyecto, puede ver los cambios que se van a publicarse haciendo doble clic en el archivo en el **Preview** ventana.

1. Haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Desde el **perfil** lista desplegable, seleccione el **prueba** perfil de publicación.
3. Haga clic en **Preview**y, a continuación, haga clic en **iniciar vista previa**.
4. En el **Preview** panel, haga doble clic en **Site.css**.

    ![Haga doble clic en Site.css](deploying-a-code-update/_static/image9.png)

    El **vista previa de cambios** cuadro de diálogo muestra una vista previa de los cambios que se va a implementar.

    ![Vista previa de cambios para Site.css](deploying-a-code-update/_static/image10.png)

    Si hace doble clic en el *Web.config* archivo, el **vista previa de cambios** cuadro de diálogo se muestra el efecto de la compilación de las transformaciones de configuración y las transformaciones de perfil de publicación. En este momento no lo haga todo lo que provocaría la *Web.config* archivo en el servidor para cambiar, por lo que espera no ver ningún cambio. Sin embargo, el **vista previa de cambios** ventana incorrectamente muestra dos cambios. Parece que se quitarán los dos elementos XML. Estos elementos se agregan mediante el proceso de publicación cuando se selecciona **ejecutar migraciones de Code First al iniciar la aplicación** para una clase de contexto de Code First. La comparación se realiza antes de que el proceso de publicación agrega esos elementos, para que parezca que se quitan aunque no se quitará. Este error se corregirá en una versión futura.
5. Haga clic en **Cerrar**.
6. Haga clic en **Publicar**.
7. Cuando el explorador se abre en la página principal del sitio de prueba, presione CTRL + F5 para hacer que una actualización de disco dura con el fin de ver el efecto del cambio CSS.

    ![Efecto del cambio CSS](deploying-a-code-update/_static/image11.png)
8. Cierre el explorador.

### <a name="publish-specific-files-from-solution-explorer"></a>Publicar archivos concretos desde el Explorador de soluciones

Supongamos que no quiere revertir al color original y, como el fondo azul. En esta sección, podrá restaurar la configuración original mediante la publicación de un archivo específico directamente desde **el Explorador de soluciones**.

1. Abra *Content/Site.css* y restaurar el `background-color` si se establece en `#fff`.
2. En **el Explorador de soluciones**, haga clic en el *Content/Site.css* archivo.

    El menú contextual muestra que tres opciones de publicación.

    ![Publicar opciones desde el Explorador de soluciones](deploying-a-code-update/_static/image12.png)
3. Haga clic en **cambios de vista previa para Site.css**.

    Se abre una ventana para mostrar las diferencias entre el archivo local y la versión del mismo en el entorno de destino.

    ![Diff/Site.css-contenido](deploying-a-code-update/_static/image13.png)
4. En **el Explorador de soluciones**, haga clic en **Site.css** nuevo y haga clic en **publicar Site.css**.

    El **actividad de publicación Web** ventana muestra que se ha publicado el archivo.

    ![Ventana de actividad de publicación Web](deploying-a-code-update/_static/image14.png)
5. Abra un explorador en la `http://localhost/contosouniversity` dirección URL y, a continuación, presione CTRL + F5 para hacer que un disco duro actualizar para ver el efecto de CSS cambiar.

    ![Página principal con CSS normal](deploying-a-code-update/_static/image15.png)
6. Cierre el explorador.

## <a name="summary"></a>Resumen

Ahora ha visto varias maneras de implementar una actualización de la aplicación que no implica un cambio de base de datos y ha visto cómo obtener una vista previa de los cambios para comprobar que se actualizarán a lo que es lo que espera. La página de instructores ahora tiene un **cursos impartidos** sección.

![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image16.png)

El siguiente tutorial muestra cómo implementar un cambio de base de datos: se agregará un campo de fecha de nacimiento en la base de datos y a la página de instructores.

> [!div class="step-by-step"]
> [Anterior](deploying-to-production.md)
> [Siguiente](deploying-a-database-update.md)
