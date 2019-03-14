---
title: État de session et d’application dans ASP.NET Core
author: rick-anderson
description: Découvrez différentes approches pour conserver l’état de session et d’application entre les requêtes.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: a510e4f49e158203dd7c5e1e0bd28472541f7925
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024976"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="c0e2c-103">État de session et d’application dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0e2c-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="c0e2c-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c0e2c-105">HTTP est un protocole sans état.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="c0e2c-106">Sans effectuer des étapes supplémentaires, les requêtes HTTP sont des messages indépendants qui ne conservent pas les valeurs utilisateur ou l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="c0e2c-107">Cet article décrit plusieurs approches pour conserver l’état de l’application et les données utilisateur entre les requêtes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="c0e2c-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c0e2c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="c0e2c-109">Gestion de l’état</span><span class="sxs-lookup"><span data-stu-id="c0e2c-109">State management</span></span>

<span data-ttu-id="c0e2c-110">L’état peut être stocké à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-110">State can be stored using several approaches.</span></span> <span data-ttu-id="c0e2c-111">Chacune d’elles est abordée plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="c0e2c-112">Approche du stockage</span><span class="sxs-lookup"><span data-stu-id="c0e2c-112">Storage approach</span></span> | <span data-ttu-id="c0e2c-113">Mécanisme de stockage</span><span class="sxs-lookup"><span data-stu-id="c0e2c-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="c0e2c-114">Cookies</span><span class="sxs-lookup"><span data-stu-id="c0e2c-114">Cookies</span></span>](#cookies) | <span data-ttu-id="c0e2c-115">Cookies HTTP (peuvent inclure des données stockées à l’aide de code d’application côté serveur)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="c0e2c-116">État de session</span><span class="sxs-lookup"><span data-stu-id="c0e2c-116">Session state</span></span>](#session-state) | <span data-ttu-id="c0e2c-117">Cookies HTTP et code d’application côté serveur</span><span class="sxs-lookup"><span data-stu-id="c0e2c-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="c0e2c-118">TempData</span><span class="sxs-lookup"><span data-stu-id="c0e2c-118">TempData</span></span>](#tempdata) | <span data-ttu-id="c0e2c-119">Cookies HTTP ou état de session</span><span class="sxs-lookup"><span data-stu-id="c0e2c-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="c0e2c-120">Chaînes de requête</span><span class="sxs-lookup"><span data-stu-id="c0e2c-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="c0e2c-121">Chaînes de requête HTTP</span><span class="sxs-lookup"><span data-stu-id="c0e2c-121">HTTP query strings</span></span> |
| [<span data-ttu-id="c0e2c-122">Champs masqués</span><span class="sxs-lookup"><span data-stu-id="c0e2c-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="c0e2c-123">Champs de formulaires HTTP</span><span class="sxs-lookup"><span data-stu-id="c0e2c-123">HTTP form fields</span></span> |
| [<span data-ttu-id="c0e2c-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="c0e2c-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="c0e2c-125">Code d’application côté serveur</span><span class="sxs-lookup"><span data-stu-id="c0e2c-125">Server-side app code</span></span> |
| [<span data-ttu-id="c0e2c-126">Cache</span><span class="sxs-lookup"><span data-stu-id="c0e2c-126">Cache</span></span>](#cache) | <span data-ttu-id="c0e2c-127">Code d’application côté serveur</span><span class="sxs-lookup"><span data-stu-id="c0e2c-127">Server-side app code</span></span> |
| [<span data-ttu-id="c0e2c-128">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="c0e2c-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="c0e2c-129">Code d’application côté serveur</span><span class="sxs-lookup"><span data-stu-id="c0e2c-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="c0e2c-130">Cookies</span><span class="sxs-lookup"><span data-stu-id="c0e2c-130">Cookies</span></span>

<span data-ttu-id="c0e2c-131">Les cookies stockent des données entre les requêtes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-131">Cookies store data across requests.</span></span> <span data-ttu-id="c0e2c-132">Les cookies sont envoyés avec chaque requête. Il est donc essentiel de limiter leur taille au minimum.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="c0e2c-133">Dans l’idéal, un cookie doit stocker uniquement un identificateur et les données stockées par l’application.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="c0e2c-134">La plupart des navigateurs limitent la taille des cookies à 4096 octets.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="c0e2c-135">Seul un nombre limité de cookies est disponible pour chaque domaine.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="c0e2c-136">Pour éviter les risques de falsification, les cookies doivent être validés par l’application.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="c0e2c-137">Les cookies peuvent être supprimés par les utilisateurs et ils expirent sur les clients.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="c0e2c-138">Toutefois, ils constituent généralement la forme la plus durable de persistance des données sur le client.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="c0e2c-139">Les cookies servent souvent à personnaliser le contenu pour un utilisateur connu.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="c0e2c-140">L’utilisateur est uniquement identifié, et pas authentifié dans la plupart des cas.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="c0e2c-141">Le cookie peut stocker le nom de l’utilisateur, le nom du compte ou l’ID utilisateur unique (par exemple un GUID).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="c0e2c-142">Vous pouvez ensuite utiliser le cookie pour accéder aux paramètres personnalisés de l’utilisateur, telles que sa couleur d’arrière-plan de site web par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="c0e2c-143">Tenez compte du [Règlement général sur la protection des données (RGPD) de l’Union Européenne](https://ec.europa.eu/info/law/law-topic/data-protection) lors de l’émission de cookies et de la gestion de la confidentialité.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="c0e2c-144">Pour plus d’informations, consultez [Prise en charge du règlement général sur la protection des données (RGPD) de l’Union Européenne dans ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="c0e2c-145">État de session</span><span class="sxs-lookup"><span data-stu-id="c0e2c-145">Session state</span></span>

<span data-ttu-id="c0e2c-146">L’état de session est un scénario ASP.NET Core pour le stockage des données utilisateur pendant que l’utilisateur parcourt une application web.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="c0e2c-147">L’état de session utilise un magasin tenu à jour par l’application afin de conserver les données entre les requêtes d’un client.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="c0e2c-148">Les données de session sont secondées par un cache et considérées comme des données éphémères (le site doit continuer à fonctionner sans elles).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span> <span data-ttu-id="c0e2c-149">Les données d’application critiques doivent être stockées dans la base de données utilisateur et mises en cache dans la session uniquement à des fins d’optimisation des performances.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-149">Critical application data should be stored in the user database and cached in session only as a performance optimization.</span></span>

> [!NOTE]
> <span data-ttu-id="c0e2c-150">La session n’est pas prise en charge dans les applications [SignalR](xref:signalr/index) car un [hub SignalR](xref:signalr/hubs) peut s’exécuter indépendamment d’un contexte HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-150">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="c0e2c-151">Par exemple, cela peut se produire quand une longue requête d’interrogation est ouverte par un hub au-delà de la durée de vie du contexte de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-151">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="c0e2c-152">ASP.NET Core tient à jour l’état de session en fournissant au client un cookie qui contient un ID de session, qui est envoyé à l’application lors de chaque requête.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-152">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="c0e2c-153">Le serveur utilise l’ID de session pour récupérer les données de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-153">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="c0e2c-154">L’état de session présente les comportements suivants :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-154">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="c0e2c-155">Le cookie de session étant propre au navigateur, les sessions ne sont pas partagées par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-155">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="c0e2c-156">Les cookies de session sont supprimés uniquement quand la session se termine.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-156">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="c0e2c-157">Si un cookie est reçu pour une session qui a expiré, une session qui utilise le même cookie de session est créée.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-157">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="c0e2c-158">Les sessions vides ne sont pas conservées. La session doit avoir au moins une valeur définie pour pouvoir être conservée entre les requêtes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-158">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="c0e2c-159">Quand une session n’est pas conservée, un nouvel ID de session est généré pour chaque nouvelle requête.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-159">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="c0e2c-160">L’application conserve une session pendant une période limitée après la dernière requête.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-160">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="c0e2c-161">L’application définit le délai d’expiration d’une session ou utilise la valeur par défaut de 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-161">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="c0e2c-162">L’état de session est idéal pour le stockage des données utilisateur qui sont propres à une session particulière, mais où les données ne nécessitent pas un stockage permanent entre les sessions.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-162">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="c0e2c-163">Les données de session sont supprimées quand l’implémentation [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) est appelée ou quand la session expire.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-163">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="c0e2c-164">Il n’existe aucun mécanisme par défaut permettant de signaler au code d’application qu’un navigateur client a été fermé ou que le cookie de session a été supprimé ou a expiré sur le client.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-164">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>
<span data-ttu-id="c0e2c-165">Les modèles de pages ASP.NET Core MVC et Razor incluent la prise en charge du [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-165">The ASP.NET Core MVC and Razor pages templates include support for [General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span> <span data-ttu-id="c0e2c-166">[Les cookies d’état de session ne sont pas essentiels](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), l’état de session ne fonctionne pas lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-166">[Session state cookies aren't essential](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), session state isn't functional when tracking is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="c0e2c-167">Ne stockez pas de données sensibles dans l’état de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-167">Don't store sensitive data in session state.</span></span> <span data-ttu-id="c0e2c-168">L’utilisateur risque de ne pas fermer le navigateur et de ne pas effacer le cookie de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-168">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="c0e2c-169">Certains navigateurs conservent des cookies de session valides entre les fenêtres du navigateur.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-169">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="c0e2c-170">Une session n’est pas toujours limitée à un seul utilisateur ; l’utilisateur suivant risque donc de parcourir l’application avec le même cookie de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-170">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="c0e2c-171">Le fournisseur de cache en mémoire stocke les données de session dans la mémoire du serveur où réside l’application.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-171">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="c0e2c-172">Dans un scénario de batterie de serveurs :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-172">In a server farm scenario:</span></span>

* <span data-ttu-id="c0e2c-173">Utilisez des *sessions persistantes* pour lier chaque session à une instance d’application spécifique sur un serveur donné.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-173">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="c0e2c-174">[Azure App Service](https://azure.microsoft.com/services/app-service/) utilise [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) pour appliquer les sessions persistantes par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-174">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="c0e2c-175">Toutefois, les sessions rémanentes peuvent impacter l’extensibilité et compliquer les mises à jour des applications web.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-175">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="c0e2c-176">Une meilleure approche consiste à utiliser le cache distribué Redis ou SQL Server, qui ne nécessite pas de sessions persistantes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-176">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="c0e2c-177">Pour plus d'informations, consultez <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-177">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="c0e2c-178">Le cookie de session est chiffré par le biais d’[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-178">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="c0e2c-179">La protection des données doit être configurée correctement pour lire les cookies de session sur chaque ordinateur.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-179">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="c0e2c-180">Pour plus d’informations, consultez <xref:security/data-protection/introduction> et [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-180">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="c0e2c-181">Configurer l’état de session</span><span class="sxs-lookup"><span data-stu-id="c0e2c-181">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c0e2c-182">Le package [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), qui est inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), fournit un middleware (intergiciel) pour gérer l’état de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-182">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="c0e2c-183">Pour activer le middleware Session, `Startup` doit contenir les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-183">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c0e2c-184">Le package [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) fournit un middleware pour gérer l’état de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-184">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="c0e2c-185">Pour activer le middleware Session, `Startup` doit contenir les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-185">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="c0e2c-186">Un des caches mémoire [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-186">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="c0e2c-187">L’implémentation `IDistributedCache` est utilisée comme magasin de stockage pour la session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-187">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="c0e2c-188">Pour plus d'informations, consultez <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-188">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="c0e2c-189">Un appel à [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-189">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="c0e2c-190">Un appel à [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) dans `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-190">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="c0e2c-191">Le code suivant montre comment configurer le fournisseur de session en mémoire avec une implémentation en mémoire par défaut de `IDistributedCache` :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-191">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="c0e2c-192">L’ordre des middlewares est important.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-192">The order of middleware is important.</span></span> <span data-ttu-id="c0e2c-193">Dans l’exemple précédent, une exception `InvalidOperationException` se produit quand `UseSession` est appelée après `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-193">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="c0e2c-194">Pour plus d’informations, consultez [Ordre des intergiciels (middleware)](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-194">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="c0e2c-195">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) est disponible une fois l’état de session configuré.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-195">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="c0e2c-196">`HttpContext.Session` n’est pas accessible avant que `UseSession` ait été appelée.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-196">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="c0e2c-197">Il est impossible de créer une nouvelle session avec un nouveau cookie de session une fois que l’application a commencé à écrire dans le flux de réponse.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-197">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="c0e2c-198">L’exception est enregistrée dans le journal de serveur web et n’est pas affichée pas dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-198">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="c0e2c-199">Charger l’état de session de façon asynchrone</span><span class="sxs-lookup"><span data-stu-id="c0e2c-199">Load session state asynchronously</span></span>

<span data-ttu-id="c0e2c-200">Le fournisseur de session par défaut dans ASP.NET Core charge les enregistrements de session de façon asynchrone à partir du magasin de stockage sous-jacent [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) uniquement si la méthode [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) est appelée explicitement avant les méthodes [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) ou [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-200">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="c0e2c-201">Si `LoadAsync` n’est pas appelée en premier, l’enregistrement de session sous-jacent est chargé de façon synchrone, ce qui peut entraîner une baisse des performances à l’échelle.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-201">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="c0e2c-202">Pour forcer les applications à appliquer ce modèle, incluez les implémentations [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) et [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) dans un wrapper, avec des versions qui lèvent une exception si la méthode `LoadAsync` n’est pas appelée avant `TryGetValue`, `Set` ou `Remove`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-202">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="c0e2c-203">Inscrivez ensuite les versions incluses dans le wrapper auprès du conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-203">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="c0e2c-204">Options de session</span><span class="sxs-lookup"><span data-stu-id="c0e2c-204">Session options</span></span>

<span data-ttu-id="c0e2c-205">Pour remplacer les valeurs par défaut de la session, utilisez [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-205">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="c0e2c-206">Option</span><span class="sxs-lookup"><span data-stu-id="c0e2c-206">Option</span></span> | <span data-ttu-id="c0e2c-207">Description</span><span class="sxs-lookup"><span data-stu-id="c0e2c-207">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="c0e2c-208">Cookie</span><span class="sxs-lookup"><span data-stu-id="c0e2c-208">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="c0e2c-209">Détermine les paramètres utilisés pour créer le cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-209">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="c0e2c-210">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) prend la valeur [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`) par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-210">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="c0e2c-211">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) prend la valeur [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`) par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-211">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="c0e2c-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) prend la valeur [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`) par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="c0e2c-213">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) prend la valeur `true` par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-213">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="c0e2c-214">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) prend la valeur `false` par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-214">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="c0e2c-215">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="c0e2c-215">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="c0e2c-216">`IdleTimeout` indique la durée pendant laquelle la session peut être inactive avant que son contenu soit abandonné.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-216">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="c0e2c-217">Chaque accès à la session réinitialise le délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-217">Each session access resets the timeout.</span></span> <span data-ttu-id="c0e2c-218">Ce paramètre s’applique uniquement au contenu de la session, et non au cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-218">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="c0e2c-219">La valeur par défaut est 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-219">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="c0e2c-220">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="c0e2c-220">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="c0e2c-221">Durée maximale autorisée pour charger une session à partir du magasin ou pour la valider dans le magasin.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-221">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="c0e2c-222">Ce paramètre peut s’appliquer uniquement aux opérations asynchrones.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-222">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="c0e2c-223">Vous pouvez désactiver ce délai d’expiration à l’aide de [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-223">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="c0e2c-224">La valeur par défaut est 1 minute.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-224">The default is 1 minute.</span></span> |

<span data-ttu-id="c0e2c-225">Session utilise un cookie pour suivre et identifier les requêtes reçues d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-225">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="c0e2c-226">Par défaut, ce cookie se nomme `.AspNetCore.Session` et utilise le chemin `/`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-226">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="c0e2c-227">Étant donné que le cookie par défaut ne spécifie pas de domaine, il n’est pas mis à disposition du script côté client dans la page (car [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) a la valeur `true` par défaut).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-227">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="c0e2c-228">Option</span><span class="sxs-lookup"><span data-stu-id="c0e2c-228">Option</span></span> | <span data-ttu-id="c0e2c-229">Description</span><span class="sxs-lookup"><span data-stu-id="c0e2c-229">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="c0e2c-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="c0e2c-230">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="c0e2c-231">Détermine le domaine utilisé pour créer le cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-231">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="c0e2c-232">`CookieDomain` n’est pas défini par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-232">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="c0e2c-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="c0e2c-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="c0e2c-234">Détermine si le navigateur doit autoriser le code JavaScript côté client à accéder au cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-234">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="c0e2c-235">La valeur par défaut est `true`, ce qui signifie que le cookie est transmis uniquement aux requêtes HTTP et n’est pas accessible au script dans la page.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-235">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="c0e2c-236">CookieName</span><span class="sxs-lookup"><span data-stu-id="c0e2c-236">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="c0e2c-237">Détermine le nom du cookie utilisé pour conserver l’ID de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-237">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="c0e2c-238">La valeur par défaut est [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-238">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="c0e2c-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="c0e2c-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="c0e2c-240">Détermine le chemin utilisé pour créer le cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-240">Determines the path used to create the cookie.</span></span> <span data-ttu-id="c0e2c-241">La valeur par défaut est [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-241">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="c0e2c-242">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="c0e2c-242">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="c0e2c-243">Détermine si le cookie doit être transmis uniquement lors des requêtes HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-243">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="c0e2c-244">La valeur par défaut est [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-244">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="c0e2c-245">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="c0e2c-245">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="c0e2c-246">`IdleTimeout` indique la durée pendant laquelle la session peut être inactive avant que son contenu soit abandonné.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-246">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="c0e2c-247">Chaque accès à la session réinitialise le délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-247">Each session access resets the timeout.</span></span> <span data-ttu-id="c0e2c-248">Notez que cela s’applique uniquement au contenu de la session, et non au cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-248">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="c0e2c-249">La valeur par défaut est 20 minutes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-249">The default is 20 minutes.</span></span> |

<span data-ttu-id="c0e2c-250">Session utilise un cookie pour suivre et identifier les requêtes reçues d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-250">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="c0e2c-251">Par défaut, ce cookie se nomme `.AspNet.Session` et utilise le chemin `/`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-251">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="c0e2c-252">Pour substituer les valeurs de session de cookie par défaut, utilisez `SessionOptions` :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-252">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="c0e2c-253">L’application utilise la propriété [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) pour déterminer la durée pendant laquelle une session peut rester inactive avant que son contenu dans le cache du serveur soit abandonné.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-253">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="c0e2c-254">Cette propriété est indépendante du délai d’expiration du cookie.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-254">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="c0e2c-255">Chaque requête qui passe par le [middleware Session](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) réinitialise le délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-255">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="c0e2c-256">L’état de session est *sans verrouillage*.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-256">Session state is *non-locking*.</span></span> <span data-ttu-id="c0e2c-257">Si deux requêtes tentent simultanément de modifier le contenu d’une session, la dernière requête remplace la première.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-257">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="c0e2c-258">`Session` est implémentée comme une *session cohérente*, ce qui signifie que tout le contenu est stocké au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-258">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="c0e2c-259">Quand deux requêtes tentent de modifier différentes valeurs de session, la dernière requête peut remplacer les modifications de session effectuées par la première.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-259">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="c0e2c-260">Définir et obtenir des valeurs Session</span><span class="sxs-lookup"><span data-stu-id="c0e2c-260">Set and get Session values</span></span>

<span data-ttu-id="c0e2c-261">L’état de session est accessible à partir d’une classe Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) ou d’une classe MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) avec [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-261">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="c0e2c-262">Cette propriété est une implémentation [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-262">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c0e2c-263">L’implémentation de `ISession` fournit plusieurs méthodes d’extension pour définir et récupérer des valeurs de chaîne et d’entier.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-263">The `ISession` implementation provides several extension methods to set and retrieve integer and string values.</span></span> <span data-ttu-id="c0e2c-264">Les méthodes d’extension se trouvent dans l’espace de noms [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (ajoutez une instruction `using Microsoft.AspNetCore.Http;` pour accéder aux méthodes d’extension) quand le package [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) est référencé par le projet.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="c0e2c-265">Les deux packages sont inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-265">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c0e2c-266">L’implémentation `ISession` fournit plusieurs méthodes d’extension pour définir et récupérer des valeurs de chaîne et des entiers.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-266">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="c0e2c-267">Les méthodes d’extension se trouvent dans l’espace de noms [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (ajoutez une instruction `using Microsoft.AspNetCore.Http;` pour accéder aux méthodes d’extension) quand le package [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) est référencé par le projet.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-267">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="c0e2c-268">Méthodes d’extension `ISession` :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-268">`ISession` extension methods:</span></span>

* [<span data-ttu-id="c0e2c-269">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-269">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="c0e2c-270">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-270">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="c0e2c-271">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-271">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="c0e2c-272">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-272">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="c0e2c-273">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="c0e2c-273">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="c0e2c-274">L’exemple suivant récupère la valeur de session pour la clé `IndexModel.SessionKeyName` (`_Name` dans l’exemple d’application) dans une page Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-274">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="c0e2c-275">L’exemple suivant montre comment définir et obtenir un entier et une chaîne :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-275">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="c0e2c-276">Toutes les données de session doivent être sérialisées pour activer un scénario de cache distribué, même si vous utilisez le cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-276">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="c0e2c-277">Des sérialiseurs de nombres et de chaînes minimaux sont fournis (voir les méthodes et les méthodes d’extension de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-277">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="c0e2c-278">Les types complexes doivent être sérialisés par l’utilisateur à l’aide d’un autre mécanisme, tel que JSON.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-278">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="c0e2c-279">Ajoutez les méthodes d’extension suivantes pour définir et obtenir des objets sérialisables :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-279">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c0e2c-280">L’exemple suivant montre comment définir et obtenir un objet sérialisable avec les méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-280">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="c0e2c-281">TempData</span><span class="sxs-lookup"><span data-stu-id="c0e2c-281">TempData</span></span>

<span data-ttu-id="c0e2c-282">ASP.NET Core expose la propriété [TempData d’un modèle de page Razor Pages](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) ou [TempData d’un contrôleur MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-282">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="c0e2c-283">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-283">This property stores data until it's read.</span></span> <span data-ttu-id="c0e2c-284">Vous pouvez utiliser les méthodes [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) et [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-284">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="c0e2c-285">TempData est particulièrement utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-285">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="c0e2c-286">TempData est implémenté par des fournisseurs TempData à l’aide de cookies ou de l’état de session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-286">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="c0e2c-287">Fournisseurs TempData</span><span class="sxs-lookup"><span data-stu-id="c0e2c-287">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c0e2c-288">Dans ASP.NET Core 2.0 ou ultérieur, le fournisseur TempData basé sur les cookies est utilisé par défaut pour stocker TempData dans des cookies.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-288">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="c0e2c-289">Les données de cookie sont chiffrées avec [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encodées avec [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), puis mémorisées en bloc.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-289">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="c0e2c-290">Étant donné que le cookie est mémorisé en bloc, la limite de taille d’un cookie définie dans ASP.NET Core 1.x ne s’applique pas.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-290">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="c0e2c-291">Les données de cookie ne sont pas compressées, car la compression de données chiffrées peut entraîner des problèmes de sécurité, notamment des attaques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-291">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="c0e2c-292">Pour plus d’informations sur le fournisseur TempData basé sur les cookies, consultez [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-292">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c0e2c-293">Dans ASP.NET Core 1.0 et 1.1, le fournisseur TempData d’état de session est le fournisseur par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-293">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="c0e2c-294">Choisir un fournisseur TempData</span><span class="sxs-lookup"><span data-stu-id="c0e2c-294">Choose a TempData provider</span></span>

<span data-ttu-id="c0e2c-295">Pour choisir un fournisseur TempData, vous devez tenir compte de plusieurs points. Par exemple :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-295">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="c0e2c-296">L’application utilise-t-elle déjà l’état de session ?</span><span class="sxs-lookup"><span data-stu-id="c0e2c-296">Does the app already use session state?</span></span> <span data-ttu-id="c0e2c-297">Si c’est le cas, l’utilisation du fournisseur TempData d’état de session n’entraîne pas de surcoût pour l’application (hormis la taille des données).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-297">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="c0e2c-298">L’application utilise-t-elle TempData seulement de façon ponctuelle, pour des quantités de données relativement petites (jusqu’à 500 octets) ?</span><span class="sxs-lookup"><span data-stu-id="c0e2c-298">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="c0e2c-299">Si c’est le cas, le fournisseur TempData de cookies ajoute un petit coût à chaque requête traitée par TempData.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-299">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="c0e2c-300">Si ce n’est pas le cas, le fournisseur TempData d’état de session peut être utile pour éviter l’aller-retour d’une grande quantité de données dans chaque requête jusqu’à ce que le TempData soit consommé.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-300">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="c0e2c-301">L’application s’exécute-t-elle dans une batterie de serveurs sur plusieurs serveurs ?</span><span class="sxs-lookup"><span data-stu-id="c0e2c-301">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="c0e2c-302">Si c’est le cas, aucune configuration supplémentaire n’est nécessaire pour utiliser le fournisseur TempData de cookies en dehors de la Protection des données (consultez <xref:security/data-protection/introduction> et [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-302">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="c0e2c-303">La plupart des clients web (par exemple, les navigateurs web) appliquent des limites pour la taille maximale de chaque cookie et/ou le nombre total de cookies.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-303">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="c0e2c-304">Quand vous utilisez le fournisseur TempData de cookies, vérifiez que l’application ne dépasse pas ces limites.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-304">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="c0e2c-305">Prenez en compte la taille totale des données.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-305">Consider the total size of the data.</span></span> <span data-ttu-id="c0e2c-306">Pensez aux augmentations de taille de cookie dues au chiffrement et à la segmentation.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-306">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="c0e2c-307">Configuration du fournisseur TempData</span><span class="sxs-lookup"><span data-stu-id="c0e2c-307">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c0e2c-308">Le fournisseur TempData basé sur les cookies est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-308">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="c0e2c-309">Pour activer le fournisseur TempData basé sur la session, utilisez la méthode d’extension [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-309">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c0e2c-310">Le code de classe `Startup` suivant configure le fournisseur TempData basé sur les sessions :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-310">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="c0e2c-311">L’ordre des middlewares est important.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-311">The order of middleware is important.</span></span> <span data-ttu-id="c0e2c-312">Dans l’exemple précédent, une exception `InvalidOperationException` se produit quand `UseSession` est appelée après `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-312">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="c0e2c-313">Pour plus d’informations, consultez [Ordre des intergiciels (middleware)](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-313">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0e2c-314">Si vous ciblez le .NET Framework et que vous utilisez le fournisseur TempData basé sur la session, ajoutez le package [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) au projet.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-314">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="c0e2c-315">Chaînes de requête</span><span class="sxs-lookup"><span data-stu-id="c0e2c-315">Query strings</span></span>

<span data-ttu-id="c0e2c-316">Vous pouvez passer une quantité limitée de données d’une requête à une autre en l’ajoutant à la chaîne de la nouvelle requête.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-316">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="c0e2c-317">Cela est utile pour capturer l’état de manière persistante et permettre ainsi le partage de liens avec état incorporé par e-mail ou sur les réseaux sociaux.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-317">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="c0e2c-318">Les chaînes de requête URL étant publiques, vous ne devez jamais utiliser de chaînes de requête pour des données sensibles.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-318">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="c0e2c-319">En plus du partage accidentel, l’ajout de données dans des chaînes de requête peut favoriser les attaques par [falsification de requête intersites (CSRF, Cross Site Request Forgery)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) et conduire des utilisateurs authentifiés à visiter des sites malveillants.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-319">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="c0e2c-320">Les attaquants peuvent ensuite dérober des données utilisateur à partir de l’application ou effectuer des actions malveillantes en utilisant un compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-320">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="c0e2c-321">Chaque état de session ou d’application conservé doit garantir une protection contre les attaques CSRF.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-321">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="c0e2c-322">Pour plus d’informations, consultez [Empêcher les attaques par falsification de requête intersites (XSRF/CSRF)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="c0e2c-322">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="c0e2c-323">Champs masqués</span><span class="sxs-lookup"><span data-stu-id="c0e2c-323">Hidden fields</span></span>

<span data-ttu-id="c0e2c-324">Les données peuvent être enregistrées dans des champs de formulaire masqués et être republiées lors de la requête suivante.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-324">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="c0e2c-325">Cela est courant dans les formulaires de plusieurs pages.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-325">This is common in multi-page forms.</span></span> <span data-ttu-id="c0e2c-326">Étant donné que le client peut potentiellement falsifier les données, l’application doit toujours revalider les données stockées dans les champs masqués.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-326">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="c0e2c-327">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="c0e2c-327">HttpContext.Items</span></span>

<span data-ttu-id="c0e2c-328">La collection [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) est utilisée pour stocker les données lors du traitement d’une seule requête.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-328">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="c0e2c-329">Le contenu de la collection est supprimé une fois la requête traitée.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-329">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="c0e2c-330">La collection `Items` est souvent utilisée pour permettre à des composants ou des middlewares de communiquer quand ils opèrent à différents moments de l’exécution d’une requête et qu’ils ne peuvent pas passer les paramètres de façon directe.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-330">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="c0e2c-331">Dans l’exemple suivant, le [middleware](xref:fundamentals/middleware/index) ajoute `isVerified` à la collection `Items`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-331">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="c0e2c-332">Plus tard dans le pipeline, un autre middleware peut accéder à la valeur de `isVerified` :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-332">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="c0e2c-333">Si un middleware est destiné uniquement à l’usage d’une seule application, les clés `string` sont acceptables.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-333">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="c0e2c-334">Le middleware partagé entre des instances de l’application doit utiliser des clés d’objets uniques afin d’éviter les conflits de clé.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-334">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="c0e2c-335">L’exemple suivant montre comment utiliser une clé d’objet unique définie dans une classe de middleware :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-335">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="c0e2c-336">Tout autre code peut accéder à la valeur stockée dans `HttpContext.Items` à l’aide de la clé exposée par la classe du middleware :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-336">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="c0e2c-337">Cette approche a également l’avantage d’éliminer l’utilisation des chaînes de clés dans le code.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-337">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="c0e2c-338">d'instance/de clé</span><span class="sxs-lookup"><span data-stu-id="c0e2c-338">Cache</span></span>

<span data-ttu-id="c0e2c-339">La mise en cache est un moyen efficace de stocker et récupérer des données.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-339">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="c0e2c-340">L’application peut contrôler la durée de vie des éléments mis en cache.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-340">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="c0e2c-341">Les données mises en cache ne sont pas associées à une requête, un utilisateur ou une session spécifique.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-341">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="c0e2c-342">**Veillez à ne pas mettre en cache des données propres à l’utilisateur susceptibles d’être récupérées par les requêtes d’autres utilisateurs.**</span><span class="sxs-lookup"><span data-stu-id="c0e2c-342">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="c0e2c-343">Pour plus d'informations, consultez <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-343">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="c0e2c-344">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="c0e2c-344">Dependency Injection</span></span>

<span data-ttu-id="c0e2c-345">Utilisez [l’injection de dépendances](xref:fundamentals/dependency-injection) pour rendre les données accessibles à tous les utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-345">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="c0e2c-346">Définissez un service contenant les données.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-346">Define a service containing the data.</span></span> <span data-ttu-id="c0e2c-347">Par exemple, une classe nommée `MyAppData` est définie :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-347">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="c0e2c-348">Ajoutez la classe de service à `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-348">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="c0e2c-349">Consommez la classe de service de données :</span><span class="sxs-lookup"><span data-stu-id="c0e2c-349">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="c0e2c-350">Erreurs courantes</span><span class="sxs-lookup"><span data-stu-id="c0e2c-350">Common errors</span></span>

* <span data-ttu-id="c0e2c-351">« Impossible de résoudre le service pour le type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' lors de la tentative d’activation de 'Microsoft.AspNetCore.Session.DistributedSessionStore'. »</span><span class="sxs-lookup"><span data-stu-id="c0e2c-351">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="c0e2c-352">Cette erreur est généralement due à un échec de configuration d’au moins une implémentation `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-352">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="c0e2c-353">Pour plus d’informations, consultez <xref:performance/caching/distributed> et <xref:performance/caching/memory>.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-353">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="c0e2c-354">Dans le cas où le middleware Session ne parvient pas à rendre une session persistante (par exemple si le magasin de stockage n’est pas disponible), il enregistre l’exception dans le journal et la requête se poursuit normalement.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-354">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="c0e2c-355">Cela entraîne un comportement imprévisible.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-355">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="c0e2c-356">Par exemple, imaginez qu’un utilisateur stocke un panier d’achat dans la session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-356">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="c0e2c-357">Il ajoute un élément au panier, mais la validation échoue.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-357">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="c0e2c-358">L’application n’a pas connaissance de l’échec et signale donc à l’utilisateur que l’élément a été ajouté au panier, ce qui est faux.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-358">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="c0e2c-359">L’approche recommandée pour rechercher les erreurs de ce type consiste à appeler `await feature.Session.CommitAsync();` à partir du code d’application quand l’application a terminé d’écrire dans la session.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-359">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="c0e2c-360">`CommitAsync` lève une exception si le magasin de stockage n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-360">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="c0e2c-361">Si `CommitAsync` échoue, l’application peut traiter l’exception.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-361">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="c0e2c-362">`LoadAsync` lève une exception dans les mêmes conditions, quand le magasin de données n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="c0e2c-362">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0e2c-363">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c0e2c-363">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
