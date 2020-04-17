---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Perfiles, temas y elementos web ? Microsoft Docs
author: rick-anderson
description: Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 4bc98cca226a0bd9bd766a21e88b0facf2a4b610
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543644"
---
# <a name="profiles-themes-and-web-parts"></a>Perfiles, temas y elementos web

por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración mediante programación. Además, existen muchas opciones de configuración nuevas que permiten nuevas configuraciones e instrumentación.

ASP.NET 2.0 representa una mejora sustancial en el área de sitios web personalizados. Además de las características de pertenencia que ya hemos cubierto, ASP.NET perfiles, temas y elementos web mejoran significativamente la personalización en los sitios web.

## <a name="aspnet-profiles"></a>Perfiles ASP.NET

ASP.NET perfiles son similares a las sesiones. La diferencia es que un perfil es persistente mientras que una sesión se pierde cuando se cierra el navegador. Otra gran diferencia entre sesiones y perfiles es que los perfiles están fuertemente tipados, por lo que le proporciona IntelliSense durante el proceso de desarrollo.

Un perfil se define en el archivo de configuración de máquinas o en el archivo web.config de la aplicación. (No se puede definir un perfil en un archivo web.config de subcarpetas.) El código siguiente define un perfil para almacenar el nombre y apellido de los visitantes del sitio web.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

El tipo de datos predeterminado para una propiedad de perfil es System.String. En el ejemplo anterior, no se especificó ningún tipo de datos. Por lo tanto, las propiedades FirstName y LastName son de tipo String. Como se mencionó anteriormente, las propiedades de perfil están fuertemente tipadas. El código siguiente agrega una nueva propiedad para la edad que es de tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Los perfiles se usan generalmente con la autenticación de formularios ASP.NET. Cuando se usa en combinación con la autenticación de formularios, cada usuario tiene un perfil independiente asociado con su ID de usuario. Sin embargo, también es posible permitir el uso &lt;de&gt; perfiles en una aplicación anónima utilizando el elemento anonymousIdentification en el archivo de configuración junto con el atributo **allowAnonymous** como se muestra a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Cuando un usuario anónimo examina el sitio, ASP.NET crea una instancia de **ProfileCommon** para el usuario. Este perfil utiliza un ID único almacenado en una cookie en el navegador para identificar al usuario como un visitante único. De esta manera, puede almacenar información de perfil para los usuarios que navegan de forma anónima.

## <a name="profile-groups"></a>Grupos de perfiles

Es posible agrupar propiedades de perfiles. Mediante la agrupación de propiedades, es posible simular varios perfiles para una aplicación específica.

La siguiente configuración configura una propiedad FirstName y LastName para dos grupos; Compradores y prospectos.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

A continuación, es posible establecer propiedades en un grupo determinado de la siguiente manera:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Almacenamiento de objetos complejos

Hasta ahora, los ejemplos que hemos cubierto han almacenado tipos de datos simples en un perfil. También es posible almacenar tipos de datos complejos en un perfil especificando el método de serialización mediante el atributo **serializeAs** de la siguiente manera:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

En este caso, el tipo es PurchaseInvoice. La clase PurchaseInvoice debe marcarse como serializable y puede contener cualquier número de propiedades. Por ejemplo, si PurchaseInvoice tiene una propiedad denominada **NumItemsPurchased**, puede hacer referencia a esa propiedad en el código de la siguiente manera:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Herencia de perfil

Es posible crear un perfil para su uso en múltiples aplicaciones. Mediante la creación de una clase de perfil que deriva de ProfileBase, puede reutilizar un perfil en varias aplicaciones mediante el atributo **inherits** como se muestra a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

En este caso, la clase **PurchasingProfile** tendría ese aspecto:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Proveedores de perfiles

ASP.NET perfiles utilizan el modelo de proveedor. El proveedor predeterminado almacena la información en una\_base de datos de SQL Server Express en la carpeta Datos de aplicación de la aplicación web mediante el proveedor SqlProfileProvider. Si la base de datos no existe, ASP.NET la creará automáticamente cuando el perfil intente almacenar información.

En algunos casos, sin embargo, es posible que desee desarrollar su propio proveedor de perfiles. La función de perfil ASP.NET le permite utilizar fácilmente diferentes proveedores.

Puede crear un proveedor de perfiles personalizado cuando:

- Debe almacenar información de perfil en un origen de datos, como en una base de datos FoxPro o en una base de datos de Oracle, que no es compatible con los proveedores de perfiles incluidos con .NET Framework.
- Debe administrar la información de perfil mediante un esquema de base de datos que es diferente del esquema de base de datos utilizado por los proveedores incluidos con .NET Framework. Un ejemplo común es que desea integrar la información de perfil con los datos de usuario en una base de datos de SQL Server existente.

### <a name="required-classes"></a>Clases requeridas

Para implementar un proveedor de perfiles, cree una clase que herede la clase abstracta System.Web.Profile.ProfileProvider. La clase abstracta **ProfileProvider** a su vez hereda la clase abstracta System.Configuration.SettingsProvider, que hereda la clase abstracta System.Configuration.Provider.ProviderBase. Debido a esta cadena de herencia, además de los miembros necesarios de la **ProfileProvider** clase, debe implementar los miembros necesarios de la **SettingsProvider** y **ProviderBase** clases.

En las tablas siguientes se describen las propiedades y métodos que debe implementar desde las clases abstractas **ProviderBase**, **SettingsProvider**y **ProfileProvider** .

### <a name="providerbase-members"></a>Miembros de ProviderBase

| **Member** | **Descripción** |
| --- | --- |
| Initialize (método) | Toma como entrada el nombre de la instancia del proveedor y un NameValueCollection de valores de configuración. Se utiliza para establecer opciones y valores de propiedad para la instancia del proveedor, incluidos los valores específicos de la implementación y las opciones especificadas en la configuración del equipo o el archivo Web.config. |

### <a name="settingsprovider-members"></a>SettingsProvider Miembros

| **Member** | **Descripción** |
| --- | --- |
| Propiedad ApplicationName | El nombre de la aplicación que se almacena con cada perfil. El proveedor de perfiles utiliza el nombre de la aplicación para almacenar la información de perfil por separado para cada aplicación. Esto permite que varias aplicaciones ASP.NET usen el mismo origen de datos sin conflicto si se crea el mismo nombre de usuario en aplicaciones diferentes. Como alternativa, varias aplicaciones de ASP.NET pueden compartir un origen de datos de perfil especificando el mismo nombre de aplicación. |
| GetPropertyValues método | Toma como entrada un SettingsContext y un SettingsPropertyCollection objeto. **SettingsContext** proporciona información sobre el usuario. Puede usar la información como clave principal para recuperar información de propiedad de perfil para el usuario. Utilice el objeto **SettingsContext** para obtener el nombre de usuario y si el usuario está autenticado o es anónimo. El **SettingsPropertyCollection** contiene una colección de SettingsProperty objetos. Cada objeto **SettingsProperty** proporciona el nombre y el tipo de la propiedad, así como información adicional, como el valor predeterminado de la propiedad y si la propiedad es de solo lectura. El **GetPropertyValues** método rellena un SettingsPropertyValueCollection con SettingsPropertyValue objetos basados en el **SettingsProperty** objetos proporcionados como entrada. Los valores del origen de datos para el usuario especificado se asignan a la PropertyValue propiedades para cada **SettingsPropertyValue** objeto y se devuelve toda la colección. Llamar al método también actualiza el lastActivityDate valor para el perfil de usuario especificado a la fecha y hora actuales. |
| SetPropertyValues método | Toma como entrada un **SettingsContext** y un **SettingsPropertyValueCollection** objeto. **SettingsContext** proporciona información sobre el usuario. Puede usar la información como clave principal para recuperar información de propiedad de perfil para el usuario. Utilice el objeto **SettingsContext** para obtener el nombre de usuario y si el usuario está autenticado o es anónimo. El **SettingsPropertyValueCollection** contiene una colección de **SettingsPropertyValue** objetos. Cada objeto **SettingsPropertyValue** proporciona el nombre, el tipo y el valor de la propiedad, así como información adicional, como el valor predeterminado de la propiedad y si la propiedad es de solo lectura. El **SetPropertyValues** método actualiza los valores de propiedad de perfil en el origen de datos para el usuario especificado. Llamar al método también actualiza el **LastActivityDate** y LastUpdatedDate valores para el perfil de usuario especificado a la fecha y hora actual. |

### <a name="profileprovider-members"></a>Miembros ProfileProvider

| **Member** | **Descripción** |
| --- | --- |
| DeleteProfiles método | Toma como entrada una matriz de cadenas de nombres de usuario y elimina del origen de datos toda la información de perfil y los valores de propiedad de los nombres especificados donde el nombre de la aplicación coincide con el valor de la propiedad **ApplicationName.** Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| DeleteProfiles método | Toma como entrada una colección de ProfileInfo objetos y elimina del origen de datos toda la información de perfil y valores de propiedad para cada perfil donde el nombre de la aplicación coincide con el **ApplicationName** valor de propiedad. Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| DeleteInactiveProfiles método | Toma como entrada un ProfileAuthenticationOption valor y un DateTime objeto y elimina del origen de datos toda la información de perfil y valores de propiedad donde la última fecha de actividad es menor o igual que la fecha y hora especificadas y donde el nombre de la aplicación coincide con el **ApplicationName** valor de propiedad. El parámetro **ProfileAuthenticationOption** especifica si solo se van a eliminar perfiles anónimos, solo perfiles autenticados o todos los perfiles. Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| GetAllProfiles método | Toma como entrada un **ProfileAuthenticationOption** valor, un entero que especifica el índice de página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un ProfileInfoCollection que contiene **ProfileInfo** objetos para todos los perfiles en el origen de datos donde el nombre de la aplicación coincide con el **ApplicationName** valor de propiedad. El parámetro **ProfileAuthenticationOption** especifica si solo se devolverán perfiles anónimos, solo perfiles autenticados o todos los perfiles. Los resultados devueltos por el **GetAllProfiles** método están restringidos por el índice de página y los valores de tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se devolverán en **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados se va a devolver, donde 1 identifica la primera página. El parámetro para los registros totales es un parámetro out (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando se devuelve el método. |
| GetAllInactiveProfiles método | Toma como entrada un **ProfileAuthenticationOption** valor, un **DateTime** objeto, un entero que especifica el índice de página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un **ProfileInfoCollection** que contiene **ProfileInfo** objetos para todos los perfiles en el origen de datos donde la última fecha de actividad es menor o igual que el **DateTime** especificado y donde el nombre de la aplicación coincide con el **ApplicationName** valor de propiedad. El parámetro **ProfileAuthenticationOption** especifica si solo se devolverán perfiles anónimos, solo perfiles autenticados o todos los perfiles. Los resultados devueltos por el **GetAllInactiveProfiles** método están restringidos por el índice de página y los valores de tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se devolverán en **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados se va a devolver, donde 1 identifica la primera página. El parámetro para los registros totales es un parámetro out (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando se devuelve el método. |
| FindProfilesByUserName (método) | Toma como entrada un **ProfileAuthenticationOption** valor, una cadena que contiene un nombre de usuario, un entero que especifica el índice de página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un **ProfileInfoCollection** que contiene **ProfileInfo** objetos para todos los perfiles en el origen de datos donde el nombre de usuario coincide con el nombre de usuario especificado y donde el nombre de aplicación coincide con el **ApplicationName** valor de propiedad. El parámetro **ProfileAuthenticationOption** especifica si solo se devolverán perfiles anónimos, solo perfiles autenticados o todos los perfiles. Si el origen de datos admite capacidades de búsqueda adicionales, como caracteres comodín, puede proporcionar capacidades de búsqueda más amplias para los nombres de usuario. Los resultados devueltos por el **FindProfilesByUserName** método están restringidos por el índice de página y los valores de tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se devolverán en **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados se va a devolver, donde 1 identifica la primera página. El parámetro para los registros totales es un parámetro out (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando se devuelve el método. |
| Método FindInactiveProfilesByUserName | Toma como entrada un **ProfileAuthenticationOption** valor, una cadena que contiene un nombre de usuario, un **DateTime** objeto, un entero que especifica el índice de página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un **ProfileInfoCollection** que contiene **ProfileInfo** objetos para todos los perfiles en el origen de datos donde el nombre de usuario coincide con el nombre de usuario especificado, donde la última fecha de actividad es menor o igual que el **especificado DateTime**y donde el nombre de aplicación coincide con el **ApplicationName** valor de propiedad. El parámetro **ProfileAuthenticationOption** especifica si solo se devolverán perfiles anónimos, solo perfiles autenticados o todos los perfiles. Si el origen de datos admite capacidades de búsqueda adicionales, como caracteres comodín, puede proporcionar capacidades de búsqueda más amplias para los nombres de usuario. Los resultados devueltos por el **FindInactiveProfilesByUserName** método están restringidos por el índice de página y los valores de tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se devolverán en **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados se va a devolver, donde 1 identifica la primera página. El parámetro para los registros totales es un parámetro out (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando se devuelve el método. |
| GetNumberOfInActiveProfiles (método) | Toma como entrada un **ProfileAuthenticationOption** valor y un **DateTime** objeto y devuelve un recuento de todos los perfiles en el origen de datos donde la última fecha de actividad es menor o igual que el **DateTime** especificado y donde el nombre de la aplicación coincide con el **ApplicationName** valor de propiedad. El parámetro **ProfileAuthenticationOption** especifica si solo se deben contar perfiles anónimos, solo perfiles autenticados o todos los perfiles. |

### <a name="applicationname"></a>ApplicationName

Dado que los proveedores de perfiles almacenan información de perfil por separado para cada aplicación, debe asegurarse de que el esquema de datos incluye el nombre de la aplicación y que las consultas y actualizaciones también incluyen el nombre de la aplicación. Por ejemplo, el siguiente comando se utiliza para recuperar un valor de propiedad de una base de datos basada en el nombre de usuario y si el perfil es anónimo, y garantiza que el valor **ApplicationName** se incluye en la consulta.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>temas ASP.NET

## <a name="what-are-aspnet-20-themes"></a>¿Cuáles son ASP.NET 2.0 Temas?

Uno de los aspectos más importantes de una aplicación web es una apariencia coherente en todo el sitio. ASP.NET desarrolladores de 1.x suelen usar hojas de estilo sen cascada (CSS) para implementar una apariencia coherente. ASP.NET temas 2.0 mejoran significativamente en CSS porque dan al desarrollador de ASP.NET la capacidad de definir la apariencia de ASP.NET controles de servidor, así como elementos HTML. ASP.NET temas se pueden aplicar a controles individuales, una página web específica o una aplicación web completa. Los temas utilizan una combinación de archivos CSS, un archivo de aspecto opcional y un directorio Images opcional si se necesitan imágenes. El archivo de aspecto controla la apariencia visual de ASP.NET controles de servidor.

## <a name="where-are-themes-stored"></a>¿Dónde se almacenan los temas?

La ubicación donde se almacenan los temas difiere en función de su ámbito. Los temas que se pueden aplicar a cualquier aplicación se almacenan en la siguiente carpeta:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema que es específico de una `App\_Themes\<Theme\_Name>` aplicación determinada se almacena en un directorio en la raíz del sitio Web.

> [!NOTE]
> Un archivo de aspecto solo debe modificar las propiedades de control del servidor que afectan a la apariencia.

Un tema global es un tema que se puede aplicar a cualquier aplicación o sitio web que se ejecute en el servidor web. Estos temas se almacenan de forma predeterminada en el directorio ASP.NETClientfiles-Themes que se encuentra dentro del directorio v2.x.xxxxx. Como alternativa, puede mover los archivos de\_tema\_a la carpeta aspnet client/system\_web/[version]/Themes/[thename name] en la raíz de su sitio Web.

Los temas específicos de la aplicación solo se pueden aplicar a la aplicación en la que residen los archivos. Estos archivos se `App\_Themes/<theme\_name>` almacenan en el directorio de la raíz del sitio Web.

## <a name="the-components-of-a-theme"></a>Los componentes de un tema

Un tema se compone de uno o más archivos CSS, un archivo de aspecto opcional y una carpeta Images opcional. Los archivos CSS pueden ser cualquier nombre que desee (es decir, default.css o theme.css, etc.) y deben estar en la raíz de la carpeta de temas. Los archivos CSS se utilizan para definir clases y atributos CSS ordinarios para selectores específicos. Para aplicar una de las clases CSS a un elemento de página, se utiliza la propiedad **CSSClass.**

El archivo de aspecto es un archivo XML que contiene definiciones de propiedades para ASP.NET controles de servidor. El código que se muestra a continuación es un archivo de aspecto de ejemplo.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**La figura 1** a continuación muestra una pequeña página de ASP.NET explorada sin un tema aplicado. **La figura 2** muestra el mismo archivo con un tema aplicado. El color de fondo y el color de texto se configuran a través de un archivo CSS. La apariencia del botón y el cuadro de texto se configuran utilizando el archivo de aspecto mencionado anteriormente.

![Sin tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: Sin tema

![Tema aplicado](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: Tema aplicado

El archivo de aspecto enumerado anteriormente define un aspecto predeterminado para todos los controles TextBox y Button. Esto significa que cada control TextBox y Button insertado en una página asumirá esta apariencia. También puede definir un aspecto que se puede aplicar a instancias específicas de estos controles mediante la propiedad **SkinID** del control.

El código siguiente define un aspecto para un Button control. Solo Button controles con un **SkinID** propiedad de **goButton** asumirá la apariencia del aspecto.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Solo puede tener un aspecto predeterminado por tipo de control de servidor. Si necesita skins adicionales, debe usar la propiedad SkinID.

## <a name="applying-themes-to-pages"></a>Aplicación de temas a páginas

Un tema se puede aplicar utilizando cualquiera de los siguientes métodos:

- En &lt;el&gt; elemento pages del archivo web.config
- En @Page la directiva de una página
- De manera programática

## <a name="applying-a-theme-in-the-configuration-file"></a>Aplicación de un tema en el archivo de configuración

Para aplicar un tema en el archivo de configuración de aplicaciones, utilice la sintaxis siguiente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

El nombre del tema especificado aquí debe coincidir con el nombre de la carpeta de temas. Esta carpeta puede existir en cualquiera de las ubicaciones mencionadas anteriormente en este curso. Si intenta aplicar un tema que no existe, se producirá un error de configuración.

## <a name="applying-a-theme-in-the-page-directive"></a>Aplicación de un tema en la Directiva de páginas

También puede aplicar un tema en la directiva de página. Este método le permite utilizar un tema para una página específica.

Para aplicar un tema @Page en la directiva, utilice la sintaxis siguiente:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Una vez más, el tema especificado aquí debe coincidir con la carpeta del tema como se mencionó anteriormente. Si intenta aplicar un tema que no existe, se producirá un error de compilación. Visual Studio también resaltará el atributo y le notificará que no existe dicho tema.

## <a name="applying-a-theme-programmatically"></a>Aplicación de un tema mediante programación

Para aplicar un tema mediante programación, debe especificar el **Theme** propiedad para la página en el **Page\_PreInit** método.

Para aplicar un tema mediante programación, utilice la sintaxis siguiente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Es necesario aplicar el tema en el método PreInit debido al ciclo de vida de la página. Si lo aplica después de ese punto, el tema de páginas ya habrá sido aplicado por el tiempo de ejecución y un cambio en ese momento es demasiado tarde en el ciclo de vida. Si aplica un tema que no existe, se produce una **excepción HttpException.** Cuando se aplica un tema mediante programación, se producirá una advertencia de compilación si algún control de servidor tiene un SkinID propiedad especificada. Esta advertencia está destinada a informarle de que ningún tema se aplica mediante declaración y se puede omitir.

## <a name="exercise-1--applying-a-theme"></a>Ejercicio 1 : Aplicación de un tema

En este ejercicio, aplicará un tema ASP.NET a un sitio web.

> [!IMPORTANT]
> Si utiliza Microsoft Word para introducir información en un archivo de aspecto, asegúrese de que no está reemplazando comillas regulares por comillas inteligentes. Las citas inteligentes causarán problemas con los archivos de piel.

1. Cree un nuevo sitio web de ASP.NET.
2. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y elija Agregar nuevo elemento.
3. Elija Web Configuration File (Archivo de configuración web) en la lista de archivos y haga clic en Add (Agregar).
4. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y elija Agregar nuevo elemento.
5. Elija Skin File (Archivo de aspecto) y haga clic en Add (Agregar).
6. Haga clic en Sí cuando se le pregunte si\_desea colocar el archivo dentro de la carpeta Temas de la aplicación.
7. Haga clic con el botón derecho\_en la carpeta SkinFile dentro de la carpeta Temas de aplicación en el Explorador de soluciones y elija Agregar nuevo elemento.
8. Elija Hoja de estilos en la lista de archivos y haga clic en Agregar. Ahora tiene todos los archivos necesarios para implementar su nuevo tema. Sin embargo, Visual Studio ha denominado la carpeta de temas SkinFile. Haga clic con el botón derecho en esa carpeta y cambie el nombre a CoolTheme.
9. Abra el archivo SkinFile.skin y agregue el siguiente código al final del archivo: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Guarde el archivo SkinFile.skin.
11. Abra styleSheet.css.
12. Reemplace todo el texto que contiene por lo siguiente: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Guarde el archivo StyleSheet.css.
14. Abra el Default.aspx página.
15. Agregue un control TextBox y un control Button.
16. Guarde la página. Ahora examine el Default.aspx página. Debe mostrarse como un formulario Web normal.
17. Abra el archivo web.config.
18. Agregue lo siguiente directamente debajo de la etiqueta de apertura: `<system.web>` 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Guarde el archivo web.config. Ahora examine el Default.aspx página. Debe mostrarse con el tema aplicado.
20. Si aún no está abierto, abra el Default.aspx página en Visual Studio.
21. Seleccione el botón.
22. Cambie la propiedad **SkinID** a goButton. Tenga en cuenta que Visual Studio proporciona una lista desplegable con valores SkinID válidos para un Button control.
23. Guarde la página. Ahora previsualiza la página en tu navegador de nuevo. El botón ahora debe decir "ir" y debe ser más amplio en apariencia.

Mediante la propiedad **SkinID,** puede configurar fácilmente diferentes aspectos para diferentes instancias de un tipo determinado de control de servidor.

## <a name="the-stylesheettheme-property"></a>La propiedad StyleSheetTheme

Hasta ahora, solo hemos hablado de aplicar temas mediante la propiedad Theme. Cuando se utiliza la propiedad Theme, el archivo de aspecto invalidará cualquier configuración declarativa para los controles de servidor. Por ejemplo, en el ejercicio 1, especificó un SkinID de "goButton" para el Button control y que cambió el texto del botón a "go". Es posible que haya notado que el Text propiedad de la Button en el diseñador se estableció en "Button", pero el tema sobreloró. El tema siempre invalidará cualquier configuración de propiedad en el diseñador.

Si desea poder invalidar las propiedades definidas en el archivo de aspecto del tema con las propiedades especificadas en el diseñador, puede usar la propiedad **StyleSheetTheme** en lugar de la propiedad Theme. El StyleSheetTheme propiedad es la misma que el Theme propiedad excepto que no invalida todos los valores de propiedad explícita como lo hace la propiedad Theme.

Para ver esto en acción, abra el archivo web.config `<pages>` del proyecto en el ejercicio 1 y cambie el elemento a lo siguiente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Ahora examine el Default.aspx página y verá que el Button control tiene un Text propiedad de "Button" de nuevo. Esto se debe a que el valor de la propiedad explícita en el diseñador reemplaza la propiedad Text establecida por goButton SkinID.

## <a name="overriding-themes"></a>Temas anulados

Un tema global se puede invalidar aplicando un tema con\_el mismo nombre en la carpeta Temas de aplicación de la aplicación. Sin embargo, el tema no se aplica en un escenario de invalidación real. Si el tiempo de ejecución encuentra\_archivos de tema en la carpeta Temas de aplicación, aplicará el tema con esos archivos e ignorará el tema global.

La propiedad StyleSheetTheme es reemplazable y se puede invalidar en el código de la siguiente manera:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>elementos web

ASP.NET elementos web es un conjunto integrado de controles para crear sitios web que permiten a los usuarios finales modificar el contenido, la apariencia y el comportamiento de las páginas web directamente desde un explorador. Las modificaciones se pueden aplicar a todos los usuarios del sitio o a usuarios individuales. Cuando los usuarios modifican páginas y controles, la configuración se puede guardar para conservar las preferencias personales de un usuario en futuras sesiones del navegador, una característica denominada personalización. Estas capacidades de elementos web significan que los desarrolladores pueden capacitar a los usuarios finales para personalizar una aplicación web dinámicamente, sin intervención del desarrollador o del administrador.

Mediante el conjunto de controles de elementos web, como desarrollador puede permitir a los usuarios finales:

- Personalizar el contenido de la página. Los usuarios pueden agregar nuevos controles de elementos web a una página, quitarlos, ocultarlos o minimizarlos como ventanas normales.
- Personalizar el diseño de página. Los usuarios pueden arrastrar un control de elementos web a una zona diferente de una página o cambiar su apariencia, propiedades y comportamiento.
- Controles de exportación e importación. Los usuarios pueden importar o exportar la configuración del control de elementos web para su uso en otras páginas o sitios, conservando las propiedades, la apariencia e incluso los datos de los controles. Esto reduce las demandas de entrada y configuración de datos en los usuarios finales.
- Crear conexiones. Los usuarios pueden establecer conexiones entre controles para que, por ejemplo, un control de gráfico pueda mostrar un gráfico para los datos en un control de ticker de stock. Los usuarios podían personalizar no sólo la conexión en sí, sino también la apariencia y los detalles de cómo el control de gráfico muestra los datos.
- Gestione y personalice la configuración de nivel de sitio. Los usuarios autorizados pueden configurar la configuración de nivel de sitio, determinar quién puede tener acceso a un sitio o página, establecer el acceso basado en roles a los controles, etc. Por ejemplo, un usuario de un rol administrativo podría establecer un control de elementos web para que todos los usuarios lo compartan e impedir que los usuarios que no son administradores personaliquen el control compartido.

Normalmente trabajará con elementos web de una de estas tres maneras: crear páginas que usen controles de elementos web, crear controles de elementos web individuales o crear aplicaciones web completas y personalizables, como un portal.

## <a name="page-development"></a>Desarrollo de páginas

Los desarrolladores de páginas pueden usar herramientas de diseño visual como Microsoft Visual Studio 2005 para crear páginas que usan elementos web. Una ventaja al usar una herramienta como Visual Studio es que el conjunto de controles de elementos web proporciona características para la creación y configuración de elementos web en un diseñador visual. Por ejemplo, puede usar el diseñador para arrastrar una zona de elementos web, o un control de editor de elementos web, a la superficie de diseño y, a continuación, configurar el control directamente en el diseñador mediante la interfaz de usuario proporcionada por el conjunto de controles de elementos web. Esto puede acelerar el desarrollo de aplicaciones de elementos web y reducir la cantidad de código que tiene que escribir.

## <a name="control-development"></a>Desarrollo de control

Puede usar cualquier control de ASP.NET existente como un control de elementos web, incluidos los controles de servidor web estándar, los controles de servidor personalizados y los controles de usuario. Para obtener el máximo control mediante programación del entorno, también puede crear controles de elementos web personalizados que se derivan de la WebPart clase. Para el desarrollo de controles de elementos web individuales, normalmente creará un control de usuario y lo usará como un control de elementos web o desarrollará un control de elementos web personalizado.

Como ejemplo de desarrollo de un control de elementos web personalizado, puede crear un control para proporcionar cualquiera de las características proporcionadas por otros controles de servidor de ASP.NET que podrían ser útiles para empaquetar como un control de elementos web personalizable: calendarios, listas, información financiera, noticias, calculadoras, controles de texto enriquecido para actualizar contenido, cuadrículas editables que se conectan a bases de datos, gráficos que actualizan dinámicamente sus pantallas , o información meteorológica y de viaje. Si proporciona un diseñador visual con el control, cualquier desarrollador de páginas que use Visual Studio puede simplemente arrastrar el control a una zona de elementos web y configurarlo en tiempo de diseño sin tener que escribir código adicional.

La personalización es la base de la característica de elementos web. Permite a los usuarios modificar (o personalizar) el diseño, la apariencia y el comportamiento de los controles de elementos web en una página. La configuración personalizada es de larga duración: se conservan no sólo durante la sesión actual del navegador (como con el estado de vista), sino también en el almacenamiento a largo plazo, por lo que la configuración de un usuario se guarda para futuras sesiones del navegador también. La personalización está habilitada de forma predeterminada para las páginas de elementos web.

Los componentes estructurales de la interfaz de usuario se basan en la personalización y proporcionan la estructura principal y los servicios necesarios para todos los controles de elementos web. Un componente estructural de la interfaz de usuario necesario en cada página de elementos web es el WebPartManager control. Aunque nunca es visible, este control tiene la tarea crítica de coordinar todos los controles de elementos web en una página. Por ejemplo, realiza un seguimiento de todos los controles de elementos web individuales. Administra las zonas de elementos web (regiones que contienen controles de elementos web en una página) y qué controles se encuentran en qué zonas. También realiza un seguimiento y controla los diferentes modos de visualización en los que puede estar una página, como el modo de exploración, conexión, edición o catálogo, y si los cambios de personalización se aplican a todos los usuarios o a usuarios individuales. Por último, inicia y realiza un seguimiento de las conexiones y la comunicación entre los controles de elementos web.

El segundo tipo de componente estructural de la interfaz de usuario es la zona. Las zonas actúan como administradores de diseño en una página de elementos web. Contienen y organizan los controles que derivan de la clase Part (controles de pieza) y proporcionan la capacidad de realizar un diseño de página modular en orientación horizontal o vertical. Las zonas también ofrecen elementos de interfaz de usuario comunes y coherentes (como el estilo de encabezado y pie de página, el título, el estilo de borde, los botones de acción, etc.) para cada control que contienen; estos elementos comunes se conocen como el cromo de un control. Varios tipos especializados de zonas se utilizan en los diferentes modos de visualización y con varios controles. Los diferentes tipos de zonas se describen en la sección Controles esenciales de elementos web a continuación.

Los controles de interfaz de usuario de elementos web, todos los cuales derivan de la **clase Part,** comprenden la interfaz de usuario principal en una página de elementos web. El conjunto de controles de elementos web es flexible e inclusivo en las opciones que ofrece para crear controles de pieza. Además de crear sus propios controles de elementos web personalizados, también puede usar controles de servidor de ASP.NET existentes, controles de usuario o controles de servidor personalizados como controles de elementos web. Los controles esenciales que se usan con más frecuencia para crear páginas de elementos web se describen en la sección siguiente.

## <a name="web-parts-essential-controls"></a>Controles esenciales de elementos web

El conjunto de controles de elementos web es extenso, pero algunos controles son esenciales porque son necesarios para que los elementos web funcionen o porque son los controles que se usan con más frecuencia en las páginas de elementos web. Al empezar a usar elementos web y crear páginas básicas de elementos web, resulta útil estar familiarizado con los controles de elementos web esenciales que se describen en la tabla siguiente.

| **control de elementos web** | **Descripción** |
| --- | --- |
| WebPartManager | Administra todos los controles de elementos web de una página. Se requiere un (y solo uno) **WebPartManager** control para cada página de elementos web. |
| CatalogZone | Contiene CatalogPart controles. Utilice esta zona para crear un catálogo de controles de elementos web desde el que los usuarios pueden seleccionar los controles que desea agregar a una página. |
| EditorZone | Contiene EditorPart controles. Utilice esta zona para permitir a los usuarios editar y personalizar los controles de elementos web en una página. |
| WebPartZone | Contiene y proporciona diseño general para el WebPart controles que componen la interfaz de usuario principal de una página. Utilice esta zona siempre que cree páginas con controles de elementos web. Las páginas pueden contener una o más zonas. |
| ConnectionsZone | Contiene WebPartConnection controles y proporciona una interfaz de usuario para administrar las conexiones. |
| WebPart (GenericWebPart) | Representa la interfaz de usuario principal; la mayoría de los controles de interfaz de usuario de elementos web entran en esta categoría. Para obtener el máximo control mediante programación, puede crear controles de elementos web personalizados que se derivan de la base **WebPart** control. También puede usar controles de servidor existentes, controles de usuario o controles personalizados como controles de elementos web. Siempre que cualquiera de estos controles se colocan en una zona, el **WebPartManager** control los ajusta automáticamente con **GenericWebPart** controles en tiempo de ejecución para que pueda usarlos con la funcionalidad de elementos web. |
| CatalogPart | Contiene una lista de controles de elementos web disponibles que los usuarios pueden agregar a la página. |
| WebPartConnection | Crea una conexión entre dos controles de elementos web en una página. La conexión define uno de los controles de elementos web como un proveedor (de datos) y el otro como consumidor. |
| EditorPart | Sirve como la clase base para los controles de editor especializados. |
| Controles EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart y PropertyGridEditorPart) | Permitir a los usuarios personalizar varios aspectos de los controles de interfaz de usuario de elementos web en una página |

## <a name="lab-create-a-web-part-page"></a>Lab: Crear una página de elementos web

En este laboratorio, creará una página de elementos web que conservará la información a través de perfiles de ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Creación de una página sencilla con elementos web

En esta parte del tutorial, se crea una página que usa controles de elementos web para mostrar contenido estático. El primer paso para trabajar con elementos web es crear una página con dos elementos estructurales necesarios. En primer lugar, una página de elementos web necesita un WebPartManager control para realizar un seguimiento y coordinar todos los controles de elementos web. En segundo lugar, una página de elementos web necesita una o varias zonas, que son controles compuestos que contienen WebPart u otros controles de servidor y ocupan una región especificada de una página.

> [!NOTE]
> No es necesario hacer nada para habilitar la personalización de elementos web; está habilitado de forma predeterminada para el conjunto de controles de elementos web. La primera vez que ejecuta una página de elementos web en un sitio, ASP.NET configura un proveedor de personalización predeterminado para almacenar la configuración de personalización del usuario. Para obtener más información acerca de la personalización, vea Información general sobre la personalización de elementos web.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Para crear una página para contener controles de elementos web

1. Cierre la página predeterminada y agregue una nueva página al sitio denominado WebPartsDemo.aspx.
2. Cambie a la vista **Diseño.**
3. En el menú **Ver,** asegúrese de que las opciones Controles no **visuales** y **Detalles** están seleccionadas para que pueda ver las etiquetas de diseño y los controles que no tienen una interfaz de usuario.
4. Coloque el punto `<div>` de inserción antes de las etiquetas en la superficie de diseño y presione ENTRAR para agregar una nueva línea. Coloque el punto de inserción antes del nuevo carácter de línea, haga clic en el control de lista desplegable **Formato** de bloque del menú y seleccione la opción **Encabezado 1.** En el encabezado, agregue el texto Página de demostración de **elementos web**.
5. En la pestaña **WebParts** del Cuadro de herramientas, arrastre un control **WebPartManager** a la `<div>`página, colocándolo justo después del nuevo carácter de línea y antes de las etiquetas.   
  
   El **WebPartManager** control no representa ninguna salida, por lo que aparece como un cuadro gris en la superficie del diseñador.
6. Coloque el punto `<div>` de inserción dentro de las etiquetas.
7. En el menú **Diseño,** haga clic en **Insertar tabla**y cree una nueva tabla que tenga una fila y tres columnas. Haga clic en el botón **Propiedades** de celda, seleccione la **parte superior** de la lista desplegable **Alineación** vertical, haga clic en **Aceptar**y haga clic en **Aceptar** de nuevo para crear la tabla.
8. Arrastre un WebPartZone control a la columna de la tabla izquierda. Haga clic con el botón derecho en el control **WebPartZone,** elija **Propiedades**y establezca las siguientes propiedades:   
  
   ID: SidebarZone   
  
   HeaderText: Sidebar
9. Arrastre un segundo **control WebPartZone** a la columna de la tabla central y establezca las siguientes propiedades:   
  
   ID: MainZone   
  
   HeaderText: Main
10. Guarde el archivo.

La página ahora tiene dos zonas distintas que puede controlar por separado. Sin embargo, ninguna de las dos zonas tiene contenido, por lo que la creación de contenido es el siguiente paso. En este tutorial, se trabaja con controles de elementos web que solo muestran contenido estático.

El diseño de una zona de &lt;elementos&gt; web se especifica mediante un elemento zonetemplate. Dentro de la plantilla de zona, puede agregar cualquier control de ASP.NET, ya sea un control de elementos web personalizado, un control de usuario o un control de servidor existente. Tenga en cuenta que aquí está utilizando el Label control y que simplemente está agregando texto estático. Al colocar un control de servidor normal en una zona **WebPartZone,** ASP.NET trata el control como un control de elementos web en tiempo de ejecución, lo que habilita las características de elementos web en el control.

**Para crear contenido para la zona principal**

1. En la vista **Diseño,** arrastre un **control Label** desde la pestaña **Estándar** del Cuadro de herramientas al área de contenido de la zona cuya propiedad **ID** está establecida en MainZone.
2. Cambie a la vista **Origen.** Observe que &lt;se&gt; ha agregado un elemento zonetemplate para ajustar el control **Label** en MainZone.
3. Agregue un **title** atributo denominado &lt;title&gt; al elemento asp:label y establezca su valor en Content. Elimine el atributo Text-"Label" del elemento &lt;asp:label.&gt; Entre las etiquetas de &lt;apertura y&gt; cierre del elemento asp:label, agregue texto &lt;como&gt; Bienvenido a mi **página principal** dentro de un par de etiquetas de elemento h2. El código debe tener el siguiente aspecto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Guarde el archivo.

A continuación, cree un control de usuario que también se pueda agregar a la página como un control de elementos web.

### <a name="to-create-a-user-control"></a>Crear un control de usuario

1. Agregue un nuevo control de usuario Web a su sitio para que sirva como control de búsqueda. Anule la selección de la opción **Colocar código fuente en un archivo independiente.** Agréguelo en el mismo directorio que el WebPartsDemo.aspx página y asímócelo SearchUserControl.ascx.   
  
    > [!NOTE]
    > El control de usuario de este tutorial no implementa la funcionalidad de búsqueda real; solo se utiliza para demostrar las características de elementos web.
2. Cambie a la vista **Diseño.** En la pestaña **Estándar** del Cuadro de herramientas, arrastre un control TextBox a la página.
3. Coloque el punto de inserción después del cuadro de texto que acaba de agregar y presione ENTRAR para agregar una nueva línea.
4. Arrastre un Button control a la página en la nueva línea debajo del cuadro de texto que acaba de agregar.
5. Cambie a la vista **Origen.** Asegúrese de que el código fuente del control de usuario es similar al ejemplo siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Guarde y cierre el archivo.

Ahora puede agregar controles de elementos web a la zona de barra lateral. Está agregando dos controles a la zona de barra lateral, uno que contiene una lista de vínculos y otro que es el control de usuario que creó en el procedimiento anterior. Los vínculos se agregan como un control de servidor **de etiquetas** estándar, similar a la forma en que creó el texto estático para la zona principal. Sin embargo, aunque los controles de servidor individuales contenidos en el control de usuario podrían estar contenidos directamente en la zona (como el control de etiqueta), en este caso no lo son. En su lugar, forman parte del control de usuario que creó en el procedimiento anterior. Esto muestra una forma común de empaquetar los controles y la funcionalidad adicional que desee en un control de usuario y, a continuación, hacer referencia a ese control en una zona como un control de elementos web.

En tiempo de ejecución, el conjunto de controles de elementos web ajusta ambos controles con GenericWebPart controles. Cuando un **GenericWebPart** control ajusta un control de servidor Web, el control de elemento genérico es el control primario y puede tener acceso al control de servidor a través de la propiedad ChildControl del control primario. Este uso de controles de elemento genéricos permite que los controles de servidor Web estándar tengan el mismo comportamiento básico y atributos que los controles de elementos web que se derivan de la **WebPart** clase.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Para agregar controles de elementos web a la zona de la barra lateral

1. Abra el WebPartsDemo.aspx página.
2. Cambie a la vista **Diseño.**
3. Arrastre la página de control de usuario que creó, SearchUserControl.ascx, desde el **Explorador** de soluciones a la zona cuya propiedad **ID** está establecida en SidebarZone y colóquela allí.
4. Guarde el WebPartsDemo.aspx página.
5. Cambie a la vista **Origen.**
6. Dentro &lt;del elemento asp:webpartzone&gt; para SidebarZone, justo encima de &lt;la&gt; referencia al control de usuario, agregue un elemento asp:label con vínculos contenidos, como se muestra en el ejemplo siguiente. Además, agregue un atributo **Title** a la etiqueta de control de usuario, con un valor de **Search**, como se muestra. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Guarde y cierre el archivo.

Ahora puede probar su página navegando a ella en su navegador. La página muestra las dos zonas. La siguiente captura de pantalla muestra la página.

**Página de demostración de elementos web con dos zonas**

![Elementos web VS Tutorial 1 Captura de pantalla](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: Elementos web VS Tutorial 1 Captura de pantalla

En la barra de título de cada control hay una flecha hacia abajo que proporciona acceso a un menú de verbos de acciones disponibles que puede realizar en un control. Haga clic en el menú de verbos de uno de los controles y, a continuación, haga clic en el verbo **Minimizar** y tenga en cuenta que el control está minimizado. En el menú de verbos, haga clic en **Restaurar**y el control vuelve a su tamaño normal.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permitir a los usuarios editar páginas y cambiar el diseño

Elementos web proporciona a los usuarios la capacidad de cambiar el diseño de los controles de elementos web arrastrándolos de una zona a otra. Además de permitir a los usuarios mover **WebPart** controles de una zona a otra, puede permitir a los usuarios editar varias características de los controles, incluida su apariencia, diseño y comportamiento. El conjunto de controles de elementos web proporciona funcionalidad de edición básica para **WebPart** controles. Aunque no lo hará en este tutorial, también puede crear controles de editor personalizados que permiten a los usuarios editar las características de **WebPart** controles. Al igual que con el cambio de la ubicación de un **WebPart** control, editar las propiedades de un control se basa en ASP.NET personalización para guardar los cambios que realizan los usuarios.

En esta parte del tutorial, agregue la capacidad para que los usuarios editen las características básicas de cualquier **WebPart** control en la página. Para habilitar estas características, agregue otro control de usuario &lt;personalizado a&gt; la página, junto con un elemento asp:editorzone y dos controles de edición.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Para crear un control de usuario que permita cambiar el diseño de página

1. En Visual Studio, en **el** archivo menú, seleccione el **nuevo** submenú y haga clic en el **archivo** opción.
2. En el cuadro de diálogo **Agregar nuevo elemento,** seleccione **Control de usuario web**. Asigne al nuevo archivo el nombre DisplayModeMenu.ascx. Anule la selección de la opción **Colocar código fuente en un archivo independiente.**
3. Haga clic en Agregar para crear el nuevo control.
4. Cambie a la vista **Origen.**
5. Quite todo el código existente en el nuevo archivo y pegue el código siguiente. Este código de control de usuario usa características del conjunto de controles de elementos web que permiten a una página cambiar su modo de vista o presentación, y también le permite cambiar la apariencia física y el diseño de la página mientras se encuentra en determinados modos de presentación. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Guarde el archivo haciendo clic en el icono Guardar de la barra de herramientas o seleccionando **Guardar** en el menú **Archivo.**

### <a name="to-enable-users-to-change-the-layout"></a>Para permitir que los usuarios cambien el diseño

1. Abra el WebPartsDemo.aspx página y cambie a la vista **Diseño.**
2. Coloque el punto de inserción en la vista **Diseño** justo después del control **WebPartManager** que agregó anteriormente. Agregue un retorno duro después del texto para que haya una línea en blanco después de la **WebPartManager** control. Coloque el punto de inserción en la línea en blanco.
3. Arrastre el control de usuario que acaba de crear (el archivo se denomina DisplayModeMenu.ascx) a la WebPartsDemo.aspx página y colóquelo en la línea en blanco.
4. Arrastre un EditorZone control desde el **WebParts** sección del cuadro de herramientas a la celda de tabla abierta restante en el WebPartsDemo.aspx página.
5. Desde el **WebParts** sección de la cuadro de herramientas, arrastre un AppearanceEditorPart control y un LayoutEditorPart control en el **EditorZone** control.
6. Cambie a la vista **Origen.** El código resultante en la celda de la tabla debe ser similar al código siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Guarde el archivo WebPartsDemo.aspx. Ha creado un control de usuario que le permite cambiar los modos de presentación y cambiar el diseño de página, y ha hecho referencia al control en la página Web principal.

Ahora puede probar la capacidad de editar páginas y cambiar el diseño.

### <a name="to-test-layout-changes"></a>Para probar los cambios de diseño

1. Cargue la página en un explorador.
2. Haga clic en el menú desplegable Modo de **visualización** y seleccione **Editar**. Se muestran los títulos de zona.
3. Arrastre el control **Mis vínculos** por su barra de título desde la zona de la barra lateral hasta la parte inferior de la zona principal. Su página debe tener el aspecto de la siguiente captura de pantalla.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Página de demostración de elementos web con el control Mis vínculos movido

![Elementos web VS Tutorial 2 Captura de pantalla](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: Elementos web VS Tutorial 2 Captura de pantalla

1. Haga clic en el menú desplegable Modo de **visualización** y seleccione **Examinar**. La página se actualiza, los nombres de zona desaparecen y el control **Mis vínculos** permanece donde se colocó.
2. Para demostrar que la personalización funciona, cierre el explorador y, a continuación, vuelva a cargar la página. Los cambios realizados se guardan para futuras sesiones del navegador.
3. En el menú Modo de **visualización,** seleccione **Editar**.   
  
   Cada control de la página ahora se muestra con una flecha hacia abajo en su barra de título, que contiene el menú desplegable de verbos.
4. Haga clic en la flecha para mostrar el menú de verbos en el control **Mis vínculos.** Haga clic en el verbo **Editar.**   
  
   Aparece el control **EditorZone,** que muestra los controles EditorPart que agregó.
5. En la sección **Apariencia** del control de edición, cambie el **Título** a Mis favoritos, use la lista desplegable **Tipo** de Chrome para seleccionar **Solo título**y, a continuación, haga clic en **Aplicar**. La siguiente captura de pantalla muestra la página en modo de edición.

### <a name="web-parts-demo-page-in-edit-mode"></a>Página de demostración de elementos web en modo de edición

![Elementos web VS Tutorial 3 Captura de pantalla](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: Elementos web VS Tutorial 3 Captura de pantalla

1. Haga clic en el menú Modo de **visualización** y seleccione **Examinar** para volver al modo de exploración.
2. El control ahora tiene un título actualizado y ningún borde, como se muestra en la siguiente captura de pantalla.

### <a name="edited-web-parts-demo-page"></a>Página de demostración de elementos web editados

![Elementos web VS Tutorial 4 Captura de pantalla](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: Elementos web VS Tutorial 4 Captura de pantalla

### <a name="adding-web-parts-at-run-time"></a>Adición de elementos web en tiempo de ejecución

También puede permitir que los usuarios agreguen controles de elementos web a su página en tiempo de ejecución. Para ello, configure la página con un catálogo de elementos web, que contiene una lista de controles de elementos web que desea poner a disposición de los usuarios.

**Para permitir que los usuarios agreguen elementos web en tiempo de ejecución**

1. Abra el WebPartsDemo.aspx página y cambie a la vista **Diseño.**
2. En la pestaña **WebParts** del Cuadro de herramientas, arrastre un control CatalogZone a la columna derecha de la tabla, debajo del control **EditorZone.**   
  
   Ambos controles pueden estar en la misma celda de tabla porque no se mostrarán al mismo tiempo.
3. En el panel Propiedades, asigne la cadena **Agregar elementos web** a la propiedad HeaderText del control **CatalogZone.**
4. Desde el **WebParts** sección de la cuadro de herramientas, arrastre un DeclarativeCatalogPart control en el área de contenido de la **CatalogZone** control.
5. Haga clic en la flecha de la esquina superior derecha del control **DeclarativeCatalogPart** para exponer su menú Tareas y, a continuación, seleccione **Editar plantillas**.
6. En la sección **Estándar** del cuadro de herramientas, arrastre un control **FileUpload** y un control **Calendar** a la sección **WebPartsTemplate** del control **DeclarativeCatalogPart.**
7. Cambie a la vista **Origen.** Inspeccione el código &lt;fuente del&gt; elemento asp:catalogzone. Tenga en cuenta que el &lt; **Control DeclarativeCatalogPart** contiene un elemento webpartstemplate&gt; con los dos controles de servidor incluidos que podrá agregar a la página desde el catálogo.
8. Agregue una propiedad **Title** a cada uno de los controles que agregó al catálogo, utilizando el valor de cadena que se muestra para cada título en el ejemplo de código siguiente. Aunque el título no es una propiedad que normalmente se puede establecer en estos dos controles de servidor en tiempo de diseño, cuando un usuario agrega estos controles a un **WebPartZone** zona desde el catálogo en tiempo de ejecución, cada uno se ajustan con un **GenericWebPart** control. Esto les permite actuar como controles de elementos web, por lo que podrán mostrar títulos.   
  
   El código de los dos controles contenidos en el **Control DeclarativeCatalogPart** debe tener el siguiente aspecto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Guarde la página.

Ahora puede probar el catálogo.

### <a name="to-test-the-web-parts-catalog"></a>Para probar el catálogo de elementos web

1. Cargue la página en un explorador.
2. Haga clic en el menú desplegable Modo de **visualización** y seleccione **Catálogo**.   
  
   Se muestra el catálogo titulado **Agregar elementos web.**
3. Arrastre el control **Mis favoritos** desde la zona principal hasta la parte superior de la zona de la barra lateral y suéltelo allí.
4. En el catálogo **Agregar elementos web,** active ambas casillas de verificación y, a continuación, seleccione **Principal** en la lista desplegable que contiene las zonas disponibles.
5. Haga clic en **Agregar** en el catálogo. Los controles se agregan a la zona principal. Si lo desea, puede agregar varias instancias de controles del catálogo a la página.   
  
   La siguiente captura de pantalla muestra la página con el control de carga de archivos y el calendario en la zona principal. 

![Controles añadidos a la zona principal desde el catálogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Haga clic en el menú desplegable Modo de **visualización** y seleccione **Examinar**. El catálogo desaparece y la página se actualiza.
7. Cierre el explorador. Cargue la página de nuevo. Los cambios realizados persisten.
