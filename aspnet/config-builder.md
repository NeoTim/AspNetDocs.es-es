---
uid: config-builder
title: Generadores de configuración de ASP.NET
author: rick-anderson
description: Obtenga información sobre cómo obtener datos de configuración de orígenes que no sean web.config valores de orígenes externos.
ms.author: riande
ms.date: 7/17/2020
msc.type: content
ms.openlocfilehash: 1f95efcceb2ecf33fece12174cecf65cd8b27675
ms.sourcegitcommit: 000cbcd1de66fe8a766f203ef2d6f009e4435f53
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2020
ms.locfileid: "86424805"
---
# <a name="configuration-builders-for-aspnet"></a>Generadores de configuración de ASP.NET

Por [Stephen Molloy](https://github.com/StephenMolloy) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Los generadores de configuración proporcionan un mecanismo moderno y ágil para que las aplicaciones de ASP.NET obtengan valores de configuración de orígenes externos.

Generadores de configuración:

* Están disponibles en .NET Framework 4.7.1 y versiones posteriores.
* Proporcionar un mecanismo flexible para leer los valores de configuración.
* Solucione algunas de las necesidades básicas de las aplicaciones cuando se mueven a un contenedor y a un entorno centrado en la nube.
* Se puede usar para mejorar la protección de los datos de configuración mediante el dibujo de orígenes no disponibles anteriormente (por ejemplo, Azure Key Vault y variables de entorno) en el sistema de configuración de .NET.

## <a name="keyvalue-configuration-builders"></a>Generadores de configuración de clave/valor

Un escenario común que pueden controlar los generadores de configuraciones es proporcionar un mecanismo básico de sustitución de clave-valor para las secciones de configuración que siguen un patrón de clave y valor. El concepto .NET Framework de ConfigurationBuilders no se limita a secciones o patrones de configuración específicos. Sin embargo, muchos de los generadores de configuración de `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionan dentro del patrón de clave-valor.

## <a name="keyvalue-configuration-builders-settings"></a>Configuración de los generadores de configuración de clave/valor

La siguiente configuración se aplica a todos los generadores de configuración de clave y valor en `Microsoft.Configuration.ConfigurationBuilders` .

### <a name="mode"></a>Mode

Los generadores de configuración usan un origen externo de la información de clave y valor para rellenar los elementos de clave y valor seleccionados del sistema de configuración. En concreto, `<appSettings/>` las `<connectionStrings/>` secciones y reciben un tratamiento especial de los generadores de configuración. Los generadores funcionan en tres modos:

* `Strict`: El modo predeterminado. En este modo, el generador de configuración solo funciona en secciones de configuración con clave y valor bien conocidas. `Strict`el modo enumera cada clave de la sección. Si se encuentra una clave coincidente en el origen externo:

   * Los generadores de configuración reemplazan el valor de la sección de configuración resultante por el valor del origen externo.
* `Greedy`: Este modo está estrechamente relacionado con el `Strict` modo. En lugar de limitarse a las claves que ya existen en la configuración original:

  * Los generadores de configuración agregan todos los pares clave-valor del origen externo a la sección de configuración resultante.

* `Expand`: Opera en el XML sin formato antes de analizarlo en un objeto de sección de configuración. Puede considerarse como una expansión de tokens en una cadena. Cualquier parte de la cadena XML sin formato que coincida con el patrón `${token}` es candidata para la expansión de tokens. Si no se encuentra ningún valor correspondiente en el origen externo, no se cambia el token. Los generadores de este modo no se limitan a `<appSettings/>` las `<connectionStrings/>` secciones y.

El siguiente marcado de *web.config* habilita [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en `Strict` modo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

En el código siguiente `<appSettings/>` se lee y `<connectionStrings/>` se muestra en el archivo de *web.config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

El código anterior establecerá los valores de propiedad en:

* Valores del archivo de *web.config* si las claves no se establecen en variables de entorno.
* Los valores de la variable de entorno, si se establece.

Por ejemplo, contendrá `ServiceID` :

* "Valor de ServiceID desde web.config", si no se establece la variable de entorno `ServiceID` .
* El valor de la `ServiceID` variable de entorno, si se establece.

En la imagen siguiente se muestran las `<appSettings/>` claves o los valores del conjunto de archivos *web.config* anterior en el editor de entorno:

![Editor de entorno](config-builder/static/env.png)

Nota: es posible que tenga que salir y reiniciar Visual Studio para ver los cambios en las variables de entorno.

### <a name="prefix-handling"></a>Control de prefijos

Los prefijos de clave pueden simplificar la configuración de claves porque:

* La configuración .NET Framework es compleja y está anidada.
* Los orígenes de clave/valor externos suelen ser básicos y planos por naturaleza. Por ejemplo, las variables de entorno no están anidadas.

Use cualquiera de los métodos siguientes para insertar `<appSettings/>` y `<connectionStrings/>` en la configuración mediante variables de entorno:

* Con `EnvironmentConfigBuilder` en el modo predeterminado `Strict` y los nombres de clave adecuados en el archivo de configuración. El código y el marcado anteriores tienen este enfoque. Con este enfoque **no** puede tener claves con el mismo nombre en `<appSettings/>` y `<connectionStrings/>` .
* Use dos `EnvironmentConfigBuilder` s en `Greedy` modo con prefijos distintos y `stripPrefix` . Con este enfoque, la aplicación puede leer `<appSettings/>` y `<connectionStrings/>` sin necesidad de actualizar el archivo de configuración. En la sección siguiente, [stripPrefix](#stripprefix), se muestra cómo hacerlo.
* Use dos `EnvironmentConfigBuilder` s en `Greedy` modo con prefijos distintos. Con este enfoque no puede tener nombres de clave duplicados, ya que los nombres de clave deben ser diferentes en el prefijo.  Por ejemplo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Con el marcado anterior, se puede usar el mismo origen de clave y valor sin formato para rellenar la configuración de dos secciones diferentes.

En la imagen siguiente se muestran las `<appSettings/>` `<connectionStrings/>` claves y los valores del conjunto de archivos *web.config* anterior en el editor de entorno:

![Editor de entorno](config-builder/static/prefix.png)

El código siguiente lee las `<appSettings/>` `<connectionStrings/>` claves y los valores que contiene el archivo de *web.config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

El código anterior establecerá los valores de propiedad en:

* Valores del archivo de *web.config* si las claves no se establecen en variables de entorno.
* Los valores de la variable de entorno, si se establece.

Por ejemplo, con el archivo de *web.config* anterior, las claves/valores de la imagen del editor del entorno anterior y el código anterior, se establecen los valores siguientes:

|  Clave              | Valor |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID de variables de env|
|    AppSetting_default            | AppSetting_default valor de env |
|       ConnStr_default         | ConnStr_default Val from env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: booleano, su valor predeterminado es `false` . 

El marcado XML anterior separa la configuración de la aplicación de las cadenas de conexión, pero requiere que todas las claves del archivo *web.config* usen el prefijo especificado. Por ejemplo, el prefijo `AppSetting` debe agregarse a la `ServiceID` clave ("AppSetting_ServiceID"). Con `stripPrefix` , el prefijo no se utiliza en el archivo de *web.config* . El prefijo es necesario en el origen del generador de configuraciones (por ejemplo, en el entorno). Se prevé que la mayoría de los desarrolladores usarán `stripPrefix` .

Normalmente, las aplicaciones quitan el prefijo. En el siguiente *web.config* se quita el prefijo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

En el archivo de *web.config* anterior, la `default` clave está en `<appSettings/>` y `<connectionStrings/>` .

En la imagen siguiente se muestran las `<appSettings/>` `<connectionStrings/>` claves y los valores del conjunto de archivos *web.config* anterior en el editor de entorno:

![Editor de entorno](config-builder/static/prefix.png)

El código siguiente lee las `<appSettings/>` `<connectionStrings/>` claves y los valores que contiene el archivo de *web.config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

El código anterior establecerá los valores de propiedad en:

* Valores del archivo de *web.config* si las claves no se establecen en variables de entorno.
* Los valores de la variable de entorno, si se establece.

Por ejemplo, con el archivo de *web.config* anterior, las claves/valores de la imagen del editor del entorno anterior y el código anterior, se establecen los valores siguientes:

|  Clave              | Valor |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID de variables de env|
|    default            | AppSetting_default valor de env |
|    default         | ConnStr_default Val from env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: String, el valor predeterminado es.`@"\$\{(\w+)\}"`

El `Expand` comportamiento de los generadores busca en el XML sin formato los tokens que se parezcan a `${token}` . La búsqueda se realiza con la expresión regular predeterminada `@"\$\{(\w+)\}"` . El conjunto de caracteres que coincide `\w` es más estricto que XML y muchos orígenes de configuración permiten. Use `tokenPattern` cuando haya más caracteres de `@"\$\{(\w+)\}"` los necesarios en el nombre del token.

`tokenPattern`String@

* Permite a los desarrolladores cambiar la expresión regular que se usa para la coincidencia de tokens.
* No se realiza ninguna validación para asegurarse de que se trata de una expresión regular correcta y no peligrosa.
* Debe contener un grupo de capturas. La expresión regular completa debe coincidir con el token completo. La primera captura debe ser el nombre del token que se va a buscar en el origen de configuración.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Generadores de configuración en Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Es la más sencilla de los generadores de configuración.
* Lee los valores del entorno.
* No tiene ninguna opción de configuración adicional.
* El `name` valor del atributo es arbitrario.

**Nota:** En un entorno de contenedor de Windows, las variables que se establecen en tiempo de ejecución solo se insertan en el entorno de proceso EntryPoint. Las aplicaciones que se ejecutan como un servicio o un proceso sin punto de entrada no recogen estas variables a menos que se inserten de otro modo a través de un mecanismo del contenedor. En [IIS](https://github.com/Microsoft/iis-docker/pull/41) / el caso de los contenedores basados en[ASP.net](https://github.com/Microsoft/aspnet-docker)de IIS, la versión actual de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) lo controla solo en *DefaultAppPool* . Otras variantes de contenedor basadas en Windows pueden necesitar desarrollar su propio mecanismo de inyección para los procesos que no son de punto de entrada.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nunca almacene contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente. Los secretos de producción no deben usarse para el desarrollo o las pruebas.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Este generador de configuración proporciona una característica similar a [ASP.net Core administrador de secretos](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) se puede usar en proyectos de .NET Framework, pero debe especificarse un archivo de secretos. Como alternativa, puede definir la `UserSecretsId` propiedad en el archivo de proyecto y crear el archivo de secretos sin formato en la ubicación correcta para la lectura. Para mantener las dependencias externas fuera del proyecto, el archivo secreto tiene un formato XML. El formato XML es un detalle de implementación y no se debe confiar en el formato. Si necesita compartir un *secrets.jsen* el archivo con proyectos de .net Core, considere la posibilidad de usar [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). El `SimpleJsonConfigBuilder` formato de .net Core también debe considerarse un detalle de implementación sujeto a cambios.

Atributos de configuración para `UserSecretsConfigBuilder` :

* `userSecretsId`: Es el método preferido para identificar un archivo de secretos XML. Funciona de forma similar a .NET Core, que usa una `UserSecretsId` propiedad de proyecto para almacenar este identificador. La cadena debe ser única, por lo que no es necesario que sea un GUID. Con este atributo, `UserSecretsConfigBuilder` busca en una ubicación local conocida ( `%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml` ) un archivo de secretos que pertenezca a este identificador.
* `userSecretsFile`: Un atributo opcional que especifica el archivo que contiene los secretos. El `~` carácter se puede usar en el inicio para hacer referencia a la raíz de la aplicación. Este atributo o el `userSecretsId` atributo son obligatorios. Si se especifican ambos, `userSecretsFile` tiene prioridad.
* `optional`: booleano, valor predeterminado `true` : impide una excepción si no se encuentra el archivo de secretos. 
* El `name` valor del atributo es arbitrario.

El archivo de secretos tiene el siguiente formato:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) Lee los valores almacenados en el [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName`es obligatorio (el nombre del almacén o un URI para el almacén). Los otros atributos permiten controlar el almacén al que se va a conectar, pero solo son necesarios si la aplicación no se está ejecutando en un entorno que funcione con `Microsoft.Azure.Services.AppAuthentication` . La biblioteca de autenticación de servicios de Azure se usa para seleccionar automáticamente la información de conexión del entorno de ejecución si es posible. Puede invalidar la recogida automática de información de conexión proporcionando una cadena de conexión.

* `vaultName`-Requerido si `uri` no se proporciona. Especifica el nombre del almacén en la suscripción de Azure desde el que se van a leer pares de clave y valor.
* `connectionString`: Una cadena de conexión que puede usar [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri`: Se conecta a otros proveedores de Key Vault con el `uri` valor especificado. Si no se especifica, Azure ( `vaultName` ) es el proveedor de almacén.
* `version`-Azure Key Vault proporciona una característica de control de versiones para secretos. Si `version` se especifica, el generador solo recupera los secretos que coinciden con esta versión.
* `preloadSecretNames`: De forma predeterminada, este generador consulta **todos** los nombres de clave en el almacén de claves cuando se inicializa. Para evitar la lectura de todos los valores de clave, establezca este atributo en `false` . Si se establece en, se `false` leerán los secretos de uno en uno. Leer secretos de uno en uno puede ser útil si el almacén permite el acceso "Get" pero no "list". **Nota:** Cuando `Greedy` se usa el modo, `preloadSecretNames` debe ser `true` (el valor predeterminado).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) es un generador de configuración básico que usa los archivos de un directorio como origen de valores. El nombre de un archivo es la clave y el contenido es el valor. Este generador de configuración puede ser útil cuando se ejecuta en un entorno de contenedor organizado. Los sistemas como Docker Swarm y Kubernetes proporcionan `secrets` a sus contenedores de Windows orquestados en función de la clave por archivo.

Detalles del atributo:

* `directoryPath` - Requerido. Especifica una ruta de acceso en la que buscar valores. Docker para Windows secretos se almacenan de forma predeterminada en el directorio *C:\ProgramData\Docker\secrets*
* `ignorePrefix`-Se excluyen los archivos que comienzan con este prefijo. Se establece en "ignore." de forma predeterminada.
* `keyDelimiter`-El valor predeterminado es `null` . Si se especifica, el generador de configuración atraviesa varios niveles del directorio y crea nombres de clave con este delimitador. Si este valor es `null` , el generador de configuración solo busca en el nivel superior del directorio.
* `optional`-El valor predeterminado es `false` . Especifica si el generador de configuración debe producir errores si el directorio de origen no existe.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nunca almacene contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente. Los secretos de producción no deben usarse para el desarrollo o las pruebas.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Los proyectos de .NET Core usan frecuentemente archivos JSON para la configuración. El generador de [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) permite usar archivos JSON de .net Core en el .NET Framework. Este generador de configuración proporciona una asignación básica desde un origen de valor o clave sin formato en áreas de clave y valor específicas de la configuración de .NET Framework. Este generador de configuración **no** proporciona configuraciones jerárquicas. El archivo de copia de seguridad JSON es similar a un diccionario, no a un objeto jerárquico complejo. Se puede usar un archivo jerárquico de varios niveles. Este proveedor `flatten` es la profundidad anexando el nombre de la propiedad en cada nivel usando `:` como un delimitador.

Detalles del atributo:

* `jsonFile` - Requerido. Especifica el archivo JSON del que se va a leer. El `~` carácter se puede usar en el inicio para hacer referencia a la raíz de la aplicación.
* `optional`-Boolean, el valor predeterminado es `true` . Evita producir excepciones si no se encuentra el archivo JSON.
* `jsonMode` - `[Flat|Sectional]`. `Flat` es el valor predeterminado. Cuando `jsonMode` es `Flat` , el archivo JSON es un origen de valor o clave plana único. `EnvironmentConfigBuilder`Y `AzureKeyVaultConfigBuilder` también son orígenes de clave/valor sin formato. Cuando `SimpleJsonConfigBuilder` se configura en el `Sectional` modo:

  * El archivo JSON se divide conceptualmente solo en el nivel superior en varios diccionarios.
  * Cada uno de los diccionarios solo se aplica a la sección de configuración que coincide con el nombre de la propiedad de nivel superior asociado a ellos. Por ejemplo:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="configuration-builders-order"></a>Orden de los generadores de configuración

Consulte [orden de ejecución de ConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) en el repositorio de github de [ASPNET/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) .

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementar un generador de configuración de clave/valor personalizado

Si los generadores de configuración no satisfacen sus necesidades, puede escribir uno personalizado. La `KeyValueConfigBuilder` clase base controla los modos de sustitución y la mayoría de los prefijos. Un proyecto de implementación solo necesita:

* Herede de la clase base e implemente un origen básico de pares clave-valor a través de `GetValue` y `GetAllValues` :
* Agregue el [Microsoft.Configuration.ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al proyecto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

La `KeyValueConfigBuilder` clase base proporciona gran parte del comportamiento coherente y de trabajo en los generadores de configuración de clave/valor.

## <a name="additional-resources"></a>Recursos adicionales

* [Repositorio de los generadores de configuración de GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticación entre servicios en Azure Key Vault mediante .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
