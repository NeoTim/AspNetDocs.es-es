---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'ASP.NET Web Deployment using Visual Studio: Web.config File Transformations ( Web.config File Transformations) Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación web ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675752"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET Implementación web mediante Visual Studio: Transformaciones de archivos Web.config

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar Starter Project](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación web ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, consulte [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general

En este tutorial se muestra cómo automatizar el proceso de cambiar el archivo *Web.config* al implementarlo en diferentes entornos de destino. La mayoría de las aplicaciones tienen valores en el archivo *Web.config* que debe ser diferente cuando se implementa la aplicación. Automatizar el proceso de realizar estos cambios le impide tener que hacerlos manualmente cada vez que implemente, lo que sería tedioso y propenso a errores.

Recordatorio: Si recibe un mensaje de error o algo no funciona a medida que avanza por el tutorial, asegúrese de comprobar la [página de solución de problemas](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformaciones Web.config frente a parámetros de Web Deploy

Hay dos maneras de automatizar el proceso de cambiar la configuración del archivo *Web.config:* [transformaciones Web.config](https://msdn.microsoft.com/library/dd465326.aspx) y parámetros de [Web Deploy.](https://msdn.microsoft.com/library/ff398068.aspx) Un archivo de transformación *Web.config* contiene marcado XML que especifica cómo cambiar el archivo *Web.config* cuando se implementa. Puede especificar diferentes cambios para configuraciones de compilación específicas y para perfiles de publicación específicos. Las configuraciones de compilación predeterminadas son Depurar y Liberar, y puede crear configuraciones de compilación personalizadas. Un perfil de publicación normalmente corresponde a un entorno de destino. (Obtendrá más información sobre los perfiles de publicación en el implementar en [IIS como un entorno](deploying-to-iis.md) de prueba tutorial.)

Los parámetros de Web Deploy se pueden usar para especificar muchos tipos diferentes de configuración que se deben configurar durante la implementación, incluida la configuración que se encuentra en los archivos *Web.config.* Cuando se usa para especificar cambios en el archivo *Web.config,* los parámetros de Web Deploy son más complejos de configurar, pero son útiles cuando no se conoce el valor que se establecerá hasta que se implemente. Por ejemplo, en un entorno empresarial, puede crear un paquete de *implementación* y dárselo a una persona del departamento de TI para instalarlo en producción, y esa persona debe poder escribir cadenas de conexión o contraseñas que no conozca.

Para el escenario que cubre esta serie de tutoriales, sabe de antemano todo lo que se debe hacer en el archivo *Web.config,* por lo que no es necesario usar parámetros de Web Deploy. Configurará algunas transformaciones que difieren en función de la configuración de compilación utilizada y otras que difieren en función del perfil de publicación utilizado.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especificación de la configuración de Web.config en Azure

Si la configuración del archivo *Web.config* que `<connectionStrings>` desea `<appSettings>` cambiar se encuentra en el elemento o en el elemento, y si va a implementar en Web Apps en Azure App Service, tiene otra opción para automatizar los cambios durante la implementación. Puede escribir la configuración que desea que surta efecto en Azure en la pestaña **Configurar** de la página del portal de administración de la aplicación web (desplazarse hacia abajo hasta las secciones de configuración de la **aplicación** y cadenas de **conexión).** Al implementar el proyecto, Azure aplica automáticamente los cambios. Para obtener más información, vea Sitios web de [Windows Azure: Cómo funcionan](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)las cadenas de aplicación y las cadenas de conexión.

## <a name="default-transformation-files"></a>Archivos de transformación predeterminados

En **el Explorador**de soluciones , expanda *Web.config* para ver los archivos de transformación *Web.Debug.config* y *Web.Release.config* que se crean de forma predeterminada para las dos configuraciones de compilación predeterminadas.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Puede crear archivos de transformación para configuraciones de compilación personalizadas haciendo clic con el botón derecho en el archivo Web.config y seleccionando **Agregar transformaciones** de configuración en el menú contextual. Para este tutorial no es necesario hacerlo y la opción de menú está deshabilitada, porque no ha creado ninguna configuración de compilación personalizada.

Más adelante creará tres archivos de transformación más, uno para los perfiles de publicación de prueba, ensayo y producción. Un ejemplo típico de una configuración que controlaría en un archivo de transformación de perfil de publicación porque depende del entorno de destino es un extremo WCF que es diferente para la prueba frente a la producción. Creará archivos de transformación de perfil de publicación en tutoriales posteriores después de crear los perfiles de publicación con los que van.

## <a name="disable-debug-mode"></a>Desactivar el modo de depuración

Un ejemplo de una configuración que depende de `debug` la configuración de compilación en lugar del entorno de destino es el atributo. Para una compilación de versión, normalmente desea deshabilitar la depuración independientemente del entorno en el que se va a implementar. Por lo tanto, de forma predeterminada, las plantillas de proyecto de `debug` Visual `compilation` Studio crean archivos de transformación *Web.Release.config* con código que quita el atributo del elemento. Aquí está el valor predeterminado *Web.Release.config*: además de algún código `compilation` de transformación `debug` de ejemplo que se comenta, incluye código en el elemento que quita el atributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

El `xdt:Transform="RemoveAttributes(debug)"` atributo especifica que `debug` desea que el `system.web/compilation` atributo se quite del elemento en el archivo *Web.config* implementado. Esto se hará cada vez que implemente una compilación de versión.

## <a name="limit-error-log-access-to-administrators"></a>Limite el acceso al registro de errores a los administradores

Si se produce un error mientras se ejecuta la aplicación, la aplicación muestra una página de error genérica en lugar de la página de error generada por el sistema y usa el [paquete NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) de Elmah para el registro de errores y los informes. El `customErrors` elemento del archivo *Web.config* de la aplicación especifica la página de error:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver la página de `mode` error, `customErrors` cambie temporalmente el atributo del elemento de "RemoteOnly" a "On" y ejecute la aplicación desde Visual Studio. Causa un error solicitando una dirección URL no válida, como *Studentsxxx.aspx*. En lugar de una página de error generada por IIS "No se puede encontrar el recurso", verá la *genericErrorPage.aspx* página.

![Página de error](web-config-transformations/_static/image2.png)

Para ver el registro de errores, reemplace todo en la dirección URL después del número de puerto por *elmah.axd* (por ejemplo, `http://localhost:51130/elmah.axd`) y pulse Intro:

![Página ELMAH](web-config-transformations/_static/image3.png)

No olvide volver a `customErrors` establecer el elemento en el modo "RemoteOnly" cuando haya terminado.

En el equipo de desarrollo es conveniente permitir el acceso gratuito a la página de registro de errores, pero en producción que sería un riesgo para la seguridad. Para el sitio de producción, desea agregar una regla de autorización que restrinja el acceso al registro de errores a los administradores y asegurarse de que la restricción funciona también en la prueba y el ensayo. Por lo tanto, este es otro cambio que desea implementar cada vez que implemente una compilación de versión, por lo que pertenece al archivo *Web.Release.config.*

Abra *Web.Release.config* y `location` agregue un nuevo `configuration` elemento inmediatamente antes de la etiqueta de cierre, como se muestra aquí.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

El `Transform` valor de atributo de `location` "Insert" hace que este `location` elemento se agregue como un elemento relacionado a los elementos existentes en el *Web.config* archivo. (Ya hay `location` un elemento que especifica reglas de autorización para la página **Actualizar créditos.)**

Ahora puede obtener una vista previa de la transformación para asegurarse de que la ha codificado correctamente.

En **el Explorador**de soluciones , haga clic con el botón secundario en *Web.Release.config* y haga clic en **Transformación previa**.

![Menú Vista previa de la transformación](web-config-transformations/_static/image4.png)

Se abre una página que muestra el archivo *Web.config* de desarrollo a la izquierda y el aspecto que tendrá el archivo *Web.config* implementado a la derecha, con los cambios resaltados.

![Vista previa de la transformación de depuración](web-config-transformations/_static/image5.png)

![Vista previa de la transformación de ubicación](web-config-transformations/_static/image6.png)

( En la vista previa, es posible que observe algunos cambios adicionales para los que no escribió transformaciones: normalmente implican la eliminación de espacios en blanco que no afectan a la funcionalidad.)

Cuando pruebe el sitio después de la implementación, también probará para comprobar que la regla de autorización es eficaz.

> [!NOTE] 
> 
> **Nota de seguridad** Nunca muestre los detalles de error al público en una aplicación de producción ni almacene esa información en una ubicación pública. Los atacantes pueden usar información de error para detectar vulnerabilidades en un sitio. Si utiliza ELMAH en su propia aplicación, configure ELMAH para minimizar los riesgos de seguridad. El ejemplo ELMAH de este tutorial no debe considerarse una configuración recomendada. Es un ejemplo que se eligió para ilustrar cómo manejar una carpeta en la que la aplicación debe ser capaz de crear archivos. Para obtener más información, consulte [protección del punto](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)de conexión ELMAH .

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Una configuración que controlará en los archivos de transformación de perfil de publicación

Un escenario común es tener la configuración del archivo *Web.config* que debe ser diferente en cada entorno en el que se implemente. Por ejemplo, una aplicación que llama a un servicio WCF puede necesitar un extremo diferente en entornos de prueba y producción. La aplicación Contoso University también incluye una configuración de este tipo. Esta configuración controla un indicador visible en las páginas de un sitio que le indica en qué entorno se encuentra, como desarrollo, prueba o producción. El valor de configuración determina si la aplicación anexará "(Dev)" o "(Test)" al encabezado principal de la página maestra *Site.Master:*

![Indicador de medio ambiente](web-config-transformations/_static/image7.png)

El indicador de entorno se omite cuando la aplicación se ejecuta en ensayo o en producción.

Las páginas web de Contoso University `appSettings` leen un valor que se establece en el archivo *Web.config* para determinar en qué entorno se ejecuta la aplicación:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

El valor debe ser "Test" en el entorno de prueba y "Prod" para la puesta en escena y la producción.

El código siguiente en un archivo de transformación implementará esta transformación:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

El `xdt:Transform` valor de atributo "SetAttributes" indica que el propósito de esta transformación es cambiar los valores de atributo de un elemento existente en el archivo *Web.config.* El `xdt:Locator` valor de atributo "Match(key)" indica que el `key` elemento que `key` se va a modificar es aquel cuyo atributo coincide con el atributo especificado aquí. El único otro `add` atributo `value`del elemento es , y eso es lo que se cambiará en el archivo *Web.config* implementado. El código que `value` se muestra `Environment` `appSettings` aquí hace que el atributo del elemento se establezca en "Test" en el archivo *Web.config* que se implementa.

Esta transformación pertenece a los archivos de transformación de perfil de publicación, que aún no ha creado. Creará y actualizará los archivos de transformación que implementan este cambio al crear los perfiles de publicación para los entornos de prueba, ensayo y producción. Lo hará en la [implementación en IIS](deploying-to-iis.md) y los tutoriales de [implementación en producción.](deploying-to-production.md)

> [!NOTE]
> Dado que esta `<appSettings>` configuración está en el elemento, tiene otra alternativa para especificar la transformación al implementar en Web Apps en Azure App Service Consulte Especificación de la configuración de [Web.config en Azure](#watransforms) anteriormente en este tema.

## <a name="setting-connection-strings"></a>Definición de cadenas de conexión

Aunque el archivo de transformación predeterminado contiene un ejemplo que muestra cómo actualizar una cadena de conexión, en la mayoría de los casos no es necesario configurar transformaciones de cadena de conexión, ya que puede especificar cadenas de conexión en el perfil de publicación. Lo hará en la [implementación en IIS](deploying-to-iis.md) y los tutoriales de [implementación en producción.](deploying-to-production.md)

## <a name="summary"></a>Resumen

Ahora ha hecho todo lo que puede con las transformaciones *Web.config* antes de crear los perfiles de publicación y ha visto una vista previa de lo que estará en el archivo Web.config implementado.

![Vista previa de la transformación de ubicación](web-config-transformations/_static/image8.png)

En el siguiente tutorial, se encargará de las tareas de configuración de implementación que requieren la configuración de las propiedades del proyecto.

## <a name="more-information"></a>Más información

Para obtener más información acerca de los temas cubiertos por este tutorial, vea Uso de [transformaciones Web.config para cambiar la configuración en el archivo Web.config de destino o el archivo app.config durante](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) la implementación en el mapa de contenido de implementación web para Visual Studio y ASP.NET.

> [!div class="step-by-step"]
> [Anterior](preparing-databases.md)
> [Siguiente](project-properties.md)
