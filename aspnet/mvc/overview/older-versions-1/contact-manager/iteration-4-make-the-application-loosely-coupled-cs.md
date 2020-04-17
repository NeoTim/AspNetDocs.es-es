---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iteración #4: haga que la aplicación se acople de forma flexible (C-) Microsoft Docs'
author: rick-anderson
description: En esta cuarta iteración, aprovechamos varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Para...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: c4ba6c9a130995c095653f4316a5fefdfc03b91d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542370"
---
# <a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iteración n.º 4: Hacer que la aplicación tenga un acoplamiento flexible (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> En esta cuarta iteración, aprovechamos varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, refactorizamos nuestra aplicación para usar el patrón Repository y el patrón Dependency Injection.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación de administración de contactos ASP.NET MVC (C-)

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

En esta cuarta iteración de la aplicación Contact Manager, refactorizamos la aplicación para que la aplicación esté más acoplada. Cuando una aplicación está acoplada libremente, puede modificar el código en una parte de la aplicación sin necesidad de modificar el código en otras partes de la aplicación. Las aplicaciones acopladas de forma flexible son más resistentes a los cambios.

Actualmente, toda la lógica de validación y acceso a datos utilizada por la aplicación Contact Manager se encuentra en las clases de controlador. Es una mala idea. Siempre que necesite modificar una parte de la aplicación, corre el riesgo de introducir errores en otra parte de la aplicación. Por ejemplo, si modifica la lógica de validación, corre el riesgo de introducir nuevos errores en el acceso a datos o la lógica del controlador.

> [!NOTE] 
> 
> (SRP), una clase nunca debe tener más de una razón para cambiar. Mezclar controlador, validación y lógica de base de datos es una violación masiva del principio de responsabilidad única.

Hay varias razones por las que es posible que deba modificar la aplicación. Es posible que deba agregar una nueva característica a la aplicación, es posible que deba corregir un error en la aplicación o que tenga que modificar cómo se implementa una característica de la aplicación. Las aplicaciones rara vez son estáticas. Tienden a crecer y mutar con el tiempo.

Imagine, por ejemplo, que decide cambiar la forma de implementar la capa de acceso a datos. En este momento, la aplicación Contact Manager usa Microsoft Entity Framework para tener acceso a la base de datos. Sin embargo, puede decidir migrar a una tecnología de acceso a datos nueva o alternativa, como ADO.NET Data Services o NHibernate. Sin embargo, dado que el código de acceso a datos no está aislado del código de validación y controlador, no hay ninguna manera de modificar el código de acceso a datos en la aplicación sin modificar otro código que no esté directamente relacionado con el acceso a datos.

Cuando una aplicación está acoplada libremente, por otro lado, puede realizar cambios en una parte de una aplicación sin tocar otras partes de una aplicación. Por ejemplo, puede cambiar las tecnologías de acceso a datos sin modificar la lógica de validación o controlador.

En esta iteración, aprovechamos varios patrones de diseño de software que nos permiten refactorizar nuestra aplicación Contact Manager en una aplicación más acoplada. Cuando hayamos terminado, el Contact Manager no hará nada que no haya hecho antes. Sin embargo, podremos cambiar la aplicación más fácilmente en el futuro.

> [!NOTE] 
> 
> La refactorización es el proceso de reescribir una aplicación de tal manera que no pierda ninguna funcionalidad existente.

## <a name="using-the-repository-software-design-pattern"></a>Uso del patrón de diseño de software de repositorio

Nuestro primer cambio es aprovechar un patrón de diseño de software llamado patrón de repositorio. Usaremos el patrón Repository para aislar nuestro código de acceso a datos del resto de nuestra aplicación.

La implementación del patrón Derepositorio requiere que completemos los dos pasos siguientes:

1. Creación de una interfaz
2. Cree una clase concreta que implemente la interfaz

En primer lugar, necesitamos crear una interfaz que describa todos los métodos de acceso a datos que necesitamos realizar. La interfaz IContactManagerRepository se encuentra en el listado 1. Esta interfaz describe cinco métodos: CreateContact(), DeleteContact(), EditContact(), GetContact y ListContacts().

**Listado 1 - Modelos-IContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

A continuación, necesitamos crear una clase concreta que implemente la interfaz IContactManagerRepository. Dado que estamos usando Microsoft Entity Framework para tener acceso a la base de datos, crearemos una nueva clase denominada EntityContactManagerRepository. Esta clase se encuentra en el listado 2.

**Listado 2 - Modelos-EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Observe que la clase EntityContactManagerRepository implementa la interfaz IContactManagerRepository. La clase implementa los cinco métodos descritos por esa interfaz.

Tal vez te preguntes por qué tenemos que molestarnos con una interfaz. ¿Por qué necesitamos crear una interfaz y una clase que la implemente?

Con una excepción, el resto de nuestra aplicación interactuará con la interfaz y no con la clase concreta. En lugar de llamar a los métodos expuestos por la EntityContactManagerRepository clase, llamaremos a los métodos expuestos por el IContactManagerRepository interfaz.

De esta manera, podemos implementar la interfaz con una nueva clase sin necesidad de modificar el resto de nuestra aplicación. Por ejemplo, en alguna fecha futura, es posible que desee implementar una clase DataServicesContactManagerRepository que implemente la interfaz IContactManagerRepository. La clase DataServicesContactManagerRepository puede usar ADO.NET Data Services para tener acceso a una base de datos en lugar de Microsoft Entity Framework.

Si nuestro código de aplicación está programado en la interfaz IContactManagerRepository en lugar de la clase EntityContactManagerRepository concreta, podemos cambiar clases concretas sin modificar el resto de nuestro código. Por ejemplo, podemos cambiar de la clase EntityContactManagerRepository a la clase DataServicesContactManagerRepository sin modificar nuestra lógica de validación o acceso a datos.

La programación en interfaces (abstracciones) en lugar de clases concretas hace que nuestra aplicación sea más resistente al cambio.

> [!NOTE] 
> 
> Puede crear rápidamente una interfaz a partir de una clase concreta dentro de Visual Studio seleccionando la opción de menú Refactorizar, Extraer interfaz. Por ejemplo, puede crear primero la clase EntityContactManagerRepository y, a continuación, usar Extract Interface para generar automáticamente la interfaz IContactManagerRepository.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Uso del patrón de diseño de software de inserción de dependencias

Ahora que hemos migrado nuestro código de acceso a datos a una clase Repository independiente, necesitamos modificar nuestro controlador Contact para usar esta clase. Aprovecharemos un patrón de diseño de software llamado Dependency Injection para usar la clase Repository en nuestro controlador.

El controlador de contacto modificado se encuentra en el listado 3.

**Listado 3 - Controllers-ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Observe que el controlador de contacto en el listado 3 tiene dos constructores. El primer constructor pasa una instancia concreta de la IContactManagerRepository interfaz al segundo constructor. La clase de controlador Contact utiliza *Constructor Dependency Injection*.

El único lugar en el que se utiliza la clase EntityContactManagerRepository está en el primer constructor. El resto de la clase utiliza la interfaz IContactManagerRepository en lugar de la clase EntityContactManagerRepository concreta.

Esto facilita el cambio de implementaciones de la clase IContactManagerRepository en el futuro. Si desea utilizar la clase DataServicesContactRepository en lugar de la clase EntityContactManagerRepository, modifique el primer constructor.

La inserción de dependencias del constructor también hace que la clase de controlador de contacto sea muy comprobable. En las pruebas unitarias, puede crear instancias de la Contact controlador pasando una implementación simulada de la IContactManagerRepository clase. Esta característica de la inyección de dependencia sin será muy importante para nosotros en la siguiente iteración cuando compilemos pruebas unitarias para la aplicación Contact Manager.

> [!NOTE] 
> 
> Si desea desacoplar completamente la clase de controlador Contact de una implementación determinada de la interfaz IContactManagerRepository, puede aprovechar un marco de trabajo que admite la inserción de dependencias como StructureMap o Microsoft Entity Framework (MEF). Al aprovechar un marco de inserción de dependencias, nunca es necesario hacer referencia a una clase concreta en el código.

## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Es posible que haya notado que nuestra lógica de validación todavía está mezclada con nuestra lógica de controlador en la clase de controlador modificada en el listado 3. Por la misma razón que es una buena idea aislar nuestra lógica de acceso a datos, es una buena idea aislar nuestra lógica de validación.

Para solucionar este problema, podemos crear una capa de [*servicio*](http://martinfowler.com/eaaCatalog/serviceLayer.html)independiente. La capa de servicio es una capa independiente que podemos insertar entre nuestras clases de controlador y repositorio. La capa de servicio contiene nuestra lógica de negocios, incluida toda nuestra lógica de validación.

ContactManagerService se encuentra en el listado 4. Contiene la lógica de validación de la clase de controlador de contacto.

**Listado 4 - Modelos-ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Tenga en cuenta que el constructor para el ContactManagerService requiere un ValidationDictionary. La capa de servicio se comunica con la capa de controlador a través de este ValidationDictionary. Se describe el ValidationDictionary en detalle en la siguiente sección cuando se describe el Patrón Decorator.

Observe, además, que el ContactManagerService implementa el IContactManagerService interfaz. Siempre debe esforzarse por programar contra interfaces en lugar de clases concretas. Otras clases de la aplicación Contact Manager no interactúan directamente con la clase ContactManagerService. En su lugar, con una excepción, el resto de la aplicación Contact Manager se programa en la interfaz IContactManagerService.

La interfaz IContactManagerService se encuentra en el listado 5.

**Listado 5 - Models-IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

La clase de controlador Contact modificada se encuentra en el listado 6. Observe que el controlador Contact ya no interactúa con el repositorio ContactManager. En su lugar, el controlador de contacto interactúa con el servicio ContactManager. Cada capa está aislada tanto como sea posible de otras capas.

**Listado 6 - Controllers-ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Nuestra aplicación ya no se aplica al principio de responsabilidad única (SRP). El controlador de contacto en el listado 6 se ha eliminado de toda responsabilidad que no sea controlar el flujo de ejecución de la aplicación. Toda la lógica de validación se ha quitado del controlador de contacto y se ha insertado en la capa de servicio. Toda la lógica de la base de datos se ha insertado en la capa de repositorio.

## <a name="using-the-decorator-pattern"></a>Uso del patrón decorador

Queremos poder desacoplar completamente nuestra capa de servicio de nuestra capa de controlador. En principio, deberíamos ser capaces de compilar nuestra capa de servicio en un ensamblado independiente de nuestra capa de controlador sin necesidad de agregar una referencia a nuestra aplicación MVC.

Sin embargo, nuestra capa de servicio debe poder pasar mensajes de error de validación a la capa de controlador. ¿Cómo podemos habilitar la capa de servicio para comunicar mensajes de error de validación sin acoplar el controlador y la capa de servicio? Podemos aprovechar un patrón de diseño de software llamado el [patrón Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controlador utiliza un ModelStateDictionary denominado ModelState para representar errores de validación. Por lo tanto, es posible que tenga la tentación de pasar ModelState de la capa de controlador a la capa de servicio. Sin embargo, el uso de ModelState en la capa de servicio haría que la capa de servicio dependiera de una característica del marco de ASP.NET MVC. Esto sería malo porque, algún día, es posible que desee usar la capa de servicio con una aplicación WPFWPF en lugar de una aplicación ASP.NET MVC. En ese caso, no desea hacer referencia al marco de ASP.NET MVC para usar la ModelStateDictionary clase.

El patrón Decorator permite ajustar una clase existente en una nueva clase para implementar una interfaz. Nuestro proyecto Contact Manager incluye la clase ModelStateWrapper contenida en el listado 7. La clase ModelStateWrapper implementa la interfaz en el listado 8.

**Listado 7 - Modelos-Validation-ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Listado 8 - Modelos-Validation-IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Si echa un vistazo de cerca al listado 5, verá que la capa de servicio ContactManager utiliza la interfaz IValidationDictionary exclusivamente. El ContactManager servicio no depende de la ModelStateDictionary clase. Cuando el controlador Contact crea el servicio ContactManager, el controlador ajusta su ModelState de la siguiente manera:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Resumen

En esta iteración, no agregamos ninguna funcionalidad nueva a la aplicación Contact Manager. El objetivo de esta iteración era refactorizar la aplicación Contact Manager para que sea más fácil de mantener y modificar.

En primer lugar, implementamos el patrón de diseño de software de repositorio. Migramos todo el código de acceso a datos a una clase de repositorio ContactManager independiente.

También aislamos nuestra lógica de validación de nuestra lógica de controlador. Hemos creado una capa de servicio independiente que contiene todo nuestro código de validación. La capa de controlador interactúa con la capa de servicio y la capa de servicio interactúa con la capa de repositorio.

Cuando creamos la capa de servicio, aprovechamos el patrón Decorator para aislar ModelState de nuestra capa de servicio. En nuestra capa de servicio, programamos en la interfaz IValidationDictionary en lugar de ModelState.

Por último, aprovechamos un patrón de diseño de software denominado patrón de inserción de dependencias. Este patrón nos permite programar contra interfaces (abstracciones) en lugar de clases concretas. La implementación del patrón de diseño de inserción de dependencias también hace que nuestro código sea más comprobable. En la siguiente iteración, agregamos pruebas unitarias a nuestro proyecto.

> [!div class="step-by-step"]
> [Anterior](iteration-3-add-form-validation-cs.md)
> [Siguiente](iteration-5-create-unit-tests-cs.md)
