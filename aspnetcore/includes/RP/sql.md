---
ms.openlocfilehash: d963e3b52db7703e9b2fad1466a23fdeb1b8f848
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035356"
---
# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a>Utilisation de SQLite dans une application Razor Pages ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

L’objet `MovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données. Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` du fichier *Startup.cs* :

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

Pour plus d’informations sur l’utilisation de `DbContext` avec l’injection de dépendances, consultez [Utilisation de DbContext avec l’injection de dépendances](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).

## <a name="sqlite"></a>SQLite

Le site web [SQLite](https://www.sqlite.org/) indique :

> SQLite est un moteur de base de données SQL autonome, embarqué, à haute fiabilité, aux fonctionnalités complètes et accessible dans le domaine public. SQLite est le moteur de base de données le plus utilisé au monde.

Vous pouvez télécharger de nombreux outils tiers pour gérer et afficher une base de données SQLite. L’image ci-dessous provient de [DB Browser for SQLite](http://sqlitebrowser.org/). Si vous avez un outil SQLite préféré, laissez un commentaire pour nous dire ce que vous aimez chez lui.

![DB Browser for SQLite montrant la base de données de films](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Amorcer la base de données

Créez une classe nommée `SeedData` dans l’espace de noms *Modèles*. Remplacez le code généré par ce qui suit :

[!code-csharp[](code/Models/SeedData.cs)]

Si la base de données contient des films, l’initialiseur de valeur initiale retourne.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Ajouter l’initialiseur de valeur initiale

Ajoutez l’initialiseur de valeur initiale à la méthode `Main` dans le fichier *Program.cs* :

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a>Tester l’application

Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute). Arrêtez et démarrez l’application pour amorcer la base de données.

L’application affiche les données de départ.
