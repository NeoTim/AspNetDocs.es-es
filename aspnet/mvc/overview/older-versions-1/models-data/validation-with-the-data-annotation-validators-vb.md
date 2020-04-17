---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validación con los validadores de anotaciones de datos (VB) Microsoft Docs
author: rick-anderson
description: Aproveche el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación ASP.NET MVC. Aprende a usar los diferentes tipos de validador...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: cabdf6dab9e5de53a45adcf126705533fca02de7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542643"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Validación con los validadores de anotación de datos (VB)

por [Microsoft](https://github.com/microsoft)

> Aproveche el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación ASP.NET MVC. Aprenda a usar los diferentes tipos de atributos de validador y trabajar con ellos en Microsoft Entity Framework.

En este tutorial, aprenderá a usar los validadores de anotación de datos para realizar la validación en una aplicación ASP.NET MVC. La ventaja de usar los validadores de anotación de datos es que permiten realizar la validación simplemente agregando uno o varios atributos, como el atributo Required o StringLength, a una propiedad de clase.

Para poder usar los validadores de anotación de datos, debe descargar el enlazador de modelos de anotaciones de datos. Puede descargar el ejemplo de enlazador de modelos de anotaciones de datos desde el sitio web de CodePlex haciendo clic [aquí](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

Es importante comprender que el enlazador de modelos de anotaciones de datos no es una parte oficial del marco de Microsoft ASP.NET MVC. Aunque el enlazador de modelos de anotaciones de datos fue creado por el equipo de Microsoft ASP.NET MVC, Microsoft no ofrece soporte oficial del producto para el enlazador de modelos de anotaciones de datos descrito y utilizado en este tutorial.

## <a name="using-the-data-annotation-model-binder"></a>Uso del enlazador de modelos de anotación de datos

Para usar el enlazador de modelos de anotaciones de datos en una aplicación ASP.NET MVC, primero debe agregar una referencia al ensamblado Microsoft.Web.Mvc.DataAnnotations.dll y al ensamblado System.ComponentModel.DataAnnotations.dll. Seleccione la opción de menú **Proyecto, Agregar referencia**. A continuación, haga clic en la pestaña **Examinar** y vaya a la ubicación donde descargó (y descomprimió) el ejemplo de enlazador de modelos de anotaciones de datos (consulte **la figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: Adición de una referencia al enlazador de modelos de anotaciones de datos ([Haga clic para ver la imagen de tamaño completo](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Seleccione el ensamblado Microsoft.Web.Mvc.DataAnnotations.dll y el ensamblado System.ComponentModel.DataAnnotations.dll y haga clic en el botón **Aceptar.**

No puede usar el ensamblado System.ComponentModel.DataAnnotations.dll incluido con .NET Framework Service Pack 1 con el enlazador de modelos de anotaciones de datos. Debe usar la versión del ensamblado System.ComponentModel.DataAnnotations.dll incluido con la descarga de ejemplo de enlazador de modelos de anotaciones de datos.

Por último, debe registrar el DataAnnotations Model Binder en el Global.asax archivo. Agregue la siguiente línea de\_código al controlador de\_eventos Application Start() para que el método Application Start() tenga este aspecto:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Esta línea de código registra el DataAnnotationsModelBinder como el enlazador de modelos predeterminado para toda la aplicación ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Uso de los atributos del validador de anotación de datos

Cuando se utiliza el enlazador de modelos de anotaciones de datos, se utilizan atributos de validador para realizar la validación. El espacio de nombres System.ComponentModel.DataAnnotations incluye los siguientes atributos de validador:

- Range: permite validar si el valor de una propiedad se encuentra entre un intervalo especificado de valores.
- RegularExpression: permite validar si el valor de una propiedad coincide con un patrón de expresión regular especificado.
- Required: permite marcar una propiedad según sea necesario.
- StringLength: permite especificar una longitud máxima para una propiedad de cadena.
- Validación: la clase base para todos los atributos del validador.

> [!NOTE] 
> 
> Si ninguno de los validadores estándar satisface las necesidades de validación, siempre tiene la opción de crear un atributo de validador personalizado heredando un nuevo atributo de validador del atributo validation base.

La clase Product **del listado 1** ilustra cómo utilizar estos atributos del validador. Las propiedades Name, Description y UnitPrice se marcan como necesarias. La propiedad Name debe tener una longitud de cadena inferior a 10 caracteres. Por último, la propiedad UnitPrice debe coincidir con un patrón de expresión regular que represente un importe de moneda.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listado 1**: Modelos-Product.vb

La clase Product ilustra cómo utilizar un atributo adicional: el atributo DisplayName. El atributo DisplayName permite modificar el nombre de la propiedad cuando la propiedad se muestra en un mensaje de error. En lugar de mostrar el mensaje de error "El campo UnitPrice es obligatorio" puede mostrar el mensaje de error "El campo Precio es obligatorio".

> [!NOTE] 
> 
> Si desea personalizar completamente el mensaje de error mostrado por un validador, puede asignar un mensaje de error personalizado a la propiedad ErrorMessage del validador de la siguiente manera:`<Required(ErrorMessage:="This field needs a value!")>`

Puede utilizar la clase Product en **el listado 1** con la acción del controlador Create() en el listado **2.** Esta acción del controlador vuelve a mostrar la vista Crear cuando el estado del modelo contiene errores.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listado 2**: Controllers-ProductController.vb

Por último, puede crear la vista en el **listado 3** haciendo clic con el botón derecho en la acción Crear() y seleccionando la opción de menú **Agregar vista**. Cree una vista fuertemente tipada con la clase Product como clase de modelo. Seleccione **Crear** en la lista desplegable de contenido de vista (consulte **la figura 2).**

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: Adición de la vista Crear

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listado 3**: Vistas-Producto-Create.aspx

> [!NOTE] 
> 
> Quite el campo Id del formulario Crear formulario generado por la opción de menú **Agregar vista.** Dado que el campo Id corresponde a una columna Identity, no desea permitir que los usuarios escriban un valor para este campo.

Si envía el formulario para crear un producto y no especifica valores para los campos obligatorios, se muestran los mensajes de error de validación de **la figura 3.**

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: Faltan campos obligatorios

Si especifica un importe de moneda no válido, se muestra el mensaje de error en **la Figura 4.**

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: Importe de moneda no válido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Uso de validadores de anotaciones de datos con Entity Framework

Si usa Microsoft Entity Framework para generar las clases de modelo de datos, no puede aplicar los atributos del validador directamente a las clases. Dado que Entity Framework Designer genera las clases de modelo, los cambios que realice en las clases de modelo se sobrescribirán la próxima vez que realice cambios en el Diseñador.

Si desea usar los validadores con las clases generadas por Entity Framework, debe crear clases de metadatos. Los validadores se aplican a la clase de metadatos en lugar de aplicar los validadores a la clase real.

Por ejemplo, imagine que ha creado una clase Movie mediante Entity Framework (consulte **la figura 5**). Imagine, además, que desea que las propiedades Título de película y Director sean obligatorias. En ese caso, puede crear la clase parcial y la clase de datos meta en el **listado 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: Clase de película generada por Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listado 4**: Modelos-Movie.vb

El archivo del **listado 4** contiene dos clases denominadas Movie y MovieMetaData. La clase Movie es una clase parcial. Corresponde a la clase parcial generada por Entity Framework que se encuentra en el archivo DataModel.Designer.vb.

Actualmente, .NET Framework no admite propiedades parciales. Por lo tanto, no hay ninguna manera de aplicar los atributos del validador a las propiedades de la clase Movie definidas en el archivo DataModel.Designer.vb aplicando los atributos del validador a las propiedades de la clase Movie definidas en el archivo **del listado 4.**

Observe que la clase parcial Movie está decorada con un atributo MetadataType que apunta a la clase MovieMetaData. La clase MovieMetaData contiene propiedades de proxy para las propiedades de la clase Movie.

Los atributos del validador se aplican a las propiedades de la MovieMetaData clase. Las propiedades Title, Director y DateReleased se marcan como propiedades necesarias. A la propiedad Director se le debe asignar una cadena que contenga menos de 5 caracteres. Por último, el atributo DisplayName se aplica a la propiedad DateReleased para mostrar un mensaje de error como "Se requiere el campo Fecha de lanzamiento." en lugar del error "El campo DateReleased es obligatorio."

> [!NOTE] 
> 
> Tenga en cuenta que las propiedades de proxy en el MovieMetaData clase no necesitan representar los mismos tipos que las propiedades correspondientes en el Movie clase. Por ejemplo, la propiedad Director es una propiedad string en la clase Movie y una propiedad de objeto en la clase MovieMetaData.

La página **de la figura 6** ilustra los mensajes de error devueltos al escribir valores no válidos para las propiedades Movie.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: Uso de validadores con Entity Framework ( Haga clic para ver la[imagen de tamaño completo](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Resumen

En este tutorial, aprendió a aprovechar el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación ASP.NET MVC. Ha aprendido a usar los diferentes tipos de atributos de validador, como los atributos Required y StringLength. También aprendió a usar estos atributos al trabajar con Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Anterior](validating-with-a-service-layer-vb.md)
