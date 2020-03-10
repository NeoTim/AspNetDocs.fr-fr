---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mappage des utilisateurs Signalr aux connexions dans Signalr 1. x | Microsoft Docs
author: bradygaster
description: Cette rubrique montre comment conserver des informations sur les utilisateurs et leurs connexions.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558484"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="8a169-103">Mappage des utilisateurs SignalR aux connexions dans SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="8a169-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>

<span data-ttu-id="8a169-104">de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8a169-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="8a169-105">Cette rubrique montre comment conserver des informations sur les utilisateurs et leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="8a169-105">This topic shows how to retain information about users and their connections.</span></span>

## <a name="introduction"></a><span data-ttu-id="8a169-106">Introduction</span><span class="sxs-lookup"><span data-stu-id="8a169-106">Introduction</span></span>

<span data-ttu-id="8a169-107">Chaque client qui se connecte à un hub passe un ID de connexion unique. Vous pouvez récupérer cette valeur dans la propriété `Context.ConnectionId` du contexte du Hub.</span><span class="sxs-lookup"><span data-stu-id="8a169-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="8a169-108">Si votre application doit mapper un utilisateur à l’ID de connexion et conserver ce mappage, vous pouvez utiliser l’une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="8a169-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="8a169-109">[Stockage en mémoire](#inmemory), tel qu’un dictionnaire</span><span class="sxs-lookup"><span data-stu-id="8a169-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="8a169-110">Groupe signalr pour chaque utilisateur</span><span class="sxs-lookup"><span data-stu-id="8a169-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="8a169-111">[Stockage externe et permanent](#database), tel qu’une table de base de données ou un stockage table Azure</span><span class="sxs-lookup"><span data-stu-id="8a169-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="8a169-112">Chacune de ces implémentations est présentée dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="8a169-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="8a169-113">Vous utilisez les méthodes `OnConnected`, `OnDisconnected`et `OnReconnected` de la classe `Hub` pour suivre l’état de la connexion utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a169-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="8a169-114">La meilleure approche pour votre application dépend des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8a169-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="8a169-115">Le nombre de serveurs Web qui hébergent votre application.</span><span class="sxs-lookup"><span data-stu-id="8a169-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="8a169-116">Si vous avez besoin d’obtenir la liste des utilisateurs actuellement connectés.</span><span class="sxs-lookup"><span data-stu-id="8a169-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="8a169-117">Si vous devez conserver des informations sur les groupes et les utilisateurs lors du redémarrage de l’application ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="8a169-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="8a169-118">Indique si la latence de l’appel d’un serveur externe est un problème.</span><span class="sxs-lookup"><span data-stu-id="8a169-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="8a169-119">Le tableau suivant indique l’approche qui fonctionne pour ces considérations.</span><span class="sxs-lookup"><span data-stu-id="8a169-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="8a169-120">Plusieurs serveurs</span><span class="sxs-lookup"><span data-stu-id="8a169-120">More than one server</span></span> | <span data-ttu-id="8a169-121">Obtient la liste des utilisateurs actuellement connectés</span><span class="sxs-lookup"><span data-stu-id="8a169-121">Get list of currently connected users</span></span> | <span data-ttu-id="8a169-122">Conserver les informations après le redémarrage</span><span class="sxs-lookup"><span data-stu-id="8a169-122">Persist information after restarts</span></span> | <span data-ttu-id="8a169-123">Performances optimales</span><span class="sxs-lookup"><span data-stu-id="8a169-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="8a169-124">En mémoire</span><span class="sxs-lookup"><span data-stu-id="8a169-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="8a169-125">Groupes à un seul utilisateur</span><span class="sxs-lookup"><span data-stu-id="8a169-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="8a169-126">Permanent, externe</span><span class="sxs-lookup"><span data-stu-id="8a169-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="8a169-127">Stockage en mémoire</span><span class="sxs-lookup"><span data-stu-id="8a169-127">In-memory storage</span></span>

<span data-ttu-id="8a169-128">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans un dictionnaire stocké en mémoire.</span><span class="sxs-lookup"><span data-stu-id="8a169-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="8a169-129">Le dictionnaire utilise une `HashSet` pour stocker l’ID de connexion. À tout moment, un utilisateur peut avoir plusieurs connexions à l’application Signalr.</span><span class="sxs-lookup"><span data-stu-id="8a169-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="8a169-130">Par exemple, un utilisateur connecté via plusieurs appareils ou plusieurs onglets de navigateur a plusieurs ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="8a169-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="8a169-131">Si l’application s’arrête, toutes les informations sont perdues, mais elles sont à nouveau renseignées à mesure que les utilisateurs rétablissent leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="8a169-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="8a169-132">Le stockage en mémoire ne fonctionne pas si votre environnement comprend plusieurs serveurs Web, car chaque serveur possède un regroupement distinct de connexions.</span><span class="sxs-lookup"><span data-stu-id="8a169-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="8a169-133">Le premier exemple montre une classe qui gère le mappage des utilisateurs aux connexions.</span><span class="sxs-lookup"><span data-stu-id="8a169-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="8a169-134">La clé pour HashSet sera le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a169-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="8a169-135">L’exemple suivant montre comment utiliser la classe de mappage de connexion à partir d’un Hub.</span><span class="sxs-lookup"><span data-stu-id="8a169-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="8a169-136">L’instance de la classe est stockée dans un nom de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="8a169-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="8a169-137">Groupes à un seul utilisateur</span><span class="sxs-lookup"><span data-stu-id="8a169-137">Single-user groups</span></span>

<span data-ttu-id="8a169-138">Vous pouvez créer un groupe pour chaque utilisateur, puis envoyer un message à ce groupe lorsque vous souhaitez atteindre uniquement cet utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a169-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="8a169-139">Le nom de chaque groupe est le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a169-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="8a169-140">Si un utilisateur a plusieurs connexions, chaque ID de connexion est ajouté au groupe de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a169-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="8a169-141">Vous ne devez pas supprimer manuellement l’utilisateur du groupe lorsque l’utilisateur se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="8a169-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="8a169-142">Cette action est effectuée automatiquement par l’infrastructure Signalr.</span><span class="sxs-lookup"><span data-stu-id="8a169-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="8a169-143">L’exemple suivant montre comment implémenter des groupes d’utilisateurs uniques.</span><span class="sxs-lookup"><span data-stu-id="8a169-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="8a169-144">Stockage externe et permanent</span><span class="sxs-lookup"><span data-stu-id="8a169-144">Permanent, external storage</span></span>

<span data-ttu-id="8a169-145">Cette rubrique montre comment utiliser une base de données ou un stockage table Azure pour stocker les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="8a169-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="8a169-146">Cette approche fonctionne lorsque vous avez plusieurs serveurs Web, car chaque serveur Web peut interagir avec le même référentiel de données.</span><span class="sxs-lookup"><span data-stu-id="8a169-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="8a169-147">Si vos serveurs Web cessent de fonctionner ou si l’application redémarre, la méthode `OnDisconnected` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="8a169-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="8a169-148">Par conséquent, il est possible que votre dépôt de données contiendra des enregistrements pour les ID de connexion qui ne sont plus valides.</span><span class="sxs-lookup"><span data-stu-id="8a169-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="8a169-149">Pour nettoyer ces enregistrements orphelins, vous pouvez invalider les connexions qui ont été créées en dehors d’une plage de temps adaptée à votre application.</span><span class="sxs-lookup"><span data-stu-id="8a169-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="8a169-150">Les exemples de cette section incluent une valeur pour le suivi lors de la création de la connexion, mais n’indiquent pas comment nettoyer les anciens enregistrements, car vous pouvez le faire en tant que processus en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="8a169-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="8a169-151">Base de données</span><span class="sxs-lookup"><span data-stu-id="8a169-151">Database</span></span>

<span data-ttu-id="8a169-152">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="8a169-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="8a169-153">Vous pouvez utiliser n’importe quelle technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a169-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="8a169-154">Ces modèles d’entité correspondent aux tables et aux champs de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8a169-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="8a169-155">La structure de vos données peut varier considérablement en fonction des exigences de votre application.</span><span class="sxs-lookup"><span data-stu-id="8a169-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="8a169-156">Le premier exemple montre comment définir une entité utilisateur qui peut être associée à de nombreuses entités de connexion.</span><span class="sxs-lookup"><span data-stu-id="8a169-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="8a169-157">Ensuite, à partir du Hub, vous pouvez suivre l’état de chaque connexion avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a169-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="8a169-158">Stockage de table Azure</span><span class="sxs-lookup"><span data-stu-id="8a169-158">Azure table storage</span></span>

<span data-ttu-id="8a169-159">L’exemple de stockage de table Azure suivant est similaire à l’exemple de base de données.</span><span class="sxs-lookup"><span data-stu-id="8a169-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="8a169-160">Il n’inclut pas toutes les informations dont vous pourriez avoir besoin pour vous familiariser avec le service de stockage table Azure.</span><span class="sxs-lookup"><span data-stu-id="8a169-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="8a169-161">Pour plus d’informations, consultez [utilisation du stockage de tables à partir de .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="8a169-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="8a169-162">L’exemple suivant montre une entité de table pour le stockage des informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="8a169-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="8a169-163">Elle partitionne les données par nom d’utilisateur et identifie chaque entité par l’ID de connexion, de sorte qu’un utilisateur peut avoir plusieurs connexions à tout moment.</span><span class="sxs-lookup"><span data-stu-id="8a169-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="8a169-164">Dans le Hub, vous suivez l’état de la connexion de chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a169-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
