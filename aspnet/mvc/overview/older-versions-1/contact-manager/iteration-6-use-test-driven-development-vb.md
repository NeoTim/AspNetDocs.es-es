---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: '#6 de iteración: utilice el desarrollo controlado por pruebas (VB) Microsoft Docs'
author: rick-anderson
description: En esta sexta iteración, agregamos nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 8464f4cdee673ef75d561e4cf89613fdca2c16af
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542812"
---
# <a name="iteration-6--use-test-driven-development-vb"></a>Iteración n.º 6: Usar el desarrollo mediante pruebas (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> En esta sexta iteración, agregamos nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

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

En la iteración anterior de la aplicación Contact Manager, creamos pruebas unitarias para proporcionar una red de seguridad para nuestro código. La motivación para crear las pruebas unitarias era hacer que nuestro código fuera más resistente al cambio. Con las pruebas unitarias en su lugar, podemos hacer felizmente cualquier cambio en nuestro código e inmediatamente saber si hemos roto la funcionalidad existente.

En esta iteración, usamos pruebas unitarias para un propósito completamente diferente. En esta iteración, usamos pruebas unitarias como parte de una filosofía de diseño de aplicación denominada *desarrollo controlado por pruebas.* Cuando se practica el desarrollo controlado por pruebas, primero se escriben pruebas y, a continuación, se escribe código en las pruebas.

Más precisamente, al practicar el desarrollo controlado por pruebas, hay tres pasos que se completan al crear código (Rojo/Verde/Refactor):

1. Escribir una prueba unitaria que falle (Rojo)
2. Escribir código que pase la prueba unitaria (verde)
3. Refactorizar el código (Refactorizar)

En primer lugar, escriba la prueba unitaria. La prueba unitaria debe expresar su intención de cómo espera que se comporte el código. La primera vez que cree la prueba unitaria, la prueba unitaria debe fallar. La prueba debe producir un error porque aún no ha escrito ningún código de aplicación que satisfaga la prueba.

A continuación, escriba suficiente código para que pase la prueba unitaria. El objetivo es escribir el código de la manera más laz, más descuidada y rápida posible. No debe perder el tiempo pensando en la arquitectura de su aplicación. En su lugar, debe centrarse en escribir la cantidad mínima de código necesaria para satisfacer la intención expresada por la prueba unitaria.

Por último, después de haber escrito suficiente código, puede retroceder y considerar la arquitectura general de la aplicación. En este paso, reescribirá (refactorizará) el código aprovechando los patrones de diseño de software, como el patrón de repositorio, para que el código sea más mantenible. Puede reescribir sin miedo el código en este paso porque el código está cubierto por pruebas unitarias.

Hay muchos beneficios que resultan de la práctica del desarrollo basado en pruebas. En primer lugar, el desarrollo controlado por pruebas le obliga a centrarse en el código que realmente necesita ser escrito. Debido a que constantemente se centra en escribir suficiente código para pasar una prueba en particular, se le impide vagar por las malas hierbas y escribir cantidades masivas de código que nunca usará.

En segundo lugar, una metodología de diseño "test first" le obliga a escribir código desde la perspectiva de cómo se usará el código. En otras palabras, cuando se practica el desarrollo basado en pruebas, se escriben constantemente las pruebas desde la perspectiva del usuario. Por lo tanto, el desarrollo controlado por pruebas puede dar lugar a API más limpias y comprensibles.

Por último, el desarrollo controlado por pruebas le obliga a escribir pruebas unitarias como parte del proceso normal de escritura de una aplicación. A medida que se acerca una fecha límite del proyecto, las pruebas suelen ser lo primero que sale por la ventana. Al practicar el desarrollo basado en pruebas, por otro lado, es más probable que sea virtuoso sobre la escritura de pruebas unitarias porque el desarrollo basado en pruebas hace que las pruebas unitarias sean fundamentales para el proceso de creación de una aplicación.

> [!NOTE] 
> 
> Para obtener más información sobre el desarrollo basado en pruebas, le recomiendo que lea el libro de Michael Feathers **Trabajar eficazmente con código heredado**.

En esta iteración, agregamos una nueva característica a nuestra aplicación Contact Manager. Agregamos soporte para grupos de contacto. Puedes usar Grupos de contactos para organizar tus contactos en categorías como Grupos de empresas y amigos.

Agregaremos esta nueva funcionalidad a nuestra aplicación siguiendo un proceso de desarrollo controlado por pruebas. Primero escribiremos nuestras pruebas unitarias y escribiremos todo nuestro código con estas pruebas.

## <a name="what-gets-tested"></a>Lo que se prueba

Como hemos explicado en la iteración anterior, normalmente no se escriben pruebas unitarias para la lógica de acceso a datos o la lógica de vista. No se escriben pruebas unitarias para la lógica de acceso a datos porque el acceso a una base de datos es una operación relativamente lenta. No se escriben pruebas unitarias para la lógica de vista porque el acceso a una vista requiere poner en marcha un servidor web que es una operación relativamente lenta. No debe escribir una prueba unitaria a menos que la prueba se pueda ejecutar una y otra vez muy rápido

Dado que el desarrollo controlado por pruebas está impulsado por pruebas unitarias, nos centramos inicialmente en escribir el controlador y la lógica empresarial. Evitamos tocar la base de datos o las vistas. No modificaremos la base de datos ni crearemos nuestras vistas hasta el final de este tutorial. Comenzamos con lo que se puede probar.

## <a name="creating-user-stories"></a>Creación de historias de usuario

Cuando se practica el desarrollo basado en pruebas, siempre comienza escribiendo una prueba. Esto plantea inmediatamente la pregunta: ¿Cómo decide qué prueba escribir primero? Para responder a esta pregunta, debe escribir un conjunto de [*historias*](http://en.wikipedia.org/wiki/User_stories)de usuario .

Una historia de usuario es una descripción muy breve (normalmente una frase) de un requisito de software. Debe ser una descripción no técnica de un requisito escrito desde la perspectiva del usuario.

Aquí está el conjunto de historias de usuario que describen las características requeridas por la nueva funcionalidad del grupo de contacto:

1. El usuario puede ver una lista de grupos de contactos.
2. El usuario puede crear un nuevo grupo de contactos.
3. El usuario puede eliminar un grupo de contactos existente.
4. El usuario puede seleccionar un grupo de contactos al crear un nuevo contacto.
5. El usuario puede seleccionar un grupo de contactos al editar un contacto existente.
6. Se muestra una lista de grupos de contactos en la vista de índice.
7. Cuando un usuario hace clic en un grupo de contactos, se muestra una lista de contactos coincidentes.

Tenga en cuenta que esta lista de historias de usuario es completamente comprensible para un cliente. No se mencionan los detalles técnicos de la implementación.

Mientras que en el proceso de creación de la aplicación, el conjunto de historias de usuario puede ser más refinado. Puede dividir una historia de usuario en varias historias (requisitos). Por ejemplo, puede decidir que la creación de un nuevo grupo de contactos debe implicar la validación. El envío de un grupo de contactos sin un nombre debe devolver un error de validación.

Después de crear una lista de historias de usuario, está listo para escribir la primera prueba unitaria. Comenzaremos creando una prueba unitaria para ver la lista de grupos de contactos.

## <a name="listing-contact-groups"></a>Listado de grupos de contacto

Nuestra primera historia de usuario es que un usuario debe ser capaz de ver una lista de grupos de contacto. Necesitamos expresar esta historia con una prueba.

Cree una nueva prueba unitaria haciendo clic con el botón derecho en la carpeta Controladores del proyecto ContactManager.Tests, seleccionando **Agregar, Nueva prueba**y seleccionando la plantilla **Prueba unitaria** (consulte la figura 1). Asigne un nombre a la nueva prueba unitaria GroupControllerTest.vb y haga clic en el botón **Aceptar.**

[![Agregar la prueba unitaria GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figura 01**: Adición de la prueba unitaria GroupControllerTest([Haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-vb/_static/image2.png))

Nuestra primera prueba unitaria está contenida en el listado 1. Esta prueba comprueba que el método Index() del controlador Group devuelve un conjunto de groups. La prueba comprueba que se devuelve una colección de grupos en los datos de vista.

**Listado 1 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

La primera vez que escriba el código en el listado 1 de Visual Studio, obtendrá muchas líneas onduladas rojas. No hemos creado las clases GroupController o Group.

En este punto, ni siquiera podemos construir nuestra aplicación por lo que no podemos ejecutar nuestra primera prueba unitaria. Eso es bueno. Eso cuenta como una prueba fallida. Por lo tanto, ahora tenemos permiso para empezar a escribir código de aplicación. Necesitamos escribir suficiente código para ejecutar nuestra prueba.

La clase de controlador de grupo en el listado 2 contiene el mínimo de código necesario para pasar la prueba unitaria. La acción Index() devuelve una lista de grupos codificada estáticamente (la clase Group se define en el listado 3).

**Listado 2 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Listado 3 - Models-Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Después de agregar las clases GroupController y Group a nuestro proyecto, nuestra primera prueba unitaria se completa correctamente (consulte la figura 2). Hemos hecho el trabajo mínimo necesario para pasar la prueba. Es hora de celebrar.

[![¡Éxito!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figura 02**: ¡El éxito! ( Haga clic para ver la[imagen de tamaño completo](iteration-6-use-test-driven-development-vb/_static/image4.png))

## <a name="creating-contact-groups"></a>Creación de grupos de contacto

Ahora podemos pasar a la segunda historia del usuario. Necesitamos ser capaces de crear nuevos grupos de contacto. Tenemos que expresar esta intención con una prueba.

La prueba del listado 4 comprueba que al llamar al método Create() con un nuevo grupo se agrega el grupo a la lista de grupos devueltos por el método Index(). En otras palabras, si creo un nuevo grupo, entonces debería ser capaz de recuperar el nuevo grupo de la lista de grupos devueltos por el método Index().

**Listado 4 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

La prueba en el listado 4 llama al método Create() del controlador de grupo con un nuevo grupo de contacto. A continuación, la prueba comprueba que al llamar al método Index() del controlador de grupo se devuelve el nuevo grupo en los datos de vista.

El controlador de grupo modificado en el listado 5 contiene el mínimo de cambios necesarios para pasar la nueva prueba.

**Listado 5 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>El controlador de grupo en el listado 5 tiene una nueva acción Create(). Esta acción agrega un grupo a una colección de grupos. Observe que la acción Index() se ha modificado para devolver el contenido de la colección de grupos.

Una vez más, hemos realizado la cantidad mínima de trabajo necesaria para pasar la prueba unitaria. Después de realizar estos cambios en el controlador de grupo, todas nuestras pruebas unitarias pasan.

## <a name="adding-validation"></a>Agregar una validación

Este requisito no se indicó explícitamente en la historia del usuario. Sin embargo, es razonable exigir que un Grupo tenga un nombre. De lo contrario, organizar los contactos en Grupos no sería muy útil.

El listado 6 contiene una nueva prueba que expresa esta intención. Esta prueba comprueba que al intentar crear un grupo sin proporcionar un nombre, se produce un mensaje de error de validación en el estado del modelo.

**Listado 6 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Para satisfacer esta prueba, necesitamos agregar una propiedad Name a nuestra clase Group (consulte el listado 7). Además, necesitamos agregar un poco de lógica de validación a nuestra acción Create() del controlador de grupo (consulte el listado 8).

**Listado 7 - Models-Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Listado 8 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Observe que la acción Create() del controlador de grupo ahora contiene lógica de validación y de base de datos. Actualmente, la base de datos utilizada por el controlador de grupo consta de nada más que una colección en memoria.

## <a name="time-to-refactor"></a>Tiempo para refactorizar

El tercer paso en Rojo/Verde/Refactores es la parte Refactorizar. En este punto, tenemos que retroceder de nuestro código y considerar cómo podemos refactorizar nuestra aplicación para mejorar su diseño. La etapa Refactores es la etapa en la que pensamos mucho en la mejor manera de implementar los principios y patrones de diseño de software.

Somos libres de modificar nuestro código de cualquier manera que elijamos para mejorar el diseño del código. Tenemos una red de seguridad de pruebas unitarias que nos impiden romper la funcionalidad existente.

En este momento, nuestro controlador de grupo es un desastre desde la perspectiva de un buen diseño de software. El controlador de grupo contiene un lío enredado de validación y código de acceso a datos. Para evitar violar el Principio de Responsabilidad Única, necesitamos separar estas preocupaciones en diferentes clases.

Nuestra clase de controlador de grupo refactorizado se encuentra en el listado 9. El controlador se ha modificado para utilizar la capa de servicio ContactManager. Esta es la misma capa de servicio que usamos con el controlador de contacto.

El listado 10 contiene los nuevos métodos agregados a la capa de servicio ContactManager para admitir la validación, la lista y la creación de grupos. La interfaz IContactManagerService se actualizó para incluir los nuevos métodos.

El listado 11 contiene una nueva clase FakeContactManagerRepository que implementa la interfaz IContactManagerRepository. A diferencia de la EntityContactManagerRepository clase que también implementa el IContactManagerRepository interfaz, nuestra nueva Clase FakeContactManagerRepository no se comunica con la base de datos. La clase FakeContactManagerRepository utiliza una colección en memoria como proxy para la base de datos. Usaremos esta clase en nuestras pruebas unitarias como una capa de repositorio falsa.

**Listado 9 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Listado 10 - Controllers-ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Listado 11 - Controllers-FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]

Modificar la interfaz IContactManagerRepository requiere el uso para implementar los métodos CreateGroup() y ListGroups() en la clase EntityContactManagerRepository. La forma más laz y más rápida de hacer esto es agregar métodos de código auxiliar que se ven así:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]

Por último, estos cambios en el diseño de nuestra aplicación requieren que hagamos algunas modificaciones a nuestras pruebas unitarias. Ahora necesitamos usar FakeContactManagerRepository al realizar las pruebas unitarias. La clase GroupControllerTest actualizada se encuentra en el listado 12.

**Listado 12 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Después de que hagamos todos estos cambios, una vez más, todas nuestras pruebas unitarias pasan. Hemos completado todo el ciclo de Rojo/Verde/Refactor. Hemos implementado las dos primeras historias de usuario. Ahora tenemos pruebas unitarias de apoyo para los requisitos expresados en las historias de usuario. La implementación del resto de las historias de usuario implica repetir el mismo ciclo de Rojo/Verde/Refactor.

## <a name="modifying-our-database"></a>Modificación de nuestra base de datos

Desafortunadamente, a pesar de que hemos satisfecho todos los requisitos expresados por nuestras pruebas unitarias, nuestro trabajo no está hecho. Todavía tenemos que modificar nuestra base de datos.

Necesitamos crear una nueva tabla de base de datos de grupo. Siga estos pasos:

1. En la ventana Explorador de servidores, haga clic con el botón derecho en la carpeta Tablas y seleccione la opción de menú **Agregar nueva tabla**.
2. Escriba las dos columnas que se describen a continuación en el Diseñador de tablas.
3. Marque la columna Id como clave principal y la columna Identity.
4. Guarde la nueva tabla con el nombre Grupos haciendo clic en el icono del disquete.

<a id="0.12_table01"></a>

| **Nombre de columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Identificador | int | False |
| NOMBRE | nvarchar(50) | False |

A continuación, necesitamos eliminar todos los datos de la tabla Contactos (de lo contrario, no podremos crear una relación entre las tablas Contactos y Grupos). Siga estos pasos:

1. Haga clic con el botón derecho en la tabla Contactos y seleccione la opción de menú **Mostrar datos**de tabla .
2. Elimine todas las filas.

A continuación, necesitamos definir una relación entre la tabla de base de datos Grupos y la tabla de base de datos Contactos existente. Siga estos pasos:

1. Haga doble clic en la tabla Contactos en la ventana Explorador de servidores para abrir el Diseñador de tablas.
2. Agregue una nueva columna de enteros a la tabla Contactos denominada GroupId.
3. Haga clic en el botón Relación para abrir el cuadro de diálogo Relaciones de clave externa (consulte la figura 3).
4. Haga clic en el botón Agregar.
5. Haga clic en el botón de puntos suspensivos que aparece junto al botón Especificación de tablas y columnas.
6. En el cuadro de diálogo Tablas y columnas, seleccione Grupos como tabla de claves principal e Id como columna de clave principal. Seleccione Contactos como tabla de clave externa y GroupId como columna de clave externa (consulte la figura 4). Haga clic en el botón Aceptar.
7. En **INSERT y UPDATE Specification**, seleccione el valor **Cascade** para Delete **Rule**.
8. Haga clic en el botón Cerrar para cerrar el cuadro de diálogo Relaciones de clave externa.
9. Haga clic en el botón Guardar para guardar los cambios en la tabla Contactos.

[![Creación de una relación de tabla de base de datos](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figura 03**: Creación de una relación de tabla de base de datos[(Haga clic para ver la imagen de tamaño completo)](iteration-6-use-test-driven-development-vb/_static/image6.png)

[![Especificación de relaciones de tabla](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figura 04**: Especificación de relaciones de tabla[(Haga clic para ver la imagen de tamaño completo)](iteration-6-use-test-driven-development-vb/_static/image8.png)

### <a name="updating-our-data-model"></a>Actualización de nuestro modelo de datos

A continuación, necesitamos actualizar nuestro modelo de datos para representar la nueva tabla de base de datos. Siga estos pasos:

1. Haga doble clic en el archivo ContactManagerModel.edmx de la carpeta Modelos para abrir Entity Designer.
2. Haga clic con el botón derecho en la superficie Diseñador y seleccione la opción de menú **Actualizar modelo desde base de datos**.
3. En el Asistente para actualización, seleccione la tabla Grupos y haga clic en el botón Finalizar (consulte la figura 5).
4. Haga clic con el botón derecho en la entidad Grupos y seleccione la opción de menú **Cambiar nombre**. Cambie el nombre de la entidad *Grupos* a *Grupo* (singular).
5. Haga clic con el botón derecho en la propiedad de navegación Grupos que aparece en la parte inferior de la entidad Contacto. Cambie el nombre de la propiedad de navegación *Grupos* a *Grupo* (singular).

[![Actualización de un modelo de Entity Framework desde la base de datos](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figura 05**: Actualización de un modelo de Entity Framework desde la base de datos[(Haga clic para ver la imagen de tamaño completo)](iteration-6-use-test-driven-development-vb/_static/image10.png)

Después de completar estos pasos, el modelo de datos representará las tablas Contactos y Grupos. Entity Designer debe mostrar ambas entidades (consulte la figura 6).

[![Entity Designer que muestra Grupo y Contacto](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figura 06**: Entity Designer que muestra Group y Contact(Click para ver la[imagen de tamaño completo)](iteration-6-use-test-driven-development-vb/_static/image12.png)

### <a name="creating-our-repository-classes"></a>Creación de nuestras clases de repositorio

A continuación, necesitamos implementar nuestra clase de repositorio. En el transcurso de esta iteración, agregamos varios métodos nuevos a la interfaz IContactManagerRepository mientras se escribía código para satisfacer nuestras pruebas unitarias. La versión final de la interfaz IContactManagerRepository se encuentra en el listado 14.

**Listado 14 - Modelos-IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

En realidad no hemos implementado ninguno de los métodos relacionados con el trabajo con grupos de contacto según nuestra clase EntityContactManagerRepository real. Actualmente, la clase EntityContactManagerRepository tiene métodos de código auxiliar para cada uno de los métodos de grupo de contactos enumerados en la interfaz IContactManagerRepository. Por ejemplo, el método ListGroups() tiene este aspecto actualmente:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Los métodos de código auxiliar nos permitieron compilar nuestra aplicación y pasar las pruebas unitarias. Sin embargo, ahora es el momento de implementar realmente estos métodos. La versión final de la Clase EntityContactManagerRepository se encuentra en el listado 13.

**Listado 13 - Modelos-EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Creación de las vistas

ASP.NET aplicación MVC cuando se usa el motor de vista de ASP.NET predeterminado. Por lo tanto, no se crean vistas en respuesta a una prueba unitaria determinada. Sin embargo, dado que una aplicación sería inútil sin vistas, no podemos completar esta iteración sin crear y modificar las vistas contenidas en la aplicación Contact Manager.

Necesitamos crear las siguientes nuevas vistas para administrar grupos de contactos (consulte la figura 7):

- Views-Group-Index.aspx: muestra la lista de grupos de contactos
- Views-Group-Delete.aspx: muestra el formulario de confirmación para eliminar un grupo de contactos

[![La vista de índice de grupo](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figura 07**: La vista de índice de grupo[(Haga clic para ver la imagen de tamaño completo)](iteration-6-use-test-driven-development-vb/_static/image14.png)

Necesitamos modificar las siguientes vistas existentes para que incluyan grupos de contactos:

- Views-Home-Create.aspx
- Views-Home-Edit.aspx
- Vistas-Home-Index.aspx

Puede ver las vistas modificadas examinando la aplicación de Visual Studio que acompaña a este tutorial. Por ejemplo, la Figura 8 ilustra la vista de índice de contacto.

[![La vista de índice de contacto](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figura 08**: La vista de índice de contacto[(Haga clic para ver la imagen de tamaño completo)](iteration-6-use-test-driven-development-vb/_static/image16.png)

## <a name="summary"></a>Resumen

En esta iteración, agregamos nueva funcionalidad a nuestra aplicación Contact Manager siguiendo una metodología de diseño de aplicaciones de desarrollo controlada por pruebas. Comenzamos creando un conjunto de historias de usuario. Hemos creado un conjunto de pruebas unitarias que se corresponde con los requisitos expresados por las historias de usuario. Por último, escribimos el código suficiente para satisfacer los requisitos expresados por las pruebas unitarias.

Después de que terminamos de escribir suficiente código para satisfacer los requisitos expresados por las pruebas unitarias, actualizamos nuestra base de datos y vistas. Agregamos una nueva tabla Groups a nuestra base de datos y actualizamos nuestro Entity Framework Data Model. También creamos y modificamos un conjunto de vistas.

En la siguiente iteración, la iteración final, reescribimos nuestra aplicación para aprovechar Ajax. Aprovechando Ajax, mejoraremos la capacidad de respuesta y el rendimiento de la aplicación Contact Manager.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-vb.md)
> [Siguiente](iteration-7-add-ajax-functionality-vb.md)
