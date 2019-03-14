---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injection de dépendances d’ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Remarque : Ce laboratoire pratique suppose que vous avez une connaissance élémentaire des filtres ASP.NET MVC et ASP.NET MVC 4. Si vous n’avez pas utilisé les filtres ASP.NET MVC 4 avant, nous rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046746"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Injection de dépendances d’ASP.NET MVC 4

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC** et **filtres ASP.NET MVC 4**. Si vous n’avez pas utilisé **filtres ASP.NET MVC 4** auparavant, nous vous recommandons de consulter **filtres d’Action ASP.NET MVC personnalisée** les ateliers pratiques.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à partir d’à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [Injection de dépendances d’ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

Dans **la programmation orientée objet** paradigme, objets fonctionnent conjointement dans un modèle de collaboration où il existe des contributeurs et les consommateurs. Naturellement, ce modèle de communication génère des dépendances entre les objets et composants, devient difficile à gérer la complexité augmente.

![Classe de dépendances et la complexité du modèle](aspnet-mvc-4-dependency-injection/_static/image1.png "classe les dépendances et la complexité du modèle")

*Dépendances de la classe et la complexité du modèle*

Vous avez probablement entendu parler du **modèle de fabrique** et la séparation entre l’interface et l’implémentation à l’aide de services, où les objets de client sont souvent responsables pour l’emplacement du service.

Le modèle d’Injection de dépendance est une implémentation particulière d’Inversion de contrôle. **L’inversion de contrôle (IoC)** signifie que les objets ne créent pas d’autres objets sur lesquels elles s’appuient pour faire leur travail. Au lieu de cela, ils obtiennent les objets dont ils ont besoin d’une source externe (par exemple, un fichier de configuration xml).

**L’Injection de dépendance (DI)** signifie qu’il est fait sans l’intervention de l’objet, généralement par un composant d’infrastructure qui transmet les paramètres de constructeur et définir les propriétés.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Le modèle de conception de dépendance (DI) de l’injection de code

À un niveau élevé, l’objectif de l’Injection de dépendance qui est une classe de client (par exemple, *le dsmessages*) a besoin d’un élément qui satisfait à une interface (par exemple, *IClub*). Il ne soucie pas ce qui est le type concret (par exemple, *WoodClub, IronClub, WedgeClub* ou *PutterClub*), qu’il veut que quelqu'un d’autre pour qui gérer (par exemple, une bonne *plateau*). Le résolveur de dépendance dans ASP.NET MVC peut vous permettre d’inscrire votre logique de dépendance vers un autre emplacement (par exemple, un conteneur ou un *sac de trèfle*).

![Diagramme de l’Injection de dépendance](aspnet-mvc-4-dependency-injection/_static/image2.png "illustration de l’Injection de dépendances")

*Injection de dépendances - analogie de Golf*

Les avantages de l’utilisation d’injection de dépendance et Inversion de contrôle sont les suivantes :

- Réduit les couplages de classe
- Augmente la réutilisation de code
- Améliore la maintenabilité du code
- Améliore le test des applications

> [!NOTE]
> L’Injection de dépendances est parfois comparée avec le modèle de Design Factory abstraite, mais il existe une légère différence entre les deux approches. L’injection de dépendances a une infrastructure fonctionne derrière pour résoudre les dépendances en appelant les fabriques et les services inscrits.


Maintenant que vous comprenez le modèle d’Injection de dépendance, vous allez apprendre tout au long de ce laboratoire pour l’appliquer dans ASP.NET MVC 4. Vous commencerez à l’aide de l’Injection de dépendances dans le **contrôleurs** d’inclure un service d’accès de base de données. Ensuite, vous appliquerez l’Injection de dépendances pour le **vues** pour consommer un service et afficher des informations. Enfin, vous allez étendre l’injection de dépendances pour les filtres ASP.NET MVC 4, injecter un filtre d’action personnalisé dans la solution.

Dans cet atelier pratique, vous allez apprendre comment :

- Intégrer d’ASP.NET MVC 4 avec Unity pour l’Injection de dépendances à l’aide de Packages NuGet
- Utiliser l’Injection de dépendance à l’intérieur d’un contrôleur ASP.NET MVC
- Utiliser l’Injection de dépendance à l’intérieur d’une vue ASP.NET MVC
- Utiliser l’Injection de dépendance à l’intérieur d’un filtre d’Action ASP.NET MVC

> [!NOTE]
> Ce laboratoire est à l’aide de Unity.Mvc3 le NuGet Package pour la résolution des dépendances, mais il est possible de s’adapter à n’importe quel Framework de l’Injection de dépendance pour travailler avec ASP.NET MVC 4.


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Vous devez disposer des éléments suivants pour terminer ce laboratoire :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

**L’installation des extraits de Code**

Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio. Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [annexe b : À l’aide d’extraits de Code](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué par les exercices suivants :

1. [Exercice 1 : Injection d’un contrôleur](#Exercise1)
2. [Exercice 2 : Injection d’une vue](#Exercise2)
3. [Exercice 3 : Injection de filtres](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.


Durée estimée pour effectuer ce laboratoire : **30 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Exercice 1 : Injection d’un contrôleur

Dans cet exercice, vous allez apprendre à utiliser l’Injection de dépendances dans ASP.NET MVC Controllers en intégrant Unity à l’aide d’un NuGet Package. Pour cette raison, vous inclurez des services dans vos contrôleurs MvcMusicStore à séparer la logique de l’accès aux données. Les services créera une nouvelle dépendance dans le constructeur de contrôleur, qui est résolu à l’aide de l’Injection de dépendances à l’aide de **Unity**.

Cette approche est vous montrer comment générer moins des applications couplées sont plus flexibles et plus facile à maintenir et à tester. Vous allez également apprendre à intégrer ASP.NET MVC avec Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Sur le Service de StoreManager

Le Store de musique MVC fourni dans la solution begin maintenant inclut un service qui gère les données de Store contrôleur nommées **StoreService**. Vous trouverez ci-dessous l’implémentation du Service de Store. Notez que toutes les méthodes retournent des entités du modèle.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** à partir du début de la solution consomme **StoreService**. Toutes les références de données ont été supprimées à partir de **StoreController**et il est désormais possible de modifier le fournisseur d’accès aux données en cours sans modifier n’importe quelle méthode consomme **StoreService**.

Vous trouverez ci-dessous la **StoreController** implémentation a une dépendance avec **StoreService** dans le constructeur de classe.

> [!NOTE]
> La dépendance introduite dans cet exercice est liée à **Inversion de contrôle** (IoC).
> 
> Le **StoreController** constructeur de classe reçoit un **IStoreService** paramètre de type, ce qui est essentiel d’effectuer des appels de service à l’intérieur de la classe. Toutefois, **StoreController** n’implémente pas le constructeur par défaut (sans aucun paramètre) qui n’importe quel contrôleur doit comporter pour fonctionner avec ASP.NET MVC.
> 
> Pour résoudre la dépendance, le contrôleur doit être créée par une fabrique abstraite (une classe qui retourne un objet du type spécifié).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Vous obtiendrez une erreur lors de la classe tente de créer le StoreController sans envoyer l’objet de service, car il n’existe aucun constructeur sans paramètre déclaré.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tâche 1 : exécution de l’Application

Dans cette tâche, vous allez exécuter l’application Begin, qui inclut le service dans le contrôleur de Store qui sépare l’accès aux données à partir de la logique d’application.

Lorsque vous exécutez l’application, vous recevrez une exception, comme le service du contrôleur n’est pas passé en tant que paramètre par défaut :

1. Ouvrez le **commencer** solution situé dans **Controller\Begin injection Source\Ex01**.

   1. Vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Appuyez sur **Ctrl + F5** pour exécuter l’application sans débogage. Vous obtiendrez le message d’erreur &quot; **aucun constructeur sans paramètre défini pour cet objet**&quot;:

    ![Erreur lors de l’exécution d’Application ASP.NET MVC commencer](aspnet-mvc-4-dependency-injection/_static/image3.png "erreur lors de l’exécution d’Application ASP.NET MVC commencer")

    *Erreur lors de l’exécution d’Application ASP.NET MVC commencer*
3. Fermez le navigateur.

Dans les étapes suivantes, vous allez travailler sur la Solution du Store musique pour injecter la dépendance qu'a besoin de ce contrôleur.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tâche 2 - y compris de Unity dans MvcMusicStore Solution

Dans cette tâche, vous inclurez **Unity.Mvc3** NuGet Package à la solution.

> [!NOTE]
> Unity.Mvc3 package a été conçu pour ASP.NET MVC 3, mais il est entièrement compatible avec ASP.NET MVC 4.
> 
> Unity est un conteneur d’injection de dépendance de légère et extensible avec prise en charge facultative par exemple et de types. Il est un conteneur à usage général pour une utilisation dans n’importe quel type d’application .NET. Il fournit toutes les fonctionnalités courantes trouvées dans les mécanismes de l’injection de dépendance, y compris : la création d’objets, l’abstraction des exigences par spécification de dépendances au démarrage et de flexibilité, en différant la configuration du composant au conteneur.


1. Installer **Unity.Mvc3** NuGet Package dans le **MvcMusicStore** projet. Pour ce faire, ouvrez le **Console du Gestionnaire de Package** de **vue** | **Windows autres**.
2. Exécutez la commande suivante.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installer le Package NuGet de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "l’installation de Package NuGet de Unity.Mvc3")

    *Installation du Package NuGet Unity.Mvc3*
3. Une fois le **Unity.Mvc3** package est installé, d’Explorer les fichiers et dossiers, il ajoute automatiquement afin de simplifier la configuration de Unity.

    ![Package Unity.Mvc3 installé](aspnet-mvc-4-dependency-injection/_static/image5.png "package Unity.Mvc3 installé")

    *Package Unity.Mvc3 installé*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Tâche 3 : inscription de Unity dans Global.asax.cs Application\_Démarrer

Dans cette tâche, vous mettrez à jour la **Application\_Démarrer** méthode situé dans **Global.asax.cs** pour appeler l’initialiseur de programme d’amorçage de Unity et ensuite, mettez à jour le fichier de programme d’amorçage inscrit le Service et le contrôleur, vous allez utiliser l’Injection de dépendance.

1. Maintenant, vous raccorder le programme d’amorçage qui est le fichier qui initialise le conteneur Unity et résolveur de dépendance. Pour ce faire, ouvrez **Global.asax.cs** et ajoutez le code en surbrillance suivant au sein de la **Application\_Démarrer** (méthode).

    (Code Snippet - *Lab de l’Injection de dépendance ASP.NET - Ex01 - initialiser Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Ouvrez **Bootstrapper.cs** fichier.
3. Incluez les espaces de noms suivants : **MvcMusicStore.Services** et **MusicStore.Controllers**.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex01 - programme d’amorçage Ajout d’espaces de noms*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Remplacez **BuildUnityContainer** de contenu par le code suivant qui enregistre le contrôleur de Store et Service de Store de la méthode.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex01 - Register Store contrôleur et Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’Application

Dans cette tâche, vous allez exécuter l’application pour vérifier qu’il peut maintenant être chargé après avoir inclus Unity.

1. Appuyez sur **F5** pour exécuter l’application, l’application doit maintenant se charge sans afficher de message d’erreur.

    ![Application en cours d’exécution avec l’Injection de dépendance](aspnet-mvc-4-dependency-injection/_static/image6.png "Application en cours d’exécution avec l’Injection de dépendance")

    *Application en cours d’exécution avec l’Injection de dépendances*
2. Accédez à **/Store**. Cette action appelle **StoreController**, qui est maintenant créé à l’aide de **Unity**.

    ![Store de musique MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Store de musique MVC")

    *Store de musique MVC*
3. Fermez le navigateur.

Dans les exercices suivants, vous allez apprendre à étendre la portée de l’Injection de dépendances pour l’utiliser à l’intérieur des vues ASP.NET MVC et les filtres d’Action.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Exercice 2 : Injection d’une vue

Dans cet exercice, vous allez apprendre à utiliser l’Injection de dépendances dans une vue avec les nouvelles fonctionnalités d’ASP.NET MVC 4 pour l’intégration de Unity. Pour ce faire, vous appelez un service personnalisé à l’intérieur de la vue de parcourir Store, qui affiche un message et une image ci-dessous.

Ensuite, vous intégrer le projet Unity et créer un résolveur de dépendance personnalisées pour injecter des dépendances.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tâche 1 : création d’une vue qui utilise un Service

Dans cette tâche, vous allez créer une vue qui effectue un appel de service pour générer une nouvelle dépendance. Le service est composé dans un service de messagerie simple inclus dans cette solution.

1. Ouvrez le **commencer** solution situé dans le **View\Begin injection Source\Ex02** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
      > 
      > Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Inclure le **MessageService.cs** et le **IMessageService.cs** classes situé dans le **Source \Assets** dossier dans **/Services**. Pour ce faire, cliquez sur **Services** dossier et sélectionnez **ajouter un élément existant**. Accédez à l’emplacement des fichiers et les inclure.

    ![Ajout de Service de Message et l’Interface de Service](aspnet-mvc-4-dependency-injection/_static/image8.png "Ajout du Service de Message et l’Interface de Service")

    *Ajout de Message Service et l’Interface de Service*

    > [!NOTE]
    > Le **IMessageService** interface définit deux propriétés implémentées par le **MessageService** classe. Ces propriétés -**Message** et **ImageUrl**-stocker le message et l’URL de l’image à afficher.
3. Créez le dossier **/Pages** dans le projet dossier racine, puis ajoutez la classe existante **MyBasePage.cs** de **Source\Assets**. La page de base de que vous héritera présente la structure suivante.

    ![Dossier pages](aspnet-mvc-4-dependency-injection/_static/image9.png "dossier Pages")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Ouvrez **Browse.cshtml** afficher à partir de **/vues/Store** dossier et faites en sorte qu’il hérite **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. Dans le **Parcourir** afficher, ajouter un appel à **MessageService** pour afficher une image et un message récupéré par le service.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tâche 2 - y compris un résolveur de dépendance personnalisée et un activateur de Page de vue personnalisée

Dans la tâche précédente, vous avez injecté une nouvelle dépendance à l’intérieur d’une vue pour effectuer un appel de service qu’il contient. À présent, vous allez résoudre cette dépendance en implémentant les interfaces de l’Injection de dépendances ASP.NET MVC **IViewPageActivator** et **IDependencyResolver**. Vous allez inclure dans la solution à une implémentation de **IDependencyResolver** qui traitera la récupération de service à l’aide de Unity. Ensuite, vous allez inclure une autre implémentation personnalisée de **IViewPageActivator** interface qui résout la création des vues.

> [!NOTE]
> Depuis ASP.NET MVC 3, l’implémentation de l’Injection de dépendance a simplifié les interfaces pour inscrire les services. **IDependencyResolver** et **IViewPageActivator** font partie des fonctionnalités d’ASP.NET MVC 3 pour l’Injection de dépendances.
> 
> **-IDependencyResolver** interface remplace la précédente IMvcServiceLocator. Les implémenteurs de IDependencyResolver doivent retourner une instance du service ou une collection de service.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interface fournit un contrôle plus précis sur la façon dont les pages de vue sont instanciés par le biais de l’injection de dépendances. Les classes qui implémentent **IViewPageActivator** interface peut créer des instances d’affichages à l’aide des informations de contexte.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Créer le /**fabriques** dossier dans le dossier du projet racine.
2. Inclure **CustomViewPageActivator.cs** à votre solution à partir de **/Sources/ressources/** à **fabriques** dossier. Pour ce faire, cliquez sur le **/Factories** dossier, puis sélectionnez **ajouter | Un élément existant** , puis sélectionnez **CustomViewPageActivator.cs**. Cette classe implémente le **IViewPageActivator** interface pour contenir le conteneur Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** est responsable de la gestion de la création d’une vue à l’aide d’un conteneur Unity.
3. Inclure **UnityDependencyResolver.cs** à partir de fichiers **/Sources/ressources** à **/Factories** dossier. Pour ce faire, cliquez sur le **/Factories** dossier, puis sélectionnez **ajouter | Un élément existant** , puis sélectionnez **UnityDependencyResolver.cs** fichier.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** classe est un DependencyResolver personnalisé pour Unity. Lorsqu’un service est introuvable dans le conteneur Unity, le programme de résolution de base est invocated.

Les deux implémentations seront inscrit pour permettre au modèle de connaître l’emplacement des services et les vues dans la tâche suivante.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tâche 3 : l’inscription pour l’Injection de dépendances au sein du conteneur Unity

Dans cette tâche, vous allez placer toutes les opérations précédentes pour optimiser l’Injection de dépendances.

Jusqu'à présent, votre solution comporte les éléments suivants :

- Un **Parcourir** vue hérite **MyBaseClass** et consomme **MessageService**.
- Une classe intermédiaire -**MyBaseClass**-qui a déclaré pour l’interface de service de l’injection de dépendances.
- Un service - **MessageService** - et son interface **IMessageService**.
- Un résolveur de dépendance personnalisées pour Unity - **UnityDependencyResolver** -qui porte sur la récupération de service.
- Un activateur de Page de vue - **CustomViewPageActivator** -qui crée la page.

Pour injecter **Parcourir** vue, vous allez maintenant inscrire le résolveur de dépendance personnalisées dans le conteneur Unity.

1. Ouvrez **Bootstrapper.cs** fichier.
2. Inscrire une instance de **MessageService** dans le conteneur Unity pour initialiser le service :

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex02 - Register Message Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Ajoutez une référence à **MvcMusicStore.Factories** espace de noms.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex02 - fabriques Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Inscrire **CustomViewPageActivator** comme un activateur de Page de vue dans le conteneur Unity :

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex02 - Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Remplacez le résolveur de dépendance ASP.NET MVC 4 avec une instance de **UnityDependencyResolver**. Pour ce faire, remplacez **Initialise** méthode contenu par le code suivant :

    (Code Snippet - *résolveur de dépendance de mise à jour ASP.NET dépendance Injection Lab - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC fournit une classe de programme de résolution de dépendance par défaut. Pour utiliser des programmes de résolution de dépendance personnalisées que celui que nous avons créé pour unity, ce programme de résolution doit être remplacé.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’Application

Dans cette tâche, vous allez exécuter l’application pour vérifier que le navigateur Store utilise le service et montre l’image et le message récupéré :

1. Appuyez sur **F5** pour exécuter l’application.
2. Cliquez sur **Rock** dans le Menu de Genres et voir comment la **MessageService** a été injecté dans la vue de chargement et le message d’accueil et l’image. Dans cet exemple, nous entrons dans à &quot; **Rock**&quot;:

    ![Store de musique MVC - une Injection de vue](aspnet-mvc-4-dependency-injection/_static/image10.png "Store de musique MVC - une Injection de vue")

    *Store de musique MVC - une Injection de vue*
3. Fermez le navigateur.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Exercice 3 : Injecter des filtres d’Action

Dans l’atelier précédent **filtres d’Action personnalisé** vous avez travaillé avec la personnalisation des filtres et injection de code. Dans cet exercice, vous allez apprendre à injecter des filtres avec l’Injection de dépendances à l’aide du conteneur Unity. Pour ce faire, vous allez ajouter à la solution de musique Store un filtre d’action personnalisé qui analyse l’activité du site.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tâche 1 - y compris le filtre de suivi dans la Solution

Dans cette tâche, vous inclurez dans le Store musique un filtre d’action personnalisé à des événements de trace. En tant que filtre d’action personnalisé concepts sont déjà traitées dans l’atelier précédent &quot;filtres d’Action personnalisée&quot;, vous incluez simplement la classe de filtre à partir du dossier de ressources de ce laboratoire et puis créer un fournisseur de filtre pour Unity :

1. Ouvrir le **commencer** solution situé dans le **Source\Ex03 - injectant des Filter\Begin Action** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
      > 
      > Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Inclure **TraceActionFilter.cs** à partir de fichiers **/Sources/ressources** à **/filtre** dossier.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Ce filtre d’action personnalisé effectue le suivi ASP.NET. Vous pouvez vérifier &quot;local d’ASP.NET MVC 4 et les filtres d’Action dynamique&quot; Lab pour plus de détails sur.
3. Ajoutez la classe vide **FilterProvider.cs** au projet dans le dossier   **/filtre.**
4. Ajouter le **System.Web.Mvc** et **Microsoft.Practices.Unity** espaces de noms dans **FilterProvider.cs**.

    (Code Snippet - *fournisseur filtre ASP.NET dépendance Injection Lab - Ex03 - Ajout d’espaces de noms*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Faites en sorte que la classe hérite **IFilterProvider** Interface.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Ajouter un **IUnityContainer** propriété dans le **FilterProvider** classe et ensuite créer un constructeur de classe pour affecter le conteneur.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - filtre fournisseur constructeur*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Le constructeur de classe de fournisseur de filtre n’est pas Création un **nouvelle** à l’intérieur de l’objet. Le conteneur est passé comme paramètre, et la dépendance est résolue par Unity.
7. Dans le **FilterProvider** de classe, implémentez la méthode **GetFilters** de **IFilterProvider** interface.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - filtre fournisseur GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tâche 2 : inscription et d’activer le filtre

Dans cette tâche, vous allez activer le suivi de site. Pour ce faire, vous inscrirez le filtre dans **Bootstrapper.cs BuildUnityContainer** méthode pour démarrer le suivi :

1. Ouvrez **Web.config** situé dans la racine du projet et activer le suivi de trace au niveau groupe de System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Ouvrez **Bootstrapper.cs** à la racine du projet.
3. Ajoutez une référence à la **MvcMusicStore.Filters** espace de noms.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - programme d’amorçage Ajout d’espaces de noms*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Sélectionnez le **BuildUnityContainer** (méthode) et enregistrez le filtre dans le conteneur Unity. Vous devez inscrire le fournisseur de filtre, ainsi que le filtre d’action.

    (Code Snippet - *ASP.NET dépendance Injection Lab - Ex03 - Register FilterProvider et ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’Application

Dans cette tâche, vous exécutez l’application et que le filtre d’action personnalisé est le suivi de l’activité de test :

1. Appuyez sur **F5** pour exécuter l’application.
2. Cliquez sur **Rock** dans le Menu de Genres. Si vous le souhaitez, vous pouvez accéder à plusieurs genres.

    ![Musique Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Magasin de musique*
3. Accédez à **/trace.axd** pour afficher la Trace de l’Application de page, puis cliquez sur **afficher les détails**.

    ![Journal des traces application](aspnet-mvc-4-dependency-injection/_static/image12.png "journal de suivi d’Application")

    *Journal de suivi d’application*

    ![Trace de l’application - détails de la demande](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - détails de la demande")

    *Trace de l’application - détails de la demande*
4. Fermez le navigateur.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier pratique, vous avez appris à utiliser l’Injection de dépendances dans ASP.NET MVC 4 en intégrant Unity à l’aide d’un NuGet Package. Pour ce faire, vous avez utilisé l’Injection de dépendances à l’intérieur des contrôleurs, des vues et des filtres d’action.

Les concepts suivants ont été traités :

- Fonctionnalités d’ASP.NET MVC 4, Injection de dépendances
- Intégration de Unity à l’aide de NuGet Package de Unity.Mvc3
- Injection de dépendances dans les contrôleurs
- Injection de dépendances dans les vues
- Injection de dépendances des filtres d’Action

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : Installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express pour une vignette de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Annexe b : À l’aide d’extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-dependency-injection/_static/image19.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-dependency-injection/_static/image20.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-dependency-injection/_static/image21.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-dependency-injection/_static/image22.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-dependency-injection/_static/image23.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-dependency-injection/_static/image24.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
