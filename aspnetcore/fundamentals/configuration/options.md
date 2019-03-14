---
title: Patrón de opciones en ASP.NET Core
author: guardrex
description: Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 9566ed75375bdfaa9d6d8bf898b9fb2054356017
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065222"
---
# <a name="options-pattern-in-aspnet-core"></a>Patrón de opciones en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

::: moniker range="<= aspnetcore-1.1"

Para obtener la versión 1.1 de este tema, descargue [Patrón de opciones en ASP.NET Core (versión 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).

::: moniker-end

El patrón de opciones usa clases para representar grupos de configuraciones relacionadas. Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:

* El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.
* [Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.

Las opciones también proporcionan un mecanismo para validar los datos de configuración. Para obtener más información, consulte la sección [Opciones de validación](#options-validation).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).

## <a name="options-interfaces"></a>Interfaces de opciones

<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`. <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> admite las siguientes situaciones:

* Notificaciones de cambios
* [Opciones con nombre](#named-options-support-with-iconfigurenamedoptions)
* [Configuración que se puede recargar](#reload-configuration-data-with-ioptionssnapshot)
* Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)

Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.

<xref:Microsoft.Extensions.Options.IOptionsFactory`1> es responsable de crear nuevas instancias de opciones. Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>. La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions`1> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores. Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> y <xref:Microsoft.Extensions.Options.IConfigureOptions`1>, y solo llama a la interfaz adecuada.

<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> para almacenar en caché las instancias de `TOptions`. <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>). Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>. Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.

<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud. Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).

<xref:Microsoft.Extensions.Options.IOptions`1> puede utilizarse para admitir las opciones. Sin embargo, <xref:Microsoft.Extensions.Options.IOptions`1> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>. Aún puede usar <xref:Microsoft.Extensions.Options.IOptions`1> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions`1> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.

## <a name="general-options-configuration"></a>Configuración de opciones generales

La configuración de opciones generales se muestra en el ejemplo &num;1 en la aplicación de ejemplo.

Una clase de opciones debe ser no abstracta con un constructor público sin parámetros. La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`. Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`. `Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> para acceder a la configuración (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.

## <a name="configure-simple-options-with-a-delegate"></a>Configurar opciones simples con un delegado

La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la aplicación de ejemplo.

Use un delegado para establecer los valores de opciones. La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> se agrega al contenedor de servicios. Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Puede agregar varios proveedores de configuración. Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados. Para obtener más información, consulta <xref:fundamentals/configuration/index>.

Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenedor de servicios. En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.

Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración. Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configuración de subopciones

La configuración de subopciones se muestra en el ejemplo &num;3 en la aplicación de ejemplo.

Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación. Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.

Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`. Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.

En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenedor de servicios. Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Si la aplicación usa el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), el paquete se incluye automáticamente.

El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opciones proporcionadas por un modelo de vista o con inserción de vista directa

Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la aplicación de ejemplo.

Las opciones se pueden suministrar en un modelo de vista o insertando <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> directamente en una vista (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Volver a cargar los datos de configuración con IOptionsSnapshot

El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> se muestra en el ejemplo &num;5 en la aplicación de ejemplo.

<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> admite volver a cargar opciones con la mínima sobrecarga de procesamiento.

Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.

En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*). Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`. Guarde el archivo *appSettings.json*. Actualice el explorador para ver qué valores de opciones se han actualizado:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Compatibilidad de opciones con nombre con IConfigureNamedOptions

La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> se muestra en el ejemplo &num;6 de la aplicación de ejemplo.

La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre. En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*. Los valores de `named_options_2` los proporciona:

* El delegado `named_options_2` en `ConfigureServices` para `Option1`.
* El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.

## <a name="configure-all-options-with-the-configureall-method"></a>Configurar todas las opciones con el método ConfigureAll

Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>. El siguiente código configura `Option1` para todas las instancias de configuración con un valor común. Agregue manualmente este código al método `Startup.ConfigureServices`:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> Todas las opciones son instancias con nombre. Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions`1> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`. <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions`1>. La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory`1> tiene lógica para usar cada una de forma adecuada. La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).

## <a name="optionsbuilder-api"></a>API OptionsBuilder

<xref:Microsoft.Extensions.Options.OptionsBuilder`1> se usa para configurar instancias `TOptions`. `OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores. La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a>Uso de servicios de DI para configurar opciones

Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:

* Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1). [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions`1> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, y se registrar como un servicio.

Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada. La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*). La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados. 

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a>Opciones de validación

Las opciones de validación permiten validar las opciones cuando se configuran. Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");

// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
}
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`. La instancia predeterminada es `Options.DefaultName`.

La validación se ejecuta cuando se crea la instancia de opciones. La instancia de opciones pasa seguro la validación la primera vez que se accede.

> [!IMPORTANT]
> La validación de opciones no protege contra las modificaciones de opciones después de configurarse y validarse inicialmente.

El método `Validate` acepta una expresión `Func<TOptions, bool>`. Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:

* Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`

`IValidateOptions` valida:

* Una instancia de opciones con nombre específica.
* Todas las opciones cuando `name` es `null`.

Devuelve `ValidateOptionsResult` de la implementación de la interfaz:

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método `ValidateDataAnnotations` en `OptionsBuilder<TOptions>`. `Microsoft.Extensions.Options.DataAnnotations` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 o versiones posteriores).

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.

::: moniker-end

## <a name="options-post-configuration"></a>Configuración posterior de las opciones

Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>. La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions`1>:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a>Acceso a opciones durante el inicio

<xref:Microsoft.Extensions.Options.IOptions`1> y <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

No use <xref:Microsoft.Extensions.Options.IOptions`1> o <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> en `Startup.ConfigureServices`. Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
