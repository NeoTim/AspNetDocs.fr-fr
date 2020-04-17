---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Activez les tests d’unités automatisés . Microsoft Docs
author: rick-anderson
description: L’étape 12 montre comment développer une suite de tests unitaires automatisés qui vérifient notre fonctionnalité NerdDinner, et qui nous donnera la confiance nécessaire pour apporter des changements...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 7fe84efd9e4cc359c19d5ab9e22c579b80207e9c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541466"
---
# <a name="enable-automated-unit-testing"></a>Activer les tests unitaires automatisés

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 12 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 12 montre comment développer une suite de tests unitaires automatisés qui vérifient notre fonctionnalité NerdDinner, et qui nous donnera la confiance nécessaire pour apporter des modifications et des améliorations à l’application à l’avenir.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner Étape 12: Test unitaire

Développons une suite de tests unitaires automatisés qui vérifient notre fonctionnalité NerdDinner, et qui nous donnera la confiance nécessaire pour apporter des modifications et des améliorations à l’application à l’avenir.

### <a name="why-unit-test"></a>Pourquoi l’essai unitaire?

Sur le lecteur dans le travail un matin, vous avez un éclair soudain d’inspiration sur une application que vous travaillez sur. Vous vous rendez compte qu’il y a un changement que vous pouvez mettre en œuvre qui rendra l’application considérablement meilleure. Il peut s’agir d’une refonte qui nettoie le code, ajoute une nouvelle fonctionnalité ou corrige un bogue.

La question qui vous confronte lorsque vous arrivez à votre ordinateur est - "comment est-il sûr de faire cette amélioration?" Que se passe-t-il si l’apport du changement a des effets secondaires ou casse quelque chose? Le changement peut être simple et ne prendre que quelques minutes à implémenter, mais que faire si elle prend des heures pour tester manuellement tous les scénarios d’application? Que faire si vous oubliez de couvrir un scénario et une application cassée entre en production? Cette amélioration vaut-elle vraiment tout l’effort?

Les tests unitaires automatisés peuvent fournir un filet de sécurité qui vous permet d’améliorer continuellement vos applications et d’éviter d’avoir peur du code sur lequel vous travaillez. Avoir des tests automatisés qui vérifient rapidement la fonctionnalité vous permet de coder en toute confiance et vous donner les moyens d’apporter des améliorations que vous n’auriez peut-être pas ressenties à l’aise de faire. Ils aident également à créer des solutions plus maintenables et qui ont une durée de vie plus longue - ce qui conduit à un retour sur investissement beaucoup plus élevé.

Le cadre ASP.NET MVC facilite et naturel l’unité des fonctionnalités d’application de test. Il permet également un flux de travail de développement piloté par test (TDD) qui permet le développement basé sur le test d’abord.

### <a name="nerddinnertests-project"></a>Projet NerdDinner.Tests

Lorsque nous avons créé notre application NerdDinner au début de ce tutoriel, nous avons été invités par un dialogue demandant si nous voulions créer un projet de test unitaire pour aller de pair avec le projet d’application:

![](enable-automated-unit-testing/_static/image1.png)

Nous avons gardé le bouton radio « Oui, créer un projet de test unitaire » sélectionné, ce qui a permis d’ajouter un projet « NerdDinner.Tests » à notre solution :

![](enable-automated-unit-testing/_static/image2.png)

Le projet NerdDinner.Tests fait référence à l’assemblage du projet d’application NerdDinner et nous permet d’y ajouter facilement des tests automatisés qui vérifient la fonctionnalité de l’application.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Création de tests unitaires pour notre classe de modèle de dîner

Ajoutons quelques tests à notre projet NerdDinner.Tests qui vérifient la classe de dîner que nous avons créée lorsque nous avons construit notre couche de modèle.

Nous allons commencer par créer un nouveau dossier dans notre projet de test appelé "Modèles" où nous allons placer nos tests liés au modèle. Nous allons ensuite cliquer à droite sur le dossier et choisir la commande de menu **Add-&gt;New Test.** Cela fera ressortir le dialogue "Ajouter un nouveau test".

Nous choisirons de créer un « test unitaire » et de l’appeler « DinnerTest.cs » :

![](enable-automated-unit-testing/_static/image3.png)

Lorsque nous clions sur le bouton "ok" Visual Studio ajouter (et ouvrir) un fichier DinnerTest.cs au projet:

![](enable-automated-unit-testing/_static/image4.png)

Le modèle de test d’unité Visual Studio par défaut a un tas de code de plaque de chaudière en elle que je trouve un peu désordonné. Nettoyons-le pour simplement contenir le code ci-dessous :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

L’attribut [TestClass] de la classe DinnerTest ci-dessus l’identifie comme une classe qui contiendra des tests, ainsi que l’initialisation et le code de démontage optionnels. Nous pouvons définir les tests en son sein en y ajoutant des méthodes publiques qui ont un attribut [TestMethod] sur eux.

Voici le premier de deux tests, nous allons ajouter cet exercice de notre classe de dîner. Le premier test vérifie que notre dîner est invalide si un nouveau dîner est créé sans que toutes les propriétés soient définies correctement. Le deuxième test vérifie que notre dîner est valide lorsqu’un dîner a toutes les propriétés définies avec des valeurs valides:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Vous remarquerez ci-dessus que nos noms de test sont très explicites (et un peu verbeux). Nous le faisons parce que nous pourrions finir par créer des centaines ou des milliers de petits tests, et nous voulons qu’il soit facile de déterminer rapidement l’intention et le comportement de chacun d’eux (surtout lorsque nous regardons à travers une liste d’échecs dans un coureur d’essai). Les noms de test doivent être nommés d’après la fonctionnalité qu’ils testent. Ci-dessus, nous utilisons\_\_un "Nom should Verb" modèle de nommage.

Nous structurons les tests à l’aide du modèle de test « AAA » qui signifie « Organiser, agir, affirmer » :

- Arrangement : Configurer l’unité testée
- Loi : Exercez l’unité à l’essai et capturez les résultats
- Assert: Vérifier le comportement

Lorsque nous écrivons des tests, nous voulons éviter que les tests individuels en fassent trop. Au lieu de cela, chaque test ne devrait vérifier qu’un seul concept (ce qui facilitera l’identification de la cause des défaillances). Une bonne ligne directrice est d’essayer d’avoir seulement une seule déclaration d’affirmation pour chaque test. Si vous avez plus d’une déclaration d’affirmation dans une méthode de test, assurez-vous qu’ils sont tous utilisés pour tester le même concept. En cas de doute, faites un autre test.

### <a name="running-tests"></a>Exécution des tests

Visual Studio 2008 Professional (et éditions supérieures) comprend un coureur d’essai intégré qui peut être utilisé pour exécuter des projets de test d’unité de studio visuel au sein de l’IDE. Nous pouvons sélectionner les **tests Test-&gt;Run-&gt;All Tests dans** la commande du menu Solution (ou le type Ctrl R, A) pour exécuter tous nos tests unitaires. Ou bien, nous pouvons positionner notre curseur dans une classe de test ou une méthode de test spécifique et utiliser la commande de menu **Test-&gt;Run-&gt;In Current Context** (ou type Ctrl R, T) pour exécuter un sous-ensemble des tests unitaires.

Positionnez notre curseur dans la classe DinnerTest et tapez "Ctrl R, T" pour exécuter les deux tests que nous venons de définir. Lorsque nous faisons cela, une fenêtre "Test Results" apparaîtra dans Visual Studio et nous verrons les résultats de notre exécution de test énumérés en elle:

![](enable-automated-unit-testing/_static/image5.png)

*Remarque : La fenêtre de résultats des tests VS n’affiche pas la colonne Nom de classe par défaut. Vous pouvez l’ajouter en cliquant à droite dans la fenêtre des résultats de test et en utilisant la commande de menu Add/Remove Columns.*

Nos deux tests n’ont pris qu’une fraction de seconde à exécuter - et comme vous pouvez le voir, ils ont tous deux passé. Nous pouvons maintenant continuer et les augmenter en créant des tests supplémentaires qui vérifient des validations de règles spécifiques, ainsi que couvrir les deux méthodes d’aide - IsUserHost() et IsUserRegistered() - que nous avons ajouté à la classe Dîner. Le fait d’avoir tous ces tests en place pour la classe Dîner rendra beaucoup plus facile et plus sûr d’y ajouter de nouvelles règles et validations d’affaires à l’avenir. Nous pouvons ajouter notre nouvelle logique de règle à Dîner, puis en quelques secondes vérifier qu’il n’a pas cassé l’une de nos fonctionnalités logiques précédentes.

Remarquez comment l’utilisation d’un nom de test descriptif permet de comprendre facilement ce que chaque test vérifie. Je recommande d’utiliser la commande de menu&gt; **Tools-&gt;Options,** d’ouvrir l’écran de configuration d’exécution de test d’outils de test et de vérifier le « Double-clicking a failed or unconclusive unit test result displays the point of failure in the test » case. Cela vous permettra de cliquer deux fois sur un échec dans la fenêtre des résultats des tests et de sauter immédiatement à l’échec d’affirmation.

### <a name="creating-dinnerscontroller-unit-tests"></a>Création de tests d’unité DinnersController

Créons maintenant des tests unitaires qui vérifient notre fonctionnalité DinnersController. Nous commencerons par cliquer à droite sur le dossier "Controllers" dans notre projet Test, puis choisirons la commande de menu **Add-&gt;New Test.** Nous allons créer un "Test Unitaire" et le nommer "DinnersControllerTest.cs".

Nous allons créer deux méthodes de test qui vérifient la méthode d’action Détails() sur le DinnersController. Le premier vérifiera qu’une vue est retournée lorsqu’un dîner existant est demandé. Le second vérifiera qu’une vue « Nonfound » est retournée lorsqu’un dîner inexistant est demandé :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Le code ci-dessus compile propre. Lorsque nous passons les tests, cependant, ils échouent tous les deux:

![](enable-automated-unit-testing/_static/image6.png)

Si nous regardons les messages d’erreur, nous verrons que la raison pour laquelle les tests ont échoué était parce que notre classe DinnersRepository n’a pas été en mesure de se connecter à une base de données. Notre application NerdDinner utilise une chaîne de connexion à un fichier local\_SQL Server Express qui vit sous l’annuaire «App Data» du projet d’application NerdDinner. Parce que notre projet NerdDinner.Tests compile et fonctionne dans un répertoire différent puis le projet d’application, l’emplacement relatif du chemin de notre chaîne de connexion est incorrect.

Nous *pourrions* corriger cela en copiant le fichier de base de données SQL Express à notre projet de test, puis y ajouter une chaîne de connexion de test appropriée dans l’App.config de notre projet d’essai. Cela permettrait d’obtenir les tests ci-dessus débloqué et en cours d’exécution.

Le code de test unitaire à l’aide d’une véritable base de données, cependant, apporte avec lui un certain nombre de défis. Plus précisément :

- Il ralentit considérablement le temps d’exécution des tests unitaires. Plus il faut de temps pour exécuter des tests, moins vous êtes susceptible de les exécuter fréquemment. Idéalement, vous voulez que vos tests unitaires soient en mesure d’être exécutés en quelques secondes - et que ce soit quelque chose que vous faites aussi naturellement que la compilation du projet.
- Il complique la logique de configuration et de nettoyage dans les tests. Vous voulez que chaque test unitaire soit isolé et indépendant des autres (sans effets secondaires ni dépendances). Lorsque vous travaillez contre une véritable base de données, vous devez être conscient de l’état et la réinitialiser entre les tests.

Examinons un modèle de conception appelé «injection de dépendance» qui peut nous aider à contourner ces questions et éviter la nécessité d’utiliser une véritable base de données avec nos tests.

### <a name="dependency-injection"></a>Injection de dépendances

En ce moment DinnersController est étroitement "couplé" à la classe DinnerRepository. Le « couplage » désigne une situation où une classe s’appuie explicitement sur une autre classe pour travailler :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Parce que la classe DinnerRepository nécessite l’accès à une base de données, la dépendance étroitement couplée de la classe DinnersController a sur le DinnerRepository finit par nous obliger à avoir une base de données afin que les méthodes d’action DinnersController à tester.

Nous pouvons contourner cela en utilisant un modèle de conception appelé « injection de dépendance » - qui est une approche où les dépendances (comme les classes de dépôt qui fournissent l’accès aux données) ne sont plus implicitement créées dans les classes qui les utilisent. Au lieu de cela, les dépendances peuvent être explicitement transmises à la classe qui les utilise à l’aide d’arguments constructeurs. Si les dépendances sont définies à l’aide d’interfaces, nous avons alors la flexibilité de passer dans les implémentations de dépendance "faux" pour les scénarios de test unitaire. Cela nous permet de créer des implémentations de dépendance spécifiques aux tests qui ne nécessitent pas réellement l’accès à une base de données.

Pour voir cela en action, mettons en œuvre l’injection de dépendance avec nos DînersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraction d’une interface IDinnerRepository

Notre première étape sera de créer une nouvelle interface IDinnerRepository qui résume le contrat de dépôt dont nos contrôleurs ont besoin pour récupérer et mettre à jour les dîners.

Nous pouvons définir ce contrat d’interface manuellement en cliquant à droite sur le dossier «Modèles», puis en choisissant la commande de menu **Add-&gt;New Item** et en créant une nouvelle interface nommée IDinnerRepository.cs.

Alternativement, nous pouvons utiliser les outils de refactorisation intégrés dans Visual Studio Professional (et des éditions supérieures) pour extraire et créer une interface pour nous à partir de notre classe existante DinnerRepository. Pour extraire cette interface à l’aide de VS, il suffit de placer le curseur dans l’éditeur de texte sur la classe DinnerRepository, puis cliquez à droite et choisissez la commande **refactor-&gt;Extract Interface** menu:

![](enable-automated-unit-testing/_static/image7.png)

Cela lancera le dialogue "Extract Interface" et nous incitera à créer le nom de l’interface. Il sera par défaut à IDinnerRepository et sélectionnera automatiquement toutes les méthodes publiques sur la classe existante DinnerRepository à ajouter à l’interface:

![](enable-automated-unit-testing/_static/image8.png)

Lorsque nous cliquez sur le bouton "ok", Visual Studio ajoutera une nouvelle interface IDinnerRepository à notre application :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Et notre classe DînerRepository existante sera mise à jour afin qu’il implémente l’interface:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Mise à jour DinnersController pour soutenir l’injection de constructeurs

Nous allons maintenant mettre à jour la classe DinnersController pour utiliser la nouvelle interface.

Actuellement DinnersController est hard-coded tel que son "dînerRepository" champ est toujours un cours DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Nous allons le changer de sorte que le champ "dînerRepository" est de type IDinnerRepository au lieu de DinnerRepository. Nous ajouterons ensuite deux constructeurs publics DinnersController. L’un des constructeurs permet de passer un IDinnerRepository comme argument. L’autre est un constructeur par défaut qui utilise notre implémentation existante DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Étant donné que ASP.NET MVC crée par défaut des classes de contrôleurs à l’aide de constructeurs par défaut, notre DinnersController à l’heure d’exécution continuera d’utiliser la classe DinnerRepository pour effectuer l’accès aux données.

Nous pouvons maintenant mettre à jour nos tests unitaires, cependant, pour passer dans un "faux" rédilitaire de dîner mise en œuvre en utilisant le constructeur de paramètres. Ce « faux » dépôt de dîner n’aura pas besoin d’accéder à une véritable base de données, et utilisera plutôt des données d’échantillons en mémoire.

#### <a name="creating-the-fakedinnerrepository-class"></a>Création de la classe FakeDinnerRepository

Créons un cours FakeDinnerRepository.

Nous allons commencer par créer un répertoire "Fakes" dans notre projet NerdDinner.Tests, puis ajouter une nouvelle classe FakeDinnerRepository à elle (clic droit sur le dossier et choisir **Add-&gt;Nouvelle Classe):**

![](enable-automated-unit-testing/_static/image9.png)

Nous mettrons à jour le code afin que la classe FakeDinnerRepository implémente l’interface IDinnerRepository. Nous pouvons alors cliquer à droite sur elle et choisir la commande de menu de contexte "Implement interface IDinnerRepository" :

![](enable-automated-unit-testing/_static/image10.png)

Cela amènera Visual Studio à ajouter automatiquement tous les membres de l’interface IDinnerRepository à notre classe FakeDinnerRepository avec par défaut "stub out" implémentations:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Nous pouvons ensuite mettre à jour la mise en œuvre&lt;&gt; FakeDinnerRepository pour travailler hors d’une collection de dîner liste en mémoire passé à elle comme un argument constructeur:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Nous avons maintenant une fausse implémentation IDinnerRepository qui ne nécessite pas une base de données, et peut plutôt travailler sur une liste de mémoire des objets Dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Utilisation du FakeDinnerRepository avec des tests unitaires

Revenons aux tests de l’unité DinnersController qui ont échoué plus tôt parce que la base de données n’était pas disponible. Nous pouvons mettre à jour les méthodes de test pour utiliser un FakeDinnerRepository peuplé de données de dîner en mémoire de l’échantillon De DinnersController en utilisant le code ci-dessous:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Et maintenant, quand nous passons ces tests, ils réussissent tous les deux:

![](enable-automated-unit-testing/_static/image11.png)

Le meilleur de tous, ils ne prennent qu’une fraction de seconde pour courir, et ne nécessitent aucune configuration compliquée / logique de nettoyage. Nous pouvons maintenant unir le test de tous nos codes de méthode d’action DinnersController (y compris la liste, le pagination, les détails, créer, mettre à jour et supprimer) sans jamais avoir besoin de se connecter à une véritable base de données.

| **Sujet secondaire : Cadres d’injection de dépendance** |
| --- |
| Effectuer l’injection de dépendance manuelle (comme nous sommes ci-dessus) fonctionne très bien, mais devient plus difficile à maintenir que le nombre de dépendances et de composants dans une application augmente. Plusieurs cadres d’injection de dépendance existent pour .NET qui peut aider à fournir encore plus de flexibilité de gestion de la dépendance. Ces cadres, aussi parfois appelés contenants « Inversion de contrôle » (IoC), fournissent des mécanismes qui permettent un niveau supplémentaire de prise en charge de configuration pour spécifier et transmettre des dépendances aux objets en cours d’exécution (le plus souvent à l’aide d’injection de constructeurs). Parmi les cadres les plus populaires de l’OSS Dependency Injection / IOC dans .NET, mentionnons : AutoFac, Ninject, Spring.NET, StructureMap et Windsor. ASP.NET MVC expose les API d’extétabilité qui permettent aux développeurs de participer à la résolution et à l’instantanéisation des contrôleurs, et qui permettent d’intégrer proprement les cadres d’injection de dépendance / IoC dans ce processus. L’utilisation d’un cadre DI/CIO nous permettrait également de supprimer le constructeur par défaut de notre DinnersController, ce qui supprimerait complètement le couplage entre lui et le DinnerRepository. Nous n’utiliserons pas d’injection de dépendance / cadre IOC avec notre application NerdDinner. Mais c’est quelque chose que nous pourrions envisager pour l’avenir si la base de code NerdDinner et les capacités ont augmenté. |

### <a name="creating-edit-action-unit-tests"></a>Création de tests d’unités d’action d’édition

Créons maintenant des tests unitaires qui vérifient la fonctionnalité Edit du DinnersController. Nous commencerons par tester la version HTTP-GET de notre action Edit :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Nous allons créer un test qui vérifie qu’une vue soutenue par un objet DinnerFormViewModel est rendue lorsque un dîner valide est demandé:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Lorsque nous effectuons le test, cependant, nous constaterons qu’il échoue parce qu’une exception de référence nulle est jetée lorsque la méthode Edit accède à la propriété User.Identity.Name pour effectuer le Dinner.IsHostedBy() vérifier.

L’objet utilisateur de la classe de base Controller encapsule les détails sur l’utilisateur connecté, et est peuplé de ASP.NET MVC lorsqu’il crée le contrôleur au moment de l’exécution. Parce que nous testons le DinnersController en dehors d’un environnement de serveur Web, l’objet utilisateur n’est pas défini (d’où l’exception de référence nulle).

### <a name="mocking-the-useridentityname-property"></a>Mocking la propriété User.Identity.Name

Les cadres moqueurs facilitent les tests en nous permettant de créer dynamiquement de fausses versions d’objets dépendants qui prennent en charge nos tests. Par exemple, nous pouvons utiliser un cadre de moquerie dans notre test d’action Edit pour créer dynamiquement un objet utilisateur que notre DinnersController peut utiliser pour rechercher un nom d’utilisateur simulé. Cela évitera une référence nulle d’être jeté lorsque nous exécuteons notre test.

Il existe de nombreux cadres moqueurs .NET qui peuvent être utilisés avec ASP.NET MVC [http://www.mockframeworks.com/](http://www.mockframeworks.com/)(vous pouvez voir une liste d’entre eux ici: ). Pour tester notre application NerdDinner, nous allons utiliser un cadre de moquerie open [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)source appelé "Moq", qui peut être téléchargé gratuitement à partir de .

Une fois téléchargé, nous ajouterons une référence dans notre projet NerdDinner.Tests à l’assemblage Moq.dll :

![](enable-automated-unit-testing/_static/image12.png)

Nous ajouterons ensuite une méthode d’aide "CreateDinnersControllerAs(username)" à notre classe de test qui prend un nom d’utilisateur comme paramètre, et qui "mocks" ensuite la propriété User.Identity.Name sur l’instance DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Ci-dessus, nous utilisons Moq pour créer un objet Mock qui simule un objet ControllerContext (qui est ce que ASP.NET MVC passe aux classes Controller pour exposer des objets en temps d’exécution comme l’utilisateur, la demande, la réponse et la session). Nous appelons la méthode "SetupGet" sur le Mock pour indiquer que la propriété HttpContext.User.Identity.Name sur ControllerContext devrait retourner la chaîne de nom d’utilisateur que nous avons passé à la méthode d’aide.

Nous pouvons nous moquer d’un certain nombre de propriétés et de méthodes ControllerContext. Pour illustrer cela, j’ai également ajouté un appel SetupGet () pour la propriété Request.IsAuthenticated (qui n’est pas réellement nécessaire pour les tests ci-dessous - mais qui aide à illustrer comment vous pouvez se moquer des propriétés de la demande). Lorsque nous avons terminé, nous assignons un exemple de la maquette ControllerContext au DinnersController notre méthode d’aide retourne.

Nous pouvons maintenant écrire des tests unitaires qui utilisent cette méthode d’aide pour tester les scénarios d’édition impliquant différents utilisateurs :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Et maintenant, quand nous passons les tests, ils passent:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Test updateModel() scénarios

Nous avons créé des tests qui couvrent la version HTTP-GET de l’action Edit. Créons maintenant quelques tests qui vérifient la version HTTP-POST de l’action Edit :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Le nouveau scénario de test intéressant pour nous de prendre en charge avec cette méthode d’action est son utilisation de la méthode d’aide UpdateModel() sur la classe de base Controller. Nous utilisons cette méthode d’aide pour lier les valeurs de formulaire-post à notre instance d’objet de dîner.

Voici deux tests qui démontrent comment nous pouvons fournir des valeurs affichées de formulaire pour la méthode d’aide UpdateModel() à utiliser. Nous le ferons en créant et en remplissant un objet FormCollection, puis en l’assignant à la propriété "ValueProvider" sur le Contrôleur.

Le premier test vérifie que sur un enregistrement réussi du navigateur est redirigé vers l’action des détails. Le deuxième test vérifie que lorsque l’entrée invalide est affichée, l’action redisjoue à nouveau la vue d’édition avec un message d’erreur.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Résumé des tests

Nous avons couvert les concepts de base impliqués dans les classes de contrôleurs d’essai unitaires. Nous pouvons utiliser ces techniques pour créer facilement des centaines de tests simples qui vérifient le comportement de notre application.

Parce que nos tests de contrôleur et de modèle ne nécessitent pas une véritable base de données, ils sont extrêmement rapides et faciles à exécuter. Nous serons en mesure d’exécuter des centaines de tests automatisés en quelques secondes, et immédiatement obtenir des commentaires quant à savoir si un changement que nous avons fait cassé quelque chose. Cela nous aidera à nous donner la confiance nécessaire pour améliorer, refactoriser et affiner continuellement notre application.

Nous avons couvert les tests comme le dernier sujet de ce chapitre - mais pas parce que le test est quelque chose que vous devriez faire à la fin d’un processus de développement! Au contraire, vous devriez écrire des tests automatisés le plus tôt possible dans votre processus de développement. Cela vous permet d’obtenir des commentaires immédiats au fur et à mesure que vous développez, vous aide à réfléchir aux scénarios de cas d’utilisation de votre application et vous guide pour concevoir votre application avec des superpositions propres et des accouplements à l’esprit.

Un chapitre ultérieur du livre discutera du développement piloté par les tests (TDD) et de la façon de l’utiliser avec ASP.NET MVC. TDD est une pratique de codage itératif où vous écrivez d’abord les tests que votre code résultant satisfera. Avec TDD, vous commencez chaque fonctionnalité en créant un test qui vérifie les fonctionnalités que vous êtes sur le point d’implémenter. L’écriture du test unitaire permet d’abord de vous assurer que vous comprenez clairement la fonctionnalité et comment elle est censée fonctionner. Ce n’est qu’après l’écriture du test (et que vous avez vérifié qu’il échoue) que vous implémentez ensuite la fonctionnalité réelle que le test vérifie. Parce que vous avez déjà passé du temps à réfléchir au cas d’utilisation de la façon dont la fonctionnalité est censée fonctionner, vous aurez une meilleure compréhension des exigences et de la meilleure façon de les mettre en œuvre. Lorsque vous avez terminé avec la mise en œuvre, vous pouvez ré-exécuter le test et obtenir des commentaires immédiats quant à savoir si la fonctionnalité fonctionne correctement. Nous couvrirons plus de TDD dans le chapitre 10.

### <a name="next-step"></a>étape suivante

Quelques commentaires de conclusion finale.

> [!div class="step-by-step"]
> [Suivant précédent](use-ajax-to-implement-mapping-scenarios.md)
> [Next](nerddinner-wrap-up.md)
