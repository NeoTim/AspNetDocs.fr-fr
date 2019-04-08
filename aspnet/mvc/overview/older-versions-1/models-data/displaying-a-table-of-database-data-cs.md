---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Affichage d’une Table de base de données (C#) | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, je vais montrer deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. Je montrerai des deux méthodes de mise en forme un jeu d’enregistrements de base de données dans le code HTML ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d0d3f6a574a4b923d5da73ccb2ab3bfbd6f305ef
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054826"
---
<a name="displaying-a-table-of-database-data-c"></a>Affichage d’une table de données de la base de données (C#)
====================
by [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> Dans ce didacticiel, je vais montrer deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. Je montrerai des deux méthodes de mise en forme d’un jeu d’enregistrements de base de données dans une table HTML. Tout d’abord, je montrerai comment vous pouvez mettre en forme les enregistrements de base de données directement dans une vue. Ensuite, je vais montrer comment vous pouvez tirer parti des vues partielles lors de la mise en forme d’enregistrements de base de données.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez afficher un tableau HTML de base de données dans une application ASP.NET MVC. Tout d’abord, vous allez apprendre à utiliser les outils de génération de modèles automatique inclus dans Visual Studio pour générer une vue qui affiche un ensemble d’enregistrements automatiquement. Ensuite, vous allez apprendre à utiliser un partiel en tant que modèle lors de la mise en forme d’enregistrements de base de données.

## <a name="create-the-model-classes"></a>Créer les Classes de modèle

Nous allons afficher le jeu d’enregistrements de la table de base de données de films. La table de base de données de films contient les colonnes suivantes :

<a id="0.3_table01"></a>


| **Nom de la colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | Int | False |
| Titre | Nvarchar(200) | False |
| Directeur | NVarchar(50) | False |
| DateReleased | DateTime | False |


Afin de représenter la table de films dans notre application ASP.NET MVC, nous devons créer une classe de modèle. Dans ce didacticiel, nous utilisons Microsoft Entity Framework pour créer nos classes de modèle.

> [!NOTE] 
> 
> Dans ce didacticiel, nous utilisons Microsoft Entity Framework. Toutefois, il est important de comprendre que vous pouvez utiliser une variété de technologies différentes pour interagir avec une base de données à partir d’une application ASP.NET MVC, y compris LINQ to SQL, NHibernate ou ADO.NET.


Suivez ces étapes pour lancer l’Assistant Entity Data Model :

1. Cliquez sur le dossier de modèles dans la fenêtre Explorateur de solutions puis sélectionnez l’option de menu **ajouter, nouvel élément**.
2. Sélectionnez le **données** catégorie, puis sélectionnez le **ADO.NET Entity Data Model** modèle.
3. Nommez votre modèle de données *MoviesDBModel.edmx* et cliquez sur le **ajouter** bouton.

Après avoir cliqué sur le bouton Ajouter, l’Assistant Entity Data Model s’affiche (voir Figure 1). Suivez ces étapes pour terminer l’Assistant :

1. Dans le **choisir le contenu du modèle** étape, sélectionnez le **générer à partir de la base de données** option.
2. Dans le **choisir votre connexion de données** étape, utilisez le *MoviesDB.mdf* connexion de données et le nom *MoviesDBEntities* des paramètres de connexion. Cliquez sur le **suivant** bouton.
3. Dans le **choisir vos objets de base de données** étape, développez le nœud Tables, sélectionnez la table de films. Entrez l’espace de noms *modèles* et cliquez sur le **Terminer** bouton.


[![Création de LINQ aux classes SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figure 01**: Création de LINQ aux classes SQL ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image2.png))


Après avoir terminé l’Assistant Entity Data Model, Entity Data Model Designer s’ouvre. Le concepteur doit afficher l’entité de films (voir Figure 2).


[![L’Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figure 02**: L’Entity Data Model Designer ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image4.png))


Nous devons apporter une modification avant de continuer. L’Assistant génère une classe de modèle nommée *films* qui représente la table de base de données de films. Étant donné que nous allons utiliser la classe de films pour représenter un film, nous devons modifier le nom de la classe à être *film* au lieu de *films* (singulier plutôt qu’au pluriel).

Double-cliquez sur le nom de la classe sur l’aire du concepteur et modifiez le nom de la classe films par film. Après avoir apporté cette modification, cliquez sur le **enregistrer** bouton (l’icône de la disquette) pour générer la classe Movie.

## <a name="create-the-movies-controller"></a>Créer le contrôleur de films

Maintenant que nous avons un moyen pour représenter des enregistrements de notre base de données, nous pouvons créer un contrôleur qui retourne la collection de films. Dans la fenêtre Explorateur de solutions Visual Studio, cliquez sur le dossier contrôleurs, puis sélectionnez l’option de menu **ajouter, de contrôleur** (voir Figure 3).


[![Le contrôleur Menu ajouter](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figure 03**: Le contrôleur de Menu d’ajout ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image6.png))


Lorsque le **ajouter un contrôleur** boîte de dialogue s’affiche, entrez le nom du contrôleur MovieController (voir Figure 4). Cliquez sur le **ajouter** pour ajouter le nouveau contrôleur.


[![La boîte de dialogue Ajouter un contrôleur](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figure 04**: La boîte de dialogue Ajouter un contrôleur ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image8.png))


Nous avons besoin modifier l’action Index() exposée par le contrôleur de films afin qu’il retourne le jeu d’enregistrements de base de données. Modifier le contrôleur afin qu’il semble que le contrôleur dans le Listing 1.

**Liste 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Dans la liste 1, la classe MoviesDBEntities est utilisée pour représenter la base de données MoviesDB. Pour utiliser cette classe, vous devez importer l’espace de noms MvcApplication1.Models comme suit :

à l’aide de MvcApplication1.Models ;

L’expression *entités. MovieSet.ToList()* retourne le jeu de tous les films à partir de la table de base de données de films.

## <a name="create-the-view"></a>Créer la vue

Pour afficher un jeu d’enregistrements de base de données dans un tableau HTML, le plus simple consiste à tirer parti de la génération de modèles fournie par Visual Studio.

Générez votre application en sélectionnant l’option de menu **créer, générer la Solution**. Vous devez générer votre application avant d’ouvrir le **ajouter une vue** boîte de dialogue ou de vos classes de données n’apparaîtront pas dans la boîte de dialogue.

L’action Index() de clic droit et sélectionnez l’option de menu **ajouter une vue** (voir Figure 5).


[![Ajout d’une vue](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figure 05**: Ajout d’une vue ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image10.png))


Dans le **ajouter une vue** boîte de dialogue, cochez la case intitulée **créer une vue fortement typée**. Sélectionnez la classe Movie comme le **afficher la classe de données**. Sélectionnez *liste* en tant que le **afficher le contenu** (voir Figure 6). Sélection de ces options génère une vue fortement typée qui affiche une liste de films.


[![La boîte de dialogue Ajouter une vue](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figure 06**: La boîte de dialogue Ajouter une vue ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image12.png))


Après avoir cliqué sur le **ajouter** bouton, la vue dans la liste 2 est généré automatiquement. Cette vue contient le code requis pour effectuer une itération dans la collection de films et afficher chacune des propriétés d’un film.

**Listing 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Vous pouvez exécuter l’application en sélectionnant l’option de menu **déboguer, démarrer le débogage** (ou en appuyant sur la touche F5). Exécution de l’application lance Internet Explorer. Si vous accédez à l’URL de /Movie vous verrez la page dans la Figure 7.


[![Une table de films](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figure 07**: Une table de films ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image14.png))


Si vous n’aimez pas quoi que ce soit sur l’apparence de la grille d’enregistrements de base de données dans la Figure 7 vous pouvez simplement modifier la vue Index. Par exemple, vous pouvez modifier le *DateReleased* en-tête à *Date de publication* en modifiant la vue Index.

## <a name="create-a-template-with-a-partial"></a>Créer un modèle avec un partiel

Lorsqu’une vue est trop compliquée, il est judicieux de commencer avec rupture de la vue dans les vues partielles. À l’aide de vues partielles rend plus facile à comprendre et à gérer vos vues. Nous allons créer un partiel qui nous pouvons utiliser comme modèle pour mettre en forme chacun des enregistrements de base de données de film.

Suivez ces étapes pour créer le partielle :

1. Cliquez sur le dossier Views\Movie et sélectionnez l’option de menu **ajouter une vue**.
2. Cochez la case intitulée *créer une vue partielle (.ascx)*.
3. Nommez le partielle *MovieTemplate*.
4. Cochez la case intitulée **créer une vue fortement typée**.
5. Sélectionnez le film comme le *afficher la classe de données*.
6. Sélectionnez vide en tant que le *afficher le contenu*.
7. Cliquez sur le **ajouter** pour ajouter le partielle à votre projet.

Après avoir effectué ces étapes, modifiez le MovieTemplate partielle ressemble à la liste 3.

**Liste 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Partielle dans le Listing 3 contient un modèle pour une seule ligne d’enregistrements.

La vue Index modifiée sur la liste 4 utilise la MovieTemplate partielle.

**Liste 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La vue sur la liste 4 contient une boucle foreach qui effectue une itération dans tous les films. Pour chaque film, le MovieTemplate partielle est utilisé pour mettre en forme le film. Le MovieTemplate est rendu en appelant la méthode d’assistance RenderPartial().

La vue Index modifiée restitue la même table HTML d’enregistrements de base de données. Toutefois, la vue a été considérablement simplifiée.


La méthode RenderPartial() est différente de la plupart des autres méthodes d’assistance, car elle ne retourne pas une chaîne. Par conséquent, vous devez appeler la méthode RenderPartial() à l’aide &lt;% Html.RenderPartial() ; %&gt; au lieu de &lt;% = Html.RenderPartial() ; %&gt;.


## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel est d’expliquer comment vous pouvez afficher un jeu d’enregistrements de base de données dans une table HTML. Tout d’abord, vous avez appris à retourner un jeu d’enregistrements de base de données à partir d’une action de contrôleur en tirant parti de Microsoft Entity Framework. Ensuite, vous avez appris à utiliser la structure de Visual Studio pour générer une vue qui affiche une collection d’éléments automatiquement. Enfin, vous avez appris à simplifier l’affichage en tirant parti d’un partiel. Vous avez appris comment utiliser un partiel en tant que modèle afin que vous pouvez mettre en forme chaque enregistrement de base de données.

> [!div class="step-by-step"]
> [Précédent](creating-model-classes-with-linq-to-sql-cs.md)
> [Suivant](performing-simple-validation-cs.md)
