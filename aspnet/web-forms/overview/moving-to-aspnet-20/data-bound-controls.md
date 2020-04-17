---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Contrôles liés aux données (fr) Microsoft Docs
author: rick-anderson
description: La plupart des applications ASP.NET s’appuient sur un certain degré de présentation de données provenant d’une source de données back-end. Les contrôles liés aux données ont été un élément essentiel de l’interaction w ...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 941e2ed15b3da28991e7b06cbab570eb1b5b8899
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543078"
---
# <a name="data-bound-controls"></a>Contrôles liés aux données

par [Microsoft](https://github.com/microsoft)

> La plupart des applications ASP.NET s’appuient sur un certain degré de présentation de données provenant d’une source de données back-end. Les contrôles liés aux données ont joué un rôle essentiel dans l’interaction avec les données dans des applications Web dynamiques. ASP.NET 2.0 introduit des améliorations substantielles aux contrôles liés aux données, y compris une nouvelle classe BaseDataBoundControl et une syntaxe déclarative.

La plupart des applications ASP.NET s’appuient sur un certain degré de présentation de données provenant d’une source de données back-end. Les contrôles liés aux données ont joué un rôle essentiel dans l’interaction avec les données dans des applications Web dynamiques. ASP.NET 2.0 introduit des améliorations substantielles aux contrôles liés aux données, y compris une nouvelle classe BaseDataBoundControl et une syntaxe déclarative.

Le BaseDataBoundControl agit comme la classe de base pour la classe DataBoundControl et la classe HiérarchicalDataBoundControl. Dans ce module, nous discuterons des classes suivantes qui dérivent de DataBoundControl :

- AdRotator
- Contrôles de liste
- Affichage de grille
- Formview
- Detailsview

Nous discuterons également des classes suivantes qui découlent de la classe HiérarchicalDataBoundControl :

- TreeView
- Menu
- SiteMapPath (en)

## <a name="databoundcontrol-class"></a>Classe DataBoundControl

La classe DataBoundControl est une classe abstraite (marquée MustInherit en VB) utilisée pour interagir avec des données tabulaires ou de type liste. Les contrôles suivants sont quelques-uns des contrôles qui dérivent de DataBoundControl.

## <a name="adrotator"></a>AdRotator

Le contrôle AdRotator vous permet d’afficher une bannière graphique sur une page Web qui est liée à une URL spécifique. Le graphique qui est affiché est tourné en utilisant des propriétés pour le contrôle. La fréquence d’une annonce particulière s’présentant sur une page peut être configurée à l’aide de la propriété **Impressions** et les annonces peuvent être filtrées à l’aide du filtrage des mots clés.

Les contrôles AdRotator utilisent soit un fichier XML, soit une table dans une base de données pour les données. Les attributs suivants sont utilisés dans les fichiers XML pour configurer le contrôle AdRotator.

### <a name="imageurl"></a>ImageUrl
L’URL d’une image à afficher pour l’annonce.

### <a name="navigateurl"></a>NavigateUrl (en anglais)
L’URL à laquelle l’utilisateur doit être pris lorsque l’annonce est cliqué. Cela doit être codé par URL.

### <a name="alternatetext"></a>AlternateText (en)
Le texte alternatif qui est affiché dans un tooltip et lu par les lecteurs d’écran. Affiche également lorsque l’image spécifiée par ImageUrl n’est pas disponible.

### <a name="keyword"></a>Mot clé
Définit un mot clé qui peut être utilisé lors de l’utilisation du filtrage des mots clés. Si spécifié, seules les annonces avec un mot clé correspondant au filtre mot clé seront affichées.

### <a name="impressions"></a>Impressions
Un numéro de pondération qui détermine la fréquence à laquelle une annonce particulière est susceptible d’apparaître. Il est relatif à l’impression d’autres annonces dans le même fichier. La valeur maximale des impressions collectives pour toutes les annonces dans un fichier XML est de 2 048 000 000 1.

### <a name="height"></a>Hauteur
La hauteur de l’annonce en pixels.

### <a name="width"></a>Largeur
La largeur de l’annonce en pixels.

> [!NOTE]
> La hauteur et la largeur attribuent le remplacement de la hauteur et de la largeur pour le contrôle AdRotator lui-même.

Un fichier XML typique peut ressembler à ce qui suit :

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Dans l’exemple ci-dessus, l’annonce pour Contoso est deux fois plus susceptible d’apparaître que l’annonce pour le site Web ASP.NET en raison de la valeur de l’attribut Impressions.

Pour afficher les annonces à partir du fichier XML ci-dessus, ajoutez un contrôle AdRotator à une page et définissez la propriété **AdvertisementFile** pour indiquer le fichier XML tel qu’indiqué ci-dessous :

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si vous choisissez d’utiliser une table de base de données comme source de données pour votre contrôle AdRotator, vous devrez d’abord configurer une base de données à l’aide du schéma suivant :

| **Nom de la colonne** | **Type de données** | **Description** |
| --- | --- | --- |
| id | int | Clé primaire Cette colonne peut avoir n’importe quel nom. |
| ImageUrl | nvarchar (*longueur*) | L’URL relative ou absolue de l’image à afficher pour l’annonce. |
| NavigateUrl (en anglais) | nvarchar (*longueur*) | L’URL cible pour l’annonce. Si vous ne fournissez pas de valeur, l’annonce n’est pas un lien hypertexte. |
| AlternateText (en) | nvarchar (*longueur*) | Le texte affiché si l’image ne peut pas être trouvée. Dans certains navigateurs, le texte est affiché sous forme d’OutilTip. Le texte alternatif est également utilisé pour l’accessibilité de sorte que les utilisateurs qui ne peuvent pas voir le graphique peuvent entendre sa description lue à haute voix. |
| Mot clé | nvarchar (*longueur*) | Une catégorie pour l’annonce sur laquelle la page peut filtrer. |
| Impressions | int(4) | Un nombre qui indique la probabilité de la fréquence à laquelle l’annonce est affichée. Plus le nombre est élevé, plus l’annonce sera affichée. Le total de toutes les valeurs d’impression dans le fichier XML ne peut pas dépasser 2 048 000 000 - 1. |
| Largeur | int(4) | La largeur de l’image en pixels. |
| Hauteur | int(4) | La hauteur de l’image en pixels. |

Dans les cas où vous avez déjà une base de données avec un schéma différent, vous pouvez utiliser les propriétés **AlternateTextField**, **ImageUrlField**et **NavigateUrlField** pour cartographier les attributs AdRotator à votre base de données existante. Pour afficher les données de la base de données dans le contrôle AdRotator, ajoutez un contrôle de source de données à la page, configurez la chaîne de connexion pour le contrôle de source de données pour pointer vers votre base de données, et définissez la propriété **DataSourceID** du contrôle AdRotator à l’ID du contrôle de source de données. Dans les cas où vous avez besoin de configurer les annonces AdRotator programmatiquement, utilisez l’événement AdCreated. L’événement AdCreated prend deux paramètres; l’un un objet, et l’autre un exemple d’AdCreatedEventArgs. L’AdCreatedEventArgs est une référence à l’annonce qui est en cours de création.

L’extrait de code suivant définit l’ImageUrl, NavigateUrl et AlternateText pour une annonce programmatique :

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Contrôles de liste

Les contrôles de liste incluent la ListBox, DropDownList, CheckBoxList, RadioButtonList et BulletedList. Chacun de ces contrôles peut être lié à une source de données. Ils utilisent un champ dans la source de données comme texte d’affichage et peuvent utiliser optionnellement un deuxième champ comme valeur d’un élément. Les éléments peuvent également être ajoutés statiquement à l’heure de la conception, et vous pouvez mélanger des éléments statiques et des éléments dynamiques ajoutés à partir d’une source de données.

Pour lier les données d’un contrôle de liste, ajoutez un contrôle de source de données à la page. Spécifiez une commande SELECT pour le contrôle de source de données, puis définissez la propriété DataSourceID du contrôle de liste à l’ID du contrôle de source de données. Utilisez les propriétés **DataTextField** et **DataValueField** pour définir le texte d’affichage et la valeur du contrôle. En outre, vous pouvez utiliser la propriété **DataTextFormatString** pour contrôler l’apparence du texte d’affichage comme suit :

| **Expression** | **Description** |
| --- | --- |
| Prix:{0:C} | Pour les données numériques/décimales. Affiche le littéral "Prix:" suivi par les nombres en format de devise. Le format de la monnaie dépend du réglage de culture spécifié dans l’attribut culturel de la directive **Page** ou dans le fichier Web.config. |
| {0:D4} | Pour les données d’intégrant. Impossible d’utiliser avec des nombres décimaux. Les intégraux sont affichés dans un champ sans rembourrage qui est de quatre caractères de large. |
| {0:N2}% | Pour les données numériques. Affiche le nombre avec une précision de lieu décimale à 2 décimales suivie du « % » littéral. |
| {0:000.0} | Pour les données numériques/décimales. Les nombres sont arrondis à un endroit décimal. Les nombres de moins de trois chiffres sont remplis de zéros. |
| {0:D} | Pour les données de date/heure. Affiche le format long date (" Jeudi 06 août 1996 "). Le format de date dépend des paramètres de culture de la page ou du fichier Web.config. |
| {0:d} | Pour les données de date/heure. Affiche le format de date courte (« 31/12/99 »). |
| {0:yy-MM-dd} | Pour les données de date/heure. Date d’affichage en format numérique du mois de l’année (96-08-06) |

## <a name="gridview"></a>Affichage de grille

Le contrôle GridView permet l’affichage et l’édition de données tabulaires à l’aide d’une approche déclarative et est le successeur du contrôle DataGrid. Les fonctionnalités suivantes sont disponibles dans le contrôle GridView.

- Liaison aux contrôles de source de données, tels que SqlDataSource.
- Capacités de tri intégrées.
- Capacités intégrées de mise à jour et de suppression.
- Capacités de pagination intégrées.
- Capacités de sélection en rangée intégrées.
- Accès programmatique au modèle d’objet GridView pour définir dynamiquement les propriétés, gérer les événements, etc.
- Plusieurs champs clés.
- Plusieurs champs de données pour les colonnes hyperliens.
- Apparence personnalisable à travers les thèmes et les styles.

**Champs de colonnes**

Chaque colonne du contrôle GridView est représentée par un objet DataControlField. Par défaut, la propriété AutoGenerateColumns est définie comme **vraie,** ce qui crée un objet AutoGeneratedField pour chaque champ dans la source de données. Chaque champ est ensuite rendu comme une colonne dans le contrôle GridView dans l’ordre que chaque champ apparaît dans la source de données. Vous pouvez également contrôler manuellement les champs de colonnes qui apparaissent dans le contrôle **GridView** en plaçant la propriété **AutoGenerateColumns** à **faux,** puis en définissant votre propre collection de champ de colonne. Différents types de champ de colonne déterminent le comportement des colonnes dans le contrôle.

Le tableau suivant répertorie les différents types de champ de colonnes qui peuvent être utilisés.

| **Type de champ de colonne** | **Description** |
| --- | --- |
| BoundField | Affiche la valeur d’un champ dans une source de données. Il s’agit du type de colonne par défaut du contrôle GridView. |
| ButtonField | Affiche un bouton de commande pour chaque élément du contrôle GridView. Cela vous permet de créer une colonne de commandes de boutons personnalisées, telles que le bouton Ajouter ou supprimer. |
| CheckBoxField | Affiche une case à cocher pour chaque élément du contrôle GridView. Ce type de champ de colonne est couramment utilisé pour afficher des champs avec une valeur Boolean. |
| CommandField | Affiche des boutons de commande prédéfinis pour effectuer des opérations de sélection, d’édition ou de suppression. |
| HyperLinkField | Affiche la valeur d’un champ dans une source de données comme un lien hypertexte. Ce type de champ de colonne vous permet de lier un deuxième champ à l’URL de l’hyperl liaison. |
| ImageField | Affiche une image pour chaque élément dans le contrôle GridView. |
| TemplateField (en) | Affiche du contenu défini par l’utilisateur pour chaque élément du contrôle GridView selon un modèle spécifié. Ce type de champ de colonne vous permet de créer un champ de colonne personnalisé. |

Pour définir une collection de champ de colonne de façon déclarative, ajoutez d’abord des balises d’ouverture et de fermeture ** &lt;des colonnes&gt; ** entre les balises d’ouverture et de fermeture du contrôle GridView. Ensuite, énumérez les champs de colonne que vous souhaitez inclure entre les balises d’ouverture et de fermeture ** &lt;des colonnes.&gt; ** Les colonnes spécifiées sont ajoutées à la collection Colonnes dans l’ordre indiqué. La collection **Colonnes** stocke tous les champs de colonnes dans le contrôle et vous permet de gérer programmatiquement les champs de colonnes dans le contrôle GridView.

Les champs de colonnes explicitement déclarés peuvent être affichés en combinaison avec des champs de colonnes générés automatiquement. Lorsque les deux sont utilisés, les champs de colonnes explicitement déclarés sont rendus en premier, suivis par les champs de colonnes générés automatiquement.

## <a name="binding-to-data"></a>Liaison aux données

Le contrôle GridView peut être lié à un contrôle de source de données (tels que **SqlDataSource**, **ObjectDataSource**, et ainsi de suite), ainsi que toute source de données qui implémente l’interface System.Collections.IEnumerable (tels que System.DataView, System.Collections.ArrayList, ou System.Collections.Hashtable). Utilisez l’une des méthodes suivantes pour lier le contrôle GridView au type de source de données approprié :

- Pour vous lier à un contrôle de source de données, définissez la propriété DataSourceID du contrôle GridView à la valeur ID du contrôle de source de données. Le contrôle GridView se lie automatiquement au contrôle spécifié de la source de données et peut tirer parti des capacités du contrôle de la source de données pour effectuer des fonctionnalités de tri, de mise à jour, de suppression et de mise à jour. C’est la méthode préférée pour se lier aux données.
- Pour vous lier à une source de données qui implémente l’interface System.Collections.IEnumerable, définissez programmatiquement la propriété DataSource du contrôle GridView à la source de données, puis appelez la méthode DataBind. Lors de l’utilisation de cette méthode, le contrôle GridView ne fournit pas intégré de tri, mise à jour, suppression, et la fonctionnalité de poche. Vous devez fournir cette fonctionnalité vous-même.

## <a name="data-operations"></a>Opérations de données

Le contrôle GridView fournit de nombreuses fonctionnalités intégrées qui permettent à l’utilisateur de trier, mettre à jour, supprimer, sélectionner et page à travers les éléments du contrôle. Lorsque le contrôle GridView est lié à un contrôle de source de données, le contrôle GridView peut tirer parti des capacités du contrôle de la source de données et fournir des fonctionnalités de tri, de mise à jour et de suppression automatiques.

> [!NOTE]
> Le contrôle GridView peut fournir un soutien pour le tri, la mise à jour et la suppression avec d’autres types de sources de données; cependant, vous devrez fournir à un gestionnaire d’événement approprié la mise en œuvre de ces opérations.

Le tri permet à l’utilisateur de trier les éléments dans le contrôle GridView par rapport à une colonne spécifique en cliquant sur l’en-tête de la colonne. Pour permettre le tri, définissez la propriété AllowSorting à **vrai**.

Les fonctionnalités de mise à jour automatique, de suppression et de sélection sont activées lorsqu’un bouton dans un champ de colonne **ButtonField** ou **TemplateField,** avec un nom de commande de "Modifier", "Supprimer" et "Sélectionner", respectivement, est cliqué. Le contrôle GridView peut automatiquement ajouter un champ de colonne **CommandField** avec un bouton Modifier, supprimer ou sélectionner si la propriété AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateSelectButton est définie pour **être vraie,** respectivement.

> [!NOTE]
> L’insertion d’enregistrements dans la source de données n’est pas directement prise en charge par le contrôle GridView. Cependant, il est possible d’insérer des enregistrements en utilisant le contrôle GridView en conjonction avec le contrôle DetailsView ou FormView.

Au lieu d’afficher tous les enregistrements de la source de données en même temps, le contrôle GridView peut automatiquement décomposer les enregistrements en pages. Pour permettre le pagination, définissez la propriété AllowPaging à **vrai**.

## <a name="customizing-the-user-interface"></a>Personnaliser l’interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle GridView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété de style** | **Description** |
| --- | --- |
| AlternatingRowStyle | Les paramètres de style pour les lignes de données alternées dans le contrôle GridView. Lorsque cette propriété est définie, les lignes de données s’affichent en alternance entre les paramètres RowStyle et les paramètres **AlternatingRowStyle.** |
| EditRowStyle EditRowStyle | Les paramètres de style pour la ligne en cours d’édition dans le contrôle GridView. |
| EmptyDataRowStyle (en) | Les paramètres de style de la ligne de données vides affichés dans le contrôle GridView lorsque la source de données ne contient aucun enregistrement. |
| FooterStyle (FooterStyle) | Les réglages de style pour la rangée de pied du contrôle GridView. |
| HeaderStyle (en)à-tête | Les paramètres de style pour la ligne d’en-tête du contrôle GridView. |
| PagerStyle (PagerStyle) | Les paramètres de style pour la ligne de téléavertisseur du contrôle GridView. |
| RowStyle (RowStyle) | Les paramètres de style pour les lignes de données dans le contrôle GridView. Lorsque la propriété **AlternatingRowStyle** est également définie, les lignes de données s’affichent en alternance entre les paramètres **RowStyle** et les paramètres **AlternatingRowStyle.** |
| SelectedRowStyle | Les paramètres de style pour la ligne sélectionnée dans le contrôle GridView. |

Vous pouvez également montrer ou cacher différentes parties du contrôle. Le tableau suivant répertorie les propriétés qui contrôlent les pièces qui sont affichées ou cachées.

| **Propriété** | **Description** |
| --- | --- |
| ShowFooter ShowFooter | Affiche ou cache la section de pied du contrôle GridView. |
| ShowHeader ShowHeader | Affiche ou cache la section en-tête du contrôle GridView. |

### <a name="events"></a>Événements

Le contrôle GridView fournit plusieurs événements contre lequel vous pouvez programmer. Cela vous permet d’exécuter une routine personnalisée chaque fois qu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle GridView.

| **Événement** | **Description** |
| --- | --- |
| PageIndexMé | Se produit lorsque l’un des boutons de téléavertisseur est cliqué, mais après que le contrôle GridView gère l’opération de pagination. Cet événement est couramment utilisé lorsque vous avez besoin d’effectuer une tâche après que l’utilisateur navigue vers une page différente dans le contrôle. |
| PageIndexChanging | Se produit lorsque l’un des boutons de téléavertisseur est cliqué, mais avant que le contrôle GridView poignées de l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |
| RowCancelingEdit | Se produit lorsque le bouton Annuler d’une ligne est cliqué, mais avant que le contrôle GridView ne quitte le mode d’édition. Cet événement est souvent utilisé pour arrêter l’opération d’annulation. |
| LigneCommand | Se produit lorsqu’un bouton est cliqué dans le contrôle GridView. Cet événement est souvent utilisé pour effectuer une tâche lorsqu’un bouton est cliqué dans le contrôle. |
| RowCreated RowCreated RowCreated RowCre | Se produit quand une nouvelle ligne est créée dans le contrôle GridView. Cet événement est souvent utilisé pour modifier le contenu d’une ligne lorsque la ligne est créée. |
| RowDataBound | Se produit lorsqu’une ligne de données est liée aux données dans le contrôle GridView. Cet événement est souvent utilisé pour modifier le contenu d’une ligne lorsque la ligne est liée aux données. |
| RowDeleted RowDeleted RowDeleted RowDe | Se produit lorsque le bouton Supprimer d’une ligne est cliqué, mais après que le contrôle GridView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| RowDeleting RowDeleting RowDeleting RowDe | Se produit lorsque le bouton Supprimer d’une ligne est cliqué, mais avant que le contrôle GridView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| RowEditing RowEditing | Se produit lorsque le bouton Modifier d’une ligne est cliqué, mais avant que le contrôle GridView n’entre en mode d’édition. Cet événement est souvent utilisé pour annuler l’opération d’édition. |
| Rowupdated | Se produit lorsque le bouton de mise à jour d’une ligne est cliqué, mais après que le contrôle GridView met à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| Rowupdating | Se produit lorsque le bouton de mise à jour d’une ligne est cliqué, mais avant que le contrôle GridView met à jour la ligne. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| SelectedIndexChanged | Se produit lorsque le bouton Select d’une ligne est cliqué, mais après que le contrôle GridView s’en occupe, l’opération de sélection. Cet événement est souvent utilisé pour effectuer une tâche après une rangée est sélectionnée dans le contrôle. |
| SélectionIndexChanging | Se produit lorsque le bouton Select d’une ligne est cliqué, mais avant que le contrôle GridView ne gère l’opération sélectionnée. Cet événement est souvent utilisé pour annuler l’opération de sélection. |
| Triées | Se produit lorsque l’hyperlien pour trier une colonne est cliqué, mais après que le contrôle GridView gère le fonctionnement de tri. Cet événement est couramment utilisé pour effectuer une tâche après que l’utilisateur clique sur un hyperlien pour trier une colonne. |
| Tri | Se produit lorsque l’hyperlien pour trier une colonne est cliqué, mais avant que le contrôle GridView gère le fonctionnement de tri. Cet événement est souvent utilisé pour annuler l’opération de tri ou pour effectuer une routine de tri personnalisée. |

## <a name="formview"></a>Formview

Le contrôle FormView est utilisé pour afficher un seul enregistrement à partir d’une source de données. Il est similaire au contrôle DetailsView, sauf qu’il affiche des modèles définis par l’utilisateur au lieu de champs de rangées. La création de vos propres modèles vous donne une plus grande flexibilité dans le contrôle de la façon dont les données sont affichées. Le contrôle FormView prend en charge les fonctionnalités suivantes :

- Liaison aux contrôles de source de données, tels que SqlDataSource et ObjectDataSource.
- Capacités d’insertion intégrées.
- Capacités intégrées de mise à jour et de suppression.
- Capacités de pagination intégrées.
- Accès programmatique au modèle d’objet FormView pour définir dynamiquement les propriétés, gérer les événements, etc.
- Apparence personnalisable grâce à des modèles, des thèmes et des styles définis par l’utilisateur.

## <a name="templates"></a>Modèles

Pour que le contrôle FormView affiche du contenu, vous devez créer des modèles pour les différentes parties du contrôle. La plupart des modèles sont facultatifs; cependant, vous devez créer un modèle pour le mode dans lequel le contrôle est configuré. Par exemple, un contrôle FormView qui prend en charge l’insertion d’enregistrements doit avoir un modèle d’élément d’insertion défini. Le tableau suivant répertorie les différents modèles que vous pouvez créer.

| **Type de modèle** | **Description** |
| --- | --- |
| EditItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode d’édition. Ce modèle contient généralement des contrôles d’entrée et des boutons de commande avec lesquels l’utilisateur peut modifier un enregistrement existant. |
| EmptyDataTemplate (en) | Définit le contenu de la ligne de données vide affichée lorsque le contrôle FormView est lié à une source de données qui ne contient aucun enregistrement. Ce modèle contient généralement du contenu pour alerter l’utilisateur que la source de données ne contient pas d’enregistrements. |
| FooterTemplate | Définit le contenu de la ligne de pied. Ce modèle contient généralement tout contenu supplémentaire que vous souhaitez afficher dans la rangée de pied. Comme alternative, vous pouvez simplement spécifier le texte à afficher dans la rangée de pied en définissant la propriété FooterText. |
| HeaderTemplate (en) | Définit le contenu de la ligne d’en-tête. Ce modèle contient généralement tout contenu supplémentaire que vous souhaitez afficher dans la rangée d’en-têtes. Comme alternative, vous pouvez simplement spécifier le texte à afficher dans la rangée d’en-tête en définissant la propriété HeaderText. |
| ItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode lecture uniquement. Ce modèle contient généralement du contenu pour afficher les valeurs d’un enregistrement existant. |
| InsertItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode insertion. Ce modèle contient généralement des contrôles d’entrée et des boutons de commande avec lesquels l’utilisateur peut ajouter un nouvel enregistrement. |
| Pagertemplate | Définit le contenu de la ligne de téléavertisseur affiché lorsque la fonction de pagination est activée (lorsque la propriété AllowPaging est définie pour **vrai**). Ce modèle contient généralement des contrôles avec lesquels l’utilisateur peut naviguer vers un autre enregistrement. |

Les contrôles d’entrée dans le modèle d’élément de modification et le modèle d’élément d’insertion peuvent être liés aux champs d’une source de données en utilisant une expression contraignante bidirectionnel. Cela permet au contrôle FormView d’extraire automatiquement les valeurs du contrôle des entrées pour une mise à jour ou une opération d’insertion. Les expressions contraignantes bidirectionnels permettent également aux contrôles d’entrée dans un modèle d’élément de modification d’afficher automatiquement les valeurs de champ d’origine.

### <a name="binding-to-data"></a>Liaison aux données

Le contrôle FormView peut être lié à un contrôle de source de données (tels que **SqlDataSource**, AccessDataSource, **ObjectDataSource** et ainsi de suite), ou à toute source de données qui implémente l’interface System.Collections.IEnumerable (tels que System.Data.DataView, System.Collections.ArrayList, et System.Collections.Hashtable). Utilisez l’une des méthodes suivantes pour lier le contrôle FormView au type de source de données approprié :

- Pour vous lier à un contrôle de source de données, définissez la propriété DataSourceID du contrôle FormView à la valeur ID du contrôle de source de données. Le contrôle FormView se lie automatiquement au contrôle spécifié de source de données et peut tirer parti des capacités du contrôle de source de données pour effectuer des fonctionnalités d’insertion, de mise à jour, de suppression et de paging. C’est la méthode préférée pour se lier aux données.
- Pour vous lier à une source de données qui implémente l’interface **System.Collections.IEnumerable,** définissez programmatiquement la propriété DataSource du contrôle FormView à la source de données, puis appelez la méthode DataBind. Lors de l’utilisation de cette méthode, le contrôle FormView ne fournit pas intégré l’insertion, la mise à jour, la suppression et la fonctionnalité de mise en service. Vous devez fournir cette fonctionnalité en utilisant l’événement approprié.

## <a name="data-operations"></a>Opérations de données

Le contrôle FormView fournit de nombreuses fonctionnalités intégrées qui permettent à l’utilisateur de mettre à jour, supprimer, insérer et page à travers les éléments du contrôle. Lorsque le contrôle FormView est lié à un contrôle de source de données, le contrôle FormView peut tirer parti des capacités du contrôle de la source de données et fournir des fonctionnalités de mise à jour, de suppression, d’insertion et de mise à jour automatiques. Le contrôle FormView peut fournir un support pour les opérations de mise à jour, de suppression, d’insertion et de paging avec d’autres types de sources de données; toutefois, vous devez fournir à un gestionnaire d’événement approprié la mise en œuvre de ces opérations.

Étant donné que le contrôle FormView utilise des modèles, il ne fournit pas de moyen de générer automatiquement des boutons de commande pour effectuer des opérations de mise à jour, de suppression ou d’insertion. Vous devez inclure manuellement ces boutons de commande dans le modèle approprié. Le contrôle FormView reconnaît certains boutons dont les propriétés **CommandName** sont définies à des valeurs spécifiques. Le tableau suivant répertorie les boutons de commande que le contrôle FormView reconnaît.

| **Bouton** | **Valeur du nom de commandement** | **Description** |
| --- | --- | --- |
| Annuler | "Annuler" | Utilisé dans la mise à jour ou l’insertion des opérations pour annuler l’opération et de rejeter les valeurs saisies par l’utilisateur. Le contrôle FormView retourne ensuite au mode spécifié par la propriété DefaultMode. |
| DELETE | "Delete" | Utilisé dans la suppression des opérations pour supprimer l’enregistrement affiché de la source de données. Soulève les événements ItemDeleting et ItemDeleted. |
| Modifier | "Modifier" | Utilisé dans la mise à jour des opérations pour mettre le contrôle FormView en mode d’édition. Le contenu spécifié dans la propriété **EditItemTemplate** est affiché pour la ligne de données. |
| Insérer | "Insérer" | Utilisé dans l’insertion des opérations pour tenter d’insérer un nouvel enregistrement dans la source de données en utilisant les valeurs fournies par l’utilisateur. Soulève les événements ItemInserting et ItemInserted. |
| Nouveau | "Nouveau" | Utilisé dans l’insertion des opérations pour mettre le contrôle FormView en mode insérer. Le contenu spécifié dans la propriété **InsertItemTemplate** est affiché pour la ligne de données. |
| Page | "Page" | Utilisé dans les opérations de pagination pour représenter un bouton dans la rangée de téléavertisseur qui effectue le paaging. Pour spécifier l’opération de pagination, définissez la propriété **CommandArgument** du bouton à "Suivant", "Prev", "Premier", "Dernier", ou l’index de la page à laquelle naviguer. Soulève les événements PageIndexChanging et PageIndexChanged. |
| Update | "Mise à jour" | Utilisé dans la mise à jour des opérations pour tenter de mettre à jour l’enregistrement affiché dans la source de données avec les valeurs fournies par l’utilisateur. Soulève les événements ItemUpdating et ItemUpdated. |

Contrairement au bouton Supprimer (qui supprime immédiatement l’enregistrement affiché), lorsque le bouton Modifier ou nouveau est cliqué, le contrôle FormView passe en mode modifier ou insérer respectivement. En mode modification, le contenu contenu dans la propriété **EditItemTemplate** est affiché pour l’élément de données actuel. En règle générale, le modèle d’élément de modification est défini de telle sorte que le bouton Modifier est remplacé par une mise à jour et un bouton Annuler. Les contrôles d’entrée appropriés pour le type de données du champ (comme une TextBox ou un contrôle CheckBox) sont également généralement affichés avec la valeur d’un champ pour l’utilisateur à modifier. En cliquant sur le bouton Mise à jour, le bouton Mise à jour est l’enregistrement de la source de données, tout en cliquant sur le bouton Annuler abandonne toute modification.

De même, le contenu contenu contenu dans la propriété **InsertItemTemplate** est affiché pour l’élément de données lorsque le contrôle est en mode insérer. Le modèle d’élément d’insertion est généralement défini de telle sorte que le nouveau bouton est remplacé par un insert et un bouton Annuler, et des contrôles d’entrée vides sont affichés pour que l’utilisateur entre les valeurs du nouvel enregistrement. En cliquant sur le bouton Insert, l’enregistrement est disponible dans la source de données, tout en cliquant sur le bouton Annuler les modifications.

Le contrôle FormView fournit une fonction de pagination, qui permet à l’utilisateur de naviguer vers d’autres enregistrements dans la source de données. Lorsqu’il est activé, une ligne de téléavertisseur s’affiche dans le contrôle FormView qui contient les commandes de navigation de la page. Pour permettre le pagination, définissez la propriété **AllowPaging** à **vrai**. Vous pouvez personnaliser la ligne de téléavertisseur en définissant les propriétés des objets contenus dans la propriété PagerStyle et PagerSettings. Au lieu d’utiliser l’interface utilisateur intégrée de la rangée de téléavertisses, vous pouvez créer votre propre interface utilisateur en utilisant la propriété **PagerTemplate.**

## <a name="customizing-the-user-interface"></a>Personnaliser l’interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle FormView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété de style** | **Description** |
| --- | --- |
| EditRowStyle EditRowStyle | Les paramètres de style pour la ligne de données lorsque le contrôle FormView est en mode d’édition. |
| EmptyDataRowStyle (en) | Les paramètres de style de la ligne de données vides affichés dans le contrôle FormView lorsque la source de données ne contient aucun enregistrement. |
| FooterStyle (FooterStyle) | Les réglages de style pour la rangée de pied du contrôle FormView. |
| HeaderStyle (en)à-tête | Les paramètres de style pour la ligne d’en-tête du contrôle FormView. |
| InsertRowStyle (en) | Les paramètres de style pour la ligne de données lorsque le contrôle FormView est en mode insérer. |
| PagerStyle (PagerStyle) | Les paramètres de style pour la ligne de téléavertisseur affichés dans le contrôle FormView lorsque la fonction de pagination est activée. |
| RowStyle (RowStyle) | Les paramètres de style pour la ligne de données lorsque le contrôle FormView est en mode lecture uniquement. |

## <a name="events"></a>Événements

Le contrôle FormView fournit plusieurs événements contre lequel vous pouvez programmer. Cela vous permet d’exécuter une routine personnalisée chaque fois qu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle FormView.

| **Événement** | **Description** |
| --- | --- |
| ArticleCommand | Se produit lorsqu’un bouton dans un contrôle FormView est cliqué. Cet événement est souvent utilisé pour effectuer une tâche lorsqu’un bouton est cliqué dans le contrôle. |
| Itemcreated | Se produit après que tous les objets FormViewRow sont créés dans le contrôle FormView. Cet événement est souvent utilisé pour modifier les valeurs d’un enregistrement avant qu’il ne soit affiché. |
| Itemdeleted | Se produit lorsqu’un bouton Supprimer (un bouton avec sa propriété **CommandName** défini pour "Supprimer") est cliqué, mais après que le contrôle FormView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| ItemDeleting (en) | Se produit lorsqu’un bouton Supprimer est cliqué, mais avant que le contrôle FormView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| ArticleInserted | Se produit lorsqu’un bouton Insert (un bouton avec sa propriété **CommandName** réglé pour "Insert") est cliqué, mais après que le contrôle FormView insère l’enregistrement. Cet événement est souvent utilisé pour vérifier les résultats de l’opération d’insertion. |
| ArticlesInserting | Se produit lorsqu’un bouton Insert est cliqué, mais avant que le contrôle FormView insère l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération d’insertion. |
| ItemUpdated | Se produit lorsqu’un bouton de mise à jour (un bouton avec sa propriété **CommandName** réglé pour "Mise à jour") est cliqué, mais après que le contrôle FormView met à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| ItemUpdating (en) | Se produit lorsqu’un bouton de mise à jour est cliqué, mais avant que le contrôle FormView ne s’actualise. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| Mode Modifié | Se produit après que le contrôle FormView change de mode (pour modifier, insérer ou lire uniquement le mode). Cet événement est souvent utilisé pour effectuer une tâche lorsque le contrôle FormView change de mode. |
| ModeChanging (modeChanging) | Se produit avant que le contrôle FormView change de mode (pour modifier, insérer ou lire uniquement le mode). Cet événement est souvent utilisé pour annuler un changement de mode. |
| PageIndexMé | Se produit lorsque l’un des boutons de téléavertisseur est cliqué, mais après que le contrôle FormView gère l’opération de pagination. Cet événement est couramment utilisé lorsque vous avez besoin d’effectuer une tâche après que l’utilisateur navigue vers un enregistrement différent dans le contrôle. |
| PageIndexChanging | Se produit lorsque l’un des boutons de téléavertisseur est cliqué, mais avant que le contrôle FormView poignées de l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |

## <a name="detailsview"></a>Detailsview

Le contrôle DetailsView est utilisé pour afficher un seul enregistrement à partir d’une source de données dans une table, où chaque champ de l’enregistrement est affiché dans une rangée de la table. Il peut être utilisé en combinaison avec un contrôle GridView pour les scénarios de master-detail. Le contrôle DetailsView prend en charge les fonctionnalités suivantes :

- Liaison aux contrôles de source de données, tels que SqlDataSource.
- Capacités d’insertion intégrées.
- Capacités intégrées de mise à jour et de suppression.
- Capacités de pagination intégrées.
- Accès programmatique au modèle d’objet DetailsView pour définir dynamiquement les propriétés, gérer les événements, etc.
- Apparence personnalisable à travers les thèmes et les styles.

## <a name="row-fields"></a>Champs de rangée

Chaque ligne de données dans le contrôle DetailsView est créée en déclarant un contrôle sur le terrain. Différents types de champ de ligne déterminent le comportement des lignes dans le contrôle. Les commandes sur le terrain proviennent de DataControlField. Le tableau suivant répertorie les différents types de champ de ligne qui peuvent être utilisés.

| **Type de champ de colonne** | **Description** |
| --- | --- |
| BoundField | Affiche la valeur d’un champ dans une source de données sous forme de texte. |
| ButtonField | Affiche un bouton de commande dans le contrôle DetailsView. Cela vous permet d’afficher une ligne avec un contrôle de bouton personnalisé, comme un bouton Ajouter ou un bouton Supprimer. |
| CheckBoxField | Affiche une case à cocher dans le contrôle DetailsView. Ce type de champ de ligne est couramment utilisé pour afficher des champs avec une valeur Boolean. |
| CommandField | Affiche des boutons de commande intégrés pour effectuer des opérations de modification, d’insertion ou de suppression dans le contrôle DetailsView. |
| HyperLinkField | Affiche la valeur d’un champ dans une source de données comme un lien hypertexte. Ce type de champ de ligne vous permet de lier un deuxième champ à l’URL de l’hyperl liaison. |
| ImageField | Affiche une image dans le contrôle DetailsView. |
| TemplateField (en) | Affiche du contenu défini par l’utilisateur pour une ligne dans le contrôle DetailsView selon un modèle spécifié. Ce type de champ de ligne vous permet de créer un champ de ligne personnalisé. |

Par défaut, la propriété AutoGenerateRows est définie comme **vraie,** ce qui génère automatiquement un objet de champ de ligne lié pour chaque champ d’un type liant dans la source de données. Les types liants valides sont String, DateTime, Decimal, Guid, et l’ensemble des types primitifs. Chaque champ est ensuite affiché dans une rangée sous forme de texte, dans l’ordre dans lequel chaque champ apparaît dans la source de données.

Générer automatiquement les lignes fournit un moyen rapide et facile d’afficher tous les champs de l’enregistrement. Toutefois, pour utiliser les capacités avancées du contrôle DetailsView, vous devez déclarer explicitement les champs de ligne à inclure dans le contrôle DetailsView. Pour déclarer les champs de ligne, d’abord mis la propriété **AutoGenerateRows** à **faux**. Ensuite, ajoutez des balises ** &lt;Fields&gt; ** d’ouverture et de fermeture entre les balises d’ouverture et de fermeture du contrôle DetailsView. Enfin, énumérez les champs de ligne que vous souhaitez inclure entre les balises ** &lt;Fields&gt; ** d’ouverture et de fermeture. Les champs de ligne spécifiés sont ajoutés à la collection Fields dans l’ordre indiqué. La collection **Fields** vous permet de gérer de manière programmatique les champs de lignes dans le contrôle DetailsView.

> [!NOTE]
> Les champs de rangées générés automatiquement ne sont pas ajoutés à la collection Fields.

## <a name="binding-to-data"></a>Liaison aux données

Le contrôle DetailsView peut être lié à un contrôle de source de données, tels que **SqlDataSource** ou AccessDataSource, ou à toute source de données qui implémente l’interface System.Collections.IEnumerable, telles que System.DataView, System.Collections.ArrayList et System.Collections.Hashtable.

Utilisez l’une des méthodes suivantes pour lier le contrôle DetailsView au type de source de données approprié :

- Pour vous lier à un contrôle de source de données, définissez la propriété DataSourceID du contrôle DetailsView à la valeur ID du contrôle de source de données. Le contrôle DetailsView se lie automatiquement au contrôle spécifié de source de données. C’est la méthode préférée pour se lier aux données.
- Pour vous lier à une source de données qui implémente l’interface **System.Collections.IEnumerable,** définissez programmatiquement la propriété DataSource du contrôle DetailsView à la source de données, puis appelez la méthode DataBind.

## <a name="security"></a>Sécurité

Ce contrôle peut être utilisé pour afficher l’entrée de l’utilisateur, qui peut inclure le script malveillant du client. Vérifiez toute information envoyée par un client pour obtenir un script exécutable, des relevés SQL ou tout autre code avant de les afficher dans votre demande. ASP.NET fournit une fonction de validation de demande d’entrée pour bloquer le script et LE HTML dans l’entrée de l’utilisateur.

## <a name="data-operations"></a>Opérations de données

Le contrôle DetailsView fournit des fonctionnalités intégrées qui permettent à l’utilisateur de mettre à jour, supprimer, insérer et page à travers les éléments du contrôle. Lorsque le contrôle DetailsView est lié à un contrôle de source de données, le contrôle DetailsView peut tirer parti des capacités du contrôle de la source de données et fournir des fonctionnalités de mise à jour, de suppression, d’insertion et de mise à jour automatiques.

Le contrôle DetailsView peut fournir un support pour les opérations de mise à jour, de suppression, d’insertion et de paging avec d’autres types de sources de données ; toutefois, vous devez fournir la mise en œuvre de ces opérations dans un gestionnaire d’événements approprié.

Le contrôle DetailsView peut automatiquement ajouter un champ de ligne **CommandField** avec un bouton Edit, Supprimer ou Nouveau en définissant les propriétés AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateInsertButton, **true**respectivement. Contrairement au bouton Supprimer (qui supprime immédiatement l’enregistrement sélectionné), lorsque le bouton Modifier ou nouveau est cliqué, le contrôle DetailsView passe en mode modifier ou insérer, respectivement. En mode modification, le bouton Modifier est remplacé par une mise à jour et un bouton Annuler. Les contrôles d’entrée appropriés au type de données du champ (comme une TextBox ou un contrôle CheckBox) sont affichés avec la valeur d’un champ que l’utilisateur doit modifier. En cliquant sur le bouton Mise à jour, le bouton Mise à jour est l’enregistrement de la source de données, tout en cliquant sur le bouton Annuler abandonne toute modification. De même, en mode insérer, le nouveau bouton est remplacé par un insert et un bouton Annuler, et des contrôles d’entrée vides sont affichés pour que l’utilisateur entre les valeurs du nouvel enregistrement.

Le contrôle DetailsView fournit une fonction de pagination, qui permet à l’utilisateur de naviguer vers d’autres enregistrements dans la source de données. Lorsqu’elles sont activées, les commandes de navigation de page s’affichent dans une rangée de téléavertisseur. Pour permettre le pagination, définissez la propriété AllowPaging à **vrai**. La ligne de téléavertisseur peut être personnalisée à l’aide des propriétés PagerStyle et PagerSettings.

## <a name="customizing-the-user-interface"></a>Personnaliser l’interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle DetailsView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété de style** | **Description** |
| --- | --- |
| AlternatingRowStyle | Les paramètres de style pour les lignes de données alternées dans le contrôle DetailsView. Lorsque cette propriété est définie, les lignes de données s’affichent en alternance entre les paramètres RowStyle et les paramètres **AlternatingRowStyle.** |
| CommandRowStyle (en) | Les paramètres de style pour la ligne contenant les boutons de commande intégrés dans le contrôle DetailsView. |
| EditRowStyle EditRowStyle | Les paramètres de style pour les lignes de données lorsque le contrôle DetailsView est en mode d’édition. |
| EmptyDataRowStyle (en) | Les paramètres de style de la ligne de données vides affichés dans le contrôle DetailsView lorsque la source de données ne contient aucun enregistrement. |
| FooterStyle (FooterStyle) | Les réglages de style pour la ligne de pied du contrôle DetailsView. |
| HeaderStyle (en)à-tête | Les paramètres de style pour la ligne d’en-tête du contrôle DetailsView. |
| InsertRowStyle (en) | Les paramètres de style pour les lignes de données lorsque le contrôle DetailsView est en mode insérer. |
| PagerStyle (PagerStyle) | Les paramètres de style pour la ligne de téléavertisseur du contrôle DetailsView. |
| RowStyle (RowStyle) | Les paramètres de style pour les lignes de données dans le contrôle DetailsView. Lorsque la propriété **AlternatingRowStyle** est également définie, les lignes de données s’affichent en alternance entre les paramètres **RowStyle** et les paramètres **AlternatingRowStyle.** |
| FieldHeaderStyle (en) | Les paramètres de style pour la colonne d’en-tête du contrôle DetailsView. |

## <a name="events"></a>Événements

Le contrôle DetailsView fournit plusieurs événements contre lequel vous pouvez programmer. Cela vous permet d’exécuter une routine personnalisée chaque fois qu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle DetailsView. Le contrôle DetailsView hérite également de ces événements de ses classes de base : DataBinding, DataBound, Disposed, Init, Load, PreRender et Render.

| **Événement** | **Description** |
| --- | --- |
| ArticleCommand | Se produit lorsqu’un bouton est cliqué dans le contrôle DetailsView. |
| Itemcreated | Se produit après que tous les objets DetailsViewRow sont créés dans le contrôle DetailsView. Cet événement est souvent utilisé pour modifier les valeurs d’un enregistrement avant qu’il ne soit affiché. |
| Itemdeleted | Se produit lorsqu’un bouton Supprimer est cliqué, mais après que le contrôle DetailsView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| ItemDeleting (en) | Se produit lorsqu’un bouton Supprimer est cliqué, mais avant que le contrôle De DetailsView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| ArticleInserted | Se produit lorsqu’un bouton Insert est cliqué, mais après que le contrôle DetailsView insère l’enregistrement. Cet événement est souvent utilisé pour vérifier les résultats de l’opération d’insertion. |
| ArticlesInserting | Se produit lorsqu’un bouton Insert est cliqué, mais avant que le contrôle DetailsView insère l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération d’insertion. |
| ItemUpdated | Se produit quand un bouton de mise à jour est cliqué, mais après que le contrôle DetailsView met à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| ItemUpdating (en) | Se produit lorsqu’un bouton de mise à jour est cliqué, mais avant que le contrôle DetailsView ne s’actualise, l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| Mode Modifié | Se produit après que le contrôle De DetailsView change les modes (modifier, insérer ou lire uniquement le mode). Cet événement est souvent utilisé pour effectuer une tâche lorsque le contrôle DetailsView change de mode. |
| ModeChanging (modeChanging) | Se produit avant que le contrôle DetailsView change les modes (modifier, insérer ou lire uniquement le mode). Cet événement est souvent utilisé pour annuler un changement de mode. |
| PageIndexMé | Se produit lorsque l’un des boutons de téléavertisseur est cliqué, mais après que le contrôle DetailsView gère l’opération de pagination. Cet événement est couramment utilisé lorsque vous avez besoin d’effectuer une tâche après que l’utilisateur navigue vers un enregistrement différent dans le contrôle. |
| PageIndexChanging | Se produit lorsque l’un des boutons de téléavertisseur est cliqué, mais avant que le contrôle DetailsView gère l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |

## <a name="the-menu-control"></a>Le contrôle du menu

Le contrôle du menu dans ASP.NET 2.0 est conçu pour être un système de navigation complet. Il peut être facilement relié à des données hiérarchiques telles que le SiteMapDataSource.

Une structure de contrôle de menu peut être définie de façon déclarative ou dynamique et se compose d’un nœud à racines unique et d’un certain nombre de sous-nœuds. Le code suivant définit de façon déclarativement un menu pour le contrôle du menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Dans l’exemple ci-dessus, le nœud Home.aspx est le nœud racine. Tous les autres nœuds sont imbriqués dans le nœud de racine à différents niveaux.

Il existe deux types de menus que le contrôle du menu peut rendre; menus statiques et menus dynamiques. Les menus statiques se composent d’éléments de menu qui sont toujours visibles. Les menus dynamiques se composent d’éléments de menu qui ne sont visibles que lorsque l’utilisateur plane sur eux avec la souris. Les clients peuvent souvent confondre les menus statiques avec des menus définis de façon déclarative et dynamique avec des menus qui sont disponibles à l’heure de l’exécution. En fait, les menus dynamiques et statiques n’ont rien à voir avec la méthode de population. Les termes *statiques* et *dynamiques* ne se réfèrent qu’à la question de savoir si le menu est affiché de façon statique par défaut ou seulement affiché lorsque l’utilisateur prend des mesures.

La propriété **StaticDisplayLevels** est utilisée pour configurer le nombre de niveaux du menu sont statiques et donc affichés par défaut. Dans l’exemple ci-dessus, le fait de définir la propriété **StaticDisplayLevels** à une valeur de 2 ferait en sorte que le menu afficherait statiquement le nœud à la maison, le nœud de musique et le nœud Films. Tous les autres nœuds s’afficheraient dynamiquement lorsque l’utilisateur plane au-dessus du nœud parent.

La propriété **MaximumDynamicDisplayLevels** configure le nombre maximum de niveaux dynamiques que le menu est capable d’afficher. Tous les menus dynamiques à un niveau supérieur à la valeur spécifiée par la propriété **MaximumDynamicDisplayLevels** sont jetés.

> [!NOTE]
> Il est presque certain que vous pourriez rencontrer des situations où les menus ne semblent pas rendre en raison de la propriété MaximumDynamicDisplayLevels. Dans ces cas, assurez-vous que la propriété est suffisamment définie pour permettre l’affichage des menus des clients.

## <a name="data-binding-the-menu-control"></a>Données Liant le contrôle du menu

Le contrôle du menu peut être lié à n’importe quelle source de données hiérarchiques telles que le SiteMapDataSource ou le XMLDataSource. Le SiteMapDataSource est la méthode la plus couramment utilisée pour la liaison de données à un contrôle de menu parce qu’il se nourrit du fichier Web.sitemap et son schéma fournit une API connue pour le contrôle du menu. La liste ci-dessous affiche un fichier Web.sitemap simple.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Notez qu’il n’y a qu’un seul site racineMapNode élément, dans ce cas, l’élément Accueil. Plusieurs attributs peuvent être configurés pour chaque siteMapNode. Les attributs les plus couramment utilisés sont :

- **URL** Spécifie l’URL pour afficher lorsqu’un utilisateur clique sur l’élément du menu. Si cet attribut n’est pas présent, le nœud se contente de publier en arrière lorsqu’il est cliqué.
- **titre** Spécifie le texte qui s’affiche sur l’élément menu.
- **description** Utilisé comme documentation pour le nœud. Affiche également comme une pointe d’outil lorsque la souris est planée au-dessus du nœud.
- **siteMapFile** Permet les emplacements imbriqués. Cet attribut doit indiquer un fichier de carte de site ASP.NET bien formé.
- **rôles** Permet de contrôler l’apparence d’un nœud par ASP.NET l’élagage de sécurité.

Notez que si ces attributs sont tous facultatifs, le comportement du menu peut ne pas être ce qui est attendu si elles ne sont pas spécifiées. Par exemple, si l’attribut *d’URL* est spécifié mais que l’attribut *de description* ne l’est pas, le nœud ne sera pas visible et il n’y aura aucun moyen de naviguer vers l’URL spécifiée.

## <a name="controlling-a-menus-operation"></a>Contrôle d’une opération Menus

Il existe plusieurs propriétés qui affectent le fonctionnement d’un ASP.NET contrôle du menu; la propriété **Orientation,** la propriété **DisappearAfter,** la propriété **StaticItemFormatString,** et la propriété **StaticPopoutImageUrl** ne sont que quelques-uns d’entre eux.

- **L’orientation** peut être définie *horizontalement* ou *verticale* et contrôle si les éléments statiques du menu sont disposés horizontalement dans une rangée ou verticalement et empilés les uns sur les autres. Cette propriété n’affecte pas les menus dynamiques.
- La propriété **DisappearAfter** configure combien de temps un menu dynamique doit rester visible après que la souris a été déplacée loin de lui. La valeur est spécifiée en millisecondes et par défaut à 500. La définition de cette propriété à une valeur de -1 fera en sorte que le menu ne disparaîtra jamais automatiquement. Dans ce cas, le menu ne disparaîtra que lorsque l’utilisateur clique en dehors du menu.
- La propriété **StaticItemFormatString** facilite le maintien d’un verbiage cohérent dans votre système de menu. Lors de la *{0}* spécifier cette propriété, doit être entré à la place de la description qui apparaît dans la source de données. Par exemple, afin d’avoir l’élément de menu de l’exercice 1 {0} dire Visitez notre page de produits, etc, vous spécifiez Visitez notre page pour le StaticItemFormatString. À l’heure d’exécution, ASP.NET {0} remplacera toute occurrence de la description correcte pour l’élément de menu.
- La propriété **StaticPopoutImageUrl** spécifie l’image utilisée pour indiquer qu’un nœud de menu particulier a des nœuds d’enfant qui peuvent être consultés en planant au-dessus. Les menus dynamiques continueront d’utiliser l’image par défaut.

## <a name="templated-menu-controls"></a>Contrôles de menu modélistes

Le contrôle du menu est un contrôle modélé et permet deux ItemTemplates différents; la StaticItemTemplate et la DynamicItemTemplate. À l’aide de ces modèles, vous pouvez facilement ajouter des commandes de serveur ou des contrôles utilisateur à vos menus.

Pour modifier les modèles de Visual Studio .NET, cliquez sur le bouton Smart Tag sur le menu et choisissez les modèles d’édition. Vous pouvez ensuite choisir entre l’édition de la StaticItemTemplate ou la DynamicItemTemplate.

Tous les contrôles ajoutés à la StaticItemTemplate apparaîtront dans le menu statique lorsque la page se charge. Tous les contrôles ajoutés au DynamicItemTemplate apparaîtront sur tous les menus pop-up.

## <a name="menu-events"></a>Menus Événements

Le contrôle du menu a deux événements qui lui sont propres; **menuItemClicked** et l’événement **MenuItemDatabound.**

L’événement MenuItemClicked est soulevé lorsqu’un élément de menu est cliqué. L’événement MenuItemDatabound est soulevé lorsqu’un élément de menu est relié aux données. Le **MenuEventArgs** qui est passé au gestionnaire de l’événement donne accès à l’élément de menu via la propriété Article.

## <a name="controlling-a-menus-appearance"></a>Contrôle d’une apparence de menus

Vous pouvez également affecter l’apparence d’un contrôle de menu en utilisant un ou plusieurs des nombreux styles disponibles pour les menus de format. Parmi ceux-ci sont **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**, et **DynamicHoverStyle**. Ces propriétés sont configurées à l’aide d’une chaîne HTML Style standard. Par exemple, ce qui suit affectera le style des menus dynamiques.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si vous utilisez l’un des styles Hover, &lt;&gt; vous devrez ajouter un élément de tête à la page avec l’élément *runat* réglé sur *le serveur*.

Les contrôles de menu prennent également en charge l’utilisation de ASP.NET thèmes 2.0.

## <a name="the-treeview-control"></a>Le contrôle TreeView

Le contrôle TreeView affiche des données dans une structure ressemblant à un arbre. Comme avec le contrôle du menu, il peut être facilement des données liées à n’importe quelle source de données hiérarchiques telles que le SiteMapDataSource.

La première question que les clients sont susceptibles de poser au sujet du contrôle TreeView dans ASP.NET 2.0 est de savoir si oui ou non il est lié à l’ArbreView IE WebControl qui était disponible pour ASP.NET 1.x. Ce n’est pas le cas. Le ASP.NET 2.0 TreeView contrôle a été écrit à partir de zéro et offre une amélioration significative par rapport à l’IE TreeView WebControl qui était auparavant disponible.

Je n’entrerai pas dans les détails sur la façon de lier un contrôle TreeView à une carte du site car il est effectué exactement de la même manière que le contrôle du menu. Cependant, le contrôle TreeView a quelques différences distinctes dans la façon dont il fonctionne.

Par défaut, un contrôle TreeView apparaît entièrement élargi. Pour modifier le niveau d’expansion lors de la charge initiale, modifiez la propriété **ExpandDepth** du contrôle. Ceci est particulièrement important dans les cas où le TreeView est lié aux données sur l’expansion des nœuds particuliers.

## <a name="databinding-the-treeview-control"></a>DataBinding the TreeView Control

Contrairement au contrôle du menu, l’TreeView se prête bien à la manipulation de grandes quantités de données. Par conséquent, en plus de databinding à un SiteMapDataSource ou XMLDataSource, l’TreeView est souvent des données liées à un DataSet ou d’autres données relationnelles. Dans les cas où le contrôle TreeView est lié à de grandes quantités de données, il est préférable de se lier uniquement aux données qui sont réellement visibles dans le contrôle. Vous pouvez ensuite les données se lier à des données supplémentaires au fur et à mesure que les nœuds TreeView sont élargis.

Dans ces cas, la propriété **PopulateOnDemand** de l’TreeView devrait être définie pour être mise en *œil*. Vous devrez ensuite fournir une implémentation pour la méthode **TreeNodePopulate.**

## <a name="data-binding-without-postback"></a>Liaison de données sans postback

Notez que lorsque vous élargissez un nœud dans l’exemple précédent pour la première fois, la page s’affiche et se rafraîchit. Ce n’est pas un problème dans cet exemple, mais vous pouvez imaginer qu’il pourrait être dans un environnement de production avec une grande quantité de données. Un meilleur scénario serait celui dans lequel le TreeView serait encore dynamiquement peupler ses nœuds, mais sans un poste de retour sur le serveur.

En mettant en valeur les propriétés **PopulateNodesDeCromClient** et **PopulateOnDemand,** le contrôle ASP.NET TreeView peuplera dynamiquement les nœuds sans poteau arrière. Lorsque le nœud parent est élargi, une demande XMLHttp est faite auprès du client et l’événement OnTreeNodePopulate est congédié. Le serveur répond avec une île de données XML qui est ensuite utilisé pour les données lier les nœuds enfant.

ASP.NET crée dynamiquement le code client qui implémente cette fonctionnalité. Les &lt;&gt; balises de script qui contiennent le script sont générées pointant vers un fichier AXD. Par exemple, la liste ci-dessous affiche les liens de script pour le code de script qui génère la demande XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Si vous naviguez le fichier AXD ci-dessus dans votre navigateur et l’ouvrez, vous verrez le code qui implémente la demande XMLHttp. Cette méthode empêche les clients de modifier le fichier de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Contrôle de l’opération du contrôle TreeView

Le contrôle TreeView a plusieurs propriétés qui affectent le fonctionnement du contrôle. Les propriétés les plus évidentes sont les **ShowCheckBoxes**, **ShowExpandCollapse**, et **ShowLines**.

La propriété **ShowCheckBoxes** affecte si oui ou non les nœuds affichent une case à cocher lorsqu’elles sont rendues. Les valeurs valides pour cette propriété **sont None**, **Root**, **Parent**, **Leaf**, et **Tous**. Ceux-ci affectent le contrôle TreeView comme suit:

| **Valeur de propriété** | **Résultat** |
| --- | --- |
| None | Les case à cocher ne sont pas affichées sur les nœuds. Il s'agit du paramètre par défaut. |
| Root | Une case à cocher n’est affichée que sur le nœud racine. |
| Parent | Une case à cocher n’est affichée que sur les nœuds qui ont des nœuds d’enfant. Ces nœuds d’enfant peuvent être des nœuds parents ou des nœuds-feuilles. |
| Feuille | Une case à cocher n’est affichée que sur les nœuds qui n’ont pas de nœuds d’enfant. |
| Tous | Une case à cocher est affichée sur tous les nœuds. |

Lorsque des case à cocher sont utilisées, la propriété **CheckedNodes** retournera une collection de nœuds TreeView qui sont vérifiés après l’arrière.

La propriété **ShowExpandCollapse** contrôle l’apparence de l’image d’expansion/effondrement à côté des nœuds racines et parentaux. Si cette propriété est réglée à **faux,** les nœuds TreeView sont rendus comme hyperliens et sont élargis / effondrés en cliquant sur le lien.

La propriété **ShowLines** contrôle si les lignes sont affichées reliant les nœuds parentaux aux nœuds d’enfant. **Lorsqu’elles sont fausses** (la valeur par défaut), aucune ligne n’est affichée. Lorsque **c’est vrai,** le contrôle TreeView utilisera des images de lignes dans le dossier spécifié par la propriété **LineImagesFolder.**

Pour personnaliser l’apparence des lignes TreeView, Visual Studio .NET 2005 comprend un outil Line Designer. Vous pouvez accéder à cet outil en utilisant le bouton Smart Tag sur le contrôle TreeView ci-dessous.

![](data-bound-controls/_static/image1.jpg)

**La figure 1**

Lorsque vous choisissez l’option de menu **Personnaliserize Line Images,** l’outil Line Designer vous permettra de configurer l’apparence des lignes TreeView.

## <a name="treeview-events"></a>Événements TreeView

Le contrôle TreeView a les événements uniques suivants:

- SelectedNodeChanged se produit lorsqu’un nœud est sélectionné en fonction de la propriété **SelectAction.**
- TreeNodeCheckChanged se produit lorsqu’un état de casettes de nœuds est modifié.
- TreeNodeExpanded se produit lorsqu’un nœud est agrandi en fonction de la propriété **SelectAction.**
- TreeNodeCollapsed se produit quand un nœud est effondré.
- TreeNodeDataBound se produit lorsqu’un nœud est lié aux données.
- TreeNodePopulate se produit lorsqu’un nœud est peuplé.

La propriété **SelectAction** vous permet de configurer quel événement est déclenché lorsqu’un nœud est sélectionné. La propriété SelectAction fournit les actions suivantes :

- TreeNodeSelectAction.Expand soulève TreeNodeExpanded lorsque le nœud est sélectionné.
- TreeNodeSelectAction.None n’augmente aucun événement lorsque le nœud est sélectionné.
- TreeNodeSelectAction.Select soulève l’événement SelectedNodeChanged lorsque le nœud est sélectionné.
- TreeNodeSelectAction.SelectExpand soulève à la fois l’événement SelectedNodeChanged et l’événement TreeNodeExpanded lorsque le nœud est sélectionné.

## <a name="controlling-appearance-with-styles"></a>Contrôle de l’apparence avec des styles

Le contrôle TreeView fournit de nombreuses propriétés pour contrôler l’apparence du contrôle avec des styles. Les propriétés suivantes sont disponibles.

| **Nom de la propriété** | **Contrôles** |
| --- | --- |
| HoverNodeStyle (HoverNodeStyle) | Contrôle le style des nœuds lorsque la souris est planée au-dessus d’eux. |
| LeafNodeStyle | Contrôle le style des nœuds-feuilles. |
| NodeStyle (en) | Contrôle le style pour tous les nœuds. Des styles de nœuds spécifiques (tels que LeafNodeStyle) l’emportent sur ce style. |
| ParentNodeStyle (en) | Contrôle le style de tous les nœuds parent. |
| RootNodeStyle (RootNodeStyle) | Contrôle le style pour le nœud de racine. |
| SélectionnéNodeStyle | Contrôle le style pour le nœud sélectionné. |

Chacune de ces propriétés est lue uniquement. Cependant, ils retourneront chacun un objet **TreeNodeStyle,** et les propriétés de cet objet pourront être modifiées à l’aide du format *propriété-sous-propriété.* Par exemple, pour définir la propriété **ForeColor** de la **SelectedNodeStyle**, vous utiliseriez la syntaxe suivante :

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Notez que l’étiquette ci-dessus n’est pas fermée. C’est parce que lors de l’utilisation de la syntaxe déclarative montrée ici, vous incluriez les nœuds TreeViews dans le code HTML ainsi.

Les propriétés de style peuvent également être spécifiées dans le code en utilisant le format *property.subproperty.* Par exemple, pour définir la propriété **ForeColor** du **RootNodeStyle** dans le code, vous utiliseriez la syntaxe suivante :

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Pour une liste complète des différentes propriétés de style, consultez la documentation MSDN sur l’objet TreeNodeStyle.

## <a name="the-sitemappath-control"></a>Le contrôle SiteMapPath

Le contrôle SiteMapPath fournit un contrôle de navigation de miette de pain pour les développeurs de ASP.NET. Comme les autres contrôles de navigation, il peut être facilement des données liées à des sources de données hiérarchiques telles que le SiteMapDataSource ou XmlDataSource.

Un contrôle SiteMapPath est composé d’objets SiteMapNodeItem. Il existe trois types de nœuds; le nœud racine, les nœuds parent, et le nœud actuel. Le nœud de racine est le nœud au sommet de la structure hiérarchique. Le nœud actuel représente la page actuelle. Tous les autres nœuds sont des nœuds parent.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Contrôle de l’opération du contrôle SiteMapPath

Les propriétés qui contrôlent le fonctionnement du contrôle SiteMapPath sont les suivantes :

| **Propriété** | **Description de la propriété** |
| --- | --- |
| ParentLevelsDisplayed | Contrôle le nombre de nœuds parentaux affichés. La valeur par défaut est de -1 qui n’impose aucune restriction sur le nombre de nœuds parent affichés. |
| PathDirection (en) | Contrôle la direction du SiteMapPath. Les valeurs valides sont RootToCurrent (par défaut) et CurrentToRoot. |
| PathSeparator (en) | Une chaîne qui contrôle le personnage qui sépare les nœuds dans un contrôle SiteMapPath. La valeur par défaut est de :. |
| RenderCurrentNodeAsLink | Une valeur Boolean qui contrôle si oui ou non le nœud actuel est rendu comme un lien. La valeur par défaut est False. |
| SkipLinkText (en) | Aide à l’accessibilité lorsque la page est vue par les lecteurs d’écran. Cette propriété permet aux lecteurs d’écran de sauter le contrôle SiteMapPath. Pour désactiver cette fonctionnalité, définissez la propriété sur String.Empty. |

## <a name="templated-sitemappath-controls"></a>Contrôles Templated SiteMapPath

Le SiteMapControl est un contrôle modélial, et en tant que tel, vous pouvez définir différents modèles pour une utilisation dans l’affichage du contrôle. Pour modifier des modèles dans un contrôle SiteMapPath, cliquez sur le bouton Smart Tag sur le contrôle et choisissez Edit Templates dans le menu. Cela affiche le menu SiteMapTasks comme indiqué ci-dessous où vous pouvez choisir entre les différents modèles disponibles.

![](data-bound-controls/_static/image2.jpg)

**Figure 2**

Le modèle **NodeTemplate** fait référence à n’importe quel nœud dans le SiteMapPath. Si le nœud est un nœud de racine ou le nœud actuel et **qu’un RootNodeTemplate** ou **CurrentNodeTemplate** est configuré, le NodeTemplate est remplacé.

## <a name="sitemappath-events"></a>Événements SiteMapPath

Le contrôle SiteMapPath a deux événements qui ne sont pas dérivés de la classe de contrôle; l’événement **ItemCreated** et l’événement **ItemDataBound.** L’événement ItemCreated est soulevé lors de la création d’un élément SiteMapPath. ItemDataBound est soulevé lorsque la méthode DataBind est appelée lors de la liaison de données d’un nœud SiteMapPath. Un objet **SiteMapNodeItemEventArgs** donne accès au site spécifiqueMapNodeItem via la propriété Item.

## <a name="controlling-appearance-with-styles"></a>Contrôle de l’apparence avec des styles

Les styles suivants sont disponibles pour le formatage d’un contrôle SiteMapPath.

| **Nom de la propriété** | **Contrôles** |
| --- | --- |
| CurrentNodeStyle (en anglais) | Contrôle le style du texte pour le nœud actuel. |
| RootNodeStyle (RootNodeStyle) | Contrôle le style du texte pour le nœud racine. |
| NodeStyle (en) | Contrôle le style du texte pour tous les nœuds en supposant qu’un CurrentNodeStyle ou RootNodeStyle ne s’applique pas. |

La propriété NodeStyle est remplacée par le CurrentNodeStyle ou le RootNodeStyle. Chacune de ces propriétés est lue uniquement et renvoie un objet **Style.** Pour affecter l’apparence d’un nœud à l’aide d’une de ces propriétés, vous devrez définir les propriétés de l’objet Style qui est retourné. Par exemple, le code ci-dessous modifie la propriété avant-couleur du nœud actuel.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La propriété peut également être appliquée sur le plan programmatique comme suit :

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si un modèle est appliqué, le style ne sera pas appliqué.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratoire 1 : Configurer un contrôle de menu ASP.NET

1. Créez un nouveau site Web.
2. Ajoutez un fichier De carte de site en sélectionnant Fichier, Nouveau, Fichier et en choisissant la carte de site de la liste des modèles de fichiers.
3. Ouvrez la carte du site (Web.sitemap par défaut) et modifiez-la de sorte qu’il ressemble à la liste ci-dessous. Les pages auxquelles vous établissez des liens dans le fichier de la carte du site n’existent pas vraiment, mais ce ne sera pas un problème pour cet exercice.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Ouvrez le formulaire Web par défaut dans la vue Design.
5. De la section Navigation de la boîte à outils, ajoutez un nouveau contrôle de menu à la page.
6. De la section Données de la Boîte à outils, ajoutez un nouveau SiteMapDataSource. Le SiteMapDataSource utilisera automatiquement le fichier Web.sitemap dans votre site. (Le fichier Web.sitemap *doit* être dans le dossier racine du site.)
7. Cliquez sur le contrôle du menu, puis cliquez sur le bouton Smart Tag pour afficher le dialogue des tâches de menu.
8. Dans le dropdown Choose Data Source, sélectionnez SiteMapDataSource1.
9. Cliquez sur le lien AutoFormat et choisissez un format pour le menu.
10. Dans le volet Propriétés, définissez la propriété **StaticDisplayLevels** à 2. Le contrôle du menu doit maintenant afficher le nœud Maison, Produits et Services dans le concepteur.
11. Parcourez la page de votre navigateur pour utiliser le menu. (Parce que les pages que vous avez ajoutées à la carte du site n’existent pas réellement, vous verrez une erreur lorsque vous essayez de les parcourir.)

Expérimentez la modification des propriétés StaticDisplayLevels et MaximumDynamicDisplayLevels et découvrez comment elles affectent la façon dont le menu est rendu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratoire 2 : Liaison dynamique d’un contrôle TreeView

Cet exercice suppose que vous avez SQL Server en cours d’exécution localement et que la base de données Northwind est présent sur l’instance SQL Server. Si ces conditions ne sont pas remplies, veuillez modifier la chaîne de connexion de l’échantillon. Notez que vous devrez peut-être également spécifier l’authentification SQL Server au lieu d’une connexion de confiance.

1. Créez un nouveau site Web.
2. Passez à la vue de code pour Default.aspx et remplacez tout le code avec le code indiqué ci-dessous. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Enregistrer la page comme treeview.aspx.
4. Parcourez la page.
5. Lorsque la page est affichée pour la première fois, affichez la source de la page dans votre navigateur. Notez que seuls les nœuds visibles ont été envoyés au client.
6. Cliquez sur le signe positif à côté de n’importe quel nœud.
7. Voir la source sur la page à nouveau. Notez que les nœuds nouvellement affichés sont maintenant présents.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratoire 3 : Détails Afficher et modifier les données à l’aide d’un GridView et de DetailsView

1. Créez un nouveau site Web.
2. Ajoutez un nouveau web.config au site Web.
3. Ajoutez une chaîne de connexion au fichier web.config tel qu’il est indiqué ci-dessous : 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Vous devrez peut-être modifier la chaîne de connexion en fonction de votre environnement.
4. Enregistrez et fermez le fichier web.config.
5. Ouvrez Default.aspx et ajoutez un nouveau contrôle SqlDataSource.
6. Modifier l’ID du contrôle SqlDataSource en **produits**.
7. Dans le menu **SqlDataSource Tasks,** cliquez sur **Configure Data Source**.
8. Sélectionnez **Northwind** dans le dropdown de connexion et cliquez sur Next.
9. Sélectionnez les **produits** de la baisse **de nom** et vérifiez le **ProductID**, **ProductName**, **UnitPrice**, et **UnitsInStock** caseboxes comme indiqué ci-dessous. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Cliquez sur **Suivant**.
11. Cliquez sur **Terminer**.
12. Passez à La vue Source et examinez le code qui a été généré. Remarquez le **SelectCommand**, **DeleteCommand**, **InsertCommand**, et **UpdateCommand** qui ont été ajoutés au contrôle SqlDataSource. Remarquez également les paramètres qui ont été ajoutés.
13. Passez à la vue design et ajoutez un nouveau contrôle GridView à la page.
14. Sélectionnez les **produits** parmi le **dropdown Choose Data Source.**
15. Vérifiez **Activez la sélection de Paging** et **d’activation** comme indiqué ci-dessous. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Cliquez sur le lien **Edit Columns** et assurez-vous que les **champs Auto-generate** sont vérifiés.
17. Cliquez sur **OK**.
18. Avec le contrôle GridView sélectionné, cliquez sur le bouton à côté de la propriété **DataKeyNames** dans le volet Propriétés.
19. Sélectionnez **ProductID** dans la liste **&gt;** des champs de données **disponibles** et cliquez sur le bouton pour l’ajouter.
20. Cliquez sur OK.
21. Ajoutez un nouveau contrôle SqlDataSource à la page.
22. Modifier l’ID du contrôle SqlDataSource en **détails**.
23. Dans le menu SqlDataSource Tasks, choisissez **Configure Data Source**.
24. Choisissez **Northwind** parmi le dropdown et cliquez sur **Next**.
25. Sélectionnez <strong>les produits</strong> de l’abandon <strong>du nom</strong> et vérifiez <strong> \<la case à cocher> forte dans la boîte de liste <strong>Colonnes.</strong>
26. Cliquez sur le bouton **Où.**
27. Sélectionnez **ProductID** à partir du décrochage de la **colonne.**
28. Sélectionnez **=** dans le décrochage de l’opérateur.
29. Sélectionnez **le contrôle** à partir de la **chute source.**
30. Sélectionnez **GridView1** à partir du dropdown **Control ID.**
31. Cliquez sur le bouton **Ajouter** pour ajouter la clause WHERE.
32. Cliquez sur **OK**.
33. Cliquez sur le bouton **Advanced** et vérifiez la boîte de contrôle **generate INSERT, UPDATE et DELETE.**
34. Cliquez sur **OK**.
35. Cliquez **sur Next** et cliquez sur **Finition**.
36. Ajoutez un contrôle DetailsView à la page.
37. Dans le **dropdown De la source de données Choisir,** choisissez **les détails**.
38. Vérifiez la case **à cocher Enable Editing** comme indiqué ci-dessous. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Enregistrer la page et parcourir Default.aspx.
40. Cliquez sur le lien **Select** à côté de différents enregistrements pour voir automatiquement la mise à jour DetailsView.
41. Cliquez sur le lien **Modifier** dans le contrôle DetailsView.
42. Modifier l’enregistrement et cliquer **sur la mise à jour**.
