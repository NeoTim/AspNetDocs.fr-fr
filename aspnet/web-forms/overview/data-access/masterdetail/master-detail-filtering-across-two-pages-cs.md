---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Filtrage maître/détail sur deux pages (C#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne de fournisseur dans le contrôle GridView contient une vie...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c7391c1ea7ef33febd4c2344cb40ac788a56034
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045024"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtrage maître/détail sur deux pages (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) ou [Télécharger le PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne de fournisseur dans le contrôle GridView contient un lien Afficher les produits qui, lorsque vous cliquez dessus, permet à l’utilisateur d’accéder à une page distincte qui répertorie les produits pour le fournisseur sélectionné.

## <a name="introduction"></a>Introduction

Dans les deux didacticiels précédents, nous avons vu comment [afficher des rapports maître/détail dans une page Web unique à l’aide de DropDownList](master-detail-filtering-with-a-dropdownlist-cs.md) pour [afficher les enregistrements « maîtres » et un contrôle GridView ou DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) pour afficher les détails. Un autre modèle courant utilisé pour les rapports maître/détails consiste à avoir les enregistrements maîtres sur une page Web et les détails affichés sur un autre. Un site Web de forum, comme [les Forums ASP.net](https://forums.asp.net/), est un excellent exemple de ce modèle dans la pratique. Les Forums ASP.NET sont composés de différents forums Prise en main, Web Forms, des contrôles de présentation des données, etc. Chaque forum est composé de nombreux threads et chaque thread se compose d’un certain nombre de publications. Sur la page d’accueil des forums ASP.NET, les forums sont répertoriés. Cliquer sur un forum vous `ShowForum.aspx` donne une page qui répertorie les threads de ce forum. De même, le fait de cliquer sur un thread vous amène à `ShowPost.aspx` , qui affiche les publications du thread sur lequel l’utilisateur a cliqué.

Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne de fournisseur dans le contrôle GridView contient un lien Afficher les produits qui, lorsque vous cliquez dessus, permet à l’utilisateur d’accéder à une page distincte qui répertorie les produits pour le fournisseur sélectionné.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Étape 1 : ajout `SupplierListMaster.aspx` des `ProductsForSupplierDetails.aspx` pages et au `Filtering` dossier

Lors de la définition de la mise en page dans le troisième didacticiel, nous avons ajouté plusieurs pages « Starter » dans les `BasicReporting` `Filtering` dossiers, et `CustomFormatting` . Toutefois, nous n’avons pas ajouté de page de démarrage pour ce didacticiel à ce moment-là, prenez un moment pour ajouter deux nouvelles pages au `Filtering` dossier : `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx` . `SupplierListMaster.aspx` répertorie les enregistrements « principaux » (fournisseurs) tout en `ProductsForSupplierDetails.aspx` affichant les produits du fournisseur sélectionné.

Lorsque vous créez ces deux nouvelles pages, veillez à les associer à la `Site.master` page maître.

![Ajouter les pages SupplierListMaster. aspx et ProductsForSupplierDetails. aspx au dossier de filtrage](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figure 1**: ajouter les `SupplierListMaster.aspx` `ProductsForSupplierDetails.aspx` pages et au `Filtering` dossier

En outre, lors de l’ajout de nouvelles pages au projet, veillez à mettre à jour le fichier de plan de site, `Web.sitemap` , en conséquence. Pour ce didacticiel, il vous suffit d’ajouter la `SupplierListMaster.aspx` page à la carte de site en utilisant le contenu XML suivant comme enfant de l’élément de filtrage des rapports `<siteMapNode>` :

[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Vous pouvez aider à automatiser le processus de mise à jour du fichier de plan de site lors de l’ajout de nouvelles pages ASP.NET à l’aide de la [macro plan de site](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)Visual Studio gratuite de [K. Scott Allen](http://odetocode.com/Blogs/scott/).

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Étape 2 : affichage de la liste des fournisseurs dans`SupplierListMaster.aspx`

Avec les `SupplierListMaster.aspx` `ProductsForSupplierDetails.aspx` pages et créées, l’étape suivante consiste à créer le contrôle GridView des fournisseurs dans `SupplierListMaster.aspx` . Ajoutez un GridView à la page et liez-le à un nouvel ObjectDataSource. Cet ObjectDataSource doit utiliser la `SuppliersBLL` méthode de la classe `GetSuppliers()` pour retourner tous les fournisseurs.

[![Sélectionnez la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figure 2**: sélectionner la `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image4.png))

[![Configurer ObjectDataSource pour utiliser la méthode GetSuppliers ()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figure 3**: configurer ObjectDataSource pour utiliser la `GetSuppliers()` méthode ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image7.png))

Nous devons inclure un lien intitulé Products View dans chaque ligne GridView qui, lorsque vous cliquez dessus, amène l’utilisateur à `ProductsForSupplierDetails.aspx` passer la valeur de la ligne sélectionnée `SupplierID` via la chaîne de chaîne. Par exemple, si l’utilisateur clique sur le lien Afficher les produits pour le fournisseur Tokyo Traders (qui a une `SupplierID` valeur de 4), il doit être envoyé à `ProductsForSupplierDetails.aspx?SupplierID=4` .

Pour ce faire, ajoutez un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) au GridView, qui ajoute un lien hypertexte à chaque ligne GridView. Commencez par cliquer sur le lien modifier les colonnes à partir de la balise active de GridView. Ensuite, sélectionnez le HyperLinkField dans la liste en haut à gauche, puis cliquez sur Ajouter pour inclure le HyperLinkField dans la liste de champs du contrôle GridView.

[![Ajouter un HyperLinkField au GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figure 4**: ajouter un HyperLinkField à GridView ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image10.png))

Le HyperLinkField peut être configuré pour utiliser les mêmes valeurs de texte ou d’URL que le lien dans chaque ligne GridView, ou peut baser ces valeurs sur les valeurs de données liées à chaque ligne particulière. Pour spécifier une valeur statique sur toutes les lignes, utilisez les `Text` Propriétés ou du HyperLinkField `NavigateUrl` . Étant donné que nous voulons que le texte du lien soit le même pour toutes les lignes, définissez la propriété de HyperLinkField `Text` sur Afficher les produits.

[![Définir la propriété Text de HyperLinkField pour afficher les produits](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figure 5**: définir la propriété de HyperLinkField `Text` pour afficher les produits ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image13.png))

Pour définir les valeurs de texte ou d’URL en fonction des données sous-jacentes liées à la ligne GridView, spécifiez les champs de données à partir desquels les valeurs de texte ou d’URL doivent être extraites dans les `DataTextField` `DataNavigateUrlFields` Propriétés ou. `DataTextField` peut uniquement être défini sur un champ de données unique ; `DataNavigateUrlFields`Toutefois, peut être défini sur une liste de champs de données délimités par des virgules. Nous devons souvent baser le texte ou l’URL sur une combinaison de la valeur du champ de données de la ligne actuelle et d’un balisage statique. Dans ce didacticiel, par exemple, nous voulons que l’URL des liens de HyperLinkField soit `ProductsForSupplierDetails.aspx?SupplierID=supplierID` , où correspond à la *`supplierID`* valeur row’s de chaque GridView `SupplierID` . Notez que nous avons besoin des valeurs statiques et pilotées par les données ici : la `ProductsForSupplierDetails.aspx?SupplierID=` partie de l’URL du lien est statique, tandis que la *`supplierID`* partie est pilotée par les données, car sa valeur correspond à la propre valeur de chaque ligne `SupplierID` .

Pour indiquer une combinaison de valeurs statiques et pilotées par les données, utilisez les `DataTextFormatString` `DataNavigateUrlFormatString` Propriétés et. Dans ces propriétés, entrez le balisage statique si nécessaire, puis utilisez le marqueur `{0}` dans lequel vous voulez que la valeur du champ spécifié dans les `DataTextField` `DataNavigateUrlFields` Propriétés ou apparaisse. Si `DataNavigateUrlFields` plusieurs champs sont spécifiés pour la propriété, utilisez l' `{0}` emplacement où vous souhaitez que la première valeur de champ soit insérée, `{1}` pour la deuxième valeur de champ, et ainsi de suite.

En appliquant cela à notre didacticiel, nous devons affecter `DataNavigateUrlFields` à la propriété la `SupplierID` valeur, car il s’agit du champ de données dont nous devons personnaliser la valeur en fonction de chaque ligne, et la `DataNavigateUrlFormatString` propriété à `ProductsForSupplierDetails.aspx?SupplierID={0}` .

[![Configurez le HyperLinkField de façon à inclure l’URL de lien appropriée basée sur RéfFournisseur.](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figure 6**: configurer HyperLinkField pour inclure l’URL de lien appropriée en fonction du `SupplierID` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image16.png))

Après avoir ajouté le HyperLinkField, n’hésitez pas à personnaliser et à réorganiser les champs du contrôle GridView. Le balisage suivant montre le GridView une fois que j’ai effectué des personnalisations mineures au niveau des champs.

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Prenez un moment pour afficher la `SupplierListMaster.aspx` page via un navigateur. Comme le montre la figure 7, la page répertorie actuellement tous les fournisseurs, y compris un lien Afficher les produits. Le fait de cliquer sur le lien Afficher les produits vous permet d’accéder à la `ProductsForSupplierDetails.aspx` chaîne de QueryString du fournisseur `SupplierID` .

[![Chaque ligne de fournisseur contient un lien Afficher les produits](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figure 7**: chaque ligne de fournisseur contient un lien Afficher les produits ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Étape 3 : énumération des produits du fournisseur dans`ProductsForSupplierDetails.aspx`

À ce stade `SupplierListMaster.aspx` , la page envoie des utilisateurs `ProductsForSupplierDetails.aspx` à, en passant le fournisseur sélectionné `SupplierID` dans la chaîne de chaîne. La dernière étape du didacticiel consiste à afficher les produits dans un GridView dans `ProductsForSupplierDetails.aspx` dont est `SupplierID` égal au `SupplierID` passé dans la chaîne de chaîne. Pour ce faire, ajoutez un GridView à la `ProductsForSupplierDetails.aspx` page, à l’aide d’un nouveau contrôle ObjectDataSource nommé `ProductsBySupplierDataSource` qui appelle la `GetProductsBySupplierID(supplierID)` méthode à partir de la `ProductsBLL` classe.

[![Ajouter un nouvel ObjectDataSource nommé ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figure 8**: ajouter un nouvel ObjectDataSource nommé `ProductsBySupplierDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image22.png))

[![Sélectionnez la classe ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figure 9**: sélectionner la `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image25.png))

[![Demander à ObjectDataSource d’appeler la méthode GetProductsBySupplierID (RéfFournisseur)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figure 10**: demander à ObjectDataSource d’appeler la `GetProductsBySupplierID(supplierID)` méthode ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image28.png))

L’étape finale de l’Assistant Configuration de la source de données nous invite à fournir la source du paramètre de la `GetProductsBySupplierID(supplierID)` méthode *`supplierID`* . Pour utiliser la valeur QueryString, définissez la source du paramètre sur QueryString et entrez le nom de la valeur QueryString à utiliser dans la zone de texte QueryStringField ( `SupplierID` ).

[![Renseignez la valeur du paramètre RéfFournisseur à partir de la valeur QueryString RéfFournisseur.](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figure 11**: remplir la *`supplierID`* valeur de paramètre à partir de la `SupplierID` valeur QueryString ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image31.png))

C’est tout ! La figure 12 affiche la `ProductsForSupplierDetails.aspx` page lorsqu’elle est visitée en cliquant sur le lien Tokyo traders à partir de `SupplierListMaster.aspx` .

[![Les produits fournis par Tokyo traders sont affichés](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figure 12**: les produits fournis par Tokyo traders sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Affichage des informations sur les fournisseurs dans`ProductsForSupplierDetails.aspx`

Comme le montre la figure 12, la `ProductsForSupplierDetails.aspx` page répertorie simplement les produits fournis par le `SupplierID` spécifié dans QueryString. Une personne directement envoyée à cette page ne sait pas que la figure 12 montre les produits de Tokyo Traders. Pour y remédier, nous pouvons également afficher des informations sur les fournisseurs dans cette page.

Commencez par ajouter un FormView au-dessus du GridView Products. Créez un nouveau contrôle ObjectDataSource nommé `SuppliersDataSource` qui appelle la `SuppliersBLL` méthode de la classe `GetSupplierBySupplierID(supplierID)` .

[![Sélectionnez la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figure 13**: sélectionner la `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image37.png))

[![Demander à ObjectDataSource d’appeler la méthode GetSupplierBySupplierID (RéfFournisseur)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figure 14**: demander à ObjectDataSource d’appeler la `GetSupplierBySupplierID(supplierID)` méthode ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image40.png))

Comme avec `ProductsBySupplierDataSource` , le paramètre doit être affecté à la valeur *`supplierID`* `SupplierID` QueryString.

[![Renseignez la valeur du paramètre RéfFournisseur à partir de la valeur QueryString RéfFournisseur.](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figure 15**: remplir la *`supplierID`* valeur de paramètre à partir de la `SupplierID` valeur QueryString ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image43.png))

Lors de la liaison du FormView au ObjectDataSource dans le Mode Création, Visual Studio crée automatiquement les `ItemTemplate` contrôles Web du FormView, `InsertItemTemplate` , et `EditItemTemplate` avec les contrôles Web Label et TextBox pour chacun des champs de données retournés par l’ObjectDataSource. Étant donné que nous voulons juste afficher les informations des fournisseurs, n’hésitez pas à supprimer le `InsertItemTemplate` et `EditItemTemplate` . Modifiez ensuite le ItemTemplate afin qu’il affiche le nom de la société du fournisseur dans un `<h3>` élément et l’adresse, la ville, le pays et le numéro de téléphone sous le nom de la société. Vous pouvez également définir manuellement les informations du FormView `DataSourceID` et créer le `ItemTemplate` balisage, comme nous l’avons fait dans le didacticiel «[affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)».

Une fois ces modifications apportées, le balisage déclaratif du FormView doit ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

La figure 16 illustre une capture d’écran de la `ProductsForSupplierDetails.aspx` page une fois que les informations sur le fournisseur détaillées ci-dessus ont été incluses.

[![La liste des produits comprend un résumé du fournisseur.](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figure 16**: la liste des produits comprend un résumé du fournisseur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Application des touches finales de l' `ProductsForSupplierDetails.aspx` interface utilisateur

Pour améliorer l’expérience utilisateur pour ce rapport, nous devons effectuer quelques ajouts à la `ProductsForSupplierDetails.aspx` page. Actuellement, la seule manière dont un utilisateur peut accéder à `ProductsForSupplierDetails.aspx` la liste des fournisseurs est de cliquer sur le bouton précédent du navigateur. Nous allons ajouter un contrôle de lien hypertexte à la `ProductsForSupplierDetails.aspx` page qui renvoie vers, ce qui permet `SupplierListMaster.aspx` à l’utilisateur de revenir à la liste principale.

[![Ajouter un contrôle de lien hypertexte pour reconnecter l’utilisateur à SupplierListMaster. aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figure 17**: ajouter un contrôle de lien hypertexte pour reconnecter l’utilisateur `SupplierListMaster.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image49.png))

Si l’utilisateur clique sur le lien Afficher les produits pour un fournisseur qui n’a pas de produits, le `ProductsBySupplierDataSource` ObjectDataSource dans `ProductsForSupplierDetails.aspx` ne retourne aucun résultat. Le GridView lié à ObjectDataSource n’affiche aucun balisage résultant dans une zone vide de la page dans le navigateur de l’utilisateur. Pour communiquer plus clairement à l’utilisateur qu’aucun produit n’est associé au fournisseur sélectionné, nous pouvons définir la propriété du contrôle GridView `EmptyDataText` sur le message que nous souhaitons afficher lorsqu’une telle situation se produit. J’ai défini cette propriété sur « aucun produit n’est fourni par ce fournisseur ».

Par défaut, tous les fournisseurs de la base de données Northwind fournissent au moins un produit. Toutefois, pour ce didacticiel, j’ai modifié manuellement la `Products` table afin que le fournisseur escargots nouveaux ne soit plus associé à aucun produit. La figure 18 affiche la page de détails de escargots nouveaux après cette modification.

[![Les utilisateurs sont informés que le fournisseur ne fournit aucun produit](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figure 18**: les utilisateurs sont informés que le fournisseur ne fournit aucun produit ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image52.png))

## <a name="summary"></a>Résumé

Bien que les rapports maître/détail puissent afficher à la fois les enregistrements maître et détail sur une seule page, dans de nombreux sites Web, ils sont séparés sur deux pages Web. Dans ce didacticiel, nous avons vu comment implémenter ce rapport maître/détail en faisant en sorte que les fournisseurs soient listés dans un contrôle GridView dans la page Web « master » et les produits associés figurant dans la page « Détails ». Chaque ligne de fournisseur dans la page Web principale contient un lien vers la page de détails qui a été transmise le long de la valeur de la ligne `SupplierID` . De tels liens spécifiques à la ligne peuvent être ajoutés facilement à l’aide de l’HyperLinkField du contrôle GridView.

Dans la page de détails, la récupération de ces produits pour le fournisseur spécifié a été effectuée en appelant la `ProductsBLL` méthode de la classe `GetProductsBySupplierID(supplierID)` . La *`supplierID`* valeur du paramètre a été spécifiée de manière déclarative à l’aide de QueryString comme source du paramètre. Nous avons également vu comment afficher les détails du fournisseur dans la page de détails à l’aide d’un FormView.

Le [prochain didacticiel](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) est le dernier dans les rapports maître/détail. Nous allons voir comment afficher une liste de produits dans un contrôle GridView où chaque ligne a un bouton Sélectionner. Le fait de cliquer sur le bouton Sélectionner affiche les détails de ce produit dans un contrôle DetailsView sur la même page.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à l’adresse [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à l’adresse [http://ScottOnWriting.NET](http://ScottOnWriting.NET) .

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, déposez-moi une ligne à l’adresse [ mitchell@4GuysFromRolla.com .](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-two-dropdownlists-cs.md) 
>  [Suivant](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
