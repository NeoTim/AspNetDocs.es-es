---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Uso de ASP.NET MVC con diferentes versiones de IIS (VB) Microsoft Docs
author: rick-anderson
description: En este tutorial, aprenderá a usar ASP.NET MVC y enrutamiento de direcciones URL, con diferentes versiones de Internet Information Services. Aprendes diferentes estrategias...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 5e04ae14026e6d5dd1e603be6c52ff6876a468cf
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542006"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Usar ASP.NET MVC con distintas versiones de IIS (VB)

por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC y enrutamiento de direcciones URL, con diferentes versiones de Internet Information Services. Aprenderá diferentes estrategias para usar ASP.NET MVC con IIS 7.0 (modo clásico), IIS 6.0 y versiones anteriores de IIS.

El marco de ASP.NET MVC depende de ASP.NET enrutamiento para enrutar las solicitudes del explorador a las acciones del controlador. Para aprovechar ASP.NET enrutamiento, es posible que tenga que realizar pasos de configuración adicionales en el servidor web. Todo depende de la versión de Internet Information Services (IIS) y del modo de procesamiento de solicitudes para la aplicación.

Este es un resumen de las diferentes versiones de IIS:

- IIS 7.0 (modo integrado): no es necesario ninguna configuración especial para usar ASP.NET Routing.
- IIS 7.0 (modo clásico): debe realizar una configuración especial para usar ASP.NET enrutamiento.
- IIS 6.0 o inferior: debe realizar una configuración especial para usar ASP.NET enrutamiento.

La última versión de IIS es la versión 7.5 (en Win7). IIS 7 de IIS se incluye con Windows Server 2008 y VISTA/SP1 y versiones posteriores. También puede instalar IIS 7.0 en cualquier versión del [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)sistema operativo Vista excepto Home Basic (consulte ).

IIS 7.0 admite dos modos para procesar solicitudes. Puede utilizar el modo integrado o el modo clásico. No es necesario realizar ningún paso de configuración especial al usar IIS 7.0 en modo integrado. Sin embargo, es necesario realizar una configuración adicional al usar IIS 7.0 en modo clásico.

Microsoft Windows Server 2003 incluye IIS 6.0. No puede actualizar IIS 6.0 a IIS 7.0 cuando se utiliza el sistema operativo Windows Server 2003. Debe realizar pasos de configuración adicionales al usar IIS 6.0.

Microsoft Windows XP Professional incluye IIS 5.1. Debe realizar pasos de configuración adicionales al usar IIS 5.1.

Por último, Microsoft Windows 2000 y Microsoft Windows 2000 Professional incluye IIS 5.0. Debe realizar pasos de configuración adicionales al usar IIS 5.0.

## <a name="integrated-versus-classic-mode"></a>Modo integrado frente al modo clásico

IIS 7.0 puede procesar solicitudes mediante dos modos de procesamiento de solicitudes diferentes: integrado y clásico. El modo integrado proporciona un mejor rendimiento y más características. El modo clásico se incluye para la compatibilidad con versiones anteriores de IIS.

El modo de procesamiento de solicitudes viene determinado por el grupo de aplicaciones. Puede determinar qué modo de procesamiento está utilizando una aplicación web determinada determinando el grupo de aplicaciones asociado a la aplicación. Siga estos pasos:

1. Inicie el Administrador de Servicios de Internet Information Server
2. En la ventana Conexiones, seleccione una aplicación
3. En la ventana Acciones, haga clic en el vínculo **Configuración básica** para abrir el cuadro de diálogo Editar aplicación (consulte la figura 1)
4. Tome nota del grupo de aplicaciones seleccionado.

De forma predeterminada, IIS está configurado para admitir dos grupos de aplicaciones: **DefaultAppPool** y **Classic .NET AppPool**. Si defaultAppPool está seleccionado, la aplicación se ejecuta en modo de procesamiento de solicitudes integrado. Si se selecciona Classic .NET AppPool, la aplicación se ejecuta en modo de procesamiento de solicitudes clásico.

[![Cuadro de diálogo Nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figura 1**: Detectar el modo de procesamiento de solicitudes[(Haga clic para ver la imagen de tamaño completo)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png)

Tenga en cuenta que puede modificar el modo de procesamiento de solicitudes en el cuadro de diálogo Editar aplicación. Haga clic en el botón Seleccionar y cambie el grupo de aplicaciones asociado a la aplicación. Tenga en cuenta que hay problemas de compatibilidad al cambiar una aplicación ASP.NET de modo clásico a modo integrado. Para más información, consulte los siguientes artículos.

- Actualización de ASP.NET 1.1 a IIS 7.0 en Windows Vista y Windows Server 2008 --[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- ASP.NET Integración con IIS 7.0 -[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Si una aplicación ASP.NET usa DefaultAppPool, no es necesario realizar ningún paso adicional para que ASP.NET Routing (y, por lo tanto, ASP.NET MVC) funcionen. Sin embargo, si la aplicación ASP.NET está configurada para usar Classic .NET AppPool y, a continuación, siga leyendo, tiene más trabajo que hacer.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Uso de ASP.NET MVC con versiones anteriores de IIS

Si necesita usar ASP.NET MVC con una versión anterior de IIS que IIS 7.0, o necesita usar IIS 7.0 en modo clásico, tiene dos opciones. En primer lugar, puede modificar la tabla de rutas para utilizar extensiones de archivo. Por ejemplo, en lugar de solicitar una dirección URL como /Store/Details, solicitaría una dirección URL como /Store.aspx/Details.

La segunda opción es crear algo llamado mapa de *script comodín.* Una asignación de script comodín le permite asignar cada solicitud al marco de trabajo de ASP.NET.

Si no tiene acceso al servidor web (por ejemplo, la aplicación ASP.NET MVC está siendo hospedada por un proveedor de servicios de Internet), tendrá que usar la primera opción. Si no desea modificar el aspecto de las direcciones URL y tiene acceso a su servidor web, puede usar la segunda opción.

Exploramos cada opción en detalle en las siguientes secciones.

## <a name="adding-extensions-to-the-route-table"></a>Adición de extensiones a la tabla de rutas

La forma más fácil de ASP.NET Routing funcione con versiones anteriores de IIS es modificar la tabla de rutas en el archivo Global.asax. El archivo Global.asax predeterminado y sin modificar del listado 1 configura una ruta denominada Ruta predeterminada.

**Listado 1 - Global.asax (sin modificar)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

La ruta predeterminada configurada en el listado 1 le permite rutear los URL que parecen esto:

/Home/Index

/Producto/Detalles/3

/Producto

Desafortunadamente, las versiones anteriores de IIS no pasarán estas solicitudes al marco de trabajo de ASP.NET. Por lo tanto, estas solicitudes no se enrutarán a un controlador. Por ejemplo, si realiza una solicitud de explorador para la dirección URL /Home/Index, obtendrá la página de error en la figura 2.

[![Cuadro de diálogo Nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figura 2**: Recepción de un 404 No encontrado error[(Haga clic para ver la imagen de tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))

Las versiones anteriores de IIS solo asignan determinadas solicitudes al marco de trabajo ASP.NET. La solicitud debe ser para una URL con la extensión de archivo correcta. Por ejemplo, una solicitud para /SomePage.aspx se asigna al marco de trabajo de ASP.NET. Sin embargo, una solicitud para /SomePage.htm no lo hace.

Por lo tanto, para que ASP.NET Routing funcione, debemos modificar la ruta predeterminada para que incluya una extensión de archivo que se asigne al marco de trabajo de ASP.NET.

Esto se hace mediante `registermvc.wsf`un script denominado . Se incluyó con la versión ASP.NET MVC 1 en `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, pero a partir de [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)ASP.NET 2 este script se ha movido al ASP.NET Futuros, disponible en .

La ejecución de este script registra una nueva extensión .mvc con IIS. Después de registrar la extensión .mvc, puede modificar las rutas en el archivo Global.asax para que las rutas usen la extensión .mvc.

El archivo Global.asax modificado del listado 2 funciona con versiones anteriores de IIS.

**Listado 2 - Global.asax (modificado con extensiones)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]

Importante: recuerde volver a compilar la aplicación ASP.NET MVC después de cambiar el archivo Global.asax.

Hay dos cambios importantes en el archivo Global.asax en el listado 2. Ahora hay dos rutas definidas en Global.asax. El patrón URL para la ruta predeterminada, la primera ruta, ahora parece:

•controlador.mvc/'acción'/'id'

La adición de la extensión .mvc cambia el tipo de archivos que intercepta el módulo ASP.NET Routing. Con este cambio, la aplicación ASP.NET MVC ahora enruta solicitudes como las siguientes:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

La segunda ruta, la ruta raíz, es nueva. Este patrón de DIRECCIÓN URL para la ruta raíz es una cadena vacía. Esta ruta es necesaria para hacer coincidir las solicitudes realizadas con la raíz de la aplicación. Por ejemplo, la ruta raíz coincidirá con una solicitud que tenga este aspecto:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Después de realizar estas modificaciones en la tabla de rutas, deberá asegurarse de que todos los vínculos de la aplicación son compatibles con estos nuevos patrones de dirección URL. En otras palabras, asegúrese de que todos los vínculos incluyen la extensión .mvc. Si utiliza el método auxiliar Html.ActionLink() para generar sus enlaces, no debería tener que realizar ningún cambio.

En lugar de usar el script registermvc.wcf, puede agregar una nueva extensión a IIS que se asigna al marco de trabajo de ASP.NET de forma manual. Al agregar una nueva extensión usted mismo, asegúrese de que la casilla de **verificación verificación Verificación de que el archivo existe** no está marcada.

## <a name="hosted-server"></a>Servidor alojado

No siempre tiene acceso a su servidor web. Por ejemplo, si hospeda la aplicación ASP.NET MVC mediante un proveedor de hospedaje de Internet, no tendrá acceso a IIS.

En ese caso, debe usar una de las extensiones de archivo existentes que se asignan al marco de trabajo de ASP.NET. Entre los ejemplos de extensiones de archivo asignadas a ASP.NET se incluyen las extensiones .aspx, .axd y .ashx.

Por ejemplo, el archivo Global.asax modificado del listado 3 usa la extensión .aspx en lugar de la extensión .mvc.

**Listado 3 - Global.asax (modificado con extensiones .aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

El Global.asax archivo en el listado 3 es exactamente el mismo que el anterior Global.asax archivo, excepto por el hecho de que utiliza la extensión .aspx en lugar de la extensión .mvc. No es posible realizar ninguna configuración en el servidor web remoto para usar la extensión .aspx.

## <a name="creating-a-wildcard-script-map"></a>Creación de un mapa de secuencias de comandos comodín

Si no desea modificar las direcciones URL de la aplicación ASP.NET MVC y tiene acceso al servidor web, tendrá una opción adicional. Puede crear un mapa de script comodín que asigne todas las solicitudes al servidor web al marco de trabajo de ASP.NET. De este modo, puede usar la tabla de rutas ASP.NET MVC predeterminada con IIS 7.0 (en modo clásico) o IIS 6.0.

Tenga en cuenta que esta opción hace que IIS intercepte todas las solicitudes realizadas en el servidor web. Esto incluye solicitudes de imágenes, páginas ASP clásicas y páginas HTML. Por lo tanto, habilitar una asignación de script comodín para ASP.NET tiene implicaciones de rendimiento.

A continuación se muestra cómo habilitar una asignación de script comodín para IIS 7.0:

1. Seleccione la aplicación en la ventana Conexiones
2. Asegúrese de que la vista **Características** esté seleccionada
3. Haga doble clic en el botón **Asignaciones** de controlador
4. Haga clic el link del mapa del **script del comodín** del agregar (véase el cuadro 3)
5. Escriba la ruta de\_acceso al archivo isapi.dll de aspnet (puede copiar esta ruta de acceso desde el mapa de scripts PageHandlerFactory)
6. Escriba el nombre MVC
7. Haga clic en el botón **Aceptar**

[![Cuadro de diálogo Nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figura 3**: Creación de un mapa de script comodín con IIS 7.0(Haga[clic para ver la imagen de tamaño completo)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png)

Siga estos pasos para crear un mapa de script comodín con IIS 6.0:

1. Haga clic con el botón derecho en un sitio web y seleccione Propiedades
2. Seleccione la pestaña **Directorio de** inicio
3. Haga clic en el botón **Configuración**
4. Seleccione la pestaña **Asignaciones**
5. Haga clic en el botón **Insertar** (consulte la figura 4)
6. Pegue la ruta de\_acceso al archivo aspnet isapi.dll en el campo Ejecutable (puede copiar esta ruta de acceso desde la asignación de scripts para archivos .aspx)
7. Desmarque la casilla de verificación etiquetada **Verificar que el archivo existe**
8. Haga clic en el botón **Aceptar**

[![Cuadro de diálogo Nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figura 4**: Creación de un mapa de script comodín con IIS 6.0[(Haga clic para ver la imagen de tamaño completo)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png)

Después de habilitar las asignaciones de script comodín, debe modificar la tabla de rutas en el archivo Global.asax para que incluya una ruta raíz. De lo contrario, obtendrá la página de error en la figura 5 cuando realice una solicitud para la página raíz de la aplicación. Puede utilizar el archivo Global.asax modificado en el listado 4.

[![Cuadro de diálogo Nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figura 5**: Falta error de ruta raíz[(haga clic para ver la imagen de tamaño completo)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png)

**Listado 4 - Global.asax (modificado con ruta Root)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Después de habilitar una asignación de script comodín para IIS 7.0 o IIS 6.0, puede realizar solicitudes que funcionen con la tabla de rutas predeterminada que tenga este aspecto:

/

/Home/Index

/Producto/Detalles/3

/Producto

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede usar ASP.NET MVC al usar una versión anterior de IIS (o IIS 7.0 en modo clásico). Hemos discutido dos métodos para que ASP.NET Routing funcione con versiones anteriores de IIS: Modificar la tabla de rutas predeterminada o crear una asignación de script comodín.

La primera opción requiere que modifique las direcciones URL utilizadas en la aplicación ASP.NET MVC. Una ventaja muy significativa de esta primera opción es que usted no necesita el acceso a un servidor Web para modificar la tabla de ruteo. Esto significa que puede usar esta primera opción incluso al hospedar la aplicación ASP.NET MVC con una empresa de hospedaje de Internet.

La segunda opción es crear un mapa de script comodín. La ventaja de esta segunda opción es que no es necesario modificar las direcciones URL. La desventaja de esta segunda opción es que puede afectar al rendimiento de la aplicación ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
