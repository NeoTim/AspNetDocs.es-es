---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apéndice: La aplicación de ejemplo Fix It (Creación de aplicaciones en la nube del mundo real con Azure) Microsoft Docs'
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675638"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apéndice: La aplicación de ejemplo Fix It (Creación de aplicaciones en la nube del mundo real con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Descargar el proyecto Fix It](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Building Real World Cloud Apps with Azure** e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, véase [el primer capítulo](introduction.md).

Este apéndice del libro electrónico Building Real World Cloud Apps with Azure contiene las siguientes secciones que proporcionan información adicional sobre la aplicación de ejemplo Fix It que puede descargar:

- [Problemas conocidos](#knownissues)
- [procedimientos recomendados](#bestpractices)
- [Cómo ejecutar la aplicación desde Visual Studio en el equipo local](#run-in-vs)
- [Cómo implementar la aplicación base en Azure App Service Web Apps mediante los scripts de Windows PowerShell](#deploybase)
- [Solución de problemas de los scripts de Windows PowerShell](#troubleshooting)
- [Cómo implementar la aplicación con procesamiento de colas en Azure App Service Web Apps y Azure Cloud Service](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conocidos

La aplicación Fix It fue desarrollada originalmente con el fin de ilustrar tan simple como sea posible algunos de los patrones presentados en este libro electrónico. Sin embargo, dado que el libro electrónico trata sobre la creación de aplicaciones del mundo real, sometimos el código Fix It a un proceso de revisión y prueba similar a lo que haríamos para el software lanzado. Encontramos una serie de problemas, y como con cualquier aplicación del mundo real, algunos de ellos hemos corregido y algunos de ellos que aplazamos a una versión posterior.

La siguiente lista incluye problemas que deben abordarse en una aplicación de producción, pero por una razón u otra decidimos no abordar en la versión inicial de la aplicación de ejemplo Fix It.

### <a name="security"></a>Seguridad

- Asegúrese de que no puede asignar una tarea a un propietario inexistente.
- Asegúrese de que solo puede ver y modificar las tareas que ha creado o que le han asignado.
- Utilice HTTPS para las páginas de inicio de sesión y las cookies de autenticación.
- Especifique un límite de tiempo para las cookies de autenticación.

### <a name="input-validation"></a>Validación de entradas

En general, una aplicación de producción haría más validación de entrada que la aplicación Fix It. Por ejemplo, el tamaño de imagen / tamaño de archivo de imagen permitido para la carga debe ser limitado.

### <a name="administrator-functionality"></a>Funcionalidad de administrador

Un administrador debe poder cambiar la propiedad de las tareas existentes. Por ejemplo, el creador de una tarea podría abandonar la empresa, sin dejar a nadie con autoridad para mantener la tarea a menos que se habilite el acceso administrativo.

### <a name="queue-message-processing"></a>Procesamiento de mensajes de cola

El procesamiento de mensajes de cola en la aplicación Fix It se diseñó para ser simple con el fin de ilustrar el patrón de trabajo centrado en la cola con una cantidad mínima de código. Este código simple no sería adecuado para una aplicación de producción real.

- El código no garantiza que cada mensaje de cola se procesará como máximo una vez. Cuando recibe un mensaje de la cola, hay un período de tiempo de espera, durante el cual el mensaje es invisible para otros agentes de escucha de cola. Si el tiempo de espera expira antes de que se elimine el mensaje, el mensaje vuelve a ser visible. Por lo tanto, si una instancia de rol de trabajo pasa mucho tiempo procesando un mensaje, teóricamente es posible que el mismo mensaje se procese dos veces, lo que resulta en una tarea duplicada en la base de datos. Para obtener más información acerca de este problema, consulte Uso de colas de [Azure Storage](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La lógica de sondeo de cola podría ser más rentable, mediante la recuperación por lotes de mensajes. Cada vez que se llama a [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), hay un costo de transacción. En su lugar, puede llamar a [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (tenga en cuenta el plural 's'), que obtiene varios mensajes en una sola transacción. Los costos de transacción de las colas de azure Storage son muy bajos, por lo que el impacto en los costos no es sustancial en la mayoría de los escenarios.
- El bucle estrecho en el código de procesamiento de mensajes de cola provoca la afinidad de CPU, que no utiliza las máquinas virtuales multinúcleo de forma eficaz. Un mejor diseño usaría el paralelismo de tareas para ejecutar varias tareas asincrónicas en paralelo.
- El procesamiento de mensajes de cola solo tiene un control rudimentario de excepciones. Por ejemplo, el código no controla los [mensajes dudosos.](https://msdn.microsoft.com/library/ms789028.aspx) (Cuando el procesamiento de mensajes provoca una excepción, tiene que registrar el error y eliminar el mensaje, o el rol de trabajo intentará procesarlo de nuevo y el bucle continuará indefinidamente.)

### <a name="sql-queries-are-unbounded"></a>Las consultas SQL no están enlazadas

El código Fix It actual no establece ningún límite en el número de filas que podrían devolver las consultas para las páginas de índice. Si se introduce un gran volumen de tareas en la base de datos, el tamaño de las listas resultantes recibidas podría causar problemas de rendimiento. La solución es implementar la paginación. Para obtener un ejemplo, vea [Ordenar, filtrar y paginación con Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Ver modelos recomendados

La aplicación Fix It usa la clase de entidad FixItTask para pasar información entre el controlador y la vista. Una práctica recomendada es usar modelos de vista. El modelo de dominio (por ejemplo, la clase de entidad FixItTask) está diseñado en torno a lo que se necesita para la persistencia de datos, mientras que un modelo de vista se puede diseñar para la presentación de datos. Para obtener más información, vea [12 ASP.NET mvc procedimientos recomendados](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Se recomienda un blob seguro en imágenes

La aplicación Fix It almacena las imágenes cargadas como públicas, lo que significa que cualquier persona que encuentre la dirección URL puede acceder a las imágenes. Las imágenes podrían estar protegidas en lugar de públicas.

### <a name="no-powershell-automation-scripts-for-queues"></a>Sin scripts de automatización de PowerShell para colas

Los scripts de automatización de PowerShell de ejemplo solo se escribieron para la versión base de Fix It que se ejecuta por completo en Azure App Service Web Apps. No hemos proporcionado scripts para configurar e implementar en la aplicación web más el entorno de servicio en la nube necesario para el procesamiento de colas.

### <a name="special-handling-for-html-codes-in-user-input"></a>Manejo especial para códigos HTML en la entrada del usuario

ASP.NET impide automáticamente muchas formas en las que los usuarios malintencionados pueden intentar ataques de scripting entre sitios escribiendo script en los cuadros de texto de entrada del usuario. Y la `DisplayFor` aplicación auxiliar MVC utilizada para mostrar títulos de tareas y notas codifica automáticamente los valores HTML que envía al explorador. Pero en una aplicación de producción es posible que desee tomar medidas adicionales. Para obtener más información, consulte Validación de [solicitudes en ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedimientos recomendados

A continuación se muestran algunos problemas que se han corregido después de ser descubiertos en la revisión de código y pruebas de la versión original de la aplicación Fix It. Algunos fueron causados por el codificador original no ser consciente de una práctica recomendada en particular, algunos simplemente porque el código fue escrito rápidamente y no estaba destinado para el software liberado. Estamos enumerando los problemas aquí en caso de que haya algo que aprendimos de esta revisión y pruebas que podría ser útil para otros que también están desarrollando aplicaciones web.

### <a name="dispose-the-database-repository"></a>Deseche el repositorio de la base de datos

La `FixItTaskRepository` clase debe eliminar `DbContext` la instancia de Entity Framework. Lo hicimos `IDisposable` implementando `FixItTaskRepository` en la clase:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Tenga en cuenta que `FixItTaskRepository` AutoFac eliminará automáticamente la instancia, por lo que no es necesario eliminarla explícitamente.

Otra opción es `DbContext` quitar la `FixItTaskRepository`variable miembro de `DbContext` , y en su `using` lugar crear una variable local dentro de cada método de repositorio, dentro de una instrucción. Por ejemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar singletons como tal con DI

Puesto que solo `PhotoService` se `Logger` necesita una instancia de la clase y la clase, estas clases deben [registrarse como instancias únicas para la inserción](https://code.google.com/p/autofac/wiki/InstanceScope) de dependencias en *DependenciesConfig.cs:*

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Seguridad: No muestre detalles de error a los usuarios

La aplicación Fix It original no tenía una página de error genérica y simplemente dejaque que todas las excepciones se extiendan a la interfaz de usuario, por lo que algunas excepciones, como los errores de conexión de base de datos, podrían dar lugar a que se muestre un seguimiento de pila completo en el explorador. Información detallada de error a veces puede facilitar los ataques de los usuarios malintencionados. La solución consiste en registrar los detalles de la excepción y mostrar una página de error al usuario que no incluye detalles de error. La aplicación Fix It ya estaba registrando, y `<customErrors mode=On>` para mostrar una página de error, agregamos en el archivo Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

De forma predeterminada, esto hace que se muestren los errores *views-Shared-Error.cshtml.* Puede personalizar *Error.cshtml* o crear su propia `defaultRedirect` vista de página de error y agregar un atributo. También puede especificar diferentes páginas de error para errores específicos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Seguridad: solo permite que una tarea sea editada por su creador

La página Índice de panel solo muestra las tareas creadas por el usuario que ha iniciado sesión, pero un usuario malintencionado podría crear una dirección URL con un identificador para la tarea de otro usuario. Agregamos código en *DashboardController.cs* para devolver un 404 en ese caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>No trague excepciones

La aplicación Fix It original acaba de devolver null después de registrar una excepción que resultó de una consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Esto haría que se viera para el usuario como si la consulta se realizara correctamente, pero simplemente no devolvió ninguna fila. La solución es volver a iniciar la excepción después de detectar y registrar:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Capturar todas las excepciones en roles de trabajo

Cualquier excepción no controlada en un rol de trabajo hará que la máquina virtual se recicle, por lo que desea ajustar todo lo que hace en un bloque try-catch y controlar todas las excepciones.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especifique la longitud de las propiedades de cadena en las clases de entidad

Para mostrar código simple, la versión original de la aplicación Fix It no especificaba longitudes para los campos de la entidad FixItTask y, como resultado, se definieron como varchar(max) en la base de datos. Como resultado, la interfaz de usuario aceptaría casi cualquier cantidad de entrada. Especificar longitudes establece límites que se aplican tanto a la entrada del usuario en la página web como al tamaño de columna en la base de datos:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marque a los miembros privados como de solo lectura cuando no se espera que cambien

Por ejemplo, `DashboardController` en la `FixItTaskRepository` clase se crea una instancia de y no se espera que cambie, por lo que la definimos como [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Usar lista. Any() en lugar de list. Recuento() &gt; 0

Si lo único que le interesa es si uno o más elementos de una lista se ajustan a los criterios especificados, utilice el método [Any,](https://msdn.microsoft.com/library/bb534972.aspx) ya que se devuelve tan pronto como se encuentra un elemento que se ajuste a los criterios, mientras que el `Count` método siempre tiene que recorrer en iteración cada elemento. El archivo *Dashboard Index.cshtml* originalmente tenía este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Lo cambiamos a esto:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generar direcciones URL en vistas MVC mediante aplicaciones auxiliares MVC

Para el botón **Crear un Fix It** en la página de inicio, la aplicación Fix It codificó un elemento de anclaje:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Para vínculos de vista/acción como este, es mejor utilizar la aplicación auxiliar [HTML Url.Action,](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) por ejemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Use Task.Delay en lugar de Thread.Sleep en el rol de trabajo

La plantilla de nuevo `Thread.Sleep` proyecto coloca el código de ejemplo para un rol de trabajo, pero hacer que el subproceso se quede en suspensión puede hacer que el grupo de subprocesos genere subprocesos innecesarios adicionales. Puede evitarlo mediante [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) en su lugar.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitar el vacío asincrónico

Si un método asincrónico no necesita devolver `Task` un `void`valor, devuelva un tipo en lugar de .

Este ejemplo es `FixItQueueManager` de la clase:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Solo debe `async void` usar para controladores de eventos de nivel superior. Si define un `async void`método como , el llamador no puede **esperar** el método ni detectar ninguna excepción que produce el método. Para obtener más información, vea [Prácticas recomendadas en programación asincrónica](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Use un token de cancelación para romper con el bucle de rol de trabajo

Normalmente, el método **Run** en un rol de trabajo contiene un bucle infinito. Cuando se detiene el rol de trabajo, se llama al método [RoleEntryPoint.OnStop.](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) Debe usar este método para cancelar el trabajo que se está realizando dentro del método **Run** y salir correctamente. De lo contrario, el proceso podría terminarse en medio de una operación.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Opt out of Automatic MIME Sniffing Procedure

En algunos casos, Internet Explorer notifica un tipo MIME diferente del tipo especificado por el servidor web. Por ejemplo, si Internet Explorer encuentra contenido HTML en un archivo entregado con el encabezado de respuesta HTTP Content-Type: text/plain, Internet Explorer determina que el contenido debe representarse como HTML. Desafortunadamente, este "MIME-sniffing" también puede conducir a problemas de seguridad para los servidores que alojan contenido que no es de confianza. Para combatir este problema, Internet Explorer 8 ha realizado una serie de cambios en el código de determinación de tipo MIME y permite a los desarrolladores de aplicaciones [optar por no oler MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). El código siguiente se agregó al archivo *Web.config.*

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitar agrupación y minificación

Cuando Visual Studio crea un nuevo proyecto web, la agrupación y minificación de archivos JavaScript no está habilitada de forma predeterminada. Hemos añadido una línea de código en BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Establecer un tiempo de espera de expiración para las cookies de autenticación

De forma predeterminada, las cookies de autenticación caducan en dos semanas. Un tiempo más corto es más seguro. Puede cambiar esta configuración en *StartupAuth.cs:*

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Cómo ejecutar la aplicación desde Visual Studio en el equipo local

Hay dos maneras de ejecutar la aplicación Fix It:

- Ejecute la aplicación base que escribe nuevas tareas directamente en la base de datos SQL.
- Ejecute la aplicación mediante una cola más un servicio back-end para crear tareas. El patrón de cola se describe en el capítulo [Patrón de trabajo centrado](queue-centric-work-pattern.md)en la cola .

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Ejecute la aplicación base

1. Instale [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Instale el SDK de [Azure para .NET para Visual Studio.](https://azure.microsoft.com/downloads/)
3. Descargue el archivo .zip de [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. En el Explorador de archivos, haga clic con el botón derecho en el archivo .zip y haga clic en Propiedades y, a continuación, en la ventana Propiedades, haga clic en Desbloquear.
5. Descomprima el archivo.
6. Haga doble clic en el archivo .sln para iniciar Visual Studio.
7. En el menú **Herramientas** , haga clic en Administrador de **paquetes NuGet**y, a continuación, en **Consola del Administrador de paquetes**.
8. En la consola del Administrador de paquetes (PMC), haga clic en Restaurar.
9. Salga de Visual Studio.
10. Inicie el [emulador de Azure Storage](/azure/storage/common/storage-use-emulator).
11. Reinicie Visual Studio, abriendo el archivo de solución que cerró en el paso anterior.
12. Asegúrese de que el proyecto FixIt está establecido como el proyecto de inicio y, a continuación, presione CTRL+F5 para ejecutar el proyecto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Ejecute la aplicación con el procesamiento de colas

1. Siga las instrucciones para [Ejecutar la aplicación base](#runbase)y, a continuación, cierre el explorador y cierre Visual Studio.
2. Inicie Visual Studio con privilegios de administrador. (Usará el emulador de proceso de Azure y eso requiere privilegios de administrador.)
3. En el archivo *Web.config* de la aplicación en el proyecto `appSettings/UseQueues` *MyFixIt* (el proyecto web), cambie el valor de "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Si el emulador de [Azure Storage](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) aún no se está ejecutando, inícielo de nuevo.
5. Ejecute el proyecto web FixIt y el proyecto MyFixItCloudService simultáneamente.

    Uso de Visual Studio:

   1. Presione **F5** para ejecutar el proyecto FixIt.
   2. En el **Explorador**de soluciones , haga clic con el botón secundario en el proyecto MyFixItCloudService y, a continuación, haga clic en **Depurar** > **iniciar nueva instancia**.

    Uso de Visual Studio 2013 Express para Web:

   3. En el Explorador de soluciones, haga clic con el botón secundario en la solución FixIt y seleccione **Propiedades**.
   4. Seleccione **Varios proyectos de inicio**.
   5. En la lista desplegable **Acción** en MyFixIt y MyFixItCloudService, seleccione **Iniciar**.
   6. Haga clic en **OK**.
   7. Presione **F5** para ejecutar ambos proyectos.

      Al ejecutar el proyecto MyFixItCloudService, Visual Studio inicia el emulador de proceso de Azure. Dependiendo de la configuración del firewall, es posible que deba permitir el emulador a través del firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Cómo implementar la aplicación base en Azure App Service Web Apps mediante los scripts de Windows PowerShell

Para ilustrar el patrón [Automatizar todo,](automate-everything.md) la aplicación Fix It se proporciona con scripts que configuran un entorno en Azure e implementan el proyecto en el nuevo entorno. Las instrucciones siguientes explican cómo utilizar los scripts.

Si desea ejecutar en Azure sin usar colas y ha realizado los cambios para que se ejecuten localmente con colas, asegúrese de establecer el valor de UseQueues appSetting en false antes de continuar con las siguientes instrucciones.

En estas instrucciones se supone que ya ha descargado y ejecutado la solución Fix It localmente y que tiene una cuenta de Azure o una suscripción de Azure que está autorizado a administrar.

1. Instale la consola de **Azure PowerShell.** Para obtener instrucciones, consulte [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Esta consola personalizada está configurada para funcionar con su suscripción de Azure.This customized console is configured to work with your Azure subscription. El módulo azure se instala en el directorio *Archivos* de programa y se importa automáticamente en cada uso de la consola de Azure PowerShell.

    Si prefiere trabajar en un programa host diferente, como Windows PowerShell ISE, asegúrese de usar el cmdlet [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) para importar el módulo de Azure o usar un comando en el módulo de Azure para desencadenar la importación automática del módulo.
2. Inicie Azure PowerShell con la opción **Ejecutar como administrador.**
3. Ejecute el cmdlet [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) para establecer la `RemoteSigned`directiva de ejecución de Azure PowerShell en . Escriba **Y** (para Sí) para completar el cambio de directiva.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Esta configuración le permite ejecutar scripts locales que no están firmados digitalmente. (También puede establecer la `Unrestricted`directiva de ejecución en , lo que eliminaría la necesidad del paso de desbloqueo más adelante, pero esto no se recomienda por razones de seguridad.)
4. Ejecute `Add-AzureAccount` el cmdlet para configurar PowerShell con credenciales para su cuenta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Estas credenciales caducan después de un período `Add-AzureAccount` de tiempo y debe volver a ejecutar el cmdlet. A medida que se escribe este libro electrónico, el límite de tiempo antes de que caduquen las credenciales es de 12 horas.
5. Si tiene varias suscripciones, use el cmdlet Select-AzureSubscription para especificar la suscripción en la que desea crear el entorno de prueba.
6. Importe un certificado de administración para `Get-AzurePublishSettingsFile` la `Import-AzurePublishSettingsFile` misma suscripción de Azure mediante los cmdlets y. El primero de estos cmdlets descarga un archivo de certificado y en el segundo se especifica la ubicación de ese archivo para importarlo. > [!IMPORTANT]
   > Mantenga el archivo descargado en una ubicación segura o elimínelo cuando haya terminado con él, ya que contiene un certificado que se puede usar para administrar los servicios de Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    El certificado se utiliza para una llamada a la API REST que detecta la dirección IP de la máquina de desarrollo para establecer una regla de firewall en el servidor de SQL Database.
7. Ejecute el cmdlet [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) `cd`(los alias son , `chdir`, y `sl`) para navegar hasta el directorio que contiene los scripts. (Se encuentran en la carpeta *Automation* de la carpeta Fix It.) Coloque la ruta entre comillas si alguno de los nombres de directorio contiene espacios. Por ejemplo, para `c:\Sample Apps\FixIt\Automation` navegar al directorio, puede escribir el siguiente comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que Windows PowerShell ejecute estos scripts, use el cmdlet [Unblock-File.](https://go.microsoft.com/fwlink/p/?linkid=294021) (Los scripts están bloqueados porque se descargaron de Internet.)

    > [!WARNING]
    > Seguridad: antes `Unblock-File` de ejecutarse en cualquier script o archivo ejecutable, abra el archivo en el Bloc de notas, examine los comandos y compruebe que no contienen ningún código malintencionado.

    Por ejemplo, el siguiente `Unblock-File` comando ejecuta el cmdlet en todos los scripts del directorio actual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para crear la aplicación web para la aplicación base (sin colas) Fix It, ejecute el script de creación de entorno.

    El `Name` parámetro necesario especifica el nombre de la base de datos y también se utiliza para la cuenta de almacenamiento que crea el script. El nombre debe ser único globalmente dentro del dominio azurewebsites.net. Si especifica un nombre que no es único, como Fixit o Test (o `New-AzureWebsite` incluso como en el ejemplo, fixitdemo), se produce un error en el cmdlet con un error interno que notifica un conflicto. El script convierte el nombre a todos los casos en minúsculas para cumplir con los requisitos de nombre para aplicaciones web, cuentas de almacenamiento y bases de datos.

    El `SqlDatabasePassword` parámetro necesario especifica la contraseña de la cuenta de administrador que se creará para SQL Database. No incluya caracteres XML especiales&amp; &lt; &gt; en la contraseña ( ;). Esta es una limitación de la forma en que se escribieron los scripts, no una limitación de Azure.

    Por ejemplo, si desea crear una aplicación web denominada "fixitdemo" y usar una contraseña de administrador de SQL Server de "Passw0rd1", puede escribir el siguiente comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    El nombre debe ser único en el dominio azurewebsites.net y la contraseña debe cumplir los requisitos de SQL Database para la complejidad de la contraseña. (El ejemplo Passw0rd1 cumple los requisitos.)

    Tenga en cuenta que el comando comienza con ". \". Para ayudar a evitar la ejecución malintencionada de scripts, Windows PowerShell requiere que proporcione la ruta de acceso completa al archivo de script al ejecutar un script. Puede utilizar un punto para indicar\"el directorio actual (". ) o proporcionar la ruta completa, como:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obtener más información acerca `Get-Help` del script, use el cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Puede usar `Detailed`los `Full` `Parameters`parámetros `Examples` , , y del cmdlet Get-Help para filtrar la ayuda que se devuelve.

    Si el script falla o genera errores, como "New-AzureWebsite : Call Set-AzureSubscription y Select-AzureSubscription first", es posible que no haya completado la configuración de Azure PowerShell.

    Una vez finalizado el script, puede usar el Portal de administración de Azure para ver los recursos que se crearon, como se muestra en el capítulo [Automatizar todo.](automate-everything.md)
10. Para implementar el proyecto FixIt en el nuevo entorno de Azure, use el script *AzureWebsite.ps1.* Por ejemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Cuando se realiza la implementación, el explorador se abre con Fix It ejecutándose en Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solución de problemas de los scripts de Windows PowerShell

Los errores más comunes encontrados al ejecutar estos scripts están relacionados con los permisos. Asegúrese `Add-AzureAccount` de `Import-AzurePublishSettingsFile` que se han realizado correctamente y de que los usó para la misma suscripción de Azure. Incluso `Add-AzureAccount` si tuvo éxito, es posible que tenga que ejecutarlo de nuevo. Los permisos `Add-AzureAccount` agregados expiran en 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Referencia de objeto no establecida en una instancia de un objeto.

Si el script devuelve errores, como "Referencia de objeto no establecida en una instancia de un objeto", lo que significa que `Add-AzureAccount` Windows PowerShell no puede encontrar un objeto que procesar (se trata de una excepción de referencia nula), ejecute el cmdlet e intente el script de nuevo.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: El servidor ha encontrado un error interno.

El `New-AzureWebsite` cmdlet devuelve un error interno cuando el nombre no es único en el dominio azurewebsites.net. Para resolver el error, use un valor diferente para el nombre, que se encuentra en el parámetro Name de *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reiniciar el script

Si necesita reiniciar el script *New-AzureWebsiteEnv.ps1* porque ha fallado antes de imprimir el mensaje "Script is complete", es posible que desee eliminar los recursos que creó el script antes de que se detuviera. Por ejemplo, si el script ya ha creado la aplicación web ContosoFixItDemo y vuelve a ejecutar el script con el mismo nombre, se producirá un error en el script porque el nombre está en uso.

Para determinar qué recursos creó el script antes de detenerse, use los siguientes cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: para ejecutar este cmdlet, canalizar el nombre del servidor de base de datos a: `Get-AzureSqlDatabase``Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para eliminar estos recursos, utilice los siguientes comandos. Tenga en cuenta que si elimina el servidor de bases de datos, eliminará automáticamente las bases de datos asociadas al servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Cómo implementar la aplicación con procesamiento de colas en Azure App Service Web Apps y Azure Cloud Service

Para habilitar las colas, realice el siguiente cambio en el archivo MyFixIt-Web.config. En `appSettings`, cambie `UseQueues` el valor de "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

A continuación, implemente la aplicación MVC en una aplicación web en Azure App Service, como se describió [anteriormente.](#deploybase)

A continuación, cree un nuevo servicio en la nube de Azure. Los scripts incluidos con la aplicación Fix It no crean ni implementan el servicio en la nube, por lo que debe usar Azure Portal para ello. En el portal, haga clic en **Nuevo** -- **proceso:** **creación rápida**de servicios en **la** -- nube y, a continuación, escriba una dirección URL y una ubicación del centro de datos. Use el mismo centro de datos donde implementó la aplicación web.

![](the-fix-it-sample-application/_static/image1.png)

Para poder implementar el servicio en la nube, debe actualizar algunos de los archivos de configuración.

En MyFixIt.WorkerRole-app.config, `connectionStrings`en , reemplace `appdb` el valor de la cadena de conexión por la cadena de conexión real para la base de datos SQL. Puede obtener la cadena de conexión del portal. En el portal, haga clic en Bases de **datos** - SQL**appdb** - Ver cadenas de**conexión de SQL Database para ADO .Net, ODBC, PHP y JDBC**. Copie la cadena de conexión ADO.NET y pegue el valor en el archivo app.config. Sustituya la\_contraseña\_de su base de datos por la contraseña de la base de datos. (Suponiendo que usó los scripts para implementar la aplicación MVC, especificó la contraseña de la base de datos en el `SqlDatabasePassword` parámetro de script.)

El resultado debe ser similar al siguiente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

En el mismo archivo MyFixIt.WorkerRole-app.config, en `appSettings`, reemplace los dos valores de marcador de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Puede obtener la clave de acceso desde el portal. Consulte [Cómo administrar cuentas](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)de almacenamiento .

En MyFixItCloudService-ServiceConfiguration.Cloud.cscfg, reemplace los mismos dos valores de marcador de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Ahora está listo para implementar el servicio en la nube. En Exploración de soluciones, haga clic con el botón derecho en el proyecto MyFixItCloudService y seleccione **Publicar**. Para obtener más información, vea["Implementar la aplicación en Azure",](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)que se encuentra en la parte 2 de [este tutorial.](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
