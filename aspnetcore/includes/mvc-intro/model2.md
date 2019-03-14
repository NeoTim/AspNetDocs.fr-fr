---
ms.openlocfilehash: f2c9cad82eb25d350426465b3632862761740abe
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040496"
---
<a name="dc"></a>

Ajoutez la classe `MvcMovieContext` suivante au dossier *Models* :  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]


Le code précédent crée une propriété `DbSet` pour le jeu d’entités. Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Ajouter une chaîne de connexion de base de données

Ajoutez une chaîne de connexion au fichier *appsettings.json* :

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a>Ajouter les packages NuGet nécessaires

Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Inscrire le contexte de base de données

En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Générez le projet en tant que vérification des erreurs.