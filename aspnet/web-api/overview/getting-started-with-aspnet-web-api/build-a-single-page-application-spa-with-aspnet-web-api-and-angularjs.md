---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Atelier pratique : Créer une Application à Page unique (SPA) avec l’API Web ASP.NET et Angular.js | Microsoft Docs'
author: rick-anderson
description: Dans les applications web traditionnelles, le client (navigateur) lance la communication avec le serveur en demandant une page. Le serveur traite ensuite la demande...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 03409e2fda831a07bbc5321ad842633b23ec25e5
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58422406"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Atelier pratique : Créer une application à une seule page avec l’API web ASP.NET et Angular.js
====================
par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

> Dans les applications web traditionnelles, le client (navigateur) lance la communication avec le serveur en demandant une page. Ensuite, le serveur traite la demande et envoie le code HTML de la page au client. Dans les interactions suivantes avec la page, par exemple, l’utilisateur accède à un lien ou soumet un formulaire avec des données, une nouvelle demande est envoyée au serveur et le flux recommence : le serveur traite la demande et envoie une nouvelle page au navigateur en réponse à la demande d’action Ed par le client.
> 
> Dans les Applications à Page unique (SPA) toute la page est chargée dans le navigateur après la demande initiale, mais les interactions suivantes prennent place via les demandes Ajax. Cela signifie que le navigateur doit mettre à jour uniquement la partie de la page qui a changé ; Il est inutile pour recharger la page entière. L’approche SPA réduit le temps pris par l’application pour répondre aux actions de l’utilisateur, ce qui entraîne une expérience plus fluide.
> 
> L’architecture d’une application SPA implique certains problèmes qui ne sont pas présentes dans les applications web traditionnelles. Toutefois, les nouvelles technologies telles que les API Web ASP.NET, infrastructures JavaScript, telles qu’AngularJS et nouvelles fonctionnalités de style fournies par CSS3 rendent très facile à concevoir et créer des applications à page unique.
> 
> Dans cet atelier sur main, vous serez tirer parti de ces technologies pour implémenter le questionnaire de Geek, un site Web de trivia basé sur le concept SPA. Vous allez tout d’abord implémenter la couche de service avec l’API Web ASP.NET pour exposer les points de terminaison requis pour récupérer les questions du test et de stocker les réponses. Ensuite, vous allez générer une interface utilisateur riche et réactive à l’aide de AngularJS et CSS3 effets de transformation.
> 
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [ https://aka.ms/webcamps-training-kit ](https://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Vue d'ensemble

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier, vous allez apprendre comment :

- Créer un service API Web ASP.NET pour envoyer et recevoir des données JSON
- Créer une interface utilisateur réactive, à l’aide d’AngularJS
- Améliorer l’expérience de l’interface utilisateur avec des transformations de CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Les éléments suivants sont nécessaire pour terminer ce laboratoire pratique :

- [Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/) ou supérieur

<a id="Setup"></a>
### <a name="setup"></a>Installation

Afin d’exécuter les exercices dans cet atelier, vous devez configurer votre environnement tout d’abord.

1. Ouvrez l’Explorateur Windows et accédez à l’atelier **Source** dossier.
2. Avec le bouton droit sur **Setup.cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui configure votre environnement et installer les extraits de code Visual Studio pour ce laboratoire.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Assurez-vous que vous avez activé toutes les dépendances pour ce laboratoire avant d’exécuter le programme d’installation.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>À l’aide d’extraits de Code

Dans le document de laboratoire, vous serez invité à insérer des blocs de code. Pour votre commodité, la majeure partie de ce code est fourni en tant que Visual Studio extraits de Code, vous pouvez y accéder à partir de Visual Studio 2013 pour éviter d’avoir à ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagnée d’une solution de départ située dans le **commencer** dossier de l’exercice qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code sont ajoutés au cours d’un exercice sont manquants à partir de ces solutions de démarrage et peut ne pas fonctionnent jusqu'à ce que vous avez terminé l’exercice. Dans le code source pour un exercice, vous y trouverez également un **fin** dossier qui contient une solution Visual Studio avec le code qui résulte d’effectuer les étapes dans l’exercice correspondant. Si vous avez besoin d’aide au cours de cet atelier, vous pouvez utiliser ces solutions en tant que guide.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique inclut les exercices suivants :

1. [Création d’une API Web](#Exercise1)
2. [Création d’une Interface SPA](#Exercise2)

Durée estimée pour effectuer ce laboratoire : **60 minutes**

> [!NOTE]
> Lorsque vous démarrez Visual Studio, vous devez sélectionner une des collections de paramètres prédéfinis. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, extraits de code IntelliSense et les options de boîte de dialogue. Les procédures décrites dans ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lorsque vous utilisez le **paramètres de développement généraux** collection. Si vous choisissez une collection de paramètres différents pour votre environnement de développement, il peut y avoir des différences dans les étapes que vous devez prendre en compte.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Exercice 1 : Création d’une API Web

Une des parties clés d’une application SPA est la couche de service. Il est chargé de traiter les appels Ajax envoyées par l’interface utilisateur et les données renvoyées en réponse à cet appel. Les données récupérées doivent être présentées dans un format lisible par machine afin d’être analysée et consommées par le client.

L’infrastructure API Web fait partie de la pile ASP.NET et est conçu pour le rendre facile à implémenter des services HTTP, généralement envoyer et recevoir des données JSON ou XML via une API RESTful. Dans cet exercice, vous allez créer le site Web pour héberger l’application de questionnaire de Geek, puis implémentez le service back-end pour exposer et de conserver les données de test à l’aide d’API Web ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tâche 1 : création du projet Initial pour le Geek questionnaire

Dans cette tâche, vous allez démarrer la création d’un projet ASP.NET MVC avec prise en charge pour API Web ASP.NET en fonction de la **One ASP.NET** type qui est fourni avec Visual Studio de projet. **One ASP.NET** unifie toutes les technologies ASP.NET et vous donne la possibilité de mélanger et de les mettre en correspondance comme vous le souhaitez. Vous ajouterez ensuite des classes de modèle d’Entity Framework et de l’initialiseur de base de données à insérer les questions du test.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et sélectionnez **fichier | Nouveau projet...**  pour démarrer une nouvelle solution.

    ![Création d’un projet](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "création d’un projet")

    *Création d’un projet*
2. Dans le **nouveau projet** boîte de dialogue, sélectionnez **Application Web ASP.NET** sous le **Visual C# | Web** onglet. Assurez-vous que **.NET Framework 4.5** est sélectionnée, nommez-le *GeekQuiz*, choisissez un **emplacement** et cliquez sur **OK**.

    ![Création d’un nouveau projet d’Application Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "création d’un nouveau projet d’Application Web ASP.NET")

    *Création d’un nouveau projet d’Application Web ASP.NET*
3. Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **MVC** modèle, puis sélectionnez le **API Web** option. En outre, assurez-vous que le **authentification** option est définie sur **comptes d’utilisateur individuels**. Cliquez sur **OK** pour continuer.

    ![Création d’un projet avec le modèle MVC, y compris les composants de l’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Création d’un projet avec le modèle MVC, y compris les composants de l’API Web*
4. Dans **l’Explorateur de solutions**, avec le bouton droit le **modèles** dossier de la **GeekQuiz** de projet et sélectionnez **ajouter | Élément existant...** .

    ![Ajout d’un élément existant](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Ajout d’un élément existant")

    *Ajout d’un élément existant*
5. Dans le **ajouter un élément existant** boîte de dialogue, accédez à la **Source/ressources/modèles** dossier et sélectionnez tous les fichiers. Cliquez sur **Ajouter**.

    ![Ajouter les ressources de modèle](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "ajoutant les ressources de modèle")

    *Ajouter les ressources de modèle*

    > [!NOTE]
    > En ajoutant ces fichiers, vous ajoutez le modèle de données, de contexte de base de données d’Entity Framework et de l’initialiseur de base de données pour l’application Geek questionnaire.
    > 
    > **Entity Framework (EF)** est un mappeur objet-relationnel (ORM) qui vous permet de créer des applications d’accès aux données par programmation avec un modèle d’application conceptuel au lieu de programmer directement à l’aide d’un schéma de stockage relationnel. Plus d’informations sur Entity Framework [ici](../../../entity-framework.md).
    > 
    > Voici une description des classes que vous venez d’ajouter :
    > 
    > - **TriviaOption :** représente une option unique associée à une question de questionnaire
    > - **TriviaQuestion :** représente une question de questionnaire et expose les options associées via le **Options** propriété
    > - **TriviaAnswer :** représente l’option sélectionnée par l’utilisateur en réponse à une question de questionnaire
    > - **TriviaContext :** représente le contexte de base de données d’Entity Framework de l’application Geek questionnaire. Cette classe est dérivée de **DContext** et expose **DbSet** propriétés qui représentent des collections d’entités décrites ci-dessus.
    > - **TriviaDatabaseInitializer :** l’implémentation de l’initialiseur d’Entity Framework pour le **TriviaContext** classe qui hérite de **CreateDatabaseIfNotExists**. Le comportement par défaut de cette classe consiste à créer la base de données uniquement si elle n’existe pas, insertion d’entités spécifiée dans le **Seed** (méthode).
6. Ouvrez le **Global.asax.cs** fichier et ajoutez le code suivant à l’aide d’instruction.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Ajoutez le code suivant au début de la **Application\_Démarrer** méthode pour définir le **TriviaDatabaseInitializer** en tant que l’initialiseur de base de données.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modifier le **accueil** contrôleur pour restreindre l’accès à des utilisateurs authentifiés. Pour ce faire, ouvrez le **HomeController.cs** de fichiers à l’intérieur de la **contrôleurs** dossier et ajoutez le **Authorize** attribut le **HomeController**définition de classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Le **Authorize** filtrer vérifie si l’utilisateur est authentifié. Si l’utilisateur n’est pas authentifié, il retourne le code d’état HTTP 401 (non autorisé) sans appeler l’action. Vous pouvez appliquer le filtre dans le monde entier, au niveau du contrôleur ou au niveau des actions individuelles.
9. Vous allez maintenant personnaliser la disposition des pages web et la personnalisation. Pour ce faire, ouvrez le  **\_Layout.cshtml** de fichiers à l’intérieur de la **vues | Partagé** dossier et mettre à jour le contenu de la **&lt;titre&gt;** élément en remplaçant *mon Application ASP.NET* avec *Geek questionnaire* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Dans le même fichier, vous devez mettre à jour la barre de navigation en supprimant le *sur* et *Contact* liens et renommer le *accueil* lier à *lire*. En outre, renommez le *nom de l’Application* lier à *Geek questionnaire*. Le code HTML de la barre de navigation doit se présenter comme le code suivant.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Mettre à jour le pied de page de la page de disposition en remplaçant *mon Application ASP.NET* avec *Geek questionnaire*. Pour ce faire, remplacez le contenu de la **&lt;pied de page&gt;** élément avec le code en surbrillance suivant.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tâche 2 : création de l’API de Web TriviaController

La tâche précédente, vous avez créé la structure initiale de l’application web Geek questionnaire. Vous allez maintenant générer un service API Web simple qui interagit avec le modèle de données questionnaire et expose les actions suivantes :

- **GET/api/trivia**: Récupère la question suivante dans la liste de questionnaire doit répondre à l’utilisateur authentifié.
- **POST/api/trivia**: Stocke la réponse de test spécifiée par l’utilisateur authentifié.

Vous utiliserez les outils de génération de modèles automatique ASP.NET fournis par Visual Studio pour créer la ligne de base pour la classe de contrôleur d’API Web.

1. Ouvrir le **WebApiConfig.cs** de fichiers à l’intérieur de la **application\_Démarrer** dossier. Ce fichier définit la configuration du service API Web, tels que la façon dont les itinéraires sont mappés aux actions de contrôleur d’API Web.
2. Ajoutez le code suivant à l’aide d’instruction au début du fichier.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Ajoutez le code en surbrillance suivant à la **inscrire** méthode pour configurer globalement le formateur pour les données JSON récupérées par les méthodes d’action API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Le **CamelCasePropertyNamesContractResolver** convertit automatiquement les noms de propriété aux *mixte* cas, qui est la convention générale pour les noms de propriétés dans JavaScript.
4. Dans **l’Explorateur de solutions**, avec le bouton droit le **contrôleurs** dossier de la **GeekQuiz** de projet et sélectionnez **ajouter | Nouvel élément généré automatiquement...** .

    ![Création d’un nouvel élément généré automatiquement](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "création d’un nouvel élément généré automatiquement")

    *Création d’un nouvel élément généré automatiquement*
5. Dans le **ajouter une structure** boîte de dialogue zone, assurez-vous que le **commune** nœud est sélectionné dans le volet gauche. Ensuite, sélectionnez le **contrôleur Web API 2 - vide** modèle dans le volet central et cliquez sur **ajouter**.

    ![En sélectionnant le modèle vide de contrôleur Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "en sélectionnant le modèle vide de contrôleur Web API 2")

    *En sélectionnant le modèle vide de contrôleur Web API 2*

    > [!NOTE]
    > **Génération de modèles automatique ASP.NET** est une infrastructure de génération de code pour les applications Web ASP.NET. Visual Studio 2013 inclut des générateurs de code préalablement installées pour les projets MVC et API Web. Vous devez utiliser la génération de modèles automatique dans votre projet lorsque vous souhaitez ajouter rapidement le code qui interagit avec les modèles de données afin de réduire le temps requis pour développer des opérations de données standard.
    > 
    > Le processus de génération de modèles automatique garantit également que toutes les dépendances requises sont installées dans le projet. Par exemple, si vous démarrez avec un projet ASP.NET vide et ensuite utilisez la structure pour ajouter un contrôleur d’API Web, les packages NuGet de l’API Web requis et les références sont ajoutés à votre projet automatiquement.
6. Dans le **ajouter un contrôleur** boîte de dialogue, tapez *TriviaController* dans le **nom du contrôleur** zone de texte et cliquez sur **ajouter**.

    ![Ajout du contrôleur de Trivia](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Ajout du contrôleur de Trivia")

    *Ajout du contrôleur de Trivia*
7. Le **TriviaController.cs** fichier est ensuite ajouté à la **contrôleurs** dossier de la **GeekQuiz** projet, contenant un vide **TriviaController** classe. Ajoutez le code suivant à l’aide d’instructions au début du fichier.

    (Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Ajoutez le code suivant au début de la **TriviaController** classe pour définir, initialiser et supprimer le **TriviaContext** instance dans le contrôleur.

    (Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Le **Dispose** méthode de **TriviaController** appelle le **Dispose** méthode de la **TriviaContext** instance, ce qui garantit que toutes les les ressources utilisées par l’objet de contexte sont libérés lorsque la **TriviaContext** instance est supprimée ou nettoyage de la mémoire. Cela inclut la fermeture de toutes les connexions de base de données ouvertes par Entity Framework.
9. Ajoutez la méthode d’assistance suivante à la fin de la **TriviaController** classe. Cette méthode récupère la question de questionnaire suivante à partir de la base de données doit répondre à l’utilisateur spécifié.

    (Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Ajoutez le code suivant **obtenir** méthode d’action à la **TriviaController** classe. Cette méthode d’action appelle le **NextQuestionAsync** méthode d’assistance définie dans l’étape précédente pour récupérer la question suivante pour l’utilisateur authentifié.

    (Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Ajoutez la méthode d’assistance suivante à la fin de la **TriviaController** classe. Cette méthode stocke la réponse spécifiée dans la base de données et retourne une valeur booléenne indiquant si la réponse est correcte ou non.

    (Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Ajoutez le code suivant **Post** méthode d’action à la **TriviaController** classe. Cette méthode d’action associe la réponse à l’utilisateur authentifié et appelle le **StoreAsync** méthode d’assistance. Ensuite, il envoie une réponse avec la valeur booléenne retournée par la méthode d’assistance.

    (Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modifier le contrôleur d’API Web pour restreindre l’accès aux utilisateurs authentifiés en ajoutant le **Authorize** attribut le **TriviaController** définition de classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tâche 3 : exécution de la Solution

Dans cette tâche, vous allez vérifier que le service API Web que vous avez créé dans la tâche précédente fonctionne comme prévu. Vous allez utiliser Internet Explorer **outils de développement F12** pour capturer le trafic réseau et d’inspecter la réponse complète à partir du service API Web.

> [!NOTE]
> Assurez-vous que l’option **Internet Explorer** est sélectionné dans le **Démarrer** bouton situé sur la barre d’outils de Visual Studio.
> 
> ![Option d’Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Appuyez sur **F5** pour exécuter la solution. Le **connectez-vous** page doit s’afficher dans le navigateur.

    > [!NOTE]
    > Lorsque l’application démarre, l’itinéraire MVC par défaut est déclenchée, qui par défaut est mappé à la **Index** action de la **HomeController** classe. Dans la mesure où **HomeController** est limité aux utilisateurs authentifiés (n’oubliez pas que vous décoré de cette classe avec le **Authorize** attribut dans l’exercice 1) et qu’il en existe aucun utilisateur authentifié encore, l’application redirige la demande d’origine vers la page de connexion.

    ![Exécution de la solution](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "la solution en cours d’exécution")

    *Exécution de la solution*
2. Cliquez sur **inscrire** pour créer un nouvel utilisateur.

    ![Enregistrer un nouvel utilisateur](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "enregistrer un nouvel utilisateur")

    *Enregistrer un nouvel utilisateur*
3. Dans le **inscrire** page, entrez un **nom d’utilisateur** et **mot de passe**, puis cliquez sur **inscrire**.

    ![Page inscrire](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "page d’inscription")

    *Page d’inscription*
4. L’application enregistre le nouveau compte et l’utilisateur est authentifié et redirigé vers la page d’accueil.

    ![Utilisateur est authentifié](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "utilisateur authentifié")

    *Utilisateur authentifié*
5. Dans le navigateur, appuyez sur **F12** pour ouvrir le **outils de développement** Panneau de configuration. Appuyez sur **CTRL + 4** ou cliquez sur le **réseau** icône, puis cliquez sur bouton de la flèche verte pour commencer la capture du trafic réseau.

    ![Lancement de capture de réseau d’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "capture réseau de lancement des API Web")

    *Lancement de capture de réseau d’API Web*
6. Ajouter **api/trivia** à l’URL dans la barre d’adresses du navigateur. Maintenant, vous allez inspecter les détails de la réponse à partir de la **obtenir** méthode d’action dans **TriviaController**.

    ![Récupération des données de question suivantes via les API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "extraire les données de question suivantes via les API Web")

    *Récupération des données de question suivantes via les API Web*

    > [!NOTE]
    > Une fois le téléchargement terminé, vous devrez effectuer une action avec le fichier téléchargé. Laissez la boîte de dialogue ouverte afin de pouvoir regarder le contenu de la réponse via la fenêtre outil de développeurs.
7. Maintenant vous serez inspecter le corps de la réponse. Pour ce faire, cliquez sur le **détails** onglet, puis cliquez sur **corps de réponse**. Vous pouvez vérifier que les données téléchargées sont un objet avec les propriétés **options** (qui est une liste de **TriviaOption** objets), **id** et **titre** qui correspondent à la **TriviaQuestion** classe.

    ![Afficher le corps de réponse de l’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "affichage le corps de réponse de l’API Web")

    *Corps de réponse de l’API Web affichage*
8. Revenez à Visual Studio, puis appuyez sur **MAJ + F5** pour arrêter le débogage.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Exercice 2 : Création de l’Interface SPA

Dans cet exercice vous allez tout d’abord créer la partie frontale web de questionnaire de Geek, se concentrant sur l’interaction d’Application à Page unique à l’aide **AngularJS**. Vous allez ensuite améliorer l’expérience utilisateur avec CSS3 pour réaliser des animations riches et de fournir un effet visuel du contexte de commutation lors de la transition d’une question à la suivante.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tâche 1 : création de l’Interface de l’application à page unique à l’aide d’AngularJS

Dans cette tâche, vous allez utiliser **AngularJS** pour implémenter le côté client de l’application Geek questionnaire. **AngularJS** est une infrastructure JavaScript open source qui augmente les applications basées sur un navigateur avec *Model-View-Controller* fonctionnalité (MVC), ce qui facilite le développement et de test.

Vous allez commencer par installer AngularJS à partir de la Console du Gestionnaire de Package de Visual Studio. Ensuite, vous allez créer le contrôleur pour fournir le comportement de l’application de Geek questionnaire et de la vue à restituer le nos questions et réponses en utilisant le moteur de modèle AngularJS.

> [!NOTE]
> Pour plus d’informations sur AngularJS, consultez [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Ouvrez **Visual Studio Express 2013 pour le Web** et ouvrez le **GeekQuiz.sln** solution situé dans le **/Ex2-CreatingASPAInterface/début du fichier Source** dossier. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue dans l’exercice précédent.
2. Ouvrez le **Console du Gestionnaire de Package** de **outils** > **Gestionnaire de Package NuGet**. Tapez la commande suivante pour installer le **AngularJS.Core** package NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. Dans **l’Explorateur de solutions**, avec le bouton droit le **Scripts** dossier de la **GeekQuiz** de projet et sélectionnez **ajouter | Nouveau dossier**. Nommez le dossier **application** et appuyez sur **entrée**.
4. Cliquez sur le **application** dossier que vous venez de créer et sélectionnez **ajouter | Fichier JavaScript**.

    ![Création d’un fichier JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Création d’un fichier JavaScript*
5. Dans le **spécifier le nom de l’élément** boîte de dialogue, tapez *questionnaire-contrôleur* dans le **nom de l’élément** zone de texte et cliquez sur **OK**.

    ![Le nouveau fichier JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Le nouveau fichier JavaScript*
6. Dans le **questionnaire-controller.js** , ajoutez le code suivant pour déclarer et initialiser le AngularJS **QuizCtrl** contrôleur.

    (Code Snippet - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La fonction de constructeur de la **QuizCtrl** contrôleur attend un paramètre injectable nommé **$scope**. L’état initial de l’étendue doit être définie dans la fonction constructeur en joignant des propriétés pour le **$scope** objet. Les propriétés contiennent le **modèle de vue**et sont accessibles au modèle lorsque le contrôleur est inscrit.
    > 
    > Le **QuizCtrl** contrôleur est défini à l’intérieur d’un module nommé **QuizApp**. Les modules sont des unités de travail qui vous permettent de diviser votre application en composants distincts. Les principaux avantages de l’utilisation de modules est que le code est plus facile à comprendre et facilite les tests unitaires, réutilisation et la maintenabilité.
7. Vous allez maintenant ajouter le comportement à l’étendue afin de réagir aux événements déclenchés à partir de la vue. Ajoutez le code suivant à la fin de la **QuizCtrl** contrôleur pour définir le **nextQuestion** fonctionner dans le **$scope** objet.

    (Code Snippet - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Cette fonction récupère la question suivante à partir de la **Trivia** API Web créé dans l’exercice précédent et attache les données de la question à la **$scope** objet.
8. Insérez le code suivant à la fin de la **QuizCtrl** contrôleur pour définir le **sendAnswer** fonctionner dans le **$scope** objet.

    (Code Snippet - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Cette fonction envoie la réponse sélectionnée par l’utilisateur pour le **Trivia** API Web et stocke le résultat : par exemple, si la réponse est correcte ou non – dans le **$scope** objet.
    > 
    > Le **nextQuestion** et **sendAnswer** fonctions ci-dessus utilisent le AngularJS **$http** objet abstraire la communication avec l’API Web par le biais de l’objet XMLHttpRequest Objet JavaScript à partir du navigateur. AngularJS prend en charge un autre service qui offre un niveau supérieur d’abstraction pour effectuer des opérations CRUD exécutées sur une ressource via des API RESTful. Le AngularJS **$resource** objet possède des méthodes d’action qui fournissent des comportements de haut niveau sans avoir besoin d’interagir avec le **$http** objet. Envisagez d’utiliser le **$resource** objet dans les scénarios nécessitant le modèle CRUD (pour plus d’informations, consultez le [$resource documentation](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. L’étape suivante consiste à créer le modèle AngularJS qui définit la vue du questionnaire. Pour ce faire, ouvrez le **Index.cshtml** de fichiers à l’intérieur de la **vues | Accueil** dossier et remplacez son contenu par le code suivant.

    (Code Snippet - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Le modèle AngularJS est une spécification déclarative qui utilise des informations à partir du modèle et le contrôleur pour transformer le balisage statique dans la vue dynamique que l’utilisateur voit dans le navigateur. Voici quelques exemples d’éléments de AngularJS et d’attributs de l’élément qui peuvent être utilisées dans un modèle :
    > 
    > - Le **ng-application** directive dit à AngularJS à l’élément DOM qui représente l’élément racine de l’application.
    > - Le **ng-controller** directive attache un contrôleur au DOM au point où la directive est déclarée.
    > - La notation avec accolades **{{}}** désigne les liaisons pour les propriétés de l’étendue définies dans le contrôleur.
    > - Le **ng clic** directive est utilisée pour appeler les fonctions définies dans l’étendue en réponse aux clics de l’utilisateur.
10. Ouvrir le **Site.css** de fichiers à l’intérieur de la **contenu** dossier et ajoutez les styles de mise en surbrillance suivants à la fin du fichier pour fournir une apparence de la vue de questionnaire.

    (Code Snippet - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tâche 2 : exécution de la Solution

Dans cette tâche, vous allez exécuter la solution en utilisant le nouvel utilisateur de l’interface que vous avez créé avec AngularJS pour répondre à certaines des questions de questionnaire.

1. Appuyez sur **F5** pour exécuter la solution.
2. Inscrire un nouveau compte d’utilisateur. Pour ce faire, suivez la procédure d’inscription décrite dans l’exercice 1, tâche 3.

    > [!NOTE]
    > Si vous utilisez la solution à partir de l’exercice précédent, vous pouvez vous connecter avec le compte d’utilisateur que vous avez créé avant.
3. Le **accueil** page doit s’afficher, indiquant la première question du questionnaire. Répondez à la question en cliquant sur une des options. Cette opération déclenche le **sendAnswer** fonction définie précédemment, qui envoie l’option sélectionnée par le **Trivia** API Web.

    ![Répondre à une question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "répondre à une question")

    *Répondre à une question*
4. Après avoir cliqué sur un des boutons, la réponse doit apparaître. Cliquez sur **Question suivante** pour afficher la question suivante. Cette opération déclenche le **nextQuestion** fonction définie dans le contrôleur.

    ![Demande la question suivante](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "demandant la question suivante")

    *Demande la question suivante*
5. La question suivante doit apparaître. Continuer à répondre à des questions autant de fois que vous le souhaitez. Après avoir effectué toutes les questions, vous devez revenir à la première question.

    ![Une autre question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "une autre question")

    *Question suivante*
6. Revenez à Visual Studio, puis appuyez sur **MAJ + F5** pour arrêter le débogage.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tâche 3 : création d’une Animation de retournement à l’aide de CSS3

Dans cette tâche, vous allez utiliser les propriétés de CSS3 réaliser des animations riches en ajoutant un effet de retournement lorsqu’une question est traitée, et lorsque la question suivante est récupérée.

1. Dans **l’Explorateur de solutions**, avec le bouton droit le **contenu** dossier de la **GeekQuiz** de projet et sélectionnez **ajouter | Élément existant...** .

    ![Ajout d’un élément existant dans le dossier Content](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Ajout d’un élément existant dans le dossier de contenu")

    *Ajout d’un élément existant dans le dossier de contenu*
2. Dans le **ajouter un élément existant** boîte de dialogue, accédez à la **Source/ressources** dossier et sélectionnez **Flip.css**. Cliquez sur **Ajouter**.

    ![Ajout du fichier de Flip.css à partir de ressources](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Ajout du fichier de Flip.css à partir de ressources")

    *Ajout du fichier de Flip.css à partir de ressources*
3. Ouvrez le **Flip.css** fichier que vous venez d’ajouter et examinez son contenu.
4. Recherchez le **flip transformation** commentaire. Les styles inférieurs à ce commentaire utilisent CSS **perspective** et **rotateY** transformations pour générer un &quot;carte retournement&quot; effet.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Recherchez le **masquer arrière volet pendant retournement** commentaire. Le style sous ce commentaire masque l’arrière des faces lorsqu’ils sont opposés à la visionneuse en définissant le **une visibilité de la face arrière** propriété CSS *masqué*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Ouvrir le **BundleConfig.cs** de fichiers à l’intérieur la **application\_Démarrer** dossier et ajoutez la référence à la **Flip.css** de fichiers dans le **&quot;~/Content/css&quot;** offre groupée de style

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Appuyez sur **F5** pour exécuter la solution et connectez-vous avec vos informations d’identification.
8. Répondre à une question en cliquant sur une des options. Notez l’effet de retournement lors de la transition entre les vues.

    ![Répondre à une question avec l’effet de retournement](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "répondre à une question avec l’effet de retournement")

    *Répondre à une question avec l’effet de retournement*
9. Cliquez sur **Question suivante** pour récupérer la question suivante. L’effet de retournement doit apparaître à nouveau.

    ![Récupération de la question suivante avec l’effet de retournement](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "récupération de la question suivante avec l’effet de retournement")

    *Récupération de la question suivante avec l’effet de retournement*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier, vous avez appris comment :

- Créer un contrôleur d’API Web ASP.NET à l’aide de la génération de modèles automatique ASP.NET
- Implémenter une action d’obtenir des API Web pour récupérer la question suivante de questionnaire
- Implémenter une action de publication de l’API Web pour stocker les réponses du questionnaire
- Installer AngularJS à partir de la Console du Gestionnaire de Package Visual Studio
- Implémentez AngularJS modèles et contrôleurs
- Les transitions CSS3 permet d’effectuer les effets d’animation
