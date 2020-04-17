---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: '#5 de iteración: crear pruebas unitarias (C-) Microsoft Docs'
author: rick-anderson
description: En la quinta iteración, hacemos que nuestra aplicación sea más fácil de mantener y modificar agregando pruebas unitarias. Nos burlamos de nuestras clases de modelos de datos y creamos pruebas unitarias para...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: c005a8ffc3b09c126d796f2feb74d402cb784aa2
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542357"
---
# <a name="iteration-5--create-unit-tests-c"></a>Iteración n.º 5: Crear pruebas unitarias (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> En la quinta iteración, hacemos que nuestra aplicación sea más fácil de mantener y modificar agregando pruebas unitarias. Nos burlamos de nuestras clases de modelo de datos y creamos pruebas unitarias para nuestros controladores y lógica de validación.

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

En la iteración anterior de la aplicación Contact Manager, refactorizamos la aplicación para que se acoplase de forma más flexible. Separamos la aplicación en capas de controlador, servicio y repositorio distintas. Cada capa interactúa con la capa debajo de ella a través de interfaces.

Hemos refactorizado la aplicación para facilitar el mantenimiento y la modificación de la aplicación. Por ejemplo, si necesitamos usar una nueva tecnología de acceso a datos, simplemente podemos cambiar la capa del repositorio sin tocar el controlador o la capa de servicio. Al hacer que el Contact Manager se acopla libremente, hemos hecho que la aplicación sea más resistente al cambio.

Pero, ¿qué sucede cuando necesitamos agregar una nueva característica a la aplicación Contact Manager? O, ¿qué pasa cuando arreglamos un error? Una triste, pero bien probada, verdad de escribir código es que cada vez que tocas código creas el riesgo de introducir nuevos errores.

Por ejemplo, un buen día, es posible que el administrador le pida que agregue una nueva característica al Gestor de contactos. Quiere que agregues soporte para grupos de contacto. Quiere que permita a los usuarios organizar sus contactos en grupos como Amigos, Negocios, etc.

Para implementar esta nueva característica, deberá modificar las tres capas de la aplicación Contact Manager. Deberá agregar nueva funcionalidad a los controladores, la capa de servicio y el repositorio. Tan pronto como empiece a modificar el código, corre el riesgo de interrumpir la funcionalidad que funcionaba antes.

Refactorizar nuestra aplicación en capas separadas, como hicimos en la iteración anterior, fue algo bueno. Fue algo bueno porque nos permite hacer cambios en capas enteras sin tocar el resto de la aplicación. Sin embargo, si desea que el código dentro de una capa sea más fácil de mantener y modificar, debe crear pruebas unitarias para el código.

Utilice una prueba unitaria para probar una unidad de código individual. Estas unidades de código son más pequeñas que las capas de aplicación completas. Normalmente, se usa una prueba unitaria para comprobar si un método determinado en el código se comporta de la manera esperada. Por ejemplo, crearía una prueba unitaria para el método CreateContact() expuesto por la clase ContactManagerService.

Las pruebas unitarias para una aplicación funcionan igual que una red de seguridad. Siempre que modifique código en una aplicación, puede ejecutar un conjunto de pruebas unitarias para comprobar si la modificación interrumpe la funcionalidad existente. Las pruebas unitarias hacen que el código sea seguro de modificar. Las pruebas unitarias hacen que todo el código de la aplicación sea más resistente al cambio.

En esta iteración, agregamos pruebas unitarias a nuestra aplicación Contact Manager. De esta manera, en la siguiente iteración, podemos agregar grupos de contacto a nuestra aplicación sin preocuparse por interrumpir la funcionalidad existente.

> [!NOTE] 
> 
> Hay una variedad de marcos de pruebas unitarias que incluyen NUnit, xUnit.net y MbUnit. En este tutorial, usamos el marco de pruebas unitarias incluido con Visual Studio. Sin embargo, podría usar fácilmente uno de estos marcos alternativos.

## <a name="what-gets-tested"></a>Lo que se prueba

En el mundo perfecto, todo su código estaría cubierto por pruebas unitarias. En el mundo perfecto, tendrías la red de seguridad perfecta. Podrá modificar cualquier línea de código de la aplicación y saber al instante, mediante la ejecución de las pruebas unitarias, si el cambio interrumpió la funcionalidad existente.

Sin embargo, no vivimos en un mundo perfecto. En la práctica, al escribir pruebas unitarias, se concentra en escribir pruebas para la lógica de negocios (por ejemplo, lógica de validación). En particular, *no* se escriben pruebas unitarias para la lógica de acceso a datos ni la lógica de vista.

Para ser útiles, las pruebas unitarias deben ejecutarse muy rápidamente. Puede acumular fácilmente cientos (o incluso miles) de pruebas unitarias para una aplicación. Si las pruebas unitarias tardan mucho tiempo en ejecutarse, evitará ejecutarlas. En otras palabras, las pruebas unitarias de larga duración son inútiles para fines de codificación diario.

Por este motivo, normalmente no se escriben pruebas unitarias para el código que interactúa con una base de datos. Ejecutar cientos de pruebas unitarias en una base de datos activa sería demasiado lento. En su lugar, se burla de la base de datos y escribe código que interactúa con la base de datos simulada (analizamos la simulación de una base de datos a continuación).

Por un motivo similar, normalmente no se escriben pruebas unitarias para las vistas. Para probar una vista, debe girar hacia arriba un servidor web. Dado que girar un servidor web es un proceso relativamente lento, no se recomienda crear pruebas unitarias para las vistas.

Si la vista contiene lógica complicada, debe considerar la posibilidad de mover la lógica a métodos auxiliares. Puede escribir pruebas unitarias para los métodos auxiliares que se ejecutan sin poner en marcha un servidor web.

> [!NOTE] 
> 
> Aunque escribir pruebas para la lógica de acceso a datos o la lógica de vista no es una buena idea al escribir pruebas unitarias, estas pruebas pueden ser muy valiosas al crear pruebas funcionales o de integración.

> [!NOTE] 
> 
> ASP.NET MVC es el motor de vista de formularios Web Forms. Aunque el motor de vista de formularios Web Forms depende de un servidor web, es posible que otros motores de vista no lo sean.

## <a name="using-a-mock-object-framework"></a>Uso de un marco de objetos ficticios

Al crear pruebas unitarias, casi siempre es necesario aprovechar un marco de trabajo de objeto sorte. Un marco de trabajo de objetos falsos le permite crear simulaciones y códigos auxiliares para las clases de la aplicación.

Por ejemplo, puede usar un marco de trabajo de objeto sin fisios para generar una versión ficticia de la clase de repositorio. De este modo, puede utilizar la clase de repositorio simulado en lugar de la clase de repositorio real en las pruebas unitarias. El uso del repositorio ficticio le permite evitar la ejecución de código de base de datos al ejecutar una prueba unitaria.

Visual Studio no incluye un marco de objetos ficticios. Sin embargo, hay varios marcos de objetos Mock comerciales y de código abierto disponibles para .NET Framework:

1. Moq - Este marco está disponible bajo la licencia BSD de código abierto. Puede descargar Moq [https://code.google.com/p/moq/](https://code.google.com/p/moq/)desde .
2. Rhino Mocks - Este marco está disponible bajo la licencia BSD de código abierto. Puede descargar Rhino Mocks desde [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - Este es un marco comercial. Puede descargar una versión de prueba desde [http://www.typemock.com/](http://www.typemock.com/).

En este tutorial, decidí usar Moq. Sin embargo, podría utilizar Rhino Mocks o Typemock Isolator con la misma facilidad para crear los objetos Mock para la aplicación Contact Manager.

Antes de poder utilizar Moq, debe completar los siguientes pasos:

1. .
2. Antes de descomprimir la descarga, asegúrese de hacer clic con el botón derecho en el archivo y haga clic en el botón **Desbloquear** (consulte la figura 1).
3. Descomprima la descarga.
4. Agregue una referencia al ensamblaje Moq haciendo clic con el botón derecho en la carpeta Referencias del proyecto ContactManager.Tests y seleccionando **Agregar referencia**. En la pestaña Examinar, vaya a la carpeta donde descomprimió Moq y seleccione el ensamblado Moq.dll. Haga clic en el botón **Aceptar** .
5. Después de completar estos pasos, la carpeta Referencias debe tener el aspecto de la figura 2.

[![Desbloqueando Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Figura 01**: Desbloqueenmo moq ([Haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-cs/_static/image2.png))

[![Referencias después de añadir Moq](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Figura 02**: Referencias después de agregar Moq([Haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-cs/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Creación de pruebas unitarias para la capa de servicio

Vamos a empezar creando un conjunto de pruebas unitarias para nuestra capa de servicio de la aplicación Contact Manager s. Usaremos estas pruebas para verificar nuestra lógica de validación.

Cree una nueva carpeta denominada Modelos en el proyecto ContactManager.Tests. A continuación, haga clic con el botón derecho en la carpeta Modelos y seleccione **Agregar, Nueva prueba**. Aparece el cuadro de diálogo **Agregar nueva prueba** que se muestra en la Figura 3. Seleccione la plantilla **Prueba unitaria** y asigne un nombre a la nueva ContactManagerServiceTest.cs de prueba. Haga clic en el botón **Aceptar** para agregar la nueva prueba al proyecto de prueba.

> [!NOTE] 
> 
> En general, desea que la estructura de carpetas del proyecto de prueba coincida con la estructura de carpetas del proyecto MVC de ASP.NET. Por ejemplo, coloque las pruebas de controlador en una carpeta Controllers, las pruebas de modelo en una carpeta Models, etc.

[![Modelos-ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Figura 03**: Modelos-ContactManagerServiceTest.cs([Haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-cs/_static/image6.png))

Inicialmente, queremos probar el método CreateContact() expuesto por la clase ContactManagerService. Crearemos las siguientes cinco pruebas:

- CreateContact() - Pruebas que CreateContact() devuelve el valor true cuando se pasa un contacto válido al método.
- CreateContactRequiredFirstName() - Comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con un nombre que falta al método CreateContact().
- CreateContactRequiredLastName() - Comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con un apellido que falta al método CreateContact().
- CreateContactInvalidPhone() - Comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con un número de teléfono no válido al método CreateContact().
- CreateContactInvalidEmail(): comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con una dirección de correo electrónico no válida al método CreateContact().

La primera prueba comprueba que un contacto válido no genera un error de validación. Las pruebas restantes comprueban cada una de las reglas de validación.

El código de estas pruebas se encuentra en el listado 1.

**Listado 1 - Modelos-ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]

Dado que usamos la clase Contact en el listado 1, necesitamos agregar una referencia a Microsoft Entity Framework a nuestro proyecto de prueba. Agregue una referencia al ensamblado System.Data.Entity.

El listado 1 contiene un método denominado Initialize() que está decorado con el atributo [TestInitialize]. Este método se llama automáticamente antes de que se ejecute cada una de las pruebas unitarias (se llama 5 veces antes de cada una de las pruebas unitarias). El método Initialize() crea un repositorio ficticio con la siguiente línea de código:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Esta línea de código utiliza el marco Moq para generar un repositorio ficticio desde la interfaz IContactManagerRepository. El repositorio ficticio se utiliza en lugar del EntityContactManagerRepository real para evitar el acceso a la base de datos cuando se ejecuta cada prueba unitaria. El repositorio ficticio implementa los métodos de la interfaz IContactManagerRepository, pero los métodos no hacen nada realmente.

> [!NOTE] 
> 
> Cuando se utiliza el marco de \_trabajo Moq, hay una distinción entre mockRepository y \_mockRepository.Object. El primero hace&lt;referencia a&gt; la clase Mock IContactManagerRepository que contiene métodos para especificar cómo se comportará el repositorio ficticio. Este último hace referencia al repositorio ficticio real que implementa la interfaz IContactManagerRepository.

El repositorio ficticio se utiliza en el método Initialize() al crear una instancia de la clase ContactManagerService. Todas las pruebas unitarias individuales utilizan esta instancia de la ContactManagerService clase.

El listado 1 contiene cinco métodos que corresponden a cada una de las pruebas unitarias. Cada uno de estos métodos está decorado con el atributo [TestMethod]. Al ejecutar las pruebas unitarias, se llama a cualquier método que tenga este atributo. En otras palabras, cualquier método que esté decorado con el atributo [TestMethod] es una prueba unitaria.

La primera prueba unitaria, denominada CreateContact(), comprueba que al llamar a CreateContact() se devuelve el valor true cuando se pasa una instancia válida de la clase Contact al método. La prueba crea una instancia de la clase Contact, llama al método CreateContact() y comprueba que CreateContact() devuelve el valor true.

Las pruebas restantes comprueban que cuando se llama al método CreateContact() con un contacto no válido, el método devuelve false y el mensaje de error de validación esperado se agrega al estado del modelo. Por ejemplo, la prueba CreateContactRequiredFirstName() crea una instancia de la clase Contact con una cadena vacía para su propiedad FirstName. A continuación, se llama al método CreateContact() con el contacto no válido. Por último, la prueba comprueba que CreateContact() devuelve false y que el estado del modelo contiene el mensaje de error de validación esperado "Se requiere el nombre."

Puede ejecutar las pruebas unitarias en el listado 1 seleccionando la opción de menú **Prueba, Ejecutar, Todas las pruebas en solución (CTRL+R, A)**. Los resultados de las pruebas se visualizan en la ventana Resultados de la prueba (consulte la figura 4).

[![Resultados de la prueba](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Figura 04**: Resultados de la prueba ([Haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-cs/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Creación de pruebas unitarias para controladores

La aplicación ASP.NETMVC controla el flujo de interacción del usuario. Al probar un controlador, desea probar si el controlador devuelve el resultado de la acción correcta y ver los datos. También es posible que desee probar si un controlador interactúa con las clases de modelo de la manera esperada.

Por ejemplo, el listado 2 contiene dos pruebas unitarias para el método Create() del controlador de contacto. La primera prueba unitaria comprueba que cuando se pasa un contacto válido al método Create(), el método Create() redirige a la acción Index. En otras palabras, cuando se pasa un contacto válido, el método Create() debe devolver un RedirectToRouteResult que representa la acción Index.

No queremos probar la capa de servicio ContactManager cuando estamos probando la capa de controlador. Por lo tanto, nos simulamos la capa de servicio con el siguiente código en el método Initialize:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

En la prueba unitaria CreateValidContact(), simulamos el comportamiento de llamar al método CreateContact() de la capa de servicio con la siguiente línea de código:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Esta línea de código hace que el servicio ContactManager ficticio devuelva el valor true cuando se llama a su método CreateContact(). Al burlarnos de la capa de servicio, podemos probar el comportamiento de nuestro controlador sin necesidad de ejecutar ningún código en la capa de servicio.

La segunda prueba unitaria comprueba que la acción Create() devuelve la vista Create cuando se pasa un contacto no válido al método. Hacemos que el método CreateContact() de la capa de servicio devuelva el valor false con la siguiente línea de código:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Si el método Create() se comporta como esperamos, debe devolver la vista Create cuando la capa de servicio devuelve el valor false. De este modo, el controlador puede mostrar los mensajes de error de validación en la vista Crear y el usuario tiene la oportunidad de corregir las propiedades contactno no válidas.

Si planea crear pruebas unitarias para los controladores, debe devolver nombres de vista explícitos de las acciones del controlador. Por ejemplo, no devuelva una vista como esta:

devolver View();

En su lugar, devuelva la vista de la siguiente manera:

return View("Create");

Si no es explícito al devolver una vista, la propiedad ViewResult.ViewName devuelve una cadena vacía.

**Listado 2 - Controllers-ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Resumen

En esta iteración, hemos creado pruebas unitarias para nuestra aplicación Contact Manager. Podemos ejecutar estas pruebas unitarias en cualquier momento para comprobar que nuestra aplicación todavía se comporta de la manera que esperamos. Las pruebas unitarias actúan como una red de seguridad para nuestra aplicación que nos permite modificar de forma segura nuestra aplicación en el futuro.

Creamos dos conjuntos de pruebas unitarias. En primer lugar, probamos nuestra lógica de validación mediante la creación de pruebas unitarias para nuestra capa de servicio. A continuación, probamos nuestra lógica de control de flujo mediante la creación de pruebas unitarias para nuestra capa de controlador. Al probar nuestra capa de servicio, aislamos nuestras pruebas para nuestra capa de servicio de nuestra capa de repositorio burlándose de nuestra capa de repositorio. Al probar la capa de controlador, aislamos nuestras pruebas para nuestra capa de controlador burlándonos de la capa de servicio.

En la siguiente iteración, modificamos la aplicación Contact Manager para que admita grupos de contactos. Agregaremos esta nueva funcionalidad a nuestra aplicación mediante un proceso de diseño de software llamado desarrollo controlado por pruebas.

> [!div class="step-by-step"]
> [Anterior](iteration-4-make-the-application-loosely-coupled-cs.md)
> [Siguiente](iteration-6-use-test-driven-development-cs.md)
