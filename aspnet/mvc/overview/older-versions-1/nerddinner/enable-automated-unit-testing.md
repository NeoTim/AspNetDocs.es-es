---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar las pruebas de unidades automatizadas Microsoft Docs
author: rick-anderson
description: El paso 12 muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que verifican nuestra funcionalidad NerdDinner, y que nos darán la confianza para hacer cambios...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 7fe84efd9e4cc359c19d5ab9e22c579b80207e9c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541473"
---
# <a name="enable-automated-unit-testing"></a>Habilitar las pruebas unitarias automatizadas

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 12 de un tutorial gratuito de la [aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) que describe cómo crear una aplicación web pequeña, pero completa, mediante ASP.NET MVC 1.
> 
> El paso 12 muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que verifican nuestra funcionalidad NerdDinner, y que nos darán la confianza para realizar cambios y mejoras en la aplicación en el futuro.
> 
> Si usa ASP.NET MVC 3, le recomendamos que siga los tutoriales [Introducción a MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner Paso 12: Pruebas unitarias

Vamos a desarrollar un conjunto de pruebas unitarias automatizadas que verifican nuestra funcionalidad NerdDinner, y que nos dará la confianza para realizar cambios y mejoras en la aplicación en el futuro.

### <a name="why-unit-test"></a>¿Por qué la prueba unitaria?

En el camino hacia el trabajo una mañana tienes un repentino destello de inspiración sobre una aplicación en la que estás trabajando. Te das cuenta de que hay un cambio que puedes implementar que hará que la aplicación sea dramáticamente mejor. Puede ser una refactorización que limpia el código, agrega una nueva característica o corrige un error.

La pregunta que te enfrentacuando cuando llegas a tu computadora es: "¿qué tan seguro es hacer esta mejora?" ¿Qué pasa si hacer el cambio tiene efectos secundarios o rompe algo? El cambio puede ser simple y solo tarda unos minutos en implementarse, pero ¿qué sucede si tarda horas en probar manualmente todos los escenarios de aplicación? ¿Qué pasa si olvida cubrir un escenario y una aplicación rota entra en producción? ¿Realmente vale la pena hacer esta mejora?

Las pruebas unitarias automatizadas pueden proporcionar una red de seguridad que le permite mejorar continuamente sus aplicaciones y evitar tener miedo del código en el que está trabajando. Tener pruebas automatizadas que verifiquen rápidamente la funcionalidad le permite codificar con confianza y le permiten realizar mejoras que de otro modo no se habría sentido cómodo haciendo. También ayudan a crear soluciones que son más mantenibles y tienen una vida útil más larga, lo que conduce a un retorno de la inversión mucho mayor.

El marco de trabajo de ASP.NET MVC facilita y natural la funcionalidad de la aplicación de prueba unitaria. También permite un flujo de trabajo de desarrollo controlado por pruebas (TDD) que permite el desarrollo basado en pruebas en primer lugar.

### <a name="nerddinnertests-project"></a>Proyecto NerdDinner.Tests

Cuando creamos nuestra aplicación NerdDinner al principio de este tutorial, se nos preguntó con un cuadro de diálogo que preguntaba si queríamos crear un proyecto de prueba unitaria para ir junto con el proyecto de aplicación:

![](enable-automated-unit-testing/_static/image1.png)

Mantuvimos seleccionado el botón de opción "Sí, crear un proyecto de prueba unitaria", lo que dio lugar a que se agregara un proyecto "NerdDinner.Tests" a nuestra solución:

![](enable-automated-unit-testing/_static/image2.png)

El proyecto NerdDinner.Tests hace referencia al ensamblado del proyecto de aplicación NerdDinner y nos permite agregar fácilmente pruebas automatizadas que comprueban la funcionalidad de la aplicación.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Creación de pruebas unitarias para nuestra clase de modelo de cena

Vamos a agregar algunas pruebas a nuestro proyecto NerdDinner.Tests que verifican la clase Dinner que creamos cuando creamos nuestra capa de modelo.

Comenzaremos creando una nueva carpeta dentro de nuestro proyecto de prueba llamada "Modelos" donde colocaremos nuestras pruebas relacionadas con el modelo. A continuación, haremos clic con el botón derecho en la carpeta y elegiremos el comando de menú **Agregar&gt;nueva prueba.** Esto abrirá el cuadro de diálogo "Agregar nueva prueba".

Elegiremos crear una "Prueba unitaria" y la llamaremos "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Al hacer clic en el botón "aceptar" Visual Studio agregará (y abrirá) un archivo de DinnerTest.cs al proyecto:

![](enable-automated-unit-testing/_static/image4.png)

La plantilla de prueba unitaria predeterminada de Visual Studio tiene un montón de código reutilizable dentro de ella que me parece un poco desordenado. Vamos a limpiarlo para contener el siguiente código:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

El atributo [TestClass] de la clase DinnerTest anterior lo identifica como una clase que contendrá pruebas, así como código opcional de inicialización y desmontaje de pruebas. Podemos definir pruebas dentro de él agregando métodos públicos que tienen un atributo [TestMethod] en ellos.

A continuación se muestra la primera de dos pruebas que agregaremos que ejercita nuestra clase De cena. La primera prueba comprueba que nuestra cena no es válida si se crea una nueva cena sin que todas las propiedades se establezcan correctamente. La segunda prueba comprueba que nuestra cena es válida cuando una cena tiene todas las propiedades establecidas con valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Usted notará anteriormente que nuestros nombres de prueba son muy explícitos (y algo detallados). Estamos haciendo esto porque podríamos terminar creando cientos o miles de pruebas pequeñas, y queremos que sea fácil determinar rápidamente la intención y el comportamiento de cada uno de ellos (especialmente cuando estamos mirando a través de una lista de errores en un corredor de pruebas). Los nombres de prueba deben tener el nombre de la funcionalidad que están probando. Arriba estamos usando un\_\_patrón de nomenclatura "Sustantivo Should Verb".

Estamos estructurando las pruebas usando el patrón de prueba "AAA", que significa "Organizar, Actuar, Afirmar":

- Organizar: Configure la unidad que se está probando
- Acto: Ejercitar la unidad bajo prueba y capturar resultados
- Afirmar: Verifique el comportamiento

Cuando escribimos pruebas queremos evitar que las pruebas individuales hagan demasiado. En su lugar, cada prueba debe verificar solo un concepto (lo que hará que sea mucho más fácil identificar la causa de los errores). Una buena directriz es tratar de tener una sola instrucción de aserción para cada prueba. Si tiene más de una instrucción assert en un método de prueba, asegúrese de que se utilizan todos para probar el mismo concepto. En caso de duda, haga otra prueba.

### <a name="running-tests"></a>Ejecutar pruebas

Visual Studio 2008 Professional (y ediciones superiores) incluye un ejecutor de pruebas integrado que se puede usar para ejecutar proyectos de prueba unitaria de Visual Studio dentro del IDE. Podemos seleccionar el comando de menú **Prueba-&gt;Ejecutar-&gt;Todas las pruebas en la solución** (o escribir Ctrl R, A) para ejecutar todas nuestras pruebas unitarias. O alternativamente podemos colocar nuestro cursor dentro de una clase de prueba específica o método de prueba y usar el comando de menú **Test-&gt;Run-&gt;Tests in Current Context** (o escriba Ctrl R, T) para ejecutar un subconjunto de las pruebas unitarias.

Vamos a colocar nuestro cursor dentro de la clase DinnerTest y escriba "Ctrl R, T" para ejecutar las dos pruebas que acabamos de definir. Cuando hagamos esto, aparecerá una ventana "Resultados de la prueba" en Visual Studio y veremos los resultados de nuestra ejecución de prueba que se enumeran en ella:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: La ventana de resultados de la prueba VS no muestra la columna del nombre de clase por abandono. Puede agregar esto haciendo clic con el botón derecho en la ventana Resultados de la prueba y utilizando el comando de menú Agregar o quitar columnas.*

Nuestras dos pruebas tardaron sólo una fracción de segundo en ejecutarse, y como puede ver ambas pasaron. Ahora podemos continuar y aumentarlos creando pruebas adicionales que verifiquen validaciones de reglas específicas, así como cubrir los dos métodos auxiliares - IsUserHost() e IsUserRegistered() - que agregamos a la clase Dinner. Tener todas estas pruebas en su lugar para la clase Dinner hará que sea mucho más fácil y seguro agregarnuevas reglas de negocio y validaciones en el futuro. Podemos agregar nuestra nueva lógica de reglas a Dinner y, a continuación, en cuestión de segundos comprobar que no ha roto ninguna de nuestras funciones lógicas anteriores.

Observe cómo el uso de un nombre de prueba descriptivo facilita la comprensión rápida de lo que comprueba cada prueba. Recomiendo usar el comando de menú **Herramientas-&gt;Opciones,** abrir la pantalla de configuración Herramientas de prueba-&gt;Ejecución de prueba y comprobar la casilla de verificación "Hacer doble clic en un resultado de prueba unitaria fallido o no concluyente muestra el punto de error en la prueba". Esto le permitirá hacer doble clic en un error en la ventana de resultados de la prueba y saltar inmediatamente al error de aserción.

### <a name="creating-dinnerscontroller-unit-tests"></a>Creación de pruebas unitarias DinnersController

Ahora vamos a crear algunas pruebas unitarias que verifican nuestra funcionalidad DinnersController. Comenzaremos haciendo clic con el botón derecho en la carpeta "Controladores" dentro de nuestro proyecto de prueba y, a continuación, elegiremos el comando de menú **Agregar&gt;nueva prueba.** Crearemos una "Prueba unitaria" y la llamaremos "DinnersControllerTest.cs".

Crearemos dos métodos de prueba que comprueban el método de acción Details() en DinnersController. El primero comprobará que se devuelve una vista cuando se solicita una cena existente. El segundo comprobará que se devuelve una vista "NotFound" cuando se solicita una cena inexistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

El código anterior compila clean. Sin embargo, cuando ejecutamos las pruebas, ambas fallan:

![](enable-automated-unit-testing/_static/image6.png)

Si examinamos los mensajes de error, veremos que la razón por la que se produjo un error en las pruebas fue porque nuestra clase DinnersRepository no pudo conectarse a una base de datos. Nuestra aplicación NerdDinner está utilizando una cadena de conexión a un\_archivo local de SQL Server Express que se encuentra en el directorio de datos de la aplicación del proyecto de aplicación NerdDinner. Dado que nuestro proyecto NerdDinner.Tests compila y ejecuta en un directorio diferente y, a continuación, en el proyecto de aplicación, la ubicación de ruta de acceso relativa de nuestra cadena de conexión es incorrecta.

*Podríamos* corregir esto copiando el archivo de base de datos de SQL Express en nuestro proyecto de prueba y, a continuación, agregarle una cadena de conexión de prueba adecuada en el archivo App.config de nuestro proyecto de prueba. Esto desbloquearía y ejecutaría las pruebas anteriores.

Sin embargo, el código de prueba unitaria con una base de datos real trae consigo una serie de desafíos. Concretamente:

- Reduce significativamente el tiempo de ejecución de las pruebas unitarias. Cuanto más tiempo tarde en ejecutar pruebas, menos probabilidades tendrá de ejecutarlas con frecuencia. Lo ideal es que desee que sus pruebas unitarias se puedan ejecutar en cuestión de segundos, y que sea algo que haga de forma tan natural como compilar el proyecto.
- Complica la lógica de configuración y limpieza dentro de las pruebas. Desea que cada prueba unitaria sea aislada e independiente de otras (sin efectos secundarios ni dependencias). Cuando se trabaja con una base de datos real, debe tener en cuenta el estado y restablecerlo entre pruebas.

Echemos un vistazo a un patrón de diseño llamado "inyección de dependencia" que puede ayudarnos a solucionar estos problemas y evitar la necesidad de utilizar una base de datos real con nuestras pruebas.

### <a name="dependency-injection"></a>Inserción de dependencias

En este momento DinnersController está estrechamente "acoplado" a la DinnerRepository clase. "Acoplamiento" se refiere a una situación en la que una clase se basa explícitamente en otra clase para trabajar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Dado que la clase DinnerRepository requiere acceso a una base de datos, la dependencia estrechamente acoplada que tiene la clase DinnersController en DinnerRepository termina requiriendo que tengamos una base de datos para que se prueben los métodos de acción DinnersController.

Podemos evitar esto empleando un patrón de diseño denominado "inserción de dependencias", que es un enfoque donde las dependencias (como las clases de repositorio que proporcionan acceso a datos) ya no se crean implícitamente dentro de las clases que las utilizan. En su lugar, las dependencias se pueden pasar explícitamente a la clase que las usa mediante argumentos de constructor. Si las dependencias se definen mediante interfaces, tenemos la flexibilidad de pasar implementaciones de dependencia "falsas" para escenarios de prueba unitaria. Esto nos permite crear implementaciones de dependencia específicas de prueba que en realidad no requieren acceso a una base de datos.

Para ver esto en acción, vamos a implementar la inserción de dependencias con nuestro DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraer una interfaz IDinnerRepository

Nuestro primer paso será crear una nueva interfaz IDinnerRepository que encapsula el contrato de repositorio que nuestros controladores requieren para recuperar y actualizar Dinners.

Podemos definir este contrato de interfaz manualmente haciendo clic con el botón derecho en la carpeta Desmodelos y, a continuación, eligiendo el comando de menú **Agregar nuevo&gt;elemento** y creando una nueva interfaz denominada IDinnerRepository.cs.

Como alternativa, podemos usar las herramientas de refactorización integradas en Visual Studio Professional (y ediciones superiores) para extraer y crear automáticamente una interfaz para nosotros a partir de nuestra clase DinnerRepository existente. Para extraer esta interfaz mediante VS, simplemente coloque el cursor en el editor de texto en la clase DinnerRepository y, a continuación, haga clic con el botón derecho y elija el comando de menú **Refactorizar&gt;la interfaz de extracción:**

![](enable-automated-unit-testing/_static/image7.png)

Esto iniciará el cuadro de diálogo "Extraer interfaz" y nos pedirá el nombre de la interfaz que se va a crear. El valor predeterminado será IDinnerRepository y seleccionará automáticamente todos los métodos públicos en la clase DinnerRepository existente para agregarlos a la interfaz:

![](enable-automated-unit-testing/_static/image8.png)

Al hacer clic en el botón "aceptar", Visual Studio agregará una nueva interfaz IDinnerRepository a nuestra aplicación:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Y nuestra clase DinnerRepository existente se actualizará para que implemente la interfaz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Actualización de DinnersController para admitir la inyección de constructores

Ahora actualizaremos la clase DinnersController para usar la nueva interfaz.

Actualmente DinnersController está codificado de forma rígida de modo que su campo "dinnerRepository" siempre es una clase DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Lo cambiaremos para que el campo "dinnerRepository" sea de tipo IDinnerRepository en lugar de DinnerRepository. A continuación, agregaremos dos constructores DinnersController públicos. Uno de los constructores permite que un IDinnerRepository se pase como argumento. El otro es un constructor predeterminado que utiliza nuestra implementación DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Dado que ASP.NET MVC de forma predeterminada crea clases de controlador mediante constructores predeterminados, nuestro DinnersController en tiempo de ejecución seguirá usando la Clase DinnerRepository para realizar el acceso a los datos.

Ahora podemos actualizar nuestras pruebas unitarias, sin embargo, para pasar una implementación de repositorio de cena "falsa" utilizando el constructor de parámetros. Este repositorio de cenas "falso" no requerirá acceso a una base de datos real y, en su lugar, utilizará datos de ejemplo en memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Creación de la clase FakeDinnerRepository

Vamos a crear una clase FakeDinnerRepository.

Comenzaremos creando un directorio "Fakes" dentro de nuestro proyecto NerdDinner.Tests y luego agregaremos una nueva clase FakeDinnerRepository a él (haga clic con el botón derecho en la carpeta y elija **Add-&gt;New Class**):

![](enable-automated-unit-testing/_static/image9.png)

Actualizaremos el código para que la clase FakeDinnerRepository implemente la interfaz IDinnerRepository. A continuación, podemos hacer clic con el botón derecho en él y elegir el comando de menú contextual "Implement interface IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Esto hará que Visual Studio agregue automáticamente todos los miembros de la interfaz IDinnerRepository a nuestra clase FakeDinnerRepository con implementaciones predeterminadas de "stub out":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

A continuación, podemos actualizar la implementación de FakeDinnerRepository para trabajar en una colección List&lt;Dinner&gt; en memoria que se le pasa como argumento de constructor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Ahora tenemos una implementación Falsa de IDinnerRepository que no requiere una base de datos y, en su lugar, puede trabajar en una lista en memoria de Dinner objetos.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Uso de FakeDinnerRepository con pruebas unitarias

Volvamos a las pruebas unitarias DinnersController que fallaron anteriormente porque la base de datos no estaba disponible. Podemos actualizar los métodos de prueba para usar un FakeDinnerRepository rellenado con datos de cena en memoria de ejemplo a DinnersController utilizando el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Y ahora, cuando ejecutamos estas pruebas, ambos pasan:

![](enable-automated-unit-testing/_static/image11.png)

Lo mejor de todo es que tardan solo una fracción de segundo en ejecutarse y no requieren ninguna lógica complicada de configuración/limpieza. Ahora podemos probar unitariamente todo nuestro código de método de acción DinnersController (incluyendo la lista, paginación, detalles, crear, actualizar y eliminar) sin necesidad de conectarse a una base de datos real.

| **Tema paralelo: Marcos de inserción de dependencias** |
| --- |
| Realizar la inserción manual de dependencias (como estamos arriba) funciona bien, pero se vuelve más difícil de mantener a medida que aumenta el número de dependencias y componentes en una aplicación. Existen varios marcos de inserción de dependencias para .NET que pueden ayudar a proporcionar aún más flexibilidad de administración de dependencias. Estos marcos, también denominados a veces contenedores de "Inversión de control" (IoC), proporcionan mecanismos que permiten un nivel adicional de compatibilidad de configuración para especificar y pasar dependencias a objetos en tiempo de ejecución (la mayoría de las veces mediante la inyección de constructores). Algunos de los marcos más populares de OSS Dependency Injection / IOC en .NET incluyen: AutoFac, Ninject, Spring.NET, StructureMap y Windsor. ASP.NET MVC expone las API de extensibilidad que permiten a los desarrolladores participar en la resolución y creación de instancias de controladores y que permiten que los marcos de inserción de dependencias/IoC se integren limpiamente dentro de este proceso. El uso de un marco DI/IOC también nos permitiría quitar el constructor predeterminado de nuestro DinnersController, lo que eliminaría por completo el acoplamiento entre él y DinnerRepository. No usaremos un marco de inserción de dependencias / COI con nuestra aplicación NerdDinner. Pero es algo que podríamos considerar para el futuro si la base de código Y las capacidades de NerdDinner crecieran. |

### <a name="creating-edit-action-unit-tests"></a>Creación de pruebas de unidad de acción de edición

Ahora vamos a crear algunas pruebas unitarias que comprueban la funcionalidad de edición de DinnersController. Comenzaremos probando la versión HTTP-GET de nuestra acción Editar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Crearemos una prueba que comprueba que un View respaldado por un DinnerFormViewModel objeto se representa cuando se solicita una cena válida:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Sin embargo, cuando ejecutamos la prueba, encontraremos que se produce un error porque se produce una excepción de referencia nula cuando el método Edit tiene acceso a la propiedad User.Identity.Name para realizar la comprobación Dinner.IsHostedBy().

El objeto User de la clase base Controller encapsula detalles sobre el usuario que ha iniciado sesión y se rellena con ASP.NET MVC cuando crea el controlador en tiempo de ejecución. Dado que estamos probando DinnersController fuera de un entorno de servidor web, el objeto User no está establecido (por lo tanto, la excepción de referencia nula).

### <a name="mocking-the-useridentityname-property"></a>Burlar la propiedad User.Identity.Name

Los marcos de simulación facilitan las pruebas al permitirnos crear dinámicamente versiones falsas de objetos dependientes que admiten nuestras pruebas. Por ejemplo, podemos usar un marco de simulación en nuestra prueba de acción Editar para crear dinámicamente un objeto User que nuestro DinnersController puede usar para buscar un nombre de usuario simulado. Esto evitará que se produzca una referencia nula cuando ejecutemos nuestra prueba.

Hay muchos marcos de simulación de .NET que se pueden usar con [http://www.mockframeworks.com/](http://www.mockframeworks.com/)ASP.NET MVC (puede ver una lista de ellos aquí: ). Para probar nuestra aplicación NerdDinner usaremos un marco de simulación de código abierto llamado [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)"Moq", que se puede descargar de forma gratuita desde .

Una vez descargado, agregaremos una referencia en nuestro proyecto NerdDinner.Tests al ensamblado Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

A continuación, agregaremos un método auxiliar "CreateDinnersControllerAs(username)" a nuestra clase de prueba que toma un nombre de usuario como parámetro y que, a continuación, "mocks" de la propiedad User.Identity.Name en la instancia DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Arriba estamos usando Moq para crear un objeto Mock que falsifica un objeto ControllerContext (que es lo que ASP.NET MVC pasa a las clases Controller para exponer objetos en tiempo de ejecución como User, Request, Response y Session). Estamos llamando al método "SetupGet" en el Mock para indicar que la propiedad HttpContext.User.Identity.Name en ControllerContext debe devolver la cadena de nombre de usuario que pasamos al método auxiliar.

Podemos simular cualquier número de ControllerContext propiedades y métodos. Para ilustrar esto, también he agregado una llamada SetupGet() para la propiedad Request.IsAuthenticated (que en realidad no es necesaria para las pruebas siguientes, pero que ayuda a ilustrar cómo se pueden simular las propiedades de solicitud). Cuando terminamos, asignamos una instancia de la simulación ControllerContext a DinnersController devuelve nuestro método auxiliar.

Ahora podemos escribir pruebas unitarias que utilizan este método auxiliar para probar escenarios de edición que implican diferentes usuarios:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Y ahora cuando ejecutamos las pruebas pasan:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Probar escenarios UpdateModel()

Hemos creado pruebas que cubren la versión HTTP-GET de la acción Editar. Ahora vamos a crear algunas pruebas que comprueban la versión HTTP-POST de la acción Editar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

El nuevo escenario de prueba interesante que admitimos con este método de acción es su uso del método auxiliar UpdateModel() en la clase base Controller. Estamos usando este método auxiliar para enlazar los valores de form-post a nuestra instancia de objeto Dinner.

A continuación se muestran dos pruebas que demuestran cómo podemos proporcionar valores publicados de formulario para el método auxiliar UpdateModel() que se va a utilizar. Para ello, crearemos y rellenaremos un objeto FormCollection y, a continuación, lo asignaremos a la propiedad "ValueProvider" en el controlador.

La primera prueba verifica que en un guardado correcto el navegador se redirige a la acción de los detalles. La segunda prueba comprueba que cuando se registra una entrada no válida, la acción vuelve a mostrar la vista de edición con un mensaje de error.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Prueba de envoltura

Hemos cubierto los conceptos básicos involucrados en las clases de controlador de pruebas unitarias. Podemos utilizar estas técnicas para crear fácilmente cientos de pruebas simples que verifiquen el comportamiento de nuestra aplicación.

Debido a que nuestras pruebas de controlador y modelo no requieren una base de datos real, son extremadamente rápidas y fáciles de ejecutar. Podremos ejecutar cientos de pruebas automatizadas en segundos e inmediatamente recibir comentarios sobre si un cambio que hicimos rompió algo. Esto nos ayudará a proporcionar la confianza para mejorar, refactorizar y refinar continuamente nuestra aplicación.

Cubrimos las pruebas como el último tema de este capítulo, ¡pero no porque las pruebas sean algo que debe hacer al final de un proceso de desarrollo! Por el contrario, debe escribir pruebas automatizadas tan pronto como sea posible en su proceso de desarrollo. Esto le permite obtener comentarios inmediatos a medida que desarrolla, le ayuda a pensar cuidadosamente en los escenarios de casos de uso de la aplicación y le guía a diseñar su aplicación con capas y acoplamiento limpios en mente.

Un capítulo posterior del libro tratará el desarrollo controlado por pruebas (TDD) y cómo usarlo con ASP.NET MVC. TDD es una práctica de codificación iterativa en la que primero se escriben las pruebas que el código resultante satisfará. Con TDD comienza cada característica creando una prueba que comprueba la funcionalidad que está a punto de implementar. Escribir la prueba unitaria primero ayuda a asegurarse de que entiende claramente la característica y cómo se supone que funciona. Solo después de escribir la prueba (y ha comprobado que se produce un error) implemente la funcionalidad real que comprueba la prueba. Dado que ya ha dedicado tiempo a pensar en el caso de uso de cómo se supone que funciona la característica, tendrá una mejor comprensión de los requisitos y la mejor manera de implementarlos. Cuando haya terminado con la implementación, puede volver a ejecutar la prueba y obtener comentarios inmediatos sobre si la característica funciona correctamente. Cubriremos TDD más en el Capítulo 10.

### <a name="next-step"></a>siguiente paso

Algunos comentarios finales.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-implement-mapping-scenarios.md)
> [Siguiente](nerddinner-wrap-up.md)
