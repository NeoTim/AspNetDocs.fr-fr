---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Utilisation des dépendances de cache SQL (VB) | Microsoft Docs
author: rick-anderson
description: La stratégie de mise en cache la plus simple consiste à autoriser l’expiration des données mises en cache après une période spécifiée. Toutefois, cette approche simple signifie que les données mises en cache maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 175169772ba04376e1bd1cb143b13a200652a9bf
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044790"
---
# <a name="using-sql-cache-dependencies-vb"></a>Utilisation de dépendances de cache SQL (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) ou [Télécharger le PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La stratégie de mise en cache la plus simple consiste à autoriser l’expiration des données mises en cache après une période spécifiée. Toutefois, cette approche simple signifie que les données mises en cache ne gèrent aucune association avec sa source de données sous-jacente, ce qui entraîne des données périmées qui sont conservées trop longtemps ou qui ont expiré trop tôt. Une meilleure approche consiste à utiliser la classe SqlCacheDependency afin que les données restent mises en cache jusqu’à ce que ses données sous-jacentes aient été modifiées dans la base de données SQL. Ce didacticiel vous explique les procédures.

## <a name="introduction"></a>Introduction

Les techniques de mise en cache examinées dans les [données de mise en cache avec ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) et [Caching Data dans les didacticiels de l’architecture](caching-data-in-the-architecture-vb.md) utilisaient une expiration temporelle pour supprimer les données du cache après une période spécifiée. Cette approche est la méthode la plus simple pour équilibrer les gains de performances de la mise en cache par rapport à l’obsolescence des données. En sélectionnant un délai d’expiration de *x* secondes, un développeur de pages ne peut bénéficier des avantages en matière de performances de la mise en cache que de *x* secondes, mais il est facile que les données ne soient jamais obsolètes au-delà de *x* secondes. Bien sûr, pour les données statiques, *x* peut être étendu jusqu’à la durée de vie de l’application Web, comme indiqué dans le didacticiel de [mise en cache des données au démarrage](caching-data-at-application-startup-vb.md) de l’application.

Lors de la mise en cache des données de base de données, une expiration temporelle est souvent choisie pour sa simplicité d’utilisation, mais il s’agit souvent d’une solution inadéquate. Dans l’idéal, les données de la base de données restent mises en cache jusqu’à ce que les données sous-jacentes soient modifiées dans la base de données. le cache est supprimé uniquement. Cette approche optimise les avantages en matière de performances de la mise en cache et réduit la durée des données obsolètes. Toutefois, pour bénéficier de ces avantages, vous devez disposer d’un système en place qui sache quand les données de base de données sous-jacentes ont été modifiées et supprime les éléments correspondants du cache. Avant ASP.NET 2,0, les développeurs de pages étaient responsables de l’implémentation de ce système.

ASP.NET 2,0 fournit une [ `SqlCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) et l’infrastructure nécessaire pour déterminer quand une modification s’est produite dans la base de données afin que les éléments mis en cache correspondants puissent être supprimés. Il existe deux techniques pour déterminer le moment où les données sous-jacentes ont changé : notification et interrogation. Après avoir abordé les différences entre la notification et l’interrogation, nous allons créer l’infrastructure nécessaire à la prise en charge de l’interrogation, puis découvrir comment utiliser la `SqlCacheDependency` classe dans des scénarios déclaratifs et par programme.

## <a name="understanding-notification-and-polling"></a>Compréhension des notifications et des interrogations

Il existe deux techniques qui peuvent être utilisées pour déterminer le moment où les données d’une base de données ont été modifiées : notification et interrogation. Avec la notification, la base de données avertit automatiquement le runtime ASP.NET lorsque les résultats d’une requête particulière ont été modifiés depuis la dernière exécution de la requête. les éléments mis en cache associés à la requête sont alors supprimés. Avec l’interrogation, le serveur de base de données conserve les informations relatives à la dernière mise à jour des tables particulières. Le runtime ASP.NET interroge régulièrement la base de données pour vérifier les tables qui ont été modifiées depuis leur entrée dans le cache. Les éléments de cache associés sont supprimés des tables dont les données ont été modifiées.

L’option de notification requiert moins d’installation que l’interrogation et est plus granulaire, car elle effectue le suivi des modifications au niveau de la requête plutôt qu’au niveau de la table. Malheureusement, les notifications sont uniquement disponibles dans les éditions complètes de Microsoft SQL Server 2005 (c’est-à-dire, les éditions non-Express). Toutefois, l’option d’interrogation peut être utilisée pour toutes les versions de Microsoft SQL Server de 7,0 à 2005. Étant donné que ces didacticiels utilisent l’édition Express de SQL Server 2005, nous nous concentrerons sur la configuration et l’utilisation de l’option d’interrogation. Pour plus d’informations sur les fonctionnalités de notification de SQL Server 2005 s, consultez la section de lecture supplémentaire à la fin de ce didacticiel.

Avec l’interrogation, la base de données doit être configurée pour inclure une table nommée `AspNet_SqlCacheTablesForChangeNotification` contenant trois colonnes : `tableName` , `notificationCreated` et `changeId` . Cette table contient une ligne pour chaque table qui contient des données qui devront peut-être être utilisées dans une dépendance de cache SQL dans l’application Web. La `tableName` colonne spécifie le nom de la table, tandis que `notificationCreated` indique la date et l’heure à laquelle la ligne a été ajoutée à la table. La `changeId` colonne est de type `int` et a une valeur initiale de 0. Sa valeur est incrémentée à chaque modification apportée à la table.

En plus de la `AspNet_SqlCacheTablesForChangeNotification` table, la base de données doit également inclure des déclencheurs sur chacune des tables qui peuvent apparaître dans une dépendance de cache SQL. Ces déclencheurs sont exécutés chaque fois qu’une ligne est insérée, mise à jour ou supprimée et incrémente la valeur de la table s `changeId` dans `AspNet_SqlCacheTablesForChangeNotification` .

Le runtime ASP.NET suit le actuel `changeId` pour une table lors de la mise en cache des données à l’aide d’un `SqlCacheDependency` objet. La base de données est vérifiée périodiquement et tous les `SqlCacheDependency` objets dont le `changeId` diffère de la valeur dans la base de données sont supprimés, car une `changeId` valeur différente indique qu’une modification a été apportée à la table depuis que les données ont été mises en cache.

## <a name="step-1-exploring-theaspnet_regsqlexecommand-line-program"></a>Étape 1 : exploration du `aspnet_regsql.exe` programme en ligne de commande

Avec l’approche d’interrogation, la base de données doit être configurée pour contenir l’infrastructure décrite ci-dessus : une table prédéfinie ( `AspNet_SqlCacheTablesForChangeNotification` ), une poignée de procédures stockées et des déclencheurs sur chacune des tables qui peuvent être utilisées dans les dépendances de cache SQL dans l’application Web. Ces tables, procédures stockées et déclencheurs peuvent être créés via le programme `aspnet_regsql.exe` de ligne de commande, qui se trouve dans le `$WINDOWS$\Microsoft.NET\Framework\version` dossier. Pour créer la `AspNet_SqlCacheTablesForChangeNotification` table et les procédures stockées associées, exécutez la commande suivante à partir de la ligne de commande :

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Pour exécuter ces commandes, la connexion à la base de données spécifiée doit se trouver dans les [`db_securityadmin`](https://msdn.microsoft.com/library/ms188685.aspx) [`db_ddladmin`](https://msdn.microsoft.com/library/ms190667.aspx) rôles et. Pour examiner le T-SQL envoyé à la base de données par le `aspnet_regsql.exe` programme de ligne de commande, reportez-vous à [cette entrée de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).

Par exemple, pour ajouter l’infrastructure d’interrogation à une base de données Microsoft SQL Server nommée `pubs` sur un serveur de base de données nommé `ScottsServer` à l’aide de l’authentification Windows, accédez au répertoire approprié et, à partir de la ligne de commande, entrez :

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Une fois l’infrastructure au niveau de la base de données ajoutée, nous devons ajouter les déclencheurs aux tables qui seront utilisées dans les dépendances de cache SQL. Utilisez de `aspnet_regsql.exe` nouveau le programme de ligne de commande, mais spécifiez le nom de la table à l’aide du `-t` commutateur et au lieu d' `-ed` utiliser le commutateur `-et` , comme suit :

[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Pour ajouter les déclencheurs aux `authors` tables et `titles` sur la `pubs` base de données sur `ScottsServer` , utilisez :

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Pour ce didacticiel, ajoutez les déclencheurs aux `Products` `Categories` tables, et `Suppliers` . Nous allons examiner la syntaxe de ligne de commande particulière à l’étape 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inapp_data"></a>Étape 2 : référencement d’une base de données Microsoft SQL Server 2005 Express Edition dans`App_Data`

Le `aspnet_regsql.exe` programme de ligne de commande requiert le nom de la base de données et du serveur afin d’ajouter l’infrastructure d’interrogation nécessaire. Mais qu’est-ce que la base de données et le nom de serveur pour une base de données Microsoft SQL Server 2005 Express qui réside dans le `App_Data` dossier ? Plutôt que d’avoir à découvrir ce que sont les noms de base de données et de serveur, j’ai découvert que l’approche la plus simple consiste à attacher la base de données à l' `localhost\SQLExpress` instance de base de données et à renommer les données à l’aide de [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Si l’une des versions complètes de SQL Server 2005 est installée sur votre ordinateur, il est probable que SQL Server Management Studio est déjà installé sur votre ordinateur. Si vous disposez uniquement de l’édition Express, vous pouvez télécharger le [Microsoft SQL Server gratuit Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Commencez par fermer Visual Studio. Ensuite, ouvrez SQL Server Management Studio et choisissez de vous connecter au `localhost\SQLExpress` serveur à l’aide de l’authentification Windows.

![Attacher au serveur localhost\SQLExpress](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figure 1**: attachement au `localhost\SQLExpress` serveur

Une fois connecté au serveur, Management Studio affiche le serveur et contient des sous-dossiers pour les bases de données, la sécurité, etc. Cliquez avec le bouton droit sur le dossier bases de données et choisissez l’option attacher. La boîte de dialogue attacher des bases de données s’affiche (voir figure 2). Cliquez sur le bouton Ajouter et sélectionnez le `NORTHWND.MDF` dossier base de données dans le dossier de votre application Web `App_Data` .

[![Attachez le fichier NORTHWND. Base de données MDF à partir du dossier App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figure 2**: attacher la `NORTHWND.MDF` base de données à partir du `App_Data` dossier ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image2.png))

Cette opération ajoute la base de données au dossier bases de données. Le nom de la base de données peut être le chemin d’accès complet au fichier de base de données ou le chemin d’accès complet précédé d’un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Pour éviter d’avoir à taper ce nom de base de données longue lors de l’utilisation de l' \_ outil de ligne de commande aspnetregsql.exe, renommez la base de données avec un nom plus convivial en cliquant avec le bouton droit sur la base de données qui vient d’être attachée et en choisissant renommer. J’ai renommé ma base de données en DataTutorials.

![Renommer la base de données attachée avec un nom plus convivial](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figure 3**: renommer la base de données attachée avec un nom plus convivial

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Étape 3 : ajout de l’infrastructure d’interrogation à la base de données Northwind

Maintenant que nous avons joint la `NORTHWND.MDF` base de données à partir du `App_Data` dossier, nous sommes prêts à ajouter l’infrastructure d’interrogation. En supposant que vous avez renommé la base de données en DataTutorials, exécutez les quatre commandes suivantes :

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Après avoir exécuté ces quatre commandes, cliquez avec le bouton droit sur le nom de la base de données dans Management Studio, accédez au sous-menu tâches, puis choisissez détacher. Fermez ensuite Management Studio et rouvrez Visual Studio.

Une fois que Visual Studio s’est rouvert, explorez la base de données par le biais du Explorateur de serveurs. Notez la nouvelle table ( `AspNet_SqlCacheTablesForChangeNotification` ), les nouvelles procédures stockées et les déclencheurs sur les `Products` tables, `Categories` et `Suppliers` .

![La base de données comprend désormais l’infrastructure d’interrogation nécessaire](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figure 4**: la base de données comprend désormais l’infrastructure d’interrogation nécessaire

## <a name="step-4-configuring-the-polling-service"></a>Étape 4 : configuration du service d’interrogation

Une fois que vous avez créé les tables, les déclencheurs et les procédures stockées nécessaires dans la base de données, l’étape finale consiste à configurer le service d’interrogation, qui est effectué `Web.config` en spécifiant les bases de données à utiliser et la fréquence d’interrogation en millisecondes. La balise suivante interroge la base de données Northwind une fois par seconde.

[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

La `name` valeur de l' `<add>` élément (NorthwindDB) associe un nom lisible par l’utilisateur à une base de données particulière. Lorsque vous utilisez des dépendances de cache SQL, nous devons faire référence au nom de la base de données défini ici, ainsi qu’à la table sur laquelle les données mises en cache sont basées. Nous verrons comment utiliser la `SqlCacheDependency` classe pour associer par programmation les dépendances de cache SQL aux données mises en cache à l’étape 6.

Une fois qu’une dépendance de cache SQL a été établie, le système d’interrogation se connectera aux bases de données définies dans les `<databases>` éléments chaque `pollTime` milliseconde et exécutera la `AspNet_SqlCachePollingStoredProcedure` procédure stockée. Cette procédure stockée, qui a été ajoutée à l’étape 3 à l’aide de l' `aspnet_regsql.exe` outil en ligne de commande, retourne les `tableName` `changeId` valeurs et pour chaque enregistrement dans `AspNet_SqlCacheTablesForChangeNotification` . Les dépendances de cache SQL obsolètes sont supprimées du cache.

Le `pollTime` paramètre introduit un compromis entre les performances et l’obsolescence des données. Une petite `pollTime` valeur augmente le nombre de demandes à la base de données, mais supprime plus rapidement les données obsolètes du cache. Une `pollTime` valeur supérieure réduit le nombre de demandes de base de données, mais augmente le délai entre le moment où les données principales sont modifiées et le moment où les éléments de cache associés sont supprimés. Heureusement, la requête de base de données exécute une procédure stockée simple qui retourne seulement quelques lignes d’une table simple et légère. Mais faites des essais avec différentes `pollTime` valeurs pour trouver un équilibre idéal entre l’accès aux bases de données et l’obsolescence des données pour votre application. La plus petite `pollTime` valeur autorisée est 500.

> [!NOTE]
> L’exemple ci-dessus fournit une `pollTime` valeur unique dans l' `<sqlCacheDependency>` élément, mais vous pouvez éventuellement spécifier la `pollTime` valeur dans l' `<add>` élément. Cela est utile si vous avez plusieurs bases de données spécifiées et que vous souhaitez personnaliser la fréquence d’interrogation par base de données.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Étape 5 : utilisation déclarative des dépendances de cache SQL

Dans les étapes 1 à 4, nous avons vu comment configurer l’infrastructure de base de données nécessaire et configurer le système d’interrogation. Une fois cette infrastructure en place, nous pouvons maintenant ajouter des éléments au cache de données avec une dépendance de cache SQL associée à l’aide de techniques de programmation ou déclaratives. Dans cette étape, nous allons examiner comment travailler de façon déclarative avec les dépendances de cache SQL. À l’étape 6, nous allons examiner l’approche par programmation.

La [mise en cache des données avec le didacticiel ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) a exploré les fonctionnalités de mise en cache déclaratives de l’ObjectDataSource. En définissant simplement la `EnableCaching` propriété sur `True` et la `CacheDuration` propriété sur un intervalle de temps, l’ObjectDataSource met automatiquement en cache les données retournées par l’objet sous-jacent pour l’intervalle spécifié. ObjectDataSource peut également utiliser une ou plusieurs dépendances de cache SQL.

Pour illustrer l’utilisation de dépendances de cache SQL de façon déclarative, ouvrez la `SqlCacheDependencies.aspx` page dans le `Caching` dossier et faites glisser un contrôle GridView de la boîte à outils vers le concepteur. Définissez le contrôle GridView `ID` sur `ProductsDeclarative` et, à partir de sa balise active, choisissez de le lier à un nouvel ObjectDataSource nommé `ProductsDataSourceDeclarative` .

[![Créer un ObjectDataSource nommé ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figure 5**: créer un ObjectDataSource nommé `ProductsDataSourceDeclarative` ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image4.png))

Configurez l’ObjectDataSource pour utiliser la `ProductsBLL` classe et définissez la liste déroulante de l’onglet sélectionner sur `GetProducts()` . Dans l’onglet mettre à jour, choisissez la `UpdateProduct` surcharge avec trois paramètres d’entrée : `productName` , `unitPrice` et `productID` . Définissez les listes déroulantes sur (aucune) dans les onglets insérer et supprimer.

[![Utiliser la surcharge UpdateProduct avec trois paramètres d’entrée](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figure 6**: utilisation de la surcharge UpdateProduct avec trois paramètres d’entrée ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image6.png))

[![Définir la liste déroulante sur (aucune) pour les onglets insérer et supprimer](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figure 7**: définir la liste déroulante sur (aucune) pour les onglets insérer et supprimer ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image8.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio crée BoundFields et CheckBoxFields dans le GridView pour chacun des champs de données. Supprimez tous les champs, mais, `ProductName` `CategoryName` et, `UnitPrice` et mettez en forme ces champs comme vous le voulez. À partir de la balise active GridView s, activez les cases à cocher Activer la pagination, activer le tri et activer la modification. Visual Studio affecte la valeur à la `OldValuesParameterFormatString` propriété ObjectDataSource s `original_{0}` . Pour que la fonctionnalité de modification de GridView s fonctionne correctement, supprimez entièrement cette propriété de la syntaxe déclarative ou redéfinissez-la sur sa valeur par défaut, `{0}` .

Enfin, ajoutez un contrôle Web Label au-dessus de GridView et affectez à sa propriété la valeur `ID` `ODSEvents` `EnableViewState` `False` . Après avoir apporté ces modifications, le balisage déclaratif de votre page doit ressembler à ce qui suit. Notez que j’ai apporté plusieurs personnalisations esthétiques aux champs GridView qui ne sont pas nécessaires pour illustrer la fonctionnalité de dépendance de cache SQL.

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Ensuite, créez un gestionnaire d’événements pour l’événement ObjectDataSource s `Selecting` et ajoutez le code suivant :

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

N’oubliez pas que l' `Selecting` événement ObjectDataSource s se déclenche uniquement lors de la récupération de données à partir de son objet sous-jacent. Si l’ObjectDataSource accède aux données à partir de son propre cache, cet événement n’est pas déclenché.

À présent, accédez à cette page via un navigateur. Étant donné que nous n’avons pas encore implémenté de mise en cache, chaque fois que vous pagez, triez ou modifiez la grille, la page doit afficher le texte «sélection de l’événement déclenché, comme le montre la figure 8.

[![L’événement ObjectDataSource s Selecting est déclenché chaque fois que le contrôle GridView est paginé, modifié ou trié](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figure 8**: l’événement ObjectDataSource s `Selecting` est déclenché chaque fois que le contrôle GridView est paginé, modifié ou trié ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image10.png))

Comme nous l’avons vu dans la [mise en cache des données avec le didacticiel ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) , la définition de la `EnableCaching` propriété sur `True` oblige le ObjectDataSource à mettre en cache ses données pour la durée spécifiée par sa `CacheDuration` propriété. L’ObjectDataSource possède également une [ `SqlCacheDependency` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), qui ajoute une ou plusieurs dépendances de cache SQL aux données mises en cache à l’aide du modèle :

[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Où *DatabaseName* est le nom de la base de données, tel qu’il est spécifié dans l' `name` attribut de l' `<add>` élément dans `Web.config` , et *TableName* est le nom de la table de base de données. Par exemple, pour créer un ObjectDataSource qui met en cache les données indéfiniment en fonction d’une dépendance de cache SQL par rapport à la table des comptoirs `Products` , définissez la propriété ObjectDataSource s `EnableCaching` sur `True` et sa `SqlCacheDependency` propriété sur NorthwindDB : Products.

> [!NOTE]
> Vous pouvez utiliser une dépendance de cache SQL *et* une expiration basée sur la durée en affectant à `EnableCaching` la valeur `True` , `CacheDuration` à l’intervalle de temps et `SqlCacheDependency` aux noms de la base de données et de la table. ObjectDataSource supprimera ses données lorsque l’expiration basée sur la durée est atteinte ou lorsque le système d’interrogation note que les données de la base de données sous-jacente ont changé, selon ce qui se produit en premier.

Le contrôle GridView dans `SqlCacheDependencies.aspx` affiche les données de deux tables, `Products` et `Categories` (le `CategoryName` champ produit s est récupéré via un `JOIN` sur `Categories` ). Par conséquent, nous souhaitons spécifier deux dépendances de cache SQL : NorthwindDB : Products ; NorthwindDB : catégories.

[![Configurer ObjectDataSource pour prendre en charge la mise en cache à l’aide des dépendances de cache SQL sur les produits et les catégories](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figure 9**: configurer ObjectDataSource pour prendre en charge la mise en cache à l’aide des dépendances de cache SQL sur `Products` et `Categories` ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image12.png))

Après avoir configuré ObjectDataSource pour prendre en charge la mise en cache, réexaminez la page via un navigateur. Là encore, le texte «sélection de l’événement déclenché doit apparaître sur la première page, mais doit disparaître lors de la pagination, du tri ou du clic sur les boutons modifier ou annuler. En effet, une fois que les données ont été chargées dans le cache de l’ObjectDataSource, elles sont conservées jusqu’à ce que les `Products` `Categories` tables ou soient modifiées ou que les données soient mises à jour via le contrôle GridView.

Après la pagination dans la grille et en notant l’absence du texte « sélection de l’événement déclenché », ouvrez une nouvelle fenêtre de navigateur et accédez au didacticiel de base dans la section modification, insertion et suppression ( `~/EditInsertDelete/Basics.aspx` ). Mettez à jour le nom ou le prix d’un produit. Ensuite, dans la première fenêtre du navigateur, affichez une page de données différente, triez la grille ou cliquez sur le bouton modifier une ligne. Cette fois-ci, l’événement de sélection déclenché doit réapparaître, car les données de la base de données sous-jacente ont été modifiées (voir la figure 10). Si le texte n’apparaît pas, patientez quelques instants, puis réessayez. N’oubliez pas que le service d’interrogation vérifie les modifications apportées à la `Products` table toutes les `pollTime` millisecondes. il y a donc un délai entre le moment où les données sous-jacentes sont mises à jour et le moment où les données mises en cache sont supprimées.

[![La modification de la table Products supprime les données du produit mises en cache.](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figure 10**: la modification de la table Products permet de supprimer les données du produit mises en cache ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Étape 6 : utilisation de la classe par programmation `SqlCacheDependency`

Les [données de mise en cache dans le](caching-data-in-the-architecture-vb.md) didacticiel sur l’architecture ont vu les avantages de l’utilisation d’une couche de mise en cache distincte dans l’architecture, par opposition à un couplage étroit de la mise en cache avec ObjectDataSource. Dans ce didacticiel, nous avons créé une `ProductsCL` classe pour illustrer l’utilisation du cache de données par programmation. Pour utiliser les dépendances de cache SQL dans la couche de mise en cache, utilisez la `SqlCacheDependency` classe.

Avec le système d’interrogation, un `SqlCacheDependency` objet doit être associé à une base de données et à une paire de tables spécifiques. Le code suivant, par exemple, crée un `SqlCacheDependency` objet basé sur la table s de la base de données Northwind `Products` :

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Les deux paramètres d’entrée du `SqlCacheDependency` constructeur s sont les noms de la base de données et de la table, respectivement. Comme avec la propriété ObjectDataSource s `SqlCacheDependency` , le nom de la base de données utilisé est identique à la valeur spécifiée dans l' `name` attribut de l' `<add>` élément dans `Web.config` . Le nom de la table est le nom réel de la table de base de données.

Pour associer un `SqlCacheDependency` à un élément ajouté au cache de données, utilisez l’une des `Insert` surcharges de méthode qui accepte une dépendance. Le code suivant ajoute de la *valeur* au cache de données pour une durée indéfinie, mais l’associe à un `SqlCacheDependency` sur la `Products` table. En résumé, la *valeur* reste dans le cache jusqu’à ce qu’elle soit supprimée en raison de contraintes de mémoire ou parce que le système d’interrogation a détecté que la `Products` table a été modifiée depuis qu’elle a été mise en cache.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

La classe de la couche s de mise en cache `ProductsCL` met actuellement en cache les données de la `Products` table à l’aide d’une expiration basée sur la durée de 60 secondes. Mettez à jour cette classe afin qu’elle utilise à la place des dépendances de cache SQL. La `ProductsCL` `AddCacheItem` méthode de classe s, qui est responsable de l’ajout des données au cache, contient actuellement le code suivant :

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Mettez à jour ce code pour utiliser un `SqlCacheDependency` objet au lieu de la `MasterCacheKeyArray` dépendance de cache :

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Pour tester cette fonctionnalité, ajoutez un GridView à la page sous le `ProductsDeclarative` GridView existant. Définissez ce nouveau contrôle GridView `ID` sur `ProductsProgrammatic` et, via sa balise active, liez-le à un nouvel ObjectDataSource nommé `ProductsDataSourceProgrammatic` . Configurez l’ObjectDataSource pour utiliser la `ProductsCL` classe, en définissant les listes déroulantes dans les onglets sélectionner et mettre à jour sur `GetProducts` et `UpdateProduct` , respectivement.

[![Configurer ObjectDataSource pour utiliser la classe ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figure 11**: configurer ObjectDataSource pour utiliser la `ProductsCL` classe ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image16.png))

[![Sélectionnez la méthode GetProducts dans la liste déroulante Sélectionner les onglets](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figure 12**: sélectionner la `GetProducts` méthode dans la liste déroulante Sélectionner les onglets ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image18.png))

[![Choisissez la méthode UpdateProduct dans la liste déroulante des onglets mettre à jour.](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figure 13**: choisir la méthode UpdateProduct dans la liste déroulante des onglets de mise à jour ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image20.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio crée BoundFields et CheckBoxFields dans le GridView pour chacun des champs de données. Comme avec le premier GridView ajouté à cette page, supprimez tous les champs,, `ProductName` `CategoryName` et, `UnitPrice` et mettez en forme ces champs comme vous le souhaitez. À partir de la balise active GridView s, activez les cases à cocher Activer la pagination, activer le tri et activer la modification. Comme avec `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio affecte la valeur `ProductsDataSourceProgrammatic` à la `OldValuesParameterFormatString` propriété ObjectDataSource s `original_{0}` . Pour que la fonctionnalité de modification de GridView s fonctionne correctement, affectez à cette propriété la valeur `{0}` (ou supprimez l’assignation de propriété de la syntaxe déclarative).

Une fois ces tâches terminées, le balisage déclaratif GridView et ObjectDataSource obtenu doit ressembler à ce qui suit :

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Pour tester la dépendance de cache SQL dans la couche de mise en cache, définissez un point d’arrêt dans la méthode de la `ProductCL` classe s `AddCacheItem` , puis démarrez le débogage. Lorsque vous accédez pour la première fois `SqlCacheDependencies.aspx` , le point d’arrêt doit être atteint, car les données sont demandées pour la première fois et placées dans le cache. Ensuite, accédez à une autre page dans le GridView ou Triez l’une des colonnes. Cela amène le GridView à interroger ses données, mais les données doivent se trouver dans le cache depuis que la `Products` table de base de données n’a pas été modifiée. Si les données ne se trouvent pas à plusieurs reprises dans le cache, assurez-vous qu’il y a suffisamment de mémoire disponible sur votre ordinateur, puis réessayez.

Après avoir paginé quelques pages du GridView, ouvrez une deuxième fenêtre de navigateur et accédez au didacticiel de base dans la section modification, insertion et suppression ( `~/EditInsertDelete/Basics.aspx` ). Mettez à jour un enregistrement de la table Products, puis, dans la première fenêtre du navigateur, affichez une nouvelle page ou cliquez sur l’un des en-têtes de tri.

Dans ce scénario, vous verrez l’un des deux éléments suivants : soit le point d’arrêt est atteint, ce qui indique que les données mises en cache ont été supprimées en raison de la modification de la base de données ; ou, le point d’arrêt n’est pas atteint, ce qui signifie que `SqlCacheDependencies.aspx` affiche maintenant des données obsolètes. Si le point d’arrêt n’est pas atteint, cela est probablement dû au fait que le service d’interrogation n’a pas encore été déclenché depuis que les données ont été modifiées. N’oubliez pas que le service d’interrogation vérifie les modifications apportées à la `Products` table toutes les `pollTime` millisecondes. il y a donc un délai entre le moment où les données sous-jacentes sont mises à jour et le moment où les données mises en cache sont supprimées.

> [!NOTE]
> Ce délai est plus susceptible d’apparaître lors de la modification de l’un des produits par le biais du contrôle GridView dans `SqlCacheDependencies.aspx` . Dans le didacticiel sur la [mise en cache des données dans l’architecture](caching-data-in-the-architecture-vb.md) , nous avons ajouté la `MasterCacheKeyArray` dépendance du cache pour vous assurer que les données modifiées par le biais `ProductsCL` de la méthode de la classe s `UpdateProduct` ont été supprimées du cache. Toutefois, nous avons remplacé cette dépendance de cache lors de la modification de la `AddCacheItem` méthode plus tôt dans cette étape. par conséquent, la `ProductsCL` classe continue d’afficher les données mises en cache jusqu’à ce que le système d’interrogation note la modification dans la `Products` table. Nous verrons comment réintroduire la `MasterCacheKeyArray` dépendance du cache à l’étape 7.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Étape 7 : Association de plusieurs dépendances à un élément mis en cache

Souvenez `MasterCacheKeyArray` -vous que la dépendance de cache est utilisée pour s’assurer que *toutes les* données relatives au produit sont supprimées du cache lorsqu’un seul élément associé est mis à jour. Par exemple, la `GetProductsByCategoryID(categoryID)` méthode met en cache `ProductsDataTables` des instances pour chaque valeur *CategoryID* unique. Si l’un de ces objets est expulsé, la `MasterCacheKeyArray` dépendance de cache garantit que les autres sont également supprimés. Sans cette dépendance de cache, lorsque les données mises en cache sont modifiées, il est possible que d’autres données de produit mises en cache soient obsolètes. Par conséquent, il est important que nous maintenions la `MasterCacheKeyArray` dépendance de cache lors de l’utilisation de dépendances de cache SQL. Toutefois, la méthode s du cache `Insert` de données n’autorise qu’un seul objet de dépendance.

En outre, lorsque vous utilisez des dépendances de cache SQL, nous pouvons être amenés à associer plusieurs tables de base de données en tant que dépendances. Par exemple, le `ProductsDataTable` mis en cache dans la `ProductsCL` classe contient les noms des catégories et des fournisseurs pour chaque produit, mais la `AddCacheItem` méthode utilise uniquement une dépendance sur `Products` . Dans ce cas, si l’utilisateur met à jour le nom d’une catégorie ou d’un fournisseur, les données du produit mises en cache sont conservées dans le cache et ne sont plus à jour. Par conséquent, nous souhaitons que les données du produit mises en cache dépendent non seulement de la `Products` table, mais `Categories` `Suppliers` également des tables et.

La [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) fournit un moyen d’associer plusieurs dépendances à un élément de cache. Commencez par créer une `AggregateCacheDependency` instance. Ajoutez ensuite l’ensemble de dépendances à l’aide de la `AggregateCacheDependency` `Add` méthode s. Lors de l’insertion de l’élément dans le cache de données par la suite, transmettez l' `AggregateCacheDependency` instance. Lorsque l' *une* des `AggregateCacheDependency` dépendances de l’instance change, l’élément mis en cache est supprimé.

L’exemple suivant montre le code mis à jour pour la `ProductsCL` méthode s de la classe `AddCacheItem` . La méthode crée la `MasterCacheKeyArray` dépendance du cache avec des `SqlCacheDependency` objets pour `Products` les `Categories` tables, et `Suppliers` . Ils sont tous combinés dans un `AggregateCacheDependency` objet nommé `aggregateDependencies` , qui est ensuite transmis à la `Insert` méthode.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Testez ce nouveau code. Désormais, les modifications apportées aux `Products` `Categories` tables, ou `Suppliers` provoquent la suppression des données mises en cache. De plus, la `ProductsCL` méthode s de classe `UpdateProduct` , qui est appelée lors de la modification d’un produit via le contrôle GridView, supprime la `MasterCacheKeyArray` dépendance de cache, ce qui entraîne l’éviction du cache `ProductsDataTable` et la récupération des données lors de la requête suivante.

> [!NOTE]
> Les dépendances de cache SQL peuvent également être utilisées avec [la mise en cache de sortie](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Pour une démonstration de cette fonctionnalité, consultez [utilisation de la mise en cache de sortie ASP.net avec SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).

## <a name="summary"></a>Résumé

Lors de la mise en cache des données de base de données, les données restent idéalement dans le cache jusqu’à ce qu’elles soient modifiées dans la base de données. Avec ASP.NET 2,0, les dépendances de cache SQL peuvent être créées et utilisées dans des scénarios déclaratifs et de programmation. L’une des difficultés de cette approche est la découverte lorsque les données ont été modifiées. Les versions complètes de Microsoft SQL Server 2005 fournissent des fonctionnalités de notification qui peuvent alerter une application en cas de modification d’un résultat de requête. Pour l’édition Express de SQL Server 2005 et des versions antérieures de SQL Server, un système d’interrogation doit être utilisé à la place. Heureusement, la configuration de l’infrastructure d’interrogation nécessaire est relativement simple.

Bonne programmation !

## <a name="further-reading"></a>En savoir plus

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Utilisation des notifications de requête dans Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Création d’une notification de requête](https://msdn.microsoft.com/library/ms188669.aspx)
- [Mise en cache dans ASP.NET avec la `SqlCacheDependency` classe](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server Registration Tool ( `aspnet_regsql.exe` )](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Vue d’ensemble de `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à l’adresse [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à l’adresse [http://ScottOnWriting.NET](http://ScottOnWriting.NET) .

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Marko Rangel, Teresa Murphy et Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, déposez-moi une ligne à l’adresse [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](caching-data-at-application-startup-vb.md)
