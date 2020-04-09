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
# <a name="mapping-signalr-users-to-connections"></a>Mappage des utilisateurs SignalR aux connexions

 par [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce sujet montre comment conserver des informations sur les utilisateurs et leurs connexions.
>
> Patrick Fletcher a aidé à écrire ce sujet.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans ce sujet
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Version SignalR 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de ce sujet
>
> Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page. Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="introduction"></a>Introduction

Chaque client se connectant à un hub passe une connexion unique. Vous pouvez récupérer cette `Context.ConnectionId` valeur dans la propriété du contexte du hub. Si votre application doit cartographier un utilisateur vers l’id de connexion et persister cette cartographie, vous pouvez utiliser l’un des éléments suivants :

- [Le fournisseur d’identification utilisateur (Signalr 2)](#IUserIdProvider)
- [Stockage en mémoire](#inmemory), comme un dictionnaire
- [Groupe SignalR pour chaque utilisateur](#groups)
- [Stockage externe permanent,](#database)comme une table de base de données ou un stockage de table Azure

Chacune de ces implémentations est montrée dans ce sujet. Vous utilisez `OnConnected` `OnDisconnected`le `OnReconnected` , , `Hub` et les méthodes de la classe pour suivre l’état de connexion utilisateur.

La meilleure approche pour votre application dépend de :

- Le nombre de serveurs Web hébergeant votre application.
- Si vous avez besoin d’obtenir une liste des utilisateurs actuellement connectés.
- Que vous ayez besoin de persister les informations de groupe et d’utilisateur lorsque l’application ou le serveur redémarre.
- La question de savoir si la latence d’appeler un serveur externe est un problème.

Le tableau suivant montre quelle approche fonctionne pour ces considérations.

|  | Plus d’un serveur | Obtenez la liste des utilisateurs actuellement connectés | Informations persistantes après redémarrage | Performances optimales |
| --- | --- | --- | --- | --- |
| Fournisseur UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| En mémoire |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Groupes d’utilisateurs uniques | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanent, externe | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Fournisseur IUserID

Cette fonctionnalité permet aux utilisateurs de spécifier ce que l’utilisateurId est basé sur un IRequest via une nouvelle interface IUserIdProvider.

**L’IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Par défaut, il y aura une implémentation qui utilise l’utilisateur comme nom d’utilisateur. `IPrincipal.Identity.Name` Pour modifier cela, enregistrez votre mise en œuvre auprès de l’hôte `IUserIdProvider` mondial lorsque votre application commence :

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

À partir d’un hub, vous serez en mesure d’envoyer des messages à ces utilisateurs via l’API suivante :

**Envoi d’un message à un utilisateur spécifique**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Stockage en mémoire

Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans un dictionnaire qui est stocké dans la mémoire. Le dictionnaire `HashSet` utilise un pour stocker l’id de connexion. À tout moment, un utilisateur peut avoir plus d’une connexion à l’application SignalR. Par exemple, un utilisateur connecté par plusieurs appareils ou plus d’un onglet de navigateur aurait plus d’un identifiant de connexion.

Si l’application s’arrête, toutes les informations sont perdues, mais elles seront re-peuplées au fur et à mesure que les utilisateurs rétabliront leurs connexions. Le stockage en mémoire ne fonctionne pas si votre environnement inclut plus d’un serveur Web parce que chaque serveur aurait une collection distincte de connexions.

Le premier exemple montre une classe qui gère la cartographie des utilisateurs vers les connexions. La clé pour le HashSet sera le nom de l’utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

L’exemple suivant montre comment utiliser la classe de cartographie de connexion à partir d’un hub. L’instance de la classe est `_connections`stockée dans un nom variable .

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Groupes d’utilisateurs uniques

Vous pouvez créer un groupe pour chaque utilisateur, puis envoyer un message à ce groupe lorsque vous souhaitez atteindre uniquement cet utilisateur. Le nom de chaque groupe est le nom de l’utilisateur. Si un utilisateur a plus d’une connexion, chaque identifiant de connexion est ajouté au groupe de l’utilisateur.

Vous ne devez pas supprimer manuellement l’utilisateur du groupe lorsque l’utilisateur se déconnecte. Cette action est automatiquement effectuée par le cadre SignalR.

L’exemple suivant montre comment implémenter des groupes d’utilisateurs uniques.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Stockage permanent et externe

Ce sujet montre comment utiliser une base de données ou un stockage de table Azure pour stocker des informations de connexion. Cette approche fonctionne lorsque vous avez plusieurs serveurs Web parce que chaque serveur Web peut interagir avec le même référentiel de données. Si vos serveurs Web cessent de fonctionner `OnDisconnected` ou si l’application redémarre, la méthode n’est pas appelée. Par conséquent, il est possible que votre référentiel de données dispose d’enregistrements pour les identifiants de connexion qui ne sont plus valides. Pour nettoyer ces dossiers orphelins, vous pouvez invalider tout lien qui a été créé en dehors d’un délai qui est pertinent pour votre demande. Les exemples dans cette section incluent une valeur pour le suivi lorsque la connexion a été créée, mais ne montrent pas comment nettoyer les vieux enregistrements parce que vous pouvez le faire comme processus d’arrière-plan.

### <a name="database"></a>Base de données

Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans une base de données. Vous pouvez utiliser n’importe quelle technologie d’accès aux données; cependant, l’exemple ci-dessous montre comment définir les modèles à l’aide du cadre d’entité. Ces modèles d’entités correspondent aux tables et aux champs de base de données. Votre structure de données peut varier considérablement en fonction des exigences de votre application.

Le premier exemple montre comment définir une entité utilisateur qui peut être associée à de nombreuses entités de connexion.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Ensuite, à partir du hub, vous pouvez suivre l’état de chaque connexion avec le code ci-dessous.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Stockage de table Azure

L’exemple de stockage de table Azure suivant est similaire à l’exemple de la base de données. Il n’inclut pas toutes les informations dont vous auriez besoin pour commencer avec le service de stockage de table Azure. Pour plus d’informations, voir [Comment utiliser le stockage de table à partir de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

L’exemple suivant montre une entité de table pour stocker des informations de connexion. Il divise les données par nom d’utilisateur, et identifie chaque entité par l’id de connexion, de sorte qu’un utilisateur peut avoir plusieurs connexions à tout moment.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Dans le hub, vous suivez l’état de la connexion de chaque utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
