---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Affichage d’un tableau des données de base de données (VB) Microsoft Docs
author: rick-anderson
description: Dans ce tutoriel, je démontre deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. Je montre deux méthodes de formatage d’un ensemble d’enregistrements de base de données dans un ta HTML ...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b03e6a0d2bf7d2bf59ba4dca3076fa306d3fed4
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541895"
---
# <a name="displaying-a-table-of-database-data-vb"></a>Affichage d’une table de données de la base de données (VB)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> Dans ce tutoriel, je démontre deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. Je montre deux méthodes de formatage d’un ensemble d’enregistrements de base de données dans une table HTML. Tout d’abord, je montre comment vous pouvez formater les enregistrements de base de données directement dans une vue. Ensuite, je montre comment vous pouvez profiter des partielles lors du formatage des enregistrements de base de données.

Le but de ce tutoriel est d’expliquer comment vous pouvez afficher une table HTML de données de base de données dans une application ASP.NET MVC. Tout d’abord, vous apprenez à utiliser les outils d’échafaudage inclus dans Visual Studio pour générer une vue qui affiche un ensemble d’enregistrements automatiquement. Ensuite, vous apprenez à utiliser une partie comme modèle lors du formatage des enregistrements de base de données.

## <a name="create-the-model-classes"></a>Créer les classes modèles

Nous allons afficher l’ensemble des enregistrements à partir de la table de base de données Movies. Le tableau de base de données Movies contient les colonnes suivantes :

<a id="0.4_table01"></a>

| **Nom de colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | Int | False |
| Intitulé | Nvarchar(200) | False |
| Directeur | NVarchar(50) | False |
| DateReleased | DateTime | False |

Afin de représenter la table Movies dans notre application ASP.NET MVC, nous devons créer une classe de modèle. Dans ce tutoriel, nous utilisons le cadre d’entité Microsoft pour créer nos classes de modèles.

> [!NOTE] 
> 
> Dans ce tutoriel, nous utilisons le cadre d’entité Microsoft. Cependant, il est important de comprendre que vous pouvez utiliser une variété de technologies différentes pour interagir avec une base de données à partir d’une application ASP.NET MVC, y compris LINQ à SQL, NHibernate, ou ADO.NET.

Suivez ces étapes pour lancer l’Assistant modèle de données d’entité :

1. Cliquez à droite sur le dossier Modèles dans la fenêtre Solution Explorer et la sélection de l’option menu **Ajouter, Nouvel article**.
2. Sélectionnez la catégorie **de données** et sélectionnez le modèle ADO.NET modèle de modèle de **données d’entité.**
3. Donnez à votre modèle de données le nom *MoviesDBModel.edmx* et cliquez sur le bouton **Ajouter.**

Après avoir cliqué sur le bouton Ajouter, l’Assistant modèle de données d’entité apparaît (voir la figure 1). Suivez ces étapes pour compléter l’assistant:

1. Dans l’étape Choisissez les **contenus modèles,** sélectionnez **l’option Générer à partir de l’option de base de données.**
2. Dans l’étape **Choisissez votre connexion de données,** utilisez la connexion de données *MoviesDB.mdf* et le nom *MoviesDBEntities* pour les paramètres de connexion. Cliquez sur le bouton **Suivant**.
3. Dans l’étape **Choisissez vos objets de base de données,** élargissez le nœud Tables, sélectionnez la table Films. Entrez les modèles d’espace *de* nom et cliquez sur le bouton **Finition.**

[![Création de cours LINQ à SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Figure 01**: Création de cours LINQ à SQL ([Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image2.png))

Après avoir terminé l’Assistant de modèle de données d’entité, l’Entity Data Model Designer s’ouvre. Le concepteur doit afficher l’entité Films (voir la figure 2).

[![Le concepteur de modèle de données d’entité](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Figure 02**: The Entity Data Model Designer[(Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image4.png))

Nous devons faire un changement avant de continuer. L’Assistant De Données d’Entité génère une classe de modèles nommée *Films* qui représente la table de base de données Movies. Parce que nous allons utiliser la classe Films pour représenter un film particulier, nous devons modifier le nom de la classe pour être *Movie* au lieu de *films* (singulier plutôt que pluriel).

Double-cliquez sur le nom de la classe sur la surface du concepteur et changer le nom de la classe de films à film. Après avoir apporté cette modification, cliquez sur le bouton **Enregistrer** (l’icône du disque disquette) pour générer la classe Film.

## <a name="create-the-movies-controller"></a>Créez le contrôleur des films

Maintenant que nous avons un moyen de représenter nos enregistrements de base de données, nous pouvons créer un contrôleur qui retourne la collection de films. Dans la fenêtre Visual Studio Solution Explorer, cliquez à droite sur le dossier Controllers et sélectionnez l’option menu **Ajouter, Contrôleur** (voir la figure 3).

[![Le menu Add Controller](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Figure 03**: Le menu Add Controller ([Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image6.png))

Lorsque le dialogue **Add Controller** apparaît, entrez le nom de contrôleur MovieController (voir la figure 4). Cliquez sur le bouton **Ajouter** pour ajouter le nouveau contrôleur.

[![Le dialogue Add Controller](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Figure 04**: Le dialogue Add Controller[(Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image8.png))

Nous devons modifier l’action Index() exposée par le contrôleur de film afin qu’elle retourne l’ensemble des enregistrements de base de données. Modifier le contrôleur de sorte qu’il ressemble au contrôleur dans la liste 1.

**Liste 1 - Controllers-MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Dans Listing 1, la classe MoviesDBEntities est utilisée pour représenter la base de données MoviesDB. Les *entités d’expression. MovieSet.ToList()* retourne l’ensemble de tous les films de la table de base de données Movies.

## <a name="create-the-view"></a>Créer la vue

La façon la plus simple d’afficher un ensemble d’enregistrements de base de données dans une table HTML est de profiter de l’échafaudage fourni par Visual Studio.

Construisez votre application en sélectionnant l’option de menu **Construire, Construire la solution**. Vous devez créer votre application avant d’ouvrir le dialogue **Add View** ou vos classes de données n’apparaîtront pas dans le dialogue.

Cliquez à droite sur l’action Index() et sélectionnez l’option de menu **Ajouter la vue** (voir la figure 5).

[![Ajout d’une vue](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Figure 05**: Ajout d’une vue[(Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image10.png))

Dans le dialogue **Add View,** vérifiez la case à cocher étiquetée **Créer une vue fortement typée**. Sélectionnez la classe De film comme classe **de données de vue**. Sélectionnez *Liste* comme **contenu de vue** (voir la figure 6). La sélection de ces options générera une vue fortement typée qui affiche une liste de films.

[![Le dialogue Add View](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Figure 06**: Le dialogue Add View[(Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image12.png))

Après avoir cliqué sur le bouton **Ajouter,** la vue dans la liste 2 est générée automatiquement. Cette vue contient le code nécessaire pour itérer à travers la collection de films et afficher chacune des propriétés d’un film.

**Liste 2 - Views-Movie.Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Vous pouvez exécuter l’application en sélectionnant l’option de menu **Debug, Démarrer Debugging** (ou frapper la clé F5). Exécution de l’application lance Internet Explorer. Si vous naviguez vers l’URL /Film, vous verrez la page dans la figure 7.

[![Une table de films](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Figure 07**: Une table de films[(Cliquez pour voir l’image grandeur nature](displaying-a-table-of-database-data-vb/_static/image14.png))

Si vous n’aimez rien à propos de l’apparition de la grille d’enregistrements de base de données dans la figure 7, alors vous pouvez simplement modifier la vue de l’index. Par exemple, vous pouvez modifier l’en-tête *DateReleased* à *date libérée* en modifiant la vue de l’index.

## <a name="create-a-template-with-a-partial"></a>Créez un modèle avec un

Quand une vue devient trop compliquée, c’est une bonne idée de commencer à briser la vue en partielles. L’utilisation de partielles facilite la compréhension et l’entretien de vos vues. Nous allons créer une partie que nous pouvons utiliser comme un modèle pour formater chacun des enregistrements de base de données de films.

Suivez ces étapes pour créer le partiel:

1. Cliquez à droite sur le dossier Views-Movie et sélectionnez l’option menu **Ajouter View**.
2. Vérifiez la case à cocher étiquetée *Créer une vue partielle (.ascx)*.
3. Nommez le *FilmTemplate*partiel .
4. Vérifiez la case à cocher étiquetée **Créer une vue fortement typée**.
5. Sélectionnez Movie comme *classe de données de vue*.
6. Sélectionnez Vide comme *contenu de vue*.
7. Cliquez sur le bouton **Ajouter** pour ajouter la partie à votre projet.

Après avoir terminé ces étapes, modifiez le MovieTemplate partielle pour ressembler à La liste 3.

**Liste 3 - Views-Movie-MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

La partie dans la liste 3 contient un modèle pour une seule rangée d’enregistrements.

La vue index modifiée dans la liste 4 utilise le MovieTemplate partiel.

**Liste 4 - Views-Movie.Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

La vue dans La liste 4 contient une boucle pour chaque qui itère à travers tous les films. Pour chaque film, le MovieTemplate partielle est utilisé pour formater le film. Le MovieTemplate est rendu en appelant la méthode d’aide RenderPartial().

La vue Index modifiée rend la même table HTML des enregistrements de base de données. Cependant, le point de vue a été grandement simplifié.

La méthode RenderPartial() est différente de la plupart des autres méthodes d’aide parce qu’elle ne renvoie pas une chaîne. Par conséquent, vous devez appeler la &lt;méthode RenderPartial ()&gt; en &lt;utilisant % Html.RenderPartial() % au lieu de % Html.RenderPartial() %&gt;.

## <a name="summary"></a>Récapitulatif

Le but de ce tutoriel était d’expliquer comment vous pouvez afficher un ensemble d’enregistrements de base de données dans une table HTML. Tout d’abord, vous avez appris à retourner un ensemble d’enregistrements de base de données à partir d’une action de contrôleur en profitant du cadre d’entité Microsoft. Ensuite, vous avez appris à utiliser les échafaudages Visual Studio pour générer une vue qui affiche automatiquement une collection d’objets. Enfin, vous avez appris à simplifier la vue en profitant d’une partielle. Vous avez appris à utiliser un modèle partiel pour formater chaque enregistrement de base de données.

> [!div class="step-by-step"]
> [Suivant précédent](creating-model-classes-with-linq-to-sql-vb.md)
> [Next](performing-simple-validation-vb.md)
