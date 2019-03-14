---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Création de Classes de modèle avec Entity Framework (c#) | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous allez apprendre à utiliser ASP.NET MVC avec Entity Framework de Microsoft. Vous allez apprendre à utiliser l’Assistant pour créer un Da d’entité ADO.NET...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: d1cf97a7f1dc9bae2774518cdfc13da48fc7ada2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043076"
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Création de classes de modèle avec Entity Framework (C#)
====================
by [Microsoft](https://github.com/microsoft)

> Dans ce didacticiel, vous allez apprendre à utiliser ASP.NET MVC avec Entity Framework de Microsoft. Vous allez apprendre à utiliser l’Assistant pour créer un ADO.NET Entity Data Model. Au cours de ce didacticiel, nous créons une application web qui montre comment sélectionner, insérer, mettre à jour et supprimer des données de base de données à l’aide d’Entity Framework.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer des classes d’accès aux données à l’aide de Microsoft Entity Framework lors de la création d’une application ASP.NET MVC. Ce didacticiel ne suppose aucune connaissance d’Entity Framework de Microsoft. À la fin de ce didacticiel, vous comprendrez comment utiliser Entity Framework pour sélectionner, insérer, mettre à jour et supprimer des enregistrements de base de données.

Microsoft Entity Framework est un outil de mappage relationnel objet (ORM) qui vous permet de générer une couche d’accès aux données à partir d’une base de données automatiquement. Entity Framework vous permet d’éviter le travail fastidieux de la création de vos classes d’accès aux données à la main.

Afin d’illustrer comment vous pouvez utiliser Microsoft Entity Framework avec ASP.NET MVC, nous allons créer un exemple simple d’application. Nous allons créer une application de base de données de film qui vous permet d’afficher et modifier des enregistrements de base de données de film.

Ce didacticiel suppose que vous disposez de Visual Studio 2008 ou Visual Web Developer 2008 avec Service Pack 1. Vous avez besoin de Service Pack 1 pour utiliser Entity Framework. Vous pouvez télécharger Visual Studio 2008 Service Pack 1 ou Visual Web Developer avec Service Pack 1 à partir de l’adresse suivante :

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> Il n’existe aucune connexion essentielle entre ASP.NET MVC et Entity Framework de Microsoft. Il existe plusieurs alternatives à Entity Framework que vous pouvez utiliser avec ASP.NET MVC. Par exemple, vous pouvez créer vos classes de modèle MVC à l’aide d’autres outils ORM tel que Microsoft LINQ to SQL, NHibernate ou SubSonic.


## <a name="creating-the-movie-sample-database"></a>Création de la base de données exemple Movie

L’application de base de données de films utilise une table de base de données nommée films qui contient les colonnes suivantes :

| Nom de la colonne | Type de données | Autoriser les valeurs null ? | Est la clé primaire ? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titre | nvarchar(100) | False | False |
| Directeur | nvarchar(100) | False | False |

Vous pouvez ajouter cette table à un projet ASP.NET MVC en suivant ces étapes :

1. Avec le bouton droit de l’application\_dossier de données dans la fenêtre Explorateur de solutions, puis sélectionnez l’option de menu **ajouter, nouvel élément.**
2. À partir de la **ajouter un nouvel élément** boîte de dialogue, sélectionnez **base de données SQL Server**, nommez la base de données MoviesDB.mdf, puis cliquez sur le **ajouter** bouton.
3. Double-cliquez sur le fichier MoviesDB.mdf pour ouvrir la fenêtre Server Explorer/Database Explorer.
4. Développez la connexion de base de données MoviesDB.mdf, cliquez sur le dossier Tables, puis sélectionnez l’option de menu **ajouter une nouvelle Table**.
5. Dans le Concepteur de tables, ajoutez les colonnes Id, Title et directeur.
6. Cliquez sur le **enregistrer** bouton (elle a l’icône de disquette) pour enregistrer la nouvelle table avec les films de nom.

Après avoir créé la table de base de données de films, vous devez ajouter des exemples de données à la table. Avec le bouton droit de la table de films et sélectionnez l’option de menu **afficher les données de Table**. Vous pouvez entrer des données de film factice dans la grille qui s’affiche.

## <a name="creating-the-adonet-entity-data-model"></a>Création de l’ADO.NET Entity Data Model

Pour pouvoir utiliser Entity Framework, vous devez créer un Entity Data Model. Vous pouvez tirer parti de Visual Studio *Assistant Entity Data Model* pour générer un Entity Data Model à partir d’une base de données automatiquement.

Procédez comme suit :

1. Cliquez sur le dossier de modèles dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **ajouter, nouvel élément**.
2. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez la catégorie de données (voir Figure 1).
3. Sélectionnez le **ADO.NET Entity Data Model** modèle, nommez l’Entity Data Model MoviesDBModel.edmx, puis cliquez sur le **ajouter** bouton. En cliquant sur le **ajouter** bouton lance l’Assistant de modèle de données.
4. Dans le **choisir le contenu du modèle** étape, choisissez le **générer à partir d’une base de données** , cliquez sur le **suivant** bouton (voir Figure 2).
5. Dans le **choisir votre connexion de données** étape, sélectionnez la connexion de base de données MoviesDB.mdf, entrez les entités de paramètres de connexion nom MoviesDBEntities, puis cliquez sur le **suivant** bouton (voir Figure 3).
6. Dans le **choisir vos objets de base de données** étape, sélectionnez la table de base de données de film, puis cliquez sur le **Terminer** bouton (voir Figure 4).

Après avoir effectué ces étapes, ADO.NET Entity Data Model Designer (Concepteur d’entités) s’ouvre.

**Figure 1 : création d’un nouveau modèle de données d’entité**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Figure 2 – Choisissez l’étape de contenu de modèle**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Figure 3 : choisir votre connexion de données**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Figure 4 : choisir vos objets de base de données**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modification de l’ADO.NET Entity Data Model

Après avoir créé un Entity Data Model, vous pouvez modifier le modèle en tirant parti du Concepteur d’entités (voir Figure 5). Vous pouvez ouvrir le Concepteur d’entités à tout moment en double-cliquant sur le fichier MoviesDBModel.edmx contenu dans le dossier de modèles dans la fenêtre Explorateur de solutions.

**Figure 5 – l’ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Par exemple, vous pouvez utiliser le Concepteur d’entités pour modifier les noms des classes qui génère de l’Assistant modèle. L’Assistant a créé une nouvelle classe d’accès aux données nommée films. En d’autres termes, l’Assistant a fourni la classe très identique à celui de la table de base de données. Étant donné que nous allons utiliser cette classe pour représenter une instance particulière de film, nous devons renommer la classe à partir de films à film.

Si vous souhaitez renommer une classe d’entité, vous pouvez double-cliquer sur le nom de classe dans le Concepteur d’entités et entrez un nouveau nom (voir Figure 6). Vous pouvez également modifier le nom d’une entité dans la fenêtre Propriétés après avoir sélectionné une entité dans le Concepteur d’entités.

**Figure 6 : modification d’un nom d’entité**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Pensez à enregistrer votre Entity Data Model après avoir apporté une modification en cliquant sur le bouton Enregistrer (l’icône de la disquette). Dans les coulisses, le Concepteur d’entités génère un ensemble de classes c#. Vous pouvez afficher ces classes en ouvrant le fichier MoviesDBModel.Designer.cs à partir de la fenêtre de l’Explorateur de solutions.


Ne modifiez pas le code dans le fichier Designer.cs dans la mesure où vos modifications seront remplacées la prochaine fois que vous utiliserez le Concepteur d’entités. Si vous souhaitez étendre les fonctionnalités des classes d’entité défini dans le fichier Designer.cs, vous pouvez créer *des classes partielles* dans des fichiers distincts.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Sélectionner des enregistrements de base de données avec Entity Framework

Nous allons commencer à créer notre application de base de données de films en créant une page qui affiche une liste des enregistrements de film. Le contrôleur Home dans le Listing 1 expose une action nommée Index(). L’action Index() retourne tous les enregistrements de film à partir de la table de base de données de films en tirant parti d’Entity Framework.

**Liste 1 – Controllers\HomeController.cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Notez que le contrôleur dans le Listing 1 inclut un constructeur. Le constructeur initialise un champ de niveau classe nommé \_db. Le \_champ de base de données représente les entités de base de données générées par Entity Framework de Microsoft. Le \_champ de base de données est une instance de la classe MoviesDBEntities qui a été générée par le Concepteur d’entités.


Pour pouvoir utiliser theMoviesDBEntities classe dans le contrôleur Home, vous devez importer l’espace de noms MovieEntityApp.Models (*MVCProjectName*. Modèles).


Le \_champ de base de données est utilisé au sein de l’action Index() pour récupérer les enregistrements de la table de base de données de films. L’expression \_db. MovieSet représente tous les enregistrements de la table de base de données de films. La méthode ToList() est utilisée pour convertir le jeu de films en une collection générique d’objets de film (liste&lt;film&gt;).

Les enregistrements de film sont récupérés à l’aide de LINQ to Entities. L’action Index() dans le Listing 1 utilise LINQ *syntaxe de méthode* pour récupérer le jeu d’enregistrements de base de données. Si vous préférez, vous pouvez utiliser LINQ *syntaxe de requête* à la place. Les deux instructions suivantes font la même chose :

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Utiliser quelle que soit la syntaxe LINQ, syntaxe de méthode ou de la syntaxe de requête, que vous trouvez plus intuitive. Il n’existe aucune différence de performances entre les deux approches : la seule différence est le style.

La vue dans la liste 2 est utilisée pour afficher les enregistrements de film.

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

La vue dans la liste 2 contient un **foreach** boucle qui effectue une itération dans chaque enregistrement de film et affiche les valeurs des propriétés du titre et le directeur de l’enregistrement de film. Notez qu’un lien Edit et Delete s’affiche en regard de chaque enregistrement. En outre, un lien Ajouter un film apparaît au bas de la vue (voir la Figure 7).

**Figure 7 – la vue Index**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

La vue de l’Index est un *tapé vue*. La vue Index inclut une &lt;% @ Page %&gt; directive avec un *Inherits* attribut qui effectue un cast de la propriété de modèle à une collection de liste générique fortement typée d’objets de film (liste&lt;film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insertion d’enregistrements de base de données avec Entity Framework

Vous pouvez utiliser Entity Framework pour faciliter l’insérer de nouveaux enregistrements dans une table de base de données. Liste 3 contient deux nouvelles actions ajoutées à la classe de contrôleur d’accueil que vous pouvez utiliser pour insérer de nouveaux enregistrements dans la table de base de données de film.

**Liste 3 – Controllers\HomeController.cs (ajouter des méthodes)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

La première action Add() retourne simplement une vue. La vue contient un formulaire permettant d’ajouter une nouvelle base de données de film enregistrer (voir Figure 8). Lorsque vous envoyez le formulaire, la deuxième action Add() est appelée.

Notez que la seconde action Add() est décorée avec l’attribut AcceptVerbs. Cette action peut être appelée uniquement lorsque vous effectuez une opération HTTP POST. En d’autres termes, cette action ne peut être appelée lors de la validation d’un formulaire HTML.

La deuxième action Add() crée une nouvelle instance de la classe Entity Framework film à l’aide de la méthode TryUpdateModel() de MVC ASP.NET. La méthode TryUpdateModel() prend les champs dans la passé à la méthode Add() de FormCollection et assigne les valeurs de ces champs de formulaire HTML à la classe Movie.


Lorsque vous utilisez Entity Framework, vous devez fournir une « liste verte » des propriétés lorsque vous utilisez les méthodes TryUpdateModel ou UpdateModel pour mettre à jour les propriétés d’une classe d’entité.


Ensuite, l’action Add() effectue une validation de formulaire simple. L’action vérifie que le titre et le directeur de propriétés ont des valeurs. S’il existe une erreur de validation, un message d’erreur de validation est ajouté à ModelState.

Si aucune erreur de validation un nouvel enregistrement de film est ajouté à la table de base de données de films à l’aide d’Entity Framework. Le nouvel enregistrement est ajouté à la base de données avec les deux lignes de code suivantes :

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

La première ligne de code ajoute la nouvelle entité de film à l’ensemble de films suivis par Entity Framework. La deuxième ligne de code enregistre toutes les modifications ont été apportées pour les films en cours de suivi dans la base de données sous-jacente.

**Figure 8 : la vue de l’ajouter**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Mise à jour des enregistrements de base de données avec Entity Framework

Vous pouvez suivre presque la même approche pour modifier un enregistrement de base de données avec Entity Framework en tant que l’approche que nous avons simplement suivies pour insérer un nouvel enregistrement de base de données. Liste 4 contient deux nouvelles actions de contrôleur nommées Edit(). La première action Edit() retourne un formulaire HTML pour modifier un enregistrement de film. La deuxième action Edit() tente de mettre à jour de la base de données.

**Liste 4 – Controllers\HomeController.cs (méthodes de modification)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

La deuxième action Edit() commence par récupérer l’enregistrement de film à partir de la base de données qui correspond à l’Id de la séquence en cours de modification. LINQ to instruction d’entités suivants extrait le premier enregistrement de base de données qui correspond à un Id spécifique :

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Ensuite, la méthode TryUpdateModel() est utilisée pour affecter les valeurs des champs de formulaire HTML pour les propriétés de l’entité de film. Notez qu’une liste verte est fournie pour spécifier les propriétés exactes à mettre à jour.

Ensuite, une validation simple est effectuée pour vérifier que le titre du film et le directeur des propriétés ont des valeurs. Si une valeur sont manquante dans des propriétés, puis un message d’erreur de validation est ajouté à ModelState et ModelState.IsValid retourne la valeur false.

Enfin, s’il n’existe aucune erreur de validation, puis la table de base de données de films sous-jacent est mis à jour avec toutes les modifications en appelant la méthode SaveChanges().

Lors de la modification des enregistrements de base de données, vous devez transmettre l’Id de l’enregistrement en cours de modification à l’action de contrôleur qui effectue la mise à jour de la base de données. Sinon, l’action du contrôleur ne sait pas quel enregistrement il doit mettre à jour dans la base de données sous-jacente. La vue Edit, contenue dans la liste 5, inclut un champ de formulaire masqué qui représente l’Id de l’enregistrement de base de données en cours de modification.

**Liste 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Suppression d’enregistrements de base de données avec Entity Framework

L’opération de base de données finale, nous devons aborder dans ce didacticiel, supprime les enregistrements de base de données. Vous pouvez utiliser l’action du contrôleur dans la liste 6 pour supprimer un enregistrement de base de données particulière.

**Liste 6--\Controllers\HomeController.cs (action de suppression)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

L’action Delete() récupère d’abord le film entité qui correspond à l’Id est transmise à l’action. Ensuite, le film est supprimé à partir de la base de données en appelant la méthode DeleteObject() suivie par la méthode SaveChanges(). Enfin, l’utilisateur est redirigé vers la vue Index.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été pour illustrer comment vous pouvez créer des applications web orientées sur la base de données en tirant parti d’ASP.NET MVC et Entity Framework de Microsoft. Vous avez appris à créer une application qui vous permet de sélectionner, insérer, mettre à jour et supprimer des enregistrements de base de données.

Tout d’abord, nous avons abordé la façon dont vous pouvez utiliser l’Assistant Entity Data Model pour générer un Entity Data Model à partir de Visual Studio. Ensuite, vous allez apprendre à utiliser LINQ to Entities pour récupérer un jeu d’enregistrements de base de données à partir d’une table de base de données. Enfin, nous avons utilisé l’Entity Framework pour insérer, mettre à jour et supprimer des enregistrements de base de données.

> [!div class="step-by-step"]
> [Next](creating-model-classes-with-linq-to-sql-cs.md)
