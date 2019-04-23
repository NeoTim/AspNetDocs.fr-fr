---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interaction avec la Page de contenu à partir de la Page maître (VB) | Microsoft Docs
author: rick-anderson
description: Examine la manière d’appeler des méthodes, définissez des propriétés, etc. de la Page de contenu à partir du code dans la Page maître.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f0575474bc750cad15ac74c522e3138b326d880c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59400007"
---
# <a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interaction avec la page de contenu à partir de la page maître (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Examine la manière d’appeler des méthodes, définissez des propriétés, etc. de la Page de contenu à partir du code dans la Page maître.


## <a name="introduction"></a>Introduction

Le didacticiel précédent examiné comment avoir à la page de contenu d’interagir par programmation avec sa page maître. Rappel que nous avons mis à jour la page maître pour inclure un contrôle GridView qui liste les cinq plus récemment ajouté des produits. Nous avons ensuite créé une page de contenu à partir de laquelle l’utilisateur peut ajouter un nouveau produit. Lors de l’ajout d’un nouveau produit, la page de contenu nécessaire pour indiquer à la page maître pour actualiser ses GridView afin qu’il doit inclure le produit juste-ajouté. Cette fonctionnalité a été effectuée en ajoutant une méthode publique à la page maître qu’actualisé les données liées au GridView et puis en appelant cette méthode à partir de la page de contenu.

La forme la plus courante de contenu et l’interaction de la page maître provient de la page de contenu. Toutefois, il est possible pour la page maître pour rouse la page de contenu actuelle en action, et cette fonctionnalité peut être nécessaire si la page maître contient les éléments d’interface utilisateur qui permettent aux utilisateurs de modifier les données qui s’affiche également sur la page de contenu. Considérez qu’affiche les informations de produits dans un GridView contrôle et une page maître qui inclut un bouton de contrôle qui, lorsque vous cliquez sur une page de contenu, de double les prix de tous les produits. Comme l’exemple dans le didacticiel précédent, le contrôle GridView doit être actualisé après le prix double clic sur le bouton afin qu’il affiche les nouveaux prix, mais dans ce scénario, il est la page maître devant rouse la page de contenu à l’action.

Ce didacticiel explore comment procéder pour que la page maître à appeler la fonctionnalité définie dans la page de contenu.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Incitation des interactions par programme via un événement et des gestionnaires d’événements

L’appel de fonctionnalité de page de contenu à partir d’une page maître est plus complexe que l’autre sens. Car une page de contenu a une seule page maître, lors de l’interaction par programmation à partir de la page de contenu d’origine de celles-ci, nous savons ce que sont les propriétés et les méthodes publiques à notre disposition. Une page maître, cependant, peut avoir plusieurs pages de contenu différents, chacun avec son propre ensemble de propriétés et méthodes. Comment procéder, puis, pouvons nous écrire du code dans la page maître pour effectuer une action dans sa page de contenu lorsque nous ne savez pas quelle page de contenu sera appelé avant l’exécution ?

Envisagez un contrôle Web ASP.NET, tels que le contrôle de bouton. Un contrôle de bouton peut apparaître sur n’importe quel nombre de pages ASP.NET et a besoin d’un mécanisme par lequel peut signaler la page qu’il a été cliqué. Pour cela, à l’aide de *événements*. En particulier, le contrôle de bouton déclenche son `Click` événement lorsque l’utilisateur a cliqué dessus ; la page ASP.NET qui contient le bouton peut éventuellement répondre à cette notification via un *Gestionnaire d’événements*.

Ce même modèle peut être utilisé pour avoir une fonctionnalité de déclencheur de page maître dans ses pages de contenu :

1. Ajouter un événement à la page maître.
2. Déclenchez l’événement chaque fois que la page maître a besoin pour communiquer avec sa page de contenu. Par exemple, si la page maître a besoin être averti sa page de contenu que l’utilisateur a doublé les prix, son événement est déclenché immédiatement après que les prix ont été doublés.
3. Créez un gestionnaire d’événements dans les pages de contenu qui doivent prendre des mesures.

Cette suite de ce didacticiel implémente l’exemple présenté dans l’Introduction ; à savoir, une page de contenu qui répertorie les produits dans la base de données et une page maître qui inclut un bouton de contrôle à double les prix.

## <a name="step-1-displaying-products-in-a-content-page"></a>Étape 1 : Affichage des produits dans une Page de contenu

Notre première chose consiste à créer une page de contenu qui répertorie les produits à partir de la base de données Northwind. (Nous avons ajouté la base de données Northwind au projet dans le didacticiel précédent, [ *interagir avec la Page maître depuis la Page de contenu*](interacting-with-the-master-page-from-the-content-page-vb.md).) Commencez par ajouter une nouvelle page ASP.NET pour le `~/Admin` dossier nommé `Products.aspx`, veillez à lier à la `Site.master` page maître. Figure 1 illustre l’Explorateur de solutions, une fois que cette page a été ajoutée au site Web.


[![Ajouter une nouvelle Page ASP.NET dans le dossier Admin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Figure 01**: Ajoutez une nouvelle Page ASP.NET pour le `Admin` dossier ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


N’oubliez pas que dans le [ *spécifiant le titre, les balises Meta et les autres en-têtes HTML dans la Page maître* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) didacticiel, nous avons créé une classe de page de base personnalisée nommée `BasePage` qui génère le titre de la page si elle n’est pas définir explicitement. Accédez à la `Products.aspx` code-behind de la page de classe et de la dériver de `BasePage` (au lieu d’à partir de `System.Web.UI.Page`).

Enfin, mettez à jour le `Web.sitemap` fichier à inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour le contenu à la leçon d’Interaction de la Page maître :


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

L’ajout de ce `<siteMapNode>` élément est reflété dans les leçons liste (voir Figure 5).

Retour à `Products.aspx`. Dans le contrôle de contenu pour `MainContent`, ajoutez un contrôle GridView et nommez-le `ProductsGrid`. Lier le contrôle GridView à un nouveau contrôle SqlDataSource nommé `ProductsDataSource`.


[![Lier le contrôle GridView à un nouveau contrôle SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Figure 02**: Lier le contrôle GridView à un nouveau contrôle SqlDataSource ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Configurer l’Assistant afin qu’il utilise la base de données Northwind. Si vous avez parcouru le didacticiel précédent, vous devez déjà disposer d’une chaîne de connexion nommée `NorthwindConnectionString` dans `Web.config`. Choisissez cette chaîne de connexion dans la liste déroulante, comme illustré dans la Figure 3.


[![Configurer SqlDataSource pour utiliser la base de données Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Figure 03**: Configurer SqlDataSource pour utiliser la base de données Northwind ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Ensuite, spécifiez le contrôle de source de données `SELECT` instruction en choisissant la table Products dans la liste déroulante et en retournant le `ProductName` et `UnitPrice` colonnes (voir Figure 4). Cliquez sur Suivant, puis terminez l’Assistant Configurer la Source de données.


[![Retourner le ProductName et UnitPrice champs de la Table Products](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Figure 04**: Retourner le `ProductName` et `UnitPrice` champs à partir de la `Products` Table ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


C’est aussi simple que cela ! Après la fin de l’Assistant Visual Studio ajoute deux BoundFields pour le contrôle GridView à mettre en miroir de deux champs retournés par le contrôle SqlDataSource. Balisage des contrôles GridView et SqlDataSource suit. Figure 5 montre les résultats lorsqu’ils sont affichés via un navigateur.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Chaque produit et son prix est répertorié dans le contrôle GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Figure 05**: Chaque produit et son prix est répertorié dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> N’hésitez pas à nettoyer l’apparence du contrôle GridView. Quelques suggestions incluent la mise en forme de la valeur de UnitPrice affichée sous forme de devise et à l’aide de couleurs d’arrière-plan et polices pour améliorer l’apparence de la grille. Pour plus d’informations sur l’affichage et la mise en forme des données dans ASP.NET, reportez-vous à mon [fonctionne avec la série de didacticiels de données](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Étape 2 : Ajout d’un bouton de Double les prix à la Page maître

La tâche suivante consiste à ajouter un contrôle de bouton Web pour le contrôleur de page qui, lorsque vous cliquez dessus, sera double le prix de tous les produits dans la base de données. Ouvrez le `Site.master` page maître et faites glisser un bouton à partir de la boîte à outils vers le concepteur, en le plaçant sous la `RecentProductsDataSource` contrôle SqlDataSource que nous avons ajouté dans le didacticiel précédent. Le bouton Définir `ID` propriété `DoublePrice` et son `Text` propriété à « Double les prix des produits ».

Ensuite, ajoutez un contrôle SqlDataSource à la page maître, nommez-le `DoublePricesDataSource`. Cette SqlDataSource sera utilisé pour exécuter la `UPDATE` instruction en double tous les prix. Plus précisément, nous devons définir son `ConnectionString` et `UpdateCommand` propriétés à la chaîne de connexion appropriée et `UPDATE` instruction. Puis nous devons appeler ce contrôle SqlDataSource `Update` méthode lorsque le `DoublePrice` bouton. Pour définir le `ConnectionString` et `UpdateCommand` propriétés, sélectionnez le contrôle SqlDataSource et passez à la fenêtre Propriétés. Le `ConnectionString` ces chaînes de connexion déjà stockés dans des listes de propriétés `Web.config` dans une liste déroulante ; choisissez la `NorthwindConnectionString` option comme illustré dans la Figure 6.


[![Configurer pour utiliser le NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Figure 06**: Configurer SqlDataSource à utiliser le `NorthwindConnectionString` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Pour définir le `UpdateCommand` propriété, recherchez l’option UpdateQuery dans la fenêtre Propriétés. Lorsqu’elle est sélectionnée, cette propriété affiche un bouton avec des points de suspension ; Cliquez sur ce bouton pour afficher la boîte de dialogue Éditeur de commandes et paramètre indiquée dans la Figure 7. Tapez la commande suivante `UPDATE` instruction dans la zone de texte de la boîte de dialogue :


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Cette instruction, lors de l’exécution, doubler la `UnitPrice` valeur pour chaque enregistrement dans le `Products` table.


[![Définir la propriété UpdateCommand de SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Figure 07**: Définir de SqlDataSource `UpdateCommand` propriété ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Après avoir défini ces propriétés, balisage déclaratif de vos contrôles bouton et SqlDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Ne reste qu’à appeler son `Update` méthode lorsque le `DoublePrice` bouton. Créer un `Click` Gestionnaire d’événements pour le `DoublePrice` bouton et ajoutez le code suivant :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Pour tester cette fonctionnalité, visitez le `~/Admin/Products.aspx` page, nous avons créé à l’étape 1 et cliquez sur le bouton « Les prix des produits Double ». En cliquant sur le bouton provoque une publication (postback) et exécute la `DoublePrice` du bouton `Click` Gestionnaire d’événements, le fait de doubler les prix de tous les produits. Puis nouveau rendu de la page et le balisage est retourné et nouveau affiché dans le navigateur. Le contrôle GridView dans la page de contenu, répertorie Toutefois, les mêmes prix comme avant la « Double les prix des produits » bouton. Il s’agit, car les données sont initialement chargées dans le contrôle GridView avaient son état stockée dans l’état d’affichage, donc il n’est pas rechargée sur les publications sauf instruction contraire. Si vous visitez une page différente, puis revenez à la `~/Admin/Products.aspx` page, vous verrez les prix mis à jour.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Étape 3 : Déclencher un événement lorsque le prix sont doublées.

Étant donné que le contrôle GridView dans le `~/Admin/Products.aspx` page ne reflète pas immédiatement le doublement des prix, un utilisateur peut naturellement pensez qu’ils s’est pas cliquer sur le bouton « Les prix des produits Double », ou qu’il n’a pas fonctionné. Ils peuvent essayer d’en cliquant sur le bouton que d’autres fois, le fait de doubler les prix encore et encore. Pour corriger cela nous devons disposer de la grille dans le contenu page Afficher les nouveaux tarifs dès qu’ils sont doublées.

Comme indiqué précédemment dans ce didacticiel, nous devons déclencher un événement dans la page maître chaque fois que l’utilisateur clique sur le `DoublePrice` bouton. Un événement est un moyen d’une classe (un éditeur d’événements) à notifier à un autre un ensemble d’autres classes que quelque chose d’intéressant s’est produite (les abonnés aux événements). Dans cet exemple, la page maître est l’éditeur d’événements ; les pages de contenu qui s’intéressent à lorsque le `DoublePrice` bouton sont les abonnés.

Une classe s’abonne à un événement en créant un *Gestionnaire d’événements*, qui est une méthode qui est exécutée en réponse à l’événement déclenché. Le serveur de publication définit les événements qu’il déclenche en définissant un *délégué d’événement*. Le délégué d’événement spécifie les paramètres d’entrée, le Gestionnaire d’événements doit accepter. Dans le .NET Framework, les délégués d’événements ne pas retourner n’importe quelle valeur et accepter deux paramètres d’entrée :

- Un `Object`, qui identifie la source d’événements, et
- Une classe dérivée à partir de `System.EventArgs`

Le deuxième paramètre passé à un gestionnaire d’événements peut inclure des informations supplémentaires sur l’événement. Alors que la base de `EventArgs` classe ne passe pas le long de toutes les informations, le .NET Framework inclut un nombre de classes qui étendent `EventArgs` et englobent des propriétés supplémentaires. Par exemple, un `CommandEventArgs` instance est passée aux gestionnaires d’événements qui répondent à la `Command` événement et inclut deux propriétés d’information : `CommandArgument` et `CommandName`.

> [!NOTE]
> Pour plus d’informations sur la création, le déclenchement et la gestion des événements, consultez [événements et délégués](https://msdn.microsoft.com/library/17sde2xt.aspx) et [délégués d’événements en anglais Simple](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Pour définir un événement utilisent la syntaxe suivante :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Étant donné que nous avons besoin uniquement à la page de contenu d’alerte lorsque l’utilisateur a cliqué sur le `DoublePrice` bouton et n’avez pas besoin de transmettre à d’autres informations supplémentaires, nous pouvons utiliser le délégué d’événement `EventHandler`, qui définit un gestionnaire d’événements qui accepte en tant que son deuxième un objet de type de paramètre `System.EventArgs`. Pour créer l’événement dans la page maître, ajoutez la ligne de code suivante à la classe de code-behind de la page maître :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

Le code ci-dessus ajoute un événement public à la page maître nommée `PricesDoubled`. Nous devons maintenant déclencher cet événement une fois que les prix ont été doublés. Pour déclencher un événement utilisent la syntaxe suivante :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Où *expéditeur* et *eventArgs* sont les valeurs que vous souhaitez passer au gestionnaire d’événements de l’abonné.

Mise à jour le `DoublePrice` `Click` Gestionnaire d’événements par le code suivant :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Comme auparavant, le `Click` Gestionnaire d’événements démarre en appelant le `DoublePricesDataSource` du contrôle SqlDataSource `Update` méthode à doubler les prix de tous les produits. Après qu’il n’y a deux ajouts au gestionnaire d’événements. Tout d’abord, le `RecentProducts` les données du contrôle GridView sont actualisées. Ce GridView a été ajouté à la page maître dans le didacticiel précédent et affiche les cinq produits les plus récemment ajouté. Nous devons actualiser cette grille pour qu’il affiche les prix juste à double pour ces cinq produits. Ensuite, le `PricesDoubled` événement est déclenché. Une référence à la page maître proprement dit (`Me`) est envoyé au gestionnaire d’événements en tant que la source d’événements et les vide `EventArgs` objet est envoyé en tant que les arguments d’événement.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Étape 4 : Gérer l’événement dans la Page de contenu

À ce stade la page maître déclenche son `PricesDoubled` événement chaque fois que le `DoublePrice` contrôle bouton est activé. Toutefois, il s’agit uniquement démontrer : nous avons besoin gérer l’événement dans l’abonné. Cela implique deux étapes : création du Gestionnaire d’événements et l’ajout de code de câblage d’événement afin que lorsque l’événement est déclenché le Gestionnaire d’événements soit exécuté.

Commencez par créer un gestionnaire d’événements nommé `Master_PricesDoubled`. En raison de la façon dont nous avons défini le `PricesDoubled` événement dans la page maître paramètres d’entrée deux du Gestionnaire d’événements doivent être de types `Object` et `EventArgs`, respectivement. Dans l’appel de gestionnaire d’événements le `ProductsGrid` contrôle GridView `DataBind` méthode pour relier les données à la grille.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

Le code du Gestionnaire d’événements est terminé, mais il nous faut encore au fil de la page maître `PricesDoubled` événement à ce gestionnaire d’événements. L’abonné associe un événement à un gestionnaire d’événements par le biais de la syntaxe suivante :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*serveur de publication* est une référence à l’objet qui offre l’événement *eventName*, et *Nom_méthode* est le nom du Gestionnaire d’événements défini dans l’abonné.

Ce code de câblage d’événement doit être exécuté sur la première visite de page et les publications suivantes et doit se produire à un point dans le cycle de vie de page qui précède lorsque l’événement peut être déclenché. Un bon moment pour ajouter du code de câblage d’événement est dans la phase de PreInit, ce qui se produit très tôt dans le cycle de vie de page.

Ouvrez `~/Admin/Products.aspx` et créer un `Page_PreInit` Gestionnaire d’événements :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Pour pouvoir exécuter ce code de câblage nous avons besoin d’une référence de programmation à la page maître à partir de la page de contenu. Comme indiqué dans le didacticiel précédent, il existe deux manières de procéder :

- En castant le faiblement typé `Page.Master` propriété vers le type de page maître approprié, ou
- En ajoutant un `@MasterType` directive dans le `.aspx` page, puis en utilisant le fortement typée `Master` propriété.

Nous allons utiliser cette dernière approche. Ajoutez le code suivant `@MasterType` directive au début du balisage déclaratif de la page :


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Puis ajoutez le code de câblage d’événement suivant dans le `Page_PreInit` Gestionnaire d’événements :


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Avec ce code en place, le contrôle GridView dans la page de contenu est actualisé chaque fois que le `DoublePrice` bouton.

Les figures 8 et 9 illustrent ce comportement. La figure 8 illustre la page lors de la première visite. Notez que le prix des valeurs à la fois dans le `RecentProducts` GridView (dans la colonne de gauche de la page maître) et le `ProductsGrid` GridView (dans la page de contenu). Montre la figure 9 le même écran immédiatement après le `DoublePrice` a cliqué. Comme vous pouvez le voir, les nouveaux prix sont répercutées instantanément dans les deux contrôles GridView.


[![Les valeurs de prix Initial](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Figure 08**: Les valeurs initiales de prix ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Les prix Just-Doubled s’affichent dans les contrôles GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Figure 09**: Les prix Just-Doubled s’affichent dans les contrôles GridView ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Récapitulatif

Dans l’idéal, une page maître et ses pages de contenu sont complètement séparées les unes des autres et ne nécessitent aucun niveau d’interaction. Toutefois, si vous avez une page maître ou la page de contenu qui affiche les données qui peuvent être modifiées dans la page maître ou la page de contenu, puis vous devrez peut-être d’avoir à la page maître de la page de contenu (ou inversement a) d’alerte lorsque les données sont modifiées afin que l’affichage peut être mis à jour. Dans le didacticiel précédent, nous avons vu comment une page de contenu interagir par programmation avec sa page maître ; Dans ce didacticiel, nous avons vu comment faire en sorte d’une page maître d’initier l’interaction.

Tandis que les interactions par programme entre un contenu et de la page maître peuvent provenir de contenu ou la page maître, le modèle d’interaction utilisé dépend de l’origine. Les différences sont dû au fait qu’une page de contenu a une seule page maître, mais une page maître peut avoir plusieurs pages de contenu. Au lieu d’avoir une page maître interagissent directement avec une page de contenu, une meilleure approche est d’avoir à la page maître de déclencher un événement pour signaler qu’une action a eu lieu. Ces pages de contenu qui s’intéressent à l’action peuvent créer des gestionnaires d’événements.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [L’accès et la mise à jour des données dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Événements et délégués](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Informations de transmission entre le contenu et les Pages maîtres](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilisation des données dans les didacticiels ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Suchi Banerjee. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](interacting-with-the-master-page-from-the-content-page-vb.md)
> [Suivant](master-pages-and-asp-net-ajax-vb.md)
