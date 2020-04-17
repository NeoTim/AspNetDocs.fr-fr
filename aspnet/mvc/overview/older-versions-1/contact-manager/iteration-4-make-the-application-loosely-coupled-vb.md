---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: 'Itération #4 - Rendre l’application lâchement couplée (VB) Microsoft Docs'
author: rick-anderson
description: Dans cette quatrième itération, nous profitons de plusieurs modèles de conception logicielle pour faciliter le maintien et la modification de l’application Contact Manager. Pré...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 1026e307b7e498a8b1392f2907c08cdcd05a3199
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542766"
---
# <a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Itération #4 : Rendre l’application faiblement couplée (VB)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> Dans cette quatrième itération, nous profitons de plusieurs modèles de conception logicielle pour faciliter le maintien et la modification de l’application Contact Manager. Par exemple, nous refactorrons notre application pour utiliser le modèle de dépôt et le modèle d’injection de dépendance.

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

Dans cette quatrième itération de l’application Contact Manager, nous refactorons l’application pour rendre l’application plus lâchement couplée. Lorsqu’une application est vaguement couplée, vous pouvez modifier le code dans une partie de l’application sans avoir besoin de modifier le code dans d’autres parties de l’application. Les applications peu couplées sont plus résistantes au changement.

Actuellement, toutes les logiques d’accès et de validation des données utilisées par l’application Contact Manager sont contenues dans les classes de contrôleur. C’est une mauvaise idée. Chaque fois que vous avez besoin de modifier une partie de votre application, vous risquez d’introduire des bogues dans une autre partie de votre application. Par exemple, si vous modifiez votre logique de validation, vous risquez d’introduire de nouveaux bogues dans votre logique d’accès ou de contrôleur de données.

> [!NOTE] 
> 
> (SRP), une classe ne devrait jamais avoir plus d’une raison de changer. La logique du contrôleur de mélange, de la validation et de la base de données constitue une violation massive du principe de responsabilité unique.

Il y a plusieurs raisons pour lesquelles vous pourriez avoir besoin de modifier votre application. Vous devrez peut-être ajouter une nouvelle fonctionnalité à votre application, vous devrez peut-être corriger un bogue dans votre application, ou vous devrez peut-être modifier la façon dont une fonctionnalité de votre application est implémentée. Les applications sont rarement statiques. Ils ont tendance à grandir et à muter avec le temps.

Imaginez, par exemple, que vous décidiez de modifier la façon dont vous implémentez votre couche d’accès aux données. À l’heure actuelle, l’application Contact Manager utilise le cadre d’entité Microsoft pour accéder à la base de données. Toutefois, vous pouvez décider de migrer vers une technologie d’accès aux données nouvelle ou alternative, comme ADO.NET Data Services ou NHibernate. Toutefois, comme le code d’accès aux données n’est pas isolé du code de validation et de contrôleur, il n’existe aucun moyen de modifier le code d’accès aux données de votre application sans modifier d’autre code qui n’est pas directement lié à l’accès aux données.

Lorsqu’une application est vaguement couplée, d’autre part, vous pouvez apporter des modifications à une partie d’une application sans toucher d’autres parties d’une application. Par exemple, vous pouvez changer de technologie d’accès aux données sans modifier votre logique de validation ou de contrôleur.

Dans cette itération, nous profitons de plusieurs modèles de conception logicielle qui nous permettent de refactorer notre application Contact Manager dans une application plus vaguement couplée. Lorsque nous aurons terminé, le gestionnaire de contact ne fera rien qu’il n’avait pas fait auparavant. Cependant, nous serons en mesure de modifier l’application plus facilement à l’avenir.

> [!NOTE] 
> 
> Refactoring est le processus de réécriture d’une application de telle sorte qu’il ne perd aucune fonctionnalité existante.

## <a name="using-the-repository-software-design-pattern"></a>Utilisation du modèle de conception logicielle de dépôt

Notre premier changement est de tirer parti d’un modèle de conception logicielle appelé le modèle De dépôt. Nous utiliserons le modèle Repository pour isoler notre code d’accès aux données du reste de notre application.

La mise en œuvre du modèle de dépôt nous oblige à remplir les deux étapes suivantes :

1. Créer une interface
2. Créer une classe concrète qui implémente l’interface

Tout d’abord, nous devons créer une interface qui décrit toutes les méthodes d’accès aux données que nous devons exécuter. L’interface IContactManagerRepository est contenue dans la liste 1. Cette interface décrit cinq méthodes : CreateContact(), DeleteContact(), EditContact(), GetContact et ListContacts().

**Liste 1 - Modèles-IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Ensuite, nous devons créer une classe concrète qui met en œuvre l’interface IContactManagerRepository. Parce que nous utilisons le cadre d’entité Microsoft pour accéder à la base de données, nous allons créer une nouvelle classe nommée EntityContactManagerRepository. Cette classe est contenue dans la liste 2.

**Liste 2 - Models-EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Notez que la classe EntityContactManagerRepository implémente l’interface IContactManagerRepository. La classe met en œuvre les cinq méthodes décrites par cette interface.

Vous vous demandez peut-être pourquoi nous devons nous embêter avec une interface. Pourquoi devons-nous créer à la fois une interface et une classe qui l’implémente ?

À une exception près, le reste de notre application interagira avec l’interface et non avec la classe concrète. Au lieu d’appeler les méthodes exposées par la classe EntityContactManagerRepository, nous appellerons les méthodes exposées par l’interface IContactManagerRepository.

De cette façon, nous pouvons implémenter l’interface avec une nouvelle classe sans avoir besoin de modifier le reste de notre application. Par exemple, à une date ultérieure, nous pourrions implémenter une classe DataServicesContactManagerRepository qui implémente l’interface IContactManagerRepository. La classe DataServicesContactManagerRepository peut utiliser ADO.NET Services de données pour accéder à une base de données au lieu du cadre d’entité Microsoft.

Si notre code d’application est programmé contre l’interface IContactManagerRepository au lieu de la classe concrète EntityContactManagerRepository alors nous pouvons changer de classe en béton sans modifier le reste de notre code. Par exemple, nous pouvons passer de la classe EntityContactManagerRepository à la classe DataServicesContactManagerRepository sans modifier notre logique d’accès ou de validation des données.

La programmation contre les interfaces (abstractions) au lieu de classes concrètes rend notre application plus résistante au changement.

> [!NOTE] 
> 
> Vous pouvez rapidement créer une interface à partir d’une classe concrète au sein de Visual Studio en sélectionnant l’option de menu Refactor, Extract Interface. Par exemple, vous pouvez créer la classe EntityContactManagerRepository d’abord, puis utiliser l’interface d’extrait pour générer l’interface IContactManagerRepository automatiquement.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Utilisation du modèle de conception logicielle d’injection de dépendance

Maintenant que nous avons migré notre code d’accès aux données vers une classe de dépôt séparée, nous devons modifier notre contrôleur de contact pour utiliser cette classe. Nous profiterons d’un modèle de conception logicielle appelé Injection de dépendance pour utiliser la classe Repository dans notre contrôleur.

Le contrôleur de contact modifié est contenu dans la liste 3.

**Liste 3 - Controllers-ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Notez que le contrôleur de contact dans la liste 3 a deux constructeurs. Le premier constructeur passe une instance concrète de l’interface IContactManagerRepository au deuxième constructeur. La classe de contrôleur de contact utilise *l’injection de dépendance constructeur*.

Le seul et unique endroit que la classe EntityContactManagerRepository est utilisé est dans le premier constructeur. Le reste de la classe utilise l’interface IContactManagerRepository au lieu de la classe concrète EntityContactManagerRepository.

Il est donc facile de changer d’implémentation de la classe IContactManagerRepository à l’avenir. Si vous souhaitez utiliser la classe DataServicesContactRepository au lieu de la classe EntityContactManagerRepository, il suffit de modifier le premier constructeur.

L’injection de dépendance de constructeur rend également la classe de contrôleur de contact très testable. Dans vos tests unitaires, vous pouvez instantané le contrôleur de contact en passant une mise en œuvre fictive de la classe IContactManagerRepository. Cette fonctionnalité de l’injection de dépendance sera très importante pour nous dans la prochaine itération lorsque nous construisons des tests unitaires pour l’application Contact Manager.

> [!NOTE] 
> 
> Si vous souhaitez découpler complètement la classe de contrôleur de contact à partir d’une mise en œuvre particulière de l’interface IContactManagerRepository, vous pouvez profiter d’un cadre qui prend en charge l’injection de dépendance comme StructureMap ou le cadre d’entité Microsoft (MEF). En profitant d’un cadre d’injection de dépendance, vous n’avez jamais besoin de vous référer à une classe concrète dans votre code.

## <a name="creating-a-service-layer"></a>Création d’une couche de service

Vous avez peut-être remarqué que notre logique de validation est toujours mélangée avec notre logique de contrôleur dans la classe de contrôleur modifié dans La liste 3. Pour la même raison que c’est une bonne idée d’isoler notre logique d’accès aux données, c’est une bonne idée d’isoler notre logique de validation.

Pour résoudre ce problème, nous pouvons créer une [couche de service](http://martinfowler.com/eaaCatalog/serviceLayer.html)séparée . La couche de service est une couche distincte que nous pouvons insérer entre nos classes de contrôleur et de dépôt. La couche de service contient notre logique d’entreprise, y compris toute notre logique de validation.

Le ContactManagerService est contenu dans la liste 4. Il contient la logique de validation de la classe de contrôleur de contact.

**Liste 4 - Models-ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Notez que le constructeur du ContactManagerService nécessite une validationDictionary. La couche de service communique avec la couche du contrôleur par le biais de cette ValidationDictionary. Nous discutons de la ValidationDictionary en détail dans la section suivante lorsque nous discutons du modèle de décorateur.

Notez en outre que le ContactManagerService implémente l’interface IContactManagerService. Vous devriez toujours vous efforcer de programmer contre les interfaces au lieu de classes concrètes. Les autres classes de l’application Contact Manager n’interagissent pas directement avec la classe ContactManagerService. Au lieu de cela, à une exception près, le reste de l’application Contact Manager est programmé contre l’interface IContactManagerService.

L’interface IContactManagerService est contenue dans la liste 5.

**Liste 5 - Modèles-IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La classe modifiée de contrôleur de contact est contenue dans la liste 6. Notez que le contrôleur de contact n’interagit plus avec le référentiel ContactManager. Au lieu de cela, le contrôleur de contact interagit avec le service ContactManager. Chaque couche est isolée autant que possible à partir d’autres couches.

**Liste 6 - Controllers-ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Notre application ne va plus à l’encontre du Principe de responsabilité unique (SRP). Le contrôleur de contact dans la liste 6 a été dépouillé de toute responsabilité autre que le contrôle du flux d’exécution de l’application. Toute la logique de validation a été supprimée du contrôleur de contact et poussée dans la couche de service. Toute la logique de base de données a été poussée dans la couche de dépôt.

## <a name="using-the-decorator-pattern"></a>Utilisation du modèle de décorateur

Nous voulons être en mesure de découpler complètement notre couche de service de notre couche de contrôleur. En principe, nous devrions être en mesure de compiler notre couche de service dans un assemblage séparé de notre couche de contrôleur sans avoir besoin d’ajouter une référence à notre application MVC.

Cependant, notre couche de service doit être en mesure de transmettre les messages d’erreur de validation à la couche du contrôleur. Comment pouvons-nous permettre à la couche de service de communiquer des messages d’erreur de validation sans accouplement du contrôleur et de la couche de service ? Nous pouvons profiter d’un modèle de conception de logiciel nommé le [modèle de décorateur.](http://en.wikipedia.org/wiki/Decorator_pattern)

Un contrôleur utilise un ModelStateDictionary nommé ModelState pour représenter les erreurs de validation. Par conséquent, vous pourriez être tenté de passer ModelState de la couche de contrôleur à la couche de service. Toutefois, l’utilisation de ModelState dans la couche de service rendrait votre couche de service dépendante d’une caractéristique du cadre MVC ASP.NET. Ce serait mauvais parce que, un jour, vous voudrez peut-être utiliser la couche de service avec une application WPF au lieu d’une application ASP.NET MVC. Dans ce cas, vous ne voudriez pas faire référence au cadre ASP.NET MVC pour utiliser la classe ModelStateDictionary.

Le modèle Décorateur vous permet d’envelopper une classe existante dans une nouvelle classe afin d’implémenter une interface. Notre projet De gestionnaire de contact comprend la classe ModelStateWrapper contenue dans la liste 7. La classe ModelStateWrapper implémente l’interface dans la liste 8.

**Liste 7 - Modèles-Validation-ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Liste 8 - Modèles-Validation-IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Si vous examinez de près La liste 5, vous verrez que la couche de service ContactManager utilise exclusivement l’interface IValidationDictionary. Le service ContactManager ne dépend pas de la classe ModelStateDictionary. Lorsque le contrôleur de contact crée le service ContactManager, le contrôleur enveloppe son ModelState comme ceci :

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous n’avons pas ajouté de nouvelles fonctionnalités à l’application Contact Manager. Le but de cette itération était de refactorer l’application Contact Manager de sorte qu’il est plus facile à entretenir et à modifier.

Tout d’abord, nous avons implémenté le modèle de conception de logiciels Repository. Nous avons migré tout le code d’accès aux données vers une classe de référentiel ContactManager distincte.

Nous avons également isolé notre logique de validation de notre logique de contrôleur. Nous avons créé une couche de service distincte qui contient tout notre code de validation. La couche de contrôleur interagit avec la couche de service, et la couche de service interagit avec la couche de dépôt.

Lorsque nous avons créé la couche de service, nous avons profité du modèle de décorateur pour isoler ModelState de notre couche de service. Dans notre couche de service, nous avons programmé contre l’interface IValidationDictionary au lieu de ModelState.

Enfin, nous avons profité d’un modèle de conception de logiciel nommé le modèle d’injection de dépendance. Ce modèle nous permet de programmer contre les interfaces (abstractions) au lieu de classes concrètes. La mise en œuvre du modèle de conception d’injection de dépendance rend également notre code plus testable. Dans la prochaine itération, nous ajoutons des tests unitaires à notre projet.

> [!div class="step-by-step"]
> [Suivant précédent](iteration-3-add-form-validation-vb.md)
> [Next](iteration-5-create-unit-tests-vb.md)
