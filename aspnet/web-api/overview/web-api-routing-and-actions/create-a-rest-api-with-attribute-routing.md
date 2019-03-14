---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Créer une API REST avec le routage par attributs dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 18a44c280e6df1603837938d24d7d639d8c87cc2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055176"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Créer une API REST avec routage d’attributs dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

Web API 2 prend en charge un nouveau type de routage, appelé *routage par attributs*. Pour obtenir une vue d’ensemble du routage par attributs, consultez [routage par attributs dans Web API 2](attribute-routing-in-web-api-2.md). Dans ce didacticiel, vous allez utiliser le routage par attributs pour créer une API REST pour un ensemble de livres. L’API prendra en charge les actions suivantes :

| Action | Exemple d’URI |
| --- | --- |
| Obtenir une liste de tous les livres. | documentation/api / |
| Obtenir un livre par ID. | /API/Books/1 |
| Obtenir les détails d’un livre. | /API/Books/1/Details |
| Obtenir une liste de livres par genre. | /API/Books/fantasy |
| Obtenir une liste de livres par date de publication. | /API/Books/date/2013-02-16 /api/books/date/2013/02/16 (autre forme) |
| Obtenir une liste de livres publiés par un auteur spécifique. | /api/authors/1/books |

Toutes les méthodes sont en lecture seule (requêtes HTTP GET).

Pour la couche données, nous allons utiliser Entity Framework. Enregistrements de livre aura les champs suivants :

- Id
- Titre
- Genre
- Date de publication
- Prix
- Description
- AuthorID (clé étrangère à une table Authors)

Toutefois, pour la plupart des requêtes, l’API retourne un sous-ensemble de ces données (titre, auteur et genre). Pour obtenir des demandes de l’enregistrement complet, le client `/api/books/{id}/details`.

## <a name="prerequisites"></a>Prérequis

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Commencez par exécuter Visual Studio. À partir de la **fichier** menu, sélectionnez **New** , puis sélectionnez **projet**.

Développez le **installé** > **Visual C#** catégorie. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **Application Web ASP.NET (.NET Framework)**. Nommez le projet &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

Dans le **nouvelle Application Web ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle. Sous « Ajouter des dossiers et les références principales pour », sélectionnez le **API Web** case à cocher. Cliquez sur **OK**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Cette opération crée une structure de projet qui est configuré pour la fonctionnalité de l’API Web.

### <a name="domain-models"></a>Modèles de domaine

Ensuite, ajoutez des classes pour les modèles de domaine. Dans l’Explorateur de solutions, cliquez sur le dossier de modèles. Sélectionnez **ajouter**, puis sélectionnez **classe**. Nommez la classe `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Remplacez le code dans Author.cs avec les éléments suivants :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Ajoutez maintenant une autre classe nommée `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Dans cette étape, nous allons ajouter un contrôleur d’API Web qui utilise Entity Framework en tant que la couche données.

Appuyez sur CTRL+MAJ+B pour générer le projet. Entity Framework utilise la réflexion pour découvrir les propriétés des modèles, de sorte qu’elle nécessite un assembly compilé créer le schéma de base de données.

Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs. Sélectionnez **ajouter**, puis sélectionnez **contrôleur**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

Dans le **ajouter une structure** boîte de dialogue, sélectionnez **contrôleur API Web 2 avec actions, à l’aide d’Entity Framework**.

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

Dans le **ajouter un contrôleur** boîte de dialogue, pour **nom du contrôleur**, entrez &quot;BooksController&quot;. Sélectionnez le &quot;utiliser les actions de contrôleur asynchrones&quot; case à cocher. Pour **classe de modèle**, sélectionnez &quot;livre&quot;. (Si vous ne voyez pas la `Book` classe répertorié dans la liste déroulante, assurez-vous que vous avez créé le projet.) Cliquez ensuite sur le bouton « + ».

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Cliquez sur **ajouter** dans le **nouveau contexte de données** boîte de dialogue.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Cliquez sur **ajouter** dans le **ajouter un contrôleur** boîte de dialogue. La génération de modèles automatique ajoute une classe nommée `BooksController` qui définit le contrôleur d’API. Il ajoute également une classe nommée `BooksAPIContext` dans le dossier Modèles, qui définit le contexte de données pour Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Amorcer la base de données

Dans le menu Outils, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.

Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Cette commande crée un dossier Migrations et ajoute un nouveau fichier de code nommé Configuration.cs. Ouvrez ce fichier et ajoutez le code suivant à la `Configuration.Seed` (méthode).

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Dans la fenêtre de Console du Gestionnaire de Package, tapez les commandes suivantes.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Ces commandes créent une base de données locale et appeler la méthode Seed pour remplir la base de données.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Ajouter des Classes DTO

Si vous exécutez maintenant l’application et envoyez une demande GET à /api/books/1, la réponse ressemble à ce qui suit. (J’ai ajouté la mise en retrait pour une meilleure lisibilité).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Au lieu de cela, je veux cette requête pour retourner un sous-ensemble des champs. En outre, je veux qu’il retourne le nom de l’auteur, plutôt que l’ID de l’auteur. Pour ce faire, nous allons modifier les méthodes de contrôleur pour retourner un *objet de transfert de données* (DTO) au lieu du modèle Entity Framework. Un objet DTO est un objet qui est conçu uniquement pour transporter des données.

Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** | **nouveau dossier**. Nommez le dossier &quot;DTO&quot;. Ajoutez une classe nommée `BookDto` dans le dossier DTO, avec la définition suivante :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Ajouter une autre classe nommée `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Ensuite, mettez à jour le `BooksController` classe pour retourner `BookDto` instances. Nous allons utiliser le [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) méthode au projet `Book` instances à `BookDto` instances. Voici le code mis à jour pour la classe de contrôleur.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> J’ai supprimé le `PutBook`, `PostBook`, et `DeleteBook` méthodes, car ils ne sont pas nécessaires pour ce didacticiel.


Maintenant si vous exécutez l’application et que vous demandez /api/books/1, le corps de réponse doit ressembler à ceci :

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Ajouter des attributs de Route

Ensuite, nous allons convertir le contrôleur pour utiliser le routage par attributs. Tout d’abord, ajoutez un **RoutePrefix** attribut au contrôleur. Cet attribut définit les segments d’URI initiales pour toutes les méthodes sur ce contrôleur.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Ajoutez ensuite **[Route]** des attributs pour les actions de contrôleur, comme suit :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Le modèle d’itinéraire pour chaque méthode de contrôleur est le préfixe ainsi que la chaîne spécifiée dans le **itinéraire** attribut. Pour le `GetBook` (méthode), le modèle d’itinéraire inclut la chaîne paramétrable &quot;{id : int}&quot;, qui met en correspondance si le segment d’URI contient une valeur entière.

| Méthode | Modèle de routage | Exemple d’URI |
| --- | --- | --- |
| `GetBooks` | « api/books » | `http://localhost/api/books` |
| `GetBook` | « api/la documentation / {id : int} » | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Obtenir les détails des livres

Pour obtenir les détails des livres, le client envoie une demande GET à `/api/books/{id}/details`, où *{id}* est l’ID du livre.

Ajoutez la méthode suivante à la classe `BooksController` .

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Si vous demandez `/api/books/1/details`, la réponse ressemble à ceci :

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Obtenir la documentation par Genre

Pour obtenir une liste de livres d’un genre spécifique, le client envoie une demande GET à `/api/books/genre`, où *genre* est le nom du genre. (Par exemple, `/api/books/fantasy`.)

Ajoutez la méthode suivante à `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Ici nous allons la définir un itinéraire qui contient un paramètre de {genre} dans le modèle d’URI. Notez que les API Web est en mesure de distinguer ces deux URI et de les acheminer vers les différentes méthodes :

`/api/books/1`

`/api/books/fantasy`

C’est parce que le `GetBook` méthode inclut une contrainte que le segment « id » doit être une valeur entière :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Si vous demandez /api/books/fantasy, la réponse ressemble à ceci :

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Obtenir la documentation par auteur

Pour obtenir une liste d’une documentation pour un auteur particulier, le client envoie une demande GET à `/api/authors/id/books`, où *id* est l’ID de l’auteur.

Ajoutez la méthode suivante à `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Cet exemple est intéressant car &quot;la documentation&quot; est traité une ressource enfant de &quot;auteurs&quot;. Ce modèle est assez courant dans les API RESTful.

Le tilde (~) dans le modèle d’itinéraire remplace le préfixe d’itinéraire dans le **RoutePrefix** attribut.

## <a name="get-books-by-publication-date"></a>Obtenir la documentation par Date de Publication

Pour obtenir une liste de livres par date de publication, le client envoie une demande GET à `/api/books/date/yyyy-mm-dd`, où *aaaa-mm-jj* correspond à la date.

Voici une façon de procéder :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Le `{pubdate:datetime}` paramètre est contraint pour faire correspondre un **DateTime** valeur. Cela fonctionne, mais elle est en fait plus permissive que nous voudrions. Par exemple, ces URI correspondent également l’itinéraire :

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Il n’existe pas de mal à autoriser ces URI. Toutefois, vous pouvez restreindre l’itinéraire dans un format particulier en ajoutant une contrainte d’expression régulière pour le modèle de routage :

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Désormais uniquement les dates sous la forme &quot;aaaa-mm-jj&quot; correspondra. Notez que nous n’utilisons l’expression régulière pour valider que nous avons une date réelle. Qui est gérée lors de l’API Web tente de convertir le segment d’URI dans un **DateTime** instance. Une date non valide comme « 2012-47-99 » ne pourra pas être converti, et le client reçoit une erreur 404.

Vous pouvez prennent également en charge un séparateur de barre oblique (`/api/books/date/yyyy/mm/dd`) en ajoutant un autre **[Route]** attribut avec un autre regex.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Il existe un détail subtil mais important ici. Le second modèle d’itinéraire comporte un caractère générique (\*) au début du paramètre {pubdate} :

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Cela indique au moteur de routage que pubdate {} doit correspondre à celui de l’URI. Par défaut, un paramètre de modèle correspond à un seul segment d’URI. Dans ce cas, nous souhaitons {pubdate} s’étendent sur plusieurs segments d’URI :

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Code du contrôleur

Voici le code complet pour la classe BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Récapitulatif

Routage par attributs vous donne davantage de contrôle et une plus grande flexibilité lors de la conception de l’URI pour votre API.
