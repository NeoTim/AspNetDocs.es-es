---
title: Módulos de IIS con ASP.NET Core
author: guardrex
description: Descubra módulos activos e inactivos de IIS para aplicaciones ASP.NET Core y cómo administrar módulos de IIS.
ms.author: riande
ms.custom: mvc
ms.date: 01/17/2019
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 8c32a668b3945f0da0194162e19e965b4aed3934
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043902"
---
# <a name="iis-modules-with-aspnet-core"></a>Módulos de IIS con ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Algunos de los módulos nativos de IIS y todos los módulos administrados de IIS no pueden procesar las solicitudes para las aplicaciones ASP.NET Core. En muchos casos, ASP.NET Core ofrece una alternativa a los escenarios abordados por los módulos nativos y administrados de IIS.

## <a name="native-modules"></a>Módulos nativos

En la tabla se indican los módulos nativos de IIS que son funcionales con aplicaciones ASP.NET Core y el módulo ASP.NET Core.

| Module | Funcional con aplicaciones ASP.NET Core | Opción de ASP.NET Core |
| --- | :---: | --- |
| **Autenticación anónima**<br>`AnonymousAuthenticationModule`                                  | Sí | |
| **Autenticación básica**<br>`BasicAuthenticationModule`                                          | Sí | |
| **Autenticación de asignaciones de certificados de cliente**<br>`CertificateMappingAuthenticationModule`      | Sí | |
| **CGI**<br>`CgiModule`                                                                           | No  | |
| **Validación de configuración**<br>`ConfigurationValidationModule`                                  | Sí | |
| **Errores HTTP**<br>`CustomErrorModule`                                                           | No  | [Middleware de páginas de códigos de estado](xref:fundamentals/error-handling#configure-status-code-pages) |
| **Registro personalizado**<br>`CustomLoggingModule`                                                      | Sí | |
| **Documento predeterminado**<br>`DefaultDocumentModule`                                                  | No  | [Middleware de archivos predeterminados](xref:fundamentals/static-files#serve-a-default-document) |
| **Autenticación implícita**<br>`DigestAuthenticationModule`                                        | Sí | |
| **Examen de directorios**<br>`DirectoryListingModule`                                               | No  | [Middleware de exploración de directorios](xref:fundamentals/static-files#enable-directory-browsing) |
| **Compresión dinámica**<br>`DynamicCompressionModule`                                            | Sí | [Middleware de compresión de respuestas](xref:performance/response-compression) |
| **Traza**<br>`FailedRequestsTracingModule`                                                     | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging/index#tracesource-provider) |
| **Almacenamiento en caché de archivos**<br>`FileCacheModule`                                                            | No  | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) |
| **Almacenamiento en caché HTTP**<br>`HttpCacheModule`                                                            | No  | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) |
| **Registro HTTP**<br>`HttpLoggingModule`                                                          | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging/index) |
| **Redireccionamiento HTTP**<br>`HttpRedirectionModule`                                                  | Sí | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) |
| **Autenticación de asignaciones de certificados de cliente IIS**<br>`IISCertificateMappingAuthenticationModule` | Sí | |
| **Restricciones de IP y dominio**<br>`IpRestrictionModule`                                          | Sí | |
| **Filtros ISAPI**<br>`IsapiFilterModule`                                                         | Sí | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule`                                                                       | Sí | [Middleware](xref:fundamentals/middleware/index) |
| **Compatibilidad con el protocolo**<br>`ProtocolSupportModule`                                                  | Sí | |
| **Filtrado de solicitudes**<br>`RequestFilteringModule`                                                | Sí | [Middleware de reescritura de dirección URL`IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Supervisor de solicitudes**<br>`RequestMonitorModule`                                                    | Sí | |
| **Reescritura de dirección URL**&#8224;<br>`RewriteModule`                                                      | Sí | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) |
| **Inclusiones del lado servidor**<br>`ServerSideIncludeModule`                                            | No  | |
| **Compresión estática**<br>`StaticCompressionModule`                                              | No  | [Middleware de compresión de respuestas](xref:performance/response-compression) |
| **Contenido estático**<br>`StaticFileModule`                                                         | No  | [Middleware de archivos estáticos](xref:fundamentals/static-files) |
| **Almacenamiento en caché de tokens**.<br>`TokenCacheModule`                                                          | Sí | |
| **Almacenamiento en caché de URI**<br>`UriCacheModule`                                                              | Sí | |
| **Autorización de URL**<br>`UrlAuthorizationModule`                                                | Sí | [Identidad de ASP.NET Core](xref:security/authentication/identity) |
| **Autenticación de Windows**<br>`WindowsAuthenticationModule`                                      | Sí | |

&#8224;Los tipos de coincidencia `isFile` y `isDirectory` del módulo de reescritura de direcciones URL no funcionan con las aplicaciones ASP.NET Core debido a los cambios en la [estructura de directorios](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Módulos administrados

Los módulos administrados *no* son funcionales cuando la versión de CLR de .NET del grupo de aplicaciones se establece en **No Managed Code** (Sin código administrado). ASP.NET Core ofrece alternativas de middleware en varios casos.

| Module                  | Opción de ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Middleware de autenticación de cookies](xref:security/authentication/cookie) |
| OutputCache             | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) |
| Perfil                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Sesión                 | [Middleware de sesión](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule 4.0    | [Identidad de ASP.NET Core](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Cambios en la aplicación del Administrador de IIS

Al usar el Administrador de IIS para realizar la configuración, cambia el archivo *web.config* de la aplicación. Si implementa una aplicación e incluye *web.config*, los cambios realizados con el Administrador de IIS se sobrescriben con el archivo *web.config* implementado. Si se realizan cambios en el archivo *web.config* del servidor, copie el archivo *web.config* actualizado en el servidor en el proyecto local inmediatamente.

## <a name="disabling-iis-modules"></a>Deshabilitación de los módulos de IIS

Si un módulo de IIS configurado en el nivel de servidor debe deshabilitarse en una aplicación, la adición del archivo *web.config* de la aplicación puede deshabilitar el módulo. Puede dejar el módulo en su sitio y desactivarlo mediante un valor de configuración (si está disponible), o quitar el módulo de la aplicación.

### <a name="module-deactivation"></a>Desactivación de módulos

Muchos módulos ofrecen un valor de configuración que les permite deshabilitarse sin quitar el módulo de la aplicación. Esta es la manera más sencilla y rápida de desactivar un módulo. Por ejemplo, el módulo de redireccionamiento de HTTP se puede deshabilitar con el elemento `<httpRedirect>` en *web.config*:

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Para más información sobre la deshabilitación de los módulos con valores de configuración, siga los vínculos de la sección sobre *elementos secundarios* de [IIS \<system.webServer>](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Eliminación de módulos

Si opta por quitar un módulo con un valor de configuración en *web.config*, desbloquee primero el módulo y la sección `<modules>` de *web.config*:

1. Desbloquee el módulo en el nivel de servidor. Seleccione el servidor de IIS en la barra lateral **Conexiones** del Administrador de IIS. Abra **Módulos** en el área **IIS**. Seleccione el módulo de la lista. En la barra lateral **Acciones** e la derecha, seleccione **Desbloquear**. Si la entrada de acción para el módulo aparece como **Bloqueo**, el módulo ya está desbloqueado y no se requiere ninguna acción. Desbloquee tantos módulos como quiera quitar de *web.config* más tarde.

2. Implemente la aplicación sin una sección `<modules>` en *web.config*. Si una aplicación se implementa con un archivo *web.config* que contiene la sección `<modules>` sin haber desbloqueado primero la sección en el Administrador de IIS, Configuration Manager produce una excepción al intentar desbloquear la sección. Por lo tanto, implemente la aplicación sin una sección `<modules>`.

3. Desbloquee la sección `<modules>` de *web.config*. En la barra lateral **Conexiones**, seleccione el sitio web en **Sitios**. En el área **Administración**, abra el **error de configuración**. Use los controles de navegación para seleccionar la sección `system.webServer/modules`. En la barra lateral **Acciones** de la derecha, seleccione **Desbloquear** la sección. Si la entrada de acción para la sección del módulo aparece como **Sección de bloqueo**, la sección del módulo ya está desbloqueada y no se requiere ninguna acción.

4. Agregue una sección `<modules>` al archivo local *web.config* de la aplicación con un elemento `<remove>` para quitar el módulo de la aplicación. Agregue varios elementos `<remove>` para quitar varios módulos. Si se realizan cambios en *web.config* en el servidor, realice inmediatamente los mismos cambios en el archivo *web.config* el proyecto de forma local. Quitar un módulo según este enfoque no afecta al uso del módulo con otras aplicaciones del servidor.

   ```xml
   <configuration>
    <system.webServer>
      <modules>
        <remove name="MODULE_NAME" />
      </modules>
    </system.webServer>
   </configuration>
   ```
   
Para agregar o quitar módulos para IIS Express mediante *web.config*, modifique *applicationHost.config* para desbloquear la sección `<modules>`:

1. Abra *{APPLICATION ROOT}\\.vs\config\applicationhost.config*.

1. Localice el elemento `<section>` para los módulos de IIS y cambie `overrideModeDefault` de `Deny` a `Allow`:

   ```xml
   <section name="modules" 
            allowDefinition="MachineToApplication" 
            overrideModeDefault="Allow" />
   ```
   
1. Localice la sección `<location path="" overrideMode="Allow"><system.webServer><modules>`. Para los módulos que desee quitar, establezca `lockItem` entre `true` y `false`. En el ejemplo siguiente, se desbloquea el módulo CGI:

   ```xml
   <add name="CgiModule" lockItem="false" />
   ```
   
1. Una vez la sección `<modules>` y los módulos individuales se desbloquean, tiene la opción de agregar o quitar los módulos de IIS mediante el archivo *web.config* de la aplicación para ejecutar la aplicación en IIS Express.

Un módulo de IIS también se puede quitar con *Appcmd.exe*. Proporcione `MODULE_NAME` y `APPLICATION_NAME` en el comando:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Por ejemplo, quite `DynamicCompressionModule` del sitio web predeterminado:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configuración mínima del módulo

Los únicos módulos necesarios para ejecutar una aplicación ASP.NET Core son el Módulo de autenticación anónima y el Módulo ASP.NET Core.

El Módulo de almacenamiento en caché de URI (`UriCacheModule`) permite a IIS almacenar en caché la configuración del sitio web en el nivel de dirección URL. Sin este módulo, IIS debe leer y analizar la configuración en cada solicitud, incluso cuando se solicita de forma repetida la misma dirección URL. Como consecuencia de ello, el rendimiento se reduce considerablemente. *Aunque el Módulo de almacenamiento en caché de URI no es estrictamente necesario para ejecutar una aplicación ASP.NET Core hospedada, es recomendable habilitarlo en todas las implementaciones de ASP.NET Core.*

El Módulo de almacenamiento en caché de HTTP (`HttpCacheModule`) implementa la caché de resultados de IIS y también la lógica de almacenamiento en caché de los elementos de la caché de HTTP.sys. Sin este módulo, el contenido ya no se almacena en caché en modo kernel y los perfiles de caché se pasan por alto. Quitar el Módulo de almacenamiento en caché de HTTP normalmente tiene efectos negativos sobre el rendimiento y el uso de los recursos. *Aunque el Módulo de almacenamiento en caché de HTTP no es estrictamente necesario para ejecutar una aplicación ASP.NET Core hospedada, es recomendable habilitarlo en todas las implementaciones de ASP.NET Core.*

## <a name="additional-resources"></a>Recursos adicionales

* <xref:host-and-deploy/iis/index>
* [Introduction to IIS Architectures: Modules in IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis) (Introducción a las arquitecturas de IIS: módulos de IIS)
* [IIS Modules Overview](/iis/get-started/introduction-to-iis/iis-modules-overview) (Introducción a los módulos de IIS)
* [Customizing IIS 7.0 Roles and Modules](https://technet.microsoft.com/library/cc627313.aspx) (Personalización de los roles y módulos de IIS 7.0)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
