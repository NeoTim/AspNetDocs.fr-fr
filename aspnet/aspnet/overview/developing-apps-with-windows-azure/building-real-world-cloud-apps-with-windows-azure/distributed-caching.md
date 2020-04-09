---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Caching distribué (Building Real-World Cloud Apps avec Azure) Microsoft Docs
author: MikeWasson
description: Le Building Real World Cloud Apps avec Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent-il...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 87a7516415895e761d1589fd459b93e5c15c0f85
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676019"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Caching distribué (Building Real-World Cloud Apps avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Télécharger Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps avec Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir à développer des applications Web pour le cloud. Pour plus d’informations sur le livre électronique, voir [le premier chapitre](introduction.md).

Le chapitre précédent s’est penché sur la manipulation des défauts transitoires et a mentionné la mise en cache comme stratégie de disjoncteur. Ce chapitre donne plus d’arrière-plan sur la mise en cache, y compris quand l’utiliser, des modèles communs pour l’utiliser, et comment l’implémenter en Azure.

## <a name="what-is-distributed-caching"></a>Qu’est-ce que la mise en cache distribuée

Un cache offre un débit élevé et un accès à faible latence aux données d’application généralement accessibles, en stockant les données en mémoire. Pour une application cloud, le type de cache le plus utile est le cache distribué, ce qui signifie que les données ne sont pas stockées sur la mémoire du serveur Web individuel, mais sur d’autres ressources cloud, et les données mises en cache sont mises à la disposition de tous les serveurs Web d’une application (ou d’autres machines VM cloud qui sont utilisées par l’application).

![diagramme montrant plusieurs serveurs Web accédant aux mêmes serveurs de cache](distributed-caching/_static/image1.png)

Lorsque l’application s’balance en ajoutant ou en supprimant des serveurs, ou lorsque les serveurs sont remplacés en raison de mises à niveau ou de défauts, les données mises en cache restent accessibles à tous les serveurs qui exécutent l’application.

En évitant l’accès élevé aux données de latence d’un magasin de données persistant, la mise en cache peut améliorer considérablement la réactivité des applications. Par exemple, la récupération des données à partir du cache est beaucoup plus rapide que de les récupérer à partir d’une base de données relationnelle.

Un avantage secondaire de la mise en cache est la réduction du trafic vers le magasin de données persistant, ce qui peut entraîner une baisse des coûts lorsqu’il y a des frais d’évacuation des données pour le magasin de données persistant.

## <a name="when-to-use-distributed-caching"></a>Quand utiliser la mise en cache distribuée

Le cachage fonctionne mieux pour les charges de travail des applications qui font plus de lecture que la rédaction de données, et lorsque le modèle de données prend en charge l’organisation de clé/valeur que vous utilisez pour stocker et récupérer des données en cache. Il est également plus utile lorsque les utilisateurs d’applications partagent beaucoup de données communes; par exemple, le cache ne fournirait pas autant d’avantages si chaque utilisateur récupère généralement des données uniques à cet utilisateur. Un exemple où la mise en cache pourrait être très bénéfique est un catalogue de produits, parce que les données ne changent pas fréquemment, et tous les clients regardent les mêmes données.

L’avantage de la mise en cache devient de plus en plus mesurable plus une échelle d’application, à mesure que les limites de débit et les retards de latence du magasin de données persistants deviennent plus limités sur le rendement global de l’application. Cependant, vous pouvez implémenter la mise en cache pour d’autres raisons que les performances ainsi. Pour les données qui n’ont pas besoin d’être parfaitement à jour lorsqu’elles sont montrées à l’utilisateur, l’accès au cache peut servir de disjoncteur lorsque le magasin de données persistant est insensible ou indisponible.

## <a name="popular-cache-population-strategies"></a>Stratégies populaires de population de caches

Afin d’être en mesure de récupérer des données à partir de cache, vous devez les stocker en premier. Il existe plusieurs stratégies pour obtenir des données dont vous avez besoin dans un cache :

- À la demande / Cache Mis à part

    L’application tente de récupérer les données du cache, et lorsque le cache n’a pas les données (un "miss"), l’application stocke les données dans le cache afin qu’il soit disponible la prochaine fois. La prochaine fois que l’application tente d’obtenir les mêmes données, elle trouve ce qu’elle recherche dans le cache (un «hit»). Pour éviter d’aller chercher des données mises en cache qui ont changé dans la base de données, vous invalidez le cache lorsque vous apportez des modifications au magasin de données.
- Poussée de données de fond

    Les services d’arrière-plan poussent les données dans le cache sur un calendrier régulier, et l’application tire toujours du cache. Cette approche fonctionne très bien avec des sources de données de latence élevée qui ne vous obligent pas à toujours retourner les dernières données.
- Disjoncteur

    L’application communique normalement directement avec le magasin de données persistant, mais lorsque le magasin de données persistant a des problèmes de disponibilité, l’application récupère les données du cache. Les données peuvent avoir été mises en cache à l’aide du cache de côté ou de la stratégie de poussée des données de fond. Il s’agit d’une stratégie de gestion des défauts plutôt que d’une stratégie d’amélioration de la performance.

Afin de conserver les données dans le courant de cache, vous pouvez supprimer les entrées de cache connexes lorsque votre application crée, met à jour ou supprime des données. S’il est correct pour votre application d’obtenir parfois des données qui sont légèrement obsolètes, vous pouvez compter sur un délai d’expiration configurable pour définir une limite sur l’âge des données de cache peut être.

Vous pouvez configurer l’expiration absolue (quantité de temps depuis la création de l’élément cache) ou l’expiration de la glissade (quantité de temps depuis la dernière fois qu’un élément de cache a été consulté). L’expiration absolue est utilisée lorsque vous dépendez du mécanisme d’expiration du cache pour éviter que les données ne deviennent trop périmées. Dans l’application Fix It, nous expulserons manuellement les éléments de cache périmés et nous utiliserons l’expiration coulissante pour conserver les données les plus récentes dans le cache. Quelle que soit la stratégie d’expiration que vous choisissez, le cache expulsera automatiquement les éléments les plus anciens (moins récemment utilisés ou LRU) lorsque la limite de mémoire du cache est atteinte.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Exemple de code de cache pour l’application Fix It

Dans le code d’échantillon suivant, nous vérifions d’abord le cache lors de la récupération d’une tâche Fix It. Si la tâche se trouve dans le cache, nous la retournons; si elle n’est pas trouvée, nous l’obtenons à partir de la base de données et le stocker dans le cache. Les modifications que vous apporteriez `FindTaskByIdAsync` pour ajouter la mise en cache à la méthode sont mises en évidence.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Lorsque vous mettez à jour ou supprimez une tâche Fix It, vous devez invalider (supprimer) la tâche mise en cache. Sinon, les tentatives futures de lire cette tâche continueront d’obtenir les anciennes données du cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Il s’agit d’échantillons pour illustrer un simple code de mise en cache; mise en cache n’a pas été implémentée dans le projet Fix It téléchargeable.

## <a name="azure-caching-services"></a>Services de mise en cache Azure

Azure offre les services de mise en cache suivants : [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) et [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis cache est basé sur le populaire [open source Redis Cache](http://redis.io/) et est le premier choix pour la plupart des scénarios de mise en cache.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET’état de session à l’aide d’un fournisseur de cache

Comme mentionné dans le chapitre sur les meilleures pratiques de [développement web,](web-development-best-practices.md)une pratique exemplaire consiste à éviter d’utiliser l’état de session. Si votre application nécessite l’état de la session, la prochaine meilleure pratique est d’éviter le fournisseur de mémoire par défaut, car cela ne permet pas d’échelle (plusieurs instances du serveur web). Le ASP.NET fournisseur d’état de session SQL Server permet à un site qui s’exécute sur plusieurs serveurs Web d’utiliser l’état de session, mais il entraîne un coût de latence élevé par rapport à un fournisseur de mémoire. La meilleure solution si vous devez utiliser l’état de session est d’utiliser un fournisseur de cache, tel que le [fournisseur d’État de session pour Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Récapitulatif

Vous avez vu comment l’application Fix It pourrait implémenter la mise en cache afin d’améliorer le temps de réponse et l’évolutivité, et de permettre à l’application de continuer à être réactive pour les opérations de lecture lorsque la base de données n’est pas disponible. Dans le [prochain chapitre,](queue-centric-work-pattern.md) nous allons montrer comment améliorer encore l’évolutivité et faire l’application continuer à être sensible pour les opérations d’écriture.

## <a name="resources"></a>Ressources

Pour plus d’informations sur la mise en cache, voir les ressources suivantes.

Documentation

- [Cache Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentation officielle MSDN sur la mise en cache en Azure.
- [Modèles et pratiques Microsoft - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx). Voir les conseils de cachage et le motif Cache-Aside.
- [Failsafe: Guidance for Resilient Cloud Architectures](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc de Marc Mercuri, Ulrich Homann et Andrew Townhill. Voir la section sur Caching.
- [Meilleures pratiques pour la conception de services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Heure standard Livre blanc de Mark Simms et Michael Thomassy. Voir la section sur la mise en cache distribuée.
- [Caching distribué sur le chemin de l’évolutivité](https://msdn.microsoft.com/magazine/dd942840.aspx). Un article plus ancien (2009) msDN Magazine, mais une introduction clairement écrite à la mise en cache distribuée en général; va plus en profondeur que les sections de mise en cache des livres blancs FailSafe et Best Practices.

Videos

- [FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties d’Ulrich Homann, Marc Mercuri et Mark Simms. Présente une vue à 400 niveaux de la façon d’architecturer les applications cloud. Cette série se concentre sur la théorie et les raisons pour lesquelles; pour plus de détails, voir la série Building Big par Mark Simms. Voir la discussion de mise en cache dans l’épisode 3 à partir de 1:24:14.
- [Building Big: Leçons apprises auprès des clients Azure - Partie I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discute de la mise en cache distribuée à partir de 46h00. Semblable à la série Failsafe, mais va dans plus de détails comment à. La présentation a été donnée Octobre 31, 2012, de sorte qu’il ne couvre pas le service de mise en cache de Web Apps dans Azure App Service qui a été introduit en 2013.

Exemple de code

- [Cloud Service Fundamentals in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application qui implémente la mise en cache distribuée. Voir le blog d’accompagnement [Cloud Service Fundamentals - Caching Basics](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Suivant précédent](transient-fault-handling.md)
> [Next](queue-centric-work-pattern.md)
