---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Itération #3 - Validation de formulaire d’ajout (C) Microsoft Docs'
author: rick-anderson
description: Dans la troisième itération, nous ajoutons la validation de forme de base. Nous empêchons les gens de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: c8442574d4901045f044f53ea12cd8330e8eaaa3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542376"
---
# <a name="iteration-3--add-form-validation-c"></a>Itération #3 : Ajouter une validation de formulaire (C#)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Dans la troisième itération, nous ajoutons la validation de forme de base. Nous empêchons les gens de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses e-mail et les numéros de téléphone.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Construire une application de gestion des contacts ASP.NET MVC (CMD)

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

Dans cette deuxième itération de l’application Contact Manager, nous ajoutons la validation de formulaire de base. Nous empêchons les gens de soumettre un contact sans entrer des valeurs pour les champs de forme requis. Nous validons également les numéros de téléphone et les adresses électroniques (voir la figure 1).

[![Boîte de dialogue New Project](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figure 01**: Un formulaire de validation[(Cliquez pour voir l’image grandeur nature](iteration-3-add-form-validation-cs/_static/image2.png))

Dans cette itération, nous ajoutons la logique de validation directement aux actions du contrôleur. En général, ce n’est pas la façon recommandée d’ajouter la validation à une application MVC ASP.NET. Une meilleure approche consiste à placer une logique de validation d une application dans une [couche de service](http://martinfowler.com/eaaCatalog/serviceLayer.html)distincte. Dans la prochaine itération, nous refactorons l’application Contact Manager pour rendre l’application plus maintenable.

Dans cette itération, pour garder les choses simples, nous écrivons tout le code de validation à la main. Au lieu d’écrire le code de validation nous-mêmes, nous pourrions profiter d’un cadre de validation. Par exemple, vous pouvez utiliser le bloc d’application de validation de bibliothèque d’entreprise Microsoft (VAB) pour implémenter la logique de validation de votre application ASP.NET MVC. Pour en savoir plus sur le bloc d’applications de validation, voir :

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Ajout de validation à la vue de création

Commençons par ajouter la logique de validation à la vue Créer. Heureusement, parce que nous avons généré la vue Create avec Visual Studio, la vue Create contient déjà toute la logique d’interface utilisateur nécessaire pour afficher des messages de validation. La vue Créer est contenue dans la liste 1.

**Liste 1 - 'Views’Contact’Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Notez l’appel à la méthode d’aide Html.ValidationSummary() qui apparaît immédiatement au-dessus du formulaire HTML. S’il y a des messages d’erreur de validation, cette méthode affiche les messages de validation dans une liste de balles.

Notez, en outre, les appels à Html.ValidationMessage() qui apparaissent à côté de chaque champ de formulaire. L’aide ValidationMessage () affiche un message d’erreur de validation individuel. Dans le cas de la liste 1, un astérisque s’affiche lorsqu’il y a une erreur de validation.

Enfin, l’aide Html.TextBox() rend automatiquement une classe De feuille de style en cascade lorsqu’il y a une erreur de validation associée à la propriété affichée par l’aide. L’aide Html.TextBox() rend une classe nommée **entrée-validation-erreur**.

Lorsque vous créez une nouvelle application ASP.NET MVC, une feuille de style nommée Site.css est créée automatiquement dans le dossier Content. Cette feuille de style contient les définitions suivantes pour les classes CSS liées à l’apparition de messages d’erreur de validation :

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

La classe de validation-erreur de champ est utilisée pour coiffer la sortie rendue par l’aide Html.ValidationMessage(). La classe d’entrée-validation-erreur est utilisée pour coiffer la boîte de texte (entrée) rendue par l’assistant Html.TextBox(). La classe validation-résumé-erreurs est utilisée pour coiffer la liste non ordonnée rendue par l’aide Html.ValidationSummary().

> [!NOTE] 
> 
> Vous pouvez modifier les classes de feuille de style décrites dans cette section pour personnaliser l’apparence des messages d’erreur de validation.

## <a name="adding-validation-logic-to-the-create-action"></a>Ajout de la logique de validation à l’action de création

À l’heure actuelle, la vue Create n’affiche jamais les messages d’erreur de validation parce que nous n’avons pas écrit la logique pour générer des messages. Afin d’afficher des messages d’erreur de validation, vous devez ajouter les messages d’erreur à ModelState.

> [!NOTE] 
> 
> La méthode UpdateModel() ajoute automatiquement des messages d’erreur à ModelState lorsqu’une erreur attribue la valeur d’un champ de formulaire à une propriété. Par exemple, si vous essayez d’attribuer la chaîne "pomme" à une propriété BirthDate qui accepte les valeurs DateTime, alors la méthode UpdateModel() ajoute une erreur à ModelState.

La méthode Modifiée Créer() dans la liste 2 contient une nouvelle section qui valide les propriétés de la classe De contact avant que le nouveau contact ne soit inséré dans la base de données.

**Liste 2 - Controllers-ContactController.cs (Créer avec validation)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

La section de validation applique quatre règles de validation distinctes :

- La propriété FirstName doit avoir une longueur supérieure à zéro (et elle ne peut pas se composer uniquement d’espaces)
- La propriété LastName doit avoir une longueur supérieure à zéro (et elle ne peut pas se composer uniquement d’espaces)
- Si la propriété De téléphone a une valeur (a une longueur supérieure à 0) alors la propriété de téléphone doit correspondre à une expression régulière.
- Si la propriété Email a une valeur (a une longueur supérieure à 0), alors la propriété Email doit correspondre à une expression régulière.

Lorsqu’il y a violation de la règle de validation, un message d’erreur est ajouté à ModelState à l’aide de la méthode AddModelError() . Lorsque vous ajoutez un message à ModelState, vous fournissez le nom d’une propriété et le texte d’un message d’erreur de validation. Ce message d’erreur est affiché dans la vue par les méthodes d’aide Html.ValidationSummary() et Html.ValidationMessage().

Une fois les règles de validation exécutées, la propriété IsValid de ModelState est vérifiée. La propriété IsValid revient fausse lorsque des messages d’erreur de validation ont été ajoutés à ModelState. En cas d’échec de validation, le formulaire Create est redisjoué avec les messages d’erreur.

> [!NOTE] 
> 
> J’ai obtenu les expressions régulières pour valider le numéro de téléphone et l’adresse e-mail à partir du référentiel d’expression régulière à[*http://regexlib.com*](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Ajout de la logique de validation à l’action d’édition

L’action Edit() met à jour un contact. L’action Edit() doit effectuer exactement la même validation que l’action Créer() Au lieu de dupliquer le même code de validation, nous devrions refactorer le contrôleur de contact de sorte que les actions Create() et Edit() appellent la même méthode de validation.

La classe modifiée de contrôleur de contact est contenue dans la liste 3. Cette classe a une nouvelle méthode ValidateContact() qui est appelée à la fois dans les actions Créer() et Modifier().

**Liste 3 - Controllers-ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté la validation de formulaire de base à notre application Contact Manager. Notre logique de validation empêche les utilisateurs de soumettre un nouveau contact ou d’éditer un contact existant sans fournir de valeurs pour les propriétés FirstName et LastName. De plus, les utilisateurs doivent fournir des numéros de téléphone et des adresses e-mail valides.

Dans cette itération, nous avons ajouté la logique de validation à notre application Contact Manager de la manière la plus simple possible. Cependant, le mélange de notre logique de validation dans notre logique de contrôleur va créer des problèmes pour nous à long terme. Notre application sera plus difficile à maintenir et à modifier au fil du temps.

Dans la prochaine itération, nous allons refactorer notre logique de validation et la logique d’accès à la base de données de nos contrôleurs. Nous profiterons de plusieurs principes de conception de logiciels pour nous permettre de créer une application plus vaguement couplée et plus maintenable.

> [!div class="step-by-step"]
> [Suivant précédent](iteration-2-make-the-application-look-nice-cs.md)
> [Next](iteration-4-make-the-application-loosely-coupled-cs.md)
