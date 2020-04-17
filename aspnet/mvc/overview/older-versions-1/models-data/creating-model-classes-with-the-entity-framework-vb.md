---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Création de classes modèles avec le Cadre d’entités (VB) Microsoft Docs
author: rick-anderson
description: Dans ce tutoriel, vous apprenez à utiliser ASP.NET MVC avec le cadre d’entité Microsoft. Vous apprenez à utiliser l’Assistant d’Entité pour créer une ADO.NET’Entité Da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: cdba91969a6ed9c02965999bbc48d5c5cdea140d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542649"
---
# <a name="creating-model-classes-with-the-entity-framework-vb"></a>Création de classes de modèle avec Entity Framework (VB)

par [Microsoft](https://github.com/microsoft)

> Dans ce tutoriel, vous apprenez à utiliser ASP.NET MVC avec le cadre d’entité Microsoft. Vous apprenez à utiliser l’Assistant d’Entité pour créer un modèle de données d’entité ADO.NET. Au cours de ce tutoriel, nous construisons une application Web qui illustre comment sélectionner, insérer, mettre à jour et supprimer les données de base de données en utilisant le cadre d’entité.

Le but de ce tutoriel est d’expliquer comment vous pouvez créer des classes d’accès aux données en utilisant le cadre d’entité Microsoft lors de la construction d’une application ASP.NET MVC. Ce tutoriel suppose qu’aucune connaissance préalable du cadre d’entité Microsoft. À la fin de ce tutoriel, vous comprendrez comment utiliser le cadre d’entité pour sélectionner, insérer, mettre à jour et supprimer les enregistrements de base de données.

Le cadre d’entité Microsoft est un outil de cartographie relationnelle d’objet (O/RM) qui vous permet de générer automatiquement une couche d’accès aux données à partir d’une base de données. Le cadre d’entité vous permet d’éviter le travail fastidieux de construction de vos classes d’accès aux données à la main.

> [!NOTE] 
> 
> Il n’y a pas de lien essentiel entre ASP.NET MVC et le cadre d’entité Microsoft. Il existe plusieurs alternatives au Cadre d’entité que vous pouvez utiliser avec ASP.NET MVC. Par exemple, vous pouvez créer vos classes de modèles MVC à l’aide d’autres outils O/RM tels que Microsoft LINQ à SQL, NHibernate ou SubSonic.

Afin d’illustrer comment vous pouvez utiliser le cadre d’entité Microsoft avec ASP.NET MVC, nous allons construire une application d’échantillon simple. Nous créerons une application Movie Database qui vous permettra d’afficher et d’éditer des enregistrements de bases de données de films.

Ce tutoriel suppose que vous avez Visual Studio 2008 ou Visual Web Developer 2008 avec Service Pack 1. Vous avez besoin de Service Pack 1 pour utiliser le cadre d’entité. Vous pouvez télécharger Visual Studio 2008 Service Pack 1 ou Visual Web Developer avec Service Pack 1 à partir de l’adresse suivante :

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

## <a name="creating-the-movie-sample-database"></a>Création de la base de données sur les échantillons de films

L’application Movie Database utilise un tableau de base de données nommé Films qui contient les colonnes suivantes :

| Nom de la colonne | Type de données | Autoriser Nulls? | La clé primaire est-elle la clé? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Intitulé | nvarchar(100) | False | False |
| Directeur | nvarchar(100) | False | False |

Vous pouvez ajouter cette table à un projet MVC ASP.NET en suivant ces étapes :

1. Cliquez à droite\_sur le dossier App Data dans la fenêtre Solution Explorer et sélectionnez l’option menu **Ajouter, Nouvel article.**
2. À partir de la boîte de dialogue **Add New Item,** sélectionnez **SQL Server Database**, donnez à la base de données le nom MoviesDB.mdf et cliquez sur le bouton **Ajouter.**
3. Double-cliquez sur le fichier MoviesDB.mdf pour ouvrir la fenêtre Server Explorer/Database Explorer.
4. Élargissez la connexion de base de données MoviesDB.mdf, cliquez à droite sur le dossier Tables et sélectionnez l’option menu **Ajouter une nouvelle table**.
5. Dans le concepteur de table, ajoutez les colonnes Id, Titre et Directeur.
6. Cliquez sur le bouton **Enregistrer** (il a l’icône de la disquette) pour enregistrer la nouvelle table avec le nom Films.

Après avoir créé la table de base de données Movies, vous devez ajouter des données d’exemple à la table. Cliquez à droite sur la table Films et sélectionnez l’option menu **Afficher les données de table**. Vous pouvez saisir de fausses données de film dans la grille qui apparaît.

## <a name="creating-the-adonet-entity-data-model"></a>Création du modèle de données ADO.NET entity

Afin d’utiliser le cadre d’entité, vous devez créer un modèle de données d’entité. Vous pouvez profiter de *l’assistant modèle* de données Visual Studio Entity pour générer automatiquement un modèle de données d’entité à partir d’une base de données.

Procédez comme suit :

1. Cliquez à droite sur le dossier Modèles dans la fenêtre Solution Explorer et sélectionnez l’option menu **Ajouter, Nouvel article**.
2. Dans le dialogue **Add New Item,** sélectionnez la catégorie Données (voir la figure 1).
3. Sélectionnez le modèle **ADO.NET modèle de modèle de données d’entité,** donnez au modèle de données d’entité le nom MoviesDBModel.edmx, et cliquez sur le bouton **Ajouter.** En cliquant sur le bouton **Ajouter,** le modèle de données Wizard lance l’assistant de modèle de données.
4. Dans l’étape Choisissez le **contenu du modèle,** choisissez le **Générer à partir d’une** option de base de données et cliquez sur le bouton **Suivant** (voir la figure 2).
5. Dans l’étape **Choisissez votre connexion de données,** sélectionnez la connexion de base de données MoviesDB.mdf, entrez les paramètres de connexion des entités nom de FilmsDBEntities et cliquez sur le bouton **Suivant** (voir la figure 3).
6. Dans l’étape **Choisissez vos objets de base de données,** sélectionnez la table de base de données Movie et cliquez sur le bouton **Finition** (voir la figure 4).

Après avoir terminé ces étapes, le ADO.NET Entity Data Model Designer (Entity Designer) s’ouvre.

**Figure 1 - Création d’un nouveau modèle de données d’entité**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figure 2 - Choisissez l’étape du contenu du modèle**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figure 3 - Choisissez votre connexion de données**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figure 4 - Choisissez vos objets de base de données**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modification du modèle de données ADO.NET entité

Après avoir créé un modèle de données d’entité, vous pouvez modifier le modèle en profitant du concepteur d’entités (voir la figure 5). Vous pouvez ouvrir le concepteur d’entités à tout moment en cliquant à deux reprises sur le fichier MoviesDBModel.edmx contenu dans le dossier Models dans la fenêtre Solution Explorer.

**Figure 5 - Le concepteur de modèle de données de l’entité ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Par exemple, vous pouvez utiliser le concepteur d’entités pour modifier les noms des classes générées par l’Assistant de Données Du Modèle d’Entité. Le Magicien a créé une nouvelle classe d’accès aux données nommée Movies. En d’autres termes, l’Assistant a donné à la classe le même nom que la table de base de données. Parce que nous allons utiliser cette classe pour représenter une instance de film particulier, nous devrions renommer la classe de films à film.

Si vous souhaitez renommer une classe d’entités, vous pouvez cliquer deux fois sur le nom de la classe dans le concepteur d’entités et entrer un nouveau nom (voir la figure 6). Alternativement, vous pouvez changer le nom d’une entité dans la fenêtre Propriétés après avoir sélectionné une entité dans le concepteur d’entités.

**Figure 6 - Changer un nom d’entité**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

N’oubliez pas d’enregistrer votre modèle de données d’entité après avoir apporté une modification en cliquant sur le bouton Enregistrer (l’icône du disque disquette). Dans les coulisses, le concepteur d’entités génère un ensemble de classes Visual Basic .NET. Vous pouvez consulter ces classes en ouvrant le fichier MoviesDBModel.Designer.vb depuis la fenêtre Solution Explorer.

Ne modifiez pas le code dans le fichier Designer.vb puisque vos modifications seront écrasées la prochaine fois que vous utiliserez le concepteur d’entités. Si vous souhaitez étendre la fonctionnalité des classes d’entité définies dans le fichier Designer.vb, vous pouvez créer *des classes partielles* dans des fichiers distincts.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Sélection des enregistrements de bases de données avec le Cadre d’entité

Commençons à construire notre application Movie Database en créant une page qui affiche une liste de disques. Le contrôleur à domicile dans la liste 1 expose une action nommée Index(). L’action Index() renvoie tous les enregistrements de films à partir de la table de base de données Movie en profitant du cadre d’entité.

**Liste 1 - Controllers-HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Notez que le contrôleur dans la liste 1 inclut un constructeur. Le constructeur initialise un champ de \_classe nommé db. Le \_champ db représente les entités de base de données générées par le cadre d’entité Microsoft. Le \_champ db est un exemple de la classe MoviesDBEntities qui a été généré par le concepteur d’entités.

Le \_champ db est utilisé dans l’action Index() pour récupérer les enregistrements de la table de base de données Movies. L’expression \_db. MovieSet représente tous les enregistrements de la table de base de données Movies. La méthode ToList() est utilisée pour convertir l’ensemble de films en une collection générique d’objets movie: List ( Of Movie).

Les enregistrements du film sont récupérés avec l’aide de LINQ aux entités. L’action Index() dans La liste 1 utilise *la syntaxe de méthode* LINQ pour récupérer l’ensemble des enregistrements de base de données. Si vous préférez, vous pouvez utiliser la *syntaxe de requête* LINQ à la place. Les deux déclarations suivantes font la même chose :

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Utilisez la syntaxe LINQ - syntaxe de méthode ou syntaxe de requête - que vous trouvez la plus intuitive. Il n’y a pas de différence de performance entre les deux approches - la seule différence est le style.

La vue dans La liste 2 est utilisée pour afficher les enregistrements du film.

**Liste 2 - Views-Home.Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

La vue dans Listing 2 contient une boucle **pour chaque** film qui itère à travers chaque album de film et affiche les valeurs des propriétés titre et réalisateur de l’enregistrement du film. Notez qu’un lien Edit and Delete s’affiche à côté de chaque enregistrement. De plus, un lien Add Movie apparaît au bas de la vue (voir la figure 7).

**Figure 7 - La vue de l’indice**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

La vue index est une *vue dactylographe*. L’avis de &lt;l’indice&gt; comporte une directive de % et de pourcentage de pages qui comprend un attribut Héritière. L’attribut Inherits jette la propriété ViewData.Model à une collection de listes génériques fortement dactylographiées d’objets movie - une liste (Of Movie).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insérer les enregistrements de bases de données avec le Cadre d’entité

Vous pouvez utiliser le cadre d’entité pour faciliter l’insertion de nouveaux enregistrements dans une table de base de données. La liste 3 contient deux nouvelles actions ajoutées à la classe de contrôleur d’accueil que vous pouvez utiliser pour insérer de nouveaux enregistrements dans le tableau de base de données Movie.

**Liste 3 - Controllers-HomeController.vb (Méthodes d’ajout)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

La première action Add() renvoie simplement une vue. La vue contient un formulaire pour l’ajout d’un nouvel enregistrement de base de données de films (voir la figure 8). Lorsque vous soumettez le formulaire, la deuxième action Add() est invoquée.

Notez que la deuxième action Add() est décorée avec l’attribut AcceptVerbs. Cette action ne peut être invoquée que lors de l’exécution d’une opération HTTP POST. En d’autres termes, cette action ne peut être invoquée que lors de l’affichage d’un formulaire HTML.

La deuxième action Add() crée une nouvelle instance de la classe de film-cadre d’entité avec l’aide de la méthode ASP.NET MVC TryUpdateModel() . La méthode TryUpdateModel() prend les champs dans la FormCollection passé à la méthode Add() et attribue les valeurs de ces champs de forme HTML à la classe de film.

Lorsque vous utilisez le cadre d’entité, vous devez fournir une « liste blanche » de propriétés lors de l’utilisation des méthodes TryUpdateModel ou UpdateModel pour mettre à jour les propriétés d’une classe d’entités.

Ensuite, l’action Add() effectue une validation de formulaire simple. L’action vérifie que les propriétés Titre et Directeur ont des valeurs. S’il y a une erreur de validation, un message d’erreur de validation est ajouté à ModelState.

S’il n’y a pas d’erreurs de validation, un nouvel enregistrement de film est ajouté à la table de base de données Movies avec l’aide du cadre d’entité. Le nouvel enregistrement est ajouté à la base de données avec les deux lignes de code suivantes :

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

La première ligne de code ajoute la nouvelle entité Movie à l’ensemble de films suivis par le Cadre d’entité. La deuxième ligne de code enregistre les modifications apportées aux films qui sont suivis vers la base de données sous-jacente.

**Figure 8 - La vue Add**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Mise à jour des dossiers de base de données avec le Cadre d’entité

Vous pouvez suivre presque la même approche pour modifier un enregistrement de base de données avec le cadre d’entité que l’approche que nous venons de suivre pour insérer un nouvel enregistrement de base de données. La liste 4 contient deux nouvelles actions de contrôleur nommées Edit(). La première action Edit() renvoie un formulaire HTML pour l’édition d’un disque de film. La deuxième action Edit() tente de mettre à jour la base de données.

**Liste 4 - Controllers-HomeController.vb (Méthodes d’édition)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

La deuxième action Edit() commence par récupérer l’enregistrement du film à partir de la base de données qui correspond à l’id du film en cours d’édition. La déclaration SUIVANTE de LINQ aux entités saisit le premier enregistrement de base de données qui correspond à une id particulière :

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Ensuite, la méthode TryUpdateModel() est utilisée pour attribuer les valeurs des champs de formulaires HTML aux propriétés de l’entité cinématographique. Notez qu’une liste blanche est fournie pour spécifier les propriétés exactes à mettre à jour.

Ensuite, une simple validation est effectuée pour vérifier que les propriétés du titre du film et du réalisateur ont des valeurs. Si l’une ou l’autre propriété manque une valeur, alors un message d’erreur de validation est ajouté à ModelState et ModelState.IsValid retourne la valeur fausse.

Enfin, s’il n’y a pas d’erreurs de validation, le tableau de base de données Movies sous-jacent est mis à jour avec des modifications en appelant la méthode SaveChanges().

Lors de l’édition des enregistrements de base de données, vous devez passer l’id de l’enregistrement en cours de modification à l’action du contrôleur qui effectue la mise à jour de base de données. Dans le cas contraire, l’action du contrôleur ne saura pas quel enregistrement mettre à jour dans la base de données sous-jacente. La vue Edit, contenue dans la liste 5, comprend un champ de formulaire caché qui représente l’id de l’enregistrement de base de données en cours de modification.

**Liste 5 - Views-Home-Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Suppression des dossiers de base de données avec le Cadre d’entité

L’opération de base de données finale, que nous devons aborder dans ce tutoriel, est la suppression des enregistrements de base de données. Vous pouvez utiliser l’action du contrôleur dans la liste 6 pour supprimer un enregistrement de base de données particulier.

**Liste 6 -- 'Controllers’HomeController.vb (Supprimer l’action)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

L’action Supprimer() récupère d’abord l’entité Movie qui correspond à l’Id passé à l’action. Ensuite, le film est supprimé de la base de données en appelant la méthode DeleteObject() suivie par la méthode SaveChanges(). Enfin, l’utilisateur est redirigé vers la vue Index.

## <a name="summary"></a>Récapitulatif

Le but de ce tutoriel était de démontrer comment vous pouvez construire des applications Web axées sur les bases de données en profitant de ASP.NET MVC et le cadre d’entité Microsoft. Vous avez appris à créer une application qui vous permet de sélectionner, d’insérer, de mettre à jour et de supprimer les enregistrements de base de données.

Tout d’abord, nous avons discuté de la façon dont vous pouvez utiliser l’Assistant modèle de données d’entité pour générer un modèle de données d’entité à partir de Visual Studio. Ensuite, vous apprenez à utiliser LINQ pour les entités pour récupérer un ensemble d’enregistrements de base de données à partir d’une table de base de données. Enfin, nous avons utilisé le cadre d’entité pour insérer, mettre à jour et supprimer les enregistrements de base de données.

> [!div class="step-by-step"]
> [Suivant précédent](validation-with-the-data-annotation-validators-cs.md)
> [Next](creating-model-classes-with-linq-to-sql-vb.md)
