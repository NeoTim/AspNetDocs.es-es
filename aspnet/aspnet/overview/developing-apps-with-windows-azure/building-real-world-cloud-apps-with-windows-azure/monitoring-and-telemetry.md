---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Supervisión y telemetría (Creación de aplicaciones en la nube del mundo real con Azure) Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que puede...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675674"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Supervisión y telemetría (creación de aplicaciones en la nube del mundo real con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Descargar Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Descargar E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Building Real World Cloud Apps with Azure** e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, véase [el primer capítulo](introduction.md).

Muchas personas confían en los clientes para hacerles saber cuando su aplicación está incayéndose. Eso no es realmente una mejor práctica en cualquier lugar, y especialmente no en la nube. No hay garantía de notificación rápida, y cuando recibe sunación, a menudo obtiene datos mínimos o engañosos sobre lo que sucedió. Con buenos sistemas de telemetría y registro, puede ser consciente de lo que está pasando con la aplicación, y cuando algo sale mal, de inmediato se entera y tiene información útil para solucionar problemas.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar o alquilar una solución de telemetría

> [!NOTE]
> Este artículo se escribió antes de que se publicara [Application Insights.](/azure/application-insights/app-insights-overview) Application Insights es el enfoque preferido para las soluciones de telemetría en Azure. Consulte [Configurar Application Insights para su sitio web de ASP.NET](/azure/application-insights/app-insights-asp-net) para obtener más información.

Una de las cosas que es genial sobre el entorno de nube es que es muy fácil comprar o alquilar su camino a la victoria. La telemetría es un ejemplo. Sin mucho esfuerzo puede obtener un sistema de telemetría muy bueno en funcionamiento, muy rentable. Hay un montón de excelentes socios que se integran con Azure y algunos de ellos tienen niveles gratuitos, por lo que puede obtener telemetría básica para nada. Estos son solo algunos de los que están disponibles actualmente en Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) también incluye características de supervisión.

Le guiaremos rápidamente a través de la configuración de New Relic para mostrar lo fácil que puede ser usar un sistema de telemetría.

En Azure Management Portal, regístrese en el servicio. Haga clic en **Nuevo**y, a continuación, haga clic en **Almacenar**. Aparece el cuadro de diálogo **Elegir un complemento.** Desplácese hacia abajo y haga clic en **Nueva reliquia**.

![Elija un complemento](monitoring-and-telemetry/_static/image1.png)

Haga clic en la flecha derecha y elija el nivel de servicio que desee. Para esta demostración usaremos el nivel gratuito.

![Personalizar el complemento](monitoring-and-telemetry/_static/image2.png)

Haz clic en la flecha derecha, confirma la "compra" y Nueva reliquia ahora aparece como un complemento en el portal.

![Revisar la compra](monitoring-and-telemetry/_static/image3.png)

![Nuevo complemento de Relic en el portal de administración](monitoring-and-telemetry/_static/image4.png)

Haga clic en **Información de conexión**y copie la clave de licencia.

![Información de conexión](monitoring-and-telemetry/_static/image5.png)

Vaya a la pestaña **Configurar** de la aplicación web en el portal, establezca Supervisión del **rendimiento** **en Complemento**y establezca la lista desplegable Elegir **complemento** en Nueva **reliquia**. A continuación, haga clic en **Guardar**.

![Nueva reliquia en la pestaña Configurar](monitoring-and-telemetry/_static/image6.png)

En Visual Studio, instale el paquete NuGet New Relic en la aplicación.

![Análisis de desarrolladores en la pestaña Configurar](monitoring-and-telemetry/_static/image7.png)

Implemente la aplicación en Azure y comience a usarla. Crea algunas tareas Fix It para proporcionar alguna actividad para que New Relic la supervise.

A continuación, vuelva a la página **Nueva reliquia** en la pestaña **Complementos** del portal y haga clic en **Administrar**. El portal le envía al portal de administración de New Relic, mediante el inicio de sesión único para la autenticación, de modo que no tenga que volver a escribir sus credenciales. La página Información general presenta una variedad de estadísticas de rendimiento. (Haga clic en la imagen para ver el tamaño completo de la página de información general.)

[![Nueva pestaña De monitoreo de reliquias](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Estas son solo algunas de las estadísticas que puede ver:

- Tiempo medio de respuesta en diferentes momentos del día.

    ![Tiempo de respuesta](monitoring-and-telemetry/_static/image10.png)
- Tasas de rendimiento (en solicitudes por minuto) en diferentes horas del día.

    ![Throughput](monitoring-and-telemetry/_static/image11.png)
- Tiempo de CPU del servidor dedicado a controlar diferentes solicitudes HTTP.

    ![Tiempos de transacciones web](monitoring-and-telemetry/_static/image12.png)
- Tiempo de CPU invertido en diferentes partes del código de aplicación:

    ![Detalles de la traza](monitoring-and-telemetry/_static/image13.png)
- Estadísticas históricas de rendimiento.

    ![Rendimiento histórico](monitoring-and-telemetry/_static/image14.png)
- Llamadas a servicios externos como el servicio Blob y estadísticas sobre lo confiable y responsivo que ha sido el servicio.

    ![Servicios externos](monitoring-and-telemetry/_static/image15.png)

    ![Servicios externos](monitoring-and-telemetry/_static/image16.png)

    ![Servicio externo](monitoring-and-telemetry/_static/image17.png)
- Información sobre dónde proviene el tráfico de aplicaciones web de EE. UU. en el mundo o en el tráfico de aplicaciones web de EE. UU.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

También puede configurar informes y eventos. Por ejemplo, puede decir que cada vez que empiece a ver errores, envíe un correo electrónico para alertar al personal de soporte técnico sobre el problema.

![Informes](monitoring-and-telemetry/_static/image19.png)

New Relic es solo un ejemplo de un sistema de telemetría; usted puede obtener todo esto de otros servicios, así. La belleza de la nube es que sin tener que escribir ningún código, y por un gasto mínimo o sin gastos, de repente puede obtener mucha más información sobre cómo se usa la aplicación y lo que sus clientes están experimentando realmente.

<a id="log"></a>
## <a name="log-for-insight"></a>Registro para obtener información

Un paquete de telemetría es un buen primer paso, pero todavía tiene que instrumentar su propio código. El servicio de telemetría le indica cuándo hay un problema y le indica lo que experimentan los clientes, pero puede que no le dé mucha información sobre lo que está sucediendo en el código.

No desea tener que remoto a un servidor de producción para ver lo que está haciendo la aplicación. Eso puede ser práctico cuando tienes un servidor, pero ¿qué pasa cuando has escalado a cientos de servidores y no sabes en cuáles necesitas remotos? El registro debe proporcionar suficiente información que nunca tendrá que remoto en los servidores de producción para analizar y depurar problemas. Usted debe estar registrando suficiente información para que usted pueda aislar los problemas solamente a través de los registros.

### <a name="log-in-production"></a>Inicie sesión en la producción

Muchas personas activan el seguimiento en producción solo cuando hay un problema y quieren depurar. Esto puede introducir un retraso sustancial entre el momento en que conoce un problema y el momento en que obtiene información útil para solucionar problemas al respecto. Y la información que obtiene podría no ser útil para errores intermitentes.

Lo que recomendamos en el entorno de nube donde el almacenamiento es barato es que siempre deje el inicio de sesión en producción. De esta forma, cuando se producen errores, ya los tiene registrados y tiene datos históricos que pueden ayudarle a analizar problemas que se desarrollan con el tiempo o que ocurren regularmente en diferentes momentos. Puede automatizar un proceso de purga para eliminar registros antiguos, pero es posible que le resulte más costoso configurar un proceso de este tipo que mantener los registros.

El gasto adicional de registro es trivial en comparación con la cantidad de tiempo de solución de problemas y dinero que puede ahorrar al tener toda la información que necesita ya disponible cuando algo sale mal. Luego, cuando alguien te dice que tuvo un error aleatorio en algún momento alrededor de las 8:00 de anoche, pero no recuerdan el error, puedes averiguar fácilmente cuál fue el problema.

Por menos de $4 al mes puede mantener 50 gigabytes de registros a mano, y el impacto en el rendimiento del registro es trivial siempre y cuando tenga una cosa en mente: para evitar cuellos de botella de rendimiento, asegúrese de que la biblioteca de registro es asincrónica.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar los registros que informan de los registros que requieren acción

Los registros están destinados a INFORMAR (quiero que sepas algo) o ACT (quiero que hagas algo). Tenga cuidado de escribir solo registros ACT para problemas que realmente requieren que una persona o un proceso automatizado tome medidas. Demasiados registros ACT crearán ruido, requiriendo demasiado trabajo para tamificar todo para encontrar problemas genuinos. Y si los registros de ACT activan automáticamente alguna acción, como el envío de correo electrónico al personal de soporte técnico, evite permitir que miles de estas acciones se desencadenen por un solo problema.

En el seguimiento de System.Diagnostics de .NET, los registros se pueden asignar a los niveles Error, Warning, Info y Debug/Verbose. Puede diferenciar ACT de los registros INFORM reservando el nivel de error para los registros DE ACT y utilizando los niveles inferiores para los registros INFORM.

![Niveles de registro](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurar niveles de registro en tiempo de ejecución

Aunque vale la pena tener el registro siempre activado en producción, otra práctica recomendada es implementar un marco de registro que le permita ajustar en tiempo de ejecución el nivel de detalle que está registrando, sin volver a implementar o reiniciar la aplicación. Por ejemplo, cuando utilice `System.Diagnostics` el recurso de seguimiento en puede crear registros error, advertencia, información y depuración/verbos. Se recomienda que siempre registre los registros error, advertencia e información en producción y podrá poder agregar dinámicamente el registro de depuración/verbos para solucionar problemas caso por caso.

Las aplicaciones web de Azure App Service `System.Diagnostics` tienen compatibilidad integrada para escribir registros en el sistema de archivos, Table Storage o Blob Storage. Puede seleccionar diferentes niveles de registro para cada destino de almacenamiento y puede cambiar el nivel de registro sobre la marcha sin reiniciar la aplicación. La compatibilidad con Blob Storage facilita la ejecución de trabajos de análisis de [HDInsight](https://docs.microsoft.com/azure/hdinsight/) en los registros de aplicaciones, ya que HDInsight sabe cómo trabajar directamente con Blob Storage.

### <a name="log-exceptions"></a>para registrar excepciones

No pongas *excepción. ToString()* en el código de registro. Eso deja fuera la información contextual. En el caso de errores SQL, deja fuera el número de error SQL. Para todas las excepciones, incluya información de contexto, la propia excepción y excepciones internas para asegurarse de que proporciona todo lo que se necesita para solucionar problemas. Por ejemplo, la información de contexto puede incluir el nombre del servidor, un identificador de transacción y un nombre de usuario (¡pero no la contraseña ni ningún secreto!).

Si confía en cada desarrollador para hacer lo correcto con el registro de excepciones, algunos de ellos no lo harán. Para asegurarse de que se realiza de la manera correcta cada vez, cree el control de excepciones directamente en la interfaz del registrador: pase el propio objeto de excepción a la clase de registrador y registre los datos de excepción correctamente en la clase de registrador.

### <a name="log-calls-to-services"></a>Registrar llamadas a servicios

Le recomendamos que escriba un registro cada vez que la aplicación llame a un servicio, ya sea a una base de datos o a una API de REST o a cualquier servicio externo. Incluya en los registros no solo una indicación de éxito o error, sino también cuánto tiempo tardó cada solicitud. En el entorno de nube, a menudo verá problemas relacionados con ralentizaciones en lugar de interrupciones completas. Algo que normalmente toma 10 milisegundos puede comenzar de repente a tomar un segundo. Cuando alguien te dice que tu aplicación es lenta, quieres poder ver New Relic o cualquier servicio de telemetría que tengas y validar su experiencia, y luego quieres poder buscar tus propios registros para profundizar en los detalles de por qué es lento.

### <a name="use-an-ilogger-interface"></a>Utilice una interfaz ILogger

Lo que recomendamos hacer al crear una aplicación de producción es crear una interfaz *ILogger* simple y pegar algunos métodos en ella. Esto hace que sea fácil cambiar la implementación de registro más adelante y no tener que ir a través de todo el código para hacerlo. Podríamos estar `System.Diagnostics.Trace` usando la clase en toda la aplicación Fix It, pero en su lugar la estamos usando bajo las cubiertas en una clase de registro que implementa *ILogger,* y realizamos llamadas al método *ILogger* en toda la aplicación.

De esa manera, si alguna vez desea [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) enriquecer el registro, puede reemplazarlo con cualquier mecanismo de registro que desee. Por ejemplo, a medida que la aplicación crece, es posible que decida que desea usar un paquete de registro más completo, como [NLog](http://nlog-project.org/) o [Enterprise Library Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) es otro marco de registro popular, pero no realiza el registro asincrónico.)

Una posible razón para usar un marco como NLog es facilitar la división de la salida de registro en almacenes de datos de alto volumen y alto valor independientes. Esto le ayuda a almacenar de forma eficiente grandes volúmenes de datos INFORM en los que no necesita ejecutar consultas rápidas, al tiempo que mantiene un acceso rápido a los datos DE ACT.

### <a name="semantic-logging"></a>Registro semántico

Para obtener una forma relativamente nueva de realizar el registro que puede producir información de diagnóstico más útil, vea Bloque de aplicación de [registro semántico (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)de la biblioteca empresarial. SLAB usa el seguimiento de eventos [para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) y la compatibilidad con [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) en .NET 4.5 para permitirle crear registros más estructurados y consultables. Defina un método diferente para cada tipo de evento que registre, lo que le permite personalizar la información que escribe. Por ejemplo, para registrar un error `LogSQLDatabaseError` de SQL Database puede llamar a un método. Para ese tipo de excepción, sabe que una información clave es el número de error, por lo que podría incluir un parámetro de número de error en la firma del método y registrar el número de error como un campo independiente en el registro de registro que escriba. Dado que el número está en un campo independiente, puede obtener informes de forma más fácil y fiable basados en números de error SQL que si solo estuviera concatenando el número de error en una cadena de mensaje.

## <a name="logging-in-the-fix-it-app"></a>Iniciar sesión en la aplicación Fix It

### <a name="the-ilogger-interface"></a>La interfaz ILogger

Aquí está la interfaz *iLogger* en la aplicación Fix It.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Estos métodos le permiten escribir registros en los mismos cuatro niveles admitidos por *System.Diagnostics*. Los métodos TraceApi son para registrar llamadas de servicio externo con información sobre la latencia. También puede agregar un conjunto de métodos para el nivel Debug/Verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>La implementación del maderero de la interfaz ILogger

La implementación de la interfaz es muy simple. Básicamente sólo llama a los *métodos estándar System.Diagnostics.* El siguiente fragmento de código muestra los tres métodos Information y uno de los demás.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Llamar a los métodos ILogger

Cada vez que el código de la aplicación Fix It detecta una excepción, llama a un método *ILogger* para registrar los detalles de la excepción. Y cada vez que realiza una llamada a la base de datos, blob service o una API REST, inicia un cronómetro antes de la llamada, detiene el cronómetro cuando el servicio devuelve y registra el tiempo transcurrido junto con información sobre el éxito o el error.

Observe que el mensaje de registro incluye el nombre de clase y el nombre del método. Es una buena práctica asegurarse de que los mensajes de registro identifican qué parte del código de la aplicación los escribió.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Así que ahora por cada vez que la aplicación Fix It ha realizado una llamada a SQL Database, puede ver la llamada, el método que la llamó y exactamente cuánto tiempo tardó.

![Consulta de SQL Database en registros](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si va a navegar por los registros, puede ver que el tiempo que tardan las llamadas a la base de datos es variable. Esa información podría ser útil: dado que la aplicación registra todo esto, puede analizar las tendencias históricas en el rendimiento del servicio de base de datos con el tiempo. Por ejemplo, un servicio puede ser rápido la mayor parte del tiempo, pero las solicitudes pueden fallar o las respuestas pueden ralentizarse en ciertas horas del día.

Puede hacer lo mismo para el servicio Blob: cada vez que la aplicación carga un nuevo archivo, hay un registro y puede ver exactamente cuánto tiempo se tardó en cargar cada archivo.

![Registro de carga de blobs](monitoring-and-telemetry/_static/image23.png)

Es sólo un par de líneas adicionales de código para escribir cada vez que llame a un servicio, y ahora cada vez que alguien dice que se encontró con un problema, usted sabe exactamente cuál era el problema, si fue un error, o incluso si se estaba ejecutando lento. Puede identificar el origen del problema sin tener que remoto en un servidor o activar el registro después de que se produce el error y esperar volver a crearlo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inyección de dependencia en la aplicación Fix It

Es posible que se pregunte cómo el constructor del repositorio en el ejemplo mostrado anteriormente obtiene la implementación de la interfaz del registrador:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar la interfaz a la implementación, la aplicación usa la [inserción](http://en.wikipedia.org/wiki/Dependency_injection)de dependencias (DI) con [AutoFac](http://autofac.org/). DI le permite utilizar un objeto basado en una interfaz en muchos lugares a lo largo del código y solo tiene que especificar en un lugar la implementación que se usa cuando se crea una instancia de la interfaz. Esto hace que sea más fácil cambiar la implementación: por ejemplo, es posible que desee reemplazar el registrador System.Diagnostics por un registrador NLog. O para las pruebas automatizadas, es posible que desee sustituir una versión simulada del registrador.

La aplicación Fix It utiliza DI en todos los repositorios y todos los controladores. Los constructores de las clases de controlador obtienen una interfaz *ITaskRepository* de la misma manera que el repositorio obtiene una interfaz de registrador:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

La aplicación utiliza la biblioteca DI de AutoFac para proporcionar automáticamente *TaskRepository* y *Logger* instancias para estos constructores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Este código básicamente dice que en cualquier lugar que un constructor necesita una interfaz *ILogger,* pase una instancia de la clase *Logger* y, siempre que necesite una interfaz *IFixItTaskRepository,* pase una instancia de la clase *FixItTaskRepository.*

[AutoFac](http://autofac.org/) es uno de los muchos marcos de inserción de dependencias que puede usar. Otro popular es [Unity,](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx)que es recomendado y soportado por Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Compatibilidad de registro integrada en Azure

Azure admite los siguientes tipos de [registro para Web Apps en Azure App Service:](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)

- Seguimiento de System.Diagnostics (puede activar y desactivar y establecer niveles sobre la marcha sin reiniciar el sitio).
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).

Azure admite los siguientes tipos de [registro en Cloud Services:](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics)

- Seguimiento de System.Diagnostics.
- Contadores de rendimiento.
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).
- Supervisión de directorios personalizada.

La aplicación Fix It usa el seguimiento System.Diagnostics. Todo lo que necesita hacer para habilitar el registro System.Diagnostics en una aplicación web es cambiar un conmutador en el portal o llamar a la API de REST. En el portal, haga clic en la pestaña **Configuración** del sitio y desplácese hacia abajo para ver la sección **Diagnóstico de** aplicaciones. Puede activar o desactivar el inicio de sesión y seleccionar el nivel de registro que desee. Puede hacer que Azure escriba los registros en el sistema de archivos o en una cuenta de almacenamiento.

![Diagnóstico de aplicaciones y diagnóstico de sitio en la pestaña Configurar](monitoring-and-telemetry/_static/image24.png)

Después de habilitar el registro en Azure, puede ver los registros en la ventana Salida de Visual Studio a medida que se crean.

![Menú de registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menú de registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

También puede tener registros escritos en la cuenta de almacenamiento y verlos con cualquier herramienta que pueda tener acceso al servicio Azure Storage Table, como el **Explorador** de servidores en Visual Studio o el Explorador de [Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Registros en el Explorador de servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumen

Es muy sencillo implementar un sistema de telemetría listo para usar, iniciar sesión de instrumentos en su propio código y configurar el registro en Azure. Y cuando tenga problemas de producción, la combinación de un sistema de telemetría y registros personalizados le ayudará a resolver los problemas rápidamente antes de que se conviertan en problemas importantes para sus clientes.

En el [siguiente capítulo](transient-fault-handling.md) veremos cómo manejar errores transitorios para que no se conviertan en problemas de producción que tenga que investigar.

## <a name="resources"></a>Recursos

Para obtener más información, vea los recursos siguientes.

Documentación principalmente sobre telemetría:

- [Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Guía de instrumentación y telemetría, Guía de medición de servicio, Patrón de supervisión de punto de conexión de estado y Patrón de reconfiguración en tiempo de ejecución.
- [Penny Pinching in the Cloud: Enabling New Relic Performance Monitoring on Azure Websites](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services.](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx) White paper de Mark Simms y Michael Thomassy. Consulte la sección Telemetría y diagnóstico.
- [Desarrollo de próxima generación con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artículo de MSDN Magazine.

Documentación principalmente sobre el registro:

- Bloque de aplicación de [registro semántico (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta el caso de la tala semántica con SLAB.
- [Creación de registros estructurados y significativos con registro semántico](https://channel9.msdn.com/Events/Build/2013/3-336). (Vídeo) Julian Domínguez presenta el caso de la tala semántica con SLAB.
- [EF6 Registro SQL – Parte 1: Registro simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers muestra cómo registrar consultas ejecutadas por Entity Framework en EF 6.
- [Resistencia de conexión e interceptación](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)de comandos con Entity Framework en una aplicación ASP.NET MVC . En cuarto lugar en una serie de tutoriales de nueve partes, muestra cómo utilizar la característica de interceptación de comandos de EF 6 para registrar comandos SQL enviados a la base de datos por Entity Framework.
- Mejorar el registro mediante los atributos de información de llamadas de [C- 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Cómo registrar fácilmente el nombre del método de llamada sin codificarlo de forma rígida en literales o usar la reflexión para obtenerlo manualmente.

Documentación principalmente sobre solución de problemas:

- [Blog de &amp; depuración de problemas de Azure](https://blogs.msdn.com/b/kwill/).
- [AzureTools:](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)la utilidad de diagnóstico utilizada por el equipo de soporte técnico para desarrolladores de Azure . Presenta y proporciona un vínculo de descarga para una herramienta que se puede usar en una máquina virtual de Azure para descargar y ejecutar una amplia variedad de herramientas de diagnóstico y supervisión. Resulta útil cuando necesita diagnosticar un problema en una máquina virtual determinada.
- [Solucionar problemas](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)de una aplicación web en Azure App Service con Visual Studio . Un tutorial paso a paso para empezar a usar el seguimiento de System.Diagnostics y la depuración remota.

Videos:

- FailSafe: Creación de servicios en [la nube escalables y resistentes.](https://channel9.msdn.com/Series/FailSafe) Serie de nueve partes de Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias extraídas de la experiencia del Equipo de Asesoramiento al Cliente (CAT) de Microsoft con clientes reales. Los episodios 4 y 9 tienen que ver con la supervisión y la telemetría. El episodio 9 incluye una visión general de los servicios de supervisión De MetricsHub, AppDynamics, New Relic y PagerDuty.
- [Creación grande: lecciones aprendidas de los clientes de Azure - Parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla de diseñar para el fracaso e instrumentar todo. Similar a la serie Failsafe, pero entra en más detalles de cómo hacerlo.

Ejemplo de código:

- [Fundamentos del servicio en la nube en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo creada por el equipo de asesoramiento al cliente de Microsoft Azure. Muestra las prácticas de telemetría y registro, como se explica en los artículos siguientes. El ejemplo implementa el registro de aplicaciones mediante [NLog](http://nlog-project.org/). Para obtener documentación relacionada, consulte la serie de cuatro artículos wiki de [TechNet sobre telemetría y registro.](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon)

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md)
> [Siguiente](transient-fault-handling.md)
