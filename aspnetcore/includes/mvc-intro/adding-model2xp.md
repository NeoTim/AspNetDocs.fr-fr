---
ms.openlocfilehash: 0084d8c6bc5d16eaa1c3fa36d8aabb2aa31e1283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029466"
---
<a name="cli"></a>
## <a name="perform-initial-migration"></a>Effectuer la migration initiale

À partir de la ligne de commande, exécutez les commandes CLI .NET Core suivantes :

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

La commande `dotnet ef migrations add InitialCreate` génère le code nécessaire à la création du schéma de base de données initial. Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MvcMovieContext.cs*). L’argument `Initial` est utilisé pour nommer les migrations. Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration. Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).

La commande `dotnet ef database update` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage> _InitialCreate.cs*, ce qui crée la base de données.
