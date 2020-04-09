---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Surveillance et télémétrie (Building Real-World Cloud Apps avec Azure) Microsoft Docs
author: MikeWasson
description: Le Building Real World Cloud Apps avec Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent-il...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676033"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Surveillance et télémétrie (Building Real-World Cloud Apps avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Télécharger Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger E-book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps avec Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir à développer des applications Web pour le cloud. Pour plus d’informations sur le livre électronique, voir [le premier chapitre](introduction.md).

Beaucoup de gens comptent sur les clients pour leur faire savoir quand leur application est en panne. Ce n’est pas vraiment une meilleure pratique n’importe où, et surtout pas dans le nuage. Il n’y a aucune garantie de notification rapide, et lorsque vous êtes informé, vous obtenez souvent des données minimales ou trompeuses sur ce qui s’est passé. Avec une bonne télémétrie et des systèmes d’enregistrement, vous pouvez être conscient de ce qui se passe avec votre application, et quand quelque chose ne va pas, vous découvrez tout de suite et ont des informations de dépannage utile pour travailler avec.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acheter ou louer une solution de télémétrie

> [!NOTE]
> Cet article a été écrit avant la publication [d’Application Insights.](/azure/application-insights/app-insights-overview) Application Insights est l’approche préférée des solutions de télémétrie sur Azure. Consultez [configurez les aperçus d’applications pour votre site Web ASP.NET](/azure/application-insights/app-insights-asp-net) pour plus d’informations.

Une des choses qui est grande au sujet de l’environnement nuageux, c’est qu’il est vraiment facile d’acheter ou de louer votre chemin vers la victoire. La télémétrie en est un exemple. Sans beaucoup d’efforts, vous pouvez obtenir un système de télémétrie vraiment bon en place et en cours d’exécution, très rentable. Il ya un tas de grands partenaires qui s’intègrent avec Azure, et certains d’entre eux ont des niveaux gratuits - de sorte que vous pouvez obtenir la télémétrie de base pour rien. Voici quelques-uns de ceux actuellement disponibles sur Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) comprend également des fonctionnalités de surveillance.

Nous allons rapidement marcher à travers la mise en place de New Relic pour montrer comment il peut être facile d’utiliser un système de télémétrie.

Dans le portail de gestion Azure, inscrivez-vous au service. Cliquez **sur Nouveau**, puis cliquez sur **Store**. La boîte de dialogue **Choisir un add-on** apparaît. Faites défiler vers le bas et cliquez sur **New Relic**.

![Choisissez un add-on](monitoring-and-telemetry/_static/image1.png)

Cliquez sur la flèche droite et choisissez le niveau de service que vous voulez. Pour cette démo, nous allons utiliser le niveau gratuit.

![Personnaliser l’add-on](monitoring-and-telemetry/_static/image2.png)

Cliquez sur la flèche droite, confirmez l’achat, et New Relic apparaît maintenant comme un add-on dans le portail.

![Examen de l’achat](monitoring-and-telemetry/_static/image3.png)

![Nouvelle relique add-on dans le portail de gestion](monitoring-and-telemetry/_static/image4.png)

Cliquez **sur Informations de connexion**, et copiez la clé de licence.

![Informations de connexion](monitoring-and-telemetry/_static/image5.png)

Rendez-vous à **l’onglet Configurer** pour votre application web dans le portail, **définissez** la surveillance des performances pour **ajouter,** et définissez la liste d’abandon **Choose Add-On** à **New Relic**. Ensuite, cliquez **sur Enregistrer**.

![Nouvelle relique dans l’onglet Configurer](monitoring-and-telemetry/_static/image6.png)

Dans Visual Studio, installez le nouveau paquet Relique NuGet dans votre application.

![Analyse des développeurs dans l’onglet Configurer](monitoring-and-telemetry/_static/image7.png)

Déployez l’application à Azure et commencez à l’utiliser. Créez quelques tâches Fix It pour fournir une certaine activité pour new Relic à surveiller.

Ensuite, retournez à la page **New Relic** dans l’onglet **Add-ons** du portail et cliquez sur **Gérer**. Le portail vous envoie au portail de gestion New Relic, en utilisant un seul connectement pour l’authentification afin que vous n’ayez pas à entrer vos informations d’identification à nouveau. La page Aperçu présente une variété de statistiques de performance. (Cliquez sur l’image pour voir la page d’aperçu pleine grandeur.)

[![Nouvel onglet de surveillance des reliques](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Voici quelques-unes des statistiques que vous pouvez voir:

- Temps de réponse moyen à différents moments de la journée.

    ![Temps de réponse](monitoring-and-telemetry/_static/image10.png)
- Taux de débit (en demandes par minute) à différents moments de la journée.

    ![Débit](monitoring-and-telemetry/_static/image11.png)
- Serveur CPU temps passé à traiter différentes demandes HTTP.

    ![Temps de transaction Web](monitoring-and-telemetry/_static/image12.png)
- Temps de processeur passé dans différentes parties du code d’application :

    ![Détails de trace](monitoring-and-telemetry/_static/image13.png)
- Statistiques historiques sur le rendement.

    ![Performance historique](monitoring-and-telemetry/_static/image14.png)
- Appels vers des services externes tels que le service Blob et des statistiques sur la fiabilité et la réactivité du service.

    ![Services externes](monitoring-and-telemetry/_static/image15.png)

    ![Services externes](monitoring-and-telemetry/_static/image16.png)

    ![Service externe](monitoring-and-telemetry/_static/image17.png)
- Informations sur l’endroit où dans le monde ou où dans le trafic d’applications Web aux États-Unis provenait.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Vous pouvez également configurer des rapports et des événements. Par exemple, vous pouvez dire chaque fois que vous commencez à voir des erreurs, envoyez un e-mail pour alerter le personnel de soutien sur le problème.

![Rapports](monitoring-and-telemetry/_static/image19.png)

New Relic n’est qu’un exemple d’un système de télémétrie; vous pouvez obtenir tout cela à partir d’autres services ainsi. La beauté du cloud est que sans avoir à écrire n’importe quel code, et pour des dépenses minimales ou nulles, vous pouvez soudainement obtenir beaucoup plus d’informations sur la façon dont votre application est utilisée et ce que vos clients sont réellement l’expérience.

<a id="log"></a>
## <a name="log-for-insight"></a>Connectez-vous pour un aperçu

Un forfait télémétrie est une bonne première étape, mais vous devez toujours instrumenter votre propre code. Le service de télémétrie vous indique quand il ya un problème et vous dit ce que les clients vivent, mais il ne peut pas vous donner beaucoup de perspicacité dans ce qui se passe dans votre code.

Vous ne voulez pas avoir à passer à distance dans un serveur de production pour voir ce que votre application fait. Cela pourrait être pratique lorsque vous avez un serveur, mais qu’en est-il lorsque vous avez mis à l’échelle à des centaines de serveurs et vous ne savez pas dans lesquels vous avez besoin de distance? Votre enregistrement devrait fournir suffisamment d’informations que vous n’avez jamais à distancer dans les serveurs de production pour analyser et débocher les problèmes. Vous devez enregistrer suffisamment d’informations pour isoler les problèmes uniquement à travers les journaux.

### <a name="log-in-production"></a>Log en production

Beaucoup de gens activent le traçage de la production seulement quand il ya un problème et ils veulent déboiffer. Cela peut introduire un retard important entre le moment où vous êtes au courant d’un problème et le moment où vous obtenez des informations utiles de dépannage à ce sujet. Et les informations que vous obtenez pourraient ne pas être utiles pour les erreurs intermittentes.

Ce que nous recommandons dans l’environnement cloud où le stockage est bon marché, c’est que vous laissez toujours l’enregistrement en production. De cette façon, lorsque des erreurs se produisent, vous les avez déjà enregistrées, et vous avez des données historiques qui peuvent vous aider à analyser les problèmes qui se développent au fil du temps ou se produisent régulièrement à des moments différents. Vous pouvez automatiser un processus de purge pour supprimer les vieux journaux, mais vous pourriez constater qu’il est plus coûteux de configurer un tel processus que de garder les journaux.

La dépense supplémentaire de l’exploitation forestière est triviale par rapport à la quantité de temps de dépannage et d’argent que vous pouvez économiser en ayant toutes les informations dont vous avez besoin déjà disponible lorsque quelque chose va mal. Puis quand quelqu’un vous dit qu’ils avaient eu une erreur aléatoire quelque temps autour de 8:00 la nuit dernière, mais ils ne se souviennent pas de l’erreur, vous pouvez facilement savoir quel était le problème.

Pour moins de 4 $ par mois, vous pouvez garder 50 gigaoctets de journaux sous la main, et l’impact des performances de l’exploitation forestière est trivial tant que vous gardez une chose à l’esprit - afin d’éviter les goulots d’étranglement de performance, assurez-vous que votre bibliothèque d’exploitation forestière est asynchrone.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Différencier les journaux qui informent des journaux qui nécessitent une action

Les journaux sont destinés à INFORMER (je veux que vous sachiez quelque chose) ou ACT (je veux que vous fassiez quelque chose). Veillez à écrire uniquement des journaux ACT pour les questions qui exigent véritablement qu’une personne ou un processus automatisé prenne des mesures. Trop de journaux ACT créeront du bruit, nécessitant trop de travail pour passer au crible tout cela pour trouver de véritables problèmes. Et si vos journaux ACT déclenchent automatiquement des mesures telles que l’envoi de courriels au personnel de soutien, évitez de laisser des milliers de telles actions être déclenchées par un seul problème.

Dans .NET System.Diagnostics tracing, les journaux peuvent être attribués Erreur, Avertissement, Info, et Debug / Verbose niveau. Vous pouvez différencier ACT des journaux INFORM en réservant le niveau d’erreur pour les journaux ACT et en utilisant les niveaux inférieurs pour les journaux INFORM.

![Niveaux de journalisation](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurer les niveaux d’enregistrement au moment de l’exécution

Bien qu’il soit utile d’avoir la connexion toujours en production, une autre pratique exemplaire est de mettre en œuvre un cadre d’enregistrement qui vous permet d’ajuster à l’heure d’exécution le niveau de détail que vous vous connectez, sans redéployer ou redémarrer votre application. Par exemple, lorsque vous `System.Diagnostics` utilisez l’installation de traçage, vous pouvez créer des journaux Error, Warning, Info et Debug/Verbose. Nous vous recommandons toujours de enregistrer des journaux d’erreur, d’avertissement et d’info en production, et vous voudrez être en mesure d’ajouter dynamiquement Debug/Verbose enregistrant pour le dépannage au cas par cas.

Les applications Web d’Azure App Service `System.Diagnostics` ont intégré le support d’écriture de journaux sur le système de fichiers, le stockage de table ou le stockage Blob. Vous pouvez sélectionner différents niveaux d’enregistrement pour chaque destination de stockage, et vous pouvez modifier le niveau d’enregistrement à la volée sans redémarrer votre application. Le support de stockage Blob facilite l’exécution des travaux [d’analyse HDInsight](https://docs.microsoft.com/azure/hdinsight/) sur vos journaux d’applications, car HDInsight sait comment travailler directement avec le stockage Blob.

### <a name="log-exceptions"></a>journaliser des exceptions

Ne vous contentez pas de faire *exception. ToString()* dans votre code d’enregistrement. Cela laisse de côté l’information contextuelle. Dans le cas d’erreurs SQL, il laisse de côté le numéro d’erreur SQL. Pour toutes les exceptions, inclure des informations contextuelles, l’exception elle-même, et des exceptions intérieures pour s’assurer que vous fournissez tout ce qui sera nécessaire pour le dépannage. Par exemple, les informations contextuelles peuvent inclure le nom du serveur, un identifiant de transaction et un nom d’utilisateur (mais pas le mot de passe ou des secrets!).

Si vous comptez sur chaque développeur pour faire la bonne chose à l’exception de l’enregistrement, certains d’entre eux ne seront pas. Pour vous assurer qu’il se fait de la bonne façon à chaque fois, construire la manipulation d’exception directement dans votre interface logger: passer l’objet d’exception lui-même à la classe logger et enregistrer les données d’exception correctement dans la classe logger.

### <a name="log-calls-to-services"></a>Appels journalaux vers les services

Nous vous recommandons fortement d’écrire un journal chaque fois que votre application appelle à un service, que ce soit à une base de données ou une API REST ou tout service externe. Inclure dans vos journaux non seulement une indication de succès ou d’échec, mais combien de temps chaque demande a pris. Dans l’environnement cloud, vous verrez souvent des problèmes liés aux ralentissements plutôt qu’aux pannes complètes. Quelque chose qui prend normalement 10 millisecondes pourrait soudainement commencer à prendre une seconde. Quand quelqu’un vous dit que votre application est lente, vous voulez être en mesure de regarder New Relic ou tout service de télémétrie que vous avez et valider leur expérience, et puis vous voulez être en mesure de regarder sont vos propres journaux pour plonger dans les détails de pourquoi il est lent.

### <a name="use-an-ilogger-interface"></a>Utiliser une interface ILogger

Ce que nous vous recommandons de faire lorsque vous créez une application de production est de créer une interface *ILogger* simple et coller quelques méthodes en elle. Cela rend facile de modifier l’implémentation de connexion plus tard et ne pas avoir à passer par tout votre code pour le faire. Nous pourrions utiliser `System.Diagnostics.Trace` la classe tout au long de l’application Fix It, mais au lieu de cela nous l’utilisons sous les couvertures dans une classe de journalisation qui implémente *ILogger*, et nous faisons des appels méthode *ILogger* tout au long de l’application.

De cette façon, si jamais vous voulez rendre [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) votre exploitation forestière plus riche, vous pouvez remplacer par n’importe quel mécanisme d’enregistrement que vous voulez. Par exemple, au fur et à mesure que votre application se développe, vous pouvez décider que vous souhaitez utiliser un package de journalisation plus complet comme [NLog](http://nlog-project.org/) ou [Enterprise Library Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) est un autre cadre d’exploitation forestière populaire, mais il ne fait pas l’enregistrement asynchrone.)

L’une des raisons possibles de l’utilisation d’un cadre tel que NLog est de faciliter la division de la production forestière en magasins de données distincts à volume élevé et de grande valeur. Cela vous aide à stocker efficacement de grands volumes de données INFORM que vous n’avez pas besoin d’exécuter des requêtes rapides contre, tout en maintenant un accès rapide aux données ACT.

### <a name="semantic-logging"></a>L’exploitation forestière sémantique

Pour une façon relativement nouvelle de faire l’exploitation forestière qui peut produire des informations diagnostiques plus utiles, voir [Enterprise Library Semantic Logging Application Block (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). SLAB utilise [le suivi d’événements pour Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) et le support [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) en .NET 4.5 pour vous permettre de créer des journaux plus structurés et plus requêtes. Vous définissez une méthode différente pour chaque type d’événement que vous enregistrez, ce qui vous permet de personnaliser les informations que vous écrivez. Par exemple, pour enregistrer une erreur de `LogSQLDatabaseError` base de données SQL, vous pouvez appeler une méthode. Pour ce genre d’exception, vous savez qu’un élément clé de l’information est le numéro d’erreur, de sorte que vous pouvez inclure un paramètre de numéro d’erreur dans la signature de la méthode et enregistrer le numéro d’erreur comme un champ séparé dans l’enregistrement de journal que vous écrivez. Parce que le nombre est dans un champ séparé, vous pouvez obtenir plus facilement et de façon fiable des rapports basés sur les numéros d’erreur SQL que vous pourriez si vous étiez juste concatenating le numéro d’erreur dans une chaîne de message.

## <a name="logging-in-the-fix-it-app"></a>Enregistrement dans l’application Fix It

### <a name="the-ilogger-interface"></a>L’interface ILogger

Voici l’interface *ILogger* dans l’application Fix It.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Ces méthodes vous permettent d’écrire des journaux aux quatre mêmes niveaux pris en charge par *System.Diagnostics*. Les méthodes TraceApi sont pour l’enregistrement des appels de service externe avec des informations sur la latence. Vous pouvez également ajouter un ensemble de méthodes pour le niveau Debug/Verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>La mise en œuvre logger de l’interface ILogger

La mise en œuvre de l’interface est vraiment simple. Il appelle essentiellement juste dans les méthodes *standard System.Diagnostics.* L’extrait suivant montre les trois méthodes d’information et une des autres.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Appel aux méthodes ILogger

Chaque fois que le code de l’application Fix It attrape une exception, il appelle une méthode *ILogger* pour enregistrer les détails d’exception. Et chaque fois qu’il fait un appel à la base de données, le service Blob, ou un API REST, il commence un chronomètre avant l’appel, arrête le chronomètre lorsque le service revient, et enregistre le temps écoulé avec des informations sur le succès ou l’échec.

Notez que le message journal comprend le nom de classe et le nom de la méthode. C’est une bonne pratique pour s’assurer que les messages journalaux identifient quelle partie du code d’application les a écrites.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Donc maintenant, pour chaque fois que l’application Fix It a fait un appel à SQL Database, vous pouvez voir l’appel, la méthode qui l’appelait, et exactement combien de temps il a fallu.

![Requête SQL Database dans les journaux](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si vous parcourez les journaux, vous pouvez voir que la prise d’appels de base de données de temps est variable. Ces informations pourraient être utiles: parce que l’application enregistre tout cela, vous pouvez analyser les tendances historiques dans la façon dont le service de base de données fonctionne au fil du temps. Par exemple, un service peut être rapide la plupart du temps, mais les demandes peuvent échouer ou les réponses peuvent ralentir à certains moments de la journée.

Vous pouvez faire la même chose pour le service Blob - pour chaque fois que l’application télécharge un nouveau fichier, il ya un journal, et vous pouvez voir exactement combien de temps il a fallu pour télécharger chaque fichier.

![Blob upload log](monitoring-and-telemetry/_static/image23.png)

C’est juste quelques lignes de code supplémentaires à écrire chaque fois que vous appelez un service, et maintenant chaque fois que quelqu’un dit qu’ils ont rencontré un problème, vous savez exactement ce que le problème était, si c’était une erreur, ou même si elle était juste en cours d’exécution lente. Vous pouvez identifier la source du problème sans avoir à passer à distance dans un serveur ou activer la connexion après l’erreur se produit et espérer le recréer.

## <a name="dependency-injection-in-the-fix-it-app"></a>Injection de dépendance dans l’application Fix It

Vous vous demandez peut-être comment le constructeur de référentiel dans l’exemple ci-dessus obtient la mise en œuvre de l’interface logger:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Pour transférer l’interface à l’implémentation, l’application utilise [l’injection de dépendance](http://en.wikipedia.org/wiki/Dependency_injection)(DI) avec [AutoFac](http://autofac.org/). DI vous permet d’utiliser un objet basé sur une interface dans de nombreux endroits dans votre code et n’ont qu’à spécifier en un seul endroit l’implémentation qui est utilisée lorsque l’interface est instantanée. Cela facilite la modification de la mise en œuvre : par exemple, vous pouvez remplacer le bûcheron System.Diagnostics par un enregistreur NLog. Ou pour les tests automatisés, vous voudrez peut-être remplacer une version simulée de l’enregistreur.

L’application Fix It utilise DI dans tous les dépôts et tous les contrôleurs. Les constructeurs des classes de contrôleur obtiennent une interface *ITaskRepository* de la même façon que le référentiel obtient une interface logger :

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L’application utilise la bibliothèque AutoFac DI pour fournir automatiquement des instances *TaskRepository* et *Logger* pour ces constructeurs.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Ce code dit essentiellement que partout où un constructeur a besoin d’une interface *ILogger,* passer dans un cas de la classe *Logger,* et chaque fois qu’il a besoin d’une interface *IFixItTaskRepository,* passer dans un cas de la classe *FixItTaskRepository.*

[AutoFac](http://autofac.org/) est l’un des nombreux cadres d’injection de dépendance que vous pouvez utiliser. Un autre populaire est [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), qui est recommandé et pris en charge par Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Support d’exploitation forestière intégré à Azure

Azure prend en charge les types suivants de [connexion pour les applications Web dans Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Traçage System.Diagnostics (vous pouvez allumer et éteindre et définir les niveaux à la volée sans redémarrer le site).
- Événements Windows.
- Journaux IIS (HTTP/FREB).

Azure prend en charge les types suivants [d’exploitation forestière dans les services cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Traçage System.Diagnostics.
- Compteurs de performance.
- Événements Windows.
- Journaux IIS (HTTP/FREB).
- Surveillance personnalisée de l’annuaire.

L’application Fix It utilise le traçage system.Diagnostics. Tout ce que vous devez faire pour activer System.Diagnostics connexion dans une application web est de retourner un commutateur dans le portail ou appeler l’API REST. Dans le portail, cliquez sur l’onglet **Configuration** pour votre site et faites défiler vers le bas pour voir la section **Diagnostics d’application.** Vous pouvez activer ou désactiver la connexion et sélectionner le niveau de journalisation que vous souhaitez. Vous pouvez demander à Azure d’écrire les journaux au système de fichiers ou à un compte de stockage.

![Diagnostics d’applications et diagnostics de site dans l’onglet Configure](monitoring-and-telemetry/_static/image24.png)

Après avoir permis la connexion à Azure, vous pouvez voir les journaux dans la fenêtre Visual Studio Output au fur et à mesure qu’elles sont créées.

![Menu journaux en streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu journaux en streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Vous pouvez également avoir des journaux écrits sur votre compte de stockage et les afficher avec n’importe quel outil qui peut accéder au service Azure Storage Table, tels que **Server Explorer** dans Visual Studio ou [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Logs dans Server Explorer](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Récapitulatif

Il est très simple d’implémenter un système de télémétrie hors boîte, de vous connecter aux instruments dans votre propre code et de configurer la connexion à Azure. Et lorsque vous avez des problèmes de production, la combinaison d’un système de télémétrie et de journaux personnalisés vous aidera à résoudre les problèmes rapidement avant qu’ils ne deviennent des problèmes majeurs pour vos clients.

Dans le [chapitre suivant,](transient-fault-handling.md) nous allons examiner comment gérer les erreurs transitoires afin qu’elles ne deviennent pas des problèmes de production que vous devez étudier.

## <a name="resources"></a>Ressources

Pour plus d'informations, consultez les ressources ci-dessous.

Documentation principalement sur la télémétrie :

- [Modèles et pratiques Microsoft - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx). Voir les directives d’instrumentation et de télémétrie, les conseils de comptage des services, le modèle de surveillance des points d’end de la santé et le modèle de reconfiguration du temps d’exécution.
- [Penny Pinching in the Cloud: Enabling New Relic Performance Monitoring on Azure Websites](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Meilleures pratiques pour la conception de services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc de Mark Simms et Michael Thomassy. Consultez la section Télémétrie et diagnostics.
- [Développement de nouvelle génération avec Insights d’application](https://msdn.microsoft.com/magazine/dn683794.aspx). Article du magazine MSDN.

Documentation principalement sur l’exploitation forestière :

- [Bloc d’application d’enregistrement sémantique (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie présente le cas de l’exploitation forestière sémantique avec SLAB.
- [Création de journaux structurés et significatifs avec l’exploitation forestière sémantique](https://channel9.msdn.com/Events/Build/2013/3-336). (Vidéo) Julian Dominguez présente le cas de l’exploitation sémantique avec SLAB.
- [EF6 SQL Logging - Partie 1: Simple Logging](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers montre comment enregistrer les requêtes exécutées par Entity Framework dans EF 6.
- [Liaison De résilience et d’interception de commandement avec le cadre de l’entité dans une demande de ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quatrième d’une série de tutoriels en neuf parties, montre comment utiliser la fonction d’interception de commande EF 6 pour enregistrer les commandes SQL envoyées à la base de données par Entity Framework.
- [Améliorer l’exploitation forestière à l’aide d’attributs d’info appels C 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Comment enregistrer facilement le nom de la méthode d’appel sans le coder dur dans les littérals ou en utilisant la réflexion pour l’obtenir manuellement.

Documentation principalement sur le dépannage :

- [Azure Troubleshooting &amp; Debugging blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools - Le Service De Diagnostic utilisé par l’équipe de Support Des développeurs Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Introduit et fournit un lien de téléchargement pour un outil qui peut être utilisé sur un Azure VM pour télécharger et exécuter une grande variété d’outils de diagnostic et de surveillance. Utile lorsque vous avez besoin de diagnostiquer un problème sur une VM particulière.
- [Dépannez une application web dans Azure App Service à l’aide de Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un tutoriel étape par étape pour commencer avec System.Diagnostics traçage et débugging à distance.

Vidéos :

- [FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties d’Ulrich Homann, Marc Mercuri et Mark Simms. Présente des concepts de haut niveau et des principes architecturaux d’une manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil aux clients de Microsoft (CAT) avec des clients réels. Les épisodes 4 et 9 portent sur la surveillance et la télémétrie. L’épisode 9 comprend un aperçu des services de surveillance MetricsHub, AppDynamics, New Relic et PagerDuty.
- [Building Big: Leçons apprises auprès des clients Azure - Partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parle de concevoir pour l’échec et de tout instrumenter. Semblable à la série Failsafe, mais va dans plus de détails comment à.

Exemple de code :

- [Cloud Service Fundamentals in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application créée par l’équipe de conseil client Microsoft Azure. Démontre à la fois la télémétrie et les pratiques d’exploitation forestière, comme expliqué dans les articles suivants. L’échantillon implémente l’enregistrement des applications en utilisant [NLog](http://nlog-project.org/). Pour la documentation connexe, voir la [série de quatre articles wiki TechNet sur la télémétrie et l’enregistrement](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Suivant précédent](design-to-survive-failures.md)
> [Next](transient-fault-handling.md)
