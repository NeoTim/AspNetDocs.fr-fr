---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Cartographier les utilisateurs de SignalR vers les connexions ( Microsoft Docs
author: bradygaster
description: Ce sujet montre comment conserver des informations sur les utilisateurs et leurs connexions. Patrick Fletcher a aidé à écrire ce sujet. Versions logicielles utilisées dans ce sujet...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676159"
---
# <a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="59c00-105">Mappage des utilisateurs SignalR aux connexions</span><span class="sxs-lookup"><span data-stu-id="59c00-105">Mapping SignalR Users to Connections</span></span>

<span data-ttu-id="59c00-106"> par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="59c00-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="59c00-107">Ce sujet montre comment conserver des informations sur les utilisateurs et leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="59c00-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="59c00-108">Patrick Fletcher a aidé à écrire ce sujet.</span><span class="sxs-lookup"><span data-stu-id="59c00-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="59c00-109">Versions logicielles utilisées dans ce sujet</span><span class="sxs-lookup"><span data-stu-id="59c00-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="59c00-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="59c00-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="59c00-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="59c00-111">.NET 4.5</span></span>
> - <span data-ttu-id="59c00-112">Version SignalR 2</span><span class="sxs-lookup"><span data-stu-id="59c00-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="59c00-113">Versions précédentes de ce sujet</span><span class="sxs-lookup"><span data-stu-id="59c00-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="59c00-114">Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="59c00-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="59c00-115">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="59c00-115">Questions and comments</span></span>
>
> <span data-ttu-id="59c00-116">S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="59c00-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="59c00-117">Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="59c00-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="59c00-118">Introduction</span><span class="sxs-lookup"><span data-stu-id="59c00-118">Introduction</span></span>

<span data-ttu-id="59c00-119">Chaque client se connectant à un hub passe une connexion unique. Vous pouvez récupérer cette `Context.ConnectionId` valeur dans la propriété du contexte du hub.</span><span class="sxs-lookup"><span data-stu-id="59c00-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="59c00-120">Si votre application doit cartographier un utilisateur vers l’id de connexion et persister cette cartographie, vous pouvez utiliser l’un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="59c00-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="59c00-121">Le fournisseur d’identification utilisateur (Signalr 2)</span><span class="sxs-lookup"><span data-stu-id="59c00-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="59c00-122">[Stockage en mémoire](#inmemory), comme un dictionnaire</span><span class="sxs-lookup"><span data-stu-id="59c00-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="59c00-123">Groupe SignalR pour chaque utilisateur</span><span class="sxs-lookup"><span data-stu-id="59c00-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="59c00-124">[Stockage externe permanent,](#database)comme une table de base de données ou un stockage de table Azure</span><span class="sxs-lookup"><span data-stu-id="59c00-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="59c00-125">Chacune de ces implémentations est montrée dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="59c00-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="59c00-126">Vous utilisez `OnConnected` `OnDisconnected`le `OnReconnected` , , `Hub` et les méthodes de la classe pour suivre l’état de connexion utilisateur.</span><span class="sxs-lookup"><span data-stu-id="59c00-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="59c00-127">La meilleure approche pour votre application dépend de :</span><span class="sxs-lookup"><span data-stu-id="59c00-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="59c00-128">Le nombre de serveurs Web hébergeant votre application.</span><span class="sxs-lookup"><span data-stu-id="59c00-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="59c00-129">Si vous avez besoin d’obtenir une liste des utilisateurs actuellement connectés.</span><span class="sxs-lookup"><span data-stu-id="59c00-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="59c00-130">Que vous ayez besoin de persister les informations de groupe et d’utilisateur lorsque l’application ou le serveur redémarre.</span><span class="sxs-lookup"><span data-stu-id="59c00-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="59c00-131">La question de savoir si la latence d’appeler un serveur externe est un problème.</span><span class="sxs-lookup"><span data-stu-id="59c00-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="59c00-132">Le tableau suivant montre quelle approche fonctionne pour ces considérations.</span><span class="sxs-lookup"><span data-stu-id="59c00-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="59c00-133">Plus d’un serveur</span><span class="sxs-lookup"><span data-stu-id="59c00-133">More than one server</span></span> | <span data-ttu-id="59c00-134">Obtenez la liste des utilisateurs actuellement connectés</span><span class="sxs-lookup"><span data-stu-id="59c00-134">Get list of currently connected users</span></span> | <span data-ttu-id="59c00-135">Informations persistantes après redémarrage</span><span class="sxs-lookup"><span data-stu-id="59c00-135">Persist information after restarts</span></span> | <span data-ttu-id="59c00-136">Performances optimales</span><span class="sxs-lookup"><span data-stu-id="59c00-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="59c00-137">Fournisseur UserID</span><span class="sxs-lookup"><span data-stu-id="59c00-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="59c00-138">En mémoire</span><span class="sxs-lookup"><span data-stu-id="59c00-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="59c00-139">Groupes d’utilisateurs uniques</span><span class="sxs-lookup"><span data-stu-id="59c00-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="59c00-140">Permanent, externe</span><span class="sxs-lookup"><span data-stu-id="59c00-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="59c00-141">Fournisseur IUserID</span><span class="sxs-lookup"><span data-stu-id="59c00-141">IUserID provider</span></span>

<span data-ttu-id="59c00-142">Cette fonctionnalité permet aux utilisateurs de spécifier ce que l’utilisateurId est basé sur un IRequest via une nouvelle interface IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="59c00-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="59c00-143">**L’IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="59c00-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="59c00-144">Par défaut, il y aura une implémentation qui utilise l’utilisateur comme nom d’utilisateur. `IPrincipal.Identity.Name`</span><span class="sxs-lookup"><span data-stu-id="59c00-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="59c00-145">Pour modifier cela, enregistrez votre mise en œuvre auprès de l’hôte `IUserIdProvider` mondial lorsque votre application commence :</span><span class="sxs-lookup"><span data-stu-id="59c00-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="59c00-146">À partir d’un hub, vous serez en mesure d’envoyer des messages à ces utilisateurs via l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="59c00-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="59c00-147">**Envoi d’un message à un utilisateur spécifique**</span><span class="sxs-lookup"><span data-stu-id="59c00-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="59c00-148">Stockage en mémoire</span><span class="sxs-lookup"><span data-stu-id="59c00-148">In-memory storage</span></span>

<span data-ttu-id="59c00-149">Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans un dictionnaire qui est stocké dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="59c00-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="59c00-150">Le dictionnaire `HashSet` utilise un pour stocker l’id de connexion. À tout moment, un utilisateur peut avoir plus d’une connexion à l’application SignalR.</span><span class="sxs-lookup"><span data-stu-id="59c00-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="59c00-151">Par exemple, un utilisateur connecté par plusieurs appareils ou plus d’un onglet de navigateur aurait plus d’un identifiant de connexion.</span><span class="sxs-lookup"><span data-stu-id="59c00-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="59c00-152">Si l’application s’arrête, toutes les informations sont perdues, mais elles seront re-peuplées au fur et à mesure que les utilisateurs rétabliront leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="59c00-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="59c00-153">Le stockage en mémoire ne fonctionne pas si votre environnement inclut plus d’un serveur Web parce que chaque serveur aurait une collection distincte de connexions.</span><span class="sxs-lookup"><span data-stu-id="59c00-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="59c00-154">Le premier exemple montre une classe qui gère la cartographie des utilisateurs vers les connexions.</span><span class="sxs-lookup"><span data-stu-id="59c00-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="59c00-155">La clé pour le HashSet sera le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="59c00-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="59c00-156">L’exemple suivant montre comment utiliser la classe de cartographie de connexion à partir d’un hub.</span><span class="sxs-lookup"><span data-stu-id="59c00-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="59c00-157">L’instance de la classe est `_connections`stockée dans un nom variable .</span><span class="sxs-lookup"><span data-stu-id="59c00-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="59c00-158">Groupes d’utilisateurs uniques</span><span class="sxs-lookup"><span data-stu-id="59c00-158">Single-user groups</span></span>

<span data-ttu-id="59c00-159">Vous pouvez créer un groupe pour chaque utilisateur, puis envoyer un message à ce groupe lorsque vous souhaitez atteindre uniquement cet utilisateur.</span><span class="sxs-lookup"><span data-stu-id="59c00-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="59c00-160">Le nom de chaque groupe est le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="59c00-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="59c00-161">Si un utilisateur a plus d’une connexion, chaque identifiant de connexion est ajouté au groupe de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="59c00-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="59c00-162">Vous ne devez pas supprimer manuellement l’utilisateur du groupe lorsque l’utilisateur se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="59c00-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="59c00-163">Cette action est automatiquement effectuée par le cadre SignalR.</span><span class="sxs-lookup"><span data-stu-id="59c00-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="59c00-164">L’exemple suivant montre comment implémenter des groupes d’utilisateurs uniques.</span><span class="sxs-lookup"><span data-stu-id="59c00-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="59c00-165">Stockage permanent et externe</span><span class="sxs-lookup"><span data-stu-id="59c00-165">Permanent, external storage</span></span>

<span data-ttu-id="59c00-166">Ce sujet montre comment utiliser une base de données ou un stockage de table Azure pour stocker des informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="59c00-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="59c00-167">Cette approche fonctionne lorsque vous avez plusieurs serveurs Web parce que chaque serveur Web peut interagir avec le même référentiel de données.</span><span class="sxs-lookup"><span data-stu-id="59c00-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="59c00-168">Si vos serveurs Web cessent de fonctionner `OnDisconnected` ou si l’application redémarre, la méthode n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="59c00-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="59c00-169">Par conséquent, il est possible que votre référentiel de données dispose d’enregistrements pour les identifiants de connexion qui ne sont plus valides.</span><span class="sxs-lookup"><span data-stu-id="59c00-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="59c00-170">Pour nettoyer ces dossiers orphelins, vous pouvez invalider tout lien qui a été créé en dehors d’un délai qui est pertinent pour votre demande.</span><span class="sxs-lookup"><span data-stu-id="59c00-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="59c00-171">Les exemples dans cette section incluent une valeur pour le suivi lorsque la connexion a été créée, mais ne montrent pas comment nettoyer les vieux enregistrements parce que vous pouvez le faire comme processus d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="59c00-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="59c00-172">Base de données</span><span class="sxs-lookup"><span data-stu-id="59c00-172">Database</span></span>

<span data-ttu-id="59c00-173">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="59c00-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="59c00-174">Vous pouvez utiliser n’importe quelle technologie d’accès aux données; cependant, l’exemple ci-dessous montre comment définir les modèles à l’aide du cadre d’entité.</span><span class="sxs-lookup"><span data-stu-id="59c00-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="59c00-175">Ces modèles d’entités correspondent aux tables et aux champs de base de données.</span><span class="sxs-lookup"><span data-stu-id="59c00-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="59c00-176">Votre structure de données peut varier considérablement en fonction des exigences de votre application.</span><span class="sxs-lookup"><span data-stu-id="59c00-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="59c00-177">Le premier exemple montre comment définir une entité utilisateur qui peut être associée à de nombreuses entités de connexion.</span><span class="sxs-lookup"><span data-stu-id="59c00-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="59c00-178">Ensuite, à partir du hub, vous pouvez suivre l’état de chaque connexion avec le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="59c00-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="59c00-179">Stockage de table Azure</span><span class="sxs-lookup"><span data-stu-id="59c00-179">Azure table storage</span></span>

<span data-ttu-id="59c00-180">L’exemple de stockage de table Azure suivant est similaire à l’exemple de la base de données.</span><span class="sxs-lookup"><span data-stu-id="59c00-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="59c00-181">Il n’inclut pas toutes les informations dont vous auriez besoin pour commencer avec le service de stockage de table Azure.</span><span class="sxs-lookup"><span data-stu-id="59c00-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="59c00-182">Pour plus d’informations, voir [Comment utiliser le stockage de table à partir de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="59c00-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="59c00-183">L’exemple suivant montre une entité de table pour stocker des informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="59c00-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="59c00-184">Il divise les données par nom d’utilisateur, et identifie chaque entité par l’id de connexion, de sorte qu’un utilisateur peut avoir plusieurs connexions à tout moment.</span><span class="sxs-lookup"><span data-stu-id="59c00-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="59c00-185">Dans le hub, vous suivez l’état de la connexion de chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="59c00-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
