---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Héberger des API Web ASP.NET 2 dans un rôle Worker Azure | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment héberger ASP.NET Web API dans un rôle de travail Azure, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web. Ouvrir l’Interface Web pour l’Allemagne de .NET (OWIN)...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 40cb1a4514beaf81e7ed75bbd3e478f2ba146fe5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063916"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Héberger des API Web ASP.NET 2 dans un rôle Worker Azure
====================
par [Mike Wasson](https://github.com/MikeWasson)

> Ce didacticiel montre comment héberger ASP.NET Web API dans un rôle de travail Azure, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web.
>
> [Open Web Interface pour .NET](http://owin.org/) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web. OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS – par exemple, à l’intérieur d’un rôle de travail Azure.
>
> Dans ce didacticiel, vous allez utiliser le package Microsoft.Owin.Host.HttpListener, qui fournit un serveur HTTP servir à Self-host applications OWIN.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web API 2
> - [Azure SDK pour .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Créer un projet Microsoft Azure

Démarrez Visual Studio avec des privilèges d’administrateur. Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.

Sur le **fichier** menu, cliquez sur **New**, puis cliquez sur **projet**. À partir de **modèles installés**, sous Visual c#, cliquez sur **Cloud** puis cliquez sur **Windows Azure Cloud Service**. Nommez le projet « AzureApp » et cliquez sur **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue, double-cliquez sur **rôle de travail**. Laissez le nom par défaut (« WorkerRole1 »). Cette étape ajoute un rôle de travail à la solution. Cliquez sur **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

La solution Visual Studio qui est créée contient deux projets :

- &quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.
- &quot;WorkerRole1&quot; contient le code pour le rôle de travail.

En règle générale, une application Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un seul rôle.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Ajouter les API Web et les Packages OWIN

À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet**, puis cliquez sur **Console du Gestionnaire de Package**.

Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Ajouter un point de terminaison HTTP

Dans l’Explorateur de solutions, développez le projet AzureApp. Développez le nœud rôles, cliquez sur WorkerRole1 et sélectionnez **propriétés**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Cliquez sur **points de terminaison**, puis cliquez sur **ajouter le point de terminaison**.

Dans le **protocole** liste déroulante, sélectionnez « http ». Dans **Port Public** et **Port privé**, tapez 80. Ces numéros de port peut être différent. Le port public est ce que les clients utilisent lorsqu’ils envoient une demande au rôle.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurer les API Web pour l’auto-hébergement

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Ensuite, ajoutez une classe de contrôleur d’API Web. Cliquez sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe**. Nommez la classe TestController. Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Par souci de simplicité, ce contrôleur définit uniquement deux méthodes GET qui retournent du texte brut.

## <a name="start-the-owin-host"></a>Démarrer l’hôte OWIN

Ouvrez le fichier WorkerRole.cs. Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.

Ajoutez le code suivant à l’aide d’instruction :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Ajouter un **IDisposable** membre à la `WorkerRole` classe :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

Dans le `OnStart` (méthode), ajoutez le code suivant pour démarrer l’ordinateur hôte :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Le **WebApp.Start** méthode commence à l’hôte OWIN. Le nom de la `Startup` classe est un paramètre de type à la méthode. Par convention, l’hôte appelle le `Configure` méthode de cette classe.

Remplacer le `OnStop` pour les supprimer de la  *\_application* instance :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Voici le code complet pour WorkerRole.cs :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Générez la solution, puis appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure. En fonction de vos paramètres de pare-feu, vous devrez peut-être autoriser l’émulateur via votre pare-feu.

> [!NOTE]
> Si vous obtenez une exception semblable à ce qui suit, consultez [ce billet de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) pour une solution de contournement. « Impossible de charger le fichier ou l’assembly ' Microsoft.Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou une de ses dépendances. Définition du manifeste de l’assembly trouvée ne correspond pas à la référence d’assembly. (Exception de HRESULT : 0x80131040)"


L’émulateur de calcul attribue une adresse IP locale au point de terminaison. Vous pouvez trouver l’adresse IP en affichant l’interface de l’émulateur de calcul. Cliquez sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **Show Compute Emulator UI**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Recherchez l’adresse IP sous déploiements de Service, déploiement [id], détails du Service. Ouvrez un navigateur web et accédez à http://<em>adresse</em>/test/1, où <em>adresse</em> est l’adresse IP affectée par l’émulateur de calcul ; par exemple, `http://127.0.0.1:80/test/1`. Vous devez voir la réponse à partir du contrôleur d’API Web :

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Déployer sur Azure

Pour cette étape, vous devez disposer d’un compte Azure. Si vous n’en avez déjà, vous pouvez créer un compte d’essai gratuit en quelques minutes. Pour plus d’informations, consultez [version d’évaluation gratuite de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Dans l’Explorateur de solutions, cliquez sur le projet AzureApp. Sélectionnez **Publier**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **Sign In**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Une fois que vous êtes connecté, choisissez un abonnement, puis cliquez sur **suivant**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Entrez un nom pour le service cloud et choisissez une région. Cliquez sur **Créer**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Cliquez sur **Publier**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

La fenêtre de journal d’activité Azure affiche la progression du déploiement. Quand l’application est déployée, accédez à http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble du projet Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Projet Katana sur GitHub](https://github.com/aspnet/AspNetKatana)
