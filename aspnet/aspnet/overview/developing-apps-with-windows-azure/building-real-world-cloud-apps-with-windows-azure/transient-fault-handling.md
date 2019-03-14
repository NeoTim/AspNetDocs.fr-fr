---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Des erreurs temporaires gestion (création d’applications de Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 5c9a408acb074a31aeb347ad8e33ba5319530563
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029386"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="efadc-104">Erreurs temporaires gestion (création d’applications de Cloud réalistes avec Azure)</span><span class="sxs-lookup"><span data-stu-id="efadc-104">Transient Fault Handling (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="efadc-105">par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="efadc-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="efadc-106">[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="efadc-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="efadc-107">Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="efadc-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="efadc-108">Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud.</span><span class="sxs-lookup"><span data-stu-id="efadc-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="efadc-109">Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="efadc-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="efadc-110">Lorsque vous créez une application de cloud du monde réel, l’une des choses que vous devez penser consiste à gérer les interruptions de service temporaire.</span><span class="sxs-lookup"><span data-stu-id="efadc-110">When you're designing a real world cloud app, one of the things you have to think about is how to handle temporary service interruptions.</span></span> <span data-ttu-id="efadc-111">Ce problème est important identifie de façon unique dans les applications cloud, car vous êtes dépendent des connexions réseau et les services externes.</span><span class="sxs-lookup"><span data-stu-id="efadc-111">This issue is uniquely important in cloud apps because you're so dependent on network connections and external services.</span></span> <span data-ttu-id="efadc-112">Vous trouverez souvent des problèmes peu généralement auto-adaptation, et si vous n’êtes pas prêt à les gérer intelligemment, ils amèneront entraîne une mauvaise expérience pour vos clients.</span><span class="sxs-lookup"><span data-stu-id="efadc-112">You can frequently get little glitches that are typically self-healing, and if you aren't prepared to handle them intelligently, they'll result in a bad experience for your customers.</span></span>

## <a name="causes-of-transient-failures"></a><span data-ttu-id="efadc-113">Causes des défaillances temporaires</span><span class="sxs-lookup"><span data-stu-id="efadc-113">Causes of transient failures</span></span>

<span data-ttu-id="efadc-114">Vous trouverez dans l’environnement de cloud qui a échoué et les connexions se produisent régulièrement de base de données supprimée.</span><span class="sxs-lookup"><span data-stu-id="efadc-114">In the cloud environment you'll find that failed and dropped database connections happen periodically.</span></span> <span data-ttu-id="efadc-115">C’est en partie parce que vous allez via les équilibreurs de charge plus par rapport à l’environnement local dans lequel votre serveur web et le serveur de base de données ont une connexion physique directe.</span><span class="sxs-lookup"><span data-stu-id="efadc-115">That's partly because you're going through more load balancers compared to the on-premises environment where your web server and database server have a direct physical connection.</span></span> <span data-ttu-id="efadc-116">En outre, parfois, lorsque vous êtes dépendant d’un service mutualisé, vous verrez les appels au service get plus lent ou délai d’attente, car une autre personne qui utilise le service il sollicite fortement.</span><span class="sxs-lookup"><span data-stu-id="efadc-116">Also, sometimes when you're dependent on a multi-tenant service you'll see calls to the service get slower or time out because someone else who uses the service is hitting it heavily.</span></span> <span data-ttu-id="efadc-117">Dans d’autres cas, vous pouvez être l’utilisateur qui a atteint le service trop fréquemment, et le service limite délibérément vous – refuse les connexions – pour vous éviter de nuire aux autres clients du service.</span><span class="sxs-lookup"><span data-stu-id="efadc-117">In other cases you might be the user who is hitting the service too frequently, and the service deliberately throttles you – denies connections – in order to prevent you from adversely affecting other tenants of the service.</span></span>

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a><span data-ttu-id="efadc-118">Utiliser la logique de nouvelle tentative/temporisation intelligente pour atténuer l’effet des défaillances temporaires</span><span class="sxs-lookup"><span data-stu-id="efadc-118">Use smart retry/back-off logic to mitigate the effect of transient failures</span></span>

<span data-ttu-id="efadc-119">Au lieu de lever une exception et affichage d’une page non disponible ou une erreur à votre client, vous pouvez reconnaître les erreurs sont généralement transitoires, et automatiquement réessayer l’opération qui a provoqué l’erreur, qui espère avant de la durée pendant laquelle vous allez être réussie.</span><span class="sxs-lookup"><span data-stu-id="efadc-119">Instead of throwing an exception and displaying a not available or error page to your customer, you can recognize errors that are typically transient, and automatically retry the operation that resulted in the error, in hopes that before long you'll be successful.</span></span> <span data-ttu-id="efadc-120">La plupart du temps que l’opération réussira à la deuxième tentative, et vous allez récupérer à partir de l’erreur sans le client ayant été prenant en charge qu’un problème est survenu.</span><span class="sxs-lookup"><span data-stu-id="efadc-120">Most of the time the operation will succeed on the second try, and you'll recover from the error without the customer ever having been aware that there was a problem.</span></span>

<span data-ttu-id="efadc-121">Il existe plusieurs façons vous pouvez implémenter la logique de nouvelle tentative actives.</span><span class="sxs-lookup"><span data-stu-id="efadc-121">There are several ways you can implement smart retry logic.</span></span>

- <span data-ttu-id="efadc-122">Le Microsoft Patterns &amp; pratiques groupe a un [bloc applicatif de pannes gestion](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) qui fait tout pour vous si vous utilisez ADO.NET pour l’accès de base de données SQL (et non via Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="efadc-122">The Microsoft Patterns &amp; Practices group has a [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) that does everything for you if you're using ADO.NET for SQL Database access (not through Entity Framework).</span></span> <span data-ttu-id="efadc-123">Vous venez de définir une stratégie pour les nouvelles tentatives : combien de fois pour retenter une requête ou commande le délai d’attente entre les tentatives – et de type wrap votre SQL du code dans un *à l’aide de* bloc.</span><span class="sxs-lookup"><span data-stu-id="efadc-123">You just set a policy for retries – how many times to retry a query or command and how long to wait between tries – and wrap your SQL code in a *using* block.</span></span>

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    <span data-ttu-id="efadc-124">TFH prend également en charge [Azure In-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) et [Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="efadc-124">TFH also supports [Azure In-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) and [Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span>
- <span data-ttu-id="efadc-125">Lorsque vous utilisez Entity Framework vous généralement ne sont pas travailler directement avec les connexions SQL, afin que vous ne pouvez pas utiliser ce package de modèles et pratiques, mais Entity Framework 6 génère ce type de logique de nouvelle tentative dans le framework.</span><span class="sxs-lookup"><span data-stu-id="efadc-125">When you use the Entity Framework you typically aren't working directly with SQL connections, so you can't use this Patterns and Practices package, but Entity Framework 6 builds this kind of retry logic right into the framework.</span></span> <span data-ttu-id="efadc-126">De la même façon, vous spécifiez la stratégie de nouvelle tentative, et puis EF utilise cette stratégie lorsqu’il accède à la base de données.</span><span class="sxs-lookup"><span data-stu-id="efadc-126">In a similar way you specify the retry strategy, and then EF uses that strategy whenever it accesses the database.</span></span>

    <span data-ttu-id="efadc-127">Pour utiliser cette fonctionnalité dans l’application Fix It, tout ce que nous devons faire est d’ajouter une classe qui dérive de *DbConfiguration* et activer la logique de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="efadc-127">To use this feature in the Fix It app, all we have to do is add a class that derives from *DbConfiguration* and turn on the retry logic.</span></span>

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    <span data-ttu-id="efadc-128">Pour les exceptions de base de données SQL qui le framework identifie comme des erreurs temporaires en général, le code affiché indique à EF de réessayer l’opération jusqu'à 3 fois, avec un délai de temporisation exponentielle entre les nouvelles tentatives et un délai maximal de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="efadc-128">For SQL Database exceptions that the framework identifies as typically transient errors, the code shown instructs EF to retry the operation up to 3 times, with an exponential back-off delay between retries, and a maximum delay of 5 seconds.</span></span> <span data-ttu-id="efadc-129">Temporisation exponentielle signifie que, après chaque tentative ayant échoué, il attendra pendant plus de temps avant de réessayer.</span><span class="sxs-lookup"><span data-stu-id="efadc-129">Exponential back-off means that after each failed retry it will wait for a longer period of time before trying again.</span></span> <span data-ttu-id="efadc-130">Si trois tentatives dans une ligne échouent, il lève une exception.</span><span class="sxs-lookup"><span data-stu-id="efadc-130">If three tries in a row fail, it will throw an exception.</span></span> <span data-ttu-id="efadc-131">La section suivante sur les disjoncteurs explique la raison pour laquelle vous souhaitez temporisation exponentielle et un nombre limité de nouvelles tentatives.</span><span class="sxs-lookup"><span data-stu-id="efadc-131">The following section about circuit breakers explains why you want exponential back-off and a limited number of retries.</span></span>

    <span data-ttu-id="efadc-132">Vous pouvez avoir des problèmes similaires lorsque vous utilisez le service de stockage Azure, que l’application Fix It pour les objets BLOB, et l’API de client de stockage .NET implémente déjà le même genre de logique.</span><span class="sxs-lookup"><span data-stu-id="efadc-132">You can have similar issues when you're using the Azure Storage service, as the Fix It app does for Blobs, and the .NET storage client API already implements the same kind of logic.</span></span> <span data-ttu-id="efadc-133">Vous spécifiez simplement la stratégie de nouvelle tentative, ou vous n’êtes pas obligé même pas si vous êtes satisfait des paramètres par défaut.</span><span class="sxs-lookup"><span data-stu-id="efadc-133">You just specify the retry policy, or you don't even have to do that if you're happy with the default settings.</span></span>

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a><span data-ttu-id="efadc-134">Coupe-circuits</span><span class="sxs-lookup"><span data-stu-id="efadc-134">Circuit breakers</span></span>

<span data-ttu-id="efadc-135">Il existe plusieurs raisons pourquoi vous ne souhaitez pas réessayer trop de fois sur une période trop longue :</span><span class="sxs-lookup"><span data-stu-id="efadc-135">There are several reasons why you don't want to retry too many times over too long a period:</span></span>

- <span data-ttu-id="efadc-136">Nouvelle tentative de manière permanente des demandes ayant échoués trop d’utilisateurs peut dégrader l’expérience des autres utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="efadc-136">Too many users persistently retrying failed requests might degrade other users' experience.</span></span> <span data-ttu-id="efadc-137">Si des millions de personnes sont tous rendre répétées demandes de nouvelle tentative vous pourriez monopolisent les files d’attente de distribution IIS et empêcher votre application traite les demandes qu’il sinon pu gérer avec succès.</span><span class="sxs-lookup"><span data-stu-id="efadc-137">If millions of people are all making repeated retry requests you could be tying up IIS dispatch queues and preventing your app from servicing requests that it otherwise could handle successfully.</span></span>
- <span data-ttu-id="efadc-138">Si tout le monde est une nouvelle tentative en raison d’un échec du service, il peuvent être tellement demandes en file d’attente que le service obtient alors submergé quand elle commence à récupérer.</span><span class="sxs-lookup"><span data-stu-id="efadc-138">If everyone is retrying due to a service failure, there could be so many requests queued up that the service gets flooded when it starts to recover.</span></span>
- <span data-ttu-id="efadc-139">Si l’erreur est due à la limitation et qu’une fenêtre de temps que le service utilise pour la limitation, tentatives continues peuvent déplacer cette fenêtre et entraîner la limitation continuer.</span><span class="sxs-lookup"><span data-stu-id="efadc-139">If the error is due to throttling and there's a window of time the service uses for throttling, continued retries could move that window out and cause the throttling to continue.</span></span>
- <span data-ttu-id="efadc-140">Vous pouvez avoir un utilisateur en attente pour une page web à restituer.</span><span class="sxs-lookup"><span data-stu-id="efadc-140">You might have a user waiting for a web page to render.</span></span> <span data-ttu-id="efadc-141">Fabrication personnes attente trop long peut être ennuyeux de plus que relativement rapidement leur conseillant d’essayer ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="efadc-141">Making people wait too long might be more annoying that relatively quickly advising them to try again later.</span></span>

<span data-ttu-id="efadc-142">Temporisation exponentielle résout certains de ces problèmes en limitant la fréquence des tentatives de qu'un service peut obtenir à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="efadc-142">Exponential back-off addresses some of these issue by limiting the frequency of retries a service can get from your application.</span></span> <span data-ttu-id="efadc-143">Mais vous devez également disposer *disjoncteurs*: cela signifie qu’à un certain de nouvelle tentative seuil de votre application s’arrête une nouvelle tentative et accepte d’autres actions, telles qu’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="efadc-143">But you also need to have *circuit breakers*: this means that at a certain retry threshold your app stops retrying and takes some other action, such as one of the following:</span></span>

- <span data-ttu-id="efadc-144">Personnalisé de secours.</span><span class="sxs-lookup"><span data-stu-id="efadc-144">Custom fallback.</span></span> <span data-ttu-id="efadc-145">Si vous ne pouvez pas obtenir une cotation à partir de Reuters, peut-être que vous pouvez l’obtenir à partir de Bloomberg ; ou, si vous ne pouvez pas obtenir des données à partir de la base de données, peut-être que vous pouvez l’obtenir à partir du cache.</span><span class="sxs-lookup"><span data-stu-id="efadc-145">If you can't get a stock price from Reuters, maybe you can get it from Bloomberg; or if you can't get data from the database, maybe you can get it from cache.</span></span>
- <span data-ttu-id="efadc-146">Échec en mode silencieux.</span><span class="sxs-lookup"><span data-stu-id="efadc-146">Fail silent.</span></span> <span data-ttu-id="efadc-147">Si ce dont vous avez besoin à partir d’un service n’est pas tout ou rien pour votre application, simplement retourner null lorsque vous ne pouvez pas obtenir les données.</span><span class="sxs-lookup"><span data-stu-id="efadc-147">If what you need from a service isn't all-or-nothing for your app, just return null when you can't get the data.</span></span> <span data-ttu-id="efadc-148">Si vous affichez une tâche Fix It et que le service Blob ne répond pas, vous pouvez afficher les détails de la tâche sans l’image.</span><span class="sxs-lookup"><span data-stu-id="efadc-148">If you're displaying a Fix It task and the Blob service isn't responding, you could display the task details without the image.</span></span>
- <span data-ttu-id="efadc-149">Échouer rapidement.</span><span class="sxs-lookup"><span data-stu-id="efadc-149">Fail fast.</span></span> <span data-ttu-id="efadc-150">Erreur de l’utilisateur pour éviter d’inonder le service avec une nouvelle demandes susceptible de provoquer une interruption de service pour d’autres utilisateurs ou d’étendre une fenêtre de limitation.</span><span class="sxs-lookup"><span data-stu-id="efadc-150">Error out the user to avoid flooding the service with retry requests which could cause service disruption for other users or extend a throttling window.</span></span> <span data-ttu-id="efadc-151">Vous pouvez afficher un message convivial « réessayez plus tard ».</span><span class="sxs-lookup"><span data-stu-id="efadc-151">You can display a friendly "try again later" message.</span></span>

<span data-ttu-id="efadc-152">Il n’existe aucune stratégie universelle de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="efadc-152">There is no one-size-fits-all retry policy.</span></span> <span data-ttu-id="efadc-153">Vous pouvez réessayer plusieurs fois et attendre plus longtemps dans un processus de travail asynchrone en arrière-plan que vous le feriez dans une application web synchrone où un utilisateur est en attente d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="efadc-153">You can retry more times and wait longer in an asynchronous background worker process than you would in a synchronous web app where a user is waiting for a response.</span></span> <span data-ttu-id="efadc-154">Vous pouvez attendre de plus entre les nouvelles tentatives pour un service de base de données relationnelle que vous le feriez pour un service de cache.</span><span class="sxs-lookup"><span data-stu-id="efadc-154">You can wait longer between retries for a relational database service than you would for a cache service.</span></span> <span data-ttu-id="efadc-155">Voici des exemples recommandé de réessayer des stratégies à vous donner une idée de la façon dont les nombres peuvent varier.</span><span class="sxs-lookup"><span data-stu-id="efadc-155">Here are some sample recommended retry policies to give you an idea of how the numbers might vary.</span></span> <span data-ttu-id="efadc-156">(Aucun délai avant la première nouvelle tentative signifie « Première rapide ».</span><span class="sxs-lookup"><span data-stu-id="efadc-156">("Fast First" means no delay before the first retry.</span></span>

![Exemples de stratégies de nouvelle tentative](transient-fault-handling/_static/image1.png)

<span data-ttu-id="efadc-158">Pour des conseils de stratégie de nouvelle tentative de la base de données SQL, consultez [résoudre les erreurs transitoires et connexion à SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).</span><span class="sxs-lookup"><span data-stu-id="efadc-158">For SQL Database retry policy guidance, see [Troubleshoot transient faults and connection errors to SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).</span></span>

## <a name="summary"></a><span data-ttu-id="efadc-159">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="efadc-159">Summary</span></span>

<span data-ttu-id="efadc-160">Une nouvelle tentative/stratégie de temporisation peut aider à rendre des erreurs temporaires invisible au client la plupart du temps, et Microsoft fournit des infrastructures que vous pouvez utiliser pour réduire votre travail implémentation d’une stratégie si vous utilisez ADO.NET, Entity Framework ou Azure Service de stockage.</span><span class="sxs-lookup"><span data-stu-id="efadc-160">A retry/back-off strategy can help make temporary errors invisible to the customer most of the time, and Microsoft provides frameworks that you can use to minimize your work implementing a strategy whether you're using ADO.NET, Entity Framework, or the Azure Storage service.</span></span>

<span data-ttu-id="efadc-161">Dans le [chapitre suivant](distributed-caching.md), nous allons étudier comment améliorer les performances et fiabilité à l’aide de la mise en cache distribuée.</span><span class="sxs-lookup"><span data-stu-id="efadc-161">In the [next chapter](distributed-caching.md), we'll look at how to improve performance and reliability by using distributed caching.</span></span>

## <a name="resources"></a><span data-ttu-id="efadc-162">Ressources</span><span class="sxs-lookup"><span data-stu-id="efadc-162">Resources</span></span>

<span data-ttu-id="efadc-163">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="efadc-163">For more information, see the following resources:</span></span>

<span data-ttu-id="efadc-164">Documentation</span><span class="sxs-lookup"><span data-stu-id="efadc-164">Documentation</span></span>

- <span data-ttu-id="efadc-165">[Meilleures pratiques pour la création de Services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx).</span><span class="sxs-lookup"><span data-stu-id="efadc-165">[Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx).</span></span> <span data-ttu-id="efadc-166">Livre blanc par Mark Simms et Michael Thomassy.</span><span class="sxs-lookup"><span data-stu-id="efadc-166">White paper by Mark Simms and Michael Thomassy.</span></span> <span data-ttu-id="efadc-167">Semblable à la série de prévention de défaillance, mais entrera en procédures plus de détails.</span><span class="sxs-lookup"><span data-stu-id="efadc-167">Similar to the Failsafe series but goes into more how-to details.</span></span> <span data-ttu-id="efadc-168">Consultez la section données de télémétrie et Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="efadc-168">See the Telemetry and Diagnostics section.</span></span>
- <span data-ttu-id="efadc-169">[Prévention de défaillance : Conseils pour les Architectures résilientes du Cloud](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx).</span><span class="sxs-lookup"><span data-stu-id="efadc-169">[Failsafe: Guidance for Resilient Cloud Architectures](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx).</span></span> <span data-ttu-id="efadc-170">Livre blanc par Marc Mercuri, Ulrich Homann et Andrew Townhill.</span><span class="sxs-lookup"><span data-stu-id="efadc-170">White paper by Marc Mercuri, Ulrich Homann, and Andrew Townhill.</span></span> <span data-ttu-id="efadc-171">Version de la page Web de la série de vidéos de prévention de défaillance.</span><span class="sxs-lookup"><span data-stu-id="efadc-171">Web page version of the FailSafe video series.</span></span>
- <span data-ttu-id="efadc-172">[Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx).</span><span class="sxs-lookup"><span data-stu-id="efadc-172">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx).</span></span> <span data-ttu-id="efadc-173">Consultez la nouvelle tentative modèle, un modèle de superviseur de l’Agent du planificateur.</span><span class="sxs-lookup"><span data-stu-id="efadc-173">See Retry pattern, Scheduler Agent Supervisor pattern.</span></span>
- <span data-ttu-id="efadc-174">[Tolérance de panne dans la base de données SQL Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx).</span><span class="sxs-lookup"><span data-stu-id="efadc-174">[Fault-tolerance in Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx).</span></span> <span data-ttu-id="efadc-175">Billet de blog de Tony Petrossian.</span><span class="sxs-lookup"><span data-stu-id="efadc-175">Blog post by Tony Petrossian.</span></span>
- <span data-ttu-id="efadc-176">[Entity Framework - résilience des connexions / logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835).</span><span class="sxs-lookup"><span data-stu-id="efadc-176">[Entity Framework - Connection Resiliency / Retry Logic](https://msdn.microsoft.com/data/dn456835).</span></span> <span data-ttu-id="efadc-177">Comment utiliser et personnaliser la gestion des fonctionnalités d’Entity Framework 6 d’erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="efadc-177">How to use and customize the transient fault handling feature of Entity Framework 6.</span></span>
- <span data-ttu-id="efadc-178">[Résilience des connexions et Interception des commandes avec Entity Framework dans une Application ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="efadc-178">[Connection Resiliency and Command Interception with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="efadc-179">Quatrième dans une série de didacticiels neuf parties, montre comment configurer la fonctionnalité de résilience de connexion EF 6 pour SQL Database.</span><span class="sxs-lookup"><span data-stu-id="efadc-179">Fourth in a nine-part tutorial series, shows how to set up the EF 6 connection resilience feature for SQL Database.</span></span>

<span data-ttu-id="efadc-180">Vidéos</span><span class="sxs-lookup"><span data-stu-id="efadc-180">Videos</span></span>

- <span data-ttu-id="efadc-181">[Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe).</span><span class="sxs-lookup"><span data-stu-id="efadc-181">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="efadc-182">Série de neuf en parties par Ulrich Homann, Marc Mercuri et Mark Simms.</span><span class="sxs-lookup"><span data-stu-id="efadc-182">Nine-part series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="efadc-183">Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels.</span><span class="sxs-lookup"><span data-stu-id="efadc-183">Presents high-level concepts and architectural principles in a very accessible and interesting way, with stories drawn from Microsoft Customer Advisory Team (CAT) experience with actual customers.</span></span> <span data-ttu-id="efadc-184">Consultez la section traitant des disjoncteurs en épisode 3 en commençant à 40:55.</span><span class="sxs-lookup"><span data-stu-id="efadc-184">See the discussion of circuit breakers in episode 3 starting at 40:55.</span></span>
- <span data-ttu-id="efadc-185">[Construction Big : Leçons apprises de clients Azure - partie II](https://channel9.msdn.com/Events/Build/2012/3-030).</span><span class="sxs-lookup"><span data-stu-id="efadc-185">[Building Big: Lessons learned from Azure customers - Part II](https://channel9.msdn.com/Events/Build/2012/3-030).</span></span> <span data-ttu-id="efadc-186">Mark Simms parle de conception pour les défaillances, temporaire de l’erreur gestion et l’instrumentation de tous les éléments.</span><span class="sxs-lookup"><span data-stu-id="efadc-186">Mark Simms talks about designing for failure, transient fault handling, and instrumenting everything.</span></span>

<span data-ttu-id="efadc-187">Exemple de code</span><span class="sxs-lookup"><span data-stu-id="efadc-187">Code sample</span></span>

- <span data-ttu-id="efadc-188">[Cloud Service Fundamentals dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="efadc-188">[Cloud Service Fundamentals in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="efadc-189">Exemple d’application créée par le Microsoft Azure Customer Advisory Team qui montre comment utiliser le [Enterprise Library temporaires bloc gestion des erreurs](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH).</span><span class="sxs-lookup"><span data-stu-id="efadc-189">Sample application created by the Microsoft Azure Customer Advisory Team that demonstrates how to use the [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH).</span></span> <span data-ttu-id="efadc-190">Pour plus d’informations, consultez [couche Cloud Service Fundamentals Data Access – gestion des erreurs temporaires](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx).</span><span class="sxs-lookup"><span data-stu-id="efadc-190">For more information, see [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx).</span></span> <span data-ttu-id="efadc-191">TFH est recommandé pour l’accès de base de données à l’aide d’ADO.NET directement (sans utiliser Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="efadc-191">TFH is recommended for database access using ADO.NET directly (without using Entity Framework).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="efadc-192">[Précédent](monitoring-and-telemetry.md)
> [Suivant](distributed-caching.md)</span><span class="sxs-lookup"><span data-stu-id="efadc-192">[Previous](monitoring-and-telemetry.md)
[Next](distributed-caching.md)</span></span>