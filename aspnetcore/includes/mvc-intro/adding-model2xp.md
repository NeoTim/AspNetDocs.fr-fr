---
ms.openlocfilehash: 0084d8c6bc5d16eaa1c3fa36d8aabb2aa31e1283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029466"
---
<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="bad71-101">Effectuer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="bad71-101">Perform initial migration</span></span>

<span data-ttu-id="bad71-102">À partir de la ligne de commande, exécutez les commandes CLI .NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="bad71-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="bad71-103">La commande `dotnet ef migrations add InitialCreate` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="bad71-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="bad71-104">Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="bad71-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="bad71-105">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="bad71-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="bad71-106">Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="bad71-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="bad71-107">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="bad71-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="bad71-108">La commande `dotnet ef database update` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage> _InitialCreate.cs*, ce qui crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="bad71-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
