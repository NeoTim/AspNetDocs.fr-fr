---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Annexe: The Fix It Sample Application (Building Real-World Cloud Apps with Azure) Microsoft Docs'
author: MikeWasson
description: Le Building Real World Cloud Apps avec Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent-il...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675991"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Annexe: The Fix It Sample Application (Building Real-World Cloud Apps with Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), Tom [Dykstra](https://github.com/tdykstra)

[Télécharger The Fix It Project](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> Le **Building Real World Cloud Apps avec Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir à développer des applications Web pour le cloud. Pour plus d’informations sur le livre électronique, voir [le premier chapitre](introduction.md).

Cette annexe aux applications Cloud Building Real World avec e-book Azure contient les sections suivantes qui fournissent des informations supplémentaires sur l’application d’échantillon Fix It que vous pouvez télécharger :

- [Problèmes connus](#knownissues)
- [Bonnes pratiques](#bestpractices)
- [Comment exécuter l’application à partir de Visual Studio sur votre ordinateur local](#run-in-vs)
- [Comment déployer l’application de base sur Azure App Service Web Apps en utilisant les scripts Windows PowerShell](#deploybase)
- [Dépannage des scripts Windows PowerShell](#troubleshooting)
- [Comment déployer l’application avec le traitement de file d’attente à Azure App Service Web Apps et un service Cloud Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problèmes connus

L’application Fix It a été développée à l’origine afin d’illustrer aussi simplement que possible certains des modèles présentés dans ce livre électronique. Cependant, puisque l’e-book est sur la construction d’applications du monde réel, nous avons soumis le code Fix It à un processus d’examen et de test similaire à ce que nous feriez pour les logiciels publiés. Nous avons trouvé un certain nombre de problèmes, et comme avec toute application du monde réel, certains d’entre eux que nous avons fixé et certains d’entre eux nous avons reporté à une version ultérieure.

La liste suivante comprend les questions qui devraient être abordées dans une demande de production, mais pour une raison ou une autre, nous avons décidé de ne pas aborder dans la version initiale de la demande d’échantillon Fix It.

### <a name="security"></a>Sécurité

- Assurez-vous que vous ne pouvez pas attribuer une tâche à un propriétaire inexistant.
- Assurez-vous que vous ne pouvez afficher et modifier que les tâches que vous avez créées ou qui vous sont assignées.
- Utilisez HTTPS pour les pages de connexion et les cookies d’authentification.
- Spécifier un délai pour les cookies d’authentification.

### <a name="input-validation"></a>Validation des entrées

En général, une application de production ferait plus de validation d’entrée que l’application Fix It. Par exemple, la taille de fichier d’image / image autorisée pour le téléchargement doit être limitée.

### <a name="administrator-functionality"></a>Fonctionnalité de l’administrateur

Un administrateur devrait être en mesure de modifier la propriété sur les tâches existantes. Par exemple, le créateur d’une tâche peut quitter l’entreprise, ne laissant à personne le pouvoir de maintenir la tâche à moins que l’accès administratif ne soit activé.

### <a name="queue-message-processing"></a>Traitement des messages de file d’attente

Le traitement des messages de file d’attente dans l’application Fix It a été conçu pour être simple afin d’illustrer le modèle de travail centré sur la file d’attente avec une quantité minimale de code. Ce code simple ne serait pas suffisant pour une application de production réelle.

- Le code ne garantit pas que chaque message de file d’attente sera traité tout au plus une fois. Lorsque vous obtenez un message de la file d’attente, il ya une période de délai, au cours de laquelle le message est invisible pour les autres auditeurs de file d’attente. Si le délai d’expiration expire avant que le message ne soit supprimé, le message redevient visible. Par conséquent, si une instance de rôle de travailleur passe beaucoup de temps à traiter un message, il est théoriquement possible que le même message soit traité deux fois, ce qui entraîne une tâche en double dans la base de données. Pour plus d’informations sur ce problème, voir [Utiliser Azure Storage Queues](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La logique de sondage de file d’attente pourrait être plus rentable, en passant par la récupération de messages par lots. Chaque fois que vous appelez [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), il ya un coût de transaction. Au lieu de cela, vous pouvez appeler [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (notez le pluriel 's'), qui reçoit plusieurs messages en une seule transaction. Les coûts de transaction pour les files d’attente de stockage Azure sont très faibles, de sorte que l’impact sur les coûts n’est pas substantiel dans la plupart des scénarios.
- La boucle serrée dans le code de traitement des messages de file d’attente provoque l’affinité CPU, qui n’utilise pas les VM multi-core efficacement. Une meilleure conception utiliserait le parallélisme de tâche pour exécuter plusieurs tâches async en parallèle.
- Le traitement des messages de file d’attente n’a qu’une manipulation rudimentaire des exceptions. Par exemple, le code ne traite pas les [messages empoisonnés](https://msdn.microsoft.com/library/ms789028.aspx). (Lorsque le traitement des messages provoque une exception, vous devez enregistrer l’erreur et supprimer le message, ou le rôle de travailleur va essayer de le traiter à nouveau, et la boucle se poursuivra indéfiniment.)

### <a name="sql-queries-are-unbounded"></a>Les requêtes SQL ne sont pas illimitées

Le code Fix It actuel ne limite pas le nombre de lignes que les requêtes pour les pages Index peuvent revenir. Si un grand nombre de tâches est saisie dans la base de données, la taille des listes reçues pourrait causer des problèmes de performances. La solution est de mettre en œuvre le paaging. Par exemple, voir [Le tri, le filtrage et le paging avec le cadre d’entité dans une application ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Afficher les modèles recommandés

L’application Fix It utilise la classe d’entités FixItTask pour transmettre des informations entre le contrôleur et la vue. Une meilleure pratique consiste à utiliser des modèles de vue. Le modèle de domaine (par exemple, la classe d’entité FixItTask) est conçu autour de ce qui est nécessaire pour la persistance des données, tandis qu’un modèle de vue peut être conçu pour la présentation des données. Pour plus d’informations, voir [12 ASP.NET meilleures pratiques de MVC](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob image sécurisée recommandé

L’application Fix It stocke des images téléchargées en public, ce qui signifie que toute personne qui trouve l’URL peut accéder aux images. Les images pourraient être sécurisées au lieu d’être publiques.

### <a name="no-powershell-automation-scripts-for-queues"></a>Pas de scripts d’automatisation PowerShell pour les files d’attente

Les scripts d’automatisation d’échantillon powerShell ont été écrits uniquement pour la version de base de Fix It qui s’exécute entièrement dans Azure App Service Web Apps. Nous n’avons pas fourni de scripts pour la configuration et le déploiement sur l’application Web ainsi que l’environnement Cloud Service requis pour le traitement des files d’attente.

### <a name="special-handling-for-html-codes-in-user-input"></a>Manipulation spéciale pour les codes HTML dans l’entrée de l’utilisateur

ASP.NET empêche automatiquement de nombreuses façons dont les utilisateurs malveillants peuvent tenter des attaques de script cross-site en entrant le script dans les boîtes de texte d’entrée de l’utilisateur. Et l’aide MVC `DisplayFor` utilisé pour afficher les titres de tâches et les notes automatiquement HTML-encodes valeurs qu’il envoie au navigateur. Mais dans une application de production, vous voudrez peut-être prendre des mesures supplémentaires. Pour plus d’informations, voir [Validation de la demande dans ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Meilleures pratiques

Voici quelques problèmes qui ont été résolus après avoir été découverts dans l’examen du code et les tests de la version originale de l’application Fix It. Certains ont été causés par le codeur d’origine n’étant pas au courant d’une pratique exemplaire particulière, d’autres simplement parce que le code a été écrit rapidement et n’était pas destiné à un logiciel publié. Nous énumérons les problèmes ici au cas où il ya quelque chose que nous avons appris de cet examen et de tests qui pourraient être utiles à d’autres qui sont également le développement d’applications Web.

### <a name="dispose-the-database-repository"></a>Disposer le référentiel de base de données

La `FixItTaskRepository` classe doit disposer `DbContext` de l’instance cadre d’entité. Nous l’avons `IDisposable` fait `FixItTaskRepository` en mettant en œuvre dans la classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Notez qu’AutoFac `FixItTaskRepository` se débarrasse automatiquement de l’instance, de sorte que nous n’avons pas besoin de l’éliminer explicitement.

Une autre option `DbContext` est de `FixItTaskRepository`supprimer la variable `DbContext` du membre de , et `using` à la place de créer une variable locale dans chaque méthode de dépôt, à l’intérieur d’une instruction. Par exemple :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Enregistrez les singletons en tant que tels avec DI

Étant donné qu’un `PhotoService` `Logger` seul cas de la classe et de la classe est nécessaire, ces classes doivent être [enregistrées comme des cas uniques pour l’injection de dépendance](https://code.google.com/p/autofac/wiki/InstanceScope) dans *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sécurité : Ne montrez pas les détails des erreurs aux utilisateurs

L’application d’origine Fix It n’avait pas de page d’erreur générique et il suffit de laisser toutes les exceptions bouillonner jusqu’à l’interface utilisateur, de sorte que certaines exceptions telles que les erreurs de connexion de base de données pourrait entraîner une trace de pile complète étant affiché sur le navigateur. Les informations détaillées sur les erreurs peuvent parfois faciliter les attaques d’utilisateurs malveillants. La solution consiste à enregistrer les détails d’exception et à afficher une page d’erreur à l’utilisateur qui n’inclut pas de détails d’erreur. L’application Fix It se connectait déjà, et afin `<customErrors mode=On>` d’afficher une page d’erreur, nous avons ajouté dans le fichier Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Par défaut, cela provoque *l’affichage des erreurs de Views.Shared.Error.cshtml.* Vous pouvez personnaliser *Error.cshtml* ou créer votre propre `defaultRedirect` vue de page d’erreur et ajouter un attribut. Vous pouvez également spécifier différentes pages d’erreur pour des erreurs spécifiques.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sécurité : ne permet-on qu’une tâche d’être éditée par son créateur

La page Dashboard Index n’affiche que les tâches créées par l’utilisateur connecté, mais un utilisateur malveillant peut créer une URL avec un IDENTIFIANT à la tâche d’un autre utilisateur. Nous avons ajouté du code dans *DashboardController.cs* pour retourner un 404 dans ce cas:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>N’avalez pas les exceptions

L’application Fix It originale vient de revenir nulle après avoir inscrit une exception résultant d’une requête SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Cela ferait en sorte qu’il se tournerait vers l’utilisateur comme si la requête a réussi, mais n’a tout simplement pas retourné les lignes. La solution consiste à re-jeter l’exception après la capture et l’enregistrement:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Attraper toutes les exceptions dans les rôles de travailleurs

Toute exception non gérée dans un rôle de travailleur fera recycler la VM, de sorte que vous voulez envelopper tout ce que vous faites dans un bloc try-catch et gérer toutes les exceptions.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Spécifier la longueur des propriétés de chaîne dans les classes d’entités

Afin d’afficher du code simple, la version originale de l’application Fix It n’a pas précisé les longueurs pour les champs de l’entité FixItTask, et par conséquent ils ont été définis comme varchar(max) dans la base de données. Par conséquent, l’interface utilisateur accepterait presque n’importe quelle quantité d’intrants. Spécifiant les longueurs fixe des limites qui s’appliquent à la fois à l’entrée de l’utilisateur dans la page Web et la taille de la colonne dans la base de données :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marquez les membres privés comme readonly quand ils ne sont pas censés changer

Par exemple, `DashboardController` dans la `FixItTaskRepository` classe, une instance de est créé et ne devrait pas changer, donc nous l’avons défini comme [lisible](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Utilisez la liste. N’importe quel () au lieu de la liste. Comte() &gt; 0

Si vous vous souciez tout ce qui vous intéresse est de savoir si un ou plusieurs éléments dans une liste `Count` correspondent aux critères spécifiés, utilisez la méthode [Any,](https://msdn.microsoft.com/library/bb534972.aspx) car elle revient dès qu’un élément correspondant aux critères est trouvé, alors que la méthode doit toujours itérer à travers chaque élément. Le fichier Dashboard *Index.cshtml* avait à l’origine ce code :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Nous l’avons changé pour ceci :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Générer des URL dans les vues MVC à l’aide d’aides MVC

Pour le bouton **Créer un Fix It** sur la page d’accueil, l’application Fix It a codé un élément d’ancrage :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Pour les liens View/Action comme celui-ci, il est préférable d’utiliser l’assistant HTML [Url.Action,](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) par exemple :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Utilisez Task.Delay au lieu de Thread.Sleep dans le rôle de travailleur

Le modèle de `Thread.Sleep` nouveau projet met dans le code d’échantillon pour un rôle de travailleur, mais causant le fil de dormir peut provoquer le pool de thread pour engendrer des fils inutiles supplémentaires. Vous pouvez éviter cela en utilisant [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) à la place.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Éviter le vide async

Si une méthode async n’a pas besoin `Task` de `void`retourner une valeur, retournez un type plutôt que .

Cet exemple vient `FixItQueueManager` de la classe :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Vous ne `async void` devez utiliser que pour les gestionnaires d’événements de haut niveau. Si vous définissez `async void`une méthode comme, l’appelant ne peut pas **attendre** la méthode ou attraper les exceptions que la méthode jette. Pour plus d’informations, voir [Meilleures pratiques dans la programmation asynchrone](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Utilisez un jeton d’annulation pour rompre avec la boucle de rôle des travailleurs

Typiquement, la méthode **Run** sur un rôle de travailleur contient une boucle infinie. Lorsque le rôle de travailleur s’arrête, la méthode [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) est appelée. Vous devriez utiliser cette méthode pour annuler le travail qui se fait à l’intérieur de la méthode **Run** et sortir gracieusement. Dans le cas contraire, le processus pourrait être terminé au milieu d’une opération.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Optez pour la procédure automatique de reniflement miME

Dans certains cas, Internet Explorer signale un type MIME différent du type spécifié par le serveur web. Par exemple, si Internet Explorer trouve du contenu HTML dans un fichier livré avec l’en-tête de réponse HTTP Content-Type: texte/plaine, Internet Explorer détermine que le contenu doit être rendu sous forme de HTML. Malheureusement, ce « reniflement MIME » peut également entraîner des problèmes de sécurité pour les serveurs hébergeant du contenu non fiable. Pour lutter contre ce problème, Internet Explorer 8 a apporté un certain nombre de modifications au code de détermination de type MIME et permet aux développeurs [d’applications de se retirer du MIME-sniffing](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Le code suivant a été ajouté au fichier *Web.config.*

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Permettre le regroupement et la minification

Lorsque Visual Studio crée un nouveau projet web, le regroupement et la minification des fichiers JavaScript ne sont pas activés par défaut. Nous avons ajouté une ligne de code dans BundleConfig.cs :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Définir un délai d’expiration pour les cookies d’authentification

Par défaut, les cookies d’authentification expirent dans deux semaines. Un temps plus court est plus sûr. Vous pouvez modifier ce paramètre en *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Comment exécuter l’application à partir de Visual Studio sur votre ordinateur local

Il existe deux façons d’exécuter l’application Fix It :

- Exécutez l’application de base qui écrit de nouvelles tâches directement à la base de données SQL.
- Exécutez l’application à l’aide d’une file d’attente plus un service backend pour créer des tâches. Le modèle de file d’attente est décrit dans le chapitre [Queue-Centric Work Pattern](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Exécuter l’application de base

1. Installer [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Installez [l’Azure SDK pour .NET pour Visual Studio](https://azure.microsoft.com/downloads/).
3. Téléchargez le fichier .zip de la [GALERIE de code MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. Dans File Explorer, cliquez à droite sur le fichier .zip et cliquez sur Propriétés, puis dans la fenêtre Propriétés cliquez sur Débloquez.
5. Décompressez le dossier.
6. Double-cliquez sur le fichier .sln pour lancer Visual Studio.
7. Du menu **Tools,** cliquez sur **NuGet Package Manager**, puis **Package Manager Console**.
8. Dans la console Package Manager (PMC), cliquez sur Restaurer.
9. Quittez Visual Studio.
10. Démarrer [l’émulateur de stockage Azure](/azure/storage/common/storage-use-emulator).
11. Redémarrez Visual Studio, en ouvrant le fichier de solution que vous avez fermé dans l’étape précédente.
12. Assurez-vous que le projet FixIt est défini comme le projet de démarrage, puis appuyez sur CTRL-F5 pour exécuter le projet.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Exécuter l’application avec le traitement de file d’attente

1. Suivez les instructions pour [exécuter l’application de base,](#runbase)puis fermer le navigateur et fermer Visual Studio.
2. Démarrer Visual Studio avec des privilèges d’administrateur. (Vous utiliserez l’émulateur de calcul Azure, et cela nécessite des privilèges d’administrateur.)
3. Dans le fichier *Web.config* de l’application dans le projet `appSettings/UseQueues` *MyFixIt* (le projet web), changez la valeur de "vrai":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Si [l’émulateur de stockage Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) n’est pas encore en cours d’exécution, recommencez.
5. Exécutez simultanément le projet Web FixIt et le projet MyFixItCloudService.

    Utilisation de Visual Studio:

   1. Appuyez sur **F5** pour mener à bien le projet FixIt.
   2. Dans **Solution Explorer**, cliquez à droite sur le projet MyFixItCloudService, puis cliquez sur **Debug** > Start New**Instance**.

    Utilisation de Visual Studio 2013 Express pour le Web :

   3. Dans Solution Explorer, cliquez à droite sur la solution FixIt et sélectionnez **les propriétés**.
   4. Sélectionnez **plusieurs projets de démarrage**.
   5. Dans la liste **d’abandon d’action** sous MyFixIt et MyFixItCloudService, sélectionnez **Démarrer**.
   6. Cliquez sur **OK**.
   7. Appuyez sur **F5** pour exécuter les deux projets.

      Lorsque vous exécutez le projet MyFixItCloudService, Visual Studio démarre l’émulateur de calcul Azure. Selon votre configuration de pare-feu, vous devrez peut-être autoriser l’émulateur à travers le pare-feu.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Comment déployer l’application de base sur Azure App Service Web Apps en utilisant les scripts Windows PowerShell

Pour illustrer le modèle [Automate Everything,](automate-everything.md) l’application Fix It est fournie avec des scripts qui configurent un environnement azuréen et déploient le projet dans le nouvel environnement. Les instructions suivantes expliquent comment utiliser les scripts.

Si vous voulez courir dans Azure sans utiliser de files d’attente, et que vous avez apporté les modifications pour courir localement avec des files d’attente, assurez-vous de définir la valeur UseQueues appSetting de nouveau à faux avant de procéder avec les instructions suivantes.

Ces instructions supposent que vous avez déjà téléchargé et exécuté la solution Fix It localement, et que vous avez un compte Azure ou avez un abonnement Azure que vous êtes autorisé à gérer.

1. Installez la console **Azure PowerShell.** Pour obtenir des instructions, consultez la rubrique [Installation et configuration d'Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Cette console personnalisée est configurée pour travailler avec votre abonnement Azure. Le module Azure est installé dans le répertoire *des fichiers de programme* et est automatiquement importé sur chaque utilisation de la console Azure PowerShell.

    Si vous préférez travailler dans un programme hôte différent, tel que Windows PowerShell ISE, [assurez-vous d’utiliser](https://go.microsoft.com/fwlink/?LinkID=141553) le cmdlet Import-Module pour importer le module Azure ou utiliser une commande dans le module Azure pour déclencher l’importation automatique du module.
2. Démarrez Azure PowerShell avec **l’option Run en tant qu’administrateur.**
3. Exécuter le [cmdlet Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) pour définir la politique `RemoteSigned`d’exécution Azure PowerShell à . Entrez **Y** (pour oui) pour compléter le changement de politique.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Ce paramètre vous permet d’exécuter des scripts locaux qui ne sont pas signés numériquement. (Vous pouvez également définir `Unrestricted`la politique d’exécution à , ce qui éliminerait la nécessité de l’étape de déblocage plus tard, mais ce n’est pas recommandé pour des raisons de sécurité.)
4. Exécutez `Add-AzureAccount` le cmdlet pour configurer PowerShell avec des informations d’identification pour votre compte.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Ces informations d’identification expirent après une période `Add-AzureAccount` de temps et vous devez ré-exécuter le cmdlet. Au fur et à mesure que ce livre électronique est en cours d’écriture, le délai avant l’expiration des titres de compétences est de 12 heures.
5. Si vous avez plusieurs abonnements, utilisez le cmdlet Select-AzureSubscription pour spécifier l’abonnement dans lequel vous souhaitez créer l’environnement de test.
6. Importer un certificat de gestion pour le `Get-AzurePublishSettingsFile` `Import-AzurePublishSettingsFile` même abonnement Azure en utilisant les cmdlets et les cmdlets. Le premier de ces cmdlets télécharge un fichier de certificat, et dans le second vous spécifiez l’emplacement de ce fichier afin de l’importer. > [!IMPORTANT]
   > Conservez le fichier téléchargé dans un endroit sûr ou supprimez-le lorsque vous en avez fini avec celui-ci, car il contient un certificat qui peut être utilisé pour gérer vos services Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Le certificat est utilisé pour un appel REST API qui détecte l’adresse IP de la machine de développement afin de définir une règle de pare-feu sur le serveur SQL Database.
7. Exécutez le [cmdlet Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) `chdir`(alias `sl`sont `cd`, , et ) pour naviguer vers le répertoire qui contient les scripts. (Ils sont situés dans le dossier *Automation* dans le dossier de solution Fix It.) Mettez le chemin entre guillemets si l’un des noms d’annuaire contient des espaces. Par exemple, pour `c:\Sample Apps\FixIt\Automation` naviguer vers l’annuaire, vous pouvez entrer la commande suivante :

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Pour permettre à Windows PowerShell d’exécuter ces scripts, utilisez le cmdlet [Unblock-File.](https://go.microsoft.com/fwlink/p/?linkid=294021) (Les scripts sont bloqués parce qu’ils ont été téléchargés à partir d’Internet.)

    > [!WARNING]
    > Sécurité - `Unblock-File` Avant de s’exécuter sur n’importe quel script ou fichier exécutable, ouvrez le fichier dans Notepad, examinez les commandes et vérifiez qu’elles ne contiennent aucun code malveillant.

    Par exemple, la commande `Unblock-File` suivante exécute le cmdlet sur tous les scripts dans le répertoire actuel.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Pour créer l’application web pour l’application de base (pas de traitement des files d’attente) Fix It app, exécutez le script de création de l’environnement.

    Le `Name` paramètre requis spécifie le nom de la base de données et est également utilisé pour le compte de stockage que le script crée. Le nom doit être globalement unique dans le domaine azurewebsites.net. Si vous spécifiez un nom qui n’est pas unique, comme Fixit `New-AzureWebsite` ou Test (ou même comme dans l’exemple, fixitdemo), le cmdlet échoue avec une erreur interne qui signale un conflit. Le script convertit le nom en tous les cas inférieurs pour se conformer aux exigences de nom pour les applications Web, les comptes de stockage et les bases de données.

    Le `SqlDatabasePassword` paramètre requis spécifie le mot de passe du compte d’administration qui sera créé pour SQL Database. N’incluez pas de caractères&amp; &lt; &gt; XML spéciaux dans le mot de passe (;). Il s’agit d’une limitation de la façon dont les scripts ont été écrits, pas une limitation d’Azure.

    Par exemple, si vous souhaitez créer une application web nommée "fixitdemo" et utiliser un mot de passe d’administrateur SQL Server de "Passw0rd1", vous pouvez entrer la commande suivante:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Le nom doit être unique dans le domaine azurewebsites.net, et le mot de passe doit répondre aux exigences de base de données SQL pour la complexité des mots de passe. (L’exemple Passw0rd1 répond aux exigences.)

    Notez que la commande commence par ". \". Pour aider à empêcher l’exécution malveillante de scripts, Windows PowerShell exige que vous fournissiez le chemin entièrement qualifié au fichier de script lorsque vous exécutez un script. Vous pouvez utiliser un point pour indiquer\"l’annuaire actuel (". ) ou fournir le chemin entièrement qualifié, tels que :

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Pour plus d’informations sur `Get-Help` le script, utilisez le cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Vous pouvez `Detailed`utiliser `Full` `Parameters`le `Examples` , , et les paramètres de la cmdlet Get-Help pour filtrer l’aide qui est retourné.

    Si le script échoue ou génère des erreurs, telles que "New-AzureWebsite: Call Set-AzureSubscription et Select-AzureSubscription first," vous n’avez peut-être pas terminé la configuration d’Azure PowerShell.

    Une fois le script terminé, vous pouvez utiliser le portail de gestion Azure pour voir les ressources qui ont été créées, comme le montre le chapitre [Automate Everything.](automate-everything.md)
10. Pour déployer le projet FixIt dans le nouvel environnement Azure, utilisez le script *AzureWebsite.ps1.* Par exemple :

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Lorsque le déploiement est terminé, le navigateur s’ouvre avec Fix It en cours d’exécution en Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Dépannage des scripts Windows PowerShell

Les erreurs les plus courantes rencontrées lors de l’exécution de ces scripts sont liées aux autorisations. Assurez-vous `Add-AzureAccount` `Import-AzurePublishSettingsFile` que et ont réussi et que vous les avez utilisés pour le même abonnement Azure. Même `Add-AzureAccount` si vous avez réussi, vous pourriez avoir à le courir à nouveau. Les autorisations `Add-AzureAccount` ajoutées expirent dans 12 heures.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>La référence d’objet n’a pas pour valeur une instance d’un objet.

Si le script renvoie des erreurs, telles que « référence d’objet non définie à une instance d’un objet », ce `Add-AzureAccount` qui signifie que Windows PowerShell ne peut pas trouver un objet à traiter (il s’agit d’une exception de référence nulle), exécutez le cmdlet et réessayez le script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Le serveur a rencontré une erreur interne.

Le `New-AzureWebsite` cmdlet renvoie une erreur interne lorsque le nom n’est pas unique dans le domaine azurewebsites.net. Pour résoudre l’erreur, utilisez une valeur différente pour le nom, qui est dans le paramètre Nom de *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Redémarrage du script

Si vous avez besoin de redémarrer le script *New-AzureWebsiteEnv.ps1* parce qu’il a échoué avant d’imprimer le message "Script is complete", vous pouvez supprimer les ressources que le script créé avant qu’il ne cesse. Par exemple, si le script a déjà créé l’application web ContosoFixItDemo et que vous exécutez le script à nouveau du même nom, le script échouera parce que le nom est utilisé.

Pour déterminer les ressources créées par le script avant son arrêt, utilisez les cmdlets suivants :

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Pour exécuter ce cmdlet, pipe `Get-AzureSqlDatabase`le nom du serveur de base de données à:`Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Pour supprimer ces ressources, utilisez les commandes suivantes. Notez que si vous supprimez le serveur de base de données, vous supprimez automatiquement les bases de données associées au serveur.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Comment déployer l’application avec le traitement de file d’attente à Azure App Service Web Apps et un service Cloud Azure

Pour activer les files d’attente, modifiez le fichier MyFixIt-Web.config. En `appSettings`vertu de `UseQueues` , changer la valeur de "vrai":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Ensuite, déployez l’application MVC sur une application web dans Azure App Service, comme décrit [précédemment](#deploybase).

Ensuite, créez un nouveau service cloud Azure. Les scripts inclus dans l’application Fix It ne créent pas ou ne déploient pas le service cloud, vous devez donc utiliser le portail Azure pour cela. Dans le portail, cliquez sur **New** -- **Compute** - **Cloud Service** -- Quick**Create,** puis entrez une URL et un emplacement de centre de données. Utilisez le même centre de données où vous avez déployé l’application web.

![](the-fix-it-sample-application/_static/image1.png)

Avant de pouvoir déployer le service cloud, vous devez mettre à jour certains fichiers de configuration.

Dans MyFixIt.WorkerRole-app.config, `connectionStrings`sous , remplacer `appdb` la valeur de la chaîne de connexion avec la chaîne de connexion réelle pour la base de données SQL. Vous pouvez obtenir la chaîne de connexion à partir du portail. Dans le portail, cliquez sur **SQL Databases** - **appdb** - **Voir les chaînes de connexion SQL Database pour ADO .Net, ODBC, PHP, et JDBC**. Copiez la chaîne de connexion ADO.NET et collez la valeur dans le fichier app.config. Remplacez «\_\_votre mot de passe ici » par le mot de passe de votre base de données. (En supposant que vous utumiez les scripts pour `SqlDatabasePassword` déployer l’application MVC, vous avez spécifié le mot de passe de base de données dans le paramètre de script.)

Le résultat devrait ressembler à ce qui suit :

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Dans le même fichier MyFixIt.WorkerRole-app.config, sous `appSettings`, remplacez les valeurs des deux placesholders pour le compte de stockage Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Vous pouvez obtenir la clé d’accès à partir du portail. Voir [comment gérer les comptes de stockage](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

Dans MyFixItCloudService-ServiceConfiguration.Cloud.cscfg, remplacez les mêmes valeurs de deux places pour le compte de stockage Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Maintenant, vous êtes prêt à déployer le service cloud. Dans Solution Explore, cliquez à droite sur le projet MyFixItCloudService et **sélectionnez Publier**. Pour plus d’informations, voir "[Déployer l’application à Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", qui est dans la partie 2 de [ce tutoriel](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Précédent](more-patterns-and-guidance.md)
