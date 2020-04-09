---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Novedades de ASP.NET 4.5 y Visual Studio 2012 Microsoft Docs
author: rick-anderson
description: Este documento describe las nuevas características y mejoras que se están introduciendo en ASP.NET 4.5. También describe las mejoras que se están realizando para el desarrollo web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675890"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novedades de ASP.NET 4.5 y Visual Studio 2012

> Este documento describe las nuevas características y mejoras que se están introduciendo en ASP.NET 4.5. También describe las mejoras que se están realizando para el desarrollo web en Visual Studio 2012. Este documento fue publicado originalmente el 29 de febrero de 2012.

- [ASP.NET Core Runtime and Framework](#_Toc318097372)

    - [Lectura y escritura asincrónica de solicitudes y respuestas HTTP](#_Toc318097373)
    - [Mejoras en el control de HttpRequest](#_Toc318097374)
    - [Vaciar una respuesta de forma asincrónica](#_Toc318097375)
    - [Soporte *para* módulos y controladores asincrónicos basados en *tareas*y espera](#_Toc318097376)
    - [Módulos HTTP asincrónicos](#_Toc318097377)
    - [Controladores HTTP asincrónicos](#_Toc318097378)
    - [Nuevas características de validación de solicitudes de ASP.NET](#_Toc318097379)
    - [Validación de solicitud diferida ("perezosa")](#_Toc318097380)
    - [Soporte para solicitudes no validadas](#_Toc318097381)
    - [Biblioteca AntiXSS](#_Toc318097382)
    - [Compatibilidad con WebSockets Protocol](#_Toc318097383)
    - [Agrupación y minificación](#_Toc318097384)
    - [Mejoras de rendimiento para el alojamiento web](#_Toc_perf)

        - [Factores clave de rendimiento](#_Toc_perf_1)
        - [Requisitos para nuevas características de rendimiento](#_Toc_perf_2)
        - [Compartir asambleas comunes](#_Toc_perf_3)
        - [Uso de la compilación JIT multinúcleo para un inicio más rápido](#_Toc_perf_4)
        - [Ajuste de la recolección de elementos no utilizados para optimizar la memoria](#_Toc_perf_5)
        - [Captura previa para aplicaciones web](#_Toc_perf_6)
- [Formularios Web Forms ASP.NET](#_Toc318097385)

    - [Controles de datos fuertemente tipados](#_Toc318097386)
    - [Enlace de modelos](#_Toc318097387)

        - [Selección de datos](#_Toc318097388)
        - [Proveedores de valor](#_Toc318097389)
        - [Filtrado por valores de un control](#_Toc318097390)
    - [Expresiones de enlace de datos codificadas HTML](#_Toc318097391)
    - [Validación discreta](#_Toc318097392)
    - [Actualizaciones HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Candidato de lanzamiento de Visual Studio 2012](#_Toc318097396)

    - [Uso compartido de proyectos entre Visual Studio 2010 y Visual Studio 2012 Release Candidate (compatibilidad de proyectos)](#project-compatibility)
    - [Cambios de configuración en ASP.NET 4.5 Plantillas de sitio web](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Compatibilidad nativa en IIS 7 para enrutamiento de ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor de HTML](#_Toc318097397)

        - [Tareas inteligentes](#_Toc318097398)
        - [Soporte WAI-ARIA](#_Toc318097399)
        - [Nuevos fragmentos de código HTML5](#_Toc318097400)
        - [Extraer al control del usuario](#_Toc318097401)
        - [IntelliSense para pepitas de código en atributos](#_Toc318097402)
        - [Cambio automático de nombre de la etiqueta coincidente al cambiar el nombre de una etiqueta de apertura o cierre](#_Toc318097403)
        - [Generación de controladores de eventos](#_Toc318097404)
        - [Sangría inteligente](#_Toc318097405)
        - [Reducción automática de la finalización de la declaración](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Esquema de código](#_Toc318097408)
        - [Coincidencia de llaves](#_Toc318097409)
        - [Vaya a Definición](#_Toc318097410)
        - [Compatibilidad con ECMAScript5](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [Sobrecargas de firma VSDOC](#_Toc318097413)
        - [Referencias implícitas](#_Toc318097414)
    - [Editor CSS](#_Toc318097415)

        - [Reducción automática de la finalización de la declaración](#_Toc318097416)
        - [Sangría jerárquica.](#_Toc318097417)
        - [Soporte de hacks CSS](#_Toc318097418)
        - [Esquemas específicos del proveedor (-moz-,-webkit)](#_Toc318097419)
        - [Comentarios y descommenting de apoyo](#_Toc318097420)
        - [Color picker](#_Toc318097421)
        - [Fragmentos de código](#_Toc318097422)
        - [Regiones personalizadas](#_Toc318097423)
    - [Inspector de página](#_Toc318097424)
    - [Publicación](#_Toc318097425)

        - [Perfiles de publicación](#_Toc318097426)
        - [ASP.NET precompilación y fusión](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Renuncia](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core Runtime and Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Lectura y escritura asincrónica de solicitudes y respuestas HTTP

ASP.NET 4 introdujo la capacidad de leer una entidad de solicitud HTTP como una secuencia mediante el método *HttpRequest.GetBufferlessInputStream.* Este método proporcionaba acceso de streaming a la entidad de solicitud. Sin embargo, se ejecutó sincrónicamente, lo que ató un subproceso durante la duración de una solicitud.

ASP.NET 4.5 admite la capacidad de leer secuencias de forma asincrónica en una entidad de solicitud HTTP y la capacidad de vaciar de forma asincrónica. ASP.NET 4.5 también le ofrece la capacidad de almacenar en búfer dos veces una entidad de solicitud HTTP, lo que proporciona una integración más sencilla con controladores HTTP de nivel inferior, como controladores de página .aspx y controladores MVC ASP.NET.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Mejoras en el control de HttpRequest

La referencia Stream devuelta por ASP.NET 4.5 de *HttpRequest.GetBufferlessInputStream* admite métodos de lectura sincrónicos y asincrónicos. El objeto *Stream* devuelto desde *GetBufferlessInputStream* ahora implementa los métodos BeginRead y EndRead. Los métodos *Stream* asincrónicos permiten leer de forma asincrónica la entidad de solicitud en fragmentos, mientras que ASP.NET libera el subproceso actual entre cada iteración de un bucle de lectura asincrónico.

ASP.NET 4.5 también ha agregado un método complementario para leer la entidad de solicitud de una manera almacenada en búfer: *HttpRequest.GetBufferedInputStream*. Esta nueva sobrecarga funciona como *GetBufferlessInputStream,* que admite lecturas sincrónicas y asincrónicas. Sin embargo, a medida que se lee, *GetBufferedInputStream* también copia los bytes de entidad en ASP.NET búferes internos para que los módulos y controladores de nivel inferior puedan seguir teniendo acceso a la entidad de solicitud. Por ejemplo, si algún código ascendente de la canalización ya ha leído la entidad de solicitud mediante *GetBufferedInputStream*, puede seguir utilizando *HttpRequest.Form* o *HttpRequest.Files*. Esto le permite realizar el procesamiento asincrónico en una solicitud (por ejemplo, transmitir una carga de archivos de gran tamaño en una base de datos), pero seguir ejecutando páginas .aspx y controladores de ASP.NET MVC después.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Vaciar una respuesta de forma asincrónica

El envío de respuestas a un cliente HTTP puede tardar mucho tiempo cuando el cliente está lejos o tiene una conexión de ancho de banda bajo. Normalmente, ASP.NET almacena en búfer los bytes de respuesta a medida que los crea una aplicación. ASP.NET, a continuación, realiza una sola operación de envío de los búferes acumulados al final del procesamiento de solicitudes.

Si la respuesta almacenada en búfer es grande (por ejemplo, transmitir un archivo grande a un cliente), debe llamar periódicamente a *HttpResponse.Flush* para enviar la salida almacenada en búfer al cliente y mantener el uso de memoria bajo control. Sin embargo, dado que *Flush* es una llamada sincrónica, llamar iterativamente *a Flush* todavía consume un subproceso durante la duración de las solicitudes potencialmente de ejecución prolongada.

ASP.NET 4.5 agrega compatibilidad para realizar vaciados de forma asincrónica mediante los métodos *BeginFlush* y *EndFlush* de la clase *HttpResponse.* Con estos métodos, puede crear módulos asincrónicos y controladores asincrónicos que envían datos de forma incremental a un cliente sin ataduras de subprocesos del sistema operativo. Entre *BeginFlush* y *EndFlush* llamadas, ASP.NET libera el subproceso actual. Esto reduce sustancialmente el número total de subprocesos activos que se necesitan para admitir descargas HTTP de larga ejecución.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Soporte para *await* y *Task* - Módulos y controladores asincrónicos basados

.NET Framework 4 introdujo un concepto de programación asincrónica denominado *tarea.* Las tareas se representan mediante el tipo *Task* y los tipos relacionados en el espacio de nombres *System.Threading.Tasks.* .NET Framework 4.5 se basa en esto con mejoras del compilador que simplifican el trabajo con objetos *Task.* En .NET Framework 4.5, los compiladores admiten dos nuevas palabras clave: *await* y *async*. La palabra clave *await* es una abreviatura sintáctica para indicar que un fragmento de código debe esperar de forma asincrónica en algún otro fragmento de código. La palabra clave *async* representa una sugerencia que puede usar para marcar métodos como métodos asincrónicos basados en tareas.

La combinación de *await*, *async*y el objeto *Task* facilita mucho la escritura de código asincrónico en .NET 4.5. ASP.NET 4.5 admite estas simplificaciones con nuevas API que le permiten escribir módulos HTTP asincrónicos y controladores HTTP asincrónicos mediante las nuevas mejoras del compilador.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Módulos HTTP asincrónicos

Supongamos que desea realizar un trabajo asincrónico dentro de un método que devuelve un *Task* objeto. En el ejemplo de código siguiente se define un método asincrónico que realiza una llamada asincrónica para descargar la página principal de Microsoft. Observe el uso de la palabra clave *async* en la firma del método y la llamada *await* a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Eso es todo lo que tiene que escribir: .NET Framework controlará automáticamente el desenredado de la pila de llamadas mientras espera a que se complete la descarga, así como restaurar automáticamente la pila de llamadas después de que se haya realizado la descarga.

Ahora suponga mosquese que desea utilizar este método asincrónico en un módulo ASP.NET HTTP asincrónico. ASP.NET 4.5 incluye un método auxiliar (*EventHandlerTaskAsyncHelper*) y un nuevo tipo de delegado (*TaskEventHandler*) que puede usar para integrar métodos asincrónicos basados en tareas con el modelo de programación asincrónica anterior expuesto por la canalización HTTP ASP.NET. Este ejemplo muestra cómo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Controladores HTTP asincrónicos

El enfoque tradicional para escribir controladores asincrónicos en ASP.NET es implementar la interfaz *IHttpAsyncHandler.* ASP.NET 4.5 presenta el tipo base asincrónico *HttpTaskAsyncHandler* del que puede derivar, lo que facilita mucho la escritura de controladores asincrónicos.

El tipo *HttpTaskAsyncHandler* es abstracto y requiere que reemplace el método *ProcessRequestAsync.* Internamente ASP.NET se encarga de integrar la firma de retorno (un objeto *Task)* de *ProcessRequestAsync* con el modelo de programación asincrónica más antiguo utilizado por la canalización de ASP.NET.

En el ejemplo siguiente se muestra cómo puede usar *Task* y *await* como parte de la implementación de un controlador HTTP asincrónico:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuevas características de validación de solicitudes de ASP.NET

De forma predeterminada, ASP.NET realiza la validación de solicitudes: examina las solicitudes para buscar marcado o script en campos, encabezados, cookies, etc. Si se detecta alguno, ASP.NET produce una excepción. Esto actúa como una primera línea de defensa contra posibles ataques de scripting entre sitios.

ASP.NET 4.5 facilita la lectura selectiva de los datos de solicitud no validados. ASP.NET 4.5 también integra la popular biblioteca AntiXSS, que anteriormente era una biblioteca externa.

Los desarrolladores han pedido con frecuencia la capacidad de desactivar selectivamente la validación de solicitudes para sus aplicaciones. Por ejemplo, si la aplicación es software de foro, es posible que desee permitir a los usuarios enviar mensajes y comentarios del foro con formato HTML, pero asegúrese de que la validación de solicitudes está comprobando todo lo demás.

ASP.NET 4.5 presenta dos características que facilitan el trabajo selectivo con entradas no validadas: validación de solicitudes diferidas ("perezosas") y acceso a datos de solicitudes no validados.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Validación de solicitud diferida ("perezosa")

En ASP.NET 4.5, de forma predeterminada, todos los datos de solicitud están sujetos a validación de solicitudes. Sin embargo, puede configurar la aplicación para aplazar la validación de solicitudes hasta que realmente tenga acceso a los datos de solicitud. (Esto se conoce a veces como validación de solicitud diferida, basada en términos como carga diferida para determinados escenarios de datos.) Puede configurar la aplicación para que utilice la validación diferida en el archivo Web.config estableciendo el atributo *requestValidationMode* en 4.5 en el elemento *httpRUntime,* como en el ejemplo siguiente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Cuando el modo de validación de solicitudes se establece en 4.5, la validación de solicitudes se desencadena solo para un valor de solicitud específico y solo cuando el código tiene acceso a ese valor. Por ejemplo, si el código obtiene el valor\_de Request.Form["forum post"], la validación de la solicitud se invoca solo para ese elemento de la colección de formularios. Ninguno de los demás elementos de la colección *Form* se valida. En versiones anteriores de ASP.NET, se desencadenaba la validación de solicitudes para toda la colección de solicitudes cuando se tenía acceso a cualquier elemento de la colección. El nuevo comportamiento facilita que los diferentes componentes de la aplicación examine diferentes elementos de datos de solicitud sin desencadenar la validación de solicitudes en otras partes.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Soporte para solicitudes no validadas

La validación de solicitudes diferidas por sí sola no resuelve el problema de omitir selectivamente la validación de solicitudes. La llamada a Request.Form["forum\_post"] sigue desencadenando la validación de solicitudes para ese valor de solicitud específico. Sin embargo, es posible que desee tener acceso a este campo sin desencadenar la validación porque desea permitir el marcado en ese campo.

Para permitir esto, ASP.NET 4.5 ahora admite el acceso no validado a los datos de solicitud. ASP.NET 4.5 incluye una nueva propiedad de colección *Unvalidated* en la clase *HttpRequest.* Esta colección proporciona acceso a todos los valores comunes de los datos de solicitud, como *Form*, *QueryString*, *Cookies*y *Url*.

Mediante el ejemplo de foro, para poder leer los datos de solicitud no validados, primero debe configurar la aplicación para que use el nuevo modo de validación de solicitudes:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

A continuación, puede utilizar la propiedad *HttpRequest.Unvalidated* para leer el valor del formulario no validado:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Seguridad - ¡Utilice datos de *solicitudes no validados con cuidado!* ASP.NET 4.5 agregó las propiedades y colecciones de solicitudes no validadas para facilitar el acceso a datos de solicitud no validados muy específicos. Sin embargo, debe realizar una validación personalizada en los datos de solicitud sin procesar para asegurarse de que el texto peligroso no se representa a los usuarios.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteca AntiXSS

Debido a la popularidad de la biblioteca AntiXSS de Microsoft, ASP.NET 4.5 ahora incorpora rutinas de codificación principales de la versión 4.0 de esa biblioteca.

Las rutinas de codificación se implementan mediante el tipo *AntiXssEncoder* en el nuevo espacio de nombres *System.Web.Security.AntiXss.* Puede usar el tipo *AntiXssEncoder* directamente llamando a cualquiera de los métodos de codificación estática que se implementan en el tipo. Sin embargo, el enfoque más fácil para usar las nuevas rutinas anti-XSS es configurar una aplicación ASP.NET para usar la clase *AntiXssEncoder* de forma predeterminada. Para ello, agregue el siguiente atributo al archivo Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Cuando el atributo *encoderType* se establece para usar el tipo *AntiXssEncoder,* toda la codificación de salida de ASP.NET utiliza automáticamente las nuevas rutinas de codificación.

Estas son las partes de la biblioteca externa de AntiXSS que se han incorporado a ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*y *HtmlAttributeEncode*
- *XmlAttributeEncode* y *XmlEncode*
- *UrlEncode* y *UrlPathEncode* (nuevo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Compatibilidad con WebSockets Protocol

El protocolo WebSockets es un protocolo de red basado en estándares que define cómo establecer comunicaciones bidireccionales seguras en tiempo real entre un cliente y un servidor a través de HTTP. Microsoft ha trabajado con los organismos de estándares IETF y W3C para ayudar a definir el protocolo. El protocolo WebSockets es compatible con cualquier cliente (no solo navegadores), con Microsoft invirtiendo recursos sustanciales compatibles con el protocolo WebSockets en sistemas operativos cliente y móvil.

El protocolo WebSockets facilita mucho la creación de transferencias de datos de larga ejecución entre un cliente y un servidor. Por ejemplo, escribir una aplicación de chat es mucho más fácil porque puede establecer una verdadera conexión de larga ejecución entre un cliente y un servidor. No es necesario recurrir a soluciones alternativas como sondeo periódico o sondeo largo HTTP para simular el comportamiento de un socket.

ASP.NET 4.5 e IIS 8 incluyen compatibilidad con WebSockets de bajo nivel, lo que permite a los desarrolladores de ASP.NET usar API administradas para leer y escribir datos binarios y de cadena de forma asincrónica en un objeto WebSockets. Para ASP.NET 4.5, hay un nuevo espacio de nombres *System.Web.WebSockets* que contiene tipos para trabajar con el protocolo WebSockets.

Un cliente de explorador establece una conexión WebSockets mediante la creación de un objeto *DOM WebSocket* que apunta a una dirección URL en una aplicación ASP.NET, como en el ejemplo siguiente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Puede crear puntos de conexión de WebSockets en ASP.NET mediante cualquier tipo de módulo o controlador. En el ejemplo anterior, se utilizó un archivo .ashx, porque los archivos .ashx son una forma rápida de crear un controlador.

Según el protocolo WebSockets, una aplicación ASP.NET acepta la solicitud WebSockets de un cliente indicando que la solicitud debe actualizarse de una solicitud HTTP GET a una solicitud de WebSockets. Este es un ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

El *AcceptWebSocketRequest* método acepta un delegado de función porque ASP.NET desenreda la solicitud HTTP actual y, a continuación, transfiere el control al delegado de función. Conceptualmente, este enfoque es similar a cómo se usa *System.Threading.Thread*, donde se define un delegado de inicio de subproceso en el que se realiza el trabajo en segundo plano.

Después de que ASP.NET y el cliente hayan completado correctamente un protocolo de enlace de WebSockets, ASP.NET llama al delegado y la aplicación WebSockets comienza a ejecutarse. En el ejemplo de código siguiente se muestra una aplicación de eco simple que usa la compatibilidad integrada con WebSockets en ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

La compatibilidad de .NET 4.5 para la palabra clave *await* y las operaciones asincrónicas basadas en tareas es un ajuste natural para escribir aplicaciones WebSockets. En el ejemplo de código se muestra que una solicitud de WebSockets se ejecuta de forma completamente asincrónica dentro de ASP.NET. La aplicación espera de forma asincrónica a que se envíe un mensaje desde un cliente llamando al *socket await. ReceiveAsync*. De forma similar, puede enviar un mensaje asincrónico a un cliente llamando al *socket await. SendAsync*.

En el explorador, una aplicación recibe mensajes WebSockets a través de una función *onmessage.* Para enviar un mensaje desde un explorador, llame al método *send* del tipo *WebSocket* DOM, como se muestra en este ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

En el futuro, podríamos lanzar actualizaciones de esta funcionalidad que abstraigan parte de la codificación de bajo nivel que se requiere en esta versión para las aplicaciones WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Unión y minificación

La agrupación le permite combinar archivos JavaScript y CSS individuales en un paquete que se puede tratar como un solo archivo. La minificación condensa los archivos JavaScript y CSS eliminando espacios en blanco y otros caracteres que no son necesarios. Estas características funcionan con formularios Web Forms, ASP.NET MVC y páginas web.

Los paquetes se crean mediante la clase Bundle o una de sus clases secundarias, ScriptBundle y StyleBundle. Después de configurar una instancia de un paquete, el paquete se pone a disposición de las solicitudes entrantes simplemente agregándolo a una instancia global de BundleCollection. En las plantillas predeterminadas, la configuración del paquete se realiza en un archivo BundleConfig. Esta configuración predeterminada crea paquetes para todos los scripts principales y archivos css utilizados por las plantillas.

Se hace referencia a los paquetes desde dentro de las vistas mediante uno de los dos métodos auxiliares posibles. Para admitir la representación de marcado diferente para un paquete cuando está en modo de depuración frente a versión, las clases ScriptBundle y StyleBundle tienen el método auxiliar, Render. Cuando está en modo de depuración, Render generará marcado para cada recurso del paquete. Cuando está en modo de versión, Render generará un único elemento de marcado para todo el paquete. Alternar entre el modo de depuración y el modo de versión se puede lograr modificando el atributo debug del elemento de compilación en web.config como se muestra a continuación:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Además, habilitar o deshabilitar la optimización se puede establecer directamente a través de la BundleTable.EnableOptimizations propiedad.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Cuando se agrupan los archivos, primero se ordenan alfabéticamente (la forma en que se muestran en el **Explorador**de soluciones ). A continuación, se organizan para que las bibliotecas conocidas y sus extensiones personalizadas (como jQuery, MooTools y Dojo) se carguen primero. Por ejemplo, el orden final para la agrupación de la carpeta Scripts como se muestra arriba será:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Los archivos CSS también se ordenan alfabéticamente y luego se reorganizan para que reset.css y normalize.css vengan antes que cualquier otro archivo. La clasificación final de la agrupación de la carpeta Estilos mostrada anteriormente será la siguiente:

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Mejoras de rendimiento para el alojamiento web

.NET Framework 4.5 y Windows 8 presentan características que pueden ayudarle a lograr un aumento significativo del rendimiento de las cargas de trabajo de servidor web. Esto incluye una reducción (hasta un 35%) tanto en el tiempo de inicio como en la huella de memoria de los sitios de alojamiento web que utilizan ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Factores clave de rendimiento

Idealmente, todos los sitios web deben estar activos y en la memoria para asegurar una respuesta rápida a la siguiente solicitud, siempre que llegue. Los factores que pueden afectar la capacidad de respuesta del sitio incluyen:

- El tiempo que tarda un sitio en reiniciarse después de reciclar un grupo de aplicaciones. Este es el tiempo que se tarda en iniciar un proceso de servidor web para el sitio cuando los ensamblados de sitio ya no están en memoria. (Los ensamblados de plataforma todavía están en memoria, ya que son utilizados por otros sitios.) Esta situación se conoce como "sitio frío, inicio de marco cálido" o simplemente "inicio de sitio frío".
- Cuánta memoria ocupa el sitio. Los términos para esto son "consumo de memoria por sitio" o "conjunto de trabajo no compartido".

Las nuevas mejoras de rendimiento se centran en ambos factores.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisitos para nuevas características de rendimiento

Los requisitos para las nuevas características se pueden desglosar en estas categorías:

- Mejoras que se ejecutan en .NET Framework 4.
- Mejoras que requieren .NET Framework 4.5 pero se pueden ejecutar en cualquier versión de Windows.
- Mejoras que solo están disponibles con .NET Framework 4.5 que se ejecuta en Windows 8.

El rendimiento aumenta con cada nivel de mejora que puede habilitar.

Algunas de las mejoras de .NET Framework 4.5 aprovechan las características de rendimiento más amplias que también se aplican a otros escenarios.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Compartir asambleas comunes

**Requisito:**.NET Framework 4 y Visual Studio 11 Developer Preview SDK

Los sitios diferentes de un servidor suelen usar los mismos ensamblados auxiliares (por ejemplo, ensamblados de un kit de inicio o una aplicación de ejemplo). Cada sitio tiene su propia copia de estos ensamblados en su directorio Bin. Aunque el código de objeto para los ensamblados es idéntico, son ensamblados físicamente independientes, por lo que cada ensamblado debe leerse por separado durante el inicio del sitio en frío y mantenerse por separado en la memoria.

La nueva funcionalidad de internado resuelve esta ineficiencia y reduce los requisitos de RAM y el tiempo de carga. La internación permite a Windows mantener una sola copia de cada ensamblado en el sistema de archivos y los ensamblados individuales de las carpetas bin del sitio se reemplazan por vínculos simbólicos a la única copia. Si un sitio individual necesita una versión distinta del ensamblado, el vínculo simbólico se reemplaza por la nueva versión del ensamblado y solo ese sitio se ve afectado.

Compartir ensamblados mediante vínculos simbólicos requiere\_una nueva herramienta denominada aspnet intern.exe, que le permite crear y administrar el almacén de ensamblados internos. Se proporciona como parte del SDK de Visual Studio 11 Developer Preview. (Sin embargo, funcionará en un sistema que solo tiene instalado .NET Framework 4, suponiendo que haya instalado la [actualización](https://support.microsoft.com/kb/2468871)más reciente .)

Para asegurarse de que todos los ensamblados elegibles se han internado, ejecute aspnet\_intern.exe periódicamente (por ejemplo, una vez a la semana como una tarea programada). Un uso típico es el siguiente:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Para ver todas las opciones, ejecute la herramienta sin argumentos.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Uso de la compilación JIT multinúcleo para un inicio más rápido

**Requisito**: .NET Framework 4.5

Para un inicio de sitio en frío, no solo los ensamblados deben leerse desde el disco, sino que el sitio debe compilarse jIT. Para un sitio complejo, esto puede agregar retrasos significativos. Una nueva técnica de uso general en .NET Framework 4.5 reduce estos retrasos al distribuir la compilación JIT entre los núcleos de procesador disponibles. Lo hace tanto y tan pronto como sea posible mediante el uso de la información recopilada durante los lanzamientos anteriores del sitio. Esta funcionalidad implementada por el [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) método.

La compilación JIT con varios núcleos está habilitada de forma predeterminada en ASP.NET, por lo que no es necesario hacer nada para aprovechar esta característica. Si desea deshabilitar esta característica, realice la siguiente configuración en el archivo Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ajuste de la recolección de elementos no utilizados para optimizar la memoria

**Requisito**: .NET Framework 4.5

Una vez que se ejecuta un sitio, su uso del montón de recolector de elementos no utilizados (GC) puede ser un factor importante en su consumo de memoria. Al igual que cualquier recolector de elementos no utilizados, el GC de .NET Framework realiza compensaciones entre el tiempo de CPU (frecuencia e importancia de las colecciones) y el consumo de memoria (espacio adicional que se usa para objetos nuevos, liberados o que se pueden liberar). Para versiones anteriores, hemos proporcionado instrucciones sobre cómo configurar el GC para lograr el equilibrio adecuado (por ejemplo, consulte ASP.NET configuración de [hospedaje compartido 2.0/3.5](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Para .NET Framework 4.5, en lugar de varias opciones independientes, hay disponible una configuración definida por la carga de trabajo que habilita todas las opciones de GC recomendadas anteriormente, así como un nuevo ajuste que ofrece un rendimiento adicional para el conjunto de trabajo por sitio.

Para habilitar el ajuste de memoria DE GC, agregue la siguiente configuración al archivo de Windows, Microsoft.NET, Framework, v4.0.30319 y aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Si está familiarizado con las instrucciones anteriores para los cambios en aspnet.config, tenga en cuenta que esta configuración reemplaza la configuración anterior, por ejemplo, no hay necesidad de establecer gcServer, gcConcurrent, etc. No es necesario eliminar la configuración antigua.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Captura previa para aplicaciones web

**Requisito:**.NET Framework 4.5 que se ejecuta en Windows 8

Para varias versiones, Windows ha incluido una tecnología conocida como [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) que reduce el costo de lectura en disco del inicio de la aplicación. Dado que el inicio en frío es un problema predominantemente para las aplicaciones cliente, esta tecnología no se ha incluido en Windows Server, que incluye solo los componentes que son esenciales para un servidor. La captura previa ya está disponible en la última versión de Windows Server, donde puede optimizar el inicio de sitios web individuales.

Para Windows Server, el prefetcher no está habilitado de forma predeterminada. Para habilitar y configurar el prefetcher para el alojamiento web de alta densidad, ejecute el siguiente conjunto de comandos en la línea de comandos:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

A continuación, para integrar el prefetcher con ASP.NET aplicaciones, agregue lo siguiente al archivo Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controles de datos fuertemente tipados

En ASP.NET 4.5, los formularios Web Forms incluyen algunas mejoras para trabajar con datos. La primera mejora son los controles de datos fuertemente tipados. Para los controles de formularios Web Forms en versiones anteriores de ASP.NET, se muestra un valor enlazado a datos mediante *Eval* y una expresión de enlace de datos:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Para el enlace de datos bidireccional, utilice *Enlazar:*

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

En tiempo de ejecución, estas llamadas usan la reflexión para leer el valor del miembro especificado y, a continuación, mostrar el resultado en el marcado. Este enfoque facilita el enlace de datos con datos arbitrarios y sin forma.

Sin embargo, expresiones de enlace de datos como esta no admiten características como IntelliSense para nombres de miembro, navegación (como Ir a definición) o comprobación en tiempo de compilación para estos nombres.

Para solucionar este problema, ASP.NET 4.5 agrega la capacidad de declarar el tipo de datos de los datos a los que está enlazado un control. Esto se hace mediante la nueva *ItemType* propiedad. Al establecer esta propiedad, dos nuevas variables con tipo están disponibles en el ámbito de las expresiones de enlace de datos: *Item* y *BindItem*. Dado que las variables están fuertemente tipadas, obtendrá todas las ventajas de la experiencia de desarrollo de Visual Studio.

Para las expresiones de enlace de datos bidireccionales, utilice la variable *BindItem:*

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

La mayoría de los controles del marco de ASP.NET formularios Web Forms que admiten el enlace de datos se han actualizado para admitir la propiedad *ItemType.*

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Enlace de modelos

El enlace de modelos amplía el enlace de datos en ASP.NET controles de formularios Web Forms para trabajar con el acceso a datos centrado en código. Incorpora conceptos del control *ObjectDataSource* y del enlace de modelos en ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Selección de datos

Para configurar un control de datos para usar el enlace de modelos para seleccionar datos, establezca la propiedad *SelectMethod* del control en el nombre de un método en el código de la página. El control de datos llama al método en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos. No es necesario llamar explícitamente a la *DataBind* método.

En el ejemplo siguiente, el control *GridView* está configurado para utilizar un método denominado *GetCategories:*

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Crear el *GetCategories* método en el código de la página. Para una operación de selección simple, el método no necesita parámetros y debe devolver un *IEnumerable* o *IQueryable* objeto. Si se establece la nueva propiedad *ItemType* (que habilita expresiones de enlace de datos fuertemente tipadas, como se explica en Controles de [datos fuertemente tipados](#_Toc318097386) anteriormente), se deben devolver las versiones genéricas de estas interfaces: *&lt;IEnumerable T&gt; * o *IQueryable&lt;T&gt;*, con el parámetro *T* que coincida con el tipo de la propiedad *ItemType* (por ejemplo, *IQueryable&lt;Category&gt;*).

En el ejemplo siguiente se muestra el código de un *GetCategories* método. En este ejemplo se usa el modelo Entity Framework Code First con la base de datos de ejemplo Northwind. El código se asegura de que la consulta devuelve detalles de los productos relacionados para cada categoría mediante el *Método Include.* (Esto garantiza que el *elemento TemplateField* del marcado muestra el recuento de productos de cada categoría sin necesidad de [seleccionar n+1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Cuando se ejecuta la página, el *control GridView* llama al método *GetCategories* automáticamente y representa los datos devueltos mediante los campos configurados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Dado que el método select devuelve un objeto *IQueryable,* el control *GridView* puede manipular aún más la consulta antes de ejecutarla. Por ejemplo, el control *GridView* puede agregar expresiones de consulta para ordenar y paginar el objeto *IQueryable* devuelto antes de que se ejecute, de modo que esas operaciones las realice el proveedor LINQ subyacente. En este caso, Entity Framework se asegurará de que esas operaciones se realizan en la base de datos.

En el ejemplo siguiente se muestra el *control GridView* modificado para permitir la ordenación y paginación:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Ahora, cuando se ejecuta la página, el control puede asegurarse de que solo se muestra la página actual de datos y que está ordenada por la columna seleccionada:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Para filtrar los datos devueltos, los parámetros deben agregarse al método select. El enlace de modelos rellenará estos parámetros en tiempo de ejecución y puede usarlos para modificar la consulta antes de devolver los datos.

Por ejemplo, supongamos que desea permitir que los usuarios filtren productos escribiendo una palabra clave en la cadena de consulta. Puede agregar un parámetro al método y actualizar el código para utilizar el valor del parámetro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Este código incluye una expresión *Where* si se proporciona un valor para la *palabra clave* y, a continuación, devuelve los resultados de la consulta.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Proveedores de valor

El ejemplo anterior no era específico sobre de dónde proveniera el valor del parámetro *de palabra clave.* Para indicar esta información, puede utilizar un atributo de parámetro. En este ejemplo, puede usar la clase *QueryStringAttribute* que se encuentra en el espacio de nombres *System.Web.ModelBinding:*

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Esto indica al enlace de modelos que intente enlazar un valor de la cadena de consulta al parámetro *de palabra clave* en tiempo de ejecución. (Esto puede implicar la realización de la conversión de tipos, aunque no en este caso.) Si no se puede proporcionar un valor y el tipo no acepta valores NULL, se produce una excepción.

Los orígenes de valores para estos métodos se conocen como proveedores de valores y los atributos de parámetro que indican qué proveedor de valores usar se conocen como atributos de proveedor de valor. Los formularios Web Forms incluirán proveedores de valores y atributos correspondientes para todos los orígenes típicos de entrada del usuario en una aplicación de formularios Web Forms, como la cadena de consulta, las cookies, los valores de formulario, los controles, el estado de vista, el estado de sesión y las propiedades de perfil. También puede escribir proveedores de valores personalizados.

De forma predeterminada, el nombre del parámetro se utiliza como clave para buscar un valor en la colección del proveedor de valores. En el ejemplo, el código buscará un valor de cadena de consulta denominado keyword (por ejemplo, ./default.aspx?keyword-chef). Puede especificar una clave personalizada pasándola como argumento al atributo parameter. Por ejemplo, para utilizar el valor de la variable de cadena de consulta denominada q, podría hacer lo siguiente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Si este método está en el código de la página, los usuarios pueden filtrar los resultados pasando una palabra clave mediante la cadena de consulta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

El enlace de modelos realiza muchas tareas que de otro modo tendría que codificar a mano: leer el valor, comprobar un valor nulo, intentar convertirlo al tipo adecuado, comprobar si la conversión se realizó correctamente y, por último, usar el valor de la consulta. El enlace de modelos da como resultado mucho menos código y en la capacidad de reutilizar la funcionalidad en toda la aplicación.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrado por valores de un control

Supongamos que desea ampliar el ejemplo para permitir que el usuario elija un valor de filtro de una lista desplegable. Agregue la siguiente lista desplegable al marcado y configúrela para obtener sus datos de otro método mediante la *selectMethod* propiedad:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Normalmente, también agregaría un *emptyDataTemplate* elemento a la *GridView* control para que el control mostrará un mensaje si no se encuentra ningún producto coincidente:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

En el código de página, agregue el nuevo método select para la lista desplegable:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Por último, actualice el método de selección *GetProducts* para tomar un nuevo parámetro que contenga el identificador de la categoría seleccionada en la lista desplegable:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Ahora, cuando se ejecuta la página, los usuarios pueden seleccionar una categoría de la lista desplegable y el control *GridView* se vuelve a enlazar automáticamente para mostrar los datos filtrados. Esto es posible porque el enlace de modelos realiza un seguimiento de los valores de los parámetros de los métodos select y detecta si algún valor de parámetro ha cambiado después de una devolución de datos. Si es así, el enlace de modelos obliga al control de datos asociado a volver a enlazar a los datos.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expresiones de enlace de datos codificadas HTML

Ahora puede codificar en HTML el resultado de expresiones de enlace de datos. Agregue dos puntos (:) hasta el final &lt;del prefijo %- que marca la expresión de enlace de datos:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validación discreta

Ahora puede configurar los controles de validador integrados para usar JavaScript discreto para la lógica de validación del lado cliente. Esto reduce significativamente la cantidad de JavaScript representado en línea en el marcado de página y reduce el tamaño general de la página. Puede configurar JavaScript discreto para los controles de validador de cualquiera de estas maneras:

- Globalmente agregando la siguiente configuración al elemento * &lt;appSettings&gt; * en el archivo Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalmente estableciendo la propiedad estática *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* en *UnobtrusiveValidationMode.WebForms* (normalmente en el método *Application\_Start* en el archivo Global.asax).
- Individualmente para una página estableciendo la nueva propiedad *UnobtrusiveValidationMode* de la clase *Page* en *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Actualizaciones HTML5

Se han realizado algunas mejoras en los controles de servidor de formularios Web Forms para aprovechar las nuevas características de HTML5:

- La propiedad *TextMode* del control *TextBox* se ha actualizado para admitir los nuevos tipos de entrada HTML5 como *email*, *datetime,* etc.
- El control *FileUpload* ahora admite varias cargas de archivos desde exploradores que admiten esta característica HTML5.
- Los controles de validador ahora admiten la validación de elementos de entrada HTML5.
- Los nuevos elementos HTML5 que tienen atributos que representan una dirección URL ahora admiten runat "server". Como resultado, puede utilizar ASP.NET convenciones en las rutas de acceso de url, como el operador para representar la raíz de la aplicación (por ejemplo, &lt;video runat-"server" src-"-/myVideo.wmv" /&gt;).
- El control *UpdatePanel* se ha corregido para admitir el registro de campos de entrada HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta ahora se incluye con Visual Studio 11 Beta. ASP.NET MVC es un marco para desarrollar aplicaciones web altamente comprobables y mantenibles aprovechando el patrón Model-View-Controller (MVC). ASP.NET MVC 4 facilita la creación de aplicaciones para la Web móvil e incluye ASP.NET Web API, que le ayuda a crear servicios HTTP que pueden llegar a cualquier dispositivo. Para obtener más información, vea las notas de la [versión de ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

Las nuevas características incluyen lo siguiente:

- Plantillas de sitio nuevas y actualizadas.
- Agregar validación del lado del servidor y del cliente mediante la aplicación auxiliar *Validación.*
- La capacidad de registrar scripts mediante un administrador de activos.
- Habilitar los inicios de sesión desde Facebook y otros sitios mediante OAuth y OpenID.
- Agregar mapas mediante la aplicación auxiliar *Mapas.*
- Ejecución de aplicaciones de páginas web en paralelo.
- Renderización de páginas para dispositivos móviles.

Para obtener más información acerca de estas características y ejemplos de código de página completa, vea Las características principales de [las páginas web 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

En esta sección se proporciona información acerca de las mejoras para el desarrollo web en Visual Web Developer 11 Beta y Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Uso compartido de proyectos entre Visual Studio 2010 y Visual Studio 2012 Release Candidate (compatibilidad de proyectos)

Hasta Que Visual Studio 2012 Release Candidate, al abrir un proyecto existente en una versión más reciente de Visual Studio se inició el Asistente para conversión. Esto forzó una actualización del contenido (activos) de un proyecto y solución a nuevos formatos que no eran compatibles con versiones anteriores. Por lo tanto, después de la conversión no se pudo abrir el proyecto en la versión anterior de Visual Studio.

Muchos clientes nos han dicho que este no era el enfoque correcto. En Visual Studio 11 Beta, ahora se admite el uso compartido de proyectos y soluciones con Visual Studio 2010 SP1. Esto significa que si abre un proyecto de 2010 en Visual Studio 2012 Release Candidate, podrá abrir el proyecto en Visual Studio 2010 SP1.

> [!NOTE]
> Algunos tipos de proyectos no se pueden compartir entre Visual Studio 2010 SP1 y Visual Studio 2012 Release Candidate. Estos incluyen algunos proyectos más antiguos (como ASP.NET MVC 2) o proyectos para fines especiales (como proyectos de instalación).

Al abrir un proyecto Web de Visual Studio 2010 SP1 por primera vez en Visual Studio 11 Beta, se agregan las siguientes propiedades al archivo de proyecto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation y OldToolsVersion son utilizados por el proceso que actualiza el archivo de proyecto. No tienen ningún impacto en el trabajo con el proyecto en Visual Studio 2010.

VisualStudioVersion es una nueva propiedad utilizada por MSBuild 4.5 que indica la versión de Visual Studio para el proyecto actual. Dado que esta propiedad no existía en MSBuild 4.0 (la versión de MSBuild que usa Visual Studio 2010 SP1), insertamos un valor predeterminado en el archivo de proyecto.

La propiedad VSToolsPath se utiliza para determinar el archivo .targets correcto que se va a importar desde la ruta de acceso representada por la configuración MSBuildExtensionsPath32.

También hay algunos cambios relacionados con los elementos Import. Estos cambios son necesarios para admitir la compatibilidad entre ambas versiones de Visual Studio.

> [!NOTE]
> Si se comparte un proyecto entre Visual Studio 2010 SP1 y Visual Studio 11 Beta en\_dos equipos diferentes y si el proyecto incluye una base de datos local en la carpeta Datos de la aplicación, debe asegurarse de que la versión de SQL Server utilizada por la base de datos está instalada en ambos equipos.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Cambios de configuración en ASP.NET 4.5 Plantillas de sitio web

Se han realizado los siguientes cambios en el archivo *Web.config* predeterminado para el sitio que se crean mediante plantillas de sitio web en Visual Studio 2012 Release Candidate:

- En `<httpRuntime>` el elemento, el `encoderType` atributo ahora se establece de forma predeterminada para usar los tipos de AntiXSS que se agregaron a ASP.NET. Para obtener más información, consulte [Biblioteca antiXSS](#_Toc318097382).
- También en `<httpRuntime>` el `requestValidationMode` elemento, el atributo se establece en "4.5". Esto significa que, de forma predeterminada, la validación de solicitudes está configurada para usar la validación diferida ("perezosa"). Para obtener más información, consulte [Nuevas ASP.NET características](#_Toc318097379)de validación de solicitudes .
- El `<modules>` elemento `<system.webServer>` de la sección `runAllManagedModulesForAllRequests` no contiene un atributo. (Su valor predeterminado es false.) Esto significa que si usa una versión de IIS 7 que no se ha actualizado a SP1, es posible que tenga problemas con el enrutamiento en un sitio nuevo. Para obtener más información, vea [Compatibilidad nativa en IIS 7 para ASP.NET enrutamiento](#Native_Support_In_IIS7_For_ASPNET_Routine).

Estos cambios no afectan a las aplicaciones existentes. Sin embargo, pueden representar una diferencia de comportamiento entre los sitios web existentes y los nuevos sitios web que cree para ASP.NET 4.5 con las nuevas plantillas.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Compatibilidad nativa en IIS 7 para enrutamiento de ASP.NET

Esto no es un cambio en ASP.NET como tal, sino un cambio en las plantillas para nuevos proyectos de sitio web que puede afectarle si está trabajando una versión de IIS 7 que no ha aplicado la actualización SP1.

En ASP.NET, usted puede agregar la siguiente configuración a las aplicaciones para soportar el ruteo:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Cuando **runAllManagedModulesForAllRequests** es true, `http://mysite/myapp/home` una dirección URL como va a ASP.NET, aunque no hay ninguna extensión *.aspx*, *.mvc*o similar en la dirección URL.

Una actualización que se realizó en IIS 7 hace que la configuración **runAllManagedModulesForAllRequests** sea innecesaria y admite ASP.NET enrutamiento de forma nativa. (Para obtener información acerca de la actualización, consulte el artículo de soporte técnico de Microsoft [Una actualización está disponible que permite a determinados controladores de IIS 7.0 o IIS 7.5 controlar las solicitudes cuyas direcciones URL no terminan con un punto](https://support.microsoft.com/kb/980368).)

Si el sitio web se ejecuta en IIS 7 y si IIS se ha actualizado, no es necesario establecer **runAllManagedModulesForAllRequests** en true. De hecho, no se recomienda establecerlo en true, ya que agrega una sobrecarga de procesamiento innecesaria a la solicitud. Cuando esta configuración es true, todas las solicitudes, incluidas las de *.htm*, *.jpg*y otros archivos estáticos, también pasan por la canalización de solicitudes de ASP.NET.

Si crea un nuevo sitio web de ASP.NET 4.5 con las plantillas que se proporcionan en Visual Studio 2012 RC, la configuración del sitio web no incluye la configuración **runAllManagedModulesForAllRequests.** Esto significa que, de forma predeterminada, la configuración es false.

Si, a continuación, ejecuta el sitio web en Windows 7 sin SP1 instalado, IIS 7 no incluirá la actualización necesaria. Como consecuencia, el enrutamiento no funcionará y verá errores. Si tiene un problema en el que el enrutamiento no funciona, puede hacer lo siguiente:

- Actualice Windows 7 a SP1, que agregará la actualización a IIS 7.
- Instale la actualización que se describe en el artículo de soporte técnico de Microsoft que se muestra anteriormente.
- Establezca **runAllManagedModulesForAllRequests** en true en el archivo Web.config de ese sitio web. Tenga en cuenta que esto agregará cierta sobrecarga a las solicitudes.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor de HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tareas inteligentes

En la vista Diseño, las propiedades complejas de los controles de servidor a menudo tienen cuadros de diálogo y asistentes asociados para facilitar su configuración. Por ejemplo, puede usar un cuadro de diálogo especial para agregar un origen de datos a un *Control Repeater* o agregar columnas a un *control GridView.*

Sin embargo, este tipo de ayuda de la interfaz de usuario para propiedades complejas no ha estado disponible en la vista Origen. Por lo tanto, Visual Studio 11 presenta tareas inteligentes para la vista de origen. Las tareas inteligentes son accesos directos contextuales para las características de uso común en los editores de Visual Basic y de C.

Para ASP.NET controles de formularios Web Forms, las tareas inteligentes aparecen en las etiquetas de servidor como un pequeño glifo cuando el punto de inserción está dentro del elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

La tarea inteligente se expande al hacer clic en el glifo o presionar CTRL+. (punto), al igual que en los editores de código. A continuación, muestra accesos directos similares a la vista Tareas inteligentes en diseño.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Por ejemplo, la tarea inteligente en la ilustración anterior muestra el GridView tareas opciones. Si elige Editar columnas, se muestra el siguiente cuadro de diálogo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Rellenar el cuadro de diálogo establece las mismas propiedades que puede establecer en la vista Diseño. Al hacer clic en Aceptar, el marcado del control se actualiza con la nueva configuración:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Soporte WAI-ARIA

La escritura de sitios web accesibles es cada vez más importante. El estándar de [accesibilidad WAI-ARIA](http://www.w3.org/WAI/intro/aria) define cómo los desarrolladores deben escribir sitios web accesibles. Este estándar ahora es totalmente compatible con Visual Studio.

Por ejemplo, el atributo *role* ahora tiene IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

El estándar WAI-ARIA también introduce atributos que tienen el prefijo *aria-* que le permiten agregar semántica a un documento HTML5. Visual Studio también es totalmente compatible con estos atributos *aria:*

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuevos fragmentos de código HTML5

Para que sea más rápido y fácil escribir el marcado HTML5 de uso común, Visual Studio incluye una serie de fragmentos de código. Un ejemplo es el fragmento de vídeo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Para invocar el fragmento de código, presione Tab dos veces cuando el elemento está seleccionado en IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Esto genera un fragmento de código que puede personalizar.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extraer al control del usuario

En páginas web grandes, puede ser una buena idea mover piezas individuales a los controles de usuario. Esta forma de refactorización puede ayudar a aumentar la legibilidad de la página y puede simplificar la estructura de la página.

Para facilitar esto, al editar páginas de formularios Web Forms en la vista Origen, ahora puede seleccionar texto en una página, hacer clic con el botón derecho en ella y, a continuación, elegir Extraer al control de usuario:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense para pepitas de código en atributos

Visual Studio siempre ha proporcionado IntelliSense para nuggets de código del lado servidor en cualquier página o control. Ahora Visual Studio también incluye IntelliSense para nuggets de código en atributos HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Esto facilita la creación de expresiones de enlace de datos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Cambio automático de nombre de la etiqueta coincidente al cambiar el nombre de una etiqueta de apertura o cierre

Si cambia el nombre de un elemento HTML (por ejemplo, cambia una etiqueta *div* para que sea una etiqueta de *encabezado),* la etiqueta de apertura o cierre correspondiente también cambia en tiempo real.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Esto ayuda a evitar el error en el que se olvida de cambiar una etiqueta de cierre o cambiar la incorrecta.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generación de controladores de eventos

Visual Studio ahora incluye características en la vista Origen para ayudarle a escribir controladores de eventos y enlazarlos manualmente. Si está editando un nombre de &lt;evento en&gt;la vista Origen, IntelliSense muestra Crear nuevo evento , que creará un controlador de eventos en el código de la página que tenga la firma correcta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

De forma predeterminada, el controlador de eventos usará el identificador del control para el nombre del método de control de eventos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

El controlador de eventos resultante tendrá este aspecto (en este caso, en C-):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Sangría inteligente

Al pulsar Intro mientras está dentro de un elemento HTML vacío, el editor colocará el punto de inserción en el lugar correcto:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Si pulsa Intro en esta ubicación, la etiqueta de cierre se mueve hacia abajo y se sangra para que coincida con la etiqueta de apertura. El punto de inserción también tiene sangría:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Reducción automática de la finalización de la declaración

La lista IntelliSense de Visual Studio ahora filtra en función de lo que escriba para que muestre solo las opciones relevantes:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense también filtra en función del caso de título de las palabras individuales de la lista IntelliSense. Por ejemplo, si escribe "dl", se muestran dl y asp:DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Esta característica hace que sea más rápido obtener la finalización de instrucciones para los elementos conocidos.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>editor de JavaScript

El editor de JavaScript en Visual Studio 2012 Release Candidate es completamente nuevo y mejora en gran medida la experiencia de trabajar con JavaScript en Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Esquematización de código

Las regiones de esquematización ahora se crean automáticamente para todas las funciones, lo que le permite contraer partes del archivo que no son pertinentes para el foco actual.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Coincidencia de llaves

Al colocar el punto de inserción en una llave de apertura o cierre, el editor resalta el que coincide.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Ir a definición

El comando Ir a definición le permite saltar al origen de una función o variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Compatibilidad con ECMAScript5

El editor admite la nueva sintaxis y las API de ECMAScript5, la última versión del estándar que describe el lenguaje JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

Se ha mejorado la API de IntelliSense para DOM, con compatibilidad con muchas API HTML5 nuevas, como *querySelector,* almacenamiento DOM, mensajería entre documentos y *lienzo.* DOM IntelliSense ahora está controlado por un único archivo JavaScript simple, en lugar de por una definición de biblioteca de tipos nativa. Esto hace que sea fácil de extender o reemplazar.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Sobrecargas de firma VSDOC

Los comentarios detallados de IntelliSense ahora se pueden declarar * &lt;&gt; * para sobrecargas independientes de funciones de JavaScript mediante el nuevo elemento de firma, como se muestra en este ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Referencias implícitas

Ahora puede agregar archivos JavaScript a una lista central que se incluirá implícitamente en la lista de archivos a los que hace referencia cualquier archivo o bloque de JavaScript, lo que significa que obtendrá IntelliSense por su contenido. Por ejemplo, puede agregar archivos jQuery a la lista central de archivos y obtendrá funciones IntelliSense para jQuery en cualquier bloque de &lt;archivo&gt;JavaScript, independientemente de si ha hecho referencia a él explícitamente (mediante /// reference / ) o no.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Editor CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Reducción automática de la finalización de la declaración

La lista IntelliSense para CSS ahora filtra en función de las propiedades CSS y los valores admitidos por el esquema seleccionado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense también admite búsquedas de mayúsculas y minúsculas:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Aplicación de sangría jerárquica

El editor CSS utiliza la sangría para mostrar las reglas jerárquicas, lo que le proporciona una visión general de cómo se organizan lógicamente las reglas en cascada. En el ejemplo siguiente, el #list un selector es un elemento secundario en cascada de list y, por lo tanto, tiene sangría.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

En el ejemplo siguiente se muestra una herencia más compleja:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

La sangría de una regla viene determinada por sus reglas primarias. La sangría jerárquica está habilitada de forma predeterminada, pero puede deshabilitarla en el cuadro de diálogo Opciones (Herramientas, Opciones en la barra de menús):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Soporte de hacks CSS

El análisis de cientos de archivos CSS del mundo real muestra que los hacks CSS son muy comunes, y ahora Visual Studio es compatible con los más utilizados. Este soporte incluye IntelliSense y\*validación de\_la estrella ( ) y guiones bajos ( ) hacks de propiedades:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Los hacks selectores típicos también son compatibles para que la sangría jerárquica se mantenga incluso cuando se aplican. Un hack selector típico utilizado para apuntar a Internet Explorer 7 es anteponer un selector con * \*:first-child + html*. El uso de esa regla mantendrá la sangría jerárquica:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Esquemas específicos del proveedor (-moz-, -webkit)

CSS3 introduce muchas propiedades que han sido implementadas por diferentes navegadores en diferentes momentos. Esto anteriormente obligó a los desarrolladores a codificar para exploradores específicos mediante la sintaxis específica del proveedor. Estas propiedades específicas del explorador ahora se incluyen en IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Comentarios y descommenting de apoyo

Ahora puede comentar y quitar comentarios de las reglas CSS utilizando los mismos accesos directos que usa en el editor de código (Ctrl+K,C para comentar y Ctrl+K, para quitar el comentario).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Color picker

En versiones anteriores de Visual Studio, IntelliSense para atributos relacionados con el color consistía en una lista desplegable de valores de color con nombre. Esa lista ha sido reemplazada por un selector de color con todas las funciones.

Al introducir un valor de color, el selector de color se muestra automáticamente y presenta una lista de colores utilizados anteriormente seguida de una paleta de colores predeterminada. Puede seleccionar un color con el ratón o el teclado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

La lista se puede expandir en un selector de color completo. El selector le permite controlar el canal alfa convirtiendo automáticamente cualquier color en RGBA al mover el control deslizante de opacidad:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmentos de código

Los fragmentos de código en el editor CSS hacen que sea más fácil y rápido crear estilos entre navegadores. Muchas propiedades CSS3 que requieren la configuración específica del explorador ahora se han enrollado en fragmentos de código.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Los fragmentos de código CSS admiten escenarios avanzados (como consultas de medios CSS3) escribiendo el símbolo en el símbolo , que muestra la lista de IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Al seleccionar @media value y pulsar Tab, el editor CSS inserta el siguiente fragmento de código:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Al igual que con los fragmentos de código, puede crear sus propios fragmentos de código CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Regiones personalizadas

Las regiones de código con nombre, que ya están disponibles en el editor de código, ahora están disponibles para la edición CSS. Esto le permite agrupar fácilmente bloques de estilo relacionados.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Cuando se contrae una región, muestra el nombre de la región:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspector de página

Inspector de página es una herramienta que representa una página web (HTML, formularios Web Forms, ASP.NET MVC o páginas web) en el IDE de Visual Studio y le permite examinar el código fuente y la salida resultante. Para ASP.NET páginas, el Inspector de página le permite determinar qué código del lado servidor ha generado el marcado HTML que se representa en el explorador.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Para obtener más información acerca de Inspector de página, consulte los siguientes tutoriales:

- Uso de Inspector de página en [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Uso del Inspector de página en [ASP.NET formularios Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publicación

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Perfiles de publicación

En Visual Studio 2010, la publicación de información para proyectos de aplicación web no se almacena en el control de versiones y no está diseñada para compartircon otros usuarios. En Visual Studio 2012 Release Candidate, se ha cambiado el formato del perfil de publicación. Se ha convertido en un artefacto de equipo y ahora es fácil aprovechar las compilaciones basadas en MSBuild. La información de configuración de compilación se encuentra en el cuadro de diálogo Publicar para que pueda cambiar fácilmente las configuraciones de compilación antes de publicar.

Los perfiles de publicación se almacenan en la carpeta PublishProfiles. La ubicación de la carpeta depende del lenguaje de programación que esté utilizando:

- C-: Propiedades-PublishProfiles
- Visual Basic: My Project-PublishProfiles

Cada perfil es un archivo MSBuild. Durante la publicación, este archivo se importa en el archivo mSBuild del proyecto. En Visual Studio 2010, si desea realizar cambios en el proceso de publicación o paquete, debe colocar las personalizaciones en un archivo denominado **ProjectName**.wpp.targets. Esto sigue siendo compatible, pero ahora puede colocar las personalizaciones en el propio perfil de publicación. De este modo, las personalizaciones solo se usarán para ese perfil.

Ahora también puede aprovechar los perfiles de publicación de MSBuild. Para ello, utilice el siguiente comando al compilar el proyecto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

El valor project.csproj es la ruta de acceso del proyecto y ProfileName es el nombre del perfil que se va a publicar. Como alternativa, en lugar de pasar el nombre del perfil de la propiedad *PublishProfile,* puede pasar la ruta de acceso completa al perfil de publicación.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET precompilación y fusión

Para los proyectos de aplicación web, Visual Studio 2012 Release Candidate agrega una opción en la página de propiedades Web Package/Publish que le permite precompilar y combinar el contenido del sitio al publicar o empaquetar el proyecto. Para ver estas opciones, haga clic con el botón secundario en el proyecto en el Explorador de soluciones, elija Propiedades y, a continuación, elija la página de propiedades Empaquetar/Publicar Web. En la siguiente ilustración se muestra la opción Precompilar esta aplicación antes de publicar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Cuando se selecciona esta opción, Visual Studio precompila la aplicación cada vez que se publica o empaqueta la aplicación web. Si desea controlar cómo se precompila el sitio o cómo se combinan los ensamblados, haga clic en el botón Avanzadas para configurar esas opciones.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

El servidor web predeterminado para probar proyectos web en Visual Studio ahora es IIS Express. El servidor de desarrollo de Visual Studio sigue siendo una opción para el servidor web local durante el desarrollo, pero IIS Express es ahora el servidor recomendado. La experiencia de usar IIS Express en Visual Studio 11 Beta es muy similar a su uso en Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Renuncia de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información contenida en este documento representa la visión actual de Microsoft Corporation sobre los asuntos tratados a fecha de su publicación. Dado que Microsoft debe responder a condiciones de mercado variables, no debe interpretarse como un compromiso por parte de Microsoft, y Microsoft no puede garantizar la precisión de la información presentada tras la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de todas las leyes de derechos de autor aplicables. Sin limitar los derechos otorgados por las leyes de derechos de autor, ninguna parte de este documento puede ser reproducida o introducida en un sistema de recuperación, ni transmitida de ninguna forma ni por ningún medio, ya sea electrónico, mecánico, fotocopias, grabación u otros, con ningún propósito, sin la previa autorización por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor y otros derechos de propiedad intelectual sobre los contenidos de este documento. El suministro de este documento no le otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que ello se prevea en un contrato por escrito de licencia de Microsoft.

A menos que se indique lo contrario, las empresas, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y eventos aquí descritos son ficticios, y ninguna asociación con ninguna empresa, organización, producto, nombre de dominio, dirección de correo electrónico, logotipo, persona, lugar o evento real está destinada o debe ser inferida.

© 2012 Microsoft Corporation. Todos los derechos reservados.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
