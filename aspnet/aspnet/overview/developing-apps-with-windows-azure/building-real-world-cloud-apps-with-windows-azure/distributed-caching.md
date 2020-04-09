---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Almacenamiento en caché distribuido (creación de aplicaciones en la nube del mundo real con Azure) Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que puede...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 87a7516415895e761d1589fd459b93e5c15c0f85
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675662"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Almacenamiento en caché distribuido (creación de aplicaciones en la nube del mundo real con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Descargar Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Descargar E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Building Real World Cloud Apps with Azure** e-book se basa en una presentación desarrollada por Scott Guthrie. Explica 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, véase [el primer capítulo](introduction.md).

En el capítulo anterior se examinó el manejo de fallas transitorias y se mencionó el almacenamiento en caché como una estrategia de disyuntor. En este capítulo se proporciona más información sobre el almacenamiento en caché, incluido cuándo usarlo, patrones comunes para usarlo y cómo implementarlo en Azure.

## <a name="what-is-distributed-caching"></a>Qué es el almacenamiento en caché distribuido

Una memoria caché proporciona un alto rendimiento y acceso de baja latencia a los datos de aplicación a los que se accede con frecuencia, almacenando los datos en memoria. Para una aplicación en la nube, el tipo más útil de caché es la caché distribuida, lo que significa que los datos no se almacenan en la memoria del servidor web individual, sino en otros recursos de la nube, y los datos almacenados en caché se ponen a disposición de todos los servidores web de una aplicación (u otras máquinas virtuales en la nube que usa la aplicación).

![diagrama que muestra varios servidores web que acceden a los mismos servidores de caché](distributed-caching/_static/image1.png)

Cuando la aplicación se escala agregando o quitando servidores, o cuando los servidores se reemplazan debido a actualizaciones o errores, los datos almacenados en caché siguen siendo accesibles para todos los servidores que ejecutan la aplicación.

Al evitar el acceso a datos de alta latencia de un almacén de datos persistente, el almacenamiento en caché puede mejorar drásticamente la capacidad de respuesta de las aplicaciones. Por ejemplo, recuperar datos de la memoria caché es mucho más rápido que recuperarlos de una base de datos relacional.

Una ventaja secundaria del almacenamiento en caché es la reducción del tráfico al almacén de datos persistente, lo que puede resultar en costos más bajos cuando hay cargos de salida de datos para el almacén de datos persistente.

## <a name="when-to-use-distributed-caching"></a>Cuándo utilizar el almacenamiento en caché distribuido

El almacenamiento en caché funciona mejor para las cargas de trabajo de aplicaciones que realizan más lecturas que escritura de datos y cuando el modelo de datos admite la organización clave/valor que se usa para almacenar y recuperar datos en caché. También es más útil cuando los usuarios de la aplicación comparten una gran cantidad de datos comunes; por ejemplo, la memoria caché no proporcionaría tantas ventajas si cada usuario normalmente recupera datos únicos para ese usuario. Un ejemplo donde el almacenamiento en caché podría ser muy beneficioso es un catálogo de productos, porque los datos no cambian con frecuencia y todos los clientes están mirando los mismos datos.

La ventaja del almacenamiento en caché se vuelve cada vez más medible cuanto más se escala una aplicación, a medida que los límites de rendimiento y los retrasos de latencia del almacén de datos persistente se convierten en un límite más en el rendimiento general de la aplicación. Sin embargo, también puede implementar el almacenamiento en caché por otros motivos que no sean el rendimiento. Para los datos que no tienen que estar perfectamente actualizados cuando se muestran al usuario, el acceso a la memoria caché puede servir como un disyuntor para cuando el almacén de datos persistente no responde o no está disponible.

## <a name="popular-cache-population-strategies"></a>Estrategias populares de población de caché

Para poder recuperar datos de la memoria caché, primero debe almacenarlos allí. Hay varias estrategias para obtener los datos que necesita en una memoria caché:

- A petición / Caché a parte

    La aplicación intenta recuperar datos de la memoria caché y, cuando la memoria caché no tiene los datos (una "miss"), la aplicación almacena los datos en la memoria caché para que estén disponibles la próxima vez. La próxima vez que la aplicación intenta obtener los mismos datos, encuentra lo que está buscando en la memoria caché (un "hit"). Para evitar la obtención de datos almacenados en caché que han cambiado en la base de datos, invalide la memoria caché al realizar cambios en el almacén de datos.
- Inserción de datos en segundo plano

    Los servicios en segundo plano insertan datos en la memoria caché de forma regular y la aplicación siempre se extrae de la memoria caché. Este enfoque funciona muy bien con orígenes de datos de alta latencia que no requieren que siempre devuelva los datos más recientes.
- Disyuntor

    Normalmente, la aplicación se comunica directamente con el almacén de datos persistente, pero cuando el almacén de datos persistente tiene problemas de disponibilidad, la aplicación recupera datos de la memoria caché. Es posible que los datos se hayan puesto en caché mediante la memoria caché aparte o la estrategia de inserción de datos en segundo plano. Esta es una estrategia de manejo de fallas en lugar de una estrategia que mejora el rendimiento.

Para mantener los datos en la memoria caché actualizados, puede eliminar las entradas de caché relacionadas cuando la aplicación crea, actualiza o elimina datos. Si está bien que la aplicación a veces obtenga datos ligeramente desactualizados, puede confiar en un tiempo de expiración configurable para establecer un límite en la cantidad de datos de caché antiguos que pueden ser.

Puede configurar la expiración absoluta (cantidad de tiempo desde que se creó el elemento de caché) o la expiración deslizante (cantidad de tiempo desde la última vez que se tuvo acceso a un elemento de caché). La expiración absoluta se utiliza cuando se depende del mecanismo de expiración de caché para evitar que los datos se vuelvan demasiado obsoletos. En la aplicación Fix It, desalojaremos manualmente los elementos de caché obsoletos y usaremos la expiración deslizante para mantener los datos más actuales en caché. Independientemente de la directiva de expiración que elija, la memoria caché expulsará automáticamente los elementos más antiguos (Menos utilizados recientemente o LRU) cuando se alcance el límite de memoria de la memoria caché.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Ejemplo de código de caché aparte para la aplicación Fix It

En el siguiente código de ejemplo, comprobamos primero la memoria caché al recuperar una tarea Fix It. Si la tarea se encuentra en la memoria caché, la devolvemos; si no se encuentra, lo obtenemos de la base de datos y lo almacenamos en la memoria caché. Se resaltan los cambios que `FindTaskByIdAsync` realice para agregar almacenamiento en caché al método.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Al actualizar o eliminar una tarea Fix It, debe invalidar (eliminar) la tarea almacenada en caché. De lo contrario, los intentos futuros de leer esa tarea seguirán obteniendo los datos antiguos de la memoria caché.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Estos son ejemplos para ilustrar el código de almacenamiento en caché simple; El almacenamiento en caché no se ha implementado en el proyecto Fix It descargable.

## <a name="azure-caching-services"></a>Servicios de almacenamiento en caché de Azure

Azure ofrece los siguientes servicios de almacenamiento en caché: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) y [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). La caché de Azure Redis se basa en la popular caché de [redis](http://redis.io/) de código abierto y es la primera opción para la mayoría de los escenarios de almacenamiento en caché.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET estado de sesión mediante un proveedor de caché

Como se menciona en el [capítulo de prácticas recomendadas](web-development-best-practices.md)de desarrollo web, una práctica recomendada es evitar el uso del estado de sesión. Si la aplicación requiere el estado de sesión, el siguiente procedimiento recomendado es evitar el proveedor en memoria predeterminado porque eso no habilita el escalado horizontal (varias instancias del servidor web). El ASP.NET proveedor de estado de sesión de SQL ServerSQL Server permite que un sitio que se ejecuta en varios servidores web use el estado de sesión, pero incurre en un alto costo de latencia en comparación con un proveedor en memoria. La mejor solución si tiene que usar el estado de sesión es usar un proveedor de caché, como el proveedor de estado de [sesión para Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Resumen

Ha visto cómo la aplicación Fix It podría implementar el almacenamiento en caché para mejorar el tiempo de respuesta y la escalabilidad, y para permitir que la aplicación siga respondiendo para las operaciones de lectura cuando la base de datos no está disponible. En el [siguiente capítulo](queue-centric-work-pattern.md) mostraremos cómo mejorar aún más la escalabilidad y hacer que la aplicación siga respondiendo para las operaciones de escritura.

## <a name="resources"></a>Recursos

Para obtener más información acerca del almacenamiento en caché, consulte los siguientes recursos.

Documentación

- [Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx). Documentación oficial de MSDN sobre el almacenamiento en caché en Azure.
- [Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Guía de almacenamiento en caché y patrón de caché a parte de caché.
- [Failsafe: Guía para arquitecturas](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)de nube resistentes . White paper de Marc Mercuri, Ulrich Homann y Andrew Townhill. Consulte la sección sobre almacenamiento en caché.
- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services.](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx) Hora White paper de Mark Simms y Michael Thomassy. Consulte la sección sobre el almacenamiento en caché distribuido.
- [Almacenamiento en caché distribuido en la ruta de acceso a escalabilidad](https://msdn.microsoft.com/magazine/dd942840.aspx). Un artículo anterior (2009) de MSDN Magazine, pero una introducción claramente escrita al almacenamiento en caché distribuido en general; entra en más profundidad que las secciones de almacenamiento en caché de los documentos técnicos de FailSafe y Best Practices.

Vídeos

- FailSafe: Creación de servicios en [la nube escalables y resistentes.](https://channel9.msdn.com/Series/FailSafe) Serie de nueve partes de Ulrich Homann, Marc Mercuri y Mark Simms. Presenta una vista de 400 niveles de cómo diseñar aplicaciones en la nube. Esta serie se centra en la teoría y las razones por las que; para obtener más detalles sobre cómo hacerlo, consulte la serie Building Big de Mark Simms. Vea la discusión de almacenamiento en caché en el episodio 3 a partir de la 1:24:14.
- [Creación grande: lecciones aprendidas de los clientes de Azure - Parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discute el almacenamiento en caché distribuido a partir de las 46:00. Similar a la serie Failsafe, pero entra en más detalles de cómo hacerlo. La presentación se dio el 31 de octubre de 2012, por lo que no cubre el servicio de almacenamiento en caché de Web Apps en Azure App Service que se introdujo en 2013.

Código de ejemplo

- [Fundamentos del servicio en la nube en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que implementa el almacenamiento en caché distribuido. Consulte la entrada de blog que acompaña A los Fundamentos del servicio en la nube [– Conceptos básicos](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx)de almacenamiento en caché .

> [!div class="step-by-step"]
> [Anterior](transient-fault-handling.md)
> [Siguiente](queue-centric-work-pattern.md)
