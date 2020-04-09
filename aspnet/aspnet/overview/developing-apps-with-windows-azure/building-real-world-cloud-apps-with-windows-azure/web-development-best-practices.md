---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Les meilleures pratiques de développement Web (Construire des applications Cloud Real-World avec Azure) Microsoft Docs
author: MikeWasson
description: Le Building Real World Cloud Apps avec Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent-il...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675998"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Best Practices de développement Web (Construire des applications Cloud Du monde réel avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Télécharger Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps avec Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir à développer des applications Web pour le cloud. Pour plus d’informations sur le livre électronique, voir [le premier chapitre](introduction.md).

Les trois premiers modèles portaient sur la mise en place d’un processus de développement agile; le reste sont sur l’architecture et le code. Celui-ci est une collection de meilleures pratiques de développement web:

- [Serveurs Web apatrides](#stateless) derrière un équilibreur de charge intelligent.
- [Évitez l’état de session](#sessionstate) (ou si vous ne pouvez pas l’éviter, utilisez un cache distribué plutôt qu’une base de données).
- [Utilisez un CDN](#cdn) pour mettre en cache les ressources de fichiers statiques (images, scripts).
- [Utilisez le support async de .NET 4.5](#async) pour éviter de bloquer les appels.

Ces pratiques sont valables pour tout développement web, non seulement pour les applications cloud, mais elles sont particulièrement importantes pour les applications cloud. Ils travaillent ensemble pour vous aider à faire un usage optimal de la mise à l’échelle très flexible offerte par l’environnement cloud. Si vous ne suivez pas ces pratiques, vous rencontrerez des limites lorsque vous essayez d’étendre votre application.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Niveau Web apatride derrière un équilibreur de charge intelligent

*Le niveau Web apatride* signifie que vous ne stockez aucune donnée d’application dans la mémoire ou le système de fichiers du serveur Web. Garder votre niveau Web apatride vous permet à la fois de fournir une meilleure expérience client et d’économiser de l’argent:

- Si le niveau Web est apatride et qu’il se trouve derrière un équilibreur de charge, vous pouvez rapidement répondre aux changements dans le trafic d’application en ajoutant ou en supprimant dynamiquement les serveurs. Dans l’environnement cloud où vous ne payez que pour les ressources du serveur aussi longtemps que vous les utilisez réellement, cette capacité à répondre aux changements de la demande peut se traduire par d’énormes économies.
- Un niveau web apatride est architecturalement beaucoup plus simple pour mettre à l’échelle l’application. Cela vous permet également de répondre aux besoins de mise à l’échelle plus rapidement, et de dépenser moins d’argent sur le développement et les tests dans le processus.
- Les serveurs Cloud, comme les serveurs sur site, doivent être patchés et redémarrés de temps en temps ; et si le niveau Web est apatride, le trafic réachemineur lorsqu’un serveur descend temporairement ne cause pas d’erreurs ou de comportements inattendus.

La plupart des applications du monde réel ont besoin de stocker l’état pour une session web ; le point principal ici est de ne pas le stocker sur le serveur web. Vous pouvez stocker l’état par d’autres moyens, comme sur le client dans les cookies ou hors processus côté serveur dans ASP.NET’état de session à l’aide d’un fournisseur de cache. Vous pouvez stocker des fichiers dans [le stockage Windows Azure Blob](unstructured-blob-storage.md) au lieu du système de fichiers local.

Comme exemple de la facilité avec laquelle il est facile d’étendre une application dans les sites Web de Windows Azure si votre niveau Web est apatride, consultez **l’onglet Échelle** d’un site Web Windows Azure dans le portail de gestion :

![Onglet Mettre à l’échelle](web-development-best-practices/_static/image1.png)

Si vous souhaitez ajouter des serveurs Web, vous pouvez simplement faire glisser le curseur de comptage d’instance vers la droite. Réglez-le à 5 et cliquez sur **Enregistrer,** et en quelques secondes vous avez 5 serveurs Web dans Windows Azure gérer le trafic de votre site Web.

![Cinq cas](web-development-best-practices/_static/image2.png)

Vous pouvez tout aussi bien régler le nombre d’instances jusqu’à 3 ou redessé à 1. Lorsque vous reculez, vous commencez à économiser de l’argent immédiatement parce que Windows Azure charge à la minute, pas à l’heure.

Vous pouvez également dire à Windows Azure d’augmenter ou de diminuer automatiquement le nombre de serveurs Web basés sur l’utilisation du processeur. Dans l’exemple suivant, lorsque l’utilisation du processeur passe en dessous de 60%, le nombre de serveurs Web diminuera à un minimum de 2, et si l’utilisation du processeur dépasse 80%, le nombre de serveurs Web sera augmenté jusqu’à un maximum de 4.

![Échelle par utilisation de CPU](web-development-best-practices/_static/image3.png)

Ou que faire si vous savez que votre site ne sera occupé que pendant les heures de travail? Vous pouvez dire à Windows Azure d’exécuter plusieurs serveurs pendant la journée et de diminuer à un seul serveur les soirées, les nuits et les week-ends. La série suivante de captures d’écran montre comment configurer le site Web pour exécuter un serveur en heures de congé et 4 serveurs pendant les heures de travail de 8 heures à 17 heures.

![Échelle par horaire](web-development-best-practices/_static/image4.png)

![Définir les horaires](web-development-best-practices/_static/image5.png)

![Horaire de jour](web-development-best-practices/_static/image6.png)

![Horaire de la semaine](web-development-best-practices/_static/image7.png)

![Horaire du week-end](web-development-best-practices/_static/image8.png)

Et bien sûr, tout cela peut être fait dans les scripts ainsi que dans le portail.

La possibilité de votre application à l’échelle est presque illimitée dans Windows Azure, tant que vous évitez les obstacles à l’ajout dynamique ou la suppression des VM serveur, en gardant le niveau Web apatride.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Éviter l’état de session

Dans une application cloud réelle, il n’est souvent pas pratique d’éviter de stocker une forme d’état de session utilisateur, mais certaines approches ont davantage d’incidence que d’autres sur les performances et l'extensibilité. Si vous devez stocker un état, la meilleure solution consiste à veiller à ce qu’il reste de petite taille et à le stocker dans des cookies. Si ce n’est pas faisable, la meilleure solution est d’utiliser ASP.NET’état de session avec un fournisseur de [cache distribué et en mémoire](distributed-caching.md#sessionstate). La pire solution du point de vue des performances et de l’extensibilité consiste à utiliser un fournisseur d’état de session s'appuyant sur une base de données.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Utilisez un CDN pour mettre en cache les ressources de fichiers statiques

CDN est un acronyme pour Content Delivery Network. Vous fournissez des ressources de fichiers statiques telles que des images et des fichiers de script à un fournisseur CDN, et le fournisseur cache ces fichiers dans les centres de données partout dans le monde de sorte que partout où les gens accèdent à votre application, ils obtiennent une réponse relativement rapide et une faible latence pour les actifs mis en cache. Cela accélère le temps de chargement global du site et réduit la charge sur vos serveurs Web. Les CDN sont particulièrement importants si vous atteignez un public largement réparti géographiquement.

Windows Azure dispose d’un CDN, et vous pouvez utiliser d’autres CDN dans une application qui s’exécute dans Windows Azure ou n’importe quel environnement d’hébergement Web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Utilisez le support async de .NET 4.5 pour éviter de bloquer les appels

.NET 4.5 a amélioré les langages de programmation C et VB afin de rendre beaucoup plus simple de gérer les tâches asynchrone. L’avantage d’une programmation asynchrone n’est pas seulement pour les situations de traitement parallèles telles que lorsque vous voulez lancer plusieurs appels de service Web simultanément. Il permet également à votre serveur web d’effectuer plus efficacement et plus fiable dans des conditions de charge élevée. Un serveur Web n’a qu’un nombre limité de threads disponibles, et dans des conditions de charge élevée lorsque tous les threads sont utilisés, les demandes entrantes doivent attendre jusqu’à ce que les fils sont libérés. Si votre code d’application ne gère pas les tâches comme les requêtes de base de données et les appels de service Web de manière asynchrone, de nombreux threads sont inutilement attachés pendant que le serveur attend une réponse I /O. Cela limite la quantité de trafic que le serveur peut gérer dans des conditions de charge élevée. Avec la programmation asynchrone, les threads qui attendent un service Web ou une base de données pour retourner les données sont libérés pour servir de nouvelles demandes jusqu’à ce que les données sont reçues. Dans un serveur Web occupé, des centaines ou des milliers de demandes peuvent alors être traitées rapidement, ce qui serait autrement en attente de la libération des threads.

Comme vous l’avez vu plus tôt, il est aussi facile de diminuer le nombre de serveurs Web traitant votre site Web que de les augmenter. Donc, si un serveur peut atteindre un plus grand débit, vous n’avez pas besoin d’autant d’entre eux et vous pouvez diminuer vos coûts parce que vous avez besoin de moins de serveurs pour un volume de trafic donné que vous le feriez autrement.

La prise en charge du modèle de programmation asynchrone .NET 4.5 est incluse dans ASP.NET 4,5 pour les formulaires Web, le MVC et l’API Web; dans Entity Framework 6, et dans [l’API de stockage Windows Azure](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Soutien Async en ASP.NET 4.5

Dans ASP.NET 4.5, le soutien à la programmation asynchrone a été ajouté non seulement à la langue, mais aussi aux cadres MVC, Web Forms et Web API. Par exemple, une méthode d’action ASP.NET contrôleur MVC reçoit des données d’une demande Web et transmet les données à une vue qui crée ensuite le HTML à envoyer au navigateur. Fréquemment, la méthode d’action doit obtenir des données à partir d’une base de données ou d’un service Web afin de les afficher dans une page Web ou pour enregistrer les données saisies dans une page Web. Dans ces scénarios, il est facile de rendre la méthode d’action asynchrone: au lieu de retourner un objet *ActionResult,* vous *retournez Task&lt;ActionResult&gt; * et marquer la méthode avec le mot clé *async.* À l’intérieur de la méthode, lorsqu’une ligne de code donne le coup d’envoi d’une opération qui implique le temps d’attente, vous la marquez avec le mot clé d’attente.

Voici une méthode d’action simple qui appelle une méthode de dépôt pour une requête de base de données:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Et voici la même méthode qui gère l’appel de base de données asynchrone:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Sous les couvertures, le compilateur génère le code asynchrone approprié. Lorsque l’application passe `FindTaskByIdAsync`l’appel à `FindTask` , ASP.NET fait la demande, puis dénoue le thread du travailleur et le met à disposition pour traiter une autre demande. Lorsque `FindTask` la demande est faite, un thread est redémarré pour continuer à traiter le code qui vient après cet appel. Pendant l’intervalle `FindTask` entre le moment où la demande est lancée et lorsque les données sont retournées, vous disposez d’un thread pour effectuer un travail utile qui, autrement, serait attaché en attendant la réponse.

Il ya quelques frais généraux pour le code asynchrone, mais dans des conditions de faible charge, que les frais généraux est négligeable, tandis que dans des conditions de charge élevée, vous êtes en mesure de traiter les demandes qui autrement seraient retardés en attente de threads disponibles.

Il a été possible de faire ce genre de programmation asynchrone depuis ASP.NET 1.1, mais il était difficile à écrire, sujette aux erreurs, et difficile à débocher. Maintenant que nous avons simplifié le codage pour elle dans ASP.NET 4.5, il n’y a aucune raison de ne plus le faire.

### <a name="async-support-in-entity-framework-6"></a>Soutien d’Async dans le cadre de l’entité 6

Dans le cadre du support async dans 4.5 nous avons expédié le support async pour les appels de service Web, les prises, et le système de fichiers I /O, mais le modèle le plus commun pour les applications Web est de frapper une base de données, et nos bibliothèques de données n’a pas pris en charge async. Maintenant Entity Framework 6 ajoute un support async pour l’accès aux bases de données.

Dans Entity Framework 6, toutes les méthodes qui font qu’une requête ou une commande doit être envoyée à la base de données ont des versions async. L’exemple ici montre la version async de la méthode *Trouver.*

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Et ce support async fonctionne non seulement pour les inserts, les suppressions, les mises à jour et les trouvailles simples, il fonctionne également avec des requêtes LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Il ya `Async` une version `ToList` de la méthode parce que dans ce code qui est la méthode qui provoque une requête à être envoyé à la base de données. Les `Where` `OrderByDescending` méthodes et les méthodes ne `ToListAsync` configurent la requête, que lorsque `result` la méthode exécute la requête et stocke la réponse dans la variable.

## <a name="summary"></a>Récapitulatif

Vous pouvez implémenter les meilleures pratiques de développement Web décrites ici dans n’importe quel cadre de programmation Web et n’importe quel environnement cloud, mais nous avons des outils dans ASP.NET et Windows Azure pour le rendre facile. Si vous suivez ces modèles, vous pouvez facilement décaper votre niveau Web, et vous minimiserez vos dépenses parce que chaque serveur sera en mesure de gérer plus de trafic.

Le [chapitre suivant](single-sign-on.md) examine comment le cloud permet des scénarios de connexion unique.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les ressources suivantes.

Serveurs Web apatrides :

- [Modèles et pratiques Microsoft - Autoscalage des conseils](https://msdn.microsoft.com/library/dn589774.aspx).
- [Désactiver l’Affinité d’instance d’ARR dans les sites Web de Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Blog post par Erez Benari, explique l’affinité de session dans Windows Azure Sites Web.

Cdn:

- [FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe). Série vidéo en neuf parties d’Ulrich Homann, Marc Mercuri et Mark Simms. Voir la discussion CDN dans l’épisode 3 à partir de 1:34:00.
- [Modèles et pratiques Microsoft Modèle de contenu statique modèle d’hébergement](https://msdn.microsoft.com/library/dn589776.aspx)
- [Commentaires CDN](http://www.cdnreviews.com/). Vue d’ensemble de nombreux CDN.

Programmation asynchrone:

- [Utilisation de méthodes asynchrones dans ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial par Rick Anderson.
- [Programmation asynchrone avec Async et Await (C et Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MsDN livre blanc qui explique la justification de la programmation asynchrone, comment il fonctionne dans ASP.NET 4.5, et comment écrire du code pour l’implémenter.
- [Cadre d’entité Async Requête et Save](https://msdn.microsoft.com/data/jj819165)
- [Comment construire ASP.NET applications Web à l’aide d’Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Présentation vidéo par Rowan Miller. Inclut une démonstration graphique de la façon dont la programmation asynchrone peut faciliter l’augmentation spectaculaire du débit de serveur Web dans des conditions de charge élevée.
- [FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe). Série vidéo en neuf parties d’Ulrich Homann, Marc Mercuri et Mark Simms. Pour les discussions sur l’impact de la programmation asynchrone sur l’évolutivité, voir l’épisode 4 et l’épisode 8.
- [La magie de l’utilisation des méthodes asynchrones dans ASP.NET 4,5 plus un gotcha important](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blog post par Scott Hanselman, principalement sur l’utilisation d’async dans ASP.NET applications Web Forms.

Pour d’autres pratiques exemplaires en matière de développement Web, consultez les ressources suivantes :

- [The Fix It Sample Application - Meilleures pratiques](the-fix-it-sample-application.md#bestpractices). L’annexe de ce livre électronique énumère un certain nombre de pratiques exemplaires qui ont été mises en œuvre dans l’application Fix It.
- [Liste de contrôle des développeurs Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Suivant précédent](continuous-integration-and-continuous-delivery.md)
> [Next](single-sign-on.md)
