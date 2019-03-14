---
title: Bien démarrer avec ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: c09c06f55c4179e9e2174f0063ab7387b7e4c31b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059246"
---
# <a name="get-started-with-aspnet-core-mvc"></a>Bien démarrer avec ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.

L’application gère une base de données de titres de films. Vous apprenez à :

> [!div class="checklist"]
> * Créer une application web.
> * Ajouter et structurer un modèle.
> * Utiliser une base de données.
> * Ajouter une fonctionnalité de recherche et de validation.

À la fin, vous obtenez une application qui peut gérer et afficher des données de films.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>Créer une application web

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

Renseignez la boîte de dialogue **Nouveau projet** :

* Dans le volet gauche, sélectionnez **.NET Core**
* Dans le volet central, sélectionnez **Application web ASP.NET Core (.NET Core)**
* Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).
* Sélectionnez **OK**

![Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :

* Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.2**
* Sélectionnez **Application web (Model-View-Controller)**.
* Sélectionnez **OK**.

![Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer. Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options. Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Il part du principe que vous connaissez déjà VS Code. Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).

* Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Accédez à un répertoire (`cd`) destiné à contenir le projet.
* Exécutez la commande suivante :

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**  Sélectionnez **Oui**.

  * `dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.
  * `code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Sélectionnez **Fichier** > **Nouvelle solution**.

  ![macOS - Nouvelle solution](~/tutorials/first-web-api-mac/_static/sln.png)

* Sélectionnez **Application .NET Core** > **ASP.NET Core** > **Application web ASP.NET Core (MVC)** > **Suivant**.

  ![macOS - Boîte de dialogue Nouveau projet](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut **.NET Core 2.2* pour **Framework cible**.

* Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application. Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`. C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local. Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.
* Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code. De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.
* Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`. La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`. La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local. Localhost traite uniquement les requêtes web de l’ordinateur local.

  Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code. De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application. Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.

[!INCLUDE[](~/includes/trustCertMac.md)]

* La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`. C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local. Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web. Quand vous exécutez l’application, vous voyez un autre numéro de port.
* Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.

------

* Sélectionnez **Accepter** pour accepter le suivi. Cette application n’effectue pas le suivi d’informations personnelles. Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  L’illustration suivante montre l’application une fois le suivi accepté :

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
