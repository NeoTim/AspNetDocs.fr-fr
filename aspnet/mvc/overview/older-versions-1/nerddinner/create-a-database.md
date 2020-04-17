---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Microsoft Docs
author: rick-anderson
description: L’étape 2 montre les étapes pour créer la base de données contenant toutes les données du dîner et de la RSVP pour notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: d0b87e4a6a27b37d2dbaa6d5b871da477b25586d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541609"
---
# <a name="create-a-database"></a>Créer une base de données

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 2 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 2 montre les étapes pour créer la base de données contenant toutes les données du dîner et de la RSVP pour notre application NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner Étape 2: Création de la base de données

Nous utiliserons une base de données pour stocker toutes les données Dîner et RSVP pour notre application NerdDinner.

Les étapes ci-dessous montrent la création de la base de données à l’aide de l’édition gratuite SQL Server Express (que vous pouvez facilement installer en utilisant V2 de [l’installateur de plate-forme Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Tout le code que nous écrirons fonctionne avec SQL Server Express et le serveur SQL complet.

### <a name="creating-a-new-sql-server-express-database"></a>Création d’une nouvelle base de données SQL Server Express

Nous commencerons par cliquer à droite sur notre projet web, puis sélectionnerons la commande de menu **Add-&gt;New Item** :

![](create-a-database/_static/image1.png)

Cela fera ressortir le dialogue "Add New Item" de Visual Studio. Nous filtrerons par la catégorie «Données» et sélectionnerons le modèle d’élément « Base de données de serveurs SQL » :

![](create-a-database/_static/image2.png)

Nous allons nommer la base de données SQL Server Express que nous voulons créer "NerdDinner.mdf" et frapper ok. Visual Studio nous demandera alors si nous voulons\_ajouter ce fichier à notre répertoire de données d’application (qui est un répertoire déjà configuré avec des ACL de sécurité de lecture et d’écriture) :

![](create-a-database/_static/image3.png)

Nous cliquerons sur "Oui" et notre nouvelle base de données sera créée et ajoutée à notre Solution Explorer:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Création de tables dans notre base de données

Nous avons maintenant une nouvelle base de données vide. Ajoutons quelques tables.

Pour ce faire, nous naviguerons vers la fenêtre d’onglet "Server Explorer" dans Visual Studio, ce qui nous permet de gérer les bases de données et les serveurs. Les bases de données SQL\_Server Express stockées dans le dossier de données app de notre application s’afficheront automatiquement dans le Server Explorer. Nous pouvons utiliser l’icône "Connect to Database" en haut de la fenêtre "Server Explorer" pour ajouter des bases de données SQL Server supplémentaires (locales et éloignées) à la liste :

![](create-a-database/_static/image5.png)

Nous ajouterons deux tables à notre base de données NerdDinner, l’une pour stocker nos dîners et l’autre pour suivre les acceptations de RSVP. Nous pouvons créer de nouvelles tables en cliquant à droite sur le dossier "Tables" dans notre base de données et en choisissant la commande de menu "Ajouter une nouvelle table":

![](create-a-database/_static/image6.png)

Cela ouvrira un concepteur de table qui nous permet de configurer le schéma de notre table. Pour notre table "Dîners", nous ajouterons 10 colonnes de données:

![](create-a-database/_static/image7.png)

Nous voulons que la colonne "DinnerID" soit une clé principale unique pour la table. Nous pouvons configurer ceci en cliquant à droite sur la colonne « DinnerID » et en choisissant l’élément de menu « Clé primaire » :

![](create-a-database/_static/image8.png)

En plus de faire de DinnerID une clé principale, nous voulons également la configurer comme une colonne « identitaire » dont la valeur est automatiquement incrémentée au fur et à mesure que de nouvelles lignes de données sont ajoutées à la table (ce qui signifie que la première rangée insérée aura un DinnerID de 1, la deuxième rangée insérée aura un DinnerID de 2, etc.).

Nous pouvons le faire en sélectionnant la colonne "DinnerID" et ensuite utiliser l’éditeur "Column Properties" pour définir la propriété "(Est-identité)" sur la colonne à "Oui". Nous utiliserons les défauts d’identité standard (à partir de 1 et incrément 1 sur chaque nouvelle rangée de dîner) :

![](create-a-database/_static/image9.png)

Nous enregistrerons ensuite notre table en tapant Ctrl-S ou en utilisant la commande de menu **File-Enregistrer.&gt;** Cela nous incitera à nommer la table. Nous l’appellerons "Dîners":

![](create-a-database/_static/image10.png)

Notre nouvelle table Dîners apparaîtra ensuite dans notre base de données dans l’explorateur serveur.

Nous allons ensuite répéter les étapes ci-dessus et créer une table "RSVP". Ce tableau avec 3 colonnes. Nous allons configurer la colonne RsvpID comme la clé principale, et aussi en faire une colonne d’identité:

![](create-a-database/_static/image11.png)

Nous allons le sauver et lui donner le nom "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Mise en place d’une relation clé à l’étranger entre les tables

Nous avons maintenant deux tableaux dans notre base de données. Notre dernière étape de conception de schéma sera d’installer une relation « un à plusieurs » entre ces deux tables - afin que nous puissions associer chaque rangée de dîner avec zéro ou plus de lignes RSVP qui s’appliquent à elle. Nous le ferons en configurant la colonne "DinnerID" de la table RSVP pour avoir une relation à l’étranger avec la colonne "DinnerID" dans la table "Dîners".

Pour ce faire, nous allons ouvrir la table RSVP dans le concepteur de table en le cliquant en double dans l’explorateur serveur. Nous sélectionnerons ensuite la colonne "DinnerID" en son sein, en clic droit, et choisirons les "Relations..." commande de menu contexte :

![](create-a-database/_static/image12.png)

Cela fera l’un des cas de dialogue que nous pouvons utiliser pour configurer les relations entre les tables :

![](create-a-database/_static/image13.png)

Nous allons cliquer sur le bouton "Ajouter" pour ajouter une nouvelle relation au dialogue. Une fois qu’une relation a été ajoutée, nous allons étendre le "Tables and Column Specification" noeud de vue d’arbre dans la grille de propriété à droite du dialogue, puis cliquez sur le "..." bouton à droite de celui-ci:

![](create-a-database/_static/image14.png)

En cliquant sur le "..." bouton fera ressortir un autre dialogue qui nous permet de spécifier les tableaux et les colonnes sont impliqués dans la relation, ainsi que nous permettre de nommer la relation.

Nous modifierons la Table clé primaire pour être des « dîners » et sélectionnerons la colonne « DinnerID » dans la table des dîners comme clé principale. Notre table RSVP sera la table clé à l’étranger, et le RSVP. La colonne DinnerID sera associée à la clé étrangère :

![](create-a-database/_static/image15.png)

Maintenant, chaque rangée dans la table RSVP sera associée à une rangée dans la table du dîner. SQL Server maintiendra l’intégrité référentielle pour nous et nous empêchera d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une rangée de dîner valide. Il nous empêchera également de supprimer une rangée de dîner s’il ya encore des rangées RSVP se référant à elle.

### <a name="adding-data-to-our-tables"></a>Ajout de données à nos tables

Finissons-en en ajoutant quelques exemples de données à notre table dîners. Nous pouvons ajouter des données à une table en cliquant à droite sur elle dans le Server Explorer et en choisissant la commande "Show Table Data" :

![](create-a-database/_static/image16.png)

Nous ajouterons quelques lignes de données Dîner que nous pourrons utiliser plus tard que nous commençons à implémenter l’application:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>étape suivante

Nous avons fini de créer notre base de données. Créons maintenant des classes modèles que nous pouvons utiliser pour l’interroger et le mettre à jour.

> [!div class="step-by-step"]
> [Suivant précédent](create-a-new-aspnet-mvc-project.md)
> [Next](build-a-model-with-business-rule-validations.md)
