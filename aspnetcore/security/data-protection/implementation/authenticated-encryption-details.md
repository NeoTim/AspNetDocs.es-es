---
title: Detalles de cifrado autenticado en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación del cifrado de la protección de datos de ASP.NET Core autenticado.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: ac650e5c32e7eacc4088225e63f56340f95e1913
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047722"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>Detalles de cifrado autenticado en ASP.NET Core

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Las llamadas a IDataProtector.Protect son las operaciones de cifrado autenticado. El método Protect ofrece confidencialidad y la autenticidad y está vinculado a la cadena de propósito que se usó para derivar esta instancia concreta de IDataProtector su raíz IDataProtectionProvider.

IDataProtector.Protect toma un parámetro de texto simple de byte [] y genera una byte [] protegido carga, cuyo formato se describe a continuación. (También hay una sobrecarga del método de extensión que toma un parámetro de cadena de texto simple y devuelve una carga protegida de la cadena. Si se usa esta API todavía tendrá el formato de carga protegido el por debajo de la estructura, pero será [codificados en base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Formato de carga protegido

El formato de carga protegido consta de tres componentes principales:

* Encabezado mágico de 32 bits que identifica la versión del sistema de protección de datos.

* Identificador de clave de 128 bits que identifica la clave utilizada para proteger esta carga determinada.

* El resto de la carga protegido es [específico para el sistema de cifrado encapsulada por esta clave](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). En el ejemplo siguiente representa la clave de un AES-256-CBC + cifrador HMACSHA256, y la carga se subdivide además como sigue: * modificador de clave de 128 bits. * Un vector de inicialización de 128 bits. * los 48 bytes de salida de AES-256-CBC. * Una etiqueta de autenticación HMACSHA256.

Una carga protegida de ejemplo se ilustra a continuación.

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

Desde el formato de carga por encima de los primeros 32 bits o 4 bytes son el encabezado mágico que identifica la versión (09 F0 C9 F0)

Los siguientes 128 bits, o 16 bytes es el identificador de clave (80 9 81 C 0c 19 66 19 40 95 36 53 F8 AA FF EE 57)

El resto contiene la carga y es específico para el formato utilizado.

>[!WARNING]
> Todas las cargas protegidas en una clave determinada se iniciará con el mismo encabezado de 20 bytes (valor mágico, Id. de clave). Los administradores pueden usar este hecho para fines de diagnóstico para aproximarse a cuando se generó una carga. Por ejemplo, la carga anterior corresponde a la clave {0c819c80-6619-4019-9536-53f8aaffee57}. Si después de comprobar el repositorio clave encuentra que la fecha de activación de esta clave específica fue 2015-01-01 y su fecha de expiración era 2015-03-01, entonces es razonable suponer la carga (si no modificado) se ha generado dentro de esa ventana, conceda a o tomar una pequeña factor aglutinante a ambos lados.
