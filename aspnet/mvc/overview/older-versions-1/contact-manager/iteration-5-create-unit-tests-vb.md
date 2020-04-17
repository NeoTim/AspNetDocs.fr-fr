---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Itération #5 - Créer des tests unitaires (VB) Microsoft Docs'
author: rick-anderson
description: Dans la cinquième itération, nous rendons notre application plus facile à maintenir et à modifier en ajoutant des tests unitaires. Nous nous moquons de nos classes de modèles de données et construisons des tests unitaires pour o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: cc5425de86e7481894fbea242fa555b5598167f6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542337"
---
# <a name="iteration-5--create-unit-tests-vb"></a>Itération #5 : Créer des tests unitaires (VB)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> Dans la cinquième itération, nous rendons notre application plus facile à maintenir et à modifier en ajoutant des tests unitaires. Nous nous moquons de nos classes de modèles de données et construisons des tests unitaires pour nos contrôleurs et la logique de validation.

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

Lors de l’itération précédente de l’application Contact Manager, nous avons refactorisé l’application pour qu’elle soit plus vaguement couplée. Nous avons séparé l’application en couches distinctes de contrôleur, de service et de dépôt. Chaque couche interagit avec la couche sous elle à travers des interfaces.

Nous avons refactorisé l’application pour rendre l’application plus facile à maintenir et à modifier. Par exemple, si nous avons besoin d’utiliser une nouvelle technologie d’accès aux données, nous pouvons simplement modifier la couche de dépôt sans toucher le contrôleur ou la couche de service. En rendant le Gestionnaire de contact vaguement couplé, nous avons rendu l’application plus résistante au changement.

Mais que se passe-t-il lorsque nous avons besoin d’ajouter une nouvelle fonctionnalité à l’application Contact Manager ? Ou, que se passe-t-il lorsque nous réparons un bogue? Une triste, mais bien prouvée, la vérité de la rédaction de code est que chaque fois que vous touchez le code, vous créez le risque d’introduire de nouveaux bugs.

Par exemple, un beau jour, votre gestionnaire peut vous demander d’ajouter une nouvelle fonctionnalité au gestionnaire de contact. Elle veut que vous ajoutiez un soutien aux groupes de contact. Elle veut que vous permettiez aux utilisateurs d’organiser leurs contacts en groupes tels que Friends, Business, et ainsi de suite.

Afin de mettre en œuvre cette nouvelle fonctionnalité, vous devrez modifier les trois couches de l’application Contact Manager. Vous devrez ajouter de nouvelles fonctionnalités aux contrôleurs, à la couche de service et au référentiel. Dès que vous commencez à modifier le code, vous risquez de casser les fonctionnalités qui fonctionnaient auparavant.

Refactoriser notre application en couches séparées, comme nous l’avons fait dans l’itération précédente, était une bonne chose. C’était une bonne chose car il nous permet d’apporter des modifications à des couches entières sans toucher le reste de l’application. Toutefois, si vous souhaitez faciliter la maintenance et la modification du code dans une couche, vous devez créer des tests unitaires pour le code.

Vous utilisez un test unitaire pour tester une unité de code. Ces unités de code sont plus petites que les couches d’application entières. En règle générale, vous utilisez un test unitaire pour vérifier si une méthode particulière dans votre code se comporte de la façon que vous attendez. Par exemple, vous créeriez un test unitaire pour la méthode CreateContact() exposée par la classe ContactManagerService.

L’unité teste une application comme un filet de sécurité. Chaque fois que vous modifiez le code dans une application, vous pouvez exécuter un ensemble de tests unitaires pour vérifier si la modification brise les fonctionnalités existantes. Les tests unitaires rendent votre code sécurisé pour modifier. Les tests unitaires rendent tout le code de votre application plus résistant aux modifications.

Dans cette itération, nous ajoutons des tests unitaires à notre application Contact Manager. De cette façon, dans la prochaine itération, nous pouvons ajouter des groupes de contact à notre application sans se soucier de briser les fonctionnalités existantes.

> [!NOTE] 
> 
> Il existe une variété de cadres d’essai unitaire, y compris NUnit, xUnit.net et MbUnit. Dans ce tutoriel, nous utilisons le cadre de test unitaire inclus avec Visual Studio. Cependant, vous pouvez tout aussi bien utiliser l’un de ces cadres alternatifs.

## <a name="what-gets-tested"></a>Ce qui est testé

Dans le monde parfait, tout votre code serait couvert par des tests unitaires. Dans le monde parfait, vous auriez le filet de sécurité parfait. Vous seriez en mesure de modifier n’importe quelle ligne de code dans votre application et de savoir instantanément, en exécutant vos tests unitaires, si la modification a brisé les fonctionnalités existantes.

Cependant, nous ne vivons pas dans un monde parfait. En pratique, lors de la rédaction de tests unitaires, vous vous concentrez sur l’écriture de tests pour votre logique métier (par exemple, la logique de validation). En particulier, vous *n’écrivez pas* de tests unitaires pour votre logique d’accès aux données ou votre logique de vue.

Pour être utiles, les tests unitaires doivent s’exécuter très rapidement. Vous pouvez facilement accumuler des centaines (voire des milliers) de tests unitaires pour une application. Si les tests unitaires prennent beaucoup de temps à exécuter, vous éviterez de les exécuter. En d’autres termes, les tests unitaires de longue durée sont inutiles à des fins de codage au jour le jour.

Pour cette raison, vous n’écrivez généralement pas de tests unitaires pour le code qui interagit avec une base de données. Exécuter des centaines de tests unitaires contre une base de données en direct serait trop lent. Au lieu de cela, vous vous moquez de votre base de données et écrivez du code qui interagit avec la base de données simulée (nous discutons de se moquer d’une base de données ci-dessous).

Pour une raison similaire, vous n’écrivez généralement pas de tests unitaires pour les vues. Afin de tester une vue, vous devez faire tourner un serveur web. Étant donné que la rotation d’un serveur Web est un processus relativement lent, il n’est pas recommandé de créer des tests unitaires pour vos vues.

Si votre point de vue contient une logique compliquée, alors vous devriez envisager de déplacer la logique dans les méthodes Helper. Vous pouvez écrire des tests unitaires pour les méthodes Helper qui s’exécutent sans tourner vers le haut d’un serveur web.

> [!NOTE] 
> 
> Bien que l’écriture de tests pour la logique d’accès aux données ou la logique de visualisation n’est pas une bonne idée lors de la rédaction de tests unitaires, ces tests peuvent être très précieux lors de la construction de tests fonctionnels ou d’intégration.

> [!NOTE] 
> 
> ASP.NET MVC est le moteur Web Forms View. Bien que le moteur Web Forms View dépend d’un serveur Web, d’autres moteurs de vue pourraient ne pas l’être.

## <a name="using-a-mock-object-framework"></a>Utilisation d’un cadre d’objets fictifs

Lors de la construction de tests d’unité, vous avez presque toujours besoin de profiter d’un cadre d’objet mock. Un cadre d’objet simulé vous permet de créer des maquettes et des talons pour les classes de votre application.

Par exemple, vous pouvez utiliser un cadre Mock Object pour générer une version simulée de votre classe de référentiel. De cette façon, vous pouvez utiliser la classe de dépôt simulée au lieu de la classe de dépôt réel dans vos tests unitaires. L’utilisation du faux référentiel vous permet d’éviter d’exécuter le code de base de données lors de l’exécution d’un test unitaire.

Visual Studio n’inclut pas de cadre d’objet simulé. Cependant, il existe plusieurs cadres commerciaux et open source Mock Object disponibles pour le cadre .NET:

1. Moq - Ce cadre est disponible sous la licence open source BSD. Vous pouvez télécharger [https://code.google.com/p/moq/](https://code.google.com/p/moq/)Moq à partir de .
2. Rhino Mocks - Ce cadre est disponible sous la licence open source BSD. Vous pouvez télécharger Rhino [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx)Mocks à partir de .
3. Typemock Isolator - Il s’agit d’un cadre commercial. Vous pouvez télécharger une [http://www.typemock.com/](http://www.typemock.com/)version d’essai à partir de .

Dans ce tutoriel, j’ai décidé d’utiliser Moq. Cependant, vous pouvez tout aussi bien utiliser Rhino Mocks ou Typemock Isolator pour créer les objets Mock pour l’application Contact Manager.

Avant de pouvoir utiliser Moq, vous devez remplir les étapes suivantes :

1. .
2. Avant de décompresser le téléchargement, assurez-vous que vous cliquez à droite sur le fichier et cliquez sur le bouton étiqueté **Unblock** (voir la figure 1).
3. Décompressez le téléchargement.
4. Ajoutez une référence à l’assemblage Moq à votre projet Test en sélectionnant l’option de menu **Project, Ajouter référence** pour ouvrir le dialogue **Add Reference.** Sous l’onglet Parcourir, naviguez vers le dossier où vous avez décompressé Moq et sélectionnez l’assemblage Moq.dll. Cliquez sur le bouton **OK** (voir la figure 2).

[![Déblocage de Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figure 01**: Déblocage moq[(Cliquez pour voir l’image grandeur nature](iteration-5-create-unit-tests-vb/_static/image2.png))

[![Références après l’ajout de Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figure 02**: Références après l’ajout de Moq[(Cliquez pour voir l’image grandeur nature](iteration-5-create-unit-tests-vb/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Création de tests unitaires pour la couche de service

Commençons par créer un ensemble de tests unitaires pour la couche de service de notre application Contact Manager. Nous utiliserons ces tests pour vérifier notre logique de validation.

Créez un nouveau dossier nommé Models dans le projet ContactManager.Tests. Ensuite, cliquez à droite sur le dossier Modèles et sélectionnez **Ajouter, Nouveau Test**. Le **dialogue Add New Test** indiqué dans la figure 3 apparaît. Sélectionnez le modèle **de test unitaire** et nommez votre nouveau test ContactManagerServiceTest.vb. Cliquez sur le bouton **OK** pour ajouter votre nouveau test à votre projet de test.

> [!NOTE] 
> 
> En général, vous voulez que la structure du dossier de votre projet de test corresponde à la structure de dossier de votre ASP.NET projet MVC. Par exemple, vous placez des tests de contrôleur dans un dossier Controllers, des tests de modèle dans un dossier Models, et ainsi de suite.

[![Modèles-ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figure 03**: Models-ContactManagerServiceTest.cs[(Cliquez pour voir l’image grandeur nature](iteration-5-create-unit-tests-vb/_static/image6.png))

Dans un premier temps, nous voulons tester la méthode CreateContact() exposée par la classe ContactManagerService. Nous allons créer les cinq tests suivants:

- CreateContact() - Les tests qui créentContact() retourne la valeur vraie lorsqu’un contact valide est transmis à la méthode.
- CreateContactRequiredFirstName() - Teste qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec un prénom manquant est transmis à la méthode CreateContact() .
- CreateContactRequiredLastName() - Teste qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec un nom de famille manquant est transmis à la méthode CreateContact() .
- CreateContactInvalidPhone() - Teste qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec un numéro de téléphone invalide est transmis à la méthode CreateContact() .
- CreateContactInvalidEmail() - Teste qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec une adresse e-mail invalide est transmis à la méthode CreateContact().

Le premier test vérifie qu’un contact valide ne génère pas d’erreur de validation. Les tests restants vérifient chacune des règles de validation.

Le code de ces tests est contenu dans la liste 1.

**Liste 1 - Models-ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]

Parce que nous utilisons la classe De contact dans la liste 1, nous devons ajouter une référence au cadre d’entité Microsoft à notre projet de test. Ajoutez une référence à l’assemblage System.Data.Entity.

La liste 1 contient une méthode nommée Initialize() qui est décorée avec l’attribut [TestInitialize]. Cette méthode est appelée automatiquement avant que chacun des tests unitaires soit exécuté (il est appelé 5 fois juste avant chacun des tests unitaires). La méthode Initialize() crée un faux référentiel avec la ligne de code suivante :

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Cette ligne de code utilise le cadre Moq pour générer un faux référentiel à partir de l’interface IContactManagerRepository. Le faux référentiel est utilisé au lieu de l’entité réelleContactManagerRepository pour éviter d’accéder à la base de données lorsque chaque test unitaire est exécuté. Le faux référentiel implémente les méthodes de l’interface IContactManagerRepository, mais les méthodes ne font rien.

> [!NOTE] 
> 
> Lors de l’utilisation du cadre \_Moq, il \_ya une distinction entre mockRepository et mockRepository.Object. Le premier fait référence à la classe Mock (Of IContactManagerRepository) qui contient des méthodes pour spécifier comment le faux référentiel se comportera. Ce dernier fait référence au faux référentiel réel qui implémente l’interface IContactManagerRepository.

Le faux référentiel est utilisé dans la méthode Initialize() lors de la création d’une instance de la classe ContactManagerService. Tous les tests unitaires individuels utilisent cette instance de la classe ContactManagerService.

La liste 1 contient cinq méthodes qui correspondent à chacun des tests unitaires. Chacune de ces méthodes est décorée de l’attribut [TestMethod]. Lorsque vous exécutez les tests unitaires, toute méthode qui a cet attribut est appelée. En d’autres termes, toute méthode qui est décorée avec l’attribut [TestMethod] est un test unitaire.

Le premier test unitaire, nommé CreateContact(), vérifie que l’appel CreateContact() renvoie la valeur vraie lorsqu’une instance valide de la classe de contact est transmise à la méthode. Le test crée une instance de la classe De contact, appelle la méthode CreateContact() et vérifie que CreateContact() renvoie la valeur vraie.

Les tests restants vérifient que lorsque la méthode CreateContact() est appelée avec un contact invalide, la méthode retourne fausse et le message d’erreur de validation attendu est ajouté à l’état du modèle. Par exemple, le test CreateContactRequiredFirstName() crée une instance de la classe Contact avec une chaîne vide pour sa propriété FirstName. Ensuite, la méthode CreateContact() est appelée avec le contact invalide. Enfin, le test vérifie que CreateContact() retourne faux et que l’état du modèle contient le message d’erreur de validation attendu "Le prénom est requis."

Vous pouvez exécuter les tests unitaires dans la liste 1 en sélectionnant l’option de menu **Test, Exécuter, Tous les tests en solution (CTRL-R, A)**. Les résultats des tests sont affichés dans la fenêtre des résultats des tests (voir la figure 4).

[![Résultats des tests](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figure 04**: Résultats des tests ([Cliquez pour voir l’image grandeur nature](iteration-5-create-unit-tests-vb/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Création de tests unitaires pour les contrôleurs

ASP.NET’application MVC contrôle le flux d’interaction utilisateur. Lors du test d’un contrôleur, vous souhaitez tester si le contrôleur retourne le bon résultat d’action et afficher les données. Vous pouvez également tester si un contrôleur interagit avec les classes de modèles de la manière prévue.

Par exemple, La liste 2 contient deux tests unitaires pour la méthode Contact Controller Create(). Le premier test unitaire vérifie que lorsqu’un contact valide est transmis à la méthode Créer(), la méthode Create() redirige vers l’action Index. En d’autres termes, lorsqu’il est adopté un contact valide, la méthode Créer() doit retourner un RedirectToRouteResult qui représente l’action Index.

Nous ne voulons pas tester la couche de service ContactManager lorsque nous testons la couche de contrôleur. Par conséquent, nous nous moquons de la couche de service avec le code suivant dans la méthode Initialize:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

Dans le test d’unité CreateValidContact(), nous nous moquons du comportement d’appeler la couche de service CreateContact() méthode avec la ligne de code suivante:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Cette ligne de code provoque le faux service ContactManager de retourner la valeur vraie lorsque sa méthode CreateContact() est appelée. En nous moquant de la couche de service, nous pouvons tester le comportement de notre contrôleur sans avoir besoin d’exécuter n’importe quel code dans la couche de service.

Le deuxième test unitaire vérifie que l’action Créer() renvoie la vue Créer lorsqu’un contact invalide est transmis à la méthode. Nous faisons en sorte que la méthode CreateContact() de la couche de service renvoie la valeur fausse avec la ligne de code suivante :

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Si la méthode Créer() se comporte comme nous nous y attendons, elle devrait retourner la vue Créer lorsque la couche de service renvoie la valeur fausse. De cette façon, le contrôleur peut afficher les messages d’erreur de validation dans la vue Créer et l’utilisateur a une chance de corriger ces propriétés de contact invalides.

Si vous prévoyez de construire des tests unitaires pour vos contrôleurs, vous devez retourner les noms de vue explicites de vos actions de contrôleur. Par exemple, ne retournez pas une vue comme celle-ci :

Vue de retour()

Au lieu de cela, retournez la vue comme ceci:

Vue de retour ("Créer")

Si vous n’êtes pas explicite lors du retour d’une vue, puis la propriété ViewResult.ViewName retourne une chaîne vide.

**Liste 2 - Controllers-ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons créé des tests unitaires pour notre application Contact Manager. Nous pouvons exécuter ces tests unitaires à tout moment pour vérifier que notre application se comporte toujours de la manière que nous attendons. Les tests unitaires servent de filet de sécurité pour notre application, ce qui nous permet de modifier notre application en toute sécurité à l’avenir.

Nous avons créé deux séries de tests unitaires. Tout d’abord, nous avons testé notre logique de validation en créant des tests unitaires pour notre couche de service. Ensuite, nous avons testé notre logique de contrôle du flux en créant des tests unitaires pour notre couche de contrôleur. Lors de l’essai de notre couche de service, nous avons isolé nos tests pour notre couche de service à partir de notre couche de référentiel en nous moquant de notre couche de référentiel. Lors du test de la couche de contrôleur, nous avons isolé nos tests pour notre couche de contrôleur en se moquant de la couche de service.

Dans la prochaine itération, nous modifions l’application Contact Manager afin qu’elle soutienne les groupes de contact. Nous ajouterons cette nouvelle fonctionnalité à notre application à l’aide d’un processus de conception logicielle appelé développement axé sur les tests.

> [!div class="step-by-step"]
> [Suivant précédent](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Next](iteration-6-use-test-driven-development-vb.md)
