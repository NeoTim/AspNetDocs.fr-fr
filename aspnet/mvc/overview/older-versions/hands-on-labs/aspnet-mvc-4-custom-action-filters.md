---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtres d’Action personnalisés ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fournit des filtres d’Action pour exécuter la logique de filtrage avant ou après l’appel d’une méthode d’action. Filtres d’action sont des attributs personnalisés tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 32587c7b0fd3075cd46678922b40bda2019f3a26
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59381131"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtres d’actions personnalisés d’ASP.NET MVC 4

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

ASP.NET MVC fournit des filtres d’Action pour exécuter la logique de filtrage avant ou après l’appel d’une méthode d’action. Filtres d’action sont des attributs personnalisés qui fournissent un moyen déclaratif pour ajouter un comportement pré et post-action aux méthodes d’action du contrôleur.

Dans ce laboratoire pratique, vous allez créer un attribut de filtre d’action personnalisée dans la solution MvcMusicStore pour intercepter les demandes du contrôleur et les journaux de l’activité d’un site dans une table de base de données. Vous serez en mesure d’ajouter votre filtre de journalisation par injection à n’importe quel contrôleur ou d’action. Enfin, vous voyez s’afficher le journal qui affiche la liste des visiteurs.

Ce laboratoire pratique suppose que vous avez une connaissance élémentaire de **ASP.NET MVC**. Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de consulter **principes de base ASP.NET MVC 4** les ateliers pratiques.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à partir d’à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [filtres d’Action ASP.NET MVC 4 personnalisé](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier pratique, vous allez apprendre comment :

- Créer un attribut de filtre d’action personnalisée pour étendre les fonctionnalités de filtrage
- Appliquer un attribut de filtre personnalisé à un niveau spécifique de l’injection de code
- Inscrire un filtre d’action personnalisée dans le monde entier

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

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [annexe c : À l’aide d’extraits de Code](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué par les exercices suivants :

1. [Exercice 1 : Enregistrement des actions](#Exercise1)
2. [Exercice 2 : La gestion de plusieurs filtres d’Action](#Exercise2)

Durée estimée pour effectuer ce laboratoire : **30 minutes**.

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Exercice 1 : Enregistrement des Actions

Dans cet exercice, vous allez apprendre à créer un filtre de journal d’action personnalisé à l’aide de fournisseurs de filtres ASP.NET MVC 4. Pour ce faire, vous appliquerez un filtre de journalisation sur le site du Store musique qui enregistre toutes les activités dans les contrôleurs sélectionnés.

Le filtre étendra **ActionFilterAttributeClass** et remplacer **OnActionExecuting** méthode pour intercepter chaque requête, puis effectuez les actions de journalisation. Informations de contexte sur les requêtes HTTP, l’exécution de méthodes, les résultats et les paramètres est fournie par ASP.NET MVC **ActionExecutingContext** classe **.**

> [!NOTE]
> ASP.NET MVC 4 a également des fournisseurs de filtres par défaut que vous pouvez utiliser sans créer un filtre personnalisé. ASP.NET MVC 4 fournit les types de filtres suivants :
> 
> - **Autorisation** filtrer, ce qui rend les décisions de sécurité sur la nécessité d’exécuter une méthode d’action, telles que l’exécution de l’authentification ou la validation des propriétés de la requête.
> - **Action** filtre, qui encapsule l’exécution de méthode d’action. Ce filtre peut effectuer un traitement supplémentaire, tel que fournir des données supplémentaires à la méthode d’action, inspecter la valeur de retour ou annuler l’exécution de la méthode d’action
> - **Résultat** filtre, qui encapsule l’exécution de l’objet ActionResult. Ce filtre peut exécuter d’autres traitements du résultat, telles que la modification de la réponse HTTP.
> - **Exception** filtre, qui s’exécute si une exception non gérée levée dans une méthode d’action, en commençant par les filtres d’autorisation et se terminant par l’exécution du résultat. Filtres d’exception peuvent être utilisés pour des tâches telles que la journalisation ou l’affichage d’une page d’erreur.
> 
> Pour plus d’informations sur les fournisseurs de filtres, visitez ce lien MSDN : ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>À propos de la fonctionnalité de journalisation de l’Application du Store musique MVC

Cette solution Music Store a une nouvelle table de modèle de données pour la journalisation du site, **ActionLog**, avec les champs suivants : Nom du contrôleur qui a reçu une demande, l’appelées action, l’adresse IP du Client et l’horodatage.

![Modèle de données. Table ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modèle de données. Table ActionLog.")

*Modèle de données - ActionLog table*

La solution fournit une vue de MVC ASP.NET pour le journal des actions qui se trouve à **MvcMusicStores/vues/ActionLog**:

![Affichage du journal action](aspnet-mvc-4-custom-action-filters/_static/image2.png "affichage du journal des actions")

*Affichage de journal d’action*

Avec cette structure de données, tout le travail portent sur interrompre les demande du contrôleur et l’exécution de la journalisation à l’aide de filtrage personnalisé.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tâche 1 : création d’un filtre personnalisé pour intercepter les demande d’un contrôleur

Dans cette tâche, vous allez créer une classe d’attributs de filtre personnalisé qui contiendra la logique de journalisation. Pour ce faire, vous allez étendre ASP.NET MVC **ActionFilterAttribute** et implémentez l’interface **IActionFilter**.

> [!NOTE]
> Le **ActionFilterAttribute** est la classe de base pour tous les filtres d’attribut. Il fournit les méthodes suivantes pour exécuter une logique spécifique après et avant l’exécution de l’action de contrôleur :
> 
> - **OnActionExecuting**(filterContext ActionExecutingContext) : Juste avant l’appel de la méthode d’action.
> - **OnActionExecuted**(filterContext ActionExecutedContext) : Une fois que la méthode d’action est appelée et avant l’exécution du résultat (avant le rendu de la vue).
> - **OnResultExecuting**(ResultExecutingContext filterContext) : Juste avant l’exécution du résultat (avant le rendu de la vue).
> - **OnResultExecuted**(filterContext ResultExecutedContext) : Une fois l’exécution du résultat (une fois que la vue est restituée).
> 
> En substituant une de ces méthodes dans une classe dérivée, vous pouvez exécuter votre propre code de filtrage.


1. Ouvrez le **commencer** solution situé dans **\Source\Ex01-LoggingActions\Begin** dossier.

   1. Vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
      > 
      > Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Ajoutez une nouvelle classe c# dans le **filtres** dossier et nommez-le *CustomActionFilter.cs*. Ce dossier stocke tous les filtres personnalisés.
3. Ouvrez **CustomActionFilter.cs** et ajoutez une référence à **System.Web.Mvc** et **MvcMusicStore.Models** espaces de noms :

    (Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Hériter la **CustomActionFilter** classe **ActionFilterAttribute** et apportez **CustomActionFilter** implémentent **IActionFilter** interface.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Rendre **CustomActionFilter** classe substituer la méthode **OnActionExecuting** et ajouter la logique nécessaire pour journaliser l’exécution du filtre. Pour ce faire, ajoutez le code en surbrillance suivant au sein de **CustomActionFilter** classe.

    (Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** à l’aide de la méthode **Entity Framework** pour ajouter un nouvel historique ActionLog. Il crée et remplit une instance d’entité avec les informations de contexte à partir de **filterContext**.
    > 
    > Vous pouvez en savoir plus sur **ControllerContext** classe au [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tâche 2 : injection d’un intercepteur de Code dans la classe de contrôleur Store

Dans cette tâche, vous allez ajouter le filtre personnalisé en injectant à toutes les classes de contrôleur et les actions de contrôleur qui seront enregistrées. Pour les besoins de cet exercice, la classe de contrôleur de Store aura un journal.

La méthode **OnActionExecuting** de **ActionLogFilterAttribute** filtre personnalisé s’exécute lorsqu’un élément injecté est appelé.

Il est également possible d’intercepter une méthode de contrôleur spécifique.

1. Ouvrez le **StoreController** à **MvcMusicStore\Controllers** et ajoutez une référence à la **filtres** espace de noms :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Injecter le filtre personnalisé **CustomActionFilter** dans **StoreController** classe en ajoutant **[CustomActionFilter]** attribut avant la déclaration de classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Lorsqu’un filtre est injecté dans une classe de contrôleur, tous ses actions sont également injectées. Si vous souhaitez appliquer le filtre uniquement pour un ensemble d’actions, vous seriez obligé d’injecter **[CustomActionFilter]** pour chacun d'entre eux :
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’Application

Dans cette tâche, vous allez tester que le filtre de journalisation fonctionne. Vous allez démarrer l’application et visitez le magasin, puis vous allez vérifier les activités journalisées.

1. Appuyez sur **F5** pour exécuter l’application.
2. Accédez à **/ActionLog** pour afficher l’état initial de vue de journal :

    ![État de mise hors tension avant l’activité de la page du journal](aspnet-mvc-4-custom-action-filters/_static/image3.png "état mise hors tension avant l’activité de la page du journal")

    *État de suivi du journal avant l’activité de la page*

   > [!NOTE]
   > Par défaut, elle affichera toujours un élément qui est généré lors de la récupération des genres existants pour le menu.
   > 
   > Pour des raisons de simplicité nous nettoyons le **ActionLog** chaque fois que l’application s’exécute, elle s’affiche uniquement les journaux de vérification de chaque tâche particulière la table.
   > 
   > Vous devrez peut-être supprimer le code suivant à partir de la **Session\_Démarrer** (méthode) (dans le **Global.asax** classe), afin d’enregistrer un journal d’historique pour toutes les actions exécutées dans le Store Contrôleur.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.
4. Accédez à **/ActionLog** et si le journal est vide press **F5** pour actualiser la page. Vérifiez que vos visites ont été suivies :

    ![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image4.png "journal des actions avec l’activité connectée")

    *Journal des actions avec l’activité connectée*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Exercice 2 : La gestion de plusieurs filtres d’Action

Dans cet exercice, vous ajoutez un deuxième filtre d’Action personnalisé à la classe StoreController et définir l’ordre spécifique dans lequel les deux filtres seront exécutés. Ensuite, vous mettrez à jour le code pour enregistrer le filtre dans le monde entier.

Il existe différentes options à prendre en compte lors de la définition de l’ordre d’exécution des filtres. Par exemple, la propriété de l’ordre et la portée des filtres :

Vous pouvez définir un **étendue** pour chacun des filtres, par exemple, vous pouvez définir l’étendue tous les filtres d’Action à exécuter dans le **étendue du contrôleur**et tous les filtres d’autorisation pour exécuter **portée globale** . Les étendues ont un ordre d’exécution défini.

En outre, chaque filtre d’action a une propriété de commande qui est utilisée pour déterminer l’ordre d’exécution dans la portée du filtre.

Pour plus d’informations sur l’ordre d’exécution de filtres d’Action personnalisée, consultez cet article MSDN : ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tâche 1 : Création d’un nouveau filtre d’Action personnalisé

Dans cette tâche, vous allez créer un nouveau filtre d’Action personnalisé à injecter dans la classe StoreController, apprendre à gérer l’ordre d’exécution des filtres.

1. Ouvrez le **commencer** solution situé dans **\Source\Ex02-ManagingMultipleActionFilters\Begin** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

    1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
    2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
    3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

        > [!NOTE]
        > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
        > 
        > Pour plus d’informations, consultez cet article : [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Ajoutez une nouvelle classe c# dans le **filtres** dossier et nommez-le *MyNewCustomActionFilter.cs*
3. Ouvrez **MyNewCustomActionFilter.cs** et ajoutez une référence à **System.Web.Mvc** et **MvcMusicStore.Models** espace de noms :

    (Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Remplacez la déclaration de classe par défaut par le code suivant.

    (Code Snippet - *filtres d’Action personnalisés ASP.NET MVC 4 - Ex2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Ce filtre d’Action personnalisé est presque identique à celle que vous avez créé dans l’exercice précédent. La principale différence est qu’il a le *&quot;connecté par&quot;* attribut mis à jour avec le nom de cette nouvelle classe pour identifier quel filtre enregistré le journal.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tâche 2 : Injectant un intercepteur de Code de la classe StoreController

Dans cette tâche, vous ajoutez un nouveau filtre personnalisé dans la classe StoreController et exécuter la solution pour vérifier la façon dont les deux filtres fonctionnent ensemble.

1. Ouvrez le **StoreController** classe situé dans **MvcMusicStore\Controllers** et injecter le nouveau filtre personnalisé **MyNewCustomActionFilter** dans  **StoreController** classe comme est indiqué dans le code suivant.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Maintenant, exécutez l’application afin de voir le fonctionnement de ces deux filtres d’Action personnalisée. Pour ce faire, appuyez sur **F5** et attendez que l’application démarre.
3. Accédez à **/ActionLog** pour afficher l’état initial d’affichage journal.

    ![État de mise hors tension avant l’activité de la page du journal](aspnet-mvc-4-custom-action-filters/_static/image5.png "état mise hors tension avant l’activité de la page du journal")

    *État de suivi du journal avant l’activité de la page*
4. Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.
5. Vérifiez que cette fois ; vos visites ont été suivies à deux reprises : une fois pour chacun des filtres d’Action personnalisée que vous avez ajouté dans le **StorageController** classe.

    ![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image6.png "journal des actions avec l’activité connectée")

    *Journal des actions avec l’activité connectée*
6. Fermez le navigateur.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tâche 3 : La gestion de l’ordre de filtre

Dans cette tâche, vous allez apprendre à gérer l’ordre d’exécution des filtres à l’aide de la propriété Order.

1. Ouvrez le **StoreController** classe situé dans **MvcMusicStore\Controllers** et spécifiez le **ordre** comme propriété dans les deux filtres illustré ci-dessous.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Maintenant, vérifiez comment les filtres sont exécutés en fonction de la valeur de la propriété de son ordre. Vous constaterez que le filtre avec la valeur d’ordre plus petite (**CustomActionFilter**) est la première qui est exécutée. Appuyez sur **F5** et attendez que l’application démarre.
3. Accédez à **/ActionLog** pour afficher l’état initial d’affichage journal.

    ![État de mise hors tension avant l’activité de la page du journal](aspnet-mvc-4-custom-action-filters/_static/image7.png "état mise hors tension avant l’activité de la page du journal")

    *État de suivi du journal avant l’activité de la page*
4. Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.
5. Vérifiez que cette fois, vos visites ont été suivies ordonnée par valeur d’ordre des filtres : **CustomActionFilter** journaux premier.

    ![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image8.png "journal des actions avec l’activité connectée")

    *Journal des actions avec l’activité connectée*
6. Maintenant, vous mettez à jour la valeur d’ordre des filtres et vérifier la façon dont l’ordre de journalisation change. Dans le **StoreController** class, mettre à jour de valeur d’ordre des filtres comme indiqué ci-dessous.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Réexécutez l’application en appuyant sur **F5**.
8. Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.
9. Vérifiez que cette fois, les journaux créés par **MyNewCustomActionFilter** filtre apparaît en premier.

    ![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image9.png "journal des actions avec l’activité connectée")

    *Journal des actions avec l’activité connectée*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tâche 4 : L’inscription des filtres dans le monde entier

Dans cette tâche, vous mettrez à jour la solution pour inscrire le nouveau filtre (**MyNewCustomActionFilter**) comme un filtre global. Ce faisant, il est déclenché par les actions exécutées dans l’application et pas seulement en StoreController celles comme dans la tâche précédente.

1. Dans **StoreController** classe, supprimez **[MyNewCustomActionFilter]** attribut et la propriété d’ordre à partir de **[CustomActionFilter]**. Il doit ressembler à ce qui suit :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Ouvrez **Global.asax** de fichiers et recherchez le **Application\_Démarrer** (méthode). Notez que chaque fois que l’application démarre, elle inscrit les filtres globaux en appelant **RegisterGlobalFilters** méthode au sein de **FilterConfig** classe.

    ![Enregistrement de filtres globaux dans Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "l’enregistrement de filtres globaux dans Global.asax")

    *Enregistrement de filtres globaux dans Global.asax*
3. Ouvrez **FilterConfig.cs** au sein du fichier **application\_Démarrer** dossier.
4. Ajoutez une référence à l’aide de System.Web.Mvc ; à l’aide de MvcMusicStore.Filters ; espace de noms.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Mise à jour **RegisterGlobalFilters** méthode en ajoutant votre filtre personnalisé. Pour ce faire, ajoutez le code en surbrillance :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Exécutez l’application en appuyant sur **F5**.
7. Cliquez sur une de le **Genres** à partir du menu et d’effectuer certaines actions, telles que la navigation d’un album disponible.
8. Vérification qu’à présent **[MyNewCustomActionFilter]** est injectées dans le HomeController et ActionLogController trop.

    ![Journal des actions avec l’activité connectée](aspnet-mvc-4-custom-action-filters/_static/image11.png "journal des actions avec l’activité connectée")

    *Journal des actions avec l’activité globale connectée*

> [!NOTE]
> En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [annexe b : Publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).


---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier pratique, vous avez appris à étendre un filtre d’action pour exécuter des actions personnalisées. Vous avez également appris à injecter les filtres à vos contrôleurs de page. Les concepts suivants ont été utilisés :

- Comment créer des filtres d’Action personnalisée avec la classe ActionFilterAttribute de MVC ASP.NET
- Comment injecter des filtres dans les contrôleurs MVC ASP.NET
- Comment gérer le filtre classement à l’aide de la propriété Order
- Comment inscrire des filtres dans le monde entier

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : Installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express pour une vignette de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe b : Publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau Site Web à partir de Windows Azure Portal

1. Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "ouvrez une session sur le portail Windows Azure")

    *Ouvrez une session sur le portail de gestion Azure Windows*
2. Cliquez sur **New** sur la barre de commandes.

    ![Création d’un Site Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "création d’un Site Web")

    *Création d’un Site Web*
3. Cliquez sur **calcul** | **Site Web**. Puis sélectionnez **création rapide** option. Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.

    > [!NOTE]
    > Un Site Web Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer. L’option Création rapide vous permet de déployer une application web terminée pour le Site Web Windows Azure à partir en dehors du portail. Il n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-custom-action-filters/_static/image19.png "création d’un Site Web à l’aide de la création rapide")

    *Création d’un Site Web à l’aide de la création rapide*
4. Attendez que la nouvelle **Site Web** est créé.
5. Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne. Vérifiez que le nouveau Site Web fonctionne.

    ![Navigation vers le nouveau site web](aspnet-mvc-4-custom-action-filters/_static/image20.png "exploration vers le nouveau site web")

    *Navigation vers le nouveau site web*

    ![Site Web en cours d’exécution](aspnet-mvc-4-custom-action-filters/_static/image21.png "site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.

    ![Ouvrir les pages de gestion de site web](aspnet-mvc-4-custom-action-filters/_static/image22.png "ouvrir les pages de gestion de site web")

    *Ouvrir les pages de gestion de Site Web*
7. Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.

    > [!NOTE]
    > Le *le profil de publication* contient toutes les informations requises pour publier une application web à un site Web Windows Azure pour chaque méthode de publication est activée. Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur sites Web Windows Azure.

    ![Téléchargement du site web le profil de publication](aspnet-mvc-4-custom-action-filters/_static/image23.png "téléchargement du site web le profil de publication")

    *Téléchargement du Site Web le profil de publication*
8. Télécharger le fichier de profil de publication dans un emplacement connu. Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.

    ![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-custom-action-filters/_static/image24.png "l’enregistrement du profil de publication")

    *L’enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données. Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.

1. Vous devez un serveur de base de données SQL pour stocker la base de données d’application. Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à l’adresse **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**. Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes. Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.

    ![Tableau de bord de serveur SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "tableau de bord serveur de base de données SQL")

    *Tableau de bord serveur de base de données SQL*
2. Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) bouton.

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Ajout d’adresse IP du Client*
3. Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.

    ![Publication de l’Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "publication de l’Application")

    *Publier le site web*
2. Importer le profil de publication que vous avez enregistré dans la première tâche.

    ![L’importation du profil de publication](aspnet-mvc-4-custom-action-filters/_static/image30.png "l’importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la Validation terminée. Cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.

    ![Validation de la connexion](aspnet-mvc-4-custom-action-filters/_static/image31.png "validation de la connexion")

    *Validation de la connexion*
4. Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).

    ![Configuration de déploiement Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuration de déploiement Web")

    *Configuration de déploiement Web*
5. Configurez la connexion de base de données comme suit :

   - Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.
   - Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.
   - Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.
   - Tapez un nouveau nom de base de données.

     ![Configuration de chaîne de connexion de destination](aspnet-mvc-4-custom-action-filters/_static/image33.png "configuration de chaîne de connexion de destination")

     *Configuration de chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](aspnet-mvc-4-custom-action-filters/_static/image34.png "création de la chaîne de la base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "chaîne de connexion pointant vers la base de données SQL")

    *Chaîne de connexion pointant vers la base de données SQL*
8. Dans le **aperçu** , cliquez sur **publier**.

    ![Publication de l’application web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publication de l’application web")

    *Publication de l’application web*
9. Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe c : À l’aide d’extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-custom-action-filters/_static/image37.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-custom-action-filters/_static/image38.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-custom-action-filters/_static/image39.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-custom-action-filters/_static/image40.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-custom-action-filters/_static/image41.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-custom-action-filters/_static/image42.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
