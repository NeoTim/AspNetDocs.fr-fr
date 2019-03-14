---
title: Ajouter un nouveau champ à une page Razor dans ASP.NET Core
author: rick-anderson
description: Montre comment ajouter un nouveau champ à une page Razor avec Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 98de39c63c992dce7d60563df316d848339b811a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050416"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Ajouter un nouveau champ à une page Razor dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

Dans cette section, Migrations [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First est utilisé pour :

* Ajouter un nouveau champ au modèle.
* Migrer la nouvelle modification du schéma des champs vers la base de données.

Quand vous utilisez EF Code First pour créer automatiquement une base de données, Code First :

* Ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.
* Si les classes de modèle ne sont pas synchronisées avec la base de données, EF lève une exception.

La vérification automatique de la synchronisation du schéma et du modèle facilite la détection des problèmes d’incohérence et de code de base de données.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Générez l'application.

Modifiez *Pages/Movies/Index.cshtml*et ajoutez un champ `Rating` :

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

Mettez à jour les pages suivantes :

* Ajoutez le champ `Rating` aux pages Delete et Details.
* Mettez à jour [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) avec un champ `Rating`.
* Ajoutez le champ `Rating` à la Page Edit.

L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ. Si vous l’exécutez à présent, l’application lève une `SqlException` :

`SqlException: Invalid column name 'Rating'.`

Cette erreur est due au fait que la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données. (Il n’existe pas de colonne `Rating` dans la table de base de données.)

Plusieurs approches sont possibles pour résoudre l’erreur :

1. Laisser Entity Framework supprimer et recréer automatiquement la base de données avec le nouveau schéma de classes du modèle. Cette approche est très utile au début du cycle de développement. Elle permet de faire évoluer rapidement le modèle et le schéma de base de données ensemble. L’inconvénient est que vous perdez les données existantes dans la base de données. N’utilisez pas cette approche sur une base de données de production ! La suppression de la base de données lors des changements de schéma et l’utilisation d’un initialiseur pour amorcer automatiquement la base de données avec des données de test est souvent un moyen efficace pour développer une application.

2. Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.

3. Utilisez les migrations Code First pour mettre à jour le schéma de base de données.

Pour ce didacticiel, nous allons utiliser les migrations Code First.

Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne. Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque bloc `new Movie`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).

Générez la solution.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>Ajouter une migration pour le champ d’évaluation

Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du Gestionnaire de package**.
Dans la console du Gestionnaire de package, entrez les commandes suivantes :

```powershell
Add-Migration Rating
Update-Database
```

La commande `Add-Migration` indique au framework qu’il doit :

* Comparer le modèle `Movie` au schéma de base de données `Movie`
* Créer du code pour migrer le schéma de base de données vers le nouveau modèle

Le nom « Rating » est arbitraire et utilisé pour nommer le fichier de migration. Il est utile d’utiliser un nom explicite pour le fichier de migration.

La commande `Update-Database` demande à l’infrastructure d’appliquer les modifications de schéma à la base de données.

<a name="ssox"></a>

Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`. Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox).

Une autre option consiste à supprimer la base de données et à utiliser des migrations pour recréer la base de données. Pour supprimer la base de données dans SSOX :

* Sélectionnez la base de données dans SSOX.
* Cliquez avec le bouton droit sur la base de données, puis sélectionnez *Supprimer*.
* Cochez **Fermer les connexions existantes**.
* Sélectionnez **OK**.
* Dans la [console du gestionnaire de package](xref:tutorials/razor-pages/new-field#pmc), mettez à jour la base de données :

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

Exécutez les commandes CLI .NET Core suivantes :

```console
dotnet ef migrations add Rating
dotnet ef database update
```

La commande `ef migrations add` indique au framework qu’il doit :

* Comparer le modèle `Movie` au schéma de base de données `Movie`
* Créer du code pour migrer le schéma de base de données vers le nouveau modèle

Le nom « Rating » est arbitraire et utilisé pour nommer le fichier de migration. Il est utile d’utiliser un nom explicite pour le fichier de migration.

La commande `ef database update` demande à l’infrastructure d’appliquer les modifications de schéma à la base de données.

Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`. Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de l’outil SQLite.

Une autre option consiste à supprimer la base de données et à utiliser des migrations pour recréer la base de données. Pour supprimer la base de données, supprimez le fichier de base de données (*MvcMovie.db*). Exécutez ensuite la commande `ef database update` : 

```console
dotnet ef database update
```

> [!NOTE]
> Plusieurs opérations de modification de schéma ne sont pas prises en charge par le fournisseur EF Core SQLite. Par exemple, l’ajout d’une colonne est pris en charge, mais pas sa suppression. Si vous ajoutez une migration pour supprimer une colonne, la commande `ef migrations add` réussit mais la commande `ef database update` échoue. Vous pouvez contourner certaines limitations en écrivant manuellement du code de migration pour effectuer une reconstruction de la table. Une reconstruction de la table implique la modification du nom de la table existante, la création d’une nouvelle table, la copie des données vers la nouvelle table et la suppression de l’ancienne table. Pour plus d'informations, reportez-vous aux ressources suivantes :
> * [Limites d’un fournisseur de base de données EF Core SQLite](/ef/core/providers/sqlite/limitations)
> * [Personnaliser le code de migration](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Amorçage des données](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`. Si la base de données n’est pas amorcée, définissez un point d’arrêt dans la méthode `SeedData.Initialize`.

> [!div class="step-by-step"]
> [Précédent : Ajout d’une fonction de recherche](xref:tutorials/razor-pages/search)
> [Suivant : Ajout de la validation](xref:tutorials/razor-pages/validation)
