---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Diffusion du serveur avec SignalR 2 Microsoft Docs'
author: tdykstra
description: Ce tutoriel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion du serveur.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676096"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="6b0a8-103">Tutorial: Diffusion du serveur avec SignalR 2</span><span class="sxs-lookup"><span data-stu-id="6b0a8-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="6b0a8-104">Ce tutoriel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion du serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="6b0a8-105">La diffusion du serveur signifie que le serveur démarre les communications envoyées aux clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="6b0a8-106">L’application que vous allez créer dans ce tutoriel simule un ticker stock, un scénario typique pour la fonctionnalité de diffusion du serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="6b0a8-107">Périodiquement, le serveur met au sort les prix des actions et diffuse les mises à jour de tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="6b0a8-108">Dans le navigateur, les nombres **%** et les symboles du **Changement** et les colonnes changent dynamiquement en réponse aux notifications du serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="6b0a8-109">Si vous ouvrez des navigateurs supplémentaires à la même URL, ils affichent tous les mêmes données et les mêmes modifications aux données simultanément.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Créer du web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="6b0a8-111">Dans ce tutoriel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b0a8-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="6b0a8-112">Create the project</span></span>
> * <span data-ttu-id="6b0a8-113">Configurer le code du serveur</span><span class="sxs-lookup"><span data-stu-id="6b0a8-113">Set up the server code</span></span>
> * <span data-ttu-id="6b0a8-114">Examiner le code serveur</span><span class="sxs-lookup"><span data-stu-id="6b0a8-114">Examine the server code</span></span>
> * <span data-ttu-id="6b0a8-115">Configurer le code client</span><span class="sxs-lookup"><span data-stu-id="6b0a8-115">Set up the client code</span></span>
> * <span data-ttu-id="6b0a8-116">Examiner le code client</span><span class="sxs-lookup"><span data-stu-id="6b0a8-116">Examine the client code</span></span>
> * <span data-ttu-id="6b0a8-117">Test de l’application</span><span class="sxs-lookup"><span data-stu-id="6b0a8-117">Test the application</span></span>
> * <span data-ttu-id="6b0a8-118">Activation de la journalisation</span><span class="sxs-lookup"><span data-stu-id="6b0a8-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6b0a8-119">Si vous ne souhaitez pas passer à travers les étapes de la construction de l’application, vous pouvez installer le paquet SignalR.Sample dans un nouveau projet d’application Web vide ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="6b0a8-120">Si vous installez le paquet NuGet sans effectuer les étapes de ce tutoriel, vous devez suivre les instructions dans le fichier *readme.txt.*</span><span class="sxs-lookup"><span data-stu-id="6b0a8-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="6b0a8-121">Pour exécuter le paquet, vous devez ajouter une `ConfigureSignalR` classe de démarrage OWIN qui appelle la méthode dans le paquet installé.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="6b0a8-122">Vous recevrez une erreur si vous n’ajoutez pas la classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="6b0a8-123">Voir la section [d’échantillons Install the StockTicker](#install-the-stockticker-sample) de cet article.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b0a8-124">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6b0a8-124">Prerequisites</span></span>

* <span data-ttu-id="6b0a8-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="6b0a8-126">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="6b0a8-126">Create the project</span></span>

<span data-ttu-id="6b0a8-127">Cette section montre comment utiliser Visual Studio 2017 pour créer une application Web ASP.NET vide.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="6b0a8-128">Dans Visual Studio, créez une application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer du web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="6b0a8-130">Dans la **nouvelle application Web ASP.NET - fenêtre SignalR.StockTicker,** laissez **Vide** sélectionné et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="6b0a8-131">Configurer le code du serveur</span><span class="sxs-lookup"><span data-stu-id="6b0a8-131">Set up the server code</span></span>

<span data-ttu-id="6b0a8-132">Dans cette section, vous configurez le code qui s’exécute sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="6b0a8-133">Créer la classe Stock</span><span class="sxs-lookup"><span data-stu-id="6b0a8-133">Create the Stock class</span></span>

<span data-ttu-id="6b0a8-134">Vous commencez par créer la classe de modèle *Stock* que vous utiliserez pour stocker et transmettre des informations sur un stock.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="6b0a8-135">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="6b0a8-136">Nommez la classe *Stock* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="6b0a8-137">Remplacez le code dans le *fichier Stock.cs* avec ce code :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="6b0a8-138">Les deux propriétés que vous définirez `Symbol` lorsque vous créez des stocks `Price`sont (par exemple, MSFT pour Microsoft) et .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="6b0a8-139">Les autres propriétés dépendent de `Price`comment et quand vous définissez .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="6b0a8-140">La première fois `Price`que vous définissez, `DayOpen`la valeur se propage à .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="6b0a8-141">Après cela, lorsque `Price`vous définissez, `Change` `PercentChange` l’application calcule `Price` les `DayOpen`valeurs et les valeurs de la propriété en fonction de la différence entre et .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="6b0a8-142">Créer les classes StockTickerHub et StockTicker</span><span class="sxs-lookup"><span data-stu-id="6b0a8-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="6b0a8-143">Vous utiliserez l’API SignalR Hub pour gérer l’interaction serveur-client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="6b0a8-144">Une `StockTickerHub` classe qui dérive de `Hub` la classe SignalR s’occupera des connexions de réception et des appels de méthode des clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="6b0a8-145">Vous devez également maintenir les `Timer` données de stock et exécuter un objet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="6b0a8-146">L’objet `Timer` déclenchera périodiquement des mises à jour de prix indépendantes des connexions client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="6b0a8-147">Vous ne pouvez pas mettre `Hub` ces fonctions dans une classe, parce que les hubs sont transitoires.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="6b0a8-148">L’application `Hub` crée un exemple de classe pour chaque tâche sur le hub, comme les connexions et les appels du client au serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="6b0a8-149">Ainsi, le mécanisme qui conserve les données sur les stocks, met à jour les prix et diffuse les mises à jour des prix doit fonctionner dans une classe distincte.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="6b0a8-150">Vous nommerez la `StockTicker`classe .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-150">You'll name the class `StockTicker`.</span></span>

![Diffusion de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="6b0a8-152">Vous ne voulez qu’une seule instance de la `StockTicker` classe pour s’exécuter `StockTickerHub` sur le serveur, de sorte que vous aurez besoin de configurer une référence de chaque instance à l’instance singleton. `StockTicker`</span><span class="sxs-lookup"><span data-stu-id="6b0a8-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="6b0a8-153">La `StockTicker` classe doit diffuser aux clients parce qu’elle a `StockTicker` les données `Hub` d’actions et déclenche des mises à jour, mais n’est pas une classe.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="6b0a8-154">La `StockTicker` classe doit obtenir une référence à l’objet de contexte de connexion SignalR Hub.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="6b0a8-155">Il peut ensuite utiliser l’objet de contexte de connexion SignalR pour diffuser aux clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="6b0a8-156">Créer des StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="6b0a8-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="6b0a8-157">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **un nouvel article**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="6b0a8-158">Dans **Ajouter un nouvel article - SignalR.StockTicker**, sélectionnez Le signal**web** **Visual** > **CMD** >  **installé,** > puis sélectionnez **La classe De hub SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="6b0a8-159">Nommez la classe *StockTickerHub* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="6b0a8-160">Cette étape crée le fichier de classe *StockTickerHub.cs.*</span><span class="sxs-lookup"><span data-stu-id="6b0a8-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="6b0a8-161">Simultanément, il ajoute un ensemble de fichiers de script et de références d’assemblage qui prend en charge SignalR au projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="6b0a8-162">Remplacez le code dans le fichier *StockTickerHub.cs* avec ce code :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="6b0a8-163">Enregistrez le fichier .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-163">Save the file.</span></span>

<span data-ttu-id="6b0a8-164">L’application utilise la classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) pour définir les méthodes que les clients peuvent appeler sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="6b0a8-165">Vous définissez une `GetAllStocks()`méthode : .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="6b0a8-166">Lorsqu’un client se connecte d’abord au serveur, il appelle cette méthode pour obtenir une liste de tous les stocks avec leurs prix actuels.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="6b0a8-167">La méthode peut fonctionner de `IEnumerable<Stock>` façon synchrone et revenir parce qu’elle renvoie les données de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="6b0a8-168">Si la méthode devait obtenir les données en faisant quelque chose qui impliquerait l’attente, comme une recherche de base de données ou un appel de service Web, vous pouvez spécifier `Task<IEnumerable<Stock>>` comme valeur de retour pour activer le traitement asynchrone.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="6b0a8-169">Pour plus d’informations, voir [ASP.NET SignalR Hubs API Guide - Server - Quand exécuter asynchronement](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="6b0a8-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="6b0a8-170">L’attribut `HubName` précise comment l’application fera référence au code Hub dans JavaScript sur le client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="6b0a8-171">Le nom par défaut sur le client si vous n’utilisez pas cet attribut, est une `stockTickerHub`version camelCase du nom de classe, qui dans ce cas serait .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="6b0a8-172">Comme vous le verrez plus `StockTicker` tard lorsque vous créez la classe, l’application crée une instance singleton de cette classe dans sa propriété statique. `Instance`</span><span class="sxs-lookup"><span data-stu-id="6b0a8-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="6b0a8-173">Cette instance singleton de est dans la `StockTicker` mémoire, peu importe combien de clients se connectent ou se déconnectent.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="6b0a8-174">C’est le `GetAllStocks()` cas de cette méthode pour retourner les informations actuelles sur les stocks.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="6b0a8-175">Créer des StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="6b0a8-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="6b0a8-176">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="6b0a8-177">Nommez la classe *StockTicker* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="6b0a8-178">Remplacez le code dans le *fichier StockTicker.cs* avec ce code :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="6b0a8-179">Étant donné que tous les threads seront en cours d’exécution de la même instance de code StockTicker, la classe StockTicker doit être thread-safe.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="6b0a8-180">Examiner le code serveur</span><span class="sxs-lookup"><span data-stu-id="6b0a8-180">Examine the server code</span></span>

<span data-ttu-id="6b0a8-181">Si vous examinez le code serveur, il vous aidera à comprendre comment fonctionne l’application.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="6b0a8-182">Stockage de l’instance singleton dans un champ statique</span><span class="sxs-lookup"><span data-stu-id="6b0a8-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="6b0a8-183">Le code initialise `_instance` le champ statique `Instance` qui soutient la propriété avec une instance de la classe.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="6b0a8-184">Parce que le constructeur est privé, c’est le seul exemple de la classe que l’application peut créer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="6b0a8-185">L’application utilise [l’initialisation paresseuse](/dotnet/framework/performance/lazy-initialization) pour le `_instance` domaine.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="6b0a8-186">Ce n’est pas pour des raisons de performance.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-186">It's not for performance reasons.</span></span> <span data-ttu-id="6b0a8-187">Il s’agit de s’assurer que la création d’instance est sans fil.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="6b0a8-188">Chaque fois qu’un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub en cours d’exécution dans un thread séparé obtient l’instance StockTicker singleton de la propriété statique, comme vous l’avez `StockTicker.Instance` vu plus tôt dans la `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="6b0a8-189">Stockage des données sur les stocks dans un</span><span class="sxs-lookup"><span data-stu-id="6b0a8-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="6b0a8-190">Le constructeur initialise `_stocks` la collecte avec quelques `GetAllStocks` données d’exemple de stock, et retourne les stocks.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="6b0a8-191">Comme vous l’avez vu plus `StockTickerHub.GetAllStocks`tôt, cette collection d’actions est retournée par , qui est une méthode de serveur dans la classe que les `Hub` clients peuvent appeler.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="6b0a8-192">La collecte des stocks est définie comme un type [concurrentdictionnaire](https://msdn.microsoft.com/library/dd287191.aspx) pour la sécurité des threads.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="6b0a8-193">Comme alternative, vous pouvez utiliser un objet [De dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) et verrouiller explicitement le dictionnaire lorsque vous y modifiez.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="6b0a8-194">Pour cette application d’exemple, il est acceptable de stocker les données d’application en mémoire et de perdre les données lorsque l’application se débarrasse de l’instance. `StockTicker`</span><span class="sxs-lookup"><span data-stu-id="6b0a8-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="6b0a8-195">Dans une application réelle, vous travailleriez avec un magasin de données back-end comme une base de données.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="6b0a8-196">Mise à jour périodique des cours des actions</span><span class="sxs-lookup"><span data-stu-id="6b0a8-196">Periodically updating stock prices</span></span>

<span data-ttu-id="6b0a8-197">Le constructeur démarre `Timer` un objet qui appelle périodiquement des méthodes qui mettent à jour les cours des actions sur une base aléatoire.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="6b0a8-198">`Timer`appels `UpdateStockPrices`, qui passe en null dans le paramètre de l’état.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="6b0a8-199">Avant de mettre à jour les `_updateStockPricesLock` prix, l’application prend un verrou sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="6b0a8-200">Le code vérifie si un autre thread est `TryUpdateStockPrice` déjà mise à jour des prix, puis il appelle sur chaque stock de la liste.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="6b0a8-201">La `TryUpdateStockPrice` méthode décide de changer le cours de l’action, et combien de le changer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="6b0a8-202">Si le cours de l’action change, l’application appelle `BroadcastStockPrice` à diffuser le changement de cours de l’action à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="6b0a8-203">Le `_updatingStockPrices` drapeau désigné [volatil](https://msdn.microsoft.com/library/x13ttww7.aspx) pour s’assurer qu’il est sans fil.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="6b0a8-204">Dans une application `TryUpdateStockPrice` réelle, la méthode appellerait un service web pour rechercher le prix.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="6b0a8-205">Dans ce code, l’application utilise un générateur de nombres aléatoires pour effectuer des modifications au hasard.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="6b0a8-206">Obtenir le contexte SignalR afin que la classe StockTicker puisse diffuser aux clients</span><span class="sxs-lookup"><span data-stu-id="6b0a8-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="6b0a8-207">Parce que les changements de `StockTicker` prix proviennent ici de l’objet, c’est l’objet qui doit appeler une `updateStockPrice` méthode sur tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="6b0a8-208">Dans `Hub` une classe, vous avez une API `StockTicker` pour appeler les `Hub` méthodes client, mais ne `Hub` dérive pas de la classe et n’a pas de référence à un objet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="6b0a8-209">Pour diffuser aux clients `StockTicker` connectés, la classe doit obtenir `StockTickerHub` l’exemple de contexte SignalR pour la classe et l’utiliser pour appeler des méthodes sur les clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="6b0a8-210">Le code obtient une référence au contexte SignalR quand il crée l’instance de classe singleton, `Clients` passe cette référence au constructeur, et le constructeur le met dans la propriété.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="6b0a8-211">Il ya deux raisons pour lesquelles vous voulez obtenir le contexte qu’une seule fois: obtenir le contexte est une tâche coûteuse, et l’obtenir une fois s’assure que l’application préserve l’ordre prévu de messages envoyés aux clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="6b0a8-212">Obtenir `Clients` la propriété du contexte et `StockTickerClient` de le mettre dans la propriété vous permet d’écrire du code pour appeler les méthodes client qui ressemble à ce qu’il ferait dans une `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="6b0a8-213">Par exemple, pour diffuser à `Clients.All.updateStockPrice(stock)`tous les clients que vous pouvez écrire .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="6b0a8-214">La `updateStockPrice` méthode que vous `BroadcastStockPrice` appelez n’existe pas encore.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="6b0a8-215">Vous l’ajouterez plus tard lorsque vous rédigerez du code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="6b0a8-216">Vous pouvez `updateStockPrice` vous `Clients.All` référer ici parce que c’est dynamique, ce qui signifie que l’application évaluera l’expression au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="6b0a8-217">Lorsque cet appel de méthode s’exécute, SignalR enverra le nom de la méthode `updateStockPrice`et la valeur de paramètre au client, et si le client a une méthode nommée, l’application appellera cette méthode et lui transmettra la valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="6b0a8-218">`Clients.All`signifie envoyer à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="6b0a8-219">SignalR vous offre d’autres options pour spécifier à quels clients ou groupes de clients envoyer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="6b0a8-220">Pour plus d’informations, voir [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="6b0a8-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="6b0a8-221">Enregistrer l’itinéraire SignalR</span><span class="sxs-lookup"><span data-stu-id="6b0a8-221">Register the SignalR route</span></span>

<span data-ttu-id="6b0a8-222">Le serveur doit savoir quelle URL intercepter et directement à SignalR.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="6b0a8-223">Pour ce faire, ajoutez une classe de démarrage OWIN :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="6b0a8-224">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **un nouvel article**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="6b0a8-225">Dans **Ajouter un nouvel article - SignalR.StockTicker** sélectionnez Le**Web** **visual CMD** >  **installé,** > puis sélectionnez **OWIN Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="6b0a8-226">Nommez la classe *Startup* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="6b0a8-227">Remplacez le code par défaut dans le fichier *Startup.cs* avec ce code :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="6b0a8-228">Vous avez maintenant fini de configurer le code serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="6b0a8-229">Dans la section suivante, vous configurerez le client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="6b0a8-230">Configurer le code client</span><span class="sxs-lookup"><span data-stu-id="6b0a8-230">Set up the client code</span></span>

<span data-ttu-id="6b0a8-231">Dans cette section, vous configurez le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="6b0a8-232">Créez la page HTML et le fichier JavaScript</span><span class="sxs-lookup"><span data-stu-id="6b0a8-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="6b0a8-233">La page HTML affichera les données et le fichier JavaScript organisera les données.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="6b0a8-234">Créer StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="6b0a8-234">Create StockTicker.html</span></span>

<span data-ttu-id="6b0a8-235">Tout d’abord, vous ajouterez le client HTML.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="6b0a8-236">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **la page HTML**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="6b0a8-237">Nommez le fichier *StockTicker* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="6b0a8-238">Remplacez le code par défaut dans le fichier *StockTicker.html* avec ce code :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="6b0a8-239">Le HTML crée une table avec cinq colonnes, une rangée d’en-têtes, et une ligne de données avec une seule cellule qui s’étend sur les cinq colonnes.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="6b0a8-240">La ligne de données montre "chargement..." momentanément lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="6b0a8-241">Le code JavaScript supprimera cette ligne et ajoutera dans ses rangées de place avec les données de stock récupérées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="6b0a8-242">Les balises de script spécifient :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-242">The script tags specify:</span></span>

    * <span data-ttu-id="6b0a8-243">Le fichier de script jQuery.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-243">The jQuery script file.</span></span>

    * <span data-ttu-id="6b0a8-244">Le fichier de script de base SignalR.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="6b0a8-245">Le fichier de script SignalR procurations.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="6b0a8-246">Un fichier de script StockTicker que vous créerez plus tard.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="6b0a8-247">L’application génère dynamiquement le fichier de script SignalR proxies.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="6b0a8-248">Il spécifie l’URL "/signaleur/hubs" et définit les méthodes proxy pour `StockTickerHub.GetAllStocks`les méthodes de la classe Hub, dans ce cas, pour .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="6b0a8-249">Si vous préférez, vous pouvez générer ce fichier JavaScript manuellement en utilisant [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="6b0a8-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="6b0a8-250">N’oubliez pas de désactiver la `MapHubs` création de fichiers dynamique dans l’appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="6b0a8-251">Dans **Solution Explorer**, élargir **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="6b0a8-252">Les bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6b0a8-253">Le gestionnaire de paquet installera une version ultérieure des scripts SignalR.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="6b0a8-254">Mettre à jour les références de script dans le bloc de code pour correspondre aux versions des fichiers script du projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="6b0a8-255">Dans **Solution Explorer**, clic droit *StockTicker.html*, puis sélectionnez **Set comme Page de départ**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="6b0a8-256">Créer StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="6b0a8-256">Create StockTicker.js</span></span>

<span data-ttu-id="6b0a8-257">Maintenant, créez le fichier JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="6b0a8-258">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez **Ajouter** > **Le fichier JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="6b0a8-259">Nommez le fichier *StockTicker* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="6b0a8-260">Ajoutez ce code au fichier *StockTicker.js* :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="6b0a8-261">Examiner le code client</span><span class="sxs-lookup"><span data-stu-id="6b0a8-261">Examine the client code</span></span>

<span data-ttu-id="6b0a8-262">Si vous examinez le code client, il vous aidera à apprendre comment le code client interagit avec le code serveur pour faire fonctionner l’application.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="6b0a8-263">Démarrage de la connexion</span><span class="sxs-lookup"><span data-stu-id="6b0a8-263">Starting the connection</span></span>

<span data-ttu-id="6b0a8-264">`$.connection`se réfère aux procurations SignalR.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="6b0a8-265">Le code obtient une référence `StockTickerHub` au proxy pour `ticker` la classe et le met dans la variable.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="6b0a8-266">Le nom de procuration est le `HubName` nom qui a été défini par l’attribut:</span><span class="sxs-lookup"><span data-stu-id="6b0a8-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="6b0a8-267">Après avoir défini toutes les variables et fonctions, la dernière ligne de code du `start` fichier est para parasciné la connexion SignalR en appelant la fonction SignalR.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="6b0a8-268">La `start` fonction exécute asynchronement et renvoie un [objet différé jQuery](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="6b0a8-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="6b0a8-269">Vous pouvez appeler la fonction accomplie pour spécifier la fonction à appeler lorsque l’application termine l’action asynchrone.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="6b0a8-270">Obtenir tous les stocks</span><span class="sxs-lookup"><span data-stu-id="6b0a8-270">Getting all the stocks</span></span>

<span data-ttu-id="6b0a8-271">La `init` fonction `getAllStocks` appelle la fonction sur le serveur et utilise les informations que le serveur retourne pour mettre à jour la table de stock.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="6b0a8-272">Notez que, par défaut, vous devez utiliser camelCasing sur le client, même si le nom de la méthode est pascal-cased sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="6b0a8-273">La règle de la camelcasing ne s’applique qu’aux méthodes, et non aux objets.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="6b0a8-274">Par exemple, vous `stock.Symbol` `stock.Price`vous `stock.symbol` référez à et , pas ou `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="6b0a8-275">Dans `init` la méthode, l’application crée HTML pour une ligne de `formatStock` table pour `stock` chaque objet d’actions reçu du serveur en appelant aux propriétés de format de l’objet, puis en appelant `supplant` pour remplacer les propriétaires de place dans la `rowTemplate` variable avec les valeurs de propriété `stock` d’objet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="6b0a8-276">Le HTML résultant est ensuite joint à la table de stock.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="6b0a8-277">Vous `init` appelez en le `callback` passant comme une fonction qui exécute `start` après la fonction asynchrone se termine.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="6b0a8-278">Si vous `init` avez appelé comme une `start`déclaration JavaScript séparée après l’appel , la fonction échouerait parce qu’il fonctionnerait immédiatement sans attendre la fonction de départ pour finir d’établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="6b0a8-279">Dans ce cas, la `init` fonction `getAllStocks` essaierait d’appeler la fonction avant que l’application n’établisse une connexion serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="6b0a8-280">Obtenir des prix des actions mis à jour</span><span class="sxs-lookup"><span data-stu-id="6b0a8-280">Getting updated stock prices</span></span>

<span data-ttu-id="6b0a8-281">Lorsque le serveur change le prix d’une action, il appelle les `updateStockPrice` clients connectés.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="6b0a8-282">L’application ajoute la fonction à `stockTicker` la propriété client du proxy pour le rendre disponible pour les appels à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="6b0a8-283">La `updateStockPrice` fonction formate un objet stock reçu du serveur dans `init` une ligne de table de la même manière que dans la fonction.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="6b0a8-284">Au lieu d’enchanger la ligne à la table, il trouve la ligne actuelle du stock dans la table et remplace cette rangée par la nouvelle.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="6b0a8-285">Test de l’application</span><span class="sxs-lookup"><span data-stu-id="6b0a8-285">Test the application</span></span>

<span data-ttu-id="6b0a8-286">Vous pouvez tester l’application pour vous assurer qu’elle fonctionne.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="6b0a8-287">Vous verrez toutes les fenêtres du navigateur afficher la table de stock en direct avec des prix des actions fluctuant.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="6b0a8-288">Dans la barre d’outils, activez **Script Debugging,** puis sélectionnez le bouton de lecture pour exécuter l’application en mode Debug.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Capture d’écran de l’utilisateur tournant sur le mode de débogage et la sélection de jeu.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="6b0a8-290">Une fenêtre du navigateur s’ouvrira affichant la **table de stock en direct**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="6b0a8-291">Le tableau des actions montre d’abord le "chargement..." ligne, puis, après un court laps de temps, l’application affiche les données initiales des stocks, puis les prix des actions commencent à changer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="6b0a8-292">Copiez l’URL du navigateur, ouvrez deux autres navigateurs et collez les URL dans les barres d’adresse.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="6b0a8-293">L’affichage initial du stock est le même que le premier navigateur et les modifications se produisent simultanément.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="6b0a8-294">Fermez tous les navigateurs, ouvrez un nouveau navigateur et passez à la même URL.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="6b0a8-295">L’objet De singleton StockTicker a continué à s’exécuter dans le serveur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="6b0a8-296">Le **tableau des actions en direct** montre que les actions ont continué de changer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="6b0a8-297">Vous ne voyez pas le tableau initial avec zéro chiffre de changement.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="6b0a8-298">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="6b0a8-299">Activation de la journalisation</span><span class="sxs-lookup"><span data-stu-id="6b0a8-299">Enable logging</span></span>

<span data-ttu-id="6b0a8-300">SignalR dispose d’une fonction d’enregistrement intégrée que vous pouvez activer sur le client pour aider au dépannage.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="6b0a8-301">Dans cette section, vous activez l’enregistrement et voyez des exemples qui montrent comment les journaux vous indiquent laquelle des méthodes de transport suivantes SignalR utilise :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="6b0a8-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par l’IIS 8 et les navigateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="6b0a8-303">[Événements envoyés par le serveur,](http://en.wikipedia.org/wiki/Server-sent_events)pris en charge par des navigateurs autres qu’Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="6b0a8-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="6b0a8-305">[Ajax long sondage](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), soutenu par tous les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="6b0a8-306">Pour une connexion donnée, SignalR choisit la meilleure méthode de transport que le serveur et le support client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="6b0a8-307">Ouvert *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="6b0a8-308">Ajoutez cette ligne de code mise en évidence pour activer la connexion immédiatement avant le code qui initialise la connexion à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="6b0a8-309">Appuyez sur **F5** pour mener à bien le projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="6b0a8-310">Ouvrez la fenêtre d’outils de développeur de votre navigateur et sélectionnez la Console pour voir les journaux.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="6b0a8-311">Vous devrez peut-être rafraîchir la page pour voir les journaux de SignalR négocier le mode de transport pour une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="6b0a8-312">Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), le mode de transport est **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="6b0a8-313">Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7.5), le mode de transport est **iframe**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="6b0a8-314">Si vous exécutez Firefox 19 sur Windows 8 (IIS 8), le mode de transport est **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="6b0a8-315">Dans Firefox, installez l’add-in Firebug pour obtenir une fenêtre Console.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="6b0a8-316">Si vous exécutez Firefox 19 sur Windows 7 (IIS 7.5), le mode de transport est **envoyé par serveur.**</span><span class="sxs-lookup"><span data-stu-id="6b0a8-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="6b0a8-317">Installer l’échantillon StockTicker</span><span class="sxs-lookup"><span data-stu-id="6b0a8-317">Install the StockTicker sample</span></span>

<span data-ttu-id="6b0a8-318">[L’échantillon Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe l’application StockTicker.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="6b0a8-319">Le package NuGet comprend plus de fonctionnalités que la version simplifiée que vous avez créée à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="6b0a8-320">Dans cette section du tutoriel, vous installez le paquet NuGet et passez en revue les nouvelles fonctionnalités et le code qui les implémente.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6b0a8-321">Si vous installez le paquet sans effectuer les étapes antérieures de ce tutoriel, vous devez ajouter une classe de démarrage OWIN à votre projet.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="6b0a8-322">Ce fichier readme.txt pour le paquet NuGet explique cette étape.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="6b0a8-323">Installer le paquet NuGet SignalR.Sample</span><span class="sxs-lookup"><span data-stu-id="6b0a8-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="6b0a8-324">Dans **Solution Explorer**, cliquez à droite sur le projet et sélectionnez Manage **NuGet Packages**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="6b0a8-325">Dans **NuGet Package manager: SignalR.StockTicker**, **sélectionnez Browse**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="6b0a8-326">À partir de **la source de paquet**, sélectionnez **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="6b0a8-327">Entrez *SignalR.Sample* dans la boîte de recherche et sélectionnez **Microsoft.AspNet.SignalR.Sample** > **Install**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="6b0a8-328">Dans **Solution Explorer**, étendre le dossier *SignalR.Sample.*</span><span class="sxs-lookup"><span data-stu-id="6b0a8-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="6b0a8-329">L’installation du paquet SignalR.Sample a créé le dossier et son contenu.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="6b0a8-330">Dans le dossier *SignalR.Sample,* cliquez à droite *StockTicker.html*, puis sélectionnez **Set As Start Page**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6b0a8-331">L’installation du paquet NuGet SignalR.Sample peut modifier la version de jQuery que vous avez dans votre dossier *Scripts.*</span><span class="sxs-lookup"><span data-stu-id="6b0a8-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="6b0a8-332">Le nouveau fichier *StockTicker.html* que le paquet installe dans le dossier *SignalR.Sample* sera en phase avec la version jQuery que le paquet installe, mais si vous voulez exécuter votre fichier *StockTicker.html* d’origine à nouveau, vous pourriez avoir à mettre à jour la référence jQuery dans le script tag en premier.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="6b0a8-333">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="6b0a8-333">Run the application</span></span>

 <span data-ttu-id="6b0a8-334">Le tableau que vous avez vu dans la première application avait des fonctionnalités utiles.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="6b0a8-335">L’application complète de ticker stock affiche de nouvelles fonctionnalités : une fenêtre horizontalement défilante qui montre les données de stock et les stocks qui changent de couleur au fur et à mesure qu’ils montent et tombent.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="6b0a8-336">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="6b0a8-337">Lorsque vous exécutez l’application pour la première fois, le «marché» est «fermé» et vous voyez une table statique et une fenêtre de ticker qui ne fait pas défiler.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="6b0a8-338">Sélectionnez **Marché ouvert**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-338">Select **Open Market**.</span></span>

    ![Capture d’écran du ticker en direct.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="6b0a8-340">La boîte **Live Stock Ticker** commence à faire défiler horizontalement, et le serveur commence à diffuser périodiquement des changements de cours de l’action sur une base aléatoire.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="6b0a8-341">Chaque fois qu’un cours de l’action change, l’application met à jour à la fois la **table d’actions en direct** et le **Ticker actions en direct**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="6b0a8-342">Lorsque le changement de prix d’une action est positif, l’application affiche le stock avec un fond vert.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="6b0a8-343">Lorsque le changement est négatif, l’application affiche le stock avec un fond rouge.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="6b0a8-344">Sélectionnez **Close Market**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="6b0a8-345">Les mises à jour de table s’arrêtent.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-345">The table updates stop.</span></span>

    * <span data-ttu-id="6b0a8-346">Le ticker cesse de faire défiler.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="6b0a8-347">Sélectionnez **Réinitialiser**.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-347">Select **Reset**.</span></span>

    * <span data-ttu-id="6b0a8-348">Toutes les données de stock sont réinitialisées.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-348">All stock data is reset.</span></span>

    * <span data-ttu-id="6b0a8-349">L’application restaure l’état initial avant que les changements de prix ne commencent.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="6b0a8-350">Copiez l’URL du navigateur, ouvrez deux autres navigateurs et collez les URL dans les barres d’adresse.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="6b0a8-351">Vous voyez les mêmes données mises à jour dynamiquement en même temps dans chaque navigateur.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="6b0a8-352">Lorsque vous sélectionnez l’un des contrôles, tous les navigateurs répondent de la même manière en même temps.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="6b0a8-353">Affichage en direct stock Ticker</span><span class="sxs-lookup"><span data-stu-id="6b0a8-353">Live Stock Ticker display</span></span>

<span data-ttu-id="6b0a8-354">**L’écran Live Stock Ticker** est une `<div>` liste non ordonnée dans un élément formaté en une seule ligne par les styles CSS.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="6b0a8-355">L’application initialise et met à jour le ticker de la même `<li>` manière que la `<li>` table : `<ul>` en remplaçant les détenteurs de place dans une chaîne de modèle et en ajoutant dynamiquement les éléments à l’élément.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="6b0a8-356">L’application comprend le défilement `animate` en utilisant la fonction jQuery pour varier `<div>`la marge gauche de la liste non ordonnée dans le .</span><span class="sxs-lookup"><span data-stu-id="6b0a8-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="6b0a8-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="6b0a8-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="6b0a8-358">Le code HTML de ticker de stock :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="6b0a8-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="6b0a8-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="6b0a8-360">Le code CSS de ticker de stock :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="6b0a8-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="6b0a8-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="6b0a8-362">Le code jQuery qui le fait faire défiler:</span><span class="sxs-lookup"><span data-stu-id="6b0a8-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="6b0a8-363">Méthodes supplémentaires sur le serveur que le client peut appeler</span><span class="sxs-lookup"><span data-stu-id="6b0a8-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="6b0a8-364">Pour ajouter de la flexibilité à l’application, il existe des méthodes supplémentaires que l’application peut appeler.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="6b0a8-365">SignalR.Échantillon StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="6b0a8-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="6b0a8-366">La `StockTickerHub` classe définit quatre méthodes supplémentaires que le client peut appeler :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="6b0a8-367">L’application `OpenMarket` `CloseMarket`appelle `Reset` , , et en réponse aux boutons en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="6b0a8-368">Ils démontrent le modèle d’un client déclenchant un changement d’état immédiatement propagé à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="6b0a8-369">Chacune de ces méthodes `StockTicker` appelle une méthode dans la classe qui provoque le changement d’état du marché, puis diffuse le nouvel état.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="6b0a8-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="6b0a8-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="6b0a8-371">Dans `StockTicker` la catégorie, l’application maintient l’état du marché avec une `MarketState` propriété qui retourne une `MarketState` valeur enum:</span><span class="sxs-lookup"><span data-stu-id="6b0a8-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="6b0a8-372">Chacune des méthodes qui modifient l’état du `StockTicker` marché le font à l’intérieur d’un bloc de verrouillage parce que la classe doit être sans fil:</span><span class="sxs-lookup"><span data-stu-id="6b0a8-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="6b0a8-373">Pour s’assurer que ce code `_marketState` est sans `MarketState` fil, `volatile`le champ qui soutient la propriété désignée :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="6b0a8-374">Les `BroadcastMarketStateChange` `BroadcastMarketReset` méthodes et les méthodes sont similaires à la méthode BroadcastStockPrice que vous avez déjà vue, sauf qu’elles appellent différentes méthodes définies chez le client :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="6b0a8-375">Fonctions supplémentaires sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="6b0a8-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="6b0a8-376">La `updateStockPrice` fonction gère maintenant à la fois la table `jQuery.Color` et l’affichage ticker, et il utilise pour clignoter les couleurs rouges et vertes.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="6b0a8-377">De nouvelles fonctions dans *SignalR.StockTicker.js* activent et désactivent les boutons en fonction de l’état du marché.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="6b0a8-378">Ils arrêtent ou démarrent également le défilement horizontal **Live Stock Ticker.**</span><span class="sxs-lookup"><span data-stu-id="6b0a8-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="6b0a8-379">Étant donné que de `ticker.client`nombreuses fonctions sont ajoutées à , l’application utilise la [fonction d’extension jQuery](http://api.jquery.com/jQuery.extend/) pour les ajouter.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="6b0a8-380">Configuration supplémentaire du client après l’établissement de la connexion</span><span class="sxs-lookup"><span data-stu-id="6b0a8-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="6b0a8-381">Une fois que le client établit la connexion, il a un travail supplémentaire à faire :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="6b0a8-382">Découvrez si le marché est ouvert ou `marketOpened` `marketClosed` fermé pour appeler le approprié ou la fonction.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="6b0a8-383">Fixez les appels de méthode serveur aux boutons.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="6b0a8-384">Les méthodes du serveur ne sont pas câblées vers les boutons jusqu’à ce que l’application ait établi la connexion.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="6b0a8-385">C’est ainsi que le code ne peut pas appeler les méthodes du serveur avant qu’elles ne soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6b0a8-386">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6b0a8-386">Additional resources</span></span>

<span data-ttu-id="6b0a8-387">Dans ce tutoriel, vous avez appris à programmer une application SignalR qui diffuse des messages à partir du serveur à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="6b0a8-388">Vous pouvez maintenant diffuser des messages périodiquement et en réponse aux notifications de n’importe quel client.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="6b0a8-389">Vous pouvez utiliser le concept d’instance singleton multi-threaded pour maintenir l’état du serveur dans des scénarios de jeu en ligne multi-joueurs.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="6b0a8-390">Par exemple, voir [le jeu ShootR basé sur SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="6b0a8-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="6b0a8-391">Pour les tutoriels qui montrent des scénarios de communication peer-to-peer, voir [Getting Started with SignalR](introduction-to-signalr.md) et [Real-Time Update with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="6b0a8-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="6b0a8-392">Pour en savoir plus sur SignalR, voir les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="6b0a8-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="6b0a8-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="6b0a8-394">Projet SignalR</span><span class="sxs-lookup"><span data-stu-id="6b0a8-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="6b0a8-395">SignalR GitHub et échantillons</span><span class="sxs-lookup"><span data-stu-id="6b0a8-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="6b0a8-396">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="6b0a8-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="6b0a8-397">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="6b0a8-397">Next steps</span></span>

<span data-ttu-id="6b0a8-398">Dans ce tutoriel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="6b0a8-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b0a8-399">Création du projet</span><span class="sxs-lookup"><span data-stu-id="6b0a8-399">Created the project</span></span>
> * <span data-ttu-id="6b0a8-400">Configurer le code du serveur</span><span class="sxs-lookup"><span data-stu-id="6b0a8-400">Set up the server code</span></span>
> * <span data-ttu-id="6b0a8-401">Examen du code serveur</span><span class="sxs-lookup"><span data-stu-id="6b0a8-401">Examined the server code</span></span>
> * <span data-ttu-id="6b0a8-402">Configurer le code client</span><span class="sxs-lookup"><span data-stu-id="6b0a8-402">Set up the client code</span></span>
> * <span data-ttu-id="6b0a8-403">Examen du code client</span><span class="sxs-lookup"><span data-stu-id="6b0a8-403">Examined the client code</span></span>
> * <span data-ttu-id="6b0a8-404">Test de l’application</span><span class="sxs-lookup"><span data-stu-id="6b0a8-404">Tested the application</span></span>
> * <span data-ttu-id="6b0a8-405">Enregistrement activé</span><span class="sxs-lookup"><span data-stu-id="6b0a8-405">Enabled logging</span></span>

<span data-ttu-id="6b0a8-406">Avancez à l’article suivant pour apprendre à créer une application Web en temps réel qui utilise ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="6b0a8-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6b0a8-407">Créez une application web en temps réel avec SignalR</span><span class="sxs-lookup"><span data-stu-id="6b0a8-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
