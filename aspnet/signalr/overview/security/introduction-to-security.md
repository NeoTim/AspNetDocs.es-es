---
uid: signalr/overview/security/introduction-to-security
title: Introducción a la seguridad de SignalR ( SignalR Security) Microsoft Docs
author: bradygaster
description: Describe los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación de SignalR.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675812"
---
# <a name="introduction-to-signalr-security"></a>Introducción a la seguridad de SignalR

por [Patrick Fletcher](https://github.com/pfletcher), Tom [FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describen los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación de SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Conceptos de seguridad de SignalR](#concepts)

    - [Autenticación y autorización](#authentication)
    - [Token de conexión](#connectiontoken)
    - [Volver a unirse a grupos al reconectar](#rejoingroup)
- [Cómo SignalR evita la falsificación de solicitudes entre sitios](#csrf)
- [Recomendaciones de seguridad de SignalR](#recommendations)

    - [Protocolo Ssl (Secure Socket Layers)](#ssl)
    - [No utilice grupos como mecanismo de seguridad](#groupsecurity)
    - [Manejo seguro de las aportaciones de los clientes](#input)
    - [Reconciliar un cambio en el estado del usuario con una conexión activa](#reconcile)
    - [Archivos proxy JavaScript generados automáticamente](#autogen)
    - [Excepciones](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Conceptos de seguridad de SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticación y autorización

SignalR no proporciona ninguna característica para autenticar a los usuarios. En su lugar, se integran las características de SignalR en la estructura de autenticación existente para una aplicación. Los usuarios se autentican como lo haría normalmente en la aplicación y se trabaja con los resultados de la autenticación en el código de SignalR. Por ejemplo, puede autenticar a los usuarios con autenticación de formularios ASP.NET y, a continuación, en el concentrador, exigir qué usuarios o roles están autorizados a llamar a un método. En el concentrador, también puede pasar información de autenticación, como el nombre de usuario o si un usuario pertenece a un rol, al cliente.

SignalR proporciona el [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar qué usuarios tienen acceso a un concentrador o método. El atributo Authorize se aplica a un concentrador o a métodos concretos de un concentrador. Sin el atributo Authorize, todos los métodos públicos del concentrador están disponibles para un cliente que está conectado al concentrador. Para obtener más información acerca de los concentradores, vea [Autenticación y autorización para concentradores de SignalR](hub-authorization.md).

El `Authorize` atributo se aplica a los concentradores, pero no a las conexiones persistentes. Para aplicar reglas de `PersistentConnection` autorización `AuthorizeRequest` al usar un debe invalidar el método. Para obtener más información acerca de las conexiones persistentes, vea [Autenticación y autorización para conexiones persistentes](persistent-connection-authorization.md)de SignalR .

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token de conexión

SignalR mitiga el riesgo de ejecutar comandos malintencionados validando la identidad del remitente. Para cada solicitud, el cliente y el servidor pasan un token de conexión que contiene el identificador de conexión y el nombre de usuario para los usuarios autenticados. El identificador de conexión identifica de forma única cada cliente conectado. El servidor genera aleatoriamente el identificador de conexión cuando se crea una nueva conexión y conserva ese identificador durante la conexión. El mecanismo de autenticación para la aplicación web proporciona el nombre de usuario. SignalR utiliza cifrado y una firma digital para proteger el token de conexión.

![](introduction-to-security/_static/image2.png)

Para cada solicitud, el servidor valida el contenido del token para asegurarse de que la solicitud procede del usuario especificado. El nombre de usuario debe corresponder al identificador de conexión. Al validar el identificador de conexión y el nombre de usuario, SignalR impide que un usuario malintencionado se suplante fácilmente a otro usuario. Si el servidor no puede validar el token de conexión, se produce un error en la solicitud.

![](introduction-to-security/_static/image4.png)

Dado que el identificador de conexión forma parte del proceso de verificación, no debe revelar el identificador de conexión de un usuario a otros usuarios ni almacenar el valor en el cliente, como en una cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Tokens de conexión frente a otros tipos de tokens

Las herramientas de seguridad marcan ocasionalmente los tokens de conexión porque parecen ser tokens de sesión o tokens de autenticación, lo que supone un riesgo si se exponen.

El token de conexión de SignalR no es un token de autenticación. Se utiliza para confirmar que el usuario que realiza esta solicitud es el mismo que creó la conexión. El token de conexión es necesario porque ASP.NET SignalR permite que las conexiones se muevan entre servidores. El token asocia la conexión con un usuario determinado, pero no afirma la identidad del usuario que realiza la solicitud. Para que una solicitud de SignalR se autentique correctamente, debe tener algún otro token que afirme la identidad del usuario, como una cookie o un token de portador. Sin embargo, el propio token de conexión no hace ninguna notificación de que la solicitud fue realizada por ese usuario, solo que el identificador de conexión contenido en el token está asociado a ese usuario.

Dado que el token de conexión no proporciona ninguna notificación de autenticación propia, no se considera un token de "sesión" o "autenticación". Se producirá un error al tomar el token de conexión de un usuario determinado y reproducirlo en una solicitud autenticada como un usuario diferente (o una solicitud no autenticada), porque la identidad de usuario de la solicitud y la identidad almacenada en el token no coincidirán.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Volver a unirse a grupos al reconectar

De forma predeterminada, la aplicación SignalR reasignará automáticamente un usuario a los grupos adecuados al volver a conectarse desde una interrupción temporal, como cuando se quita y restablece una conexión antes de que se agota el tiempo de espera de la conexión. Al volver a conectarse, el cliente pasa un token de grupo que incluye el identificador de conexión y los grupos asignados. El token de grupo está firmado y cifrado digitalmente. El cliente conserva el mismo identificador de conexión después de una reconexión; por lo tanto, el identificador de conexión pasado desde el cliente reconectado debe coincidir con el identificador de conexión anterior utilizado por el cliente. Esta comprobación impide que un usuario malintencionado pase solicitudes para unirse a grupos no autorizados al volver a conectarse.

Sin embargo, es importante tener en cuenta que el token de grupo no caduca. Si un usuario pertenecía a un grupo en el pasado, pero se le prohibió ese grupo, ese usuario puede imitar un token de grupo que incluye el grupo prohibido. Si necesita administrar de forma segura qué usuarios pertenecen a qué grupos, debe almacenar esos datos en el servidor, como en una base de datos. A continuación, agregue lógica a la aplicación que comprueba en el servidor si un usuario pertenece a un grupo. Para obtener un ejemplo de comprobación de la pertenencia a [grupos,](../guide-to-the-api/working-with-groups.md)consulte Trabajar con grupos .

La reincorporación automática de grupos solo se aplica cuando se vuelve a conectar una conexión después de una interrupción temporal. Si un usuario se desconecta al alejarse de la aplicación o se reinicia la aplicación, la aplicación debe controlar cómo agregar ese usuario a los grupos correctos. Para obtener más información, consulte [Trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Cómo SignalR evita la falsificación de solicitudes entre sitios

La falsificación de solicitudes entre sitios (CSRF) es un ataque en el que un sitio malintencionado envía una solicitud a un sitio vulnerable en el que el usuario ha iniciado sesión actualmente. SignalR previene CSRF por lo que es extremadamente improbable que un sitio malintencionado cree una solicitud válida para la aplicación SignalR.

### <a name="description-of-csrf-attack"></a>Descripción del ataque CSRF

Aquí está un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en www.example.com mediante la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar sesión, el usuario visita un sitio web malintencionado. Este sitio malicioso contiene el siguiente formulario HTML:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Tenga en cuenta que la acción del formulario se publica en el sitio vulnerable, no en el sitio malintencionado. Esta es la parte "entre sitios" de CSRF.
4. El usuario hace clic en el botón Enviar. El navegador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor example.com con el contexto de autenticación del usuario y puede hacer cualquier cosa que un usuario autenticado puede hacer.

Aunque este ejemplo requiere que el usuario haga clic en el botón de formulario, la página malintencionada podría ejecutar fácilmente un script que envía una solicitud AJAX a la aplicación SignalR. Además, el uso de SSL no impide un ataque CSRF, porque el sitio malintencionado puede enviar una solicitud de "https://".

Normalmente, los ataques CSRF son posibles contra sitios web que utilizan cookies para la autenticación, ya que los navegadores envían todas las cookies relevantes al sitio web de destino. Sin embargo, los ataques CSRF no se limitan a la explotación de cookies. Por ejemplo, la autenticación básica y implícita también son vulnerables. Después de que un usuario inicie sesión con la autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que finaliza la sesión.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigaciones de CSRF tomadas por SignalR

SignalR toma los pasos siguientes para evitar que un sitio malintencionado cree solicitudes válidas a la aplicación. SignalR realiza estos pasos de forma predeterminada, no es necesario realizar ninguna acción en el código.

- **Deshabilitar las solicitudes entre dominios** SignalR deshabilita las solicitudes entre dominios para evitar que los usuarios llamen a un extremo de SignalR desde un dominio externo. SignalR considera que cualquier solicitud de un dominio externo no es válida y bloquea la solicitud. Le recomendamos que mantenga este comportamiento predeterminado; de lo contrario, un sitio malicioso podría engañar a los usuarios para que envíen comandos a su sitio. Si necesita usar solicitudes entre dominios, consulte [Cómo establecer una conexión entre dominios.](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)
- **Pasar token de conexión en cadena** de consulta, no cookie SignalR pasa el token de conexión como un valor de cadena de consulta, en lugar de como una cookie. Almacenar el token de conexión en una cookie no es seguro porque el explorador puede reenviar inadvertidamente el token de conexión cuando se encuentra código malintencionado. Además, pasar el token de conexión en la cadena de consulta impide que el token de conexión persista más allá de la conexión actual. Por lo tanto, un usuario malintencionado no puede realizar una solicitud con las credenciales de autenticación de otro usuario.
- **Verificar token de conexión** Como se describe en la sección Token de [conexión,](#connectiontoken) el servidor sabe qué identificador de conexión está asociado a cada usuario autenticado. El servidor no procesa ninguna solicitud de un identificador de conexión que no coincida con el nombre de usuario. Es poco probable que un usuario malintencionado podría adivinar una solicitud válida porque el usuario malintencionado tendría que conocer el nombre de usuario y el identificador de conexión generado aleatoriamente actual. Ese identificador de conexión deja de ser válido tan pronto como finaliza la conexión. Los usuarios anónimos no deben tener acceso a ninguna información confidencial.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recomendaciones de seguridad de SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocolo Ssl (Secure Socket Layers)

El protocolo SSL utiliza cifrado para proteger el transporte de datos entre un cliente y un servidor. Si la aplicación SignalR transmite información confidencial entre el cliente y el servidor, utilice SSL para el transporte. Para obtener más información acerca de la configuración de SSL, consulte [Cómo configurar SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>No utilice grupos como mecanismo de seguridad

Los grupos son una forma cómoda de recopilar usuarios relacionados, pero no son un mecanismo seguro para limitar el acceso a la información confidencial. Esto es especialmente cierto cuando los usuarios pueden volver a unirse automáticamente a grupos durante una reconexión. En su lugar, considere la posibilidad de agregar usuarios con privilegios a un rol y limitar el acceso a un método de concentrador solo a los miembros de ese rol. Para obtener un ejemplo de restricción del acceso basado en un rol, vea [Autenticación y autorización para concentradores](hub-authorization.md)de SignalR . Para obtener un ejemplo de comprobación del acceso de los usuarios a los grupos al volver a conectarse, consulte [Trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Manejo seguro de las aportaciones de los clientes

Para asegurarse de que un usuario malintencionado no envía scripts a otros usuarios, debe codificar todas las entradas de los clientes que están destinadas a la difusión a otros clientes. Debe codificar los mensajes en los clientes receptores en lugar del servidor, porque la aplicación DeSignalR puede tener muchos tipos diferentes de clientes. Por lo tanto, la codificación HTML funciona para un cliente web, pero no para otros tipos de clientes. Por ejemplo, un método de cliente web para mostrar un mensaje `html()` de chat controlaría de forma segura el nombre de usuario y el mensaje llamando a la función.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Reconciliar un cambio en el estado del usuario con una conexión activa

Si el estado de autenticación de un usuario cambia mientras existe una conexión activa, el usuario recibirá un error que indica: "La identidad del usuario no puede cambiar durante una conexión de SignalR activa." En ese caso, la aplicación debe volver a conectarse al servidor para asegurarse de que el identificador de conexión y el nombre de usuario están coordinados. Por ejemplo, si la aplicación permite al usuario cerrar la sesión mientras existe una conexión activa, el nombre de usuario de la conexión ya no coincidirá con el nombre que se pasa para la siguiente solicitud. Deberá detener la conexión antes de que el usuario cierra la sesión y, a continuación, reiniciarla.

Sin embargo, es importante tener en cuenta que la mayoría de las aplicaciones no necesitarán detener e iniciar manualmente la conexión. Si la aplicación redirige a los usuarios a una página independiente después de cerrar la sesión, como el comportamiento predeterminado en una aplicación de formularios Web Forms o una aplicación MVC, o actualiza la página actual después de cerrar sesión, la conexión activa se desconecta automáticamente y no requiere ninguna acción adicional.

En el ejemplo siguiente se muestra cómo detener e iniciar una conexión cuando el estado del usuario ha cambiado.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

O bien, el estado de autenticación del usuario puede cambiar si su sitio utiliza la expiración deslizante con autenticación de formularios y no hay actividad para mantener válida la cookie de autenticación. En ese caso, se cerrará la sesión del usuario y el nombre de usuario ya no coincidirá con el nombre de usuario en el token de conexión. Puede solucionar este problema agregando algún script que solicite periódicamente un recurso en el servidor web para mantener la cookie de autenticación válida. En el ejemplo siguiente se muestra cómo solicitar un recurso cada 30 minutos.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Archivos proxy JavaScript generados automáticamente

Si no desea incluir todos los concentradores y métodos en el archivo proxy de JavaScript para cada usuario, puede deshabilitar la generación automática del archivo. Puede elegir esta opción si tiene varios concentradores y métodos, pero no desea que todos los usuarios sean conscientes de todos los métodos. Deshabilite la generación automática estableciendo **EnableJavaScriptProxies** en **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Para obtener más información acerca de los archivos proxy de JavaScript, consulte [el proxy generado y lo que hace por usted.](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) <a id="exceptions"></a>

### <a name="exceptions"></a>Excepciones

Debe evitar pasar objetos de excepción a los clientes porque los objetos pueden exponer información confidencial a los clientes. En su lugar, llame a un método en el cliente que muestre el mensaje de error relevante.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
