---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Mejoras en Visual Studio 2005 Microsoft Docs
author: rick-anderson
description: Visual Studio 2005 proporciona a los desarrolladores de aplicaciones web una larga lista de mejoras y mejoras en los proyectos web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: e98771614bf4e0095f8ff596e7cdb26c8c9de1ad
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542994"
---
# <a name="improvements-in-visual-studio-2005"></a>Mejoras en Visual Studio 2005

por [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 proporciona a los desarrolladores de aplicaciones web una larga lista de mejoras y mejoras en los proyectos web.

Visual Studio 2005 proporciona a los desarrolladores de aplicaciones web una larga lista de mejoras y mejoras en los proyectos web. Tan eficaz como Visual Studio .NET 2002 y 2003 son, hubo muchas quejas en la forma en que se manejaron los proyectos web. Visual Studio 2005 agrega un número significativo de nuevas características para abordar estas quejas. Para aquellos que prefieren la forma en que Visual Studio .NET 2003 controló la compilación de aplicaciones web, vea Proyectos de [aplicación web](https://go.microsoft.com/fwlink/?LinkId=57870).

En este módulo, cubre bien las mejoras en la creación, gestión y desarrollo de proyectos web. En un módulo posterior, cubra bien las mejoras en la creación de proyectos web y su implementación.

## <a name="frontpage-server-extensions"></a>Extensiones de servidor de FrontPage

Visual Studio .NET 2002 y 2003 requería extensiones de servidor de FrontPage en el cuadro para crear o compilar proyectos web. Los desarrolladores tenían la opción de elegir entre dos modos de acceso diferentes (Extensiones de servidor de FrontPage o modo de acceso a archivos), ambos usaban Extensiones de servidor de FrontPage para realizar tareas como establecer la raíz de la aplicación en IIS, etc.

Visual Studio 2005 quita la dependencia de las extensiones de servidor de FrontPage para proyectos locales. Visual Studio 2005 ahora tiene acceso a la metabase de IIS directamente en lugar de usar las extensiones de servidor de FrontPage. Visual Studio 2005 también agrega compatibilidad con FTP, lo que permite el acceso remoto a proyectos sin necesidad de extensiones de servidor de FrontPage.

Para aquellos desarrolladores que desean usar extensiones de servidor de FrontPage en sus proyectos, la opción sigue estando disponible. Sin embargo, basado en la fuerte retroalimentación de la comunidad de desarrolladores ASP.NET, no es un requisito.

> [!NOTE]
> Las extensiones de servidor de FrontPage siguen siendo necesarias para la creación, apertura, etc. de proyectos remotos.

## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Visual Studio 2005 se suministra con un nuevo servidor Web denominado ASP.NET servidor de desarrollo. (Este servidor Web se conocía anteriormente como Cassini.)

Hay varias ventajas del ASP.NET servidor de desarrollo.

- Ahora es posible que los no administradores desarrollen y depuren en un servidor Web.
- El ASP.NET Development Server asigna dinámicamente directorios virtuales a cualquier ubicación del sistema de archivos, lo que permite ubicaciones de proyectos flexibles.
- Los usuarios de Windows XP Professional que ya estén utilizando IIS ahora podrán crear nuevas aplicaciones web que no afectarán a la estructura de archivos o carpetas de su sitio Web predeterminado en IIS.

No se requiere ninguna configuración especial para aprovechar el ASP.NET servidor de desarrollo. Cuando se depura o examina un proyecto web hospedado en el sistema de archivos, Visual Studio 2005 iniciará automáticamente una instancia del servidor de desarrollo de ASP.NET en un puerto aleatorio para atender la solicitud.

Más información se tratará en el ASP.NET servidor de desarrollo más adelante en este módulo.

## <a name="improved-file-management"></a>Mejora de la gestión de archivos

En Visual Studio 2002 y 2003, un archivo de proyecto (.vbproj para VB.NET y .csproj para C-) almacenaba información sobre todos los archivos de la aplicación web. La pantalla del Explorador de soluciones se basa en la información del archivo en el archivo de proyecto. Debido a esto, el Explorador de soluciones a menudo mostraría información inexacta en los casos en que se usaban editores externos. Visual Studio 2002 y 2003 a menudo sobrescribirían los cambios de archivo o no mostrarían la versión más reciente de los archivos.

Visual Studio 2005 elimina el archivo de proyecto. En su lugar, lee la información del archivo y la carpeta directamente desde el disco, lo que resulta en una visualización precisa de los archivos en el proyecto. Dado que la carpeta Referencias de Visual Studio 2002 y 2003 no representa una carpeta real en la aplicación web, Visual Studio 2005 también quita la carpeta Referencias del Explorador de soluciones. Para tener acceso a las referencias del proyecto en Visual Studio 2005, debe usar las páginas de propiedades para el proyecto.

## <a name="creating-web-projects"></a>Creación de proyectos web

Los desarrolladores web tienen muchas opciones nuevas disponibles para la creación de proyectos en Visual Studio 2005. Los sitios web ahora se pueden crear en cualquier parte del sistema de archivos y, a continuación, se pueden depurar o examinar con el nuevo ASP.NET servidor de desarrollo. Los desarrolladores también pueden crear nuevos sitios Web mediante FTP.

Haga clic aquí para ver un tutorial de vídeo sobre la creación de proyectos web en Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Abrir vídeo a pantalla completa](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Proyectos del sistema de archivos

Como ha visto en el tutorial de vídeo, puede elegir crear sitios Web en el sistema de archivos en el equipo local o en una ubicación remota a través de un recurso compartido de archivos. Los sitios web que se crean en el sistema de archivos se examinan y depuran mediante el ASP.NET servidor de desarrollo.

> [!NOTE]
> El ASP.NET servidor de desarrollo puede causar cierta confusión para los clientes. Si se crea un proyecto Web en el sistema de archivos en la estructura de directorios de IIS (es decir, c:/inetpub/wwwroot), el sitio Web seguirá examinando a través del servidor de desarrollo de ASP.NET cuando se inicie desde Visual Studio 2005. Por lo tanto, cualquier configuración de IIS (es decir, métodos de autenticación) no es aplicable.

El proyecto web predeterminado también quita gran parte de la sobrecarga por solo incluye un Default.aspx página, default.cs archivo y una App/_Data carpeta. Los web.config y las carpetas especiales (es decir, app/_code) se agregan según se necesiten. El proyecto web solo incluye los archivos y carpetas que necesita.

### <a name="http-projects"></a>Proyectos HTTP

Los proyectos HTTP pueden ser proyectos que se crean en un sitio Web IIS local o en un sitio Web remoto. La ubicación predeterminada `http://localhost`del proyecto es . Si hace clic en el botón Examinar, hay dos opciones HTTP: IIS local y sitio remoto. La principal diferencia en estas dos opciones es el método en el que se muestra la información del sitio web en el cuadro de diálogo Elegir ubicación y en cómo se copian los archivos en el servidor Web.

La opción IIS local lee la información del sitio de la metabase en el equipo local y los archivos se copian mediante el sistema de archivos. La opción Sitio remoto usa las extensiones de servidor de FrontPage y la información del sitio y los archivos se copian mediante llamadas RPC de extensiones de servidor HTTP y FrontPage.

> [!NOTE]
> El archivo vs-/_tmp.htm y get/_aspx/_ver.aspx ya no se usan para determinar la información de la versión.

La opción HTTP predeterminada es IIS local. Esta opción lee la metabase de IIS para determinar qué sitios están disponibles y la ubicación en la que se va a crear contenido. Puede seleccionar una carpeta o directorio virtual diferente seleccionándolo en la vista de árbol. También puede crear un nuevo directorio virtual, marcar carpetas como aplicaciones, así como eliminar directorios virtuales existentes desde este cuadro de diálogo.

![El cuadro de diálogo Elegir ubicación](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: el cuadro de diálogo Elegir ubicación

A diferencia de versiones anteriores de Visual Studio, si marca la casilla **Usar capa** de sockets seguros y el certificado SSL no coincide con la dirección URL que está explorando, se le presentará un cuadro de diálogo Alerta de seguridad que le preguntará si desea continuar. Con Visual Studio .NET 2003, si el certificado no era coincidente, se produciría un error al crear el proyecto.

![Alerta de seguridad con respecto al certificado SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: Alerta de seguridad con respecto al certificado SSL

### <a name="note-on-host-headers"></a>Nota sobre los encabezados de host

Si va a crear una aplicación web en un sitio enlazado a una dirección IP específica, deberá asegurarse de que está configurado un encabezado de host. De lo contrario, Visual `http://localhost`Studio creará el sitio en , pero la dirección IP no se resolverá correctamente cuando el sitio se examine o depure desde dentro del IDE.

Si selecciona la opción Sitio remoto, el cuadro de diálogo cambia para permitirle escribir la dirección URL de destino del nuevo sitio Web. Esta dirección URL debe estar en un servidor que tenga habilitadas las extensiones de servidor de FrontPage. Si desea trabajar con el servidor Web local mediante las extensiones de servidor de FrontPage, puede usar la opción Sitio remoto y especificar una dirección URL local.

![Creación de un sitio web en un servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: Creación de un sitio Web en un servidor remoto

Al crear una aplicación en un sitio remoto a través de SSL, si el certificado SSL no coincide, el cuadro de diálogo de confirmación es ligeramente diferente del cuadro de diálogo que se muestra al utilizar la opción IIS local.

![La alerta de seguridad del sitio remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: La alerta de seguridad del sitio remoto

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 presenta la opción de crear sitios Web a través de FTP. Cuando se utiliza esta opción, el IDE crea los archivos localmente en la carpeta temporal de los usuarios y, a continuación, usa FTP para mover los archivos a la ubicación FTP.

> [!NOTE]
> La ubicación de la carpeta temporal&lt;es&gt;c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/ Server /_&lt;nombre de la aplicación&gt;

Al utilizar la opción FTP, se le presentará un cuadro de diálogo Elegir ubicación. Introduzca la información de conexión FTP necesaria en este cuadro de diálogo como se muestra a continuación.

![El cuadro de diálogo Elegir ubicación para FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: El cuadro de diálogo Elegir ubicación para FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorio: Configurar el sitio FTP y crear un proyecto

Los pasos siguientes configuran el sitio FTP para que un usuario tenga una ubicación a la que solo pueda cargar a través de FTP.

### <a name="install-the-ftp-service"></a>Instale el servicio FTP

1. Abra Agregar quitar programas, seleccione Agregar o quitar componentes de Windows
2. Seleccione Internet Information Services (Servidor de aplicaciones en Windows 2003) y haga clic en **Detalles**.
3. Marque el servicio del protocolo de transferencia de archivos **(FTP)** y haga clic la **AUTORIZA.**
4. Haga clic en **Siguiente** para instalar el servicio FTP.

### <a name="create-a-new-folder-for-content"></a>Crear una nueva carpeta para el contenido

1. En el Explorador de Windows, cree una nueva carpeta denominada **User1** dentro de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configure carpetas y permisos en carpetas.

1. Abra el complemento Servicios de Internet Information Server desde Herramientas administrativas. Ahora tendrá una carpeta Sitios FTP bajo el nodo de nombre de equipo.
2. Expanda **Sitios FTP**.
3. Haga clic con el botón derecho en el **sitio FTP predeterminado**, seleccione **Nuevo**, , luego **Directorio virtual**y, a continuación, haga clic en **Siguiente**.
4. Escriba **User1** para el nombre del directorio virtual y haga clic en **Siguiente**.
5. Ingrese **c:/inetpub/wwwroot/User1** para la ruta de acceso y haga clic **después**.
6. Haga clic en **Siguiente** y, a continuación, en **Finalizar** para completar el asistente.
7. Haga clic con el botón derecho en el directorio virtual **User1** en Sitio FTP predeterminado y seleccione **Propiedades**.
8. Marque la casilla **de verificación Escribir** y haga clic en **Aceptar** para cerrar el cuadro de diálogo.
9. Haga clic con el botón derecho en **Sitio FTP predeterminado** y seleccione **Propiedades**.
10. En la pestaña Cuentas de **seguridad,** desactive **Permitir conexiones anónimas**.
11. Haga clic en **Sí** en el cuadro de diálogo preguntando si desea continuar.
12. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
13. Expanda el **sitio Web predeterminado** en el nodo Sitios **web.**
14. Haga clic con el botón derecho en el directorio **User1** y seleccione **Propiedades**
15. En la sección **Configuración** de la aplicación, haga clic en **Crear** para marcar la carpeta como una aplicación.
16. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
17. Cierre el complemento Servicios de Internet Information Server.

### <a name="create-web-project"></a>Crear proyecto web

1. Abra Visual Studio 2005.
2. En el menú **Archivo** , seleccione **Nuevo sitio Web**.
3. En el menú desplegable **Ubicación,** seleccione **FTP**.
4. Haga clic en **Examinar**.
5. Escriba **localhost** en el cuadro de texto **Servidor.**
6. Escriba **User1** en el cuadro de texto Directorio.
7. Haga clic en **Abrir**. La ubicación FTP se introducirá en el cuadro de diálogo Nuevo sitio web.
8. Haga clic en **Aceptar**.
9. Desmarque **El inicio de sesión anónimo** en el cuadro de diálogo Inicio de sesión FTP, ingrese sus credenciales y haga clic en **Aceptar**.
10. ¿Cuál es la dirección URL del proyecto? (La dirección URL del proyecto se mostrará en el Explorador de soluciones.)
11. En el menú **Compilar** , seleccione **Compilar sitio web** o Compilar **solución**.
12. Haga clic con el botón derecho en Default.aspx en el Explorador de soluciones y seleccione **Ver en el explorador**.
13. En el cuadro de diálogo `http://localhost/user1` URL del sitio web necesaria, escriba la dirección URL y haga clic en **Aceptar**.

> [!NOTE]
> Si aparece un error que indica la incapacidad para cargar el tipo /_Default, asegúrese de que está ejecutando ASP.NET 2.0 en el sitio Web y no en una versión anterior. Puede hacerlo desde la pestaña ASP.NET de Internet Information Services.

## <a name="opening-web-projects"></a>Apertura de proyectos web

Abrir proyectos web es similar a crear proyectos. En las secciones siguientes se llaman áreas para mantener un ojo hacia fuera mientras se trabaja dentro del IDE. También cubre el trabajo con proyectos web mediante HTTP y FTP.

Para abrir un proyecto Web, seleccione Abrir sitio web en el menú Archivo. Se le pedirá con el mismo cuadro de diálogo Elegir ubicación cubierto anteriormente y tendrá las mismas cuatro opciones disponibles: Sistema de archivos, IIS local, FTP y Sitio remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de archivos

Como se indicó anteriormente en este módulo, Visual Studio ya no usa un archivo de proyecto. Por lo tanto, si decide abrir un sitio Web desde el sistema de archivos, en realidad tiene la opción de elegir cualquier carpeta que desee, incluso si la carpeta que elija no se creó como un proyecto web inicialmente en Visual Studio. Por ejemplo, puede elegir abrir la carpeta Mis documentos como un sitio Web y Visual Studio la abrirá con gusto y mostrará los archivos como se muestra a continuación.

![Mis documentos abiertos como un sitio web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Mis documentos abiertos* como un sitio web

Dado que Visual Studio solo crea archivos y carpetas adicionales cuando es necesario, no se agregan archivos o carpetas adicionales a la ubicación que abra. Un efecto secundario de esta arquitectura es que le impide anidar sitios Web en el sistema de archivos. Por ejemplo, considere la siguiente estructura de directorios.

Proyecto web en C:/MyWebSite

Otro proyecto web en C:/MyWebSite/Nested

Al abrir el sitio Web en c:/MyWebSite, la carpeta anidada aparecerá como una subcarpeta de esa aplicación.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Al abrir sitios Web a través de HTTP, la configuración se lee desde la metabase de IIS (IIS local) o mediante Extensiones de servidor de FrontPage (sitio remoto.) Si hay aplicaciones web anidadas, también se muestran con un icono que las identifica como una aplicación. Si está familiarizado con el trabajo con aplicaciones web en FrontPage, el comportamiento en Visual Studio 2005 es similar.

Aunque Visual Studio mostrará un icono para las aplicaciones que están anidadas debajo de la aplicación que se abre actualmente dentro del IDE, no le permitirá expandirlas para ver su contenido. Sin embargo, puede hacer doble clic en ellos para abrirlos. Cuando lo haga, se le presentará un cuadro de diálogo que le pedirá que abra la aplicación web (y reemplace la solución abierta actualmente) o agregue la aplicación web a la solución actual.

![Al hacer doble clic en un icono de aplicación anidada, se le presenta este cuadro de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: Al hacer doble clic en un icono de aplicación anidada, se le presenta este cuadro de diálogo

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sitio FTP

Al abrir un sitio a través de FTP, todos los archivos se copian localmente en la carpeta temporal. La ruta de acceso completa para la ubicación de almacenamiento local se muestra en el panel Propiedades del proyecto y se crea con el siguiente formato.

C:/Documents and&lt;Settings/ User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nombre de la aplicación&gt;

Al usar FTP, Visual Studio tendrá que especificar la dirección URL base del proyecto para que pueda examinarla como se muestra a continuación. Si no especifica una dirección URL base, Visual Studio le pedirá la primera vez que intente examinar una página en el sitio Web.

![Especificación de una URL base para sitios FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: Especificación de una dirección URL base para sitios FTP

## <a name="improvements-in-compilation"></a>Mejoras en la compilación

Trabajar con aplicaciones web en Visual Studio 2005 es notablemente más rápido que las versiones anteriores. Esto se debe en gran parte a los cambios en la arquitectura de compilación.

En Visual Studio 2002 y 2003, las aplicaciones web se compilaron en un ensamblado principal que reside en la carpeta /bin. En Visual Studio 2005, se agregó una carpeta App/_Code. Las clases y otro código que no sea de IU se agregan a la carpeta App/_Code. Cuando Visual Studio compila el proyecto, todos los archivos de la carpeta App/_Code se compilan en un único archivo App/_Code.dll. El resultado de este cambio es que las compilaciones posteriores son mucho más rápidas que en versiones anteriores.

> [!NOTE]
> La utilidad de línea de comandos de MSBuild también se puede usar para compilar ASP.NET aplicaciones web. Esa herramienta se tratará en el módulo 9.

Otra mejora de compilación es la nueva opción Generar página en el menú Generar. Esta característica permite a un desarrollador volver a generar solo la página actual (junto con, por supuesto, y las dependencias) para que los cambios se puedan compilar más rápidamente. Debido a que no ofrece la compilación en segundo plano con el fin de actualizar IntelliSense, etc., se beneficiarán enormemente de esta característica, ya que permitirá que IntelliSense se actualice rápidamente simplemente reconstruyendo una sola página.

Las propiedades de compilación de un proyecto permiten configurar el tipo de compilación que se produce antes de ejecutar la página de inicio. Los desarrolladores solo pueden elegir compilar la página actual para que Visual Studio pueda iniciar la depuración de aplicaciones más rápidamente después de los cambios de código.

![La acción de inicio de la página de compilación](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: La acción de inicio de la página de compilación

Otra gran mejora de Visual Studio y la arquitectura de ASP.NET se encuentra en el área de edición y continuar. En Visual Studio 2005, los desarrolladores pueden iniciar la depuración de un proyecto y realizar cambios de código en el proyecto sin separar el depurador. De hecho, puede iniciar literalmente la depuración de un proyecto, agregar una nueva clase, agregar código a esa clase, agregar código a la página que crea una nueva instancia de esa clase y ejecutar un método de la clase, todo ello sin separar el depurador. Ejecutar el nuevo código es literalmente tan fácil como actualizar el navegador!

Haga clic aquí para ver un tutorial de vídeo de la característica de edición y continuación en Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Abrir vídeo a pantalla completa](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

La sólida funcionalidad de edición y continuación de ASP.NET 2.0 y Visual Studio 2005 se debe a un cambio arquitectónico para las aplicaciones ASP.NET. En ASP.NET 1.x, las aplicaciones creadas en Visual Studio 2002/2003 se compilaron en un ensamblado principal que se almacenó en la carpeta /bin. Todas las clases, páginas, etc. de la aplicación se compilaron en ese archivo DLL. A continuación, en tiempo de ejecución, ASP.NET compilaría todos los controles, el marcado y ASP.NET código dentro de las páginas y copiaría esos archivos DLL en la carpeta temporal ASP.NET.

En Visual Studio 2005 con ASP.NET 2.0, los dos modelos de compilación anteriores (uno para Visual Studio y otro para ASP.NET en tiempo de ejecución) se han combinado en un modelo de compilación común. Esto significa que ahora se detectan todos los problemas de compilación durante la fase de desarrollo en lugar de en tiempo de ejecución. También permite la compatibilidad con el diseñador y IntelliSense para características como controles de usuario y páginas maestras.

Haga clic aquí para ver un tutorial de vídeo de la compatibilidad del diseñador para los controles de usuario.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Abrir vídeo a pantalla completa](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Cuando se quita un control de @Register usuario de una página, la directiva permanece en el marcado y debe quitarse manualmente para evitar errores de analizador si el control de usuario se elimina del sitio Web.

Otra mejora en el modelo de compilación de Visual Studio es la característica Publicar sitio web. Dado que la característica Publicar precompila el sitio Web, los desarrolladores pueden disfrutar del rendimiento agregado de no tener que compilar nada a petición. También precompila todo el código fuente de la carpeta App/_Code en un archivo DLL para que no se deba implementar ningún código fuente.

![El cuadro de diálogo Publicar sitio web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: El cuadro de diálogo Publicar sitio web

> [!NOTE]
> La utilidad aspnet/_compile.exe también se puede utilizar para precompilar una aplicación web ASP.NET. Esa herramienta se tratará en el módulo 9.

Al publicar un sitio Web, los archivos precompilados se almacenan en la carpeta Archivos de ASP.NET temporales, como se muestra a continuación. Los archivos con una extensión de archivo *.compiled* son archivos XML que definen dependencias para archivos DLL concretos. Cualquier formulario Webo o controles de usuario se compilan en archivos DLL aleatorios que comienzan con *App/_Web/_*.

Si deja activada la casilla Permitir que *este sitio precompilado sea actualizable,* el marcado dentro de los formularios Web forms y los controles de usuario no se precompilará en un archivo DLL, lo que le permitirá realizar cambios después de la implementación. Si prefiere bloquear el marcado para que no se permitan los cambios en el contenido implementado, desactive esta casilla.

La casilla *Usar nombres fijos y ensamblados* de una sola página permite deshabilitar la compilación por lotes para que cada página se compile en un ensamblado con nombre fijo. Dejar esta casilla desactivada le permite aprovechar la compilación por lotes.

La casilla *Habilitar nombres seguros en ensamblados precompilados* le permite nombrar de forma segura los ensamblados precompilados.

> [!NOTE]
> En ASP.NET 1.x, los ensamblados con nombre seguro tenían que instalarse en la caché global de ensamblados (GAC). En ASP.NET 2.0, no es necesario instalar ensamblados con nombre seguro en la GAC.

![Una ASP.NET aplicaciones precompiladas](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: Una ASP.NET aplicaciones precompiladas

> [!NOTE]
> En la aplicación anterior, no había ningún archivo web.config. Si lo hubiera habido, se habría llamado *PrecompiledApp.config* después del proceso del sitio Web de publicación.

## <a name="improvements-in-deployment"></a>Mejoras en la implementación

Al igual que con Visual Studio 2002 y 2003, Visual Studio 2005 ofrece una característica Copiar proyecto. Sin embargo, la característica se ha reforzado en Visual Studio 2005 y ahora se denomina Copiar sitio Web.

El cuadro de diálogo Copiar sitio web se divide en un marco izquierdo y un marco derecho. El marco izquierdo se denomina sitio Web de origen y el marco derecho se denomina sitio Web remoto. Una cosa que puede confundir a algunos desarrolladores es que el sitio que se muestra en el marco correcto no es necesariamente un sitio remoto. Podría ser un sitio en el sistema de archivos local o en la instancia local de IIS. Además, el sitio que se muestra en el marco izquierdo no es necesariamente el sitio Web de origen porque el cuadro de diálogo permite publicar desde el sitio Web remoto *en* el sitio Web de origen.

Si va a copiar un proyecto en un sitio Web remoto, ese sitio debe tener las extensiones de servidor de FrontPage instaladas en él. Si no es así, tendrá que conectarse mediante FTP. Por otro lado, si va a copiar un proyecto en la instancia de IIS local, las extensiones de servidor de FrontPage no son necesarias.

> [!NOTE]
> Si intenta crear un nuevo sitio Web en la instancia de IIS local y las extensiones de servidor de FrontPage 2002 están instaladas, recibirá un mensaje de error que le indicará que la creación de sitios web no se admite en un servidor de SharePoint. En ese caso, tiene la opción de instalar las extensiones de servidor de FrontPage 2000 o quitar las extensiones de servidor de FrontPage.

Haga clic aquí para ver un tutorial de vídeo de la función Copiar sitio web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Abrir vídeo a pantalla completa](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Mejoras en la depuración

Hay cuatro mejoras clave en la depuración en Visual Studio 2005.

- La depuración local como no administrador es posible de fábrica.
- El atributo Debug para el elemento Compilation ahora es false de forma predeterminada.
- La configuración de depuración remota es más fácil que antes.
- Ahora puede depurar un sitio Web abierto a través de una ubicación FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuración como no administrador

La adición del ASP.NET servidor de desarrollo permite a los no administradores depurar fácilmente ASP.NET aplicaciones de inmediato. Cuando se depura una aplicación ASP.NET que se ejecuta en el sistema de archivos local, Visual Studio inicia el ASP.NET servidor de desarrollo en el contexto del usuario que ha iniciado sesión. A continuación, ese usuario puede depurar esa aplicación sin ninguna configuración adicional.

## <a name="debug-is-false-by-default"></a>Depurar es False de forma predeterminada

En ASP.NET 1.x, el atributo *debug* en el elemento de *compilación* del archivo web.config se estableció en *true* de forma predeterminada. Siempre se ha recomendado que los desarrolladores establezcan este atributo en *false* antes de implementar una aplicación en producción, pero como la mayoría de los desarrolladores no entienden completamente las consecuencias de dejar el atributo de depuración establecido en true, simplemente lo dejaron tal y como está.

El problema más grave de tener el atributo debug establecido en true es que deshabilita el modelo de compilación por lotes ASP.NETs. Por lo tanto, cada página se compila en un archivo DLL independiente. Si una aplicación web consta de miles de páginas (no inauditas de ninguna manera), eso significa que esa aplicación creará varios miles de archivos DLL pequeños. Mientras que estos archivos DLL son pequeños en tamaño, no se cargan en ninguna ubicación determinada en la memoria. Por lo tanto, causan fragmentación en la memoria del sistema y pueden contribuir a las apariciones de OutOfMemoryException.

En ASP.NET 2.0, el atributo debug se establece en false de forma predeterminada. Como ya ha visto, cuando un desarrollador depura una aplicación ASP.NET en Visual Studio 2005, se le pide que agregue un archivo web.config con la depuración habilitada. Si lo hace, incurre en los mismos inconvenientes que estaban presentes en ASP.NET 1.x, pero ahora se advierte claramente al desarrollador que el atributo debe restablecerse a false antes de mover la aplicación a producción.

## <a name="remote-debugging-setup-and-configuration"></a>Configuración y depuración remota

En Visual Studio 2002/2003, la depuración remota se basaba en el Administrador de depuración de máquinas (mdm.exe) y el proceso vs7jit.exe. Debido a eso, solucionar problemas de depuración remota era a menudo una caja negra para los clientes y a menudo no era mucho mejor para PSS.

Visual Studio 2005 quita la dependencia de los procesos mdm.exe y vs7jit.exe. En su lugar, ahora usa el servicio Monitor de depuración remota (msvsmon.exe.)

El requisito para depurar en Visual Studio 2005 de forma remota es bastante simple. Debe ejecutar msvsmon.exe en el servidor remoto antes de la depuración. Puede instalar el Monitor de depuración remota desde el CD de Visual Studio o simplemente puede ejecutar msvsmon.exe desde un recurso compartido sin instalar nada en absoluto en el servidor Web.

Cuando ejecuta msvsmon.exe, es probable que se queje de los puertos que se bloquean para la depuración remota. Afortunadamente, puede desbloquear fácilmente los puertos desde el mismo cuadro de diálogo de advertencia como se muestra a continuación.

![Notificación de que Firewall de Windows está bloqueando la depuración remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: Notificación de que Firewall de Windows está bloqueando la depuración remota

Una vez que haya desbloqueado los puertos necesarios para la depuración, verá el Monitor de depuración remota como se muestra a continuación. Desde esta interfaz, puede supervisar las conexiones y cambiar los permisos de depuración fácilmente.

![El monitor de depuración remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: El monitor de depuración remota

También es posible depurar remotamente una aplicación web abierta a través de FTP. Los pasos son los mismos que los anteriormente cubiertos. Sin embargo, deberá especificar una dirección URL base para examinar el proyecto FTP como se describió anteriormente en este módulo.

## <a name="lab-2"></a>Laboratorio 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuración remota con Visual Studio 2005

Este laboratorio le guiará a través de la depuración remota con Visual Studio 2005.

Haga clic aquí para ver un video tutorial de este laboratorio.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Abrir vídeo a pantalla completa](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Este laboratorio requiere que tenga dos máquinas, una que ejecute Visual Studio 2005 y la otra que ejecute IIS 5 o superior.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web en el servidor remoto.

> [!NOTE]
> Puede crear el sitio Web en una instancia iIS remota o a través de FTP.

1. Desde el servidor Web remoto, busque msvsmon.exe en el equipo de desarrollo mediante una ruta UNC y ejecútela.  
 La ubicación predeterminada para msvsmon.exe es //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Si se le pide que desbloquee los puertos para la depuración remota, hágalo.
3. Desde el equipo de desarrollo, abra el código subyacente para Default.aspx y establezca un punto de interrupción en el Page/_Load método.
4. Inicie la depuración desde el equipo de desarrollo.

Usted debe golpear el punto de interrupción como se esperaba.

## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Como ya hemos explicado, Visual Studio 2005 se suministra con un servidor web denominado servidor de desarrollo de ASP.NET. (El ASP.NET servidor de desarrollo se conoce a veces como Cassini.) Este servidor Web es un medio conveniente para examinar y depurar aplicaciones web que se ejecutan en el sistema de archivos.

El servidor de desarrollo de ASP.NET es un servidor Web restringido. No permite conexiones remotas, no permite ninguna solicitud de ningún usuario que no sea el usuario que inició el servidor web. Tampoco tiene la capacidad de servir páginas ASP. Solo se sirven ASP.NET recursos y recursos HTML (incluidas imágenes, archivos CSS, etc.).

El servidor de desarrollo de ASP.NET se puede iniciar a través de la línea de comandos ejecutando el archivo WebDev.WebServer.exe ubicado en c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/*. El siguiente cuadro de diálogo muestra los parámetros que están disponibles.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**

> [!NOTE]
> El servidor de desarrollo de ASP.NET no se admite cuando se inicia explícitamente a través de la línea de comandos.
