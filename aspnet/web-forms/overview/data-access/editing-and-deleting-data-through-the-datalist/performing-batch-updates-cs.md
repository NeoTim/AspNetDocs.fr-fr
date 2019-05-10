---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Mise à jour par lots (c#) | Microsoft Docs
author: rick-anderson
description: Découvrez comment créer un entièrement modifiable DataList tous ses éléments où se trouvent dans modifier le mode et dont les valeurs peuvent être enregistrées en cliquant sur un bouton « Update All » sur le...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 01234dfab50cf608c934cb72ed06d0ad0ee58438
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65133636"
---
# <a name="performing-batch-updates-c"></a>Réalisation de mises à jour par lots (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) ou [télécharger le PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Découvrez comment créer un entièrement modifiable DataList tous ses éléments où se trouvent dans modifier le mode et dont les valeurs peuvent être enregistrées en cliquant sur un bouton « Tout mettre à jour » dans la page.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) nous avons examiné comment créer un contrôle DataList au niveau élément. Comme le GridView modifiable standard, chaque élément dans le contrôle DataList inclus une modification du bouton qui, quand vous cliquez sur, serait que l’élément puisse être modifié. Bien que cela au niveau élément édition fonctionne bien pour les données qui sont uniquement mis à jour occasionnellement, certains scénarios de cas d’utilisation oblige l’utilisateur à modifier le nombre d’enregistrements. Si un utilisateur a besoin de modifier des dizaines d’enregistrements et est obligé de modifier, rendre leurs modifications, puis cliquez sur la mise à jour pour chacune d’elles, la quantité d’un clic sur peut entraver sa productivité. Dans ce cas, une meilleure option consiste à fournir un contrôle DataList entièrement modifiable, celui dans lequel *tous les* de ses éléments sont en mode édition et dont les valeurs peuvent être modifiées en cliquant sur un bouton de tout mettre à jour sur la page (voir Figure 1).

[![Chaque élément dans un contrôle DataList modifiable entièrement peut être modifié.](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Figure 1**: Chaque élément dans un contrôle DataList modifiable complet peut être modifié ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-cs/_static/image3.png))

Dans ce didacticiel, nous allons examiner comment permettre aux utilisateurs de mettre à jour les informations d’adresse de fournisseurs à l’aide d’un contrôle DataList entièrement modifiable.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Étape 1 : Créer l’Interface utilisateur modifiable dans ItemTemplate s DataList

Dans le didacticiel précédent, où nous création d’un contrôle DataList modifiable standard, au niveau élément, nous avons utilisé deux modèles :

- `ItemTemplate` contenu de l’interface utilisateur en lecture seule (les contrôles Web Label pour afficher chaque nom de produit s et le prix).
- `EditItemTemplate` contenu de l’interface utilisateur du mode édition (les deux contrôles Web de la zone de texte).

Le contrôle DataList s `EditItemIndex` propriété dicte ce qui `DataListItem` (le cas échéant) est restitué en utilisant le `EditItemTemplate`. En particulier, le `DataListItem` dont `ItemIndex` valeur correspond à la DataList s `EditItemIndex` propriété est restituée à l’aide du `EditItemTemplate`. Ce modèle fonctionne bien lorsque qu’un seul élément peut être modifié à un moment, mais se situe les unes des autres lors de la création d’un contrôle DataList entièrement modifiable.

Pour un contrôle DataList entièrement modifiable, nous voulons *tous les* de le `DataListItem` s pour restituer à l’aide de l’interface modifiable. La façon la plus simple pour y parvenir consiste à définir l’interface modifiable dans le `ItemTemplate`. Pour modifier les informations d’adresse de fournisseurs, l’interface modifiable contient le nom du fournisseur en tant que texte, puis les zones de texte pour l’adresse, ville et les valeurs de pays.

Commencez par ouvrir le `BatchUpdate.aspx` page, ajoutez un contrôle DataList et définissez son `ID` propriété `Suppliers`. À partir de la balise active DataList s, choisir d’ajouter un nouveau contrôle ObjectDataSource nommé `SuppliersDataSource`.

[![Créer un nouveau ObjectDataSource nommé SuppliersDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Figure 2**: Créer une nouvelle nommée de ObjectDataSource `SuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-cs/_static/image6.png))

Configurer l’ObjectDataSource pour récupérer des données à l’aide de la `SuppliersBLL` classe s `GetSuppliers()` (méthode) (voir Figure 3). Comme du didacticiel précédent, plutôt que de la mise à jour les informations de fournisseur via ObjectDataSource, nous allons travailler directement avec la couche de logique métier. Par conséquent, définissez la liste déroulante (aucun) dans l’onglet de mise à jour (voir Figure 4).

[![Récupérer les informations de fournisseur à l’aide de la méthode GetSuppliers()](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Figure 3**: Récupérer les informations de fournisseur à l’aide du `GetSuppliers()` (méthode) ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-cs/_static/image9.png))

[![Définissez la liste déroulante (aucun) dans l’onglet de mise à jour](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Figure 4**: Définissez la liste déroulante (aucun) dans l’onglet de mise à jour ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-cs/_static/image12.png))

À l’issue de l’Assistant, Visual Studio génère automatiquement le contrôle DataList s `ItemTemplate` pour afficher chaque champ de données retourné par la source de données dans un contrôle Web Label. Nous devons modifier ce modèle pour qu’elle fournisse l’interface de modification à la place. Le `ItemTemplate` peuvent être personnalisés par le biais du concepteur à l’aide de l’option Modifier les modèles à partir de la balise active DataList s ou directement par le biais de la syntaxe déclarative.

Prenez un moment pour créer une interface d’édition qui affiche le nom du fournisseur s sous forme de texte, mais inclut des zones de texte pour le fournisseur s adresse, ville et les valeurs de pays. Après avoir apporté ces modifications, votre syntaxe déclarative s de page doit ressembler à ce qui suit :

[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Comme avec le didacticiel précédent, le contrôle DataList dans ce didacticiel doit avoir son état d’affichage est activé.

Dans le `ItemTemplate` je m à l’aide de deux nouvelles classes CSS, `SupplierPropertyLabel` et `SupplierPropertyValue`, qui ont été ajouté à la `Styles.css` classe et configuré pour utiliser les mêmes paramètres de style que le `ProductPropertyLabel` et `ProductPropertyValue` classes CSS.

[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Après avoir apporté ces modifications, visitez cette page via un navigateur. Comme le montre la Figure 5, chaque élément DataList affiche le nom du fournisseur au format texte et utilise des zones de texte pour afficher l’adresse, ville et pays.

[![Chaque fournisseur dans le contrôle DataList est modifiable](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Figure 5**: Chaque fournisseur dans le contrôle DataList est modifiable ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-cs/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Étape 2 : Ajout d’une mise à jour tout, bouton

Alors que chaque fournisseur dans la Figure 5 a son adresse, ville et des champs de pays affichés dans une zone de texte, il n’est actuellement aucun bouton de mise à jour disponible. Au lieu d’avoir un bouton Mettre à jour par élément, avec DataList entièrement modifiable il y a généralement un seul bouton tout mettre à jour dans la page qui, lorsque vous cliquez dessus, met à jour *tous les* des enregistrements dans le contrôle DataList. Pour ce didacticiel, permettent d’ajouter deux boutons tout mettre à jour - une en haut de la page et l’autre en bas (bien qu’en cliquant sur l’un des boutons aura le même effet) s.

Commencez par l’ajout d’un contrôle bouton Web ci-dessus les contrôles DataList et un ensemble son `ID` propriété `UpdateAll1`. Ensuite, ajoutez le deuxième contrôle de bouton Web sous le contrôle DataList, définissant son `ID` à `UpdateAll2`. Définir le `Text` propriétés pour les deux boutons à tout mettre à jour. Enfin, créez les gestionnaires d’événements pour les deux boutons `Click` événements. Au lieu de dupliquer la logique de mise à jour dans chacun des gestionnaires d’événements, s permettent de refactoriser cette logique à une troisième méthode, `UpdateAllSupplierAddresses`, avoir les gestionnaires d’événements simplement appeler cette méthode tiers.

[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Figure 6 illustre la page après que les mise à jour tous les boutons ont été ajoutés.

[![Deux boutons de toutes les mises à jour ont été ajoutées à la Page](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Figure 6**: Deux boutons de toutes les mises à jour ont été ajoutées à la Page ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-cs/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Étape 3 : La mise à jour toutes les informations d’adresse de fournisseurs

Avec tous les éléments de s DataList affichant l’interface de modification et l’ajout des mise à jour tous les boutons, il reste écrit le code pour effectuer la mise à jour par lots. Plus précisément, nous avons besoin effectuer une boucle dans les éléments du contrôle DataList s et appelez le `SuppliersBLL` classe s `UpdateSupplierAddress` méthode pour chacun d'entre eux.

La collection de `DataListItem` instances cette composition DataList est accessible via le contrôle DataList s [ `Items` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Avec une référence à un `DataListItem`, nous pouvons récupérer le correspondantes `SupplierID` à partir de la `DataKeys` collection et par programme de référence des contrôles Web de la zone de texte dans le `ItemTemplate` comme l’illustre le code suivant :

[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Lorsque l’utilisateur clique sur un des boutons tout mettre à jour, le `UpdateAllSupplierAddresses` méthode itère au sein de chaque `DataListItem` dans le `Suppliers` DataList et appelle le `SuppliersBLL` classe s `UpdateSupplierAddress` méthode, en passant les valeurs correspondantes. Une valeur non entré par l’adresse, ville ou pays passes est une valeur de `Nothing` à `UpdateSupplierAddress` (au lieu d’une chaîne vide), ce qui se traduit dans une base de données `NULL` pour les champs d’enregistrement s sous-jacent.

> [!NOTE]
> Comme une amélioration, vous souhaiterez ajouter un contrôle Web de l’étiquette d’état à la page qui fournit un message de confirmation une fois effectuée la mise à jour par lots.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>La mise à jour uniquement les adresses qui ont été modifiés

L’algorithme de mise à jour de lot utilisé pour les appels de ce didacticiel le `UpdateSupplierAddress` méthode pour *chaque* fournisseur dans le contrôle DataList, indépendamment de si leurs informations d’adresse a été changées. Bien que ces mises à jour aveugle ne sont pas généralement un problème de performances, elles peuvent entraîner des enregistrements superflus si vous faites l’audit change à la table de base de données. Par exemple, si vous utilisez des déclencheurs pour enregistrer tous les `UPDATE` s pour le `Suppliers` table à une table d’audit, chaque fois qu’un utilisateur clique sur le bouton tout mettre à jour un enregistrement d’audit sera créé pour chaque fournisseur dans le système, que si l’utilisateur effectué à chaque modifications.

Les classes ADO.NET DataTable et DataAdapter sont conçues pour prendre en charge les mises à jour par lots lorsque des enregistrements uniquement modifiés, supprimés et le nouveau conduit à toute communication de base de données. Chaque ligne de la table de données a un [ `RowState` propriété](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) qui indique si la ligne a été ajoutée au DataTable, supprimé, modifié, ou qu’il reste inchangée. Lorsqu’un DataTable est initialement renseigné, toutes les lignes sont marqués sans modification. Modification de la valeur d’une des colonnes de s de ligne de marque la ligne comme modifié.

Dans le `SuppliersBLL` classe nous mettre à jour les informations d’adresse de fournisseur spécifié s en première lecture dans l’enregistrement du fournisseur unique dans un `SuppliersDataTable` puis définissez la `Address`, `City`, et `Country` les valeurs de colonne en utilisant le code suivant :

[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Ce code assigne naïvement la passé dans adresse, ville et les valeurs de pays à la `SuppliersRow` dans le `SuppliersDataTable` , quel que soit ou non les valeurs ont changé. Ces modifications entraînent la `SuppliersRow` s `RowState` propriété marquée comme modifiée. Lors de la s de la couche d’accès aux données `Update` est appelée, il voit que le `SupplierRow` a été modifié et par conséquent envoie un `UPDATE` commande à la base de données.

Imaginez, cependant, que nous avons ajouté le code à cette méthode pour affecter uniquement le passé dans adresse, ville et les valeurs de pays s’ils diffèrent le `SuppliersRow` valeurs existantes de s. Dans le cas où l’adresse, ville et pays sont les mêmes que les données existantes, aucune modification ne sera apportée et la `SupplierRow` s `RowState` la mention de gauche reste la même. Le résultat net est que lors de la couche DAL s `Update` est appelée, aucun appel de la base de données ne sera passé, car le `SuppliersRow` n’a pas été modifié.

Pour mettre en œuvre cette modification, remplacez les instructions qui aveuglément affecter le passé dans l’adresse, ville et les valeurs de pays par le code suivant :

[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Avec cela ajouté du code, la couche DAL s `Update` méthode envoie un `UPDATE` instruction à la base de données pour que les enregistrements dont les valeurs liées aux adresses ont été modifiés.

Vous pouvez également nous aurions pu effectuer le suivi indique s’il existe des différences entre les champs d’adresse dans le passé et la base de données et, s’il en existe aucun, simplement ignorer l’appel à la couche DAL s `Update` (méthode). Cette approche fonctionne bien si vous utilisez la base de données de méthode, dirigez dans la mesure où la méthode directe de base de données n’est pas passé un `SuppliersRow` dont la propriété d’instance `RowState` peuvent être vérifiées pour déterminer si un appel de la base de données est réellement nécessaire.

> [!NOTE]
> Chaque fois que le `UpdateSupplierAddress` méthode est appelée, un appel est effectué à la base de données pour récupérer des informations sur l’enregistrement mis à jour. Ensuite, si des modifications sont apportées dans les données, un autre appel à la base de données est effectué pour mettre à jour de la ligne de table. Ce flux de travail peut être optimisée en créant un `UpdateSupplierAddress` surcharge de méthode qui accepte un `EmployeesDataTable` instance a *tous les* des modifications de la `BatchUpdate.aspx` page. Ensuite, il peut être un seul appel à la base de données pour obtenir tous les enregistrements à partir de la `Suppliers` table. Deux jeux de résultats peut ensuite être énumérée et uniquement les enregistrements où les modifications ont eu lieu peuvent être mis à jour.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment créer un contrôle DataList entièrement modifiable, qui permet à un utilisateur modifier rapidement les informations d’adresse pour plusieurs fournisseurs. Nous avons commencé en définissant un contrôle TextBox Web pour le fournisseur s adresse, ville et les valeurs de pays de l’interface de modification dans le contrôle DataList s `ItemTemplate`. Ensuite, nous avons ajouté la mise à jour tous les boutons au-dessus et au-dessous du contrôle DataList. Après un utilisateur a effectué ses modifications et cliqué sur un des boutons tout mettre à jour, le `DataListItem` s sont énumérées et un appel à la `SuppliersBLL` classe s `UpdateSupplierAddress` méthode est effectuée.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Zack Jones et Ken Pespisa. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [Suivant](handling-bll-and-dal-level-exceptions-cs.md)
