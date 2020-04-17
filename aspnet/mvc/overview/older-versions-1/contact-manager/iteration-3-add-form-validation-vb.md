---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: '#3 de iteración: agregar validación de formulario (VB) Microsoft Docs'
author: rick-anderson
description: En la tercera iteración, agregamos validación básica del formulario. Evitamos que las personas envíen un formulario sin completar los campos de formulario obligatorios. También validamos emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ee2f40996873a7af2eaa255edd5f157c3fefb29
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542396"
---
# <a name="iteration-3--add-form-validation-vb"></a>Iteración n.º 3: Agregar una validación de formulario (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> En la tercera iteración, agregamos validación básica del formulario. Evitamos que las personas envíen un formulario sin completar los campos de formulario obligatorios. También validamos direcciones de correo electrónico y números de teléfono.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de administración de contactos ASP.NET MVC (VB)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto (nombres, números de teléfono y direcciones de correo electrónico) para una lista de personas.

Creamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de iteración múltiple es permitirle comprender el motivo de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, creamos Contact Manager de la manera más sencilla posible. Agregamos compatibilidad con operaciones básicas de base de datos: Crear, leer, actualizar y eliminar (CRUD).

- Iteración #2 - Hacer que la aplicación se vea bien. En esta iteración, mejoramos el aspecto de la aplicación modificando la página maestra de vista de ASP.NET MVC predeterminada y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos validación básica del formulario. Evitamos que las personas envíen un formulario sin completar los campos de formulario obligatorios. También validamos direcciones de correo electrónico y números de teléfono.

- Iteración #4: haga que la aplicación se acople libremente. En esta cuarta iteración, aprovechamos varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, refactorizamos nuestra aplicación para usar el patrón Repository y el patrón Dependency Injection.

- Iteración #5: crear pruebas unitarias. En la quinta iteración, hacemos que nuestra aplicación sea más fácil de mantener y modificar agregando pruebas unitarias. Nos burlamos de nuestras clases de modelo de datos y creamos pruebas unitarias para nuestros controladores y lógica de validación.

- #6 de iteración: use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- de #7 iteración: agregue la funcionalidad DebAjax. En la séptima iteración, mejoramos la capacidad de respuesta y el rendimiento de nuestra aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta segunda iteración de la aplicación Contact Manager, agregamos validación básica de formularios. Evitamos que las personas envíen un contacto sin introducir valores para los campos de formulario obligatorios. También validamos números de teléfono y direcciones de correo electrónico (consulte la figura 1).

[![Cuadro de diálogo Nuevo proyecto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: Un formulario con validación[(Haga clic para ver la imagen de tamaño completo)](iteration-3-add-form-validation-vb/_static/image2.png)

En esta iteración, agregamos la lógica de validación directamente a las acciones del controlador. En general, esta no es la forma recomendada de agregar validación a una aplicación ASP.NET MVC. Un mejor enfoque es colocar una lógica de validación de s de aplicación en una capa de [servicio](http://martinfowler.com/eaaCatalog/serviceLayer.html)independiente. En la siguiente iteración, refactorizamos la aplicación Contact Manager para que la aplicación sea más mantenible.

En esta iteración, para mantener las cosas simples, escribimos todo el código de validación a mano. En lugar de escribir el código de validación nosotros mismos, podríamos aprovechar un marco de validación. Por ejemplo, puede usar el bloque de aplicación de validación de biblioteca empresarial (VAB) de Microsoft para implementar la lógica de validación para la aplicación MVC ASP.NET. Para obtener más información sobre el bloque de aplicaciones de validación, consulte:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Agregar validación a la vista Crear

Let s comenzar agregando lógica de validación a la create vista. Afortunadamente, dado que generamos la vista Crear con Visual Studio, la vista Crear ya contiene toda la lógica de interfaz de usuario necesaria para mostrar mensajes de validación. La vista Crear se encuentra en el listado 1.

**Listado 1 - "Vistas", Contacto, Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

La clase field-validation-error se utiliza para aplicar estilo a la salida representada por la aplicación auxiliar Html.ValidationMessage(). La clase input-validation-error se utiliza para aplicar estilo al cuadro de texto (entrada) representado por la aplicación auxiliar Html.TextBox(). La clase validation-summary-errors se utiliza para aplicar estilo a la lista desordenada representada por la aplicación auxiliar Html.ValidationSummary().

> [!NOTE] 
> 
> Puede modificar las clases de hoja de estilos descritas en esta sección para personalizar la apariencia de los mensajes de error de validación.

## <a name="adding-validation-logic-to-the-create-action"></a>Adición de lógica de validación a la acción Crear

En este momento, la vista Crear nunca muestra mensajes de error de validación porque no hemos escrito la lógica para generar ningún mensaje. Para mostrar los mensajes de error de validación, debe agregar los mensajes de error a ModelState.

> [!NOTE] 
> 
> El método UpdateModel() agrega mensajes de error a ModelState automáticamente cuando hay un error al asignar el valor de un campo de formulario a una propiedad. Por ejemplo, si intenta asignar la cadena "apple" a una propiedad BirthDate que acepta valores DateTime, el método UpdateModel() agrega un error a ModelState.

El método Create() modificado en el listado 2 contiene una nueva sección que valida las propiedades de la clase Contact antes de insertar el nuevo contacto en la base de datos.

**Listado 2 - Controllers-ContactController.vb (Crear con validación)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

La sección validate aplica cuatro reglas de validación distintas:

- La propiedad FirstName debe tener una longitud mayor que cero (y no puede constar únicamente de espacios)
- La propiedad LastName debe tener una longitud mayor que cero (y no puede constar únicamente de espacios)
- Si el Phone propiedad tiene un valor (tiene una longitud mayor que 0), a continuación, el Phone propiedad debe coincidir con una expresión regular.
- Si el Email propiedad tiene un valor (tiene una longitud mayor que 0), a continuación, el Email propiedad debe coincidir con una expresión regular.

Cuando hay una infracción de la regla de validación, se agrega un mensaje de error a ModelState con la ayuda del método AddModelError(). Cuando se agrega un mensaje a ModelState, se proporciona el nombre de una propiedad y el texto de un mensaje de error de validación. Este mensaje de error se muestra en la vista mediante los métodos auxiliares Html.ValidationSummary() y Html.ValidationMessage().

Después de ejecutar las reglas de validación, se comprueba la propiedad IsValid de ModelState. El IsValid propiedad devuelve false cuando se han agregado mensajes de error de validación a ModelState. Si se produce un error en la validación, el formulario Crear se vuelve a mostrar con los mensajes de error.

> [!NOTE] 
> 
> Recibí las expresiones regulares para validar el número de teléfono y la dirección de correo electrónico del repositorio de expresiones regulares en[*http://regexlib.com*](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Adición de lógica de validación a la acción de edición

La acción Edit() actualiza un contacto. La acción Edit() debe realizar exactamente la misma validación que la acción Create(). En lugar de duplicar el mismo código de validación, debemos refactorizar el controlador Contact para que las acciones Create() y Edit() llamen al mismo método de validación.

La clase de controlador Contact modificada se encuentra en el listado 3. Esta clase tiene un nuevo método ValidateContact() al que se llama dentro de las acciones Create() y Edit().

**Listado 3 - Controllers-ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Resumen

En esta iteración, agregamos validación básica de formularios a nuestra aplicación Contact Manager. Nuestra lógica de validación impide que los usuarios envíen un nuevo contacto o editen un contacto existente sin proporcionar valores para las propiedades FirstName y LastName. Además, los usuarios deben proporcionar números de teléfono y direcciones de correo electrónico válidos.

En esta iteración, agregamos la lógica de validación a nuestra aplicación Contact Manager de la manera más fácil posible. Sin embargo, mezclar nuestra lógica de validación en nuestra lógica de controlador nos creará problemas a largo plazo. Nuestra aplicación será más difícil de mantener y modificar con el tiempo.

En la siguiente iteración, refactorizaremos nuestra lógica de validación y la lógica de acceso a la base de datos de nuestros controladores. Aprovecharemos varios principios de diseño de software para permitirnos crear una aplicación más acoplada y más mantenible.

> [!div class="step-by-step"]
> [Anterior](iteration-2-make-the-application-look-nice-vb.md)
> [Siguiente](iteration-4-make-the-application-loosely-coupled-vb.md)
