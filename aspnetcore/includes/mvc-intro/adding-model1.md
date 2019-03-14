---
ms.openlocfilehash: 72e33ea44976963193d2560427fc418875be450e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038872"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="19b22-101">Agregar un modelo a una aplicación de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="19b22-101">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="19b22-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="19b22-102">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="19b22-103">En esta sección, agregará algunas clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="19b22-103">In this section, you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="19b22-104">Estas clases serán el elemento "**M**odel" de la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="19b22-104">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="19b22-105">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="19b22-105">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="19b22-106">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos que se debe escribir.</span><span class="sxs-lookup"><span data-stu-id="19b22-106">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span> <span data-ttu-id="19b22-107">[EF Core es compatible con muchos motores de base de datos](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="19b22-107">[EF Core supports many database engines](/ef/core/providers/).</span></span>

<span data-ttu-id="19b22-108">Las clases de modelo que se crean se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="19b22-108">The model classes you'll create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="19b22-109">Simplemente definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19b22-109">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="19b22-110">En este tutorial, se escriben primero las clases de modelo y EF Core creará la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19b22-110">In this tutorial you'll write the model classes first, and EF Core will create the database.</span></span> <span data-ttu-id="19b22-111">Hay un enfoque alternativo, que no se trata aquí, que consiste en generar clases de modelo a partir de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="19b22-111">An alternate approach not covered here is to generate model classes from an already-existing database.</span></span> <span data-ttu-id="19b22-112">Para más información sobre este enfoque, vea [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (ASP.NET Core - base de datos existente).</span><span class="sxs-lookup"><span data-stu-id="19b22-112">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="19b22-113">Agregar una clase de modelo de datos</span><span class="sxs-lookup"><span data-stu-id="19b22-113">Add a data model class</span></span>
