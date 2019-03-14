---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175900"
---
```console
npm run release
```

Este comando da como resultado la entrega de los activos del lado cliente cuando se ejecuta la aplicación. Los recursos se colocan en la carpeta *wwwroot*.

Webpack ha completado las tareas siguientes:

* Purgar el contenido del directorio *wwwroot*.
* Convertir TypeScript en JavaScript, proceso conocido como *transpilación*.
* Alterar el código JavaScript generado para reducir el tamaño del archivo, proceso conocido como *minificación*.
* Copiar los archivos JavaScript, CSS y HTML procesados desde *src* en el directorio *wwwroot*.
* Insertar los elementos siguientes en el archivo *wwwroot/index.html*:
    * Etiqueta `<link>`, que hace referencia al archivo *wwwroot/main.\<hash\>.css*. Esta etiqueta se coloca inmediatamente antes de la etiqueta `</head>` de cierre.
    * Etiqueta `<script>`, que hace referencia al archivo *wwwroot/main.\<hash\>.js* minificado. Esta etiqueta se coloca inmediatamente antes de la etiqueta `</body>` de cierre.
