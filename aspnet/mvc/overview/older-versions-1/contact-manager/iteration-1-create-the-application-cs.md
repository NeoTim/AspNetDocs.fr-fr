---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Itération #1 - Créer l’application (C) Microsoft Docs'
author: rick-anderson
description: 'Dans la première itération, nous créons le Gestionnaire de Contact de la manière la plus simple possible. Nous ajoutons un support pour les opérations de base de base de base: Créer, Lire, Mettre à jour, et D ...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: ecc3c1201c784e20c6b2601735bee3d4ce721f22
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542545"
---
# <a name="iteration-1--create-the-application-c"></a>Itération #1 : Créer l’application (C#)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> Dans la première itération, nous créons le Gestionnaire de Contact de la manière la plus simple possible. Nous ajoutons un support pour les opérations de base de base de base : Créer, lire, mettre à jour et supprimer (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Construire une application de gestion des contacts ASP.NET MVC (VB)

Dans cette série de tutoriels, nous construisons toute une application de gestion de contact du début à la fin. L’application Contact Manager vous permet de stocker les coordonnées - noms, numéros de téléphone et adresses e-mail - pour une liste de personnes.

Nous construisons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. Le but de cette approche d’itération multiple est de vous permettre de comprendre la raison de chaque changement.

- Itération #1 - Créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la manière la plus simple possible. Nous ajoutons un support pour les opérations de base de base de base : Créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 - Rendre l’application belle. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page principale de vue par défaut ASP.NET MVC et la feuille de style en cascade.

- Itération #3 - Ajouter la validation du formulaire. Dans la troisième itération, nous ajoutons la validation de forme de base. Nous empêchons les gens de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses e-mail et les numéros de téléphone.

- Itération #4 - Rendre l’application lâchement couplée. Dans cette quatrième itération, nous profitons de plusieurs modèles de conception logicielle pour faciliter le maintien et la modification de l’application Contact Manager. Par exemple, nous refactorrons notre application pour utiliser le modèle de dépôt et le modèle d’injection de dépendance.

- Itération #5 - Créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à maintenir et à modifier en ajoutant des tests unitaires. Nous nous moquons de nos classes de modèles de données et construisons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - Utilisez le développement axé sur les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code contre les tests unitaires. Dans cette itération, nous ajoutons des groupes de contact.

- Itération #7 - Ajouter la fonctionnalité Ajax. Dans la septième itération, nous améliorons la réactivité et la performance de notre application en ajoutant un soutien pour Ajax.

## <a name="this-iteration"></a>Cette Itération

Dans cette première itération, nous construisons l’application de base. L’objectif est de construire le Gestionnaire de contact de la manière la plus rapide et la plus simple possible. Dans les itérations ultérieures, nous améliorons la conception de l’application.

L’application Contact Manager est une application de base pilotée par la base de données. Vous pouvez utiliser l’application pour créer de nouveaux contacts, modifier les contacts existants et supprimer les contacts.

Dans cette itération, nous complétons les étapes suivantes :

1. application MVC ASP.NET
2. Créer une base de données pour stocker nos contacts
3. Générer une classe modèle pour notre base de données avec le cadre d’entité Microsoft
4. Créer une action et une vue de contrôleur qui nous permettent d’énumérer tous les contacts de la base de données
5. Créer des actions de contrôleur et une vue qui nous permet de créer un nouveau contact dans la base de données
6. Créer des actions de contrôleur et une vue qui nous permet de modifier un contact existant dans la base de données
7. Créez des actions de contrôleur et une vue qui nous permet de supprimer un contact existant dans la base de données

## <a name="software-prerequisites"></a>Composants logiciels requis

Dans ASP.NET applications MVC, vous devez avoir soit Visual Studio 2008 ou Visual Web Developer 2008 installé sur votre ordinateur (Visual Web Developer est une version gratuite de Visual Studio qui n’inclut pas toutes les fonctionnalités avancées de Visual Studio). Vous pouvez télécharger la version d’essai de Visual Studio 2008 ou Visual Web Developer à partir de l’adresse suivante :

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Pour ASP.NET applications MVC avec Visual Web Developer, vous devez avoir Visual Web Developer Service Pack 1 installé. Sans Service Pack 1, vous ne pouvez pas créer de projets d’applications Web.

ASP.NET cadre MVC. Vous pouvez télécharger le cadre MVC ASP.NET à partir de l’adresse suivante :

[https://www.asp.net/mvc](../../../index.md)

Dans ce tutoriel, nous utilisons le cadre d’entité Microsoft pour accéder à une base de données. Le cadre d’entité est inclus avec .NET Framework 3.5 Service Pack 1. Vous pouvez télécharger ce pack de service à partir de l’emplacement suivant :

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;dsplaylang-en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Comme alternative à l’exécution de chacun de ces téléchargements un par un, vous pouvez profiter de l’installateur de plate-forme Web (Web PI). Vous pouvez télécharger l’IP Web à partir de l’adresse suivante :

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>projet MVC ASP.NET

ASP.NET projet d’application Web MVC. Lancez Visual Studio et sélectionnez l’option **menu File, New Project**. Le dialogue **du nouveau projet** apparaît (voir la figure 1). Sélectionnez le type de projet **Web** et le **modèle d’application Web ASP.NET MVC.** Nommez votre nouveau projet *ContactManager* et cliquez sur le bouton OK.

Assurez-vous que vous avez .NET Framework 3.5 sélectionné parmi la liste de décrochage en haut à droite du dialogue **du nouveau projet.** Dans le cas contraire, le modèle d’application Web ASP.NET MVC n’apparaîtra pas.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figure 01**: Le dialogue du nouveau projet[(Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image2.png))

ASP.NET’application MVC, le dialogue **Create Unit Test Project** apparaît. Vous pouvez utiliser ce dialogue pour indiquer que vous souhaitez créer et ajouter un projet de test unitaire à votre solution lorsque vous créez votre application ASP.NET MVC. Bien que nous ne serons pas la construction de tests d’unité dans cette itération, vous devez sélectionner l’option **Oui, créer un projet de test unitaire** parce que nous prévoyons d’ajouter des tests unitaires dans une itération ultérieure. L’ajout d’un projet de test lorsque vous créez pour la première fois un nouveau projet ASP.NET MVC est beaucoup plus facile que d’ajouter un projet de test après la création du projet ASP.NET MVC.

> [!NOTE] 
> 
> Étant donné que Visual Web Developer ne prend pas en charge les projets de test, vous n’obtenez pas le dialogue Create Unit Test Project lorsque vous utilisez Visual Web Developer.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figure 02**: Le dialogue create Unit Test Project ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image4.png))

ASP.NET’application MVC apparaît dans la fenêtre Visual Studio Solution Explorer (voir la figure 3). Si vous ne voyez pas la fenêtre Solution Explorer, vous pouvez ouvrir cette fenêtre en sélectionnant l’option de menu **View, Solution Explorer**. Notez que la solution contient deux projets : le projet ASP.NET MVC et le projet Test. Le projet ASP.NET MVC s’appelle ContactManager et le projet Test s’appelle ContactManager.Tests.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figure 03**: La fenêtre Solution Explorer ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Suppression des fichiers d’échantillons de projet

Le modèle de projet ASP.NET MVC comprend des exemples de fichiers pour les contrôleurs et les vues. Avant de créer une nouvelle application ASP.NET MVC, vous devez supprimer ces fichiers. Vous pouvez supprimer des fichiers et des dossiers dans la fenêtre Solution Explorer en cliquant à droite sur un fichier ou un dossier et en sélectionnant l’option de menu **Supprimer**.

Vous devez supprimer les fichiers suivants du projet ASP.NET MVC :

- -Controllers-HomeController.cs

- "Views-Home".aspx

- 'Views’Home’Index.aspx

Et, vous devez supprimer le fichier suivant du projet de test:

-Controllers-HomeControllerTest.cs

## <a name="creating-the-database"></a>Création de la base de données

L’application Contact Manager est une application Web axée sur la base de données. Nous utilisons une base de données pour stocker les coordonnées.

Le cadre MVC ASP.NET avec n’importe quelle base de données moderne comprenant les bases de données Microsoft SQL Server, Oracle, MySQL et IBM DB2. Dans ce tutoriel, nous utilisons une base de données Microsoft SQL Server. Lorsque vous installez Visual Studio, vous avez la possibilité d’installer Microsoft SQL Server Express qui est une version gratuite de la base de données Microsoft SQL Server.

Créez une nouvelle base de données\_en cliquant à droite sur le dossier App Data dans la fenêtre Solution Explorer et en sélectionnant l’option menu **Ajouter, Nouvel article**. Dans le dialogue **Add New Item,** sélectionnez la catégorie **Données** et le modèle de base de données de **serveurs SQL** (voir la figure 4). Nommez la nouvelle base de données ContactManagerDB.mdf et cliquez sur le bouton OK.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figure 04**: Création d’une nouvelle base de données Microsoft SQL Server Express ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image8.png))

Après avoir créé la nouvelle base de\_données, la base de données apparaît dans le dossier App Data dans la fenêtre Solution Explorer. Double-cliquez sur le fichier ContactManager.mdf pour ouvrir la fenêtre Server Explorer et se connecter à la base de données.

> [!NOTE] 
> 
> La fenêtre Server Explorer s’appelle la fenêtre Database Explorer dans le cas de Microsoft Visual Web Developer.

Vous pouvez utiliser la fenêtre Server Explorer pour créer de nouveaux objets de base de données tels que des tables de base de données, des vues, des déclencheurs et des procédures stockées. Cliquez à droite sur le dossier Tables et sélectionnez l’option menu **Ajouter une nouvelle table**. Le concepteur de la table de base de données apparaît (voir la figure 5).

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figure 05**: The Database Table Designer ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image10.png))

Nous devons créer une table qui contient les colonnes suivantes :

<a id="0.1_table01"></a>

| **Nom de colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Téléphone | nvarchar(50) | false |
| E-mail | nvarchar(255) | false |

La première colonne, la colonne Id, est spéciale. Vous devez marquer la colonne Id comme une colonne d’identité et une colonne Clé Primaire. Vous indiquez qu’une colonne est une colonne d’identité en élargissant les propriétés de colonne (regardez le bas de la figure 6) et en faisant défiler vers le bas à la propriété de spécification d’identité. Définissez la propriété **(Est-identité)** à la valeur **Oui**.

Vous marquez une colonne comme une colonne Clé Primaire en sélectionnant la colonne et en cliquant sur le bouton avec l’icône d’une clé. Une fois qu’une colonne est marquée comme une colonne clé primaire, une icône d’une clé apparaît à côté de la colonne (voir la figure 6).

Une fois que vous avez terminé la création de la table, cliquez sur le bouton Enregistrer (le bouton avec une icône d’une disquette) pour enregistrer la nouvelle table. Donnez à votre nouvelle table le nom *Contacts*.

Après avoir terminé la création de la table de base de données Contacts, vous devez ajouter quelques enregistrements à la table. Cliquez à droite sur la table Contacts dans la fenêtre Server Explorer et sélectionnez l’option menu **Afficher les données de table**. Entrez un ou plusieurs contacts dans la grille qui apparaît.

## <a name="creating-the-data-model"></a>Création du modèle de données

L’application ASP.NET MVC se compose de modèles, de vues et de contrôleurs. Nous commençons par créer une classe modèle qui représente la table Contacts que nous avons créée dans la section précédente.

Dans ce tutoriel, nous utilisons le cadre d’entité Microsoft pour générer une classe de modèle à partir de la base de données automatiquement.

> [!NOTE] 
> 
> Le cadre MVC ASP.NET n’est en aucun cas lié au cadre d’entité Microsoft. Vous pouvez utiliser ASP.NET MVC avec d’autres technologies d’accès aux bases de données, y compris NHibernate, LINQ à SQL, ou ADO.NET.

Suivez ces étapes pour créer les classes de modèles de données :

1. Cliquez à droite sur le dossier Modèles dans la fenêtre Solution Explorer et **sélectionnez Ajouter, Nouvel article**. Le dialogue **Add New Item** apparaît (voir la figure 6).
2. Sélectionnez la catégorie **de données** et le modèle ADO.NET modèle de modèle de **données d’entité.** Nommez votre modèle de données *ContactManagerModel.edmx* et cliquez sur le bouton **Ajouter.** L’assistant du modèle de données d’entité apparaît (voir la figure 7).
3. Dans l’étape Choisissez les **contenus modèles,** **sélectionnez Générer dans la base de données** (voir la figure 7).
4. Dans l’étape **Choisissez votre connexion de données,** sélectionnez la base de données ContactManagerDB.mdf et entrez le nom *ContactManagerDBEntities* pour les paramètres de connexion d’entité (voir la figure 8).
5. Dans l’étape **Choisissez vos objets de base de données,** sélectionnez les tables étiquetées case à cocher (voir la figure 9). Le modèle de données inclura toutes les tables contenues dans votre base de données (il n’y en a qu’une, la table Contacts). Entrez les *modèles*namespace . Cliquez sur le bouton Finition pour compléter l’assistant.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figure 06**: Le dialogue Add New Item[(Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image12.png))

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figure 07**: Choisissez le contenu du modèle ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image14.png))

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figure 08**: Choisissez votre connexion de données ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image16.png))

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figure 09**: Choisissez vos objets de base de données ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image18.png))

Après avoir terminé l’Assistant de modèle de données d’entité, le concepteur de modèle de données d’entité apparaît. Le concepteur affiche une classe qui correspond à chaque table modélisé. Vous devriez voir une classe nommée Contacts.

L’assistant du modèle de données d’entité génère des noms de classe basés sur les noms de table de base de données. Vous avez presque toujours besoin de changer le nom de la classe générée par l’assistant. Cliquez à droite sur la classe Contacts dans le concepteur et sélectionnez l’option de menu **Renommer**. Changez le nom de la classe de Contacts (pluriel) à Contact (singulier). Après avoir changé le nom de la classe, la classe doit apparaître comme la figure 10.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figure 10**: La classe De contact ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image20.png))

À ce stade, nous avons créé notre modèle de base de données. Nous pouvons utiliser la classe Contact pour représenter un enregistrement de contact particulier dans notre base de données.

## <a name="creating-the-home-controller"></a>Création du contrôleur à domicile

L’étape suivante consiste à créer notre contrôleur Home. Le contrôleur à domicile est le contrôleur par défaut invoqué dans une application ASP.NET MVC.

Créez la classe de contrôleur d’accueil en cliquant à droite sur le dossier Controllers dans la fenêtre Solution Explorer et en sélectionnant l’option de menu **Ajouter, Contrôleur** (voir la figure 11). Notez la case à cocher étiquetée **Ajouter des méthodes d’action pour créer, mettre à jour et détails scénarios**. Assurez-vous que cette case à cocher est cochée avant de cliquer sur le bouton **Ajouter.**

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figure 11**: Ajout du contrôleur à domicile ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image22.png))

Lorsque vous créez le contrôleur à domicile, vous obtenez la classe dans la liste 1.

**Liste 1 - Controllers-HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Liste des contacts

Afin d’afficher les enregistrements dans le tableau de base de données Contacts, nous devons créer une action Index() et une vue d’index.

Le contrôleur à domicile contient déjà une action Index() . Nous devons modifier cette méthode afin qu’elle ressemble à la liste 2.

**Liste 2 - Controllers-HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Notez que la classe de contrôleur d’accueil dans la liste 2 contient un champ privé nommé \_entités. Le \_champ des entités représente les entités du modèle de données. Nous utilisons \_le champ des entités pour communiquer avec la base de données.

La méthode Index() renvoie une vue qui représente tous les contacts à partir du tableau de base de données Contacts. Les \_entités d’expression. ContactSet.ToList() renvoie la liste des contacts comme liste générique.

Maintenant que nous avons créé le contrôleur d’index, nous devons ensuite créer la vue Index. Avant de créer la vue Index, compilez votre application en sélectionnant l’option de menu **Construire, Construire la solution**. Vous devez toujours compiler votre projet avant d’ajouter une vue afin que la liste des classes modèles soit affichée dans le dialogue **Add View.**

Vous créez la vue de l’index en cliquant à droite sur la méthode Index() et en sélectionnant l’option de menu **Add View** (voir figure 12). La sélection de cette option de menu ouvre le dialogue **Add View** (voir la figure 13).

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figure 12**: Ajout de la vue de l’index ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image24.png))

Dans le dialogue **Add View,** vérifiez la case à cocher étiquetée **Créer une vue fortement typée**. Sélectionnez la classe de données View ContactManager.Models.Contact et la liste de contenu View. La sélection de ces options génère une vue qui affiche une liste d’enregistrements de contact.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figure 13**: Le dialogue Add View ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image26.png))

Lorsque vous cliquez sur le bouton **Ajouter,** la vue d’index dans la liste 3 est générée. Notez &lt;la directive&gt; % page % qui apparaît en haut du fichier. La vue Index hérite&lt;de la classe&lt;ViewPage IEnumerable ContactManager.Models.Contact.&gt; &gt; En d’autres termes, la classe modèle dans la vue représente une liste des entités de contact.

Le corps de la vue Index contient une boucle de foreach qui itère à travers chacun des contacts représentés par la classe Modèle. La valeur de chaque propriété de la classe Contact s’affiche dans une table HTML.

**Liste 3 - Views-Home-Index.aspx (non modifié)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Nous devons apporter une modification à la vue index. Parce que nous ne créons pas une vue Détails, nous pouvons supprimer le lien Détails. Trouver et supprimer le code suivant de la vue index :

l’article id. Id (en anglais)&gt;

Une fois que vous modifiez la vue index, vous pouvez exécuter l’application Contact Manager. Sélectionnez l’option menu Debug, Démarrer Debugging ou tout simplement appuyer sur F5. La première fois que vous exécutez l’application, vous obtenez le dialogue dans la figure 14. Sélectionnez l’option **Modifier le fichier Web.config pour activer le débogage** et cliquez sur le bouton OK.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figure 14**: Permettre le débogage ([Cliquez pour voir l’image pleine grandeur](iteration-1-create-the-application-cs/_static/image28.png))

La vue Index est retournée par défaut. Cette vue répertorie toutes les données du tableau de base de données Contacts (voir la figure 15).

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figure 15**: La vue de l’index ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image30.png))

Notez que la vue index comprend un lien étiqueté Create New au bas de la vue. Dans la section suivante, vous apprenez à créer de nouveaux contacts.

## <a name="creating-new-contacts"></a>Créer de nouveaux contacts

Pour permettre aux utilisateurs de créer de nouveaux contacts, nous devons ajouter deux actions Create() au contrôleur Home. Nous devons créer une action Créer() qui renvoie un formulaire HTML pour créer un nouveau contact. Nous devons créer une deuxième action Créer() qui effectue l’encart de base de données réel du nouveau contact.

Les nouvelles méthodes De création () que nous devons ajouter au contrôleur à domicile sont contenues dans la liste 4.

**Liste 4 - Controllers-HomeController.cs (avec méthodes de création)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

La première méthode Créer() peut être invoquée avec un HTTP GET tandis que la deuxième méthode Créer() ne peut être invoquée que par un MESSAGE HTTP. En d’autres termes, la deuxième méthode Create() ne peut être invoquée que lors de l’affichage d’un formulaire HTML. La première méthode Create() renvoie simplement une vue qui contient le formulaire HTML pour créer un nouveau contact. La deuxième méthode Create() est beaucoup plus intéressante : elle ajoute le nouveau contact à la base de données.

Notez que la deuxième méthode Créer() a été modifiée pour accepter une instance de la classe De contact. Les valeurs de formulaire affichées à partir du formulaire HTML sont liées à cette classe de contact par le cadre MVC ASP.NET automatiquement. Chaque champ de formulaire à partir du formulaire HTML Create est attribué à une propriété du paramètre De contact.

Notez que le paramètre de contact est décoré d’un attribut [Bind]. L’attribut [Bind] est utilisé pour exclure la propriété Contact Id de la liaison. Parce que la propriété Id représente une propriété Identity, nous ne voulons pas définir la propriété Id.

Dans le corps de la méthode Créer,) le cadre d’entité est utilisé pour insérer le nouveau contact dans la base de données. Le nouveau Contact est ajouté à l’ensemble existant de Contacts et la méthode SaveChanges() est appelée pour repousser ces modifications à la base de données sous-jacente.

Vous pouvez générer un formulaire HTML pour créer de nouveaux contacts en cliquant à droite sur l’une ou l’autre des deux méthodes Create() et en sélectionnant l’option de menu **Add View** (voir la figure 16).

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figure 16**: Ajout de la vue Créer ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image32.png))

Dans le dialogue **Add View,** sélectionnez la classe **ContactManager.Models.Contact** et l’option **Créer** pour le contenu de la vue (voir la figure 17). Lorsque vous cliquez sur le bouton **Ajouter,** une vue Créer est générée automatiquement.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figure 17**: Voir une page exploser ([Cliquez pour voir l’image pleine grandeur](iteration-1-create-the-application-cs/_static/image34.png))

La vue Créer contient des champs de forme pour chacune des propriétés de la classe Contact. Le code de la vue Créer est inclus dans la liste 5.

**Liste 5 - Views-Home-Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Après avoir modifié les méthodes Créer() et ajouter la vue Créer, vous pouvez exécuter l’application Contact Manger et créer de nouveaux contacts. Cliquez sur le nouveau lien **Créer** qui apparaît dans la vue de l’index pour naviguer vers la vue Créer. Vous devriez voir la vue dans la figure 18.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figure 18**: The Create View ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image36.png))

## <a name="editing-contacts"></a>Modifier les contacts

L’ajout de la fonctionnalité pour l’édition d’un enregistrement de contact est très similaire à l’ajout de la fonctionnalité pour la création de nouveaux enregistrements de contact. Tout d’abord, nous devons ajouter deux nouvelles méthodes d’édition à la classe Home Controller. Ces nouvelles méthodes Edit() sont contenues dans la liste 6.

**Liste 6 - Controllers-HomeController.cs (avec méthodes d’édition)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

La première méthode Edit() est invoquée par une opération HTTP GET. Un paramètre Id est transmis à cette méthode qui représente l’id de l’enregistrement de contact en cours de modification. Le cadre de l’entité est utilisé pour récupérer un contact qui correspond à l’Id. Une vue qui contient un formulaire HTML pour l’édition d’un enregistrement est retournée.

La deuxième méthode Edit() effectue la mise à jour réelle de la base de données. Cette méthode accepte une instance de la classe De contact comme paramètre. Le cadre MVC ASP.NET lie automatiquement les champs de formulaires du formulaire Edit à cette classe. Notez que vous n’incluez pas l’attribut [Bind] lors de l’édition d’un contact (nous avons besoin de la valeur de la propriété Id).

Le cadre d’entité est utilisé pour enregistrer le contact modifié à la base de données. Le contact d’origine doit d’abord être récupéré dans la base de données. Ensuite, la méthode Du cadre d’entité appliquepropertyChanges() est appelée pour enregistrer les modifications au Contact. Enfin, la méthode Entity Framework SaveChanges() est appelée à persister dans les modifications apportées à la base de données sous-jacente.

Vous pouvez générer la vue qui contient le formulaire Modifier en cliquant à droite sur la méthode Edit() et en sélectionnant l’option de menu Ajouter La vue. Dans le dialogue Add View, sélectionnez la classe **ContactManager.Models.Contact** et le contenu De la vue **Edit** (voir la figure 19).

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figure 19**: Ajout d’une vue d’édition ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image38.png))

Lorsque vous cliquez sur le bouton Ajouter, une nouvelle vue Edit est générée automatiquement. Le formulaire HTML généré contient des champs qui correspondent à chacune des propriétés de la classe Contact (voir Liste 7).

**Liste 7 - Views-Home-Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Supprimer les contacts

Si vous souhaitez supprimer des contacts, vous devez ajouter deux actions Supprimer () à la classe de contrôleur d’accueil. La première action Supprimer() affiche un formulaire de confirmation de suppression. La deuxième action Supprimer() effectue la suppression réelle.

> [!NOTE] 
> 
> Plus tard, dans Iteration #7, nous modifions le Contact Manager afin qu’il prend en charge une suppression d’une étape Ajax.

Les deux nouvelles méthodes Supprimer() sont contenues dans la liste 8.

**Liste 8 - Controllers-HomeController.cs (Méthodes de suppression)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

La première méthode Supprimer)) renvoie un formulaire de confirmation pour supprimer un enregistrement de contact de la base de données (voir figure20). La deuxième méthode Supprimer() effectue l’opération de suppression réelle par rapport à la base de données. Une fois que le contact d’origine a été récupéré dans la base de données, les méthodes Entity Framework DeleteObject() et SaveChanges() sont appelées à supprimer la base de données.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figure 20**: La vue de confirmation de suppression ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image40.png))

Nous devons modifier la vue de l’index afin qu’elle contienne un lien pour la suppression des enregistrements de contact (voir la figure 21). Vous devez ajouter le code suivant à la même cellule de table qui contient le lien Modifier :

Html.ActionLink (id-article. Id ) %&gt;

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figure 21**: Vue d’index avec le lien Edit ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image42.png))

Ensuite, nous devons créer la vue de confirmation de suppression. Cliquez à droite sur la méthode Supprimer () dans la catégorie contrôleur d’accueil et sélectionnez l’option menu Ajouter La vue. Le dialogue Add View apparaît (voir la figure 22).

Contrairement aux vues Liste, Créer et modifier, le dialogue Add View ne contient pas d’option pour créer une vue supprimer. Choisissez plutôt la classe de données **ContactManager.Models.Contact** et le contenu de vue **vide.** La sélection de l’option de contenu De vue vide nous obligera à créer la vue nous-mêmes.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figure 22**: Ajout de la vue de confirmation de suppression ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image44.png))

Le contenu de la vue Supprimer est contenu dans la liste 9. Ce point de vue contient un formulaire qui confirme si un contact particulier doit être supprimé (voir la figure 21).

**Liste 9 - Views-Home-Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Modification du nom du contrôleur par défaut

Il pourrait vous déranger que le nom de notre classe de contrôleur pour travailler avec des contacts est nommé la classe HomeController. Le contrôleur ne devrait-il pas être nommé ContactController ?

Ce problème est assez facile à résoudre. Tout d’abord, nous devons refactorer le nom du contrôleur à domicile. Ouvrez la classe HomeController dans l’éditeur de code visual studio, cliquez à droite sur le nom de la classe et sélectionnez l’option de menu **Refactor, Renommez.** La sélection de cette option de menu ouvre le dialogue Rename.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figure 23**: Refactoring a controller name ([Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image46.png))

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figure 24**: Utilisation du dialogue Rename[(Cliquez pour voir l’image grandeur nature](iteration-1-create-the-application-cs/_static/image48.png))

Si vous renommez votre classe de contrôleur, Visual Studio mettra à jour le nom du dossier dans le dossier Vues ainsi. Visual Studio renommera le dossier «Views-Home» au dossier « Vues »Contact ».

Une fois que vous avez apporté cette modification, votre application n’aura plus de contrôleur à domicile. Lorsque vous exécutez votre application, vous obtiendrez la page d’erreur dans la figure 25.

[![Boîte de dialogue New Project](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figure 25**: Pas de contrôleur par défaut ([Cliquez pour voir l’image pleine grandeur](iteration-1-create-the-application-cs/_static/image50.png))

Nous devons mettre à jour l’itinéraire par défaut dans le fichier Global.asax pour utiliser le contrôleur de contact au lieu du contrôleur Home. Ouvrez le fichier Global.asax et modifiez le contrôleur par défaut utilisé par l’itinéraire par défaut (voir Liste 10).

**Liste 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Une fois que vous aurez apporté ces modifications, le gestionnaire de contact fonctionnera correctement. Maintenant, il utilisera la classe de contrôleur de contact comme contrôleur par défaut.

## <a name="summary"></a>Récapitulatif

Dans cette première itération, nous avons créé une application de base Contact Manager de la manière la plus rapide possible. Nous avons profité de Visual Studio pour générer le code initial pour nos contrôleurs et vues automatiquement. Nous avons également profité du cadre d’entité pour générer automatiquement nos catégories de modèles de bases de données.

Actuellement, nous pouvons répertorier, créer, modifier et supprimer les enregistrements de contact avec l’application Contact Manager. En d’autres termes, nous pouvons effectuer toutes les opérations de base de base requises par une application Web axée sur la base de données.

Malheureusement, notre application a quelques problèmes. Tout d’abord et j’hésite à l’admettre, l’application Contact Manager n’est pas l’application la plus attrayante. Il a besoin d’un peu de travail de conception. Dans la prochaine itération, nous allons examiner comment nous pouvons changer la page principale de vue par défaut et feuille de style en cascade pour améliorer l’apparence de l’application.

Deuxièmement, nous n’avons mis en œuvre aucune validation de formulaire. Par exemple, rien ne vous empêche de soumettre le formulaire de contact Créer sans entrer des valeurs pour l’un des champs de formulaires. En outre, vous pouvez saisir des numéros de téléphone invalides et des adresses e-mail. Nous commençons à nous attaquer au problème de la validation des formulaires dans l’itération #3.

Enfin, et surtout, l’itération actuelle de l’application Contact Manager ne peut pas être facilement modifiée ou maintenue. Par exemple, la logique d’accès à la base de données est intégrée directement dans les actions du contrôleur. Cela signifie que nous ne pouvons pas modifier notre code d’accès aux données sans modifier nos contrôleurs. Dans les itérations ultérieures, nous explorons des modèles de conception de logiciels que nous pouvons mettre en œuvre pour rendre le gestionnaire de contact plus résistant au changement.

> [!div class="step-by-step"]
> [Suivant](iteration-2-make-the-application-look-nice-cs.md)
