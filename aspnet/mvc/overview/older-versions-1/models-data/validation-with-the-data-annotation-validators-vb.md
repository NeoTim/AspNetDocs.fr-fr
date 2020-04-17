---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validation avec les Validateurs d’Annotation de Données (VB) Microsoft Docs
author: rick-anderson
description: Profitez du modèle d’annotation des données pour effectuer la validation dans une application MVC ASP.NET. Apprenez à utiliser les différents types de validateur...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: cabdf6dab9e5de53a45adcf126705533fca02de7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542636"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Validation avec les validateurs d’annotation de données (VB)

par [Microsoft](https://github.com/microsoft)

> Profitez du modèle d’annotation des données pour effectuer la validation dans une application MVC ASP.NET. Apprenez à utiliser les différents types d’attributs de validateur et travaillez avec eux dans le cadre d’entité Microsoft.

Dans ce tutoriel, vous apprenez à utiliser les validateurs d’annotation de données pour effectuer la validation dans une application ASP.NET MVC. L’avantage d’utiliser les validateurs d’annotation de données est qu’ils vous permettent d’effectuer la validation simplement en ajoutant un ou plusieurs attributs - tels que l’attribut Requis ou StringLength - à une propriété de classe.

Avant de pouvoir utiliser les validateurs d’annotation de données, vous devez télécharger le lien de modèle d’annotations de données. Vous pouvez télécharger l’échantillon de reliure de modèle d’annotations de données sur le site Web de CodePlex en cliquant [ici.](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)

Il est important de comprendre que le modèle d’annotations de données Binder n’est pas une partie officielle du cadre Microsoft ASP.NET MVC. Bien que le Data Annotations Model Binder a été créé par l’équipe Microsoft ASP.NET MVC, Microsoft n’offre pas de support produit officiel pour le modèle d’annotations de données Binder décrit et utilisé dans ce tutoriel.

## <a name="using-the-data-annotation-model-binder"></a>Utilisation du liant modèle d’annotation des données

Afin d’utiliser le système d’annotations de données Dans une application MVC ASP.NET, vous devez d’abord ajouter une référence à l’assemblage Microsoft.Web.Mvc.DataAnnotations.dll et à l’assemblage System.ComponentModel.DataAnnotations.dll. Sélectionnez l’option de menu **Project, Ajouter référence**. Cliquez ensuite sur **l’onglet Parcourir** et naviguez à l’endroit où vous avez téléchargé (et décompressé) l’échantillon de l’analyse modèle d’annotations de données (voir **la figure 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figure 1**: Ajout d’une référence au modèle d’annotations de données Binder[(Cliquez pour voir l’image grandeur nature](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Sélectionnez à la fois l’assemblage Microsoft.Web.Mvc.DataAnnotations.dll et l’assemblage System.ComponentModel.DataAnnotations.dll et cliquez sur le bouton **OK.**

Vous ne pouvez pas utiliser l’assemblage System.ComponentModel.DataAnnotations.dll inclus avec .NET Framework Service Pack 1 avec le système d’annotations de données. Vous devez utiliser la version de l’assemblage System.ComponentModel.DataAnnotations.dll inclus dans le téléchargement de l’échantillon de l’échantillon de l’échantillon de type d’annotations de données.

Enfin, vous devez enregistrer le système d’analyse dataAnnotations dans le fichier Global.asax. Ajoutez la ligne de code\_suivante au gestionnaire d’événements De démarrage d’application() de sorte que la méthode De démarrage d’application()\_ressemble à ceci :

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Cette ligne de code enregistre le DataAnnotationsModelBinder comme le liant de modèle par défaut pour l’ensemble de l’application ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Utilisation des attributs de validation de l’annotation des données

Lorsque vous utilisez le lien de modèle d’annotations de données, vous utilisez des attributs de validateur pour effectuer la validation. L’espace de nom System.ComponentModel.DataAnnotations inclut les attributs de validation suivants :

- Gamme - Vous permet de valider si la valeur d’une propriété se situe entre une gamme de valeurs spécifiée.
- L’impression régulière vous permet de valider si la valeur d’une propriété correspond à un modèle d’expression régulier spécifié.
- Nécessaire - Vous permet de marquer une propriété au besoin.
- StringLength - Vous permet de spécifier une longueur maximale pour une propriété à cordes.
- Validation - La classe de base pour tous les attributs de validateur.

> [!NOTE] 
> 
> Si vos besoins de validation ne sont pas satisfaits par l’un des validateurs standard, vous avez toujours la possibilité de créer un attribut de validateur personnalisé en héritant d’un nouvel attribut de validateur à partir de l’attribut de validation de base.

La classe de produits dans **la liste 1** illustre comment utiliser ces attributs de validateur. Les propriétés Nom, Description et UnitPrice sont marquées au besoin. La propriété Name doit avoir une longueur de chaîne qui est inférieure à 10 caractères. Enfin, la propriété UnitPrice doit correspondre à un modèle d’expression régulier qui représente un montant de devise.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Liste 1**: Models-Product.vb

La classe Produit illustre comment utiliser un attribut supplémentaire : l’attribut DisplayName. L’attribut DisplayName vous permet de modifier le nom de la propriété lorsque la propriété est affichée dans un message d’erreur. Au lieu d’afficher le message d’erreur "Le champ UnitPrice est nécessaire" vous pouvez afficher le message d’erreur "Le champ de prix est nécessaire".

> [!NOTE] 
> 
> Si vous souhaitez personnaliser complètement le message d’erreur affiché par un validateur, vous pouvez affecter un message d’erreur personnalisé à la propriété ErrorMessage du validateur comme ceci :`<Required(ErrorMessage:="This field needs a value!")>`

Vous pouvez utiliser la classe de produit dans **la liste 1** avec l’action de contrôleur Créer () dans **la liste 2**. Cette action de contrôleur redisjoue la vue De création lorsque l’état du modèle contient des erreurs.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Liste 2**: Controllers-ProductController.vb

Enfin, vous pouvez créer la vue dans **la liste 3** en cliquant à droite sur l’action Créer () et en sélectionnant l’option de menu **Ajouter Vue**. Créez une vue fortement typée avec la classe de produits comme classe modèle. Sélectionnez **Créer à** partir de la liste d’abandon du contenu (voir **la figure 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figure 2**: Ajout de la vue de création

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Liste 3**: Vues-Product-Create.aspx

> [!NOTE] 
> 
> Retirez le champ Id du formulaire Créer généré par l’option de menu **Add View.** Étant donné que le champ Id correspond à une colonne Identity, vous ne souhaitez pas permettre aux utilisateurs d’entrer une valeur pour ce champ.

Si vous soumettez le formulaire pour la création d’un produit et que vous n’entrez pas les valeurs pour les champs requis, les messages d’erreur de validation de la **figure 3** s’affichent.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figure 3**: Champs manquants requis

Si vous entrez un montant de devise invalide, le message d’erreur dans **la figure 4** s’affiche.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figure 4**: Montant de la monnaie invalide

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Utilisation de validateurs d’annotation de données avec le Cadre d’entité

Si vous utilisez le cadre d’entité Microsoft pour générer vos catégories de modèles de données, vous ne pouvez pas appliquer les attributs du validateur directement à vos classes. Étant donné que le concepteur de cadre d’entité génère les classes de modèle, tous les changements que vous apporterez aux classes de modèle seront écrasés la prochaine fois que vous apporterez des changements dans le concepteur.

Si vous souhaitez utiliser les validateurs avec les classes générées par le Cadre d’entité, vous devez créer des classes de méta data. Vous appliquez les validateurs à la classe de données méta au lieu d’appliquer les validateurs à la classe réelle.

Par exemple, imaginez que vous avez créé une classe de films en utilisant le cadre de l’entité (voir **la figure 5**). Imaginez, en outre, que vous voulez faire le titre du film et les propriétés de réalisateur des propriétés requises. Dans ce cas, vous pouvez créer la classe partielle et la classe de données méta dans **la liste 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figure 5**: Classe de film générée par Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Liste 4**: Models-Movie.vb

Le fichier dans **La liste 4** contient deux classes nommées Movie et MovieMetaData. La classe de cinéma est une classe partielle. Il correspond à la classe partielle générée par le cadre d’entité qui est contenu dans le fichier DataModel.Designer.vb.

Actuellement, le cadre .NET ne prend pas en charge les propriétés partielles. Par conséquent, il n’y a aucun moyen d’appliquer les attributs de validateur aux propriétés de la classe de film définies dans le fichier DataModel.Designer.vb en appliquant les attributs de validateur aux propriétés de la classe de film définie dans le fichier dans **la liste 4**.

Notez que la classe partielle Movie est décorée avec un attribut MetadataType qui pointe à la classe MovieMetaData. La classe MovieMetaData contient des propriétés proxy pour les propriétés de la classe De film.

Les attributs de validateur sont appliqués aux propriétés de la classe MovieMetaData. Les propriétés Titre, Directeur et DateReleased sont toutes marquées comme propriétés requises. La propriété Du Directeur doit se voir attribuer une chaîne qui contient moins de 5 caractères. Enfin, l’attribut DisplayName est appliqué à la propriété DateReleased pour afficher un message d’erreur comme « Le champ de sortie de date est requis ». au lieu de l’erreur "Le champ DateReleased est nécessaire."

> [!NOTE] 
> 
> Notez que les propriétés proxy dans la classe MovieMetaData n’ont pas besoin de représenter les mêmes types que les propriétés correspondantes dans la classe Film. Par exemple, la propriété Director est une propriété à cordes dans la classe de film et une propriété d’objets dans la classe MovieMetaData.

La page **de la figure 6** illustre les messages d’erreur retournés lorsque vous entrez des valeurs invalides pour les propriétés du film.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figure 6**: Utilisation de validateurs avec le cadre de l’entité ([Cliquez pour voir l’image grandeur nature](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez appris à tirer parti du modèle d’annotation des données Pour effectuer la validation dans une application ASP.NET MVC. Vous avez appris à utiliser les différents types d’attributs de validateur tels que les attributs Requis et StringLength. Vous avez également appris à utiliser ces attributs lorsque vous travaillez avec le cadre d’entité Microsoft.

> [!div class="step-by-step"]
> [Précédent](validating-with-a-service-layer-vb.md)
