---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Création de cours modèles avec LINQ à SQL (C) Microsoft Docs
author: rick-anderson
description: Le but de ce tutoriel est d’expliquer une méthode de création de classes de modèles pour une application ASP.NET MVC. Dans ce tutoriel, vous apprenez à construire le modèle c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: eac6fec3a4cfd833b810378d946874a246d9a2c3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542727"
---
# <a name="creating-model-classes-with-linq-to-sql-c"></a>Création de classes de modèle avec LINQ to SQL (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Le but de ce tutoriel est d’expliquer une méthode de création de classes de modèles pour une application ASP.NET MVC. Dans ce tutoriel, vous apprenez à construire des classes de modèles et d’effectuer l’accès aux bases de données en profitant de Microsoft LINQ à SQL.

Le but de ce tutoriel est d’expliquer une méthode de création de classes de modèles pour une application ASP.NET MVC. Dans ce tutoriel, vous apprenez à construire des classes de modèles et d’effectuer l’accès à la base de données en profitant de Microsoft LINQ à SQL

Dans ce tutoriel, nous construisons une application de base de base de base de données Movie. Nous commençons par créer l’application de base de données Movie de la manière la plus rapide et la plus simple possible. Nous effectuons tous nos accès aux données directement à partir de nos actions de contrôleur.

Ensuite, vous apprenez à utiliser le modèle De dépôt. L’utilisation du modèle Repository nécessite un peu plus de travail. Cependant, l’avantage de l’adoption de ce modèle est qu’il vous permet de construire des applications qui sont adaptables au changement et peuvent être facilement testés.

## <a name="what-is-a-model-class"></a>Qu’est-ce qu’une classe modèle?

Un modèle MVC contient toute la logique d’application qui n’est pas contenue dans une vue MVC ou un contrôleur MVC. En particulier, un modèle MVC contient toute votre activité d’application et la logique d’accès aux données.

Vous pouvez utiliser une variété de technologies différentes pour implémenter votre logique d’accès aux données. Par exemple, vous pouvez créer vos classes d’accès aux données à l’aide des classes Microsoft Entity Framework, NHibernate, Subsonic ou ADO.NET.

Dans ce tutoriel, j’utilise LINQ à SQL pour interroger et mettre à jour la base de données. LINQ to SQL vous offre une méthode très facile d’interagir avec une base de données Microsoft SQL Server. Cependant, il est important de comprendre que le cadre ASP.NET MVC n’est pas lié à LINQ à SQL de quelque façon que ce soit. ASP.NET MVC est compatible avec toute technologie d’accès aux données.

## <a name="create-a-movie-database"></a>Créer une base de données de films

Dans ce tutoriel - afin d’illustrer comment vous pouvez construire des classes de modèles - nous construisons une application de base de données Movie simple. La première étape consiste à créer une nouvelle base de données. Cliquez à droite\_sur le dossier App Data dans la fenêtre Solution Explorer et sélectionnez l’option menu **Ajouter, Nouvel article**. Sélectionnez le modèle **de base de données de serveurs SQL,** donnez-lui le nom MoviesDB.mdf et cliquez sur le bouton **Ajouter** (voir la figure 1).

[![Ajout d’une nouvelle base de données SQL Server](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Figure 01**: Ajout d’une nouvelle base de données SQL Server[(Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))

Après avoir créé la nouvelle base de données, vous pouvez ouvrir la base\_de données en cliquant deux fois sur le fichier MoviesDB.mdf dans le dossier App Data. Le double clic du fichier MoviesDB.mdf ouvre la fenêtre Server Explorer (voir la figure 2).

La fenêtre Server Explorer s’appelle la fenêtre Database Explorer lors de l’utilisation de Visual Web Developer.

[![Utilisation de la fenêtre Server Explorer](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Figure 02**: Utilisation de la fenêtre Server Explorer ([Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))

Nous devons ajouter une table à notre base de données qui représente nos films. Cliquez à droite sur le dossier Tables et sélectionnez l’option menu **Ajouter une nouvelle table**. La sélection de cette option de menu ouvre le tableau (voir figure 3).

[![Utilisation de la fenêtre Server Explorer](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Figure 03**: The Table Designer ([Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))

Nous devons ajouter les colonnes suivantes à notre tableau de base de données :

| **Nom de colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | Int | False |
| Intitulé | Nvarchar(200) | False |
| Directeur | Nvarchar(50) | False |

Vous devez faire deux choses spéciales à la colonne Id. Tout d’abord, vous devez marquer la colonne Id comme une colonne principale en sélectionnant la colonne dans le concepteur de table et en cliquant sur l’icône d’une clé. LINQ à SQL vous oblige à spécifier vos colonnes clés principales lors de l’exécution des encarts ou des mises à jour par rapport à la base de données.

Ensuite, vous devez marquer la colonne Id comme une colonne d’identité en attribuant la valeur Oui à la propriété **Is Identity** (voir la figure 3). Une colonne d’identité est une colonne qui se voit attribuer automatiquement un nouveau numéro chaque fois que vous ajoutez une nouvelle ligne de données à une table.

## <a name="create-linq-to-sql-classes"></a>Créer LINQ à SQL Classes

Notre modèle MVC contiendra des classes LINQ à SQL qui représentent la table de base de données tblMovie. La façon la plus simple de créer ces classes LINQ à SQL est de cliquer à droite sur le dossier Modèles, de sélectionner **Ajouter, Nouvel Article,** sélectionner le modèle LINQ à SQL Classes, donner aux classes le nom Movie.dbml, et cliquez sur le bouton **Ajouter** (voir la figure 4).

[![Création de cours LINQ à SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Figure 04**: Création de cours LINQ à SQL ([Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))

Immédiatement après avoir créé le film LINQ aux classes SQL, le concepteur relationnel d’objet apparaît. Vous pouvez faire glisser les tables de base de données de la fenêtre Server Explorer sur le concepteur relationnel d’objet pour créer LINQ à SQL Classes qui représentent des tables de base de données particulières. Nous devons ajouter la table de base de données tblMovie sur le concepteur relationnel d’objet (voir la figure 5).

[![Utilisation du concepteur relationnel d’objet](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Figure 05**: Utilisation du concepteur relationnel de l’objet ([Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))

Par défaut, l’Object Relational Designer crée une classe du même nom que la table de base de données que vous faites glisser sur le Concepteur. Cependant, nous ne voulons pas `tblMovie`appeler notre classe . Par conséquent, cliquez sur le nom de la classe dans le concepteur et changer le nom de la classe en film.

Enfin, n’oubliez pas de cliquer sur le bouton **Enregistrer** (l’image de la disquette) pour enregistrer le LINQ à SQL Classes. Sinon, les classes LINQ à SQL ne seront pas générées par le concepteur relationnel d’objet.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Utilisation de LINQ à SQL dans une action de contrôleur

Maintenant que nous avons nos cours LINQ à SQL, nous pouvons utiliser ces classes pour récupérer les données de la base de données. Dans cette section, vous apprenez à utiliser les cours LINQ à SQL directement dans le cadre d’une action de contrôleur. Nous afficherons la liste des films à partir de la table de base de données tblMovies dans une vue MVC.

Tout d’abord, nous devons modifier la classe HomeController. Cette classe se trouve dans le dossier Contrôleurs de votre application. Modifier la classe de sorte qu’il ressemble à la classe dans la liste 1.

**Liste 1`Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

L’action `Index()` dans Listing 1 utilise un LINQ à `MovieDataContext`SQL `MoviesDB` DataContext classe (le ) pour représenter la base de données. La `MoveDataContext` classe a été générée par le Visual Studio Object Relational Designer.

Une requête LINQ est effectuée contre le DataContext pour `tblMovies` récupérer tous les films de la table de base de données. La liste des films est attribuée `movies`à une variable locale nommée . Enfin, la liste des films est transmise à la vue par les données de vue.

Afin de montrer les films, nous avons ensuite besoin de modifier la vue Index. Vous pouvez trouver la `Views\Home\` vue d’index dans le dossier. Mettre à jour la vue index de sorte qu’il ressemble à la vue dans la liste 2.

**Liste 2`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Notez que la vue `<%@ import namespace %>` modifiée de l’index comprend une directive en haut de la vue. Cette directive `MvcApplication1.Models namespace`importe le . Nous avons besoin de cet espace `model` de nom pour `Movie` travailler avec les classes - en particulier, la classe - dans la vue.

La vue dans la `foreach` liste 2 contient une boucle qui itère à travers tous les éléments représentés par la `ViewData.Model` propriété. La valeur `Title` de la propriété `movie`est affichée pour chacun .

Notez que la `ViewData.Model` valeur de `IEnumerable`la propriété est jetée à un . Ceci est nécessaire afin de boucler à travers le contenu de `ViewData.Model`. Une autre option ici est de `view`créer un fortement typé . Lorsque vous créez un `view`type fortement `ViewData.Model` typé, vous jetez la propriété à un type particulier dans la classe de code d’une vue.

Si vous exécutez l’application après modification de la `HomeController` classe et de la vue Index, vous obtiendrez une page blanche. Vous obtiendrez une page blanche parce qu’il `tblMovies` n’y a pas d’enregistrements de films dans le tableau de base de données.

Afin d’ajouter des `tblMovies` enregistrements à la `tblMovies` table de base de données, cliquez à droite sur le tableau de base de données de la fenêtre Server Explorer (fenêtre Database Explorer dans Visual Web Developer) et sélectionnez l’option de menu Afficher les données de table. Vous pouvez `movie` insérer des enregistrements en utilisant la grille qui apparaît (voir la figure 6).

[![Insertion de films](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Figure 06**: Insérer des films[(Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))

Après avoir ajouté des `tblMovies` enregistrements de base de données à la table et que vous avez exécuté l’application, vous verrez la page dans la figure 7. Tous les enregistrements de base de données de films sont affichés dans une liste de balles.

[![Affichage de films avec la vue Index](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Figure 07**: Affichage de films avec la vue indexée ([Cliquez pour voir l’image grandeur nature](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Utilisation du modèle de dépôt

Dans la section précédente, nous avons utilisé LINQ pour les classes SQL directement dans le cadre d’une action de contrôleur. Nous avons `MovieDataContext` utilisé la `Index()` classe directement à partir de l’action du contrôleur. Il n’y a rien de mal à le faire dans le cas d’une simple application. Cependant, travailler directement avec LINQ à SQL dans une classe de contrôleur crée des problèmes lorsque vous avez besoin de construire une application plus complexe.

L’utilisation de LINQ à SQL au sein d’une classe de contrôleur rend difficile le changement de technologie d’accès aux données à l’avenir. Par exemple, vous pouvez décider de passer de l’utilisation de Microsoft LINQ à SQL à l’utilisation du cadre d’entité Microsoft comme technologie d’accès aux données. Dans ce cas, vous devez réécrire chaque contrôleur qui accède à la base de données dans votre application.

L’utilisation de LINQ à SQL au sein d’une classe de contrôleur rend également difficile la construction de tests unitaires pour votre application. Normalement, vous ne voulez pas interagir avec une base de données lors de l’exécution des tests unitaires. Vous souhaitez utiliser vos tests unitaires pour tester la logique de votre application et non pas votre serveur de base de données.

Afin de construire une application MVC plus adaptable aux changements futurs et qui peut être testée plus facilement, vous devriez envisager d’utiliser le modèle De dépôt. Lorsque vous utilisez le modèle Repository, vous créez une classe de dépôt séparée qui contient toute la logique d’accès à votre base de données.

Lorsque vous créez la classe de dépôt, vous créez une interface qui représente toutes les méthodes utilisées par la classe de dépôt. Dans vos contrôleurs, vous écrivez votre code contre l’interface au lieu du référentiel. De cette façon, vous pouvez implémenter le référentiel à l’aide de différentes technologies d’accès aux données à l’avenir.

L’interface dans La `IMovieRepository` liste 3 est `ListAll()`nommée et elle représente une méthode unique nommée .

**Liste 3`Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

La classe de référentiel dans `IMovieRepository` La liste 4 implémente l’interface. Notez qu’il `ListAll()` contient une méthode nommée qui `IMovieRepository` correspond à la méthode requise par l’interface.

**Liste 4`Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Enfin, `MoviesController` la classe dans La liste 5 utilise le modèle de dépôt. Il n’utilise plus directement les cours linQ pour SQL.

**Liste 5`Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Notez `MoviesController` que la classe dans la liste 5 a deux constructeurs. Le premier constructeur, le constructeur sans paramètres, est appelé lorsque votre application est en cours d’exécution. Ce constructeur crée une `MovieRepository` instance de la classe et la transmet au deuxième constructeur.

Le deuxième constructeur a un `IMovieRepository` seul paramètre : un paramètre. Ce constructeur attribue simplement la valeur du paramètre à `_repository`un champ de classe nommé .

La `MoviesController` classe tire parti d’un modèle de conception logicielle appelé le modèle d’injection de dépendance. En particulier, il utilise quelque chose appelé Constructor Dependency Injection. Vous pouvez en savoir plus sur ce modèle en lisant l’article suivant par Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Notez que tout le `MoviesController` code de la classe (à l’exception `IMovieRepository` du premier `MovieRepository` constructeur) interagit avec l’interface au lieu de la classe réelle. Le code interagit avec une interface abstraite au lieu d’une implémentation concrète de l’interface.

Si vous souhaitez modifier la technologie d’accès aux données `IMovieRepository` utilisée par l’application, vous pouvez simplement implémenter l’interface avec une classe qui utilise la technologie d’accès alternative à la base de données. Par exemple, vous `EntityFrameworkMovieRepository` pouvez créer `SubSonicMovieRepository` une classe ou une classe. Parce que la classe de contrôleur est programmée contre `IMovieRepository` l’interface, vous pouvez passer une nouvelle implémentation de la classe de contrôleur et la classe continuerait à travailler.

En outre, si vous `MoviesController` voulez tester la classe, alors vous pouvez `HomeController`passer un faux film de dépôt de classe à la . Vous pouvez `IMovieRepository` implémenter la classe avec une classe qui n’accède pas réellement à la base de données mais contient toutes les méthodes requises de l’interface. `IMovieRepository` De cette façon, vous `MoviesController` pouvez tester la classe sans accéder à une véritable base de données.

## <a name="summary"></a>Récapitulatif

Le but de ce tutoriel était de démontrer comment vous pouvez créer des classes de modèles MVC en profitant de Microsoft LINQ à SQL. Nous avons examiné deux stratégies pour afficher les données de base de données dans une application MVC ASP.NET. Tout d’abord, nous avons créé LINQ à SQL classes et utilisé les classes directement dans une action de contrôleur. L’utilisation des classes LINQ à SQL au sein d’un contrôleur vous permet d’afficher rapidement et facilement les données de base de données dans une application MVC.

Ensuite, nous avons exploré un chemin un peu plus difficile, mais certainement plus vertueux, pour afficher les données de base de données. Nous avons profité du modèle De dépôt et placé toute notre logique d’accès de base de données dans une classe de dépôt séparée. Dans notre contrôleur, nous avons écrit tout notre code contre une interface au lieu d’une classe concrète. L’avantage du modèle Repository est qu’il nous permet de changer facilement les technologies d’accès aux bases de données à l’avenir et qu’il nous permet de tester facilement nos classes de contrôleurs.

> [!div class="step-by-step"]
> [Suivant précédent](creating-model-classes-with-the-entity-framework-cs.md)
> [Next](displaying-a-table-of-database-data-cs.md)
