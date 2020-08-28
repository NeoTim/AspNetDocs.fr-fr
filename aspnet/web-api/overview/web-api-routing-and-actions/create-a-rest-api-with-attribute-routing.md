---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Créer une API REST avec routage d’attribut dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: f6ff5fa18a44b3e6717ec0141ebe101bcdc0bee4
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045180"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Créer une API REST avec routage d’attribut dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

L’API Web 2 prend en charge un nouveau type de routage, appelé *routage d’attribut*. Pour obtenir une vue d’ensemble générale du routage d’attributs, consultez [routage d’attributs dans l’API Web 2](attribute-routing-in-web-api-2.md). Dans ce didacticiel, vous allez utiliser le routage d’attributs pour créer une API REST pour une collection de livres. L’API prend en charge les actions suivantes :

| Action | Exemple d’URI |
| --- | --- |
| Obtenir la liste de tous les livres. | /api/books |
| Obtenir un livre par ID. | /api/books/1 |
| Obtenir les détails d’un livre. | /api/books/1/details |
| Obtient une liste de livres par genre. | /api/books/fantasy |
| Obtenir une liste de livres par date de publication. | /API/Books/date/2013-02-16/API/Books/date/2013/02/16 (autre forme) |
| Obtenir la liste des livres d’un auteur donné. | /api/authors/1/books |

Toutes les méthodes sont en lecture seule (requêtes HTTP d’extraction).

Pour la couche de données, nous allons utiliser Entity Framework. Les enregistrements de livres comporteront les champs suivants :

- ID
- Titre
- Genre
- Date de publication
- Prix
- Description
- Réfaut-réfaut (clé étrangère à une table authors)

Toutefois, pour la plupart des requêtes, l’API retourne un sous-ensemble de ces données (titre, auteur et genre). Pour obtenir l’enregistrement complet, le client demande `/api/books/{id}/details` .

## <a name="prerequisites"></a>Prérequis

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise Edition.

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Commencez par exécuter Visual Studio. Dans le menu **File** (Fichier), sélectionnez **New** (Nouveau), puis **Project** (Projet).

Développez **Installed**la  >  catégorie**Visual C#** installée. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.net (.NET Framework)**. Nommez le projet &quot; BooksAPI &quot; .

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

Dans la boîte de dialogue **nouvelle application Web ASP.net** , sélectionnez le modèle **vide** . Sous « ajouter des dossiers et des références principales pour », activez la case à cocher **API Web** . Cliquez sur **OK**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Cela crée un projet squelette configuré pour la fonctionnalité de l’API Web.

### <a name="domain-models"></a>Modèles de domaine

Ensuite, ajoutez des classes pour les modèles de domaine. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles. Sélectionnez **Ajouter**, puis **classe**. Nommez la classe `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Remplacez le code dans Author.cs par le code suivant :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Ajoutez maintenant une autre classe nommée `Book` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Dans cette étape, nous allons ajouter un contrôleur d’API Web qui utilise Entity Framework comme couche de données.

Appuyez sur CTRL+MAJ+B pour générer le projet. Entity Framework utilise la réflexion pour découvrir les propriétés des modèles. il requiert donc un assembly compilé pour créer le schéma de base de données.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Contrôleurs. Sélectionnez **Ajouter**, puis **contrôleur**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **contrôleur d’API Web 2 avec actions, à l’aide de Entity Framework**.

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

Dans la boîte de dialogue **Ajouter un contrôleur** , pour **nom du contrôleur**, entrez &quot; BooksController &quot; . Cochez la &quot; case utiliser les actions du contrôleur Async &quot; . Pour **classe de modèle**, sélectionnez &quot; livre &quot; . (Si vous ne voyez pas la `Book` classe dans la liste déroulante, assurez-vous que vous avez créé le projet.) Cliquez ensuite sur le bouton « + ».

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Cliquez sur **Ajouter** dans la boîte de dialogue **nouveau contexte de données** .

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Cliquez sur **Ajouter** dans la boîte de dialogue **Ajouter un contrôleur** . La génération de modèles automatique ajoute une classe nommée `BooksController` qui définit le contrôleur d’API. Elle ajoute également une classe nommée `BooksAPIContext` dans le dossier Models, qui définit le contexte de données pour Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Amorcer la base de données

Dans le menu Outils, sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.

Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Cette commande crée un dossier migrations et ajoute un nouveau fichier de code nommé Configuration.cs. Ouvrez ce fichier et ajoutez le code suivant à la `Configuration.Seed` méthode.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Dans la fenêtre console du gestionnaire de package, tapez les commandes suivantes.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Ces commandes créent une base de données locale et appellent la méthode Seed pour remplir la base de données.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Ajouter des classes DTO

Si vous exécutez l’application maintenant et envoyez une requête d’extraction à/API/Books/1, la réponse ressemble à ce qui suit. (J’ai ajouté une mise en retrait pour une meilleure lisibilité.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Au lieu de cela, je souhaite que cette requête retourne un sous-ensemble des champs. En outre, je souhaite qu’elle retourne le nom de l’auteur plutôt que l’ID de l’auteur. Pour ce faire, nous allons modifier les méthodes de contrôleur pour retourner un *objet de transfert de données* (DTO) à la place du modèle EF. Un DTO est un objet conçu uniquement pour transporter des données.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter**  |  **nouveau dossier**. Nommez le dossier &quot; DTO &quot; . Ajoutez une classe nommée `BookDto` au dossier DTO, avec la définition suivante :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Ajoutez une autre classe nommée `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Ensuite, mettez à jour la `BooksController` classe pour retourner des `BookDto` instances. Nous allons utiliser la méthode [Queryable. Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) pour projeter des `Book` instances sur des instances `BookDto` . Voici le code mis à jour pour la classe Controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> J’ai supprimé `PutBook` les `PostBook` méthodes, et `DeleteBook` , car elles ne sont pas nécessaires pour ce didacticiel.

Maintenant, si vous exécutez l’application et demandez/API/Books/1, le corps de la réponse doit se présenter comme suit :

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Ajouter des attributs de route

Ensuite, nous allons convertir le contrôleur pour utiliser le routage d’attribut. Tout d’abord, ajoutez un attribut **RoutePrefix** au contrôleur. Cet attribut définit les segments d’URI initiaux pour toutes les méthodes sur ce contrôleur.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Ajoutez ensuite les attributs **[route]** aux actions du contrôleur, comme suit :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Le modèle de routage pour chaque méthode de contrôleur est le préfixe plus la chaîne spécifiée dans l’attribut de **routage** . Pour la `GetBook` méthode, le modèle de routage inclut la chaîne paramétrable &quot; {ID : int} &quot; , qui correspond si le segment d’URI contient une valeur entière.

| Méthode | Modèle de routage | Exemple d’URI |
| --- | --- | --- |
| `GetBooks` | « API/livres » | `http://localhost/api/books` |
| `GetBook` | « API/livres/{ID : int} » | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Récupérer les détails du livre

Pour obtenir les détails du livre, le client envoie une requête d’extraction à `/api/books/{id}/details` , où *{ID}* est l’ID du livre.

Ajoutez la méthode suivante à la classe `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Si vous demandez `/api/books/1/details` , la réponse ressemble à ceci :

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Récupérer des livres par genre

Pour obtenir une liste de livres dans un genre spécifique, le client envoie une requête d’extraction à `/api/books/genre` , où *genre* est le nom du genre. (Par exemple, `/api/books/fantasy`.)

Ajoutez la méthode suivante à `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Ici, nous définissons un itinéraire qui contient un paramètre {genre} dans le modèle d’URI. Notez que l’API Web est en mesure de distinguer ces deux URI et de les acheminer vers différentes méthodes :

`/api/books/1`

`/api/books/fantasy`

Cela est dû au fait que la `GetBook` méthode comprend une contrainte indiquant que le segment « ID » doit être une valeur entière :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Si vous demandez/API/Books/Fantasy, la réponse ressemble à ceci :

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Recevoir des livres par auteur

Pour obtenir la liste des livres d’un auteur donné, le client envoie une requête d’extraction à `/api/authors/id/books` , où *ID* est l’ID de l’auteur.

Ajoutez la méthode suivante à `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Cet exemple est intéressant, &quot; car &quot; la documentation est traitée comme une ressource enfant des &quot; auteurs &quot; . Ce modèle est assez courant dans les API RESTful.

Le tilde (~) dans le modèle de routage remplace le préfixe d’itinéraire dans l’attribut **RoutePrefix** .

## <a name="get-books-by-publication-date"></a>Récupérer les livres par date de publication

Pour obtenir une liste de livres par date de publication, le client envoie une requête d’extraction à `/api/books/date/yyyy-mm-dd` , où *aaaa-mm-jj* est la date.

Voici une façon de procéder :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Le `{pubdate:datetime}` paramètre est restreint pour correspondre à une valeur **DateTime** . Cela fonctionne, mais il est en fait plus permissif que ce que nous aimerions. Par exemple, ces URI correspondent également à l’itinéraire :

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Il n’y a rien de mal à autoriser ces URI. Toutefois, vous pouvez limiter l’itinéraire à un format particulier en ajoutant une contrainte d’expression régulière au modèle de routage :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

À présent, seules les dates au format &quot; aaaa-mm-jj &quot; correspondent. Notez que nous n’utilisons pas l’expression régulière pour confirmer que nous obtenons une date réelle. Qui est géré lorsque l’API Web tente de convertir le segment d’URI en une instance **DateTime** . Une date non valide telle que « 2012-47-99 » ne sera pas convertie et le client obtiendra une erreur 404.

Vous pouvez également prendre en charge un séparateur de barre oblique ( `/api/books/date/yyyy/mm/dd` ) en ajoutant un autre attribut **[route]** avec une expression régulière différente.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Il y a un détail subtil mais important ici. Le deuxième modèle de routage comporte un caractère générique ( \* ) au début du paramètre {pubDate} :

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.txt)]

Cela indique au moteur de routage que {pubDate} doit correspondre au reste de l’URI. Par défaut, un paramètre de modèle correspond à un segment d’URI unique. Dans ce cas, nous souhaitons que {pubDate} couvre plusieurs segments d’URI :

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Code du contrôleur

Voici le code complet de la classe BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Résumé

Le routage des attributs vous offre davantage de contrôle et une plus grande flexibilité lors de la conception des URI de votre API.
