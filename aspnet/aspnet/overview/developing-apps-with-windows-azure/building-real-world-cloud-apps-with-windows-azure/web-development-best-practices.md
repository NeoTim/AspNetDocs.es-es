---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Prácticas recomendadas de desarrollo web (creación de aplicaciones en la nube del mundo real con Azure) Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675644"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Prácticas recomendadas de desarrollo web (creación de aplicaciones en la nube del mundo real con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Descargar Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Descargar E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Building Real World Cloud Apps with Azure** e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, véase [el primer capítulo](introduction.md).

Los tres primeros patrones se trataba de establecer un proceso de desarrollo ágil; el resto se trata de arquitectura y código. Esta es una colección de mejores prácticas de desarrollo web:

- [Servidores web sin estado](#stateless) detrás de un equilibrador de carga inteligente.
- Evite el estado de [la sesión](#sessionstate) (o si no puede evitarlo, use la memoria caché distribuida en lugar de una base de datos).
- [Utilice una red CDN](#cdn) para almacenar en caché perimetral los activos de archivos estáticos (imágenes, scripts).
- Use la [compatibilidad asincrónica de .NET 4.5](#async) para evitar el bloqueo de llamadas.

Estas prácticas son válidas para todo el desarrollo web, no solo para las aplicaciones en la nube, sino que son especialmente importantes para las aplicaciones en la nube. Trabajan juntos para ayudarle a hacer un uso óptimo del escalado altamente flexible que ofrece el entorno de nube. Si no sigue estas prácticas, se encontrará con limitaciones cuando intente escalar la aplicación.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Nivel web sin estado detrás de un equilibrador de carga inteligente

*El nivel web sin estado* significa que no almacena ningún dato de aplicación en la memoria del servidor web o en el sistema de archivos. Mantener su nivel web sin estado le permite proporcionar una mejor experiencia al cliente y ahorrar dinero:

- Si el nivel web no tiene estado y se encuentra detrás de un equilibrador de carga, puede responder rápidamente a los cambios en el tráfico de aplicaciones agregando o quitando servidores dinámicamente. En el entorno de nube en el que solo paga por los recursos del servidor durante el tiempo que realmente los use, esa capacidad de responder a los cambios en la demanda puede traducirse en enormes ahorros.
- Un nivel web sin estado es arquitectónicamente mucho más sencillo para escalar horizontalmente la aplicación. Esto también le permite responder a las necesidades de escalado más rápidamente y gastar menos dinero en desarrollo y pruebas en el proceso.
- Los servidores en la nube, como los servidores locales, deben aplicarse parches y reiniciarse ocasionalmente; y si el nivel web no tiene estado, el tráfico de reenrutamiento cuando un servidor deja de estar temporalmente no provocará errores ni un comportamiento inesperado.

La mayoría de las aplicaciones del mundo real necesitan almacenar el estado de una sesión web; el punto principal aquí es no almacenarlo en el servidor web. Puede almacenar el estado de otras maneras, como en el cliente en las cookies o fuera del servidor fuera del proceso en ASP.NET estado de sesión mediante un proveedor de caché. Puede almacenar archivos en El almacenamiento de [blobs](unstructured-blob-storage.md) de Windows Azure en lugar del sistema de archivos local.

Como ejemplo de lo fácil que es escalar una aplicación en sitios web de Windows Azure si el nivel web no tiene estado, consulte la pestaña **Escala** para un sitio web de Windows Azure en el portal de administración:

![Pestaña Escala](web-development-best-practices/_static/image1.png)

Si desea agregar servidores web, puede arrastrar el control deslizante de recuento de instancias hacia la derecha. Establézalo en 5 y haz clic en **Guardar**, y en cuestión de segundos tienes 5 servidores web en Windows Azure que controlan el tráfico de tu sitio web.

![Cinco instancias](web-development-best-practices/_static/image2.png)

Puede establecer fácilmente la cuenta de instancias en 3 o volver a 1. Cuando se escala hacia atrás, se empieza a ahorrar dinero inmediatamente porque Windows Azure cobra por minuto, no por hora.

También puede indicar a Windows Azure que aumente o disminuya automáticamente el número de servidores web en función del uso de CPU. En el ejemplo siguiente, cuando el uso de la CPU es inferior al 60%, el número de servidores web disminuirá a un mínimo de 2, y si el uso de la CPU supera el 80%, el número de servidores web se incrementará hasta un máximo de 4.

![Escalar por uso de CPU](web-development-best-practices/_static/image3.png)

¿O qué pasa si sabes que tu sitio solo estará ocupado durante las horas de trabajo? Puede indicar a Windows Azure que ejecute varios servidores durante el día y que disminuya a un único servidor por las noches, las noches y los fines de semana. La siguiente serie de capturas de pantalla muestra cómo configurar el sitio web para ejecutar un servidor en horas libres y 4 servidores durante las horas de trabajo de 8 a. m. a 5 p. m.

![Escala por horario](web-development-best-practices/_static/image4.png)

![Establecer horarios](web-development-best-practices/_static/image5.png)

![Horario diurno](web-development-best-practices/_static/image6.png)

![Horario de la noche de la semana](web-development-best-practices/_static/image7.png)

![Horario de fin de semana](web-development-best-practices/_static/image8.png)

Y, por supuesto, todo esto se puede hacer en scripts, así como en el portal.

La capacidad de la aplicación para escalar horizontalmente es casi ilimitada en Windows Azure, siempre y cuando evite impedimentos para agregar o quitar dinámicamente máquinas virtuales de servidor, manteniendo el nivel web sin estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitar el estado de la sesión

No suele ser práctico evitar almacenar algún tipo de estado para una sesión de usuario en una aplicación de nube real, pero algunos enfoques afectan al rendimiento y a la escalabilidad más que otros  Si tiene que almacenar el estado, la mejor solución es que la cantidad sea reducida y que se almacene en cookies. Si esto no es posible, la siguiente mejor solución es usar ASP.NET estado de sesión con un proveedor para la [memoria caché distribuida en memoria.](distributed-caching.md#sessionstate) La peor solución desde el punto de vista del rendimiento y la escalabilidad es usar un proveedor de estado de sesión con copia de seguridad de base de datos.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usar una red CDN para almacenar en caché activos de archivos estáticos

CDN es un acrónimo de Content Delivery Network. Proporciona activos de archivos estáticos, como imágenes y archivos de script, a un proveedor de CDN y el proveedor almacena en caché estos archivos en centros de datos de todo el mundo para que, dondequiera que las personas accedan a la aplicación, obtengan una respuesta relativamente rápida y una latencia baja para los activos almacenados en caché. Esto acelera el tiempo de carga total del sitio y reduce la carga en los servidores web. Las CDN son especialmente importantes si se llega a un público ampliamente distribuido geográficamente.

Windows Azure tiene una red CDN y puede usar otras CDN en una aplicación que se ejecuta en Windows Azure o en cualquier entorno de hospedaje web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Utilice la compatibilidad asincrónica de .NET 4.5 para evitar el bloqueo de llamadas

.NET 4.5 mejoró los lenguajes de programación de C y VB para que sea mucho más sencillo controlar las tareas de forma asincrónica. La ventaja de la programación asincrónica no es solo para situaciones de procesamiento en paralelo, como cuando desea iniciar varias llamadas de servicio web simultáneamente. También permite que su servidor web funcione de manera más eficiente y confiable en condiciones de alta carga. Un servidor web solo tiene un número limitado de subprocesos disponibles y, en condiciones de carga elevada, cuando todos los subprocesos están en uso, las solicitudes entrantes tienen que esperar hasta que se liberen los subprocesos. Si el código de la aplicación no controla tareas como consultas de base de datos y llamadas de servicio web de forma asincrónica, muchos subprocesos están innecesariamente vinculados mientras el servidor está esperando una respuesta de E/S. Esto limita la cantidad de tráfico que el servidor puede manejar en condiciones de alta carga. Con la programación asincrónica, los subprocesos que están esperando a que un servicio web o una base de datos devuelvan datos se liberan para atender nuevas solicitudes hasta que se reciban los datos. En un servidor web ocupado, cientos o miles de solicitudes se pueden procesar con prontitud, lo que de otro modo estaría esperando a que los subprocesos se liberen.

Como vio anteriormente, es tan fácil reducir el número de servidores web que manejan su sitio web como aumentarlos. Por lo tanto, si un servidor puede lograr un mayor rendimiento, no necesita tantos de ellos y puede reducir los costos porque necesita menos servidores para un volumen de tráfico determinado de lo que de otro modo lo haría.

La compatibilidad con el modelo de programación asincrónica de .NET 4.5 se incluye en ASP.NET 4.5 para formularios Web Forms, MVC y Web API; en Entity Framework 6 y en la API de [almacenamiento de Windows Azure](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Soporte asincrónico en ASP.NET 4.5

En ASP.NET 4.5, la compatibilidad con la programación asincrónica se ha agregado no solo al lenguaje, sino también a los marcos MVC, web Forms y Web API. Por ejemplo, un método de acción de controlador ASP.NET MVC recibe datos de una solicitud web y pasa los datos a una vista que, a continuación, crea el html que se enviará al explorador. Con frecuencia, el método de acción necesita obtener datos de una base de datos o un servicio web para mostrarlos en una página web o para guardar los datos introducidos en una página web. En esos escenarios es fácil hacer que el método de acción sea asincrónico: en lugar de devolver un objeto *ActionResult,* se devuelve *&lt;Task ActionResult&gt; * y se marca el método con la palabra clave *async.* Dentro del método, cuando una línea de código inicia una operación que implica tiempo de espera, se marca con la palabra clave await.

Este es un método de acción simple que llama a un método de repositorio para una consulta de base de datos:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Y aquí está el mismo método que controla la llamada de base de datos de forma asincrónica:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

En las portadas, el compilador genera el código asincrónico adecuado. Cuando la aplicación realiza `FindTaskByIdAsync`la llamada `FindTask` a , ASP.NET realiza la solicitud y, a continuación, desenreda el subproceso de trabajo y lo hace disponible para procesar otra solicitud. Cuando `FindTask` se realiza la solicitud, se reinicia un subproceso para continuar procesando el código que viene después de esa llamada. Durante el intermedio `FindTask` entre el momento en que se inicia la solicitud y cuando se devuelven los datos, tiene un subproceso disponible para realizar un trabajo útil que, de lo contrario, estaría vinculado a la espera de la respuesta.

Hay cierta sobrecarga para el código asincrónico, pero en condiciones de baja carga, esa sobrecarga es insignificante, mientras que en condiciones de alta carga puede procesar solicitudes que, de lo contrario, se retendrían a la espera de subprocesos disponibles.

Ha sido posible hacer este tipo de programación asincrónica desde ASP.NET 1.1, pero era difícil de escribir, propenso a errores y difícil de depurar. Ahora que hemos simplificado la codificación en ASP.NET 4.5, no hay razón para no hacerlo más.

### <a name="async-support-in-entity-framework-6"></a>Compatibilidad asincrónica en Entity Framework 6

Como parte del soporte asincrónico en 4.5, enviamos soporte asincrónico para llamadas de servicio web, sockets y E/S de sistema de archivos, pero el patrón más común para las aplicaciones web es golpear una base de datos, y nuestras bibliotecas de datos no admiten async. Ahora Entity Framework 6 agrega compatibilidad asincrónica para el acceso a la base de datos.

En Entity Framework 6 todos los métodos que hacen que una consulta o comando se envíe a la base de datos tienen versiones asincrónicas. En el ejemplo aquí se muestra la versión asincrónica del método *Find.*

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Y este soporte asincrónico no solo funciona para inserciones, eliminaciones, actualizaciones y búsquedas simples, sino que también funciona con consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Hay una `Async` versión `ToList` del método porque en este código ese es el método que hace que se envíe una consulta a la base de datos. Los `Where` `OrderByDescending` métodos y solo configuran `ToListAsync` la consulta, mientras que `result` el método ejecuta la consulta y almacena la respuesta en la variable.

## <a name="summary"></a>Resumen

Puede implementar las prácticas recomendadas de desarrollo web descritas aquí en cualquier marco de programación web y cualquier entorno de nube, pero tenemos herramientas en ASP.NET y Windows Azure para que sea fácil. Si sigue estos patrones, puede escalar horizontalmente fácilmente su nivel web y minimizará sus gastos porque cada servidor podrá manejar más tráfico.

En el [siguiente capítulo](single-sign-on.md) se examina cómo la nube habilita escenarios de inicio de sesión único.

## <a name="resources"></a>Recursos

Para obtener más información, consulte los siguientes recursos.

Servidores web sin estado:

- [Microsoft Patterns and Practices - Guía de escalado automático](https://msdn.microsoft.com/library/dn589774.aspx).
- Deshabilitar la afinidad de [instancias de ARR en sitios web](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)de Windows Azure . La entrada de blog de Erez Benari explica la afinidad de sesiones en los sitios web de Windows Azure.

Cdn:

- FailSafe: Creación de servicios en [la nube escalables y resistentes.](https://channel9.msdn.com/Series/FailSafe) Serie de vídeo de nueve partes de Ulrich Homann, Marc Mercuri y Mark Simms. Vea la discusión de la CDN en el episodio 3 a partir de la 1:34:00.
- [Patrón de hospedaje de contenido estático de Microsoft Patterns and Practices](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN Reseñas](http://www.cdnreviews.com/). Descripción general de muchas CDN.

Programación asincrónica:

- [Uso de métodos asincrónicos en ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programación asincrónica con Async y Await (C- y Visual Basic).](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) Informe técnico de MSDN que explica los fundamentos de la programación asincrónica, cómo funciona en ASP.NET 4.5 y cómo escribir código para implementarlo.
- [Consulta asincrónica de Entity Framework y Guardar](https://msdn.microsoft.com/data/jj819165)
- [Cómo crear ASP.NET aplicaciones web mediante Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentación en video por Rowan Miller. Incluye una demostración gráfica de cómo la programación asincrónica puede facilitar aumentos drásticos en el rendimiento del servidor web en condiciones de alta carga.
- FailSafe: Creación de servicios en [la nube escalables y resistentes.](https://channel9.msdn.com/Series/FailSafe) Serie de vídeo de nueve partes de Ulrich Homann, Marc Mercuri y Mark Simms. Para ver discusiones sobre el impacto de la programación asincrónica en la escalabilidad, consulte el episodio 4 y el episodio 8.
- [La magia de usar métodos asincrónicos en ASP.NET 4.5 más un importante gotcha](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Entrada de blog de Scott Hanselman, principalmente sobre el uso de async en aplicaciones de formularios Web Forms ASP.NET.

Para conocer las prácticas recomendadas de desarrollo web adicionales, consulte los siguientes recursos:

- La aplicación de [ejemplo Fix It - Prácticas recomendadas](the-fix-it-sample-application.md#bestpractices). El apéndice de este libro electrónico enumera una serie de prácticas recomendadas que se implementaron en la aplicación Fix It.
- [Lista de verificación para desarrolladores web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Anterior](continuous-integration-and-continuous-delivery.md)
> [Siguiente](single-sign-on.md)
