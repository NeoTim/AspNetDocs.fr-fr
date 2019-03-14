---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Montée en puissance parallèle de SignalR avec SQL Server (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7fd05a4487be4cc96fa945cf08226841e3f01806
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032346"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="44d2c-102">Scale-out de SignalR avec SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="44d2c-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="44d2c-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="44d2c-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="44d2c-104">Dans ce didacticiel, vous allez utiliser SQL Server pour distribuer les messages sur une application de SignalR est déployée dans deux instances distinctes d’IIS.</span><span class="sxs-lookup"><span data-stu-id="44d2c-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="44d2c-105">Vous pouvez également exécuter ce didacticiel sur un ordinateur de test unique, mais pour obtenir l’effet, vous devez déployer l’application de SignalR à deux ou plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="44d2c-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="44d2c-106">Vous devez également installer SQL Server sur l’un des serveurs ou sur un serveur dédié distinct.</span><span class="sxs-lookup"><span data-stu-id="44d2c-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="44d2c-107">Une autre option consiste à exécuter le didacticiel à l’aide de machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="44d2c-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="44d2c-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="44d2c-108">Prerequisites</span></span>

<span data-ttu-id="44d2c-109">Microsoft SQL Server 2005 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="44d2c-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="44d2c-110">Le fond de panier prend en charge les éditions de bureau et serveur de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="44d2c-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="44d2c-111">Il ne prend pas en charge SQL Server Compact Edition ou la base de données SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="44d2c-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="44d2c-112">(Si votre application est hébergée sur Azure, envisagez le fond de panier de Service Bus à la place.)</span><span class="sxs-lookup"><span data-stu-id="44d2c-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="44d2c-113">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="44d2c-113">Overview</span></span>

<span data-ttu-id="44d2c-114">Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.</span><span class="sxs-lookup"><span data-stu-id="44d2c-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="44d2c-115">Créer une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="44d2c-115">Create a new empty database.</span></span> <span data-ttu-id="44d2c-116">Le fond de panier créera les tables nécessaires dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="44d2c-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="44d2c-117">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="44d2c-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="44d2c-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="44d2c-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="44d2c-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="44d2c-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="44d2c-120">Créez une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="44d2c-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="44d2c-121">Ajoutez le code suivant à global.asax de façon à configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="44d2c-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="44d2c-122">Configurer la base de données</span><span class="sxs-lookup"><span data-stu-id="44d2c-122">Configure the Database</span></span>

<span data-ttu-id="44d2c-123">Décider si l’application utilisera l’authentification Windows ou l’authentification SQL Server pour accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="44d2c-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="44d2c-124">Tous les cas, assurez-vous que l’utilisateur de base de données dispose des autorisations pour vous connecter, créer des schémas et créer des tables.</span><span class="sxs-lookup"><span data-stu-id="44d2c-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="44d2c-125">Créer une base de données pour le fond de panier à utiliser.</span><span class="sxs-lookup"><span data-stu-id="44d2c-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="44d2c-126">Vous pouvez donner à la base de données n’importe quel nom.</span><span class="sxs-lookup"><span data-stu-id="44d2c-126">You can give the database any name.</span></span> <span data-ttu-id="44d2c-127">Vous n’avez pas besoin de créer des tables dans la base de données ; le fond de panier créera les tables nécessaires.</span><span class="sxs-lookup"><span data-stu-id="44d2c-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="44d2c-128">Activer Service Broker</span><span class="sxs-lookup"><span data-stu-id="44d2c-128">Enable Service Broker</span></span>

<span data-ttu-id="44d2c-129">Il est recommandé d’activer Service Broker pour la base de données de fond de panier.</span><span class="sxs-lookup"><span data-stu-id="44d2c-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="44d2c-130">Service Broker fournit une prise en charge native pour la messagerie et files d’attente dans SQL Server, ce qui permet de recevoir des mises à jour plus efficacement le fond de panier.</span><span class="sxs-lookup"><span data-stu-id="44d2c-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="44d2c-131">(Toutefois, le fond de panier fonctionne également sans Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="44d2c-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="44d2c-132">Pour vérifier si le Service Broker est activée, interrogez la **est\_broker\_activé** colonne dans le **sys.databases** vue de catalogue.</span><span class="sxs-lookup"><span data-stu-id="44d2c-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="44d2c-133">Pour activer Service Broker, utilisez la requête SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="44d2c-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="44d2c-134">Si cette requête s’affiche en interblocage, vérifiez qu’il n’existe aucune application connectée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="44d2c-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="44d2c-135">Si vous avez activé le suivi, les traces montrera également si Service Broker est activé.</span><span class="sxs-lookup"><span data-stu-id="44d2c-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="44d2c-136">Créer une Application de SignalR</span><span class="sxs-lookup"><span data-stu-id="44d2c-136">Create a SignalR Application</span></span>

<span data-ttu-id="44d2c-137">Créez une application de SignalR en suivant une de ces didacticiels :</span><span class="sxs-lookup"><span data-stu-id="44d2c-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="44d2c-138">Bien démarrer avec SignalR</span><span class="sxs-lookup"><span data-stu-id="44d2c-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="44d2c-139">Bien démarrer avec SignalR et MVC 4</span><span class="sxs-lookup"><span data-stu-id="44d2c-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="44d2c-140">Ensuite, nous allons modifier l’application de conversation pour prendre en charge la montée en puissance parallèle avec SQL Server.</span><span class="sxs-lookup"><span data-stu-id="44d2c-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="44d2c-141">Tout d’abord, ajoutez le package NuGet de SignalR.SqlServer à votre projet.</span><span class="sxs-lookup"><span data-stu-id="44d2c-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="44d2c-142">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="44d2c-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="44d2c-143">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="44d2c-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="44d2c-144">Ensuite, ouvrez le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="44d2c-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="44d2c-145">Ajoutez le code suivant à la **Application\_Démarrer** méthode :</span><span class="sxs-lookup"><span data-stu-id="44d2c-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="44d2c-146">Déployer et exécuter l’Application</span><span class="sxs-lookup"><span data-stu-id="44d2c-146">Deploy and Run the Application</span></span>

<span data-ttu-id="44d2c-147">Préparez vos instances de Windows Server pour déployer l’application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="44d2c-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="44d2c-148">Ajouter le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="44d2c-148">Add the IIS role.</span></span> <span data-ttu-id="44d2c-149">Inclut des fonctionnalités de « Développement d’applications », y compris le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="44d2c-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="44d2c-150">Incluent également le Service de gestion (répertorié sous « Outils de gestion »).</span><span class="sxs-lookup"><span data-stu-id="44d2c-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="44d2c-151">**Installer Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="44d2c-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="44d2c-152">Lorsque vous exécutez le Gestionnaire des services Internet, il vous invite à installer Microsoft Web Platform, ou vous pouvez [télécharger l’intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="44d2c-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="44d2c-153">Dans le programme d’installation de la plateforme, recherchez de Web Deploy et installer Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="44d2c-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="44d2c-154">Vérifiez que le Service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="44d2c-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="44d2c-155">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="44d2c-155">If not, start the service.</span></span> <span data-ttu-id="44d2c-156">(Si vous ne voyez pas Service de gestion Web dans la liste des services de Windows, assurez-vous que vous avez installé le Service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="44d2c-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="44d2c-157">Enfin, ouvrir le port 8172 pour TCP.</span><span class="sxs-lookup"><span data-stu-id="44d2c-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="44d2c-158">C’est le port qui utilise l’outil Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="44d2c-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="44d2c-159">Vous êtes maintenant prêt à déployer le projet de Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="44d2c-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="44d2c-160">Dans l’Explorateur de solutions, avec le bouton droit de la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="44d2c-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="44d2c-161">Pour plus de documentation sur le déploiement web, consultez [plan de contenu de déploiement Web pour Visual Studio et ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="44d2c-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="44d2c-162">Si vous déployez l’application à deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent des messages SignalR à partir de l’autre.</span><span class="sxs-lookup"><span data-stu-id="44d2c-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="44d2c-163">(Bien sûr, dans un environnement de production, les deux serveurs passait souvent derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="44d2c-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="44d2c-164">Après avoir exécuté l’application, vous pouvez voir que SignalR a automatiquement créé des tables dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="44d2c-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="44d2c-165">SignalR gère les tables.</span><span class="sxs-lookup"><span data-stu-id="44d2c-165">SignalR manages the tables.</span></span> <span data-ttu-id="44d2c-166">Tant que votre application est déployée, ne pas supprimer des lignes, modifier la table et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="44d2c-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
