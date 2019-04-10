---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Este apéndice ofrece una visión general de la programación con las páginas Web ASP.NET en Visual Basic, mediante la sintaxis de Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: e6b63afb9492e810e19999c7c7ffe074ad510bda
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59406774"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (Visual Basic)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo ofrece una visión general de la programación con ASP.NET Web Pages con la sintaxis de Razor y Visual Basic. ASP.NET es la tecnología de Microsoft para la ejecución de páginas web dinámicas en servidores web.
> 
> **Aprenderá lo**:
> 
> - El 8 de principales sugerencias de introducción a ASP.NET Web Pages con sintaxis Razor de programación de programación.
> - Conceptos básicos de programación que necesitará.
> - ¿Qué código de servidor ASP.NET y la sintaxis Razor es todo acerca de.
>   
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


La mayoría de los ejemplos del uso de ASP.NET Web Pages con sintaxis Razor usa C#. Pero la sintaxis de Razor también es compatible con Visual Basic. Para programar una página web ASP.NET en Visual Basic, crea una página web con un *.vbhtml* extensión de nombre de archivo y, a continuación, agregue el código de Visual Basic. En este artículo se proporciona información general de cómo trabajar con el lenguaje Visual Basic y la sintaxis para crear las páginas Web ASP.NET.

> [!NOTE]
> Las plantillas de sitio Web predeterminado de Microsoft WebMatrix (**pastelería**, **Galería fotográfica**, y **Starter Site**, etc.) están disponibles en versiones de C# y Visual Basic. Puede instalar las plantillas de Visual Basic, como paquetes de NuGet. Plantillas de sitios Web se instalan en la carpeta raíz del sitio en una carpeta denominada *Templates Microsoft*.


## <a name="the-top-8-programming-tips"></a>El 8 principales sugerencias de programación

Esta sección enumeran algunas sugerencias que necesita realmente saber a medida que empiece a escribir código de servidor ASP.NET mediante la sintaxis Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Agregue código a una página con el carácter @

El `@` carácter inicia las expresiones en línea, bloques de instrucción única y bloques de múltiples instrucciones:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

El resultado se muestra en un explorador:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Codificación HTML**
> 
> Al mostrar contenido en una página con el `@` de caracteres, como se muestra en los ejemplos anteriores, ASP.NET codifica en HTML el resultado. Esto reemplaza los caracteres reservados de HTML (como `<` y `>` y `&`) con los códigos que permiten los caracteres que se va a mostrar como caracteres en una página web no se interprete como etiquetas HTML o entidades. Sin codificación HTML, la salida desde el código de servidor podría no mostrarse correctamente y podría exponer una página a riesgos de seguridad.
> 
> Si su objetivo es generar marcado HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), consulte la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.
> 
> Puede leer más acerca de la codificación HTML en [trabajar con formularios HTML en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Incluir bloques de código con el código... Final de código

Un bloque de código incluye una o varias instrucciones de código y se incluye con las palabras clave `Code` y `End Code`. Coloque la apertura `Code` palabra clave inmediatamente después de la `@` carácter &#8212; no puede haber un espacio en blanco entre ellos.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

El resultado se muestra en un explorador:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Dentro de un bloque, finaliza cada instrucción de código con un salto de línea

En un bloque de código de Visual Basic, cada instrucción finaliza con un salto de línea. (Más adelante en este artículo verá un método para rodear una instrucción de código larga en varias líneas si es necesario.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Usar variables para almacenar valores

Puede almacenar valores en un *variable*, incluidas las cadenas, números y fechas, etcetera. Crear una nueva variable mediante el `Dim` palabra clave. Puede insertar directamente en una página con los valores de variable `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

El resultado se muestra en un explorador:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Incluya los valores de cadena literal entre comillas dobles

Un *cadena* es una secuencia de caracteres que se tratan como texto. Para especificar una cadena, debe encerrarlo entre comillas dobles:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Para insertar las comillas dobles dentro de un valor de cadena, inserte dos caracteres de comillas dobles. Si desea que el carácter de comillas dobles a aparecer una vez en el resultado de la página, escríbalo como `""` dentro el entrecomillado de cadena y si desea que aparezca dos veces, escríbala como `""""` dentro de la cadena entre comillas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

El resultado se muestra en un explorador:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Código de Visual Basic no distingue mayúsculas de minúsculas

El lenguaje Visual Basic no distingue mayúsculas de minúsculas. Palabras clave de programación (como `Dim`, `If`, y `True`) y los nombres de variable (como `myString`, o `subTotal`) pueden escribirse en cualquier caso.

Las siguientes líneas de código asignación un valor a la variable `lastname` con una minúscula, asigne un nombre y, a continuación, generar el valor de variable a la página mediante un nombre en mayúsculas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

El resultado se muestra en un explorador:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Gran parte de la codificación implica trabajar con objetos

Un objeto representa algo que se puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente (fila de la base de datos), etcetera. Objetos tienen propiedades que describen sus características &#8212; un objeto de cuadro de texto tiene un `Text` propiedad, un objeto de solicitud tiene un `Url` propiedad, un mensaje de correo electrónico tiene un `From` propiedad y un objeto customer tiene una `FirstName` propiedad. Los objetos también tienen métodos que son el &quot;verbos&quot; pueden realizar. Algunos ejemplos son un objeto de archivo `Save` método, un objeto de imagen `Rotate` método y un objeto de correo electrónico `Send` método.

A menudo trabajará con la `Request` campos de objeto, lo que le ofrece información como los valores de formulario en la página (cuadros de texto, etc.), el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera. En este ejemplo se muestra cómo obtener acceso a las propiedades de la `Request` objeto y cómo llamar a la `MapPath` método de la `Request` objeto, que proporciona la ruta de acceso absoluta de la página en el servidor:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

El resultado se muestra en un explorador:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Puede escribir código que toma decisiones

Una característica clave de páginas web dinámicas es que puede determinar qué hacer en función de condiciones. La manera más común de hacerlo es con la `If` instrucción (y opcionales `Else` instrucción).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

La instrucción `If IsPost` es una manera abreviada de la escritura `If IsPost = True`. Junto con `If` instrucciones, hay una gran variedad de formas para probar condiciones, repita los bloques de código, y así sucesivamente, que se describen más adelante en este artículo.

El resultado que se muestra en un explorador (después de hacer clic **enviar**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET y POST métodos y la propiedad IsPost**
> 
> El protocolo utilizado para las páginas web (HTTP) admite un número muy limitado de métodos (&quot;verbos&quot;) que se usan para realizar solicitudes al servidor. Dos las más comunes son GET, que se utiliza para leer una página, y POST, que se usa para enviar una página. En general, la primera vez que un usuario solicita una página, se solicita la página con GET. Si el usuario escribe en un formulario y, a continuación, hace clic en **enviar**, el explorador realiza una solicitud POST al servidor.
> 
> En la programación web, a menudo resulta útil saber si una página se solicitan como una operación GET o como una entrada de blog para que sepa cómo procesar la página. En ASP.NET Web Pages, puede usar el `IsPost` propiedad para ver si una solicitud es una operación GET o POST. Si la solicitud es una publicación, el `IsPost` propiedad devolverá true, y puede hacer cosas como la lectura de los valores de los cuadros de texto en un formulario. Muchos ejemplos verá mostrarle cómo procesar la página de manera diferente dependiendo del valor de `IsPost`.


## <a name="a-simple-code-example"></a>Un ejemplo de código Simple

Este procedimiento muestra cómo crear una página que muestra las técnicas de programación básicas. En el ejemplo, cree una página que permite a los usuarios escribir dos números, a continuación, agrega y muestra el resultado.

1. En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers.vbhtml*.
2. Copie el siguiente código y marcado en la página, reemplazando cualquier cosa ya está en la página.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Estos son algunos aspectos que tener en cuenta:

    - El `@` carácter inicia el primer bloque de código en la página y precede a la `totalMessage` variable incrustada en la parte inferior.
    - El bloque en la parte superior de la página se incluye en `Code...End Code`.
    - Las variables `total`, `num1`, `num2`, y `totalMessage` almacenar varios números y una cadena.
    - El valor literal de cadena asignado a la `totalMessage` es variable entre comillas dobles.
    - Dado que el código de Visual Basic no distingue mayúsculas de minúsculas, cuando el `totalMessage` variable se usa la parte inferior de la página, su nombre sólo debe coincidir con la ortografía de la declaración de variable en la parte superior de la página. No importan las mayúsculas y minúsculas.
    - La expresión `num1.AsInt()`  +  `num2.AsInt()` muestra cómo trabajar con objetos y métodos. El `AsInt` método en cada variable convierte la cadena especificada por el usuario a un número entero (entero) que se pueden agregar.
    - El `<form>` etiqueta incluye un `method="post"` atributo. Especifica que, cuando el usuario hace clic en **agregar**, la página se enviará al servidor mediante el método HTTP POST. Cuando se envía la página, el código `If IsPost` se evalúa como true y operador condicional de código se ejecuta, muestra el resultado de sumar los números.
3. Guarde la página y ejecútelo en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Escriba los dos números enteros y, a continuación, haga clic en el **agregar** botón.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Sintaxis y el lenguaje Visual Basic

Ya vimos un ejemplo básico de cómo crear una página web ASP.NET y cómo puede agregar código de servidor en formato HTML. Aquí aprenderá los conceptos básicos del uso de Visual Basic para escribir código de servidor ASP.NET mediante la sintaxis Razor &#8212; es decir, las reglas de lenguaje de programación.

Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que lee aquí le resultarán familiar. Probablemente deberá familiarizarse con sólo de cómo se agrega código de WebMatrix para marcado en *.vbhtml* archivos.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Combinación de texto, marcado y código en bloques de código

En los bloques de código de servidor, a menudo conveniente generar texto y marcado en la página. Si un bloque de código de servidor contiene texto que no es código y que en su lugar se debe representar como está, debe ser capaz de distinguir ese texto desde el código ASP.NET. Existen varias formas de hacerlo.

- Incluir el texto en un elemento de bloque HTML como `<p></p>` o `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    El elemento HTML puede incluir texto, los elementos adicionales de HTML y expresiones de código del servidor. Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), todo lo que representa el elemento y su contenido como es el explorador (y resuelve las expresiones de código del servidor).

- Use la `@:` operador o la `<text>` elemento. El `@:` da como resultado una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el `<text>` elemento abarca varias líneas de salida. Estas opciones son útiles cuando no quiere representar un elemento HTML como parte de la salida.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    El ejemplo siguiente se repite el ejemplo anterior, pero usa un único par de `<text>` etiquetas para delimitar el texto para representar.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    En el ejemplo siguiente, la `<text>` y `</text>` etiquetas incluir tres líneas, todos ellos tienen algunas no contenido texto y etiquetas HTML no coincidentes (`<br />`), junto con el código de servidor y las etiquetas HTML coincidentes. De nuevo, también se podría preceder cada línea individualmente con el `@:` operador; cualquiera de ellas funciona de forma.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Al generar texto, como se muestra en esta sección &#8212; con el elemento HTML, el `@:` (operador), o la `<text>` elemento &#8212; ASP.NET no codifica como HTML la salida. (Como se indicó anteriormente, ASP.NET codificar la salida de las expresiones de código de servidor y los bloques de código de servidor que van precedidos por `@`, excepto en los casos especiales que se indican en esta sección.)

### <a name="whitespace"></a>Whitespace

Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Dividir instrucciones largas en varias líneas

Puede dividir una instrucción de código larga en varias líneas mediante el uso del carácter de subrayado `_` (que en Visual Basic se denomina el *carácter de continuación*) después de cada línea de código. Para interrumpir una instrucción en la siguiente línea, al final de la línea, agregue un espacio y, a continuación, el carácter de continuación. Continuar con la instrucción en la línea siguiente. Puede ajustar las instrucciones en líneas tantos como necesite para mejorar la legibilidad. Las instrucciones siguientes son los mismos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Sin embargo, no se puede ajustar una línea en el medio de un literal de cadena. No funciona en el ejemplo siguiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Para combinar una cadena larga que se ajusta a varias líneas, como el código anterior, deberá usar el *operador de concatenación* (`&`), que verá más adelante en este artículo.

### <a name="code-comments"></a>Comentarios del código

Comentarios le permite dejar notas para uso personal o a otros usuarios. Van precedidos de comentarios de la sintaxis de Razor `@*` y terminar por `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Dentro de bloques de código puede usar los comentarios de la sintaxis de Razor, o puede usar normal carácter de comentario de Visual Basic, que es una comilla simple (`'`) como prefijo a cada línea.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variables

Una variable es un objeto con nombre que usa para almacenar los datos. Puede asignar variables, pero el nombre debe comenzar con un carácter alfabético y no puede contener espacios en blanco o caracteres reservados. En Visual Basic, como se vio anteriormente, no importa el caso de las letras de un nombre de variable.

### <a name="variables-and-data-types"></a>Las variables y tipos de datos

Una variable puede tener un tipo de datos específico, lo que indica qué tipo de datos se almacena en la variable. Puede tener variables de cadena que almacenan valores de cadena (como &quot;Hola mundo&quot;), las variables de entero que almacenan valores de número entero (por ejemplo, 3 o 79) y las variables de fecha que almacenan los valores de fecha en una variedad de formatos (por ejemplo, 12/4/2012 o de marzo de 2009 ). Y hay muchos otros tipos de datos que se puede usar.

Sin embargo, no debe especificar un tipo para una variable. En la mayoría de los casos, ASP.NET puede deducir el tipo según cómo se utilizan los datos en la variable. (En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).

Para declarar una variable sin especificar un tipo, use `Dim` más el nombre de variable (por ejemplo, `Dim myVar`). Para declarar una variable con un tipo, use `Dim` más el nombre de variable, seguido de `As` y, a continuación, el nombre de tipo (por ejemplo, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

El ejemplo siguiente muestra algunas expresiones en línea que utilizan las variables en una página web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

El resultado se muestra en un explorador:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertir y pruebas de tipos de datos

Aunque ASP.NET normalmente pueden determinar automáticamente un tipo de datos, a veces, no puede. Por lo tanto, es posible que deba ayudarnos ASP.NET mediante la realización de una conversión explícita. Incluso si no tiene que convertir los tipos, a veces resulta útil probar para ver qué tipo de datos podría estar trabajando con.

El caso más común es que se debe convertir una cadena a otro tipo, como en un entero o una fecha. El ejemplo siguiente muestra un caso típico que debe convertir una cadena en un número.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Como norma, proporcionados por el usuario aparecerán como cadenas. Incluso si ha le pide al usuario que escriba un número e incluso si ha escrito un dígito, cuando se envía la entrada del usuario y leerlo en el código, los datos están en formato de cadena. Por lo tanto, debe convertir la cadena en un número. En el ejemplo, si se intenta realizar operaciones aritméticas en los valores sin necesidad de convertirlos, produce el siguiente error, porque ASP.NET no se puede agregar dos cadenas:

`Cannot implicitly convert type 'string' to 'int'.`

Para convertir los valores enteros, se llama a la `AsInt` método. Si la conversión se realiza correctamente, a continuación, puede agregar los números.

En la tabla siguiente se enumera algunos métodos de conversión y prueba comunes para las variables.


:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like &quot;593&quot;) to an integer.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a>Operadores

Un operador es una palabra clave o el carácter que le indica a ASP.NET qué tipo de comando que se ejecuta en una expresión. Visual Basic admite muchos operadores, pero deberá reconocer algunas para comenzar a desarrollar páginas web de ASP.NET. En la tabla siguiente se resume los operadores más comunes.


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignment and equality. Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Inequality. Returns `True` if the values are not equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Less than, greater than, less than or equal, and greater than or equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenation, which is used to join strings.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Dot. Used to distinguish objects and their properties and methods.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentheses. Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Not. Reverses a true value to false and vice versa. Typically used as a shorthand way to test for `False` (that is, for not `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>Trabajar con archivos y rutas de acceso de carpeta en el código

A menudo a trabajar con rutas de acceso de archivos y carpetas en el código. Este es un ejemplo de la estructura de la carpeta física de un sitio Web como podría aparecer en el equipo de desarrollo:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Estos son algunos detalles esenciales sobre las direcciones URL y rutas de acceso:

- Una dirección URL comienza con cualquiera de un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).
- Una dirección URL que se corresponde con una ruta de acceso física en un equipo host. Por ejemplo, `http://myserver` pueden corresponder a la carpeta *C:\websites\mywebsite* en el servidor.
- Una ruta de acceso virtual es una abreviatura para representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa. Incluye la parte de una dirección URL que sigue al nombre de dominio o servidor. Al usar rutas de acceso virtuales, puede mover el código a un dominio diferente o un servidor sin tener que actualizar las rutas de acceso.

Este es un ejemplo que le ayudarán a comprender las diferencias:

| Dirección URL completa | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nombre del servidor | *mycompanyserver* |
| Ruta de acceso virtual | */humanresources/CompanyPolicy.htm* |
| Ruta de acceso física | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Es la raíz virtual /, al igual que la raíz de la unidad C: es la unidad \. (Las rutas de acceso de la carpeta virtual siempre usar barras diagonales). La ruta de acceso virtual de una carpeta no debe tener el mismo nombre que la carpeta física; puede ser un alias. (En los servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta.)

A veces, cuando se trabaja con archivos y carpetas en el código, debe hacer referencia a la ruta de acceso física y a veces, una ruta de acceso virtual, dependiendo de los objetos que se trabaja con. ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el `Server.MapPath` método y el `~` operador y `Href` método.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Convertir rutas de acceso virtuales a físico: el método Server.MapPath

El `Server.MapPath` método convierte una ruta de acceso virtual (como */default.cshtml*) a una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilice este método cada vez que necesite una ruta de acceso física completa. Un ejemplo típico es cuando se va a leer o escribir un archivo de texto o imagen en el servidor web.

Normalmente no conoce la ruta de acceso física absoluta de su sitio en el servidor de hospedaje de un sitio, por lo que este método puede convertir la ruta de acceso es saber, la ruta de acceso virtual, la ruta de acceso correspondiente en el servidor para usted. Pase la ruta de acceso virtual a un archivo o carpeta al método y devuelve la ruta de acceso física:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Hacer referencia a la raíz virtual: el ~ operador y Href (método)

En un *.cshtml* o *.vbhtml* archivo, puede hacer referencia a la ruta de acceso virtual raíz mediante el `~` operador. Esto es muy útil porque puede mover las páginas en un sitio y los vínculos que contienen a otras páginas no se interrumpe. También es útil en caso de alguna vez mover su sitio Web a una ubicación diferente. A continuación se muestran algunos ejemplos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Si el sitio Web está `http://myserver/myapp`, le mostramos cómo ASP.NET tratan estas rutas de acceso cuando se ejecuta la página:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(En realidad, no verá estas rutas de acceso que los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso es lo que fueran).

Puede usar el `~` operador tanto en código del servidor (como anteriormente) en el marcado, similar al siguiente:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

En el marcado, usa el `~` operador para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS. Cuando se ejecuta la página, ASP.NET busca en la página (marcado y código) y resuelve todos los `~` referencias a la ruta de acceso adecuada.

## <a name="conditional-logic-and-loops"></a>Bucles y la lógica condicional

Código de servidor ASP.NET le permite realizar tareas basadas en condiciones y escribir código que se repite las instrucciones de código, es decir, un número específico de veces y que se ejecuta un bucle).

### <a name="testing-conditions"></a>Condiciones de pruebas

Para probar una condición sencilla que use el `If...Then` instrucción, que devuelve `True` o `False` basándose en una prueba que especifique:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

El `If` palabra clave inicia un bloque. La prueba real (condición) sigue el `If` palabra clave y devuelve true o false. El `If` instrucción termina con `Then`. Las instrucciones que se ejecutarán si la prueba es verdadera se incluyan entre `If` y `End If`. Un `If` instrucción puede incluir un `Else` bloque que especifica las instrucciones que se ejecutarán si la condición es falsa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Si un `If` instrucción inicia un bloque de código, no tiene que usar el valor normal `Code...End Code` instrucciones para incluir los bloques. Puede agregar `@` al bloque, y funcionará. Este enfoque funciona con `If` , así como otra palabras clave que se van seguidas de bloques de código, incluidos de programación de Visual Basic `For`, `For Each`, `Do While`, etcetera.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Puede agregar varias condiciones de uso de uno o varios `ElseIf` bloques:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

En este ejemplo, si la primera condición en la `If` bloque no es true, el `ElseIf` se comprueba la condición. Si se cumple esa condición, las instrucciones en el `ElseIf` bloque se ejecutan. Si se cumple ninguna de las condiciones, las instrucciones en el `Else` bloque se ejecutan. Puede agregar cualquier número de `ElseIf` bloquea y, a continuación, cierre con un `Else` bloquear como el &quot;todo lo demás&quot; condición.

Para probar un gran número de condiciones, use un `Select Case` bloque:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

El valor para probar es entre paréntesis (en el ejemplo, la variable de día de la semana). Cada prueba individual usa un `Case` instrucción que se muestra un valor. Si el valor de un `Case` instrucción coincide con el valor de prueba, el código que `Case` bloque se ejecuta.

El resultado de los dos últimos bloques condicionales mostrada en un explorador:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Código de bucles

A menudo necesitará ejecutar repetidamente las mismas instrucciones. La forma de hacerlo es mediante un bucle. Por ejemplo, se ejecuta con frecuencia las mismas instrucciones para cada elemento en una colección de datos. Si sabe exactamente cuántas veces desea crear un bucle, puede usar un `For` bucle. Este tipo de bucle es especialmente útil para contar o cuenta atrás:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

El bucle comienza con la `For` palabra clave, seguida de tres elementos:

- Inmediatamente después de la `For` la instrucción, se declara una variable de contador (no tiene que usar `Dim`) y, a continuación, indicar el intervalo, como en `i = 10 to 20`. Esto significa que la variable `i` se empezará a contar en 10 y continúa hasta que llega a 20 (ambos inclusive).
- Entre los `For` y `Next` instrucciones es el contenido del bloque. Esto puede contener una o varias instrucciones de código que se ejecutan con cada bucle.
- El `Next i` instrucción finaliza el bucle. Incrementa el contador y se inicia la siguiente iteración del bucle.

La línea de código entre la `For` y `Next` líneas contiene el código que se ejecuta para cada iteración del bucle. El marcado crea un nuevo párrafo (`<p>` elemento) cada vez y se agrega una línea a la salida, mostrar el valor de i (el contador). Al ejecutar esta página, el ejemplo crea 11 líneas que muestra la salida, con el texto de cada línea que indica el número de elemento.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Si está trabajando con una colección o matriz, a menudo se usa un `For Each` bucle. Una colección es un grupo de objetos similares y el `For Each` bucle permite llevar a cabo una tarea en cada elemento de la colección. Este tipo de bucle es conveniente para las colecciones, ya que a diferencia de un `For` bucle, no tendrá que incrementar el contador o establecer un límite. En su lugar, el `For Each` código del bucle for simplemente continúa a través de la colección hasta que haya terminado.

En este ejemplo devuelve los elementos en el `Request.ServerVariables` colección (que contiene información sobre el servidor web). Usa un `For Each` bucle para mostrar el nombre de cada elemento mediante la creación de un nuevo `<li>` elemento en una lista con viñetas en HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

El `For Each` palabra clave va seguida de una variable que representa un elemento único en la colección (en el ejemplo, `myItem`), seguido por el `In` palabra clave, seguida de la colección que desea para recorrer en iteración. En el cuerpo de la `For Each` bucle, puede obtener acceso al elemento actual utilizando la variable que declaró anteriormente.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Para crear un bucle de uso más general, utilice el `Do While` instrucción:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Este bucle comienza con la `Do While` palabra clave, seguida de una condición, seguido del bloque que se repita. Bucles normalmente incrementar (agregar a) o el decremento (restar) una variable o un objeto que se usa para el recuento. En el ejemplo, el `+=` operador de suma 1 al valor de una variable cada vez que se ejecuta el bucle. (Para disminuyen una variable en un bucle que cuenta hacia atrás, usaría el operador de decremento `-=`.)

## <a name="objects-and-collections"></a>Objetos y colecciones

Casi todo el contenido de un sitio Web ASP.NET es un objeto, incluida la propia página web. En esta sección se describe algunos objetos importantes que se va a trabajar con frecuencia en el código.

### <a name="page-objects"></a>Objetos de página

El objeto más básico de ASP.NET es la página. Puede tener acceso a las propiedades del objeto page directamente sin ningún objeto de calificación. El código siguiente obtiene la ruta de acceso de archivo de la página, utilizando el `Request` objeto de la página:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Puede usar las propiedades de la `Page` objeto para obtener una gran cantidad de información, como:

- `Request`. Como ya hemos visto, esto es una colección de información acerca de la solicitud actual, incluidos el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera.
- `Response`. Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando haya terminado de ejecutarse el código del servidor. Por ejemplo, puede utilizar esta propiedad para escribir información en la respuesta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de colección (matrices y diccionarios)

Una colección es un grupo de objetos del mismo tipo, como una colección de `Customer` objetos desde una base de datos. ASP.NET contiene muchas colecciones integradas, como el `Request.Files` colección.

A menudo a trabajar con datos en colecciones. Dos tipos de colección comunes son el *matriz* y *diccionario*. Una matriz es útil cuando desea almacenar una colección de elementos similares, pero no desea crear una variable independiente para contener cada elemento:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Con las matrices, declarar un tipo de datos específico, como `String`, `Integer`, o `DateTime`. Para indicar que la variable puede contener una matriz, agregue un paréntesis al nombre de variable en la declaración (como `Dim myVar() As String`). Puede obtener acceso a los elementos en una matriz mediante su posición (index) o mediante el `For Each` instrucción. Los índices de matriz son de base cero &#8212; es decir, el primer elemento está en posición 0, el segundo elemento es en la posición 1 y así sucesivamente.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Puede determinar el número de elementos en una matriz mediante la obtención de su `Length` propiedad. Para obtener la posición de un elemento específico de la matriz (es decir, para buscar en la matriz), use el `Array.IndexOf` método. También puede hacer cosas como inversa el contenido de una matriz (el `Array.Reverse` método) u ordenar el contenido (el `Array.Sort` método).

La salida del código de matriz de cadena mostrado en un explorador:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un diccionario es una colección de pares clave/valor, donde proporcionar la clave (o nombre) para establecer o recuperar el valor correspondiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Para crear un diccionario, utilice el `New` palabra clave para indicar que está creando un nuevo `Dictionary` objeto. Un diccionario puede asignar a una variable mediante el `Dim` palabra clave. Indicar los tipos de datos de los elementos del diccionario utilizando paréntesis ( `( )` ). Al final de la declaración, debe agregar otro par de paréntesis, ya que esto es realmente un método que crea un nuevo diccionario.

Para agregar elementos al diccionario, puede llamar a la `Add` método de la variable del diccionario (`myScores` en este caso) y, a continuación, especifique una clave y un valor. Como alternativa, puede usar paréntesis para indicar la clave y realizar una asignación sencilla, como se muestra en el ejemplo siguiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Para obtener un valor del diccionario, especifique la clave entre paréntesis:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Llamar a métodos con parámetros

Como vimos anteriormente en este artículo, los objetos que programación con tienen métodos. Por ejemplo, un `Database` objeto podría tener un `Database.Connect` método. Muchos métodos también tienen uno o más parámetros. Un *parámetro* es un valor que se pasa a un método para habilitar el método completar su tarea. Por ejemplo, examine una declaración para el `Request.MapPath` método, que toma tres parámetros:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada. Los tres parámetros del método son `virtualPath`, `baseVirtualDir`, y `allowCrossAppMapping`. (Tenga en cuenta que en la declaración, se enumeran los parámetros con los tipos de datos de los datos que aceptan). Cuando se llama a este método, debe proporcionar valores para los tres parámetros.

Cuando se está utilizando Visual Basic con la sintaxis de Razor, tiene dos opciones para pasar parámetros a un método: *parámetros posicionales* o *parámetros con nombre*. Para llamar a un método con parámetros posicionales, pasar los parámetros en un orden estricto que se especifica en la declaración del método. (Normalmente, sabría este orden, lea la documentación del método.) Debe seguir el orden y no se puede omitir cualquiera de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o null para un parámetro de posición que no tienen un valor para.

En el siguiente ejemplo se supone que tiene una carpeta denominada *scripts* en su sitio Web. El código llama a la `Request.MapPath` y pasa valores para los tres parámetros en el orden correcto. A continuación, muestra la ruta de acceso asignado resultante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Cuando hay demasiados parámetros para un método, puede mantener el código más limpio y sea más legible mediante el uso de parámetros con nombre. Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido `:=` y, a continuación, proporcione el valor. Una ventaja de parámetros con nombre es que puede agregarlos en cualquier orden que desee. (Una desventaja es que la llamada al método no es lo más compacto).

El ejemplo siguiente llama al mismo método que el anterior, pero usa parámetros para proporcionar los valores con nombre:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Como puede ver, los parámetros se pasan en un orden diferente. Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverá el mismo valor.

## <a name="handling-errors"></a>Control de errores

### <a name="try-catch-statements"></a>Instrucciones Try-Catch

A menudo tendrá las instrucciones en el código que podría producir un error por razones de fuera de su control. Por ejemplo:

- Si el código intenta abrir, crear, leer o escribir un archivo, puede producirse todo tipo de errores. No es posible que existe el archivo que desea, podría estar bloqueado, el código podría no tener los permisos y así sucesivamente.
- De forma similar, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, es posible que se pierda la conexión a la base de datos, los datos que se va a guardar podrían ser no válido y así sucesivamente.

En términos de programación, estas situaciones se conocen como *excepciones*. Si el código encuentra una excepción, genera (produce) un mensaje de error: es decir, en el mejor, molesta a los usuarios.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

En situaciones donde el código puede encontrar excepciones y con el fin de evitar los mensajes de error de este tipo, puede usar `Try/Catch` instrucciones. En el `Try` (instrucción), ejecute el código que está protegiendo. En uno o varios `Catch` instrucciones, puede buscar de determinados errores (tipos específicos de excepciones) que pudieran haberse producido. Puede incluir tantos `Catch` las instrucciones que necesitan buscar errores que están anticipación.

> [!NOTE]
> Se recomienda evitar el uso de la `Response.Redirect` método `Try/Catch` instrucciones, ya que puede provocar una excepción en la página.


El ejemplo siguiente muestra una página que se crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo. En el ejemplo se usa deliberadamente un nombre de archivo incorrecto por lo que provocará una excepción. El código incluye `Catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto, y `DirectoryNotFoundException`, lo que sucederá si ASP.NET aún no se encuentra la carpeta. (Se puede quitar el comentario una instrucción en el ejemplo con el fin de ver cómo se ejecuta cuando todo funciona correctamente.)

Si el código no controla la excepción, verá una página de error similar a la captura de pantalla anterior. Sin embargo, la `Try/Catch` sección le ayuda a impedir que el usuario se vean estos tipos de errores.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Recursos adicionales

### <a name="reference-documentation"></a>Documentación de referencia

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Lenguaje Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
